---
title: CentOS 7 python3.6.3 install
date: 2018-03-01 00:53:55
tags: 
  - Linux 
  - Python3
---

首先需要一些基本的家具，比如`vim`, `git`, `g++`, `zlib`, `python`, `python-pip` 等，那么让我们一个一个的开始吧！

<!-- more -->

```shell
yum install vim -y
yum install git -y
yum install gcc-c++ -y
yum install zlib-devel -y
yum install openssl-devel -y
echo "/usr/include/openssl/" >> /etc/ld.so.conf
ldconfig
```
这几个工具安装完成后，开始安装 `python` 和 `python-pip` 。我更喜欢 `python3`，所以在这里我选择编译安装 `python3`：
```shell
cd ~
mkdir Python3
cd Python3
wget https://www.python.org/ftp/python/3.6.3/Python-3.6.3.tgz
tar -xzvf Python-3.6.3.tgz
cd Python-3.6.3
./configure --prefix=/usr/local/python3 --enable-optimizations
make && make install
ln -s /usr/local/python3/bin/python3 /usr/bin/python3
```
至此，我们还需要修改一个文件来配置环境变量
```shell
vim ~/.bash_profile
```
会打开名为 `.bash_profile` 的文件，修改内容如下：
>```
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

PATH=$PATH:$HOME/bin:/usr/local/python3/bin
PATH=$PATH:$HOME/bin

export PATH
```

最后启用这个修改就完成了！
```shell
source ~/.bash_profile
```
