
<!--more-->

> **Screen** 是一款Linux下的全屏幕窗口管理软件。我在使用服务器（**SSH** 远
程连接）的过程中发现这类的软件可以提供很大的帮助，因此这里介绍一下如何使用
以及一些相关的配置。

# **1.** **Screen** 简介
**Screen** 是 **Linux** 下的一款全屏幕窗口管理软件，该软件可以用来在一个
物理终端下开启多个虚拟终端，并且能够实现不同虚拟终端之间的自由切换。此外，
**Screen** 还可以实现将终端暂时放入后台及重新连接并显示终端内容。在我的
日常使用的过程中，主要是借助 **Screen** 来完成以下的几个方面的内容：

- 需要在某一个终端中开启多个虚拟终端。例如，**SSH** 登陆服务器后开启多个
虚拟终端
- 需要把某一或者某些终端放在后台挂起，在需要时再次进行连接。例如，**SSH** 登
陆服务器后修改了某些文件但是未完全完成，需要后续继续修改时。

# **2.** **Screen** 的安装及使用
## **2.1** **Screen** 的安装
对于很多的 **Linux** 系统的发行版，**Screen** 不需要额外的安装。我使用的是
**Ubuntu** 系统，如果也是使用 **Ubuntu** 系统，可以通过下列的命令进行安装:
```
sudo apt install screen
```
对于其他的发行版，基本上都可以使用对应的安装命令进行安装，这里就不再详细
介绍了。

## **2.2** **Screen** 的使用
**Screen** 的详细用法可以通过 `man screen` 命令进行查看。这里主要是介
绍 **Screen** 比较常用的用法。

### **1.** 开启一个新的 **Screen** 终端

如果想打开一个新的 **Screen** 终端，只需要在命令行输入下面的命令即可：
```
screen
```

### **2.** 查询目前的 **Screen** 终端及其状态

可以通过下面的命令来查看当前系统中所有的 **Screen** 终端及其状态：
```
screen -ls
```
其结果如下所示
```
There are screens on:
	20116.pts-2.cwz-E450c	(10/08/2019 10:03:52 PM)	(Attached)
	12831.pts-2.cwz-E450c	(10/08/2019 07:51:27 PM)	(Detached)
2 Sockets in /run/screen/S-cwz.
```
可以看到当前一共存在2个 **Screen** 终端，其中 `20116` 和 `12831` 分别表示
的是两个 **Screen** 终端的 **ID**。`Attached` 表示该终端处于连接的状态，
或者可以理解成目前显示在某一实际的终端中，`Detached` 表示该终端处于断开的
状态，也就是目前属于在后台中运行，没有直接把终端的信息显示在某一实际的
终端上。
{{% admonition tip "Tip" %}}
**ID** 是比较重要的，当终端较多时，需要通过 **ID** 可以分辨出具体的某一个
终端。
{{% /admonition %}}


### **3.** 断开一个开启的 **Screen** 终端

如果想断开一个已经开启的 **Screen** 终端，可以使用下面的命令：
```
screen -d [ID]
```

### **4.** 重新连接某个 **Screen** 终端

如果想重新连接一个已经断开的 **Screen** 终端，可以使用下面的命令：
```
screen -r [ID]
```

### **5.** 退出 **Screen** 终端

当使用完 **Screen** 终端后需要退出时，可以在已经开启的 **Screen** 终端中
输入 `exit` 即可退出当前的 **Screen** 终端。

### **6.** **Screen** 快捷键
在 **Screen** 的日常使用中，更多的命令是需要使用快捷键来实现新建虚拟终端，
终端间的切换等一系列操作，下面简单列举出相关的操作。
```
C-a ? -> 显示所有键绑定信息

C-a c -> 创建一个新的运行shell的窗口并切换到该窗口

C-a n -> Next，切换到下一个 window

C-a p -> Previous，切换到前一个 window

C-a 0..9 -> 切换到第 0..9 个 window

C-a [Space] -> 由视窗0循序切换到视窗9

C-a C-a -> 在两个最近使用的 window 间切换

C-a x -> 锁住当前的 window，需用用户密码解锁

C-a d -> detach，暂时离开当前session，将目前的 screen session (可能含有多个 windows) 丢到后台执行

C-a w -> 显示所有窗口列表

C-a k -> kill window，强行关闭当前的 window

# the version 4 has this option.
C-a | -> Split the current region vertically into two new onws, 将窗口左右分屏
```

## **2.3** **Screen** 的配置
**Screen** 的配置文件可以分为两种，一种是系统默认的配置文件，其路径
为 `/etc/screenrc`，另外是个人目录下的配置文件，路径为 `~/.screenrc`。可以在这两个
文件中添加相应的语句来实现修改默认的 **Screen** 配置。通常会配置在终端的底端显示
虚拟终端的个数以及时间等信息，否则，**Screen** 默认的配置不会显示这些信息。下面
给出一个我使用的比较简单的配置文件，更加具体的配置的设置可以查询相关的文档来获得。
```
#
# ~/.screenrc
# skip the startup message
startup_message off

# Automatically detach on hangup.
autodetach on

# If a screen dies, don't freeze the whole screen waiting for it.
nonblock on

# UTF-8 is necessary.
defutf8 on

# Change default scrollback value for new windows: scrollback 10000
defscrollback 8192

termcapinfo Eterm 'AF=\E[3%p1%dm:AB=\E[4%p1%dm'
termcapinfo Eterm ti@:te@
termcap  xterm 'AF=\E[3%dm:AB=\E[4%dm'
terminfo xterm 'AF=\E[3%p1%dm:AB=\E[4%p1%dm'
termcapinfo xterm ti@:te@
vbell off
deflogin off


hardstatus on
hardstatus alwayslastline

activity            "activity in %n (%t) [%w:%s]~"
```
