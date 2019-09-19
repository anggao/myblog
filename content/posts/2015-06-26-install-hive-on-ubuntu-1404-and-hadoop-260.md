---
title: "Install Hive on Ubuntu 14.04 and Hadoop 2.6.0"
date: "2015-06-26"
template: "post"
draft: false
slug: "/posts/install-hive-on-ubuntu-1404-and-hadoop-260/"
category: "hadoop"
tags:
  - "hadoop"
description: ""
socialImage: ""
---

**Followings steps are for: Ubuntu 14.04 LTS, Hadoop-2.6.0**

For [Hadoop 2.6.0 Installation on Ubuntu 14.04](/posts/hadoop-setting-up-a-single-node-cluster/)

+ Download and unpack Hive

```shell
wget http://mirror.tcpdiag.net/apache/hive/stable/apache-hive-1.2.0-bin.tar.gz
tar -xzvf apache-hive-1.2.0-bin.tar.gz
```

+ Configure environment variables

```shell
sudo vim /etc/profile

    HIVE_HOME=/home/anggao/hive-1.2.0
    export HIVE_HOME
    export PATH=$PATH:$HIVE_HOME/bin

source /etc/profile
```

+ Create folders in Hadoop file system

```shell
hadoop fs -mkdir /tmp
hadoop fs -mkdir /user/hive/warehouse
hadoop fs -chmod g+w /tmp
hadoop fs -chmod g+w /user/hive/warehouse
```

+ Hive CLI

```shell
# Notice there is an issue with Hive 1.2.0 and YARN, you need to copy a jar file to YARN lib directory
cp hive-1.2.0/lib/jline-2.12.jar $HADOOP_HOME/share/hadoop/yarn/lib/

hive
> show tables;
```

+ Hive Metastore Metadata is stored in an embedded Derby database whose disk storage location is determined by the Hive configuration variable named `javax.jdo.option.ConnectionURL`. By default, this location is `./metastore_db` (see `conf/hive-default.xml`).

Using Derby in embedded mode allows at most one user at a time. To configure Derby to run in server mode, see [Hive Using Derby in Server Mode](https://cwiki.apache.org/confluence/display/Hive/HiveDerbyServerMode). To configure a database other than Derby for the Hive metastore, see [Hive Metastore Administration](https://cwiki.apache.org/confluence/display/Hive/AdminManual+MetastoreAdmin).

More info: [https://cwiki.apache.org/confluence/display/Hive/AdminManual+Installation](https://cwiki.apache.org/confluence/display/Hive/AdminManual+Installation)