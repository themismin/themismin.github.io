---
title: Linux-iperf服务器网络性能测试
author: 小敏-mingo
tags:
  - Linux
  - Linux 命令
  - iperf
  - 测试
  - 
  - 排序
categories:
  - IT
  - 运维
abbrlink: f7bd0241
date: 2019-02-21 20:01:32
---
Iperf 是一个网络性能测试工具。Iperf可以测试最大TCP和UDP带宽性能，具有多种参数和UDP特性，可以根据需要调整，可以报告带宽、延迟抖动和数据包丢失。
<!-- more -->

## 前言
  > iperf 默认端口 5001，所以测试前需打开防火墙 5001 端口

## 教程
  > 安装
`yum install iperf`

  > 参数文档
```
# 服务端专用选项
-s, --server Iperf服务器模式

# 客户端专用选项
-c, --client host	运行Iperf的客户端模式，连接到指定的Iperf服务器端。
-t, --time #	设置传输的总时间。Iperf在指定的时间内，重复的发送指定长度的数据包。默认是10秒钟。参考-l与-n选项。

# 共用选项
-i, --interval #	设置每次报告之间的时间间隔，单位为秒。如果设置为非零值，就会按照此时间间隔输出测试报告。默认值为零。
```

  > 命令-结果
```
# 服务器端
[xxx@server]# iperf -s -i 2 -f M
------------------------------------------------------------
Server listening on TCP port 5001
TCP window size: 0.08 MByte (default)
------------------------------------------------------------
[  4] local 192.168.0.101 port 5001 connected with 192.168.0.100 port 56006
[ ID] Interval       Transfer     Bandwidth
[  4]  0.0- 2.0 sec   457 MBytes   229 MBytes/sec
[  4]  2.0- 4.0 sec   430 MBytes   215 MBytes/sec
[  4]  4.0- 6.0 sec   370 MBytes   185 MBytes/sec
[  4]  6.0- 8.0 sec   357 MBytes   179 MBytes/sec
[  4]  8.0-10.0 sec   363 MBytes   182 MBytes/sec
[  4]  0.0-10.1 sec  1981 MBytes   197 MBytes/sec
[  4] local 192.168.0.101 port 5001 connected with 10.100.100.101 port 48270
[  4]  0.0- 2.0 sec  70.5 MBytes  35.2 MBytes/sec
[  4]  2.0- 4.0 sec  52.4 MBytes  26.2 MBytes/sec
[  4]  4.0- 6.0 sec  51.0 MBytes  25.5 MBytes/sec
[  4]  6.0- 8.0 sec  45.1 MBytes  22.5 MBytes/sec
[  4]  8.0-10.0 sec  53.0 MBytes  26.5 MBytes/sec
[  4]  0.0-10.2 sec   274 MBytes  27.0 MBytes/sec
```

```
# 客户端
[xxx@server ~]# iperf -c 192.168.0.101 -i 2 -f M
------------------------------------------------------------
Client connecting to 192.168.0.101, TCP port 5001
TCP window size: 0.18 MByte (default)
------------------------------------------------------------
[  3] local 192.168.0.100 port 56006 connected with 192.168.0.101 port 5001
[ ID] Interval       Transfer     Bandwidth
[  3]  0.0- 2.0 sec   459 MBytes   229 MBytes/sec
[  3]  2.0- 4.0 sec   431 MBytes   216 MBytes/sec
[  3]  4.0- 6.0 sec   370 MBytes   185 MBytes/sec
[  3]  6.0- 8.0 sec   358 MBytes   179 MBytes/sec
[  3]  8.0-10.0 sec   363 MBytes   181 MBytes/sec
[  3]  0.0-10.0 sec  1981 MBytes   197 MBytes/sec
[xxx@server ~]# iperf -c 10.100.100.100 -i 2 -f M
------------------------------------------------------------
Client connecting to 10.100.100.100, TCP port 5001
TCP window size: 0.39 MByte (default)
------------------------------------------------------------
[  3] local 192.168.0.100 port 48270 connected with 10.100.100.100 port 5001
[ ID] Interval       Transfer     Bandwidth
[  3]  0.0- 2.0 sec  72.9 MBytes  36.4 MBytes/sec
[  3]  2.0- 4.0 sec  52.8 MBytes  26.4 MBytes/sec
[  3]  4.0- 6.0 sec  50.6 MBytes  25.3 MBytes/sec
[  3]  6.0- 8.0 sec  44.8 MBytes  22.4 MBytes/sec
[  3]  8.0-10.0 sec  53.0 MBytes  26.5 MBytes/sec
[  3]  0.0-10.1 sec   274 MBytes  27.0 MBytes/sec
```
  
  > 以上测试数据分析，内网带宽 25MBytes 左右，外网带宽 200MBytes 左右
  
  *PS: 1Byte=8bit*
```
其中大B代表Byte(字节)，小b代表bit(比特 或 位)。
1 Kb = 1024 bit 
1 Mb = 1024 Kb 

1 KB = 1024 Byte 
1 MB = 1024 KB 

1 Byte = 8 bit
1 MB = 8Mb
```
