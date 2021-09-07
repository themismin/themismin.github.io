---
title: Hacker-Android apk 破解
abbrlink: d8b8b869
date: 2021-03-05 13:53:55
tags:
---

### 下载JAVA 1.8.0_144
```
[jdk-8u144-macosx-x64](https://download.oracle.com/otn/java/jdk/8u144-b01/090f390dda5b47b9b721c7dfaa008135/jdk-8u144-macosx-x64.dmg?AuthParam=1615170051_b8d83190c955ab5bbde8f61e99f8d4fd)
```

### 下载 apktool.jar
```
[Apktool](https://github.com/iBotPeaches/Apktool/releases)

java
```

### apk反汇编工具apktool
> 有些apk的assets目录下有加密后的Dex文件，添加–only-main-classes参数即可
```
apk反汇编工具apktool问题之DexBackedDexFile$NotADexFile

apktool d com.homelink.android.apk --only-main-classes
```

### Mac Big Sur 无法打开 JD-GUI
```
去应用程序找到 JD-GUI 显示包内容

找到文件universalJavaApplicationStub ，使用文本编辑器打开

使用 https://raw.githubusercontent.com/tofi86/universalJavaApplicationStub/master/src/universalJavaApplicationStub 中的内容替换


```


### logcat获取apk的activity名
```
adb logcat ActivityManager:I '*:s'

# 链家启动命令
adb shell am start -D -n com.homelink.android/com.homelink.android.SplashScreenActivity

# 查看进程
adb shell "dumpsys activity top | grep --color=always ACTIVITY"

adb shell ps | grep com.homelink.android

# 监听端口
adb forward tcp:5005 jdwp:1994

adb forward --remove-all
```

https://www.usmacd.com/2020/03/19/apk_debug/
https://my.oschina.net/2devil/blog/1624384

```
Authorization 加密

APP_SECRET = "93273ef46a0b880faf4466c48f74878f"

MjAxNzAzMjRfYW5kcm9pZDo0ZjQwMzc3ZTIyMTI3OTZhMjZlYzU4YTQyYmFiZmRjMjg0OGU4ZGUw
base64 反编译
20170324_android:
4f40377e2212796a26ec58a42babfdc2848e8de0


0afff969ba425c11946ee6894fc4731d0324f9df


condition:
limit_offset:20
full_filters:0
is_suggestion:0
limit_count:20
city_id:310000

93273ef46a0b880faf4466c48f74878fcity_id=310000condition=full_filters=0is_suggestion=0limit_count=20limit_offset20

破解:
93273ef46a0b880faf4466c48f74878fcity_id=310000condition=full_filters=0is_suggestion=0limit_count=20limit_offset=20request_ts=1615531281


sign origin=93273ef46a0b880faf4466c48f74878fcity_id=310000condition=full_filters=0is_suggestion=0limit_count=20limit_offset=20request_ts=1615531281


93273ef46a0b880faf4466c48f74878fcity_id=310000condition=full_filters=0is_suggestion=0limit_count=20limit_offset=20request_ts=1615531281

> sha1('93273ef46a0b880faf4466c48f74878fcity_id=310000condition=full_filters=0is_suggestion=0limit_count=20limit_offset=20request_ts=1615531281');
c625e9b2cd59adb2ce9d3b812bd45ca670b3435d

20170324_android

20170324_android:c625e9b2cd59adb2ce9d3b812bd45ca670b3435d

MjAxNzAzMjRfYW5kcm9pZDpjNjI1ZTliMmNkNTlhZGIyY2U5ZDNiODEyYmQ0NWNhNjcwYjM0MzVk

> base64_encode('20170324_android:c625e9b2cd59adb2ce9d3b812bd45ca670b3435d');

MjAxNzAzMjRfYW5kcm9pZDpjNjI1ZTliMmNkNTlhZGIyY2U5ZDNiODEyYmQ0NWNhNjcwYjM0MzVk




```

```
> sha1('93273ef46a0b880faf4466c48f74878fcity_id=310000condition=full_filters=0is_suggestion=0limit_count=20limit_offset=40request_ts=1615531281');
c625e9b2cd59adb2ce9d3b812bd45ca670b3435d

> base64_encode('20170324_android:d0999bc2ac957219511ee900d56a789959216a54');



```


```
https://blog.csdn.net/flypple/article/details/100662269

https://blog.csdn.net/yuanxiang01/article/details/80494842

https://www.jianshu.com/p/aaee2d27068e

https://blog.csdn.net/Builder_Taoge/article/details/74772359

https://blog.csdn.net/wc369646/article/details/70238938

https://cloud.tencent.com/developer/article/1720360

https://www.jianshu.com/p/1a28e6439c6a

https://github.com/bjjdkp/hupu-users/issues/1

https://my.oschina.net/2devil/blog/1624384

https://blog.csdn.net/weixin_39537721/article/details/86493782

https://bjjdkp.github.io/post/anti-lianjia-app/

https://zhuanlan.zhihu.com/p/33786412

https://cloud.tencent.com/developer/article/1548599

https://book.crifan.com/books/android_app_security_crack/website/android_crack_tool/other_integrated_tool/crack_tool_mac.html

https://ibotpeaches.github.io/Apktool/

https://www.jianshu.com/p/873916e7710f

https://blog.csdn.net/m0_37696990/article/details/103931261

https://www.oracle.com/cn/java/technologies/javase/javase8-archive-downloads.html


/Users/xumin/Library/Android/sdk
```