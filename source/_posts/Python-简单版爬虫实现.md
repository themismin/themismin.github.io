---
title: Python-简单版爬虫实现
tags:
  - python
  - 爬虫
categories:
  - IT
  - Python
abbrlink: b23ae985
date: 2021-02-02 13:57:52
---

## 安装requests
```
pip install requests
```

## 通过GET访问一个页面
```
import requests
r = requests.get('https://www.douban.com/') # 豆瓣首页

r.status_code 
# 200

r.text
# '<!DOCTYPE HTML>\n<html>\n<head>\n<meta name="description" content="提供图书、电影、音乐唱片的推荐、评论和...'
```

## 其它请求
```
r = requests.post("https://www.douban.com/")
r = requests.put("https://www.douban.com/")
r = requests.delete("https://www.douban.com/")
r = requests.head("https://www.douban.com/")
r = requests.options("https://www.douban.com/")
```

## 请求参数
```
# url：请求的URL地址，必选
url=https://www.douban.com/

# params：URL请求的参数（dist字典）
params={'key1':'value1','key2':'value2'}

# data：POST请求的参数（dist字典或file-like object文件类对象）
data={'key1':'value1','key2':'value2'}

# json：POST请求的JSON参数
json={'key1':'value1','key2':'value2'}

# files：POST请求的文件流数据（dist字典）
files={'file':open('report.xls','rb')}

# timeout：请求响应的最长等待时间。
timeout=5

# headers：https请求头信息
headers={'user-agent':'my-app/0.0.1'}

# cookies：COOKIE（dist字典）
cookies={'key':'value'}

# verify：默认TRUE，为FALSE时忽略对SSL证书的验证。它还可以接受SSL证书路径，为回话加载SSL证书
verify=False
verify='/path/to/certfile'

# cert：指定一个本地证书用作客户端证书
cert=('/path/to/client.cert','/path/to/client.key')
cert='/path/to/client.pem'

# auth：HTTP验证信息

```

### 代码
```
import requests
import time

# 抓取 链家商圈字典 623截止
n = 1
while n <= 100:
    url="http://www.douban.com/api/id/"+ str(n)
    
    headers = {"Content-Type":"application/json"}
    r = requests.get(url,headers=headers)
    json = r.json()
    if 200 == json["code"]:
        with open('./test.txt', 'a') as f:
            f.write('name:'+json["data"]["name"]+" ")
            if "avatar" in json["data"]:
                f.write(json["data"]["avatar"])
            f.write("\n")            
    time.sleep(1)
    n = n + 1
f.close()
```

### Done


