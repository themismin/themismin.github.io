---
title: Hadoop-基础教程
tags:
  - hadoop
categories:
  - IT
  - 开发
abbrlink: 33f41d5a
date: 2020-03-23 16:44:30
---
```
centos7下载 http://cloud.centos.org/centos/7/vagrant/x86_64/images/CentOS-7-x86_64-Vagrant-2002_01.VirtualBox.box

jdk下载 https://www.oracle.com/java/technologies/javase-jdk8-downloads.html

hadoop下载 https://archive.cloudera.com/cdh5/cdh/5/hadoop-2.6.0-cdh5.16.2.tar.gz

hive下载 https://archive.cloudera.com/cdh5/cdh/5/hive-1.1.0-cdh5.16.2.tar.gz

```


```Mac
vagrant box add centos7 CentOS-7-x86_64-Vagrant-2002_01.VirtualBox.box
vagrant box list
vagrant init centos7
vargrant ssh
```

```Centos
tar zxvf ~/software/jdk-8u241-linux-x64.tar.gz -C ~/app/
tar zxvf ~/software/hadoop-2.6.0-cdh5.16.2.tar.gz -C ~/app/
tar zxvf ~/software/hive-1.1.0-cdh5.16.2.tar.gz -C ~/app/

### 配置ssh
ssh-keygen
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

### 配置 ~/.bash_profile
#### java环境变量
export JAVA_HOME=~/app/jdk1.8.0_241
export PATH=$JAVA_HOME/bin:$PATH

### 验证
java -version

### haddop环境变量
export HADOOP_HOME=~/app/hadoop-2.6.0-cdh5.16.2
export PATH=$HADOOP_HOME/bin:$PATH

### 配置 etc/hadoop/hadoop-env.sh
export JAVA_HOME=~/app/jdk1.8.0_241

### 配置 etc/hadoop/core-site.xml:
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://hadoop001:8020</value>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/home/vagrant/app/tmp</value>
    </property>
</configuration>

### 配置 etc/hadoop/hdfs-site.xml:
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>

### 第一个格式化tmp
hdfs namenode -format

### 启动服务
sbin/start-dfs.sh 

### 查看服务
jps

### 查看运行状态
http://localhost:50070/

### hadoop 常用命令
hadoopfs -ls /
hadoopfs -put
hadoopfs -copyFromLocal
hadoopfs -moveFromLocal
hadoopfs -cat
hadoopfs -text
hadoopfs -get
hadoopfs -mkdir
hadoopfs -mv
hadoopfs -getmerge
hadoopfs -rm
hadoopfs -rmdir
hadoopfs -rm -r
```

### 推送数据到hdfs
```
hadoop fs -mkdir -p /wordcount/input
hadoop fs -put data/test.txt /wordcount/input
```

### 代码
```Java

```
