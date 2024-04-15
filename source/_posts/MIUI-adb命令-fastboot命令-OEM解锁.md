---
title: MIUI-adb命令 -- fastboot命令&OEM解锁
tags:
  - adb命令
  - fastboot命令
  - OEM解锁
abbrlink: 5ede5441
date: 2024-04-06 15:20:24
---
一、fastboot刷机
1.fastboot 概念
fastboot是PC与BootLoader的USB通信的命令行工具，通过向BootLoader传送刷机文件（.img）实现Android系统分区重烧。fastboot需要BootLoader支持，且需要使用USB数据线连接，因此常称为线刷模式。

2.BootLoader
BootLoader是嵌入式设备中用来引导内核启动的一段代码。内核启动是需要一定条件的，当设备上电后会首先运行BootLoader，BootLoader会初始化必要的硬件，比如DDR、Flash、串口等，通过这段程序可以将系统硬件环境设置到一个合适的状态，为进行系统内核调试准备好环境，相关初始化完成后就会去启动内核。
（在嵌入式系统中，通常没有BOIS那样的固态程序，因此整个系统的加载启动任务就有BootLoader来完成，BootLoader程序通常安排在嵌入式系统最开始运行的地址处0x0000 0000）

3.uboot & fastboot
uboot（universal bootloader）是一种可以用于多种嵌入式CPU的BootLoader程序。

在uboot下输入fastboot命令，就可以让uboot进入fastboot模式，刷机就是在fastboot模式下进行刷机。
开发板本身不是usb设备，所以当我们的开发板直接通过usb线和主机的usb接口连接时，主机是识别不到一个usb设备的。当我们在uboot下输入fastboot命令时，主机就会识别到一个usb设备，并提示安装驱动。

4.刷机模式
4.1 卡刷
Recovery模式（卡刷）:必须拷贝系统ROM
在系统进行定制时，编译系统会编译出一份ZIP的压缩包，里面是一些系统分区镜像，提供给客户进行手动升级、恢复系统。需要提前将压缩包内置SDcard，在Recovery模式进行。进入Recovery方法：将手机完全关机后，按住音量键下(上)+电源键，进入BootLoader界面。用音量加减来控制光标，电源键来进行确认(有的机器只能用音量下键进行选择，上键是确认键)。

4.2 线刷

fastboot模式（线刷）:通过刷入.img镜像文件，进行分区重烧，无需启动内核
在安卓手机中fastboot是一种比Recovery更底层的刷机模式。fastboot在开发板和主机之间定义了一套协议，是一种使用USB数据线连接手机的刷机模式，这就是所谓的线刷。

5.刷机常用分区
分区	作用
splash1	开机画面，使用Nandroid backup备份系统后的文件为splash1.img
recovery	该分区是恢复模式（即开机按Home+poweri进入的界面，使用Nandroid backup备份为recovery.img
boot	内核启动分区，使用Nandroid backup备份为boot.img
system	Android系统部分，目录表示为/system,通常为只读，使用Nandroid backup备份为system.img
cache	缓存文件夹，目录表示为/cache,事实上除了T-mobile的OTA更新外，作用不大。使用Nandroid backup备份为cache.img
userdata	用户安装的软件及各种数据，目录为/data,使用Nandroid backup备份为data.img
二、fastboot命令
重启相关
fastboot reboot                 重启⼿机
fastboot reboot-bootloader      重启到bootloader模式,其实就是再次进入fastboot
fastboot -w reboot              清除手机中所有数据然后重启
// fastboot -w reboot 等同于系统中的“恢复出厂设置”，或Recovery模式的“清空所有数据”操作

擦除相关（erase）
fastboot erase {partition}                      擦除分区
fastboot erase boot                             擦除boot分区
fastboot erase recovery                         擦除recovery分区
fastboot erase system                           擦除system分区
fastboot erase userdata                         擦除userdata分区
fastboot erase cache                            擦除cache分区
写⼊分区（flash）
fastboot flash {partition} {*.img}              烧录img文件至对应分区
fastboot flash boot boot.img                    写⼊boot分区
fastboot flash recovery recovery.img            写⼊recovery分
fastboot flash system system.img                写⼊system分区
查看相关
fastboot getvar all                             获取⼿机的全部信息
fastboot devices                                查看fastboot模式下连接的手机
其它
fastboot boot <内核镜像文件名或路径>              临时启动镜像，不会烧录和替换内核文件到存储中
fastboot oem device-info                         输出当前BL锁状态(非MTK)
fastboot oem lks                                 输出当前BL锁状态(MTK)
fastboot oem poweroff                           拔掉数据线后关机
fastboot oem lock                               重新上BL锁并清空所有数据(需未开启root)
fastboot oem unlock                             解除BL锁并清空所有数据
//小米手机必须绑定账号,主动申请解锁,等待7天,使用工具才行
fastboot flashing unlock                        解锁设备
android fastboot常见命令

三、OEM解锁（MTK）
解锁命令&操作
将手机设为开发者模式，在开发者模式中选择OEM 解锁

连接adb,重启到bootloader模式

adb reboot bootloader
当手机画面中出现fastboot mode，解锁设备

fastboot flashing unlock
根据提示按下音量上键进行解锁，提示成功之后，重启设备：

fastboot reboot
重启后会提醒设备处于解锁状态，会重置设备数据等操作。

push文件
获取root权限

adb root
关闭分区检测功能

在Android 7之后，对分区会进行相应的验证，例如system分区，不能向之前的版本一样，使用adb root;adb remount对system分区进行挂载，需要先关闭分区检测功能.

adb disable-verity
执行adb disable-verity后需要重启设备

adb reboot
设备重启后再次获取root权限

adb root
挂载（使分区可读可写）

adb remount
push以后要重启设备