# Nginx



#### Nginx

> 上传文件时 nginx client——body——temp要配置权限 chmod 777 client\_body\_temp
>
> nginx brew info nginx 查配置文件路径&#x20;
>
> \-v : show version and exit // 告诉我们nginx的配置信息&#x20;
>
> \-V : show version and configure options then exit // 不仅告诉我们配置信息配置选项，然后停止&#x20;
>
> \-t : test configuration and exit // 检查配置是不是正确
>
> &#x20;\-T : test configuration, dump it and exit // 检查配置，并把这些配置打印出来，然后就停止&#x20;
>
> \-q : suppress non-error messages during configuration testing -s signal : send signal to a master process: stop, quit, reopen, reload // 给主进程发信息，比如stop, quit, reopen, reload这些指令（Quit 是一个优雅的关闭方式，Nginx在退出前完成已经接受的连接请求；Stop 是快速关闭，不管有没有正在处理的请求）&#x20;
>
> \-p prefix : set prefix path (default: /usr/local/Cellar/nginx/1.15.5/) //&#x20;
>
> \-s reopen 重新打开日志（不懂日志）&#x20;
>
> \-c filename : set configuration file (default: /usr/local/etc/nginx/nginx.conf) // nginx到底以哪个配置文件为准&#x20;
>
> \-g directives : set global directives out of configuration file
