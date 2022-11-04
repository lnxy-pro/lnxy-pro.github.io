---
description: pache
---

# Apache





```
* [安全运维中基线检查的自动化之ansible工具巧用](https://bbs.ichunqiu.com/thread-46896-1-1.html?from=snew
```

**1，\***_**Apache\***_**低权限用户启动**

```
vi /etc/httpd/conf/httpd.conf

User apache
Group apache
```

**2，目录遍历漏洞**

```
vi /etc/httpd/conf/httpd.conf

Options Indexes FollowSymLinks
改为：
Options FollowSymLinks
```

**3，关闭版本号显示**

```
vi /etc/httpd/conf/httpd.conf

ServerTokens Prod
ServerSignature Off
```

**4，** **上传目录禁止执行**

如果web应用确实需要支持文件上传功能，应在配置文件里面限制上传目录无脚本执行权限。假设上传目录绝对路径为”/var/www/html/upload”，配置示例如下：

```
<Directory "/var/www/html/upload">
    AllowOverride None
    <Files ~ "\.php">
        Order Allow,Deny
        Deny from all
   </Files>
   ....
</Directory>
```

**5，PHP解析设置**

默认配置下**Apache**会将类似 .php.abc 扩展名的文件作为 php 脚本来处理，攻击者常利用文件上传结合这种机制来上传 WebShell 脚本。通过修改 httpd.conf 中的如下配置，可以有效避免这个问题。

修改前配置：

```
AddType application/x-httpd-php .php
AddType application/x-httpd-php-source .phps
```

修改后配置：

```
<FilesMatch \.php$>
    SetHandler application/x-httpd-php
</FilesMatch>
<FilesMatch "\.phps$">
    SetHandler application/x-httpd-php-source
</FilesMatch>
```

**6，禁用 CGI**

修改 httpd.conf 配置文件，注释相关模块及配置：

```
#LoadModule cgi_module modules/mod_cgi.so
#ScriptAlias /cgi-bin/ "/var/www/cgi-bin/"
#<Directory "/var/www/cgi-bin">
#    AllowOverride None
#    Options None 
#    Order allow,deny
#    Allow from all
#</Directory>
```

**7. 自定义错误页面**

修改 httpd.conf 配置文件，修改或添加以下内容：

```
ErrorDocument 500 /errorhtml
ErrorDocument 404 /error.html
ErrorDocument 403 /error.html
```

**8. 关闭 Trace**

修改 httpd.conf 配置文件，修改或添加以下配置（仅适用于**Apache** 2.0以上版本）：

```
TraceEnable Off
```

**9. 禁用 SSI** 修改 httpd.conf 配置文件，注释相关模块及配置：

```
#LoadModule include_module modules/mod_include.so
<Directory "/var/www/html">
    Options Indexes FollowSymLinks -Includes  AllowOverride None
    Order allow,deny
    allow from all
</Directory>
```
