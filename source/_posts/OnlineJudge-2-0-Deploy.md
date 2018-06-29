---
title: OnlineJudge 2.0 Deploy
date: 2018-03-09 19:30
tags: 
    - OnlineJudge
    - Linux
---

# OnlineJudge 2.0 document

## 环境搭建
系统：CentOS 7
环境：Docker

<!-- more -->

### 安装基础工具
```bash
yum update -y && yum upgrade -y
yum install vim curl git -y
```

### 安装 `python3` 及 `pip3`
[CentOS7 Python3.6.3 install](http://aoiyuki.org/CentOS-7-python3-6-3-install.html)

### 安装 `docker` 及 `docker-compose`
```bash
curl -sSL https://get.daocloud.io/docker | sh
echo "DOCKER_OPTS=\"\$DOCKER_OPTS --registry-mirror=http://f2d6cb40.m.daocloud.io\"" | sudo tee -a /etc/default/docker
pip3 install docker-compose
```

## 启动OJ
### 配置及启动 `docker-compose`
```bash
git clone -b 2.0 https://github.com/QingdaoU/OnlineJudgeDeploy.git
cd OnlineJudgeDeploy
systemctl start docker
vim ./docker-compose.yml
docker-compose up -d
```

### OJ状态检测
```bash
cd OnlineJudgeDeploy
docker ps
```

## OJ使用指南
### 学生
普通的注册，提交，应该没啥问题。
### 管理员
管理员通过右上角账号下来菜单的`Management`进入后台
#### 用户管理
管理员拥有更改用户的姓名，密码，邮箱等资料的权限
管理员拥有发布公告的权限
管理员拥有对OJ做一些简单更改的权限
管理员拥有启用/关闭判题机的权限
#### 题目管理
管理员拥有题目的增删改查的权限
管理员拥有创建比赛的权限

### 注意事项
- 题目创建支持markdown语法及MathJax，但有一些细微的区别，请注意使用。
- 测试数据必须以`zip`压缩包的形式上传，压缩包内的文件命名规则为`序号.in/序号.out`的格式，压缩包内不得存在任何文件夹。
- C/Cpp代码必须返回0
- Java题目的内存必须超过256Mb
- 题目必须有一个标签(分类)
- 比赛模式
 + ACM模式
 在该模式下,我们严格按照ACM-ICPC的比赛规则来进行，Contest设置项中的real time rank即为是否封榜，封榜后将不再刷新排名。
 + OI 模式
 在OI模式下，选手的提交将根据得分点来计分，多次提交以最后一次提交为准，排名规则为多个题目的总分数。
 为了照顾OI模式五花八门的规则， 也由于OI模式下没有封榜一说，我们为OI模式下的real time rank赋予了特殊的意义:
 当real time rank开启时，选手能在提交后立即看到评判结果，并可以查看Rankings， Submissions等信息(类似ACM，只是按得分点计分了)， 当real time rank关闭时， 比赛参与者在提交后将不能获取结果，直到比赛结束才可以查看。
- 用户导入
 不带头的`csv`格式文件，包含三列，`username`, `password`, `Email`
 
 ### 系统升级
 ```bash
 cd OnlineJudgeDeploy
 git pull
 docker-compose pull
 docker-compose up -d
 ```