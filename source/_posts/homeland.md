title: homeland 论坛部署填坑
date: 2017-05-09 15:14:56
tags:
banner:
---
[homeland](gethomeland.com) 是一个开源的, rails开发的论坛. 是 ruby-china 多年的运营开发中独立出来的项目. 具有诸多功能. 最近尝试搭建一个区域论坛尝试了一把. 部署经验总结如下.
<!-- more -->

### Install

官方推荐 Ubuntu 环境安装, 内存要求 4G (1G肯定不够). 官方推荐 Docker 安装方法, 安装方式参看后面链接或自行 Google

```shell
# 下载代码
git clone https://github.com/ruby-china/homeland-docker.git
cd homeland-docker/

# 安装依赖
sudo make install

# 启动
sudo make start
```


### 配置

Homeland 的应用程序配置主要基于 app.local.env, 在该文件中可以配置 app_name, 管理员邮箱, 启用模块, 用户信息, 邮箱 等多个配置.

```
admin_emails=admin@yourdomain.com
app_name=xaw123
modules=home,topic,press,wiki,site,note,team,editor.code
domain=bbs.xaw123.com
profile_fields=company,website,tagline,location,qq,weibo,wechat,battle_tag
```

另外管理员登录后可以在管理后台->通用配置论坛.

如果想调整各docker 启动配置可修改 docker-compose.yml


### ElasticSearch 配置
最开始的时候 ES 总是无故的挂掉, 通过 docker logs 查看说是 max file descriptor 不够, 
后来通过调高该配置(调整docker 全局打开文件数或docker 启动命令行参数, 或docker-compose 配置文件), 但还是没有解决问题.
最后发现是内存不够, 所以请务必确认机器配置是否足够

ES docker 配置开始log方式:
* http://stackoverflow.com/questions/40776433/cant-find-logs-in-elastic-search-docker-container
* https://www.elastic.co/guide/en/elasticsearch/reference/2.4/setup-configuration.html

### 邮箱, SSO, 云端存储文件配置
目前还没启用, 稍后补充

### MISC

```shell
# linux 查看版本, 内核信息
uname
uname -r
cat /etc/redhat-release   或  cat /etc/issue
```

* [安装docker compose](https://docs.docker.com/compose/install/)
* [安装docker](https://store.docker.com/editions/community/docker-ce-server-centos?tab=description)
* [centos 升级kernel](http://blog.csdn.net/reyleon/article/details/52229293)
* [查看centos版本信息](http://www.linuxidc.com/Linux/2014-12/110748.htm)
