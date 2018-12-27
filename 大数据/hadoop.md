### 1、安装
* 安装要求
> JDK 1.7 +

1. 到Apache Hadoop下载版本<br/>
`wget http://mirrors.tuna.tsinghua.edu.cn/apache/hadoop/common/hadoop-3.0.3/hadoop-3.0.3.tar.gz`

2. 配置java环境变量
```shell
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HADOOP_HOME=~/hadoop/hadoop3.0.3
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
修改/etc/hadoop/hadoop-env.sh的JAVA_HOME变量
```shell
export HDFS_DATANODE_SECURE_USER=root
export HDFS_DATANODE_SECURE_USER=root
export HDFS_SECONDARYNAMENODE_USER=root
export HDFS_NAMENODE_USER=root
export YARN_RESOURCEMANAGER_USER=root
#启动
% start-dfs.sh
% start-yarn.sh
% mr-jobhistory-daemon.sh start historyserver
#终止
% mr-jobhistory-daemon.sh stop historyserver
% stop-yarn.sh
% stop-dfs.sh
```
