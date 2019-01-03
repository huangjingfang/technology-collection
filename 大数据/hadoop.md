### 1、Hadoop安装
>* 安装要求
>
> JDK 1.7 +

1. 到Apache Hadoop下载版本<br/>
`wget http://mirrors.tuna.tsinghua.edu.cn/apache/hadoop/common/hadoop-3.0.3/hadoop-3.0.3.tar.gz`

2. 配置java环境变量
```shell
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HADOOP_HOME=~/hadoop/hadoop-3.0.3
export PATH=$PATH:$JAVA_HOME/bin:$HADOOP_HOME/bin
```
3. 检查是否可用
```shell
hadoop version
# Hadoop 3.0.3
# Source code repository https://yjzhangal@git-wip-us.apache.org/repos/asf/hadoop.git -r 37fd7d752db73d984dc31e0cdfd590d252f5e075
# Compiled by yzhang on 2018-05-31T17:12Z
# Compiled with protoc 2.5.0
# From source with checksum 736cdcefa911261ad56d2d120bf1fa
# This command was run using /root/hadoop/hadoop-3.0.3/share/hadoop/common/hadoop-common-3.0.3.jar
```
4. 配置<br/>
Hadoop有三种运行模式
   * 独立模式<br/>
      默认情况，无需配置
   * 伪分布式模式<br/>
   需要指定'core-site.xml'(设置文件系统),'hdfs-site.xml'(dfs副本数),'mapred-site.xml'(mapreduce的框架),'yarn-site.xml'(yarn设置)
   * 全分布式模式

5. 格式化dfs<br/>
`hdfs namenode -format`

6. 启动和终止守护进程<br/>
启动和终止hdfs,mapreduce,yarn的守护进程
修改/etc/hadoop/hadoop-env.sh的JAVA_HOME变量<br/>

```shell
export HDFS_DATANODE_SECURE_USER=root
export HDFS_DATANODE_SECURE_USER=root
export HDFS_SECONDARYNAMENODE_USER=root
export HDFS_NAMENODE_USER=root

export YARN_RESOURCEMANAGER_USER=root
export YARN_NODEMANAGER_USER=root
#启动
% start-dfs.sh
% start-yarn.sh
% mr-jobhistory-daemon.sh start historyserver
#终止
% mr-jobhistory-daemon.sh stop historyserver
% stop-yarn.sh
% stop-dfs.sh
```

主要配置如下：
```xml
<!--core-site.xml-->
<configuration>
   <property>
        <name>fs.defaultFS</name>
        <value>hdfs://192.168.40.203:9000</value>
   </property>
</configuration>

<!--yarn-site.xml-->
<configuration>
   <property>
        <name>yarn-resourcemanager.hostname</name>
        <value>localhost</value>
   </property>
   <property>
        <name>yarn.nodemanager.aux-service</name>
        <value>mapreduce_shuffle</value>
   </property>
   <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
  </property>
  <property>
        <name>yarn.nodemanager.aux-services.mapreduce_shuffle.class</name>
        <value>org.apache.hadoop.mapred.ShuffleHandler</value>
  </property>
</configuration>

<!--hdfs-site.xml-->
<configuration>
   <property>
        <name>dfs.replication</name>
        <value>1</value>
   </property>
</configuration>

<!--mapred-site.xml-->
<configuration>
   <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
   </property>
   <property>
        <name>yarn.app.mapreduce.am.env</name>
        <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
    </property>
    <property>
        <name>mapreduce.map.env</name>
        <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
    </property>
    <property>
        <name>mapreduce.reduce.env</name>
        <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
    </property>
</configuration>
```


### Hive安装

1. 下载hive<br/>
`wget http://mirror.bit.edu.cn/apache/hive/hive-1.2.2/apache-hive-1.2.2-bin.tar.gz`

2. 配置环境变量<br/>
```shell
export HIVE_HOME=/root/hadoop/hive/apache-hive-1.2.2-bin
export PATH=$PATH:$HIVE_HOME/bin
export CLASSPATH=$CLASSPATH:/usr/local/Hadoop/lib/*:.
export CLASSPATH=$CLASSPATH:/usr/local/hive/lib/*:.
```

3. 配置hive
```shell
$ cd $HIVE_HOME/conf
$ cp hive-env.sh.template hive-env.sh
$ vim hive-env.sh
$ export HADOOP_HOME=$HADOOP_HOME
```

4. 下载安装Apache Derby
```shell
#derby
wget http://archive.apache.org/dist/db/derby/db-derby-10.12.1.1/db-derby-10.12.1.1-bin.tar.gz
export DERBY_HOME=/root/hadoop/derby/db-derby-10.12.1.1-bin
export PATH=$PATH:$DERBY_HOME/bin
#创建目录存放metastore
mkdir $DERBY_HOME/data
```
```xml
<!--MYSQL配置-->
<property>
        <name>javax.jdo.option.ConnectionUserName</name>
        <value>apprentice</value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionPassword</name>
        <value>Master2.Y</value>
    </property>
   <property>
        <name>javax.jdo.option.ConnectionURL</name>
        <value>jdbc:mysql://118.31.35.1:3306/hive</value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionDriverName</name>
        <value>com.mysql.jdbc.Driver</value>
    </property>
```
Hive2+如果使用mysql存储元数据需要手动执行`schematool -dbType mysql -initSchema`


5. 配置hive的metastore
```shell
$ cd $HIVE_HOME/conf
$ cp hive-default.xml.template hive-site.xml
```
修改hive-site.xml中system:java.io.tmpdir为一个本地目录


### HBase 安装
1. 下载

2. 配置<br/>

  * hbase-env.sh
```shell
export JAVA_HOME=/usr/local/java/jdk1.6.0_27    #Java安装路径
export HBASE_CLASSPATH=/usr/local/hadoop/conf    #HBase类路径
export HBASE_MANAGES_ZK=true    #由HBase负责启动和关闭Zookeeper
```
  * hbase-site.xml

```xml
<property>
           <name>hbase.master</name>
           <value>master:6000</value>
   </property>
   <property>
           <name>hbase.master.maxclockskew</name>
           <value>180000</value>
   </property>
   <property>
           <name>hbase.rootdir</name>
           <value>hdfs://master:9000/hbase</value>
   </property>
   <property>
           <name>hbase.cluster.distributed</name>
           <value>true</value>
   </property>
   <property>
           <name>hbase.zookeeper.quorum</name>
           <value>master</value>
   </property>
   <property>
           <name>hbase.zookeeper.property.dataDir</name>
           <value>/home/${user.name}/tmp/zookeeper</value>
   </property>
   <property>
           <name>dfs.replication</name>
           <value>1</value>
   </property>

```

其中，hbase.master是指定运行HMaster的服务器及端口号；hbase.master.maxclockskew是用来防止HBase节点之间时间不一致造成regionserver启动失败，默认值是30000；hbase.rootdir指定HBase的存储目录；hbase.cluster.distributed设置集群处于分布式模式；hbase.zookeeper.quorum设置Zookeeper节点的主机名，它的值个数必须是奇数；hbase.zookeeper.property.dataDir设置Zookeeper的目录，默认为/tmp，dfs.replication设置数据备份数，集群节点小于3时需要修改，本次试验是一个节点，所以修改为1。

java.lang.IllegalStateException: The procedure WAL relies on the ability to hsync for proper operation during component failures, but the underlying filesystem does not support doing so. Please check the config value of 'hbase.procedure.store.wal.use.hsync' to set the desired level of robustness and ensure the config value of 'hbase.wal.dir' points to a FileSystem mount that can provide it.

hbase-site.xml增加配置
```xml
<property>
<name>hbase.unsafe.stream.capability.enforce</name>
<value>false</value>
</property>
```


### Kylin 安装

1. 下载

2. 配置环境变量
