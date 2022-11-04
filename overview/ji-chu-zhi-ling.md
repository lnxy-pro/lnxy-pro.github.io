# 💻 基础指令

## 网络

{% tabs %}
{% tab title="Linux" %}


#### netstat

> **Mac上的TCP连接**
>
> ```
> netstat -apv TCP
> ```
>
> **查listen结果**
>
> ```
>  netstat -a | grep -i “listen”
> ```
>
> \-a 在netstat的输出中包含服务端口(server ports) -g 列出了和广播连接(multicast connections)相关的信息 -I interface 提供指定接口(interface)的数据包数据. 所有有效的接口(interfaces) 都能通过-i flag查看, 但是en0 通常是默认的传出网络接口(interface) -n 隐藏带有名称的远程地址标签，带来的好处是：大大加快了netstat的输出，同时只牺牲了有限的信息 -p protocol 列出与特定网络协议(protocol)关联的流量. 完整的协议(protocol)列表位于/etc/protocols，但是最重要的协议是udp和tcp -r 显示了路由表，显示数据包是如何在网络中路由的 -s 显示所有协议(protocol)的网络统计信息，无论它们是否处于活动状态 -v 增加详细程度，特别是通过添加一列来显示与每个打开的端口关联的进程ID(pid)

#### lsof

> **列出所有主机名为lsof.itap和端口513的tcp连接**
>
> ```
> lsof -nP -iTCP@lsof.itap:513
> ```
>
> **返回状态LISTEN的tcp连接,显示mac上打开的端口**
>
> ```
> lsof -iTCP -sTCP:LISTEN
> ```
>
> **此命令返回当前登录用户不拥有的所有连接**
>
> ```
>  sudo lsof -i -u^$(whoami)
> ```
>
> \-i 展示了所有打开的网络连接(open network connections) 和使用这个连接(connection)的进程(process)的名称. 如果增加一个4，如-i4, 将展示IPv4连接; 如-i6 将展示IPv6连接. -i flag 还可以继续扩展以指定更多详细信息，-iTCP或者-iUDP将返回仅是TCP或UDP的链接. -iTCP:25将返回端口(port)是25的TCP连接. 还可以指定端口范围，如 -iTCP:25-50. 使用 -i@1.2.3.4 将返回ip是1.2.3.4的IPv4地址. IPv6也是一样的. @符号也可以以同样的方式用于指定hostname, -s 强制显示文件大小(file size). 但是和-i成对出现时，它的含义就不同了：它允许用户指定要返回的命令的协议和状态 -p 将lsof限制为特定的进程ID（PID）。可以使用-p 123,456,789等设置多个PID。进程ID也可以用排除，如123，456，它专门排除PID 456 -P 禁用端口号到端口名的转换，从而加快了输出速度 -n 禁止将网络号转换为主机名(network numbers to host names)。与上面的-P一起使用时，它可以显着加快lsof的输出 -u user 仅返回指定用户拥有的命令



#### Route

> Linux: sudo route add -net 10.0.0.0 -netmask 255.0.0.0 10.210.18.129
{% endtab %}

{% tab title="Win" %}

{% endtab %}
{% endtabs %}

## 路由

{% tabs %}
{% tab title="路由转发配置" %}
> 网络地址: IP +子网掩码+网关+Dns
>
> **ip addr**
>
> > mtu 默认发送包大小
> >
> > 速率
> >
> > Mac地址 eth0: 00:1c:42:16:67:b4
> >
> > &#x20;Virbr0: 52:54:00:b9:2f:bb Virbr0-nic
>
> * Service NetworkManager stop( systemctl stop NetworkManager.service )# 关闭mac地址管理服务
> * chkconfig --level 345 NetworkManager off
> * ip adds add 本机设备ip/24 dev eth0 // 配置ip
> * ip adds del 本机设备ip/24 dev eth0 // 删除网卡对应ip配置
> * Ip link set eth0 up // 网卡启动
> * ip route //路由表就是网关
> * ip route add default via 192.168.86.1 dev eth0 // 配置默认网关
> * nslookup www.baidu.com // 验证dns解析
> * vim /etc/resolv.conf //dns解析文件 添加 nameserver dns地址 \[北京/上海/广东/]dns地址
> * etc/sysconfig/network-scripts/ //网卡配置文件 永久配置
> * vim ifcfg-eth0
>
> ```
> // 通过配置文件配置网卡
> DEVICE=eth0                                        //  网卡名称
> TYPE=Ethernet                                    //  类型
> ONBOOT=yes                                        //  是否允许network服务管理
> BOOTPROTO=static                            //  静态获取
> IPADDR=192.168.1.254
> NETMASK=255.255.255.0
> GATEWAY=XXX.XXX.XX.X                    //  网关配置
> DNS1=XXX
> ```
>
> * /etc/init.d/network restart // 重启后配置生效
> * service network restart
> * /etc/sysctl.conf // 内核级配置文件
> * net.ipv4.ip\_forward=0 // 是否做路由转发, 0 不转发 1转发
> * sysctl -p // 刷新配置文件
> * Iptables -nL // 查看防火墙
> * setup // 关闭防火墙 第二个选项
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

## 防火墙



## 补丁



## 软件包



## 进程



## 用户组



## 文件权限



## 文件系统挂载



## 文件查找



## 压缩解压缩



## 查看



#### Curl

> **返回状态码**
>
> ```
> curl -o /dev/null -s -w %{http_code} www.linux.com       # 返回状态码
> ```
>
> **下载文件**
>
> ```
> curl -O http://www.linux.com/hello.sh 
> curl -o dodo1.jpg http:www.linux.com/dodo1.JPGx
> curl -O http://www.linux.com/dodo[1-5].JPG      # 循环下载[1-5]
> ```
>
> **写入请求头**
>
> ```
> curl -D test.txt www.baidu.com
> ```
>
> Curl -I www.sina.com >> text.txt // 输出返回的请求头到test
>
> _curl ipinfo.io_
>
> _curl_ [_https://ip.cn_](https://ip.cn/)
>
> _curl cip.cc_
>
> _curl myip.ipip.net_



*   md转html

    > 1. i5ting\_toc -f yourfile.md
    > 2. i5ting\_toc -f yourfile.md -o
    > 3. i5ting\_toc -f
*   语法

    ```
    details  summary


    ```



OS X

{% tabs %}
{% tab title="硬盘" %}
硬盘推出 (被占用异常)

```
df   -lh     查看挂载硬盘
sudo diskutil unmount   硬盘编号 
如果显示有进程占用 kill -9 pid  最后推出
open -a Calculator     # 计算机
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

linux 常用

```
Linux 指令
环境变量配置
 /etc/profile
 /etc/paths 
 ~/.bash_profile ..
 
历史指令
history #查看
cat /root/.bash_history # linux 查看指令文件
history -c  # 清除

创建&写入
mkdir sshs && cat >> sshs/key.txt
拷贝
cp  文件名  路径      cp  hello.csv  ./python/ml：把当前目录的hello.csv拷贝到当前目的python文件夹里的ml文件夹里
cp 源文件名  新文件名   cp  hello.txt   world.txt：复制并改名,并存放在当前目录下  
cp file1 file2 复制一个文件 
cp dir/* . 复制一个目录下的所有文件到当前工作目录 
cp -a /tmp/dir1 . 复制一个目录到当前工作目录 
cp -a dir1 dir2 复制一个目录 

服务
rpm -qa|grep tomcat #查看是否有tomcat 
ps -ef|grep tomcat  #查看tomcat进程
find / -name tomcat #找tomcat目录
cd /xx #进入tomcat路径
./shutdown.sh #关闭tomcat
./startup.sh  #启动tomcat
ps -ef|grep java # 查看tomcat是否关闭
nohup ./startup.sh # 做为服务启动
./catalina.sh run //控制台动态输出方式启动，动态的显示tomcat控制台输出信息，Ctrl+c退出并停止服务
tail -f catalina.out //查看日志，同样Ctrl+c退出
```

&#x20;

#### Iptables

> **安装**
>
> ```
> yum install iptables-services
> ```
>
> **操作**
>
> ```
> service iptables status #查看防火墙状态
> service iptables stop # 停止防火墙
> service iptables start #启动防火墙
> service iptables restart #重启防火墙
> service iptables  off #永久关闭
> chkconfig iptables on #关闭后再重启
> ```

#### Firewalld

> ```
> firewall-cmd --zone=public --add-port=80/tcp --permanent （--permanent永久生效，没有此参数重启后失效）   #开启80端口
> systemctl status firewalld查看firewalld状态，发现当前是dead状态，即防火墙未开启
> ystemctl start firewalld开启防火墙，没有任何提示即开启成功。
> 通过systemctl status firewalld查看firewalld状态，显示running即已开启了。
> ```



#### 用户和组

> ```
> adduser xx     #添加用户 ,切换到root会受限
> passwd username  #为用户设置密码   
> passw d -d username  #删除用户密码
> su parallels  # 新用户横向权限移动到parllels 
> visudo       # 需要用root权限打开/etc/sudoers文件,添加新用户,新用户可以切换root
> awk -F: '$3==0 {print $1}' /etc/passwd # 查看特权用户
> awk -F: 'length($2)==0 {print $1}' /etc/shadow
> ```
>
> /etc/sudoers 配置文件



####

> ```
>       
> ```
>
> yum\apt-get\curl \wget 区别
>
> /opt 用户级程序目录 不需要时可以 rm -rf 硬盘容量不够时也可以将opt单独挂载到其它磁盘&#x20;
>
> /usr 系统级目录 类似c:/windows&#x20;
>
> /usr/lib 类似 c:/windows/system32&#x20;
>
> /usr/local 用户级程序目录 类似 c:/progrem files&#x20;
>
> /usr/src 系统级的源码目录&#x20;
>
> /usr/local/src 用户级的源码目录
>
>
>
> 1.Linux系统用户组及权限划分&#x20;
>
> 2.软件源及安装包管理
>
> 3.软件编译运行&#x20;
>
> 4.网络配置排查&#x20;
>
> 5.进程排查&#x20;
>
> 6.ssh channl 手册&#x20;
>
> 7.linux 搭建svn 后checkout接入原理&#x20;
>
> 8.cdn 负载原理&#x20;
>
> 9.服务器ip限制
>
>
>
> reboot 重启 shutdown now 重启&#x20;
>
> 查看某程序进程 删除进程
>
> &#x20;uname -r 查看内核版本&#x20;
>
> sudo yum update 更新&#x20;
>
> systemctl set-default multi-user.target 设置开机文本模式&#x20;
>
> systemctl set-default graphical.target 设置开机图形界面&#x20;
>
> centos 7 字体设置 setfont sun12x22&#x20;
>
> 字体选择配置路径 /lib/kbd/consolefonts
>
> centos 7 命令行配置/分辨率 ……修改 boot/grub/gurb.conf 或者是 /boot/grub2/grub.cfg ……找到下面内容 nux16 /vmlinuz-3.10.0-123.el7.x86\_64 root=UUID=881ac4e6-4a55-47b1-b864-555de7051763 ro rd.lvm.lv=centos/swap vconsole.font=latarcyrheb-sun16 rd.lvm.lv=centos/root crashkernel=auto vconsole.keymap=us rhgb quiet LANG=en\_US.UTF-8 ……在UTF-8的末尾加上 vga=0x???(问好是分辨率代码) ……比如 0x340 800_600_32 ……reboot 重启
