---
title: Notes
date: 2016-11-01 22:28:20
update: 2016-11-01 22:28:20
categories: Notes
tags: [terminal, command]
---
# 1.查看端口号并关闭相关进程
> 1.查看端口占用

在windows命令行窗口下执行：  

```
netstat -aon|findstr "8080"
TCP     127.0.0.1:80         0.0.0.0:0               LISTENING       2448
```
端口“8080”被PID（进程号）为2448的进程占用。  
查看端口“8080”被哪个应用占用，，继续执行下面命令：
```
tasklist|findstr "2448"
notepad.exe                     2016 Console                 0     16,064 K
```

<!-- more -->
> 2.关闭进程

2.1 按进程号关闭进程

```
taskkill /pid 2152
```
多个时格式为：
```
taskkill /pid 2152 /pid 1284

```

2.2 按进程名关闭进程

如要关闭notepad.exe,格式为：

```
taskkill /im notepad.exe
```
指定多个时格式为：
```
taskkill /im notepad.exe /im iexplorer.exe
```

如果是要关闭所有的,则使用通配符*,即：
```
taskkill /im *.exe
```

2.3 有提示的关闭进程

```
taskkill /t /im notepad.exe
taskkill /t /pid 2152

```
这个效果是提示后在使用者确定后关闭,有提示框。
2.4 强行终止进程
```
taskkill /f /im notepad.exe
taskkill /f /pid 2152

```
2.5 关闭一个.bat文件

为mvnDebug.bat文件添加title为mvn
部分内容如下
```
@echo off
title mvn
@REM enable echoing my setting MAVEN_BATCH_ECHO to 'on'
@if "%MAVEN_BATCH_ECHO%" == "on"  echo %MAVEN_BATCH_ECHO%
```
关闭mvnDebug.bat文件
执行命令
```
C:\Users\Administrator>taskkill /fi  "windowtitle eq mvn"
```
windowtitle eq mvn
eq 后面指定需要关闭的.bat文件的title即可

> 3.端口状态


3.1 LISTENING状态  
FTP服务启动后首先处于侦听（LISTENING）状态。

3.2 ESTABLISHED状态  
ESTABLISHED的意思是建立连接。表示两台机器正在通信。
3.3 CLOSE_WAIT  
对方主动关闭连接或者网络异常导致连接中断，这时我方的状态会变成CLOSE_WAIT 此时我方要调用close()来使得连接正确关闭
3.4 TIME_WAIT  
我方主动调用close()断开连接，收到对方确认后状态变为TIME_WAIT。TCP协议规定TIME_WAIT状态会一直持续2MSL(即两倍的分段最大生存期)，以此来确保旧的连接状态不会对新连接产生影响。处于TIME_WAIT状态的连接占用的资源不会被内核释放，所以作为服务器，在可能的情况下，尽量不要主动断开连接，以减少TIME_WAIT状态造成的资源浪费。
目前有一种避免TIME_WAIT资源浪费的方法，就是关闭socket的LINGER选项。但这种做法是TCP协议不推荐使用的，在某些情况下这个操作可能会带来错误。
3.5 SYN_SENT状态  
SYN_SENT状态表示请求连接，当你要访问其它的计算机的服务时首先要发个同步信号给该端口，此时状态为SYN_SENT，如果连接成功了就变为ESTABLISHED，此时SYN_SENT状态非常短暂。但如果发现SYN_SENT非常多且在向不同的机器发出，那你的机器可能中了冲击波或震荡波之类的病毒了。这类病毒为了感染别的计算机，它就要扫描别的计算机，在扫描的过程中对每个要扫描的计算机都要发出了同步请求，这也是出现许多SYN_SENT的原因。
