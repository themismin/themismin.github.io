---
title: BTC-xmrig门罗币矿工
tags:
  - BTC
  - 门罗币
  - momero
  - 挖矿
categories:
  - BTC
  - 门罗币
abbrlink: 5c75450b
date: 2021-01-15 10:35:03
---
门罗币项目在2014年4月正式发起。门罗项目非常公平，预先公布了CryptoNote参考代码。
<!-- more -->

### f2pool 矿池
> https://xmrig.com/docs/miner/build/macos

> mac cpu 挖矿
```
brew install cmake libuv openssl hwloc
git clone https://github.com/xmrig/xmrig.git
mkdir xmrig/build && cd xmrig/build
cmake .. -DOPENSSL_ROOT_DIR=/usr/local/opt/openssl
make -j$(sysctl -n hw.logicalcpu)
```

* xxxmac 替换为自己的名称
```
# 成功
./xmrig --donate-level=0 -o xmr.f2pool.com:13531 -u 4ASwScQ953AJw54xbPDRRsdkcmjjRY2cJE3CZVHVgN9LQDLe8kSYvGeXyFFHDcdcsgCMDwShCn9faBhiNLS5pjso132KRSn.xxxmac -p x -k --threads=4

./xmrig --donate-level=0 -o xmr.f2pool.com:13531 -u 4ASwScQ953AJw54xbPDRRsdkcmjjRY2cJE3CZVHVgN9LQDLe8kSYvGeXyFFHDcdcsgCMDwShCn9faBhiNLS5pjso132KRSn.xxxmac -p x -k --no-cpu --opencl --opencl-devices=1

```

* 查看地址
```
https://www.f2pool.com/xmr/4ASwScQ953AJw54xbPDRRsdkcmjjRY2cJE3CZVHVgN9LQDLe8kSYvGeXyFFHDcdcsgCMDwShCn9faBhiNLS5pjso132KRSn
```


> ubuntu xmrig-amd 安装失败
```
sudo apt-get install git build-essential cmake libuv1-dev libmicrohttpd-dev libssl-dev
sudo apt install ocl-icd-opencl-dev

git clone https://github.com/xmrig/xmrig-amd.git
mkdir xmrig-amd/build
cd xmrig-amd/build
cmake ..
make
```