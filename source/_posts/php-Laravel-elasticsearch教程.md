---
title: php-Laravel elasticsearch教程
date: '2022-02-08 13:51:52 - PHP - elasticsearch - 搜索 - laravel'
categories:
  - IT
  - PHP
abbrlink: a3a2602b
---
## 安装 elasticsearch
```
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch

vim /etc/yum.repos.d/elasticsearch-7.x.repo
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

vim /etc/elasticsearch/elasticsearch.yml
###
...
bootstrap.memory_lock: false
bootstrap.system_call_filter: false
...
network.host: 0.0.0.0
...
cluster.initial_master_nodes: ["node-1"]
###

vim /etc/sysctl.conf
###
...
vm.max_map_count=655360
###

sysctl -p

systemctl start elasticsearch
systemctl enable elasticsearch

# 服务是否启动
curl http://127.0.0.1:9200?pretty

# 分词
curl http://127.0.0.1:9200/_analyze --header 'Content-Type: application/json' --data '{"text":"世界如此之大"}'
```

## ik插件
```
/usr/share/elasticsearch/bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.17.0/elasticsearch-analysis-ik-7.17.0.zip

/usr/share/elasticsearch/bin/elasticsearch-plugin list

# 分词
curl http://127.0.0.1:9200/_analyze --header 'Content-Type: application/json' --data '{"analyzer":"ik_max_word","text":"世界如此之大"}'

ik 带有两个分词器
ik_max_word ：会将文本做最细粒度的拆分；尽可能多的拆分出词语
ik_smart：会做最粗粒度的拆分；已被分出的词语将不会再次被其它词语占有


# 热词更新配置
vim /etc/elasticsearch/analysis-ik/IKAnalyzer.cfg.xml
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