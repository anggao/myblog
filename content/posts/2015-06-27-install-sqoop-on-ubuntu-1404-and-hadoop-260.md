---
title: "Install Sqoop on Ubuntu 14.04 and Hadoop 2.6.0"
date: "2015-06-27"
template: "post"
draft: false
slug: "install-sqoop-on-ubuntu-1404-and-hadoop-260"
category: "hadoop"
tags:
  - "hadoop"
description: ""
socialImage: ""
---

**Followings steps are for: Ubuntu 14.04 LTS, Hadoop-2.6.0**

For [Hadoop 2.6.0 Installation on Ubuntu 14.04](/posts/hadoop-setting-up-a-single-node-cluster/)

- Scoop Installation

```shell
wget http://apache.proserve.nl/sqoop/1.4.6/sqoop-1.4.6.bin__hadoop-2.0.4-alpha.tar.gz
tar -xzvf sqoop-1.4.6.bin__hadoop-2.0.4-alpha.tar.gz
mv sqoop-1.4.6.bin__hadoop-2.0.4-alpha sqoop-1.4.6

sudo vim /etc/profile

    SQOOP_HOME=/home/anggao/sqoop-1.4.6
    export SQOOP_HOME
    export PATH=$PATH:$SQOOP_HOME/bin

source /etc/profile

# obtain JDBC driver
cd $SQOOP_HOME/lib
wget https://jdbc.postgresql.org/download/postgresql-9.4-1201.jdbc4.jar

sqoop version
```

- Scoop examples

```shell
# transfer data from PostgreSQL to HDFS
sqoop import --connect jdbc:postgresql://localhost:5432/dev-db --table tableName --username userName --password randomPassword
hadoop fs -cat department/part-m-00000

# transfer data from PostgreSQL to Hive
sqoop import --connect jdbc:postgresql://localhost:5432/dev-db --username userName --password randomPassword --table tableName --hive-import -m 1

# transfer data from Hive to PostgreSQL
sqoop export --connect jdbc:postgresql://localhost:5432/dev-db --username userName --password randomPassword --table postgreTableName --export-dir /user/hive/warehouse/hiveTableName --input-fields-terminated-by '\0x001'
```
