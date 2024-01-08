# setup_hadoop_ecosystem
## config ssh
- create ssh keygen
```bash
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
```

- read keygen public and write it to authorized_keys file
```bash
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

- grand permission for folder .ssh and file authorized_keys
```bash
sudo chmod 700 ~/.ssh
sudo chmod 600 ~/.ssh/authorized_keys
```

- restart service if don't know it start or no
``` bash
sudo service ssh restart
```

- connect ssh to localhost 
```bash
ssh localhost
```

## java environment
- download java if not exists
```bash
sudo apt-get update
sudo apt-get install openjdk-8-jdk
```

## config hadoop

- download hadoop
```bash 
# for example
wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz
```

- extract file download
```bash
# for example
tar -zxvf hadoop-3.3.6.tar.gz
```

- (optional) rename folder extract
```bash
# for example
mv hadoop-3.3.6 hadoop
```

- add these to end of file ~/.bashrc
```sh
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HADOOP_HOME=/home/hadoop/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export HADOOP_YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
```

- re-run sh
```bash
source ~/.bashrc
```

- setup 4 files:
```
- hadoop-env.sh 
- core-site.xml
- hdfs-site.xml
- yarn-site.xml
```

- setup file hadoop-env.sh on dir $HADOOP_HOME/etc/hadoop/:
```sh
# add these to this file or modify them if exists
JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

export HADOOP_SSH_OPTS="-p 22"
```

- setup file core-site.xml on dir $HADOOP_HOME/etc/hadoop/:
```
<property>
    <name>fs.defaultFS</name>
    <value>hdfs://localhost:9000</value>
</property>
```

- go to path ~/hadoop and create folder datanode and namenode
```bash
cd ~/hadoop
mkdir -p data/{datanode,namenode}
```

- setup file hdfs-site.xml on dir $HADOOP_HOME/etc/hadoop/:
```
<property>
    <name>dfs.replication</name>
    <value>1</value>
    <description>The default replication factor of files on HDFS</description>
</property>
<property>
    <name>dfs.name.dir</name>
    <value>file:///home/hadoop/hadoop/data/namenode</value>
</property>
<property>
    <name>dfs.data.dir</name>
    <value>file:///home/hadoop/hadoop/data/datanode</value>
</property>
```

- setup file yarn-site.xml on dir $HADOOP_HOME/etc/hadoop/:
```
<property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
</property>
<property>
    <name>yarn.nodemanager.env-whitelist</name>
    <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_HOME,PATH,LANG,TZ, HADOOP_MAPRED_HOME</value>
</property>
```

- format namenode
```bash
hdfs namenode -format
```

- start hadoop
```bash
start-all.sh
```

## config spark
- download spark
```bash
# for example
wget https://dlcdn.apache.org/spark/spark-3.5.0/spark-3.5.0-bin-hadoop3.tgz
```

- extract file
```bash
tar -zxvf spark-3.5.0-bin-hadoop3.tgz
```

- (optional) rename file
```bash
mv spark-3.5.0-bin-hadoop3/ spark
```

- edit ~/.bashrc file
```sh
# add these to end of file
export SPARK_HOME=/home/hadoop/spark
export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin
```

- re-run ~/.bashrc file
```bash
source ~/.bashrc
```
