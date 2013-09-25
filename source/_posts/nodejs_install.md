title: How to install N ?
date: 2013-09-21 18:09:25
tags: 安装
---

N 是跨平台的, 支持 Linux, OSX, Windows, SunOS (也支持 ARM,但还不成熟,本文略过). 以上环境均支持 32 位和 64 位.  
安装内容包括: 执行环境`node`, 包管理工具`npm`.

N 大致有以下几种安装方式:

* Installer
* 系统软件管理工具
* 版本管理工具
* 源码编译安装
* 其他

相关链接: [下载地址](http://nodejs.org/download/), [Github源码](https://github.com/joyent/node)


## Installer
N 有 OSX, Windows 平台的 Installer, 可直接下载对应版本安装.  
Linux, SunOS 有编译好的二进制文件, 下载解压后可放到任意位置, 然后将`N路径/bin`添加到PATH环境变量即可.

## 系统软件管理工具
OSX系统可使用 [Homebrew](http://brew.sh) 安装:

    brew install node

Ubuntu系统可使 apt-get 安装:

    sudo apt-get install nodejs npm

[详情参看](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager)


## 版本管理工具安装
Ruby, Python都有自己的版本管理工具rvm, virtualenv. 用于安装,管理不同版本环境, 版本之间快速切换等操作. 
对于版本更迭频繁的N来说, 使用版本管理工具进行安装是不错的选择. 这里介绍N的版本管理工具 [nvm](https://github.com/creationix/nvm)

### nvm 安装
Install nvm (use git):

    curl https://raw.github.com/creationix/nvm/master/install.sh | sh

or Wget:

    wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh

这个脚本会下载nvm到~/.nvm, 然后在profile中添加source line

### nvm 使用

    nvm install 0.10    # 安装0.10版本
    nvm use 0.10        # 使用0.10版本
    nvm uninstall 0.10  # 卸载0.10版本

nvm 的具体安装和使用参看 [nvm GitHub](https://github.com/creationix/nvm)

另外[n](https://github.com/visionmedia/n), [nave](https://github.com/isaacs/nave) 也是 N 的版本管理工具



## 编译安装
首先到官网或 GitHub 下载源码
### Unix

#### Prerequisites:

* GCC 4.2 or newer
* Python 2.6 or 2.7
* GNU Make 3.81 or newer
* libexecinfo (FreeBSD and OpenBSD only)

In Mac:

1. 安装 Xcode (包含编译依赖库)
2. 安装 git (用于从 GitHub 获取代码)

In Ubuntu:

1. sudo apt-get install g++ curl libssl-dev apache2-utils
2. sudo apt-get install git-core

In CentOS:

1. yum install gcc-c++

#### Build
Unix/Macintosh:

    ./configure
    make
    make install

如果 python 没有在标准位置或者没有标准的名字, 需要使用如下命令编译

    export PYTHON=/path/to/python
    $PYTHON ./configure
    make
    make install

详细信息参看 [GitHub源码](https://github.com/joyent/node)


## 其他
有几个大公司提供了集成 N 的产品:

* [StrongLoop](http://strongloop.com/strongloop-suite/downloads/)
* Microsoft's [Webmatrix](http://www.microsoft.com/web/webmatrix/)


## 测试
在终端输入以下命令如果能看到版本号, 即安装成功:

    node -v
    # or
    node --version


## Tips

* N 部分 Module 需要编译 在 Windows 下很难或无法安装, 建议使用 OSX 或 Linux 开发


## 常见问题

## 相关博客

* [howtonode.org install instruction](http://howtonode.org/how-to-install-nodejs) (2011.3.11)
* [CentOS install instruction](https://www.digitalocean.com/community/articles/how-to-install-and-run-a-node-js-app-on-centos-6-4-64bit)
