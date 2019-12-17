---
title: "Hadoop: HDFS Shell Commands"
date: "2015-06-22"
template: "post"
draft: false
slug: "hadoop-hdfs-shell-commands"
category: "hadoop"
tags:
  - "hadoop"
description: ""
socialImage: ""
---

```shell
bin/hdfs

dfs                  run a filesystem command on the file systems supported in Hadoop.
dfsadmin             run a DFS admin client
fsck                 run a DFS filesystem checking utility
balancer             run a cluster balancing utility
```

```shell
bin/hdfs dfsadmin

[-safemode <enter | leave | get | wait>]
[-saveNamespace]
[-refreshNodes]
[-setQuota <quota> <dirname>...<dirname>]
[-setSpaceQuota <quota> <dirname>...<dirname>]
```

```shell
anggao@ubuntu:~/hadoop-2.6.0$ bin/hdfs fsck /home/anggao/README.txt -files -blocks -locations
Connecting to namenode via http://YARN001:50070
FSCK started by anggao (auth:SIMPLE) from /127.0.0.1 for path /home/anggao/README.txt at Mon Jun 22 06:14:12 PDT 2015
/home/anggao/README.txt 1366 bytes, 1 block(s):  OK
0. BP-1141233259-127.0.1.1-1434973068459:blk_1073741825_1001 len=1366 repl=1 [127.0.0.1:50010]

Status: HEALTHY
 Total size:    1366 B
 Total dirs:    0
 Total files:   1
 Total symlinks:                0
 Total blocks (validated):      1 (avg. block size 1366 B)
 Minimally replicated blocks:   1 (100.0 %)
 Over-replicated blocks:        0 (0.0 %)
 Under-replicated blocks:       0 (0.0 %)
 Mis-replicated blocks:         0 (0.0 %)
 Default replication factor:    1
 Average block replication:     1.0
 Corrupt blocks:                0
 Missing replicas:              0 (0.0 %)
 Number of data-nodes:          1
 Number of racks:               1
```

```shell
sbin/start-balancer.sh -threshold <precentage of disk capacity>
```

```xml
# Add datanode
Step 1: copy hadoop package and confige files to the new datanode.
Step 2: start new datanode:
sbin/hadoop-deamon.sh start datanode

# Remove datanode
Step 1: add datanode(ip/host) to the blacklist:
    in the NadeNode hdfs-site.xml, defines the location of the blacklist
    <property>
        <name>dfs.hosts.exclude</name>
        <value>/etc/hadoop/conf/dfs.exclude</value>
    </property>
Step 2: bin/hadoop dfsadmin -refreshNodes
```
