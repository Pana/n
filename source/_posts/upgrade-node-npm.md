title: Node & NPM upgrade
date: 2014-03-25 17:05:45
tags:
---

Node 和 Npm 升级节奏都非常快, 因此版本升级是 Noder 经常回碰到的事情.

## NPM
npm 升级非常方便, 直接使用 npm 就可以

```
$ npm update -g npm
```

npm 卸载方法如下
```
$ sudo npm uninstall npm -g
```
如果该方法失败, 可以先获取 npm 源代码, 然后
	
    $ sudo make uninstall


## Node
关于Node的安装[之前](http://n.thepana.com/2013/09/21/nodejs_install/)有详细的介绍, 具体方法可以移步过去看看.

#### Installer, 系统软件管理工具, 版本管理工具
Mac 或 windows 使用 Installer 安装, 卸载或直接覆盖都比较容易

如果使用 `brew, apt-get` 等系统管理软件, 都系统卸载或升级命令, 升级比较简单

nvm, n 等版本管理工具基本没有升级的问题

#### 下载binary, 添加PATH
如果是下载binary, 添加PATH, 只需要将 node 目录换成最新即可


#### 编译安装
源码安装, 如果还保留源代码可以在, 源代码目录执行如下命令卸载

	$ sudo make uninstall
    
如果安装源码已经删掉, 则需要手动将所有node相关文件删掉. 先使用 which 命令找到 node 路径

	$ which node

然后进入安装目录删掉相关文件

	$ rm -r bin/node bin/node-waf include/node lib/node lib/pkgconfig/nodejs.pc share/man/man1/node.1
    
其他位置 node_modules 手动删除即可

参考:

* [uninstall-node-js-using-linux-command-line](http://stackoverflow.com/questions/5650169/uninstall-node-js-using-linux-command-line)
* [Removing Node.Js from Mountain Lion](http://stackoverflow.com/questions/14673327/removing-node-js-from-mountain-lion)
* [How do I completely uninstall Node.js, and reinstall from beginning (Mac OS X)](http://stackoverflow.com/questions/11177954/how-do-i-completely-uninstall-node-js-and-reinstall-from-beginning-mac-os-x)