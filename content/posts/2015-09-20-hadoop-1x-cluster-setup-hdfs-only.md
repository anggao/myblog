---
title: "Hadoop 1.x Cluster Setup (HDFS only)"
date: "2015-09-20"
template: "post"
draft: false
slug: "hadoop-1x-cluster-setup-hdfs-only"
category: "hadoop"
tags:
  - "hadoop"
description: ""
socialImage: ""
---

### Purpose

This document describes how to set up and configure a 3-nodes Hadoop 1.x Distributed File System (HDFS) cluster.

### Prerequisites

- 3 CentOS 6.x VMs
- In the following examples I have three nodes:
- node1: 192.168.0.11
- node2: 192.168.0.12
- node3: 192.168.0.13
- The finally setup will be:
- NameNode: node1
- Secondary NN: node2
- DataNode: node2 node3

### Steps

**Followings steps are for: CentOS 6.7, Hadoop-1.2.1**

#### Installing Software (3 nodes)

- Install Java 7

```shell
tar -xzvf jdk-7u79-linux-x64.gz
cp -r jdk1.7.0_79 /opt/
alternatives --install /usr/bin/java java /opt/jdk1.7.0_79/bin/java 1
alternatives --config java
ln -sf /opt/jdk1.7.0_79 /usr/java7
```

- Configure JAVA_HOME and PATH add following lines to ~/.bashrc

```shell
export JAVA_HOME=/usr/java7
export PATH=$JAVA_HOME/bin:$PATH
```

- Prepare Hadoop 1.2.1 package

```shell
http://mirrors.whoishostingthis.com/apache/hadoop/common/hadoop-1.2.1/hadoop-1.2.1.tar.gz
tar xzvf hadoop-1.2.1.tar.gz
ln -sf /root/hadoop-1.2.1 /home/hadoop-1.2
```

#### Network Setting

The following shows exampel for node1 (192.168.0.11), node2 and node3 are similar

```shell
## Configure eth0
#
# vi /etc/sysconfig/network-scripts/ifcfg-eth0

DEVICE="eth0"
NM_CONTROLLED="yes"
ONBOOT=yes
HWADDR=A4:BA:DB:37:F1:04
TYPE=Ethernet
BOOTPROTO=static
NAME="System eth0"
UUID=5fb06bd0-0bb0-7ffb-45f1-d6edd65f3e03
IPADDR=192.168.0.11
NETMASK=255.255.255.0


## Configure Default Gateway
#
# vi /etc/sysconfig/network

NETWORKING=yes
HOSTNAME=node1
GATEWAY=192.168.0.1

## Configure hosts file
# vi /etc/hosts

192.168.0.11 node1
192.168.0.12 node2
192.168.0.13 node3

## Restart Network Interface
/etc/init.d/network restart
```

#### Setup passphraseless ssh

- Check that you can ssh to the localhost without a passphrase: `ssh localhost` If you cannot ssh to localhost without a passphrase, execute the following commands:

```shell
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

- Do the above steps for three nodes.
- To allow node1 (NameNode) able to start all DataNodes (passphraseless ssh to node2 and node3)

```shell
# From node1
scp ~/.ssh/id_rsa.pub root@node2:/root
scp ~/.ssh/id_rsa.pub root@node3:/root

# From node2 and node3
cat ~/root/id_rsa.pub >> ~/.ssh/authorized_keys
```

#### Hadoop HDFS Config

At this time basic setup is ready, we need to config HDFS

- config core-site.xml

```xml
<configuration>
  <property>
    <name>fs.default.name</name>
    <value>hdfs://node1:9000</value>
  </property>
  <property>
    <name>hadoop.tmp.dir</name>
    <value>/opt/hadoop-1.2</value>
  </property>
</configuration>
```

- config hdfs-site.xml

```xml
<configuration>
  <property>
      <name>dfs.replication</name>
      <value>2</value>
  </property>
</configuration>
```

- define DataNodes and Secondary NN

```shell
# vim /home/hadoop-1.2/slaves (define DataNodes)
node2
node3
# vim /home/hadoop-1.2/masters (define Secondary NN)
node2
```

- JAVA_HOME for hadoop

```shell
# vim /home/hadoop-1.2/conf/hadoop-env.sh
export JAVA_HOME=/usr/java7
```

- copy configs to slaves

```shell
# From node1
scp /home/hadoop-1.2/conf/* root@node2:/home/hadoop-1.2/conf
scp /home/hadoop-1.2/conf/* root@node3:/home/hadoop-1.2/conf
```

- format HDFS

```shell
# Format namenode
/home/hadoop-1.2/bin/hadoop namenode -format
```

**Need to disable the iptables before HDFS cluster**: service iptables stop

- start HDFS: `/home/hadoop-1.2/bin/start-dfs.sh`

- check: `jps` or `http://node1:50070`
