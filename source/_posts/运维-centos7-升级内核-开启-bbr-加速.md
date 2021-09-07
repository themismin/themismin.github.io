---
title: 运维-centos7 升级内核-开启 bbr 加速
author: 小敏-mingo
tags:
  - bbr
categories:
  - IT
  - 运维
abbrlink: d9de7c8f
date: 2019-06-04 14:06:09
---
BBR是来自于Google的黑科技，目的是通过优化和控制TCP的拥塞，充分利用带宽并降低延迟，起到神奇般的加速效果。
<!-- more -->
```
# 内核站点
http://elrepo.org/

# 添加源
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org (external link)
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm (external link)

# 列出可用系统内核
yum --disablerepo="*" --enablerepo="elrepo-kernel" list available
# 安装最新稳定内核
yum --enablerepo=elrepo-kernel install kernel-ml

# 更新内核配置
grub2-mkconfig -o /boot/grub2/grub.cfg

# 列出内核启动顺序
awk -F\' /^menuentry/{print\$2} /etc/grub2.cfg

# 查看当前启动内核
cat /boot/grub2/grubenv

# 设置启动内核为第一项
grub2-set-default 0
## 或
grub2-set-default "CentOS Linux (4.19.1-1.el7.elrepo.x86_64) 7 (Core)"

grub2-mkconfig -o /boot/grub2/grub.cfg

# 重启系统
reboot


uname -rs
```



```
## 开启bbr
开机后 `uname -r` 看看是不是内核 >= 4.9  

执行 `lsmod | grep bbr`，如果结果中没有 `tcp_bbr` 的话就先执行

modprobe tcp_bbr
echo "tcp_bbr" >> /etc/modules-load.d/modules.conf


执行

echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf


保存生效  
sysctl -p

执行  

sysctl net.ipv4.tcp_available_congestion_control
sysctl net.ipv4.tcp_congestion_control

如果结果都有 `bbr`, 则证明你的内核已开启 bbr  

执行 `lsmod | grep bbr`, 看到有 tcp_bbr 模块即说明 bbr 已启动  
```