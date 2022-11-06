---
description: web | 小程序 | 公众号 | 移动端站点
---

# 目标探测

## 主机&网络

{% tabs %}
{% tab title="系统" %}
Linux大小写敏感Windows大小写不敏感


{% endtab %}

{% tab title="端口服务" %}
查看header中的信息，根据报错信息判断、默认页面判断

|                          |                                   |                                                                                           |                                                     |                                                                                                  |                       |
| ------------------------ | --------------------------------- | ----------------------------------------------------------------------------------------- | --------------------------------------------------- | ------------------------------------------------------------------------------------------------ | --------------------- |
| 服务                       | 端口&协议                             | 测试点                                                                                       | 执行                                                  | 默认路径                                                                                             | 默认口令                  |
| FTP                      | 21/TCP                            | 暴力破解密码VSFTP某版本后门                                                                          |                                                     |                                                                                                  | `anonymous:anonymous` |
| SSH                      | 22/TCP                            | 部分版本SSH存在漏洞可枚举用户名暴力破解密码                                                                   |                                                     |                                                                                                  |                       |
| Telent                   | 23/TCP                            | 暴力破解密码嗅探抓取明文密码                                                                            |                                                     |                                                                                                  |                       |
| SMTP                     | 25/TCP                            | 无认证时可伪造发件人                                                                                |                                                     |                                                                                                  |                       |
| DNS                      | 53/UDP                            | <p>域传送漏洞<br>DNS劫持<br>DNS缓存投毒<br>DNS欺骗SPF / DMARC CheckDDoSDNS Query FloodDNS 反弹DNS 隧道</p> |                                                     |                                                                                                  |                       |
| DHCP                     | 67/68                             | 劫持/欺骗                                                                                     |                                                     |                                                                                                  |                       |
| TFTP                     | 69/TCP                            |                                                                                           |                                                     |                                                                                                  |                       |
| POP3                     | 110/TCP                           | 爆破                                                                                        |                                                     |                                                                                                  |                       |
| RPC                      | 135/TCP                           | wmic 服务利用                                                                                 |                                                     |                                                                                                  |                       |
| NetBIOS                  | 137/UDP & 138/UDP                 | 未授权访问弱口令                                                                                  |                                                     |                                                                                                  |                       |
| NetBIOS / Samba          | 139/TCP                           | 未授权访问弱口令                                                                                  |                                                     |                                                                                                  |                       |
| SNMP                     | 161/TCP                           | Public 弱口令                                                                                |                                                     |                                                                                                  |                       |
| LDAP                     | 389/TCP                           | 用于域上的权限验证服务 匿名访问注入                                                                        |                                                     |                                                                                                  |                       |
| SMB                      | 445/TCP                           | Windows 协议簇，主要功能为文件共享服务                                                                   | `net use \\192.168.1.1 /user:xxx\username password` |                                                                                                  |                       |
| Linux Rexec              | 512/TCP & 513/TCP & 514/TCP       | 弱口令                                                                                       |                                                     |                                                                                                  |                       |
| Java RMI                 | 1090/TCP & 1099/TCP               | 反序列化远程命令执行漏洞                                                                              |                                                     |                                                                                                  |                       |
| MSSQL                    |  1433/TCP                         | 弱密码 差异备份 GetShellSA 提权                                                                    |                                                     |                                                                                                  |                       |
| Oracle                   | 1521/TCP                          | 弱口令                                                                                       |                                                     |                                                                                                  |                       |
| Rsync                    | 873/TCP                           | 未授权访问                                                                                     |                                                     |                                                                                                  |                       |
| RPC                      | 1025/TCP                          | NFS匿名访问                                                                                   |                                                     |                                                                                                  |                       |
| NFS                      | 2049/TCP                          | 权限设置不当                                                                                    | `showmount <host>`                                  |                                                                                                  |                       |
| ZooKeeper                | 2171/TCP & 2375/TCP               | 无身份认证                                                                                     |                                                     |                                                                                                  |                       |
| Docker Remote API        | 2375/TCP&2376                     | 未限制IP / 未启用TLS身份认证                                                                        |                                                     | `http://docker.addr:2375/version`                                                                |                       |
| RDP / Terminal Services  | 3389/TCP                          | 弱口令                                                                                       |                                                     |                                                                                                  |                       |
| Postgres                 | 5432/TCP                          | 弱口令&执行系统命令                                                                                |                                                     |                                                                                                  |                       |
| VNC                      | 5900/TCP                          | 弱密码                                                                                       |                                                     |                                                                                                  |                       |
| CouchDB                  | 5984/TCP                          | 未授权访问                                                                                     |                                                     |                                                                                                  |                       |
| Kerberos                 | 88/TCP                            | 主要用于监听KDC的票据请求用于进行黄金票据和白银票据的伪造                                                            |                                                     |                                                                                                  |                       |
| Kubernetes API Server    | 6443/TCP && 10250/TCP             |                                                                                           |                                                     | `https://Kubernetes:10250/pods`                                                                  |                       |
| Elasticsearch            | 9200/TCP                          | 代码执行                                                                                      |                                                     | <p><code>http://es.addr:9200/_plugin/head/</code><br><code>http://es.addr:9200/_nodes</code></p> |                       |
| JDWP                     | 8000/TCP                          | 远程命令执行                                                                                    |                                                     |                                                                                                  |                       |
| RabbitMQ                 | 15672/TCP & 15692/TCP & 25672/TCP |                                                                                           |                                                     |                                                                                                  |                       |
| ActiveMQ                 | 8061/TCP                          |                                                                                           |                                                     |                                                                                                  |                       |
| Redis                    | 6379TCP                           | 无密码或弱密码/绝对路径写WebShell/                                                                    |                                                     |                                                                                                  |                       |
| Memcached                | 11211/TCP                         | 未授权访问                                                                                     |                                                     |                                                                                                  |                       |
| Hadoop                   | 50070/TCP 50075/TCP               | 未授权访问                                                                                     |                                                     |                                                                                                  |                       |
| Zabbix                   |                                   | 权限设置不当                                                                                    |                                                     |                                                                                                  |                       |
| Gitlab                   |                                   | 对应版本CVE                                                                                   |                                                     |                                                                                                  |                       |
| Jenkins                  | 8080/TCP                          | 未授权访问                                                                                     |                                                     |                                                                                                  |                       |
| MongoDB                  | 27017/TCP                         | 无密码或弱密码                                                                                   |                                                     |                                                                                                  |                       |
| WinRM                    | 5985/TCP                          | Windows对WS-Management的实现在Vista上需要手动启动，在Windows Server 2008中服务是默认开启的                       |                                                     |                                                                                                  |                       |


{% endtab %}

{% tab title="协议" %}
协议逆向
{% endtab %}

{% tab title="WAF" %}
探测有没有WAF，如果有，什么类型的

有WAF，找绕过方式没有，进入下一步
{% endtab %}

{% tab title="DNS" %}
查找dns服务器
{% endtab %}

{% tab title="防火墙策略测试" %}

{% endtab %}
{% endtabs %}

## 技术探测

{% tabs %}
{% tab title="技术架构" %}




如Tomcat / Jboss / Weblogic等

*
  *   后端框架

      根据Cookie判断根据CSS / 图片等资源的hash值判断根据URL路由判断如wp-admin根据网页中的关键字判断根据响应头中的X-Powered-By
*
  *   CDN信息

      常见的有Cloudflare、yunjiasu
*
{% endtab %}

{% tab title="后台URL" %}

{% endtab %}

{% tab title="路径Fuzz" %}
路径穿越/路径参数越权/扫描敏感目录，看是否存在信息泄漏
{% endtab %}

{% tab title="敏感文件" %}
robots.txt    crossdomain.xml  sitemap.xml    xx.tar.gzxx.bak

敏感文件泄漏

.git.svn

* 用户目录下的敏感文件.bash\_history.zsh\_history.profile.bashrc.gitconfig.viminfopasswd
* ​
  * 应用的配置文件/etc/apache2/apache2.conf/etc/nginx/nginx.conf
* ​
  * 应用的日志文件/var/log/apache2/access.log/var/log/nginx/access.log
* ​
  * 站点目录下的敏感文件.svn/entries.git/HEADWEB-INF/web.xml .htaccess
* ​
  * 特殊的备份文件.swp.swo.bakindex.php\~...
* ​
  * Python的Cache`__pycache__\__init__.cpython-35.pyc`
{% endtab %}

{% tab title="敏感信息" %}
代码注释

邮箱

三方插件：公开漏洞后没有及时更新

云主机注册信息

*   加密体系

    在客户端存储私钥




{% endtab %}
{% endtabs %}

## 业务探测



{% tabs %}
{% tab title="条件竞争" %}
利用数据库时间差,多次重放数据整改建议: 在处理订单、支付等关键业务时,使用悲观锁或乐观锁保证事物的ACID特性(原子性,一致性,隔离性,持久性),避免数据脏读
{% endtab %}

{% tab title="账户" %}
回退测试 10. 验证码机制 > - 爆破 > - 重复使用 > - 客户端回显 > - 验证码绕过 > - 验证码识别 11. 数据安全 >

\> > - 前端js限制绕过 > - 请求重放 > - 业务上限测试

&#x20;12\. 业务流程乱序测试 > - 针对业务处理流程是否正常.没有绕过 e.g.: 未经支付,直接充值成功&#x20;

13\. 密码找回 > - 验证码客户端回显 > - 验证码爆破 >

&#x20;\- 接口参数账号修改 (任意账号密码修改的漏洞) > - response状态值修改测试 (修改响应结果) >

&#x20;\- session覆盖 (任意密码找回) > >&#x20;

&#x20;         1\. 准备自己的账号接收短信 > >&#x20;

&#x20;         2\. 获得凭证进入密码重置 > >&#x20;

&#x20;         3\. 在浏览器新标签中打开找回密码页面,输入目标手机号 > >&#x20;

&#x20;         4\. 当前session账号已经被覆盖,重新回到第二步打开重置密码 >&#x20;

\- 弱token (任意密码找回 漏洞, 找token规律, ) >&#x20;

\- 密码找回流程绕过 (搜集 账号、凭证校验、重置的三个接口,第一步跳第三部)



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
{% endtab %}

{% tab title="认证" %}
* 重置密码后自动登录没有2FA
* OAuth登录没有启用2FA
* 2FA可爆破
* 2FA有条件竞争
* 修改返回值绕过
* 激活链接没有启用2FA
* 可通过CSRF禁用2FA
*
*
* Session机制
* Session猜测 / 爆破
* Session伪造
* Session泄漏
* Session Fixation
{% endtab %}

{% tab title="验证码" %}
xff头绕过短信发送频率限制

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
{% endtab %}

{% tab title="交易" %}


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
{% endtab %}

{% tab title="权限" %}


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
{% endtab %}
{% endtabs %}







组织信息主机 。&#x20;
