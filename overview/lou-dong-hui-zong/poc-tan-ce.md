---
description: 快速验证存在的漏洞
---

# PoC探测

路径

{% tabs %}
{% tab title="路径" %}

{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

端口服务

目录穿越 未授权



文件读取

> * ​
>   * 用户目录下的敏感文件.bash\_history.zsh\_history.profile.bashrc.gitconfig.viminfopasswd
> * ​
>   * 应用的配置文件/etc/apache2/apache2.conf/etc/nginx/nginx.conf
> * ​
>   * 应用的日志文件/var/log/apache2/access.log/var/log/nginx/access.log
> * ​
>   * 站点目录下的敏感文件.svn/entries.git/HEADWEB-INF/web.xml .htaccess
> * ​
>   * 特殊的备份文件.swp.swo.bakindex.php\~...
> * ​
>   * Python的Cache`__pycache__\__init__.cpython-35.pyc`

``

``

### 4.13.2. 安装逻辑

* 查看能否绕过判定重新安装
* 查看能否利用安装文件获取信息
* 看能否利用更新功能获取信息

### 4.13.3. 交易

#### 4.13.3.1. 购买

* 修改支付的价格
* 修改支付的状态
* 修改购买数量为负数
* 修改金额为负数
* 重放成功的请求
* 并发数据库锁处理不当

#### 4.13.3.2. 业务风控

* 刷优惠券
* 套现

### 4.13.4. 账户

#### 4.13.4.1. 注册

* 覆盖注册
* 尝试重复用户名
* 注册遍历猜解已有账号

#### 4.13.4.2. 密码

* 密码未使用哈希算法保存

#### 4.13.4.3. 邮箱用户名

* 前后空格
* 大小写变换

#### 4.13.4.4. Cookie

* 包含敏感信息
* 未验证合法性可伪造

#### 4.13.4.5. 手机号用户名

* 前后空格
* \+86

#### 4.13.4.6. 登录

* 撞库
* 账号劫持
* 恶意尝试帐号密码锁死账户

#### 4.13.4.7. 找回密码

* 重置任意用户密码
* 密码重置后新密码在返回包中
* Token验证逻辑在前端
* X-Forwarded-Host处理不正确

#### 4.13.4.8. 修改密码

* 越权修改密码
* 修改密码没有旧密码验证

#### 4.13.4.9. 申诉

* 身份伪造
* 逻辑绕过

#### 4.13.4.10. 更新

* ORM更新操作不当可更新任意字段
* 权限限制不当可以越权修改

#### 4.13.4.11. 信息查询

* 权限限制不当可以越权查询
* 用户信息ID可以猜测导致遍历

### 4.13.5. 2FA

* 重置密码后自动登录没有2FA
* OAuth登录没有启用2FA
* 2FA可爆破
* 2FA有条件竞争
* 修改返回值绕过
* 激活链接没有启用2FA
* 可通过CSRF禁用2FA

### 4.13.6. 验证码

* 验证码可重用
* 验证码可预测
* 验证码强度不够
* 验证码无时间限制或者失效时间长
* 验证码无猜测次数限制
* 验证码传递特殊的参数或不传递参数绕过
* 验证码可从返回包中直接获取
* 验证码不刷新或无效
* 验证码数量有限
* 验证码在数据包中返回
* 修改Cookie绕过
* 修改返回包绕过
* 验证码在客户端生成或校验
* 验证码可OCR或使用机器学习识别
* 验证码用于手机短信/邮箱轰炸

### 4.13.7. Session

* Session机制
* Session猜测 / 爆破
* Session伪造
* Session泄漏
* Session Fixation

### 4.13.8. 越权

* 未授权访问
*
  *   水平越权

      攻击者可以访问与他拥有相同权限的用户的资源权限类型不变，ID改变
*
  *   垂直越权

      低级别攻击者可以访问高级别用户的资源权限ID不变，类型改变
*
  *   交叉越权

      权限ID改变，类型改变

### 4.13.9. 随机数安全

* 使用不安全的随机数发生器
* 使用时间等易猜解的因素作为随机数种子

### 4.13.10. 其他

* 用户/订单/优惠券等ID生成有规律，可枚举
* 接口无权限、次数限制
* 加密算法实现误用
* 执行顺序
* 敏感信息泄露









*
  *   FTP (21/TCP)

      默认用户名密码 `anonymous:anonymous`暴力破解密码VSFTP某版本后门
*
  *   SSH (22/TCP)

      部分版本SSH存在漏洞可枚举用户名暴力破解密码
*
  *   Telent (23/TCP)

      暴力破解密码嗅探抓取明文密码
*
  *   SMTP (25/TCP)

      无认证时可伪造发件人
*
  *   DNS (53/UDP)

      域传送漏洞DNS劫持DNS缓存投毒DNS欺骗SPF / DMARC CheckDDoSDNS Query FloodDNS 反弹DNS 隧道
*
  *   DHCP 67/68

      劫持/欺骗
* TFTP (69/TCP)
* HTTP (80/TCP)
*
  *   Kerberos (88/TCP)

      主要用于监听KDC的票据请求用于进行黄金票据和白银票据的伪造
*
  *   POP3 (110/TCP)

      爆破
*
  *   RPC (135/TCP)

      wmic 服务利用
*
  *   NetBIOS (137/UDP & 138/UDP)

      未授权访问弱口令
*
  *   NetBIOS / Samba (139/TCP)

      未授权访问弱口令
*
  *   SNMP (161/TCP)

      Public 弱口令
*
  *   LDAP (389/TCP)

      用于域上的权限验证服务匿名访问注入
* HTTPS (443/TCP)
*
  *   SMB (445/TCP)

      Windows 协议簇，主要功能为文件共享服务`net use \\192.168.1.1 /user:xxx\username password`
*
  *   Linux Rexec (512/TCP & 513/TCP & 514/TCP)

      弱口令
*
  *   Rsync (873/TCP)

      未授权访问
*
  *   RPC (1025/TCP)

      NFS匿名访问
*
  *   Java RMI (1090/TCP & 1099/TCP)

      反序列化远程命令执行漏洞
*
  *   MSSQL (1433/TCP)

      弱密码差异备份 GetShellSA 提权
*
  *   Oracle (1521/TCP)

      弱密码
*
  *   NFS (2049/TCP)

      权限设置不当`showmount <host>`
*
  *   ZooKeeper (2171/TCP & 2375/TCP)

      无身份认证
*
  *   Docker Remote API (2375/TCP)

      未限制IP / 未启用TLS身份认证`http://docker.addr:2375/version`
*
  *   MySQL (3306/TCP)

      弱密码日志写WebShellUDF提权MOF提权
*
  *   RDP / Terminal Services (3389/TCP)

      弱密码
*
  *   Postgres (5432/TCP)

      弱密码执行系统命令
*
  *   VNC (5900/TCP)

      弱密码
*
  *   CouchDB (5984/TCP)

      未授权访问
*
  *   WinRM (5985/TCP)

      Windows对WS-Management的实现在Vista上需要手动启动，在Windows Server 2008中服务是默认开启的
*
  *   Redis (6379/TCP)

      无密码或弱密码绝对路径写 WebShell计划任务反弹 Shell写 SSH 公钥主从复制 RCEWindows 写启动项
*
  *   Kubernetes API Server (6443/TCP && 10250/TCP)

      `https://Kubernetes:10250/pods`
*
  *   JDWP (8000/TCP)

      远程命令执行
* ActiveMQ (8061/TCP)
*
  *   Jenkin (8080/TCP)

      未授权访问
*
  *   Elasticsearch (9200/TCP)

      代码执行`http://es.addr:9200/_plugin/head/``http://es.addr:9200/_nodes`
*
  *   Memcached (11211/TCP)

      未授权访问
* RabbitMQ (15672/TCP & 15692/TCP & 25672/TCP)
*
  *   MongoDB (27017/TCP)

      无密码或弱密码
*
  *   Hadoop (50070/TCP & 50075/TCP)

      未授权访问



*
  *   Jenkins

      未授权访问
*
  *   Gitlab

      对应版本CVE
*
  *   Zabbix

      权限设置不当







*
  *   确定网站采用的语言

      如PHP / Java / Python等找后缀，比如php/asp/jsp
*
*
  *   中间服务器

      如 Apache / Nginx / IIS 等查看header中的信息根据报错信息判断根据默认页面判断
*
  *   Web容器服务器

      如Tomcat / Jboss / Weblogic等
*
  *   后端框架

      根据Cookie判断根据CSS / 图片等资源的hash值判断根据URL路由判断如wp-admin根据网页中的关键字判断根据响应头中的X-Powered-By
*
  *   CDN信息

      常见的有Cloudflare、yunjiasu
*
  *   探测有没有WAF，如果有，什么类型的

      有WAF，找绕过方式没有，进入下一步
*
  *   扫描敏感目录，看是否存在信息泄漏

      扫描之前先自己尝试几个的url，人为看看反应



#### 业务测试点

1. xff头绕过短信发送频率限制
2. HTTP 盲攻击
3. Fuzz逻辑越权
4. XXE
5.  登录认证

    > * 暴力破解

查看数据是否采用SSL加密方式,是否可破解Wireshark抓包查看数据整改建议: 部署有效的SSL证书> >

<details>

<summary>Session测试</summary>

会话固定测试

检查用户退出后session能否重复使用利用:诱骗用户使用攻击者固定会话进行登录,窃取认证信息记录两次登录后的session,比较是否相同,相同则为固定session客户端登录系统时判断是否提交留存session认证,如果是留存session需要及时销毁并重新生成session认证

会话注销

检查用户退出后session是否依然有效,授权未释放利用: 如果未清空的session可以持续有效,攻击者可以获取用户权限sessionID销毁后未能清空服务器存储的session整改建议:用户注销或退出时,服务器应及时销毁session认证会话信息并清空客户端session标识

会话超时

检查用户10分钟无操作时,session是否被销毁并要求重新认证整改建议: 对session认证配置声明周期(常规业务系统建议30分钟内)

</details>

\> >

<details>

<summary>Cookie仿冒</summary>

修改Cookie身份标识,并拥有相关用户的权限通过篡改身份认证标识值来判断能否改变用户身份会话整改建议: 客户端标识的用户敏感信息数据,使用session会话认证方式,避免被仿冒

</details>

\> >

<details>

<summary>密文比对认证</summary>

检查密码是通过前端加密后和数据库比对,还是后端加密比对如果是前端加密啊,找到加密方式可以对用户名密码进行爆破整改建议: 将密码加密过程和比对过程放置在服务器后台执行

</details>

\> >

<details>

<summary>登录失败信息测试</summary>

查看登录失败后回显的信息提示比如:“用户名不存在”,“密码错误”,“账号不存在”等明确信息整改建议: 系统登录失败提示模糊描述

</details>

6\. 业务模块

> <details>
>
> <summary>订单ID篡改测试</summary>
>
>
>
> </details>

未考虑用户间权限隔离问题时,就会导致平行越权通过修改id,获取其他用户信息整改建议: 敏感数据要通过session机制判断用户身份,做好平行权限控制.服务端要交验请求的数据是否和登录者的身份一致,如果不一致则拒绝请求> >

<details>

<summary>手机号码篡改</summary>

篡改手机号越权没有身份校验时,抓包篡改手机号(挂失/找回密码),来越权获取信息整改建议: 服务端需要交验手机号和登录者的身份是否一致

</details>

\> >

<details>

<summary>用户ID篡改</summary>

同订单id篡改一样

</details>

\> >

<details>

<summary>邮箱和用户篡改</summary>

篡改发件人参数,导致攻击者可以伪造发信人进行钓鱼整改建议: 写信、发消息、发邮件要判断用户身份

</details>

\> >

<details>

<summary>商品编号/金额篡改</summary>

篡改商品价格或编号编号与价格不对应,1分钱成交整改建议: 商品金额不在客户端传入,防止被篡改,如果需要客户端传入金额,则服务端需要检查商品价格和交易金额是否一致,或对支付金额做签名校验

</details>

\> >

<details>

<summary>条件竞争</summary>

利用数据库时间差,多次重放数据整改建议: 在处理订单、支付等关键业务时,使用悲观锁或乐观锁保证事物的ACID特性(原子性,一致性,隔离性,持久性),避免数据脏读

</details>

1.  授权访问

    > <details>
    >
    > <summary>未授权访问</summary>
    >
    >
    >
    > </details>

查看未授权可以访问的页面整改建议: 需要安全配置或权限认证的地址、授权页面存在缺陷.对用户身份做session认证,并对用户访问的url做身份鉴别> >

<details>

<summary>越权</summary>

水平越权和垂直越权水平: A登录后篡改为B身份访问B的信息,垂直: 篡改为管理员整改建议: 服务端需要交验身份唯一性,自己的身份智能增删改查自己的信息

</details>

8\. 输入/输出模块测试 >

<details>

<summary>SQL注入测试</summary>

查看数据是否采用SSL加密方式,是否可破解Wireshark抓包查看数据整改建议: 部署有效的SSL证书

</details>

\> >

<details>

<summary>XSS测试</summary>

查看数据是否采用SSL加密方式,是否可破解Wireshark抓包查看数据整改建议: 部署有效的SSL证书

</details>

\> >

<details>

<summary>命令执行</summary>

查看数据是否采用SSL加密方式,是否可破解Wireshark抓包查看数据整改建议: 部署有效的SSL证书

</details>

9\. 回退测试 10. 验证码机制 > - 爆破 > - 重复使用 > - 客户端回显 > - 验证码绕过 > - 验证码识别 11. 数据安全 >

<details>

<summary>订单篡改</summary>

在支付页面,篡改支付金额整改建议: 商品信息、金额、折扣等原始数据的校验应来自服务器

</details>

\> >

<details>

<summary>本地加密传输</summary>

提交异常数量的订单支付请求整改建议: 服务端对产生异常情况的交易行为(积分为负 库存为0)应直接予以限制、阻断

</details>

\> > - 前端js限制绕过 > - 请求重放 > - 业务上限测试 12. 业务流程乱序测试 > - 针对业务处理流程是否正常.没有绕过 e.g.: 未经支付,直接充值成功 13. 密码找回 > - 验证码客户端回显 > - 验证码爆破 > - 接口参数账号修改 (任意账号密码修改的漏洞) > - response状态值修改测试 (修改响应结果) > - session覆盖 (任意密码找回) > > 1. 准备自己的账号接收短信 > > 2. 获得凭证进入密码重置 > > 3. 在浏览器新标签中打开找回密码页面,输入目标手机号 > > 4. 当前session账号已经被覆盖,重新回到第二步打开重置密码 > - 弱token (任意密码找回 漏洞, 找token规律, ) > - 密码找回流程绕过 (搜集 账号、凭证校验、重置的三个接口,第一步跳第三部)

***

业务风险点

1.  账号安全

    > 账号密码暴漏
    >
    > 无限制登录任意账号
    >
    > 电子邮件账号泄漏
    >
    > 中间人攻击
    >
    > 撞库攻击

    ***

    > 整改方法:
2.  密码找回

    > 密码找回凭证爆破
    >
    > 密码找回凭证回显
    >
    > > 密码找回凭证在请求链接中
    > >
    > > 加密验证字符串回显
    > >
    > > 网页源码泄漏密保
    > >
    > > 短信验证码回显
    >
    > 密码重置链接弱Token
    >
    > > 使用时间戳的md5作为密码重置token
    > >
    > > 使用服务器时间作为密码重置token
    >
    > 密码重置凭证与用户账户关联不严
    >
    > > 使用短信验证码找回密码
    > >
    > > 使用邮箱Token找回密码
    >
    > 重新绑定用户手机或邮箱
    >
    > > 重新绑定用户手机
    > >
    > > 重新绑定用户邮箱
    >
    > 服务端验证逻辑缺陷
    >
    > > 删除参数绕过验证
    > >
    > > 邮箱地址可被操控
    > >
    > > 身份验证步骤可被绕过
    >
    > 在本地验证服务端返回信息-修改返回包绕过验证
    >
    > 注册覆盖——已存在用户可被重复注册
    >
    > session覆盖——session覆盖重置他人密码

    ***

    > 整改方法
3.  越权访问

    > 平行越权
    >
    > > 查看其他用户信息
    >
    > 纵向越权
    >
    > > 权限提升

    ***

    > 整改方法
4.  OAuth2.0 安全问题

    > Oauth2.0 漏洞

    ***

    > 整改方法
5.  在线支付

    > 订单金额篡改
    >
    > 数量篡改
    >
    > 请求重放测试
    >
    > 其他参数干扰

    ***

    > 整改方法
6.  文件上传

    > 文件上传回显错误路径
7.  文件包含

    > 文件包含数据读取

***

重点关注: Spring Boot Actuator 未授权访问 远程代码执行
