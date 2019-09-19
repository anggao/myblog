---
title: "Hadoop: Setting up a Single Node Cluster"
date: "2015-06-21"
template: "post"
slug: "/posts/hadoop-setting-up-a-single-node-cluster/"
category: "hadoop"
tags:
  - "hadoop"
description: ""
socialImage: ""
---

### Purpose

This document describes how to set up and configure a single-node Hadoop installation so that you can quickly perform simple operations using Hadoop MapReduce and the Hadoop Distributed File System (HDFS).

### Prerequisites

+ GNU/Linux
+ `Java` must be installed. Recommended Java versions are described at [HadoopJavaVersions](http://wiki.apache.org/hadoop/HadoopJavaVersions)
+ `ssh` must be installed and sshd must be running to use the Hadoop scripts that manage remote Hadoop daemons.
A Hadoop distribution, download a recent stable release from one of the [Apache Download Mirrors](http://www.apache.org/dyn/closer.cgi/hadoop/common/)

### Installing Software

**Followings steps are for: Ubuntu 14.04 LTS, Hadoop-2.6.0**

TIP: Check 32-bit or 64-bit OS: `getconf LONG_BIT`

+ JDK - download [oracle 1.7.0_45](http://download.oracle.com/otn/java/jdk/7u45-b18/jdk-7u45-linux-x64.tar.gz)

```shell
sudo mkdir /usr/local/java
sudo cp jdk-7u45-linux-x64.gz /usr/local/java/
cd /usr/local/java/ && sudo tar -xzvf jdk-7u45-linux-x64.gz
```

```shell
sudo vim /etc/profile
# /etc/profile  
JAVA_HOME=/usr/local/java/jdk1.7.0_45
JRE_HOME=$JAVA_HOME/jre
PATH=$slug:$JAVA_HOME/bin:$JRE_HOME/bin
export JAVA_HOME
export JRE_HOME
export PATH
```

```shell
# Inform your Ubuntu Linux system where your Oracle Java JDK/JRE is located. 
sudo update-alternatives --install "/usr/bin/java" "java" "/usr/local/java/jdk1.7.0_45/jre/bin/java" 1
sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/local/java/jdk1.7.0_45/bin/javac" 1
sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/local/java/jre1.7.0_45/bin/javaws" 1

# Inform your Ubuntu Linux system that Oracle Java JDK/JRE must be the default Java.
sudo update-alternatives --set java /usr/local/java/jdk1.7.0_45/jre/bin/java
sudo update-alternatives --set javac /usr/local/java/jdk1.7.0_45/bin/javac
sudo update-alternatives --set javaws /usr/local/java/jdk1.7.0_45/bin/javaws

# Reload your system wide PATH /etc/profile
. /etc/profile
```

+ Hadoop-2.6.0
+ Unpack the downloaded Hadoop distribution.

```shell
# vim etc/hadoop/hadoop-env.sh

# The java implementation to use.
export JAVA_HOME=/usr/local/java/jdk1.7.0_45
```

### Configuration
Now you are ready to start your Hadoop cluster in one of the three supported modes:

- Local (Standalone) Mode
- Pseudo-Distributed Mode
- Fully-Distributed Mode

#### OS

```shell
# sudo vim /etc/hosts
# add new host
127.0.1.1 YARN001
```

**Hadoop (Pseudo-Distributed Mode with YARN)**

```xml
# vim etc/hadoop/mapred-site.xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>

# vim etc/hadoop/core-site.xml
<configuration>
    <property>
        <name>fs.default.name</name>
        <value>hdfs://YARN001:8020</value>
    </property>
</configuration>

# vim etc/hadoop/hdfs-site.xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/home/anggao/hadoop/dfs/name</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>/home/anggao/hadoop/dfs/name</value>
    </property>
</configuration>

# vim etc/hadoop/yarn-site.xml
<configuration>
<!-- Site specific YARN configuration properties -->
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>

# vim etc/hadoop/slaves
localhost
```

```shell
# You can start all processes with start-all.sh, but better starts the process one by one 
sbin/start-all.sh

# Start dfs or YARN with one command
sbin/start-dfs.sh
sbin/start-yarn.sh

# Step 1: format dfs
bin/hadoop namenode -format

# Step 2: start name node
sbin/hadoop-daemon.sh start namenode

# Step 3: Check running processes
jps

# Step 4: start data node
sbin/hadoop-daemon.sh start datanode

# Step 5: check dfs with UI
http://YARN001:50070/

# Step 6: format dfs, add a file
bin/hadoop fs -mkdir /home
bin/hadoop fs -mkdir /home/anggao
bin/hadoop fs -put README.txt /home/anggao

# Step 7: start YARN
sbin/start-yarn.sh
http://YARN001:8088

# Step 8: check mapreduce
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar pi 2 100

# Step 9: stop dfs and YARN
sbin/stop-yarn.sh
sbin/stop-dfs.sh
```