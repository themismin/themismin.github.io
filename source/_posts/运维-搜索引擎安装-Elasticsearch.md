---
title: 运维-搜索引擎安装 Elasticsearch
author: 小敏-mingo
tags:
  - 搜索引擎
  - Elasticsearch
categories:
  - IT
  - 运维
abbrlink: 9db60fc0
date: 2019-03-06 14:38:01
---

## 安装 

### 将 Elasticsearch 公共 GPG 密钥导入 rpm
```
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```

### 新增 /etc/yum.repos.d/elasticsearch.repo
```
[elasticsearch-7.x]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

### 安装 Elasticsearch
```
yum install elasticsearch
systemctl start elasticsearch
systemctl enable elasticsearch
```

### 安装 Logstash
```
yum install logstash
systemctl start logstash
systemctl enable logstash
```

### 安装 Kibana
```
yum install kibana
systemctl start kibana
systemctl enable kibana
```

### 安装 Filebeat
```
yum install filebeat
systemctl start filebeat
systemctl enable filebeat
```

### 查看端口是否正常
```
netstat -lntp |grep 9200
netstat -lntp |grep 5601
```

## 配置
- 设置kibana
> /etc/kibana/kibana.yml
```
logging.dest: /var/log/kibana/kibana.log
```

## 调试
### filebeat 配置
```
...
- type: log
  enabled: true
  paths:
    - /var/log/nginx/access.log
  # 添加字段信息
  fields:
    logtype: nginx_access
    logsource: nginx_access_log

- type: log
  enabled: true
  paths:
    - /var/log/nginx/error.log
  # 添加字段信息
  fields:
    logtype: nginx_error
    logsource: nginx_error_log
...
output.logstash:
  hosts: ["localhost:10520"]
...
```
```
### 测试配置
filebeat -e -c /etc/filebeat/filebeat.yml
```

### logstash 配置 Rsyslog 日志
- logstash 配置 rsyslog 日志
```
### 编辑 rsyslog 配置，/etc/rsyslog.conf
*.* @@127.0.0.1:10514

### 编辑 logstash 配置，/etc/logstash/conf.d/system-syslog.conf
input {
  syslog {
    port => 10514
  }
}
output {
  stdout {
    codec => rubydebug  # 将日志输出到当前的终端上显示
  }
}

### 测试配置
/usr/share/logstash/bin/logstash --path.settings /etc/logstash/ -f /etc/logstash/conf.d/system-syslog.conf
```

- logstash 配置 filebeat 日志
```
input {
  beats {
    port => 10520
  }
}
output {
  if [fields][logtype] == "nginx_access" {
    elasticsearch {
      hosts => ["127.0.0.1:9200"]
      index => "nginx_access-%{+YYYY.MM.dd}"
    }
  }
  if [fields][logtype] == "nginx_error" {
    elasticsearch {
      hosts => ["127.0.0.1:9200"]
      index => "nginx_error-%{+YYYY.MM.dd}"
    }
  }

  #stdout {
  #  codec => rubydebug  # 将日志输出到当前的终端上显示
  #}
}
```

### nginx 配置 Kibana 代理
```
## 新增密码文件

htpasswd -c /etc/nginx/passwd/.htpasswd mingo

## 追加密码用户
htpasswd /etc/nginx/passwd/.htpasswd mingo002

## 添加nginx配置
    auth_basic "Administrator Login";
    auth_basic_user_file /etc/nginx/passwd/.htpasswd;

## 重启
```



