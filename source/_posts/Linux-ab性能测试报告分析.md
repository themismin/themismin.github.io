---
title: Linux-ab性能测试报告分析
author: 小敏-mingo
tags:
  - Linux
  - Linux 命令
  - ab
  - Apache Bench
  - 测试
  - 性能测试
categories:
  - IT
  - 运维
abbrlink: e1d2de2b
date: 2019-02-21 22:05:02
---
apache ab（Apache Bench）性能测试工具，这是[apache]免费自带的性能测试工具，就在apache的bin目录下，它能模拟多个并发请求，也就是说它主要是用来测试你的apache每秒能处理多少请求的。
<!-- more -->

## 命令文档
```
Usage: ab [options] [http[s]://]hostname[:port]/path
Options are:
    -n requests     要执行的请求数(Number of requests to perform)
    -c concurrency  一次发出多个请求的数量(Number of multiple requests to make at a time)
    -t timelimit    Seconds to max. to spend on benchmarking
                    This implies -n 50000
    -s timeout      Seconds to max. wait for each response
                    Default is 30 seconds
    -b windowsize   Size of TCP send/receive buffer, in bytes
    -B address      Address to bind to when making outgoing connections
    -p postfile     File containing data to POST. Remember also to set -T
    -u putfile      File containing data to PUT. Remember also to set -T
    -T content-type Content-type header to use for POST/PUT data, eg.
                    'application/x-www-form-urlencoded'
                    Default is 'text/plain'
    -v verbosity    How much troubleshooting info to print
    -w              Print out results in HTML tables
    -i              Use HEAD instead of GET
    -x attributes   String to insert as table attributes
    -y attributes   String to insert as tr attributes
    -z attributes   String to insert as td or th attributes
    -C attribute    Add cookie, eg. 'Apache=1234'. (repeatable)
    -H attribute    Add Arbitrary header line, eg. 'Accept-Encoding: gzip'
                    Inserted after all normal header lines. (repeatable)
    -A attribute    Add Basic WWW Authentication, the attributes
                    are a colon separated username and password.
    -P attribute    Add Basic Proxy Authentication, the attributes
                    are a colon separated username and password.
    -X proxy:port   Proxyserver and port number to use
    -V              Print version number and exit
    -k              Use HTTP KeepAlive feature
    -d              Do not show percentiles served table.
    -S              Do not show confidence estimators and warnings.
    -q              Do not show progress when doing more than 150 requests
    -g filename     Output collected data to gnuplot format file.
    -e filename     Output CSV file with percentages served
    -r              Don't exit on socket receive errors.
    -h              Display usage information (this message)
    -Z ciphersuite  Specify SSL/TLS cipher suite (See openssl ciphers)
    -f protocol     Specify SSL/TLS protocol
                    (SSL3, TLS1, TLS1.1, TLS1.2 or ALL)
```

## 命令-结果
> 第一组测试数据，替换对应域名(FPM 子进程上限 200)
```
[xxx@server ~]# ab -n4000 -c100 https://www.baidu.com/index
This is ApacheBench, Version 2.3 <$Revision: 1430300 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking www.baidu.com (be patient)
Completed 400 requests
Completed 800 requests
Completed 1200 requests
Completed 1600 requests
Completed 2000 requests
Completed 2400 requests
Completed 2800 requests
Completed 3200 requests
Completed 3600 requests
Completed 4000 requests
Finished 4000 requests


Server Software:        nginx/1.12.2
Server Hostname:        www.baidu.com
Server Port:            443
SSL/TLS Protocol:       TLSv1.2,ECDHE-RSA-AES256-GCM-SHA384,2048,256

# 测试的路径
Document Path:          /index
# 页面大小
Document Length:        185550 bytes

# 测试的并发数
Concurrency Level:      100
# 整个测试持续的时间
Time taken for tests:   193.984 seconds
# 完成的请求数量
Complete requests:      4000
# 失败的请求数量
Failed requests:        0
Write errors:           0
# 整个过程中的网络传输量
Total transferred:      743108000 bytes
# 整个过程中的HTML内容传输量
HTML transferred:       742200000 bytes
# 最重要的指标之一，平均每秒请求数，后面括号中的mean表示这是一个平均值
Requests per second:    20.62 [#/sec] (mean)
# 最重要的指标之二，平均每次并发的完成时间，后面括号中的mean表示这是一个平均值
Time per request:       4849.611 [ms] (mean)
# 每个连接请求实际运行时间的平均值
Time per request:       48.496 [ms] (mean, across all concurrent requests)
# 平均每秒网络上的流量，可以帮助排除是否存在网络流量过大导致响应时间延长的问题
Transfer rate:          3740.98 [Kbytes/sec] received

Connection Times (ms)
              (最小)min  mean[+/-sd] median   max
连接(Connect):        3  666 377.4    635    1919
处理(Processing):   637 4158 525.9   4125    6036
等待(Waiting):      120 4007 476.8   3970    6011
总计(Total):        640 4824 587.0   4810    6739

# 在特定时间内提供的请求的百分比
Percentage of the requests served within a certain time (ms)
  50%   4810
  66%   5053
  75%   5199
  80%   5315
  90%   5552
  95%   5772
  98%   6013
  99%   6521
 100%   6739 (longest request)
```

> 第二组测试数据，替换对应域名(FPM 子进程上限 200)
```
[xxx@server ~]# ab -n4000 -c200 https://www.baidu.com/index
This is ApacheBench, Version 2.3 <$Revision: 1430300 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking www.baidu.com (be patient)
Completed 400 requests
Completed 800 requests
Completed 1200 requests
Completed 1600 requests
Completed 2000 requests
Completed 2400 requests
Completed 2800 requests
Completed 3200 requests
Completed 3600 requests
Completed 4000 requests
Finished 4000 requests


Server Software:        nginx/1.12.2
Server Hostname:        www.baidu.com
Server Port:            443
SSL/TLS Protocol:       TLSv1.2,ECDHE-RSA-AES256-GCM-SHA384,2048,256

Document Path:          /index
Document Length:        185551 bytes

Concurrency Level:      200
# 整个测试持续的时间
Time taken for tests:   192.855 seconds
Complete requests:      4000
Failed requests:        0
Write errors:           0
Total transferred:      743112000 bytes
HTML transferred:       742204000 bytes
# 最重要的指标之一，平均每秒请求数，后面括号中的mean表示这是一个平均值
Requests per second:    20.74 [#/sec] (mean)
# 最重要的指标之二，平均每次并发的完成时间，后面括号中的mean表示这是一个平均值
Time per request:       9642.728 [ms] (mean)
# 每个连接请求实际运行时间的平均值
Time per request:       48.214 [ms] (mean, across all concurrent requests)
# 平均每秒网络上的流量，可以帮助排除是否存在网络流量过大导致响应时间延长的问题
Transfer rate:          3762.91 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        3 4948 1390.7   5291    6770
Processing:   239 4538 1358.5   4266   11151
Waiting:      117 4356 1280.7   4081   10417
Total:        242 9487 959.0   9589   12088

Percentage of the requests served within a certain time (ms)
  50%   9589
  66%   9834
  75%  10002
  80%  10106
  90%  10424
  95%  10650
  98%  10931
  99%  11051
 100%  12088 (longest request)
```

> 第三组测试数据，替换对应域名(FPM 子进程上限 200)
```
[xxx@server ~]# ab -n4000 -c250 https://www.baidu.com/index
This is ApacheBench, Version 2.3 <$Revision: 1430300 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking www.baidu.com (be patient)
Completed 400 requests
Completed 800 requests
Completed 1200 requests
Completed 1600 requests
Completed 2000 requests
Completed 2400 requests
Completed 2800 requests
Completed 3200 requests
SSL read failed (5) - closing connection
Completed 3600 requests
Completed 4000 requests
Finished 4000 requests


Server Software:        nginx/1.12.2
Server Hostname:        www.baidu.com
Server Port:            443
SSL/TLS Protocol:       TLSv1.2,ECDHE-RSA-AES256-GCM-SHA384,2048,256

Document Path:          /index
Document Length:        185551 bytes

Concurrency Level:      250
Time taken for tests:   192.069 seconds
Complete requests:      4000
# 失败的请求数
Failed requests:        8
   (Connect: 0, Receive: 0, Length: 7, Exceptions: 1)
Write errors:           0
Non-2xx responses:      6
Total transferred:      741813504 bytes
HTML transferred:       740906181 bytes
Requests per second:    20.83 [#/sec] (mean)
Time per request:       12004.296 [ms] (mean)
Time per request:       48.017 [ms] (mean, across all concurrent requests)
Transfer rate:          3771.71 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0 7064 5236.1   6521   71138
Processing:     5 4644 2579.0   4229  127259
Waiting:        5 4406 1527.8   4030   12526
Total:        273 11708 5355.5  10865  127259

Percentage of the requests served within a certain time (ms)
  50%  10865
  66%  11272
  75%  11571
  80%  11855
  90%  13738
  95%  15152
  98%  24763
  99%  26857
 100%  127259 (longest request)
```

> 第四组测试数据，替换对应域名(FPM 子进程上限 200)
```
[xxx@server ~]# ab -n4000 -c300 https://www.baidu.com/index
This is ApacheBench, Version 2.3 <$Revision: 1430300 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking www.baidu.com (be patient)
Completed 400 requests
Completed 800 requests
Completed 1200 requests
Completed 1600 requests
Completed 2000 requests
Completed 2400 requests
Completed 2800 requests
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL handshake failed (5).
SSL handshake failed (5).
SSL read failed (5) - closing connection
Completed 3200 requests
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
Completed 3600 requests
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
Completed 4000 requests
Finished 4000 requests


Server Software:        nginx/1.12.2
Server Hostname:        www.baidu.com
Server Port:            443
SSL/TLS Protocol:       TLSv1.2,ECDHE-RSA-AES256-GCM-SHA384,2048,256

Document Path:          /index
Document Length:        185551 bytes

Concurrency Level:      300
Time taken for tests:   189.734 seconds
Complete requests:      4000
Failed requests:        80
   (Connect: 0, Receive: 0, Length: 60, Exceptions: 20)
Write errors:           0
Non-2xx responses:      38
Total transferred:      732092358 bytes
HTML transferred:       731191748 bytes
Requests per second:    21.08 [#/sec] (mean)
Time per request:       14230.075 [ms] (mean)
Time per request:       47.434 [ms] (mean, across all concurrent requests)
Transfer rate:          3768.08 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0 7608 6807.5   6524   91491
Processing:     8 5631 9272.4   4270  127295
Waiting:        8 4676 2090.6   4058   14478
Total:        289 13239 10662.1  11007  127295

Percentage of the requests served within a certain time (ms)
  50%  11007
  66%  11793
  75%  12946
  80%  13439
  90%  16238
  95%  20711
  98%  41751
  99%  63608
 100%  127295 (longest request)
```

> 第五组测试数据，替换对应域名(FPM 子进程上限 400)
```
[xxx@server ~]# ab -n4000 -c300 https://www.baidu.com/index
This is ApacheBench, Version 2.3 <$Revision: 1430300 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking www.baidu.com (be patient)
Completed 400 requests
Completed 800 requests
Completed 1200 requests
Completed 1600 requests
Completed 2000 requests
Completed 2400 requests
Completed 2800 requests
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
Completed 3200 requests
Completed 3600 requests
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
SSL read failed (5) - closing connection
Completed 4000 requests
Finished 4000 requests


Server Software:        nginx/1.12.2
Server Hostname:        www.baidu.com
Server Port:            443
SSL/TLS Protocol:       TLSv1.2,ECDHE-RSA-AES256-GCM-SHA384,2048,256

Document Path:          /index
Document Length:        185551 bytes

Concurrency Level:      300
Time taken for tests:   194.589 seconds
Complete requests:      4000
Failed requests:        101
   (Connect: 0, Receive: 0, Length: 62, Exceptions: 39)
Write errors:           0
Non-2xx responses:      23
Total transferred:      732409609 bytes
HTML transferred:       731511052 bytes
Requests per second:    20.56 [#/sec] (mean)
Time per request:       14594.181 [ms] (mean)
Time per request:       48.647 [ms] (mean, across all concurrent requests)
Transfer rate:          3675.66 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0 7223 6102.8   6513   70051
Processing:     8 6890 12389.0   4523  127282
Waiting:        8 5481 3153.5   4296   16258
Total:        246 14113 12726.8  11363  127282

Percentage of the requests served within a certain time (ms)
  50%  11363
  66%  12460
  75%  14029
  80%  15071
  90%  17581
  95%  25328
  98%  39892
  99%  73990
 100%  127282 (longest request)
 ```

> 结果分析
  服务器每秒能处理的请求上限为 20 个，平均单个请求的响应速度在 48ms，带宽流量 3.6MB/秒（服务器上限25MB），内存总占用在60%-90%区间，CPU一直处于90%以上（双核 只要超过2个并发CPU负载都会高于80%），每秒并发超过 200 以上的情况下，开始出现部分请求异常无法处理。

## 方案
  > 加大CUP个数，设置 PHP-FPM 静态方式开启进程数量 200 个最为合适。或购置更多服务器。
