# 备用文档



{% tabs %}
{% tab title="Hydra" %}
**FTP** 默认**测试口令**: anymouse:anymouse

```
hydra -L ftpuser.txt -P ftppwd.txt -t 16 -f ftp://ip
hydra.exe -l administror -P c:\pass.txt www.test.com rdp -V
```
{% endtab %}

{% tab title="AWVS/Nessus" %}


<pre><code><strong>docker pull leishianquan/awvs-nessus:v2</strong></code></pre>

**启动**

<pre data-overflow="wrap"><code><strong>docker run --name awvs-nessus -it -d -p 13443:3443 -p 8834:8834 leishianquan/awvs-nessus:v2</strong></code></pre>

**查看容器**

```
docker ps –a
```

**启动容器**

```
docker start 21d3887c79cc
docker start container-id. ff3bd861acb5
```

**进入容器**

```
> docker exec –id 21d3887c79cc  /bin/bash
```

**us**

```
/etc/init.d/nessusd start
```

**访问地址**

```
Nessus:
https://127.0.0.1:8834/#/
account:leishi/leishianquan

Awvs13:
https://127.0.0.1:13443/
account:admin@admin.com/Admin123
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="upload-labs" %}
* 安装docker
* service docker start
* git clone https://github.com/c0ny1/upload-labs
* cd upload-labs/docker
* Dockerfile镜像配置\
  RUN sed -i "s/archive.ubuntu./serdockemirrors.aliyun./g" /etc/apt/sources.list RUN sed -i "s/deb.debian.org/mirrors.aliyun.com/g" /etc/apt/sources.list RUN sed -i "s/security.debian.org/mirrors.aliyun.com/debian-security/g" /etc/apt/sources.list RUN sed -i "s/httpredir.debian.org/mirrors.aliyun.com/debian-security/g" /etc/apt/sources.list
* docker build -t upload-labs .
* docker run -d -p 80:80 upload-labs:latest
{% endtab %}

{% tab title="bwapp" %}
先localhost 访问install.PHP 文件创建数据库

若果提示 connection failed

手动创建 库及表
{% endtab %}

{% tab title="sqli-labs " %}
sqli-labs 数据库配置 docker ps -a docker exec -it containerID\[402103a4b87] /bin/bash

cd /var/www/html/sql-connections

找 db-creds.inc 用vi 确认用户名密码配置 user root pass ‘ ‘ name security dbnamel challenges

php setup-db.php

1: wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm

2: rpm -ivh mysql-community-release-el7-5.noarch.rpm 3.yum install mysql-server
{% endtab %}

{% tab title="vulstudy" %}
cd vulstudy docker-compose up -d #启动容器&#x20;

docker-compose stop #停止容器 或者 docker-compose down stop
{% endtab %}
{% endtabs %}
