# 上传绕过

> #### 4.8.1. 文件类型检测绕过 <a href="#4.8.1.-wen-jian-lei-xing-jian-ce-rao-guo" id="4.8.1.-wen-jian-lei-xing-jian-ce-rao-guo"></a>
>
> **4.8.1.1. 更改请求绕过**
>
> 有的站点仅仅在前端检测了文件类型，这种类型的检测可以直接修改网络请求绕过。 同样的，有的站点在后端仅检查了HTTP Header中的信息，比如 `Content-Type` 等，这种检查同样可以通过修改网络请求绕过。
>
> **4.8.1.2. Magic检测绕过**
>
> 有的站点使用文件头来检测文件类型，这种检查可以在Shell前加入对应的字节以绕过检查。几种常见的文件类型的头字节如下表所示

| 类型  | 二进制值                          |
| --- | ----------------------------- |
| JPG | FF D8 FF E0 00 10 4A 46 49 46 |
| GIF | 47 49 46 38 39 61             |
| PNG | 89 50 4E 47                   |
| TIF | 49 49 2A 00                   |
| BMP | 42 4D                         |

> **4.8.1.3. 后缀绕过**
>
> 部分服务仅根据后缀、上传时的信息或Magic Header来判断文件类型，此时可以绕过。asp/aspx 可解析asp,aspx,asa,asax,ascx,ashx,asmx,cer,aSp,aSpx,aSa,aSax,aScx,aShx,aSmx,cErasp 支持`asa` / `asax` / `cer` / `cdx` / `aspx` / `ascx` / `ashx` / `asmx` / `asp{80-90}` 等后缀。php由于历史原因，部分解释器可能支持符合正则 `/ph(p[2-7]?|t(ml)?)/` 的后缀，如 `php` / `php5` / `pht` / `phtml` / `shtml` / `pwml` / `phtm` 等 可在禁止上传php文件时测试该类型。jsp引擎则可能会解析 `jspx` / `jspf` / `jspa` / `jsw` / `jsv` / `jtml`/jsp,jspa,jspx,jsw,jsv,jspf,jtml,jSp,jSpx,jSpa,jSw,jSv,jSpf,jHtml 等后缀除了这些绕过，其他的后缀同样可能带来问题，如 `vbs` / `asis` / `sh` / `reg` / `cgi` / `exe` / `dll` / `com` / `bat` / `pl` / `cfc` / `cfm` / `ini` 等。
>
> **4.8.1.4. 系统命名绕过**
>
> 在Windows系统中，上传 `index.php.` 会重命名为 `.` ，可以绕过后缀检查。 也可尝试 `index.php%20` ， `index.php:1.jpg` `index.php::$DATA` 等。 在Linux系统中，可以尝试上传名为 `index.php/.` 或 `./aa/../index.php/.` 的文件
>
> **4.8.1.5. .user.ini**
>
> 在php执行的过程中，除了主 `php.ini` 之外，PHP 还会在每个目录下扫描 INI 文件，从被执行的 PHP 文件所在目录开始一直上升到 web 根目录（$\_SERVER\['DOCUMENT\_ROOT'] 所指定的）。如果被执行的 PHP 文件在 web 根目录之外，则只扫描该目录。 `.user.ini` 中可以定义除了PHP\_INI\_SYSTEM以外的模式的选项，故可以使用 `.user.ini` 加上非php后缀的文件构造一个shell，比如 `auto_prepend_file=01.gif` 。
>
> **4.8.1.6. WAF绕过**
>
> 有的waf在编写过程中考虑到性能原因，只处理一部分数据，这时可以通过加入大量垃圾数据来绕过其处理函数。另外，Waf和Web系统对 `boundary` 的处理不一致，可以使用错误的 `boundary` 来完成绕过。
>
> **4.8.1.7. 竞争上传绕过**
>
> 有的服务器采用了先保存，再删除不合法文件的方式，在这种服务器中，可以反复上传一个会生成Web Shell的文件并尝试访问，多次之后即可获得Shell。
>
> #### 4.8.2. 攻击技巧 <a href="#4.8.2.-gong-ji-ji-qiao" id="4.8.2.-gong-ji-ji-qiao"></a>
>
> **4.8.2.1. Apache重写GetShell**
>
> Apache可根据是否允许重定向考虑上传.htaccess内容为AddType application/x-httpd-php .pngphp\_flag engine 1就可以用png或者其他后缀的文件做php脚本了
>
> **4.8.2.2. 软链接任意读文件**
>
> 上传的压缩包文件会被解压的文件时，可以考虑上传含符号链接的文件 若服务器没有做好防护，可实现任意文件读取的效果
>
> #### 4.8.3. 防护技巧 <a href="#4.8.3.-fang-hu-ji-qiao" id="4.8.3.-fang-hu-ji-qiao"></a>
>
> * 使用白名单限制上传文件的类型
> * 使用更严格的文件类型检查方式
> * 限制Web Server对上传文件夹的解析
