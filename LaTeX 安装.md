
原文来自:<a href="https://github.com/seisman/seisman.info.posts/blob/master/_posts/2013-07-11-install-texlive-under-linux.md">seisman</a><br />
本文将介绍如何在 Linux 下安装 TeXLive 2017。

<!--more-->

## 依赖包

- 安装过程中需要调用 Perl 的模块 `Digest::MD5` 来检测 ISO 文件的完整性；
- 升级过程中界面需要调用 Perl 的模块 `Tk` ；

CentOS:

    $ sudo yum install perl-Digest-MD5 perl-Tk

Ubuntu:

    $ sudo apt-get install libdigest-perl-md5-perl perl-tk

## 安装

### 下载

下载地址：

- 官方镜像: [texlive2017.iso](http://mirrors.ctan.org/systems/texlive/Images/texlive2017.iso)
- USTC 镜像: [texlive2017.iso](http://mirrors.ustc.edu.cn/CTAN/systems/texlive/Images/texlive2017.iso)

Linux 下可以用 wget、axel，windows 下可以用迅雷，怎么快怎么来。

### 挂载 ISO 镜像

``` bash
$ su
# mount -o loop texlive2017.iso  /mnt/
# cd /mnt
# ./install-tl
```

出现选项后，输入 `I` 直接安装（也可以更改选项）。不出意外的话，5 分钟应该就 OK 了，
然后退出 root 用户。

### 环境变量

在当前用户的 `~/.bashrc` 中加入如下语句：

``` bash
# TeX Live 2017
export MANPATH=${MANPATH}:/usr/local/texlive/2017/texmf-dist/doc/man
export INFOPATH=${INFOPATH}:/usr/local/texlive/2017/texmf-dist/doc/info
export PATH=${PATH}:/usr/local/texlive/2017/bin/x86_64-linux
```

### 卸载 ISO 镜像

``` bash
$ cd
$ sudo umount /mnt/
```

## 更新 TeXLive

可以使用如下命令更新 TeXLive 宏包：

``` bash
$ su
# 更新 TeXLive 包管理器 tlmgr
# tlmgr update --self
# 更新 TeXLive 的全部包
# tlmgr update --all
```

默认情况下，会自动搜索合适的镜像来更新，也可以使用 `--repository` 选项指定了要使用
哪一个 CTAN 镜像。

比如 USTC 镜像:

    # tlmgr update --self --repository http://mirrors.ustc.edu.cn/CTAN/systems/texlive/tlnet/
    # tlmgr update --all --repository http://mirrors.ustc.edu.cn/CTAN/systems/texlive/tlnet/

比如阿里云镜像:

    # tlmgr update --self --repository http://mirrors.aliyun.com/CTAN/systems/texlive/tlnet/
    # tlmgr update --all --repository http://mirrors.aliyun.com/CTAN/systems/texlive/tlnet/

如果希望在图形界面下升级，可以使用如下命令调出 tlmgr 的中文图形界面：

``` bash
$ su
# tlmgr --gui --gui-lang zh_CN
```

## 安装额外的字体

TeXLive 2017 在使用 xeLaTeX 处理中文时，有自己的默认字体。大多数 Linux 发行版下，
都使用自带的 Fandol 字体。

如果想要使用 Windows 字体，可以将字体文件复制到 `~/.fonts` 目录下，并在 tex 源码中
指定字体选项即可。
或者：
sudo apt-get install latex-cjk-all 


## 卸载texlive
原文来自：<a href="https://tex.stackexchange.com/questions/95483/how-to-remove-everything-related-to-tex-live-for-fresh-install-on-ubuntu">stackexchange</a>

Try the following commands, one after another. If you progress, respective folders may already be deleted:
```bash
1. sudo apt-get purge texlive*
2. rm -rf /usr/local/texlive/* and rm -rf ~/.texlive*
3. rm -rf /usr/local/share/texmf
4. rm -rf /var/lib/texmf
5. rm -rf /etc/texmf
6. sudo apt-get remove tex-common --purge
7. rm -rf ~/.texlive
8. find -L /usr/local/bin/ -lname /usr/local/texlive/*/bin/* | xargs rm
```


This finds all the files in /usr/local/bin which point to a location within /usr/local/texlive/*/bin/* and removes them; because we’ve already deleted all of /usr/local/texlive, these are dead links. To see which files are being deleted, replace xargs rm with xargs -t rm (or tee off to a log file, or whatever).



Ubuntu 安装源已经打包了 TEX Live。对于大部分的用户，源里面的 TEX Live 安装简单，稳定性通常也足够，所以可以直接安装[1]。为了避免宏包依赖问题，推荐安装完整版（如果磁盘空间足够）：

``` bash
sudo apt-get install texlive-full
```
然而，源里面的 TEX Live 相比于「纯净版」，也有一些缺点：

相比于几乎每日都有更新的 CTAN，源里面的 TEX Live 更新较慢，频率接近每月一次，对于宏包开发者和有特殊需要的 TEX 用户是远远不够的
不使用 TEX Live 自带的包管理器，也就不能通过 tlmgr 安装、管理和更新宏包（当然实际上跟上一条说的是一件事）
下面就介绍安装「纯净版」TEX Live 的方法。

下载安装包
如果身在国内，推荐改用国内的镜像，比如清华大学的 tuna 镜像站。以下都以这个镜像为例。

在此处下载 install-tl-unx.tar.gz，解压并进入文件夹 install-tl-2018*。这里的 * 是一个日期。

命令行中的操作如下：

``` bash
wget https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/tlnet/install-tl-unx.tar.gz
tar -xzf install-tl-unx.tar.gz
cd install-tl-20*
```
安装 TEX Live
命令行中执行

``` bash
sudo ./install-tl -repository https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/tlnet/
```
注意需要管理员权限。如有必要，可能还需安装 perl-tl 和 perl-doc：

``` bash
sudo apt-get install perl-tk perl-doc
```
等待片刻后会进入选项菜单，根据需要酌情选取。也可以事先写好配置文件 texlive.profile[2]。

没有特殊需要的话，collection 可以不必全部安装，尤其是很多小语种。不过后果是之后可能会缺包。不愿意之后手动安装，并且空间足够、网速足够，也可以全部安装。注意 TEX Live 完全安装后大约要占 6 GB 空间，安装前请务必做好准备。中途断网很可能导致安装失败。

其他选项没有必要保持默认即可。

GUI 模式
开启 -gui 选项后可以在图形界面安装（当然前提是要有 GUI 支持）：

``` bash
sudo ./install-tl -gui -repository https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/tlnet/
```
如下图所示：

``` bash
texlive-gui
```
注意此时终端不可以关闭。


环境变量设置
此时 TEX Live 虽已安装，但其路径对于 Linux 来说仍是不可识别的。所以需要更改环境变量。

打开 ~/.bashrc，在最后添加

``` bash
export PATH=/usr/local/texlive/2020/bin/x86_64-linux:$PATH
export MANPATH=/usr/local/texlive/2020/texmf-dist/doc/man:$MANPATH
export INFOPATH=/usr/local/texlive/2020/texmf-dist/doc/info:$INFOPATH
```
还需保证开启 sudo 模式后路径仍然可用。命令行中执行

sudo visudo
找到如下一段代码

``` bash
Defaults        env_reset
Defaults        mail_badpass
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
```
将第三行更改为

``` bash

Defaults        secure_path="/usr/local/texlive/2018/bin/x86_64-linux:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
```
也就是加入 TEX Live 的执行路径。如果在安装时作了修改，这里的路径也都要与安装时的保持一致。

字体设置
要在整个系统中使用 TEX 字体，还需要将 TEX 自带的配置文件复制到系统目录下。命令行中执行

``` bash
sudo cp /usr/local/texlive/2018/texmf-var/fonts/conf/texlive-fontconfig.conf /etc/fonts/conf.d/09-texlive.conf
```
之后再执行

``` bash
sudo fc-cache -fv
```
刷新字体数据库。

安装「dummy package」

``` bash
TODO
```
检查
到此整个 TEX Live 2018 就已经安装完毕。可以做下面的一些检查：

基本命令

``` bash
tlmgr --version
pdftex --version
xetex --version
luatex --version
```
包管理器

``` bash
sudo tlmgr update --list
```
这一步是检查更新，如果有就顺手更了吧：

``` bash
sudo tlmgr update --self --all
```
--self 用来更新 tlmgr 自身，如果上一步没有提示可以不加这个选项。

Hello world
可以编译一个简短的测试文件：

``` bash
% hello.tex
\documentclass[UTF8]{ctexart}
\begin{document}
欢迎来到 \TeX{} 世界！
\end{document}
用 xelatex 或 lualatex 编译：

xelatex hello
lualatex hello
```
编译得到的 PDF 文件如果显示正常，则说明 TEX Live 基本工作正常。

