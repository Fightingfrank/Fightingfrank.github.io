---
layout:     post
title:      Centos升级Python3
subtitle:   Centos默认Python版本升级
date:       2017-4-20
author:     CoderFrank	
header-img: img/post-bg-desk.jpg
catalog: true
tags:
    - Python
    - Linux
    - 环境搭建
---




# centos升级python3



由于centos7系统默认安装的是python2，有时候需要另外安装python3，但不可删除python2，因为yum命令是基于python2的。
由于目前pip上的python大多都没有更新到python3，我们通过下载源码自己编译的方式进行安装：

```bash
$ wget https://www.python.org/ftp/python/3.5.1/Python-3.5.2.tar.xz
```
<font color=red>注：这里看到我们下载的python包以tar.xz结尾，那么下面就简单的介绍下.tar;tar.gz;.tgz;.gz;tar.xz;.Z;.bz2;.zip这几种后缀名文件的不同：</font>

#### 1、一些常见的压缩文件使用命令介绍

因为linux系统支持的压缩命令非常多，不同的命令所用的压缩技术也不一样，下载哪种后缀的压缩文件就需要使用对应的解压缩命令进行解压.


后缀名 | 代表意思
----  | ------
.Z    |compress程序压缩的文件 
.gz   |gzip程序压缩的文件 
.bz2  |bzip2程序压缩的文件
.tar  |tar程序打包的数据，<font color=red>并没有压缩过</font>
.zip | zip程序压缩的数据
.7z | 极高压缩比的压缩方式
.xz | 无损数据压缩方式 


下面介绍不同压缩方式的操作命令：

- .Z

```bash
$ compress [-rcv] 文件或目录  -->压缩
$ uncompress 文件.Z -->解压
参数：
-r : 连同目录下的文件也同时被压缩
-c : 将压缩数据输出成为standard output（输出到屏幕）
-v : 可以显示出压缩后文件的信息以及压缩过程中的文件名变化
```

- .gz

```bash
$ gzip filename.tar 压缩
$ gunzip filename.tar.gz  解压

一次性打包压缩和解压解包,即将tar打包过程和压缩过程一起完成
--c create创建
--x extract解包
--v verbose详细信息
--f filename文件名
$ tar -zcvf aim_filename.tar.gz original_filename(directory) 打包并压缩
$ tar -zxvf filename.tar.gz  解压并解包
```


- .bz2

```bash
$ bzip2 filename.tar
$ bunzip2 filename.tar.bz2

$ tar -jcvf aim_filename.tar.bz2 original_filename(directory)  打包并压缩
$ tar -jxvf filename.tar.bz2  解压并解包
``` 

- .xz

```bash
$ xz filename.tar  压缩
$ unxz filename.tar.xz  解压


$ tar -Jcvf aim_filename.tar.xz original_filename/directory 打包并压缩
$ tar -Jxvf filename.tar.xz 解压并解包
```


- .tar

前面的.bz2，.xz，.gz对目录的压缩指的是对目录内的所有文件<font color=red>“分别”</font>进行压缩，而不是将多个文件压缩成一个文件。

tar就是打包多个文件成一个文件，所以往往我们都是先打包，再压缩。

```bash
$ tar -cvf aim_filename.tar original_filename(directory)打包
$ tar -xvf filename.tar 解包
```

- .7z

```bash
$ 7z a 目标文件名.7z 原文件名／目录名  压缩
$ 7z x 原文件名.7z
```
<font color=red>注：7z解压命令支持rar格式：```7z x 文件名.rar```</font>


- .zip

```bash
$ zip -r 目标文件名.zip 原文件／目录名
$ unzip 文件名.zip
```


### 2、python的安装

前面我们下载了xz格式的python源码包，这里使用上面介绍的命令进行解压：

```bash
$ tar Jxvf Python-3.5.2.tar.xz
```

现在我们需要安装一些编译python包时需要的编译环境，根据官网的介绍，ubuntu需要以下包：
![](http://ofmzs1ffp.bkt.clouddn.com/ADF9F6B6-3803-4287-8366-E5781198D22D.png)

但是实际安装中发现安装该文章所写的[地址](http://www.jianshu.com/p/8bd6e0695d7f)包也能安装成功。
其中安装的'Development Tools'是一些常用的工具包，可以使用一下命令查看具体内容:

```bash
$ yum groupinfo 'Development Tools查看'
```
具体结果如下：
![](http://ofmzs1ffp.bkt.clouddn.com/F1E6CDB0-DD47-42CE-8E0D-8849C52D3BB3.png)

可以看到必装的包里面是一些make包，gcc之类的工具，如果你的系统没有安装这些包，怕麻烦的话可以直接装这个Development Tools，如果已安装，可以跳过这个步骤。

直接:

```bash
yum install zlib-devel bzip2-devel openssl-devel ncurese-devel sqlite-devel
```

然后进行编译：

```bash
cd Python-3.5.2
./configure --prefix=/usr/local/python3
make && make install
```
如果在编译过程中出了问题，找到具体的位置，查看是哪些需要的模块没有安装，```make clean```清除make编译的文件后安装该模块，再重新编译

编译完成后更换python默认的版本：

```bash
备份 mv /usr/bin/python /usr/bin/python2.7

新建指向python3和pip的软链接 
ln -s /usr/local/python/bin/python3.5 /usr/bin/python
ln -s /usr/local/python/bin/pip3 /usr/bin/pip
可以使用 python --version; pip --version查看版本
```

在更新到python3后我们发现yum用不了了，并且在使用过程中可能会报出某个文件某个模块丢失的情况，这一般都是由于该文件使用的是python2.7，而我们改了默认的python版本，这些文件无法正常运行了。所以需要修改回来：

```bash
$ vi /usr/bin/yum

把第一行的python改为python2.7
```
其他类似的错误都如此处理。




