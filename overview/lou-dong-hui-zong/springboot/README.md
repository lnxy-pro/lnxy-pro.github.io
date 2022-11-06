# SpringBoot

### 路由和版本 <a href="#ling-lu-you-he-ban-ben" id="ling-lu-you-he-ban-ben"></a>

**0x01：路由知识**

* 有些程序员会自定义 `/manage`、`/management` 、**项目 App 相关名称**为 spring 根路径
* Spring Boot Actuator 1.x 版本默认内置路由的起始路径为 `/` ，2.x 版本则统一以 `/actuator` 为起始路径
* Spring Boot Actuator 默认的内置路由名字，如 `/env` 有时候也会被程序员修改，比如修改成 `/appenv`

**0x02：版本知识**

> Spring Cloud 是基于 Spring Boot 来进行构建服务，并提供如配置管理、服务注册与发现、智能路由等常见功能的帮助快速开发分布式系统的系列框架的有序集合。

**组件版本的相互依赖关系：**

| 依赖项                        | 版本列表及依赖组件版本                                                                                                            |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| spring-boot-starter-parent | ​[spring-boot-starter-parent](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-parent)​ |
| spring-boot-dependencies   | ​[spring-boot-dependencies](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-dependencies)​     |
| spring-cloud-dependencies  | ​[spring-cloud-dependencies](https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-dependencies)​  |

**Spring Cloud 与 Spring Boot 版本之间的依赖关系：**

| Spring Cloud 大版本 | Spring Boot 版本                 |
| ---------------- | ------------------------------ |
| Angel            | 兼容 Spring Boot 1.2.x           |
| Brixton          | 兼容 Spring Boot 1.3.x、1.4.x     |
| Camden           | 兼容 Spring Boot 1.4.x、1.5.x     |
| Dalston          | 兼容 Spring Boot 1.5.x，不兼容 2.0.x |
| Edgware          | 兼容 Spring Boot 1.5.x，不兼容 2.0.x |
| Finchley         | 兼容 Spring Boot 2.0.x，不兼容 1.5.x |
| Greenwich        | 兼容 Spring Boot 2.1.x           |
| Hoxton           | 兼容 Spring Boot 2.2.x           |

**Spring Cloud 小版本号的后缀及含义:**

| 小版本号后缀         | 含义                      |
| -------------- | ----------------------- |
| BUILD-SNAPSHOT | 快照版，代码不是固定，处于变化之中       |
| MX             | 里程碑版                    |
| RCX            | 候选发布版                   |
| RELEASE        | 正式发布版                   |
| SRX            | (修复错误和 bug 并再次发布的)正式发布版 |
