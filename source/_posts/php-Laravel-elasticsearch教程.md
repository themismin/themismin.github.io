---
title: php-Laravel elasticsearch教程
date: '2022-02-08 13:51:52 - PHP - elasticsearch - 搜索 - laravel'
categories:
  - IT
  - PHP
tag:
  - 搜索引擎
  - Elasticsearch
abbrlink: a3a2602b
---
## centos 安装 elasticsearch
```
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch

sudo vim /etc/yum.repos.d/elasticsearch-7.x.repo
###
[elasticsearch-7.x]
name=Elasticsearch repository for  7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
###

yum install -y elasticsearch

sudo systemctl restart elasticsearch
sudo systemctl enable elasticsearch
```

## ubuntu 安装
```
0. 查看版本
sudo apt-cache madison elasticsearch
0. 显示包的版本
sudo dpkg -s elasticsearch

1. 卸载旧软件
sudo apt-get purge elasticsearch
sudo apt-get autoremove elasticsearch
sudo rm -rf /var/lib/elasticsearch/
sudo rm -rf /etc/elasticsearch/

#2. 安装 7.17.*
#sudo apt-get install elasticsearch

2. 安装指定版本 7.17.18
sudo apt install elasticsearch=7.17.18

3. 加载，开启启动，启动
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch.service
sudo systemctl start elasticsearch.service
```

## ik插件
```
1. 对应置顶版本插件
sudo /usr/share/elasticsearch/bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.17.0/elasticsearch-analysis-ik-7.17.0.zip

sudo /usr/share/elasticsearch/bin/elasticsearch-plugin install https://github.com/infinilabs/analysis-ik/releases/download/v7.17.18/elasticsearch-analysis-ik-7.17.18.zip

sudo /usr/share/elasticsearch/bin/elasticsearch-plugin list

sudo systemctl restart elasticsearch

# 分词
curl http://127.0.0.1:9200/_analyze --header 'Content-Type: application/json' --data '{"analyzer":"ik_max_word","text":"世界如此之大"}'

ik 带有两个分词器
ik_max_word ：会将文本做最细粒度的拆分；尽可能多的拆分出词语
ik_smart：会做最粗粒度的拆分；已被分出的词语将不会再次被其它词语占有


# 热词更新配置
sudo vim /etc/elasticsearch/analysis-ik/IKAnalyzer.cfg.xml
```

## 配置
```
sudo vim /etc/elasticsearch/elasticsearch.yml
###
...
bootstrap.memory_lock: false
bootstrap.system_call_filter: false
...
network.host: 0.0.0.0
...
cluster.initial_master_nodes: ["node-1"]
###

sudo vim /etc/sysctl.conf
###
...
vm.max_map_count=655360
###

sysctl -p

sudo vim /etc/security/limits.conf
###
...
* soft nofile 65535
* hard nofile 65535
###
# 重启服务器


sudo systemctl restart elasticsearch
sudo systemctl enable elasticsearch

# 服务是否启动
curl http://127.0.0.1:9200?pretty

# 分词
curl http://127.0.0.1:9200/_analyze --header 'Content-Type: application/json' --data '{"text":"世界如此之大"}'
```

## 命令
```
# 查看所有列表
curl -X GET 'http://localhost:9200/_cat/indices?v'


```

## 相关链接
```
# IK分词
https://github.com/medcl/elasticsearch-analysis-ik

# laravel-scout-elasticsearch 扩展
https://github.com/matchish/laravel-scout-elasticsearch

# IK分词配置和使用
https://blog.csdn.net/jam00/article/details/52983056


https://www.ar414.com/tags/Elasticsearch

# ElasticSearch实战系列
https://www.cnblogs.com/xuwujing/p/11385255.html
https://www.cnblogs.com/xuwujing/p/11567053.html
https://www.cnblogs.com/xuwujing/p/11645630.html
```