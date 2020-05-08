---
layout: post
title: "centos6安装python3.6"
author: "Daituodi"
header-style: text
tags:
  - 环境配置
---

**1、安装Python前的库环境**

```shell
yum install gcc patch libffi-devel python-devel  zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel -y
yum install openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel readline-devel.x86_64 -y
```

**2、下载Python源码包**

```shell
cd /opt/
wget https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tgz
```

**3、 安装Python**

```shell
tar zxvf Python-3.6.5.tgz
cd Python-3.6.5
./configure --prefix=/usr/local/python3
make && make install
```

**4、设置软连接**

```shell
ln -s /usr/local/python3/bin/python3.6 /usr/local/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/local/bin/pip3
```

**5、查看python3版本以及pip3版本**

```shell
python3
pip3 -V
```

**6、更新pip3版本**

```shell
pip3 install --upgrade pip
```

**7、换源**

```shell
cd ~
mkdir .pip
cd .pip
vi pip.conf

[global]
index-url=https://pypi.douban.com/simple
```

