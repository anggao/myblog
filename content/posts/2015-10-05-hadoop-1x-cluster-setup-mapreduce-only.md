---
title: "Hadoop 1.x Cluster Setup (MapReduce only)"
date: "2015-10-05"
template: "post"
draft: false
slug: "hadoop-1x-cluster-setup-mapreduce-only"
category: "hadoop"
tags:
  - "hadoop"
description: ""
socialImage: ""
---

### Purpose

This document describes how to set up and configure a 3-nodes Hadoop 1.x MapReduce cluster.

For [HDFS setup Hadoop 1.x Cluster Setup (HDFS only)](/posts/hadoop-1x-cluster-setup-hdfs-only/)

### Hadoop MapReduce Config

- config mapred-site.xml

```xml
<configuration>
  <property>
    <name>mapred.job.tracker</name>
    <value>node1:9001</value>
  </property>
</configuration>
```

- copy configs to slaves

```xml
# From node1
scp /home/hadoop-1.2/conf/* root@node2:/home/hadoop-1.2/conf
scp /home/hadoop-1.2/conf/* root@node3:/home/hadoop-1.2/conf
```

- start MapReduce

```shell
/home/hadoop-1.2/bin/start-mapred.sh
```
