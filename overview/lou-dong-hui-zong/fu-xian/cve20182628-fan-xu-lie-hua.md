---
description: Weblogic-高危通过该漏洞，攻击者可以在未授权的情况下远程执行任意代码。
---

# CVE-2018-2628 反序列化

### 漏洞影响范围

* Weblogic 10.3.6.0
* Weblogic 12.1.3.0
* Weblogic 12.2.1.2
* Weblogic 12.2.1.3

**复现：**

第一步发送测试PoC，PoC中远程连接的服务器地址就是第二步中所使用的服务器，攻击的ip是192.168.3.103的7001端口上的T3服务，该服务会解包Object结构，通过一步步的readObject去第二步服务器上的1099端口请求恶意封装的代码，然后在本地弹出计算器。

第二步在远程服务器上启用ysoserial.exploit.JRMPListener，JRMPListener会将含有恶意代码的payload发送回请求方。

查看weblogic的日志，可以看到如下错误，此时已经弹出计算器：

Weblogic已经将互联网暴露的PoC都已经加入了黑名单，如果要绕过他的黑名单的限制就只能自己动手构造。来看看InboundMsgAbbrev中resolveProxyClass的实现，resolveProxyClass是处理rmi接口类型的，只判断了java.rmi.registry.Registry，其实随便找一个rmi接口即可绕过。

protected Class\<?> resolveProxyClass(String\[] interfaces) throws IOException, ClassNotFoundException { String\[] arr$ = interfaces; int len$ = interfaces.length;

for(int i$ = 0; i$ < len$; ++i$) { String intf = arr$\[i$]; if(intf.equals("java.rmi.registry.Registry")) { throw new InvalidObjectException("Unauthorized proxy deserialization"); } }

return super.resolveProxyClass(interfaces); }

其实核心部分就是JRMP（Java Remote Methodprotocol），在这个PoC中会序列化一个RemoteObjectInvocationHandler，它会利用UnicastRef建立到远端的tcp连接获取RMI registry，加载回来再利用readObject解析，从而造成反序列化远程代码执行。

