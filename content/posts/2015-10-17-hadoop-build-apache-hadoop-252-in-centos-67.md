---
title: "Hadoop: Build Apache Hadoop 2.5.2 in CentOS 6.7"
date: "2015-10-17"
template: "post"
draft: false
slug: "hadoop-build-apache-hadoop-252-in-centos-67"
category: "hadoop"
tags:
  - "hadoop"
description: ""
socialImage: ""
---

- Install build tools

```shell
yum install gcc
yum install gcc-c++
yum install make
yum install cmake
yum install openssl-devel
yum install ncurses-devel
```

- Install Maven

```shell
wget http://ftp.heanet.ie/mirrors/www.apache.org/dist/maven/maven-3/3.3.3/binaries/apache-maven-3.3.3-bin.tar.gz
tar -xzvf apache-maven-3.3.3-bin.tar.gz
export MAVEN_HOME=/opt/apache-maven-3.3.3
export PATH=$MAVEN_HOME/bin:$PATH
mvn -v
```

- Install protobuf

```shell
wget https://protobuf.googlecode.com/files/protobuf-2.5.0.tar.bz2
tar -xjvf protobuf-2.5.0.tar.bz2
cd protobuf-2.5.0
./configure --prefix=/opt/protoc
make
make install
export PROTO_HOME=/opt/protoc
export PATH=$PATH:$PROTO_HOME/bin
```

- Install findbugs

```shell
wget http://jaist.dl.sourceforge.net/project/findbugs/findbugs/2.0.3/findbugs-2.0.3.tar.gz
tar -xzvf findbugs-2.0.3.tar.gz
export FINDBUGS_HOME=/opt/findbugs-2.0.3
export PATH=$PATH:$FINDBUGS_HOME/bin
```

- Get Hadoop src

```shell
cd /opt
wget http://mirrors.whoishostingthis.com/apache/hadoop/common/hadoop-2.5.2/hadoop-2.5.2-src.tar.gz
tar -xzvf hadoop-2.5.2-src.tar.gz
cd hadoop-2.5.2-src
```

- Build hadoop

```shell
cd  /opt/hadoop/hadoop-2.5.2-src
mvn package -Pdist,native -DskipTests -Dtar
```

- Check build result

```shell
/home/hadoop/hadoop-2.5.2-src/hadoop-dist/target/hadoop-2.5.2
/home/hadoop/hadoop-2.5.2-src/hadoop-dist/target/hadoop-2.5.2.tar.gz
```
