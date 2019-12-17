---
title: "Apache Oozie Installation on Ubuntu"
date: "2015-06-23"
template: "post"
slug: "apache-oozie-installation-on-ubuntu"
category: "hadoop"
tags:
  - "hadoop"
description: ""
socialImage: ""
---

**Followings steps are for: Ubuntu 14.04 LTS, Hadoop-2.6.0**

### Prerequisites:

- Ubuntu 14.04
- Java JDK 1.7
- Maven 3.0.5
- Hadoop 2.6.0

### Install Oozie

```shell
# install Maven
sudo apt-get update
sudo apt-get install maven

# build Oozie
wget http://mirrors.whoishostingthis.com/apache/oozie/4.1.0/oozie-4.1.0.tar.gz
tar -xzvf oozie-4.1.0.tar.gz
cd oozie-4.1.0
bin/mkdistro.sh -DskipTests -Dhadoopversion=2.6.0

# copy the binary distribution
cp -R distro/target/oozie-4.1.0-distro/oozie-4.1.0/* ~/oozie-4.1

# edit /etc/profile
sudo vim /etc/profile

    export OOZIE_VERSION=4.1.0
    export OOZIE_HOME=/home/anggao/oozie-4.1
    export PATH=$PATH:$OOZIE_HOME/bin

source /etc/profile

# enable web console for Oozie
# we need ext-*.*.zip library and extjs
cd $OOZIE_HOME
mkdir libext
cp ../oozie-4.1.0/hadooplibs/target/oozie-4.1.0-hadooplibs.tar.gz .
tar -xzvf oozie-4.1.0-hadooplibs.tar.gz
cp oozie-4.1.0/hadooplibs/hadooplib-2.3.0.oozie-4.1.0/* libext
cd libext/
wget http://archive.cloudera.com/gplextras/misc/ext-2.2.zip
```

```xml
# Configure the Hadoop cluster with proxyuser for the Oozie process.
# The following two properties are required in Hadoop core-site.xml:

  <!-- OOZIE -->
  <property>
      <name>hadoop.proxyuser.anggao.hosts</name>
      <value>*</value>
  </property>
  <property>
      <name>hadoop.proxyuser.anggao.groups</name>
      <value>*</value>
  </property>
```

```shell
# Prepare oozie war file
oozie-setup.sh prepare-war

# Create Sharelib Directory on HDFS

# first get HDFS info
hdfs getconf -confKey fs.defaultFS

# use the info obtained above
oozie-setup.sh sharelib create -fs hdfs://YARN001:8020
```

```xml
# use the info obtained above
oozie-setup.sh sharelib create -fs hdfs://YARN001:8020

# update oozie-site.xml under OOZIE_CONF_DIR
    <property>
        <name>oozie.service.HadoopAccessorService.hadoop.configurations</name>
        <value>*=/home/anggao/hadoop-2.6.0/etc/hadoop/</value>
        <description>
            Comma separated AUTHORITY=HADOOP_CONF_DIR, where AUTHORITY is the HOST:PORT of
            the Hadoop service (JobTracker, HDFS). The wildcard '*' configuration is
            used when there is no exact match for an authority. The HADOOP_CONF_DIR contains
            the relevant Hadoop *-site.xml files. If the path is relative is looked within
            the Oozie configuration directory; though the path can be absolute (i.e. to point
            to Hadoop client conf/ directories in the local filesystem.
        </description>
    </property>

    <property>
        <name>oozie.service.WorkflowAppService.system.libpath</name>
        <value>hdfs:///user/${user.name}/share/lib</value>
        <description>
            System library path to use for workflow applications.
            This path is added to workflow application if their job properties sets
            the property 'oozie.use.system.libpath' to true.
        </description>
    </property>
```

```shell
# Create oozie database
ooziedb.sh create -sqlfile oozie.sql -run

# Start Oozie Service
oozied.sh start

# Verify status of Oozie service
oozie admin --oozie http://localhost:11000/oozie -status

# Web UI
http://localhost:11000/oozie/
```
