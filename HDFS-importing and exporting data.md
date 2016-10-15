# Hadoop Distributed File System – Importing and Exporting Data

导入数据到 HDFS 和从 HDFS 导出数据。数据源和目的地包括：本地文件系统，关系型数据库，NoSQL 数据库，分布式数据库，以及其它的 Hadoop 集群。

## Importing and exporting data into HDFS using the Hadoop shell commands

```
hadoop fs -COMMAND
```

```
Usage: hadoop fs [generic options]
        [-appendToFile <localsrc> ... <dst>]
        [-cat [-ignoreCrc] <src> ...]
        [-checksum <src> ...]
        [-chgrp [-R] GROUP PATH...]
        [-chmod [-R] <MODE[,MODE]... | OCTALMODE> PATH...]
        [-chown [-R] [OWNER][:[GROUP]] PATH...]
        [-copyFromLocal [-f] [-p] [-l] <localsrc> ... <dst>]
        [-copyToLocal [-p] [-ignoreCrc] [-crc] <src> ... <localdst>]
        [-count [-q] [-h] <path> ...]
        [-cp [-f] [-p | -p[topax]] <src> ... <dst>]
        [-createSnapshot <snapshotDir> [<snapshotName>]]
        [-deleteSnapshot <snapshotDir> <snapshotName>]
        [-df [-h] [<path> ...]]
        [-du [-s] [-h] <path> ...]
        [-expunge]
        [-find <path> ... <expression> ...]
        [-get [-p] [-ignoreCrc] [-crc] <src> ... <localdst>]
        [-getfacl [-R] <path>]
        [-getfattr [-R] {-n name | -d} [-e en] <path>]
        [-getmerge [-nl] <src> <localdst>]
        [-help [cmd ...]]
        [-ls [-d] [-h] [-R] [<path> ...]]
        [-mkdir [-p] <path> ...]
        [-moveFromLocal <localsrc> ... <dst>]
        [-moveToLocal <src> <localdst>]
        [-mv <src> ... <dst>]
        [-put [-f] [-p] [-l] <localsrc> ... <dst>]
        [-renameSnapshot <snapshotDir> <oldName> <newName>]
        [-rm [-f] [-r|-R] [-skipTrash] <src> ...]
        [-rmdir [--ignore-fail-on-non-empty] <dir> ...]
        [-setfacl [-R] [{-b|-k} {-m|-x <acl_spec>} <path>]|[--set <acl_spec> <path>]]
        [-setfattr {-n name [-v value] | -x name} <path>]
        [-setrep [-R] [-w] <rep> <path> ...]
        [-stat [format] <path> ...]
        [-tail [-f] <file>]
        [-test -[defsz] <path>]
        [-text [-ignoreCrc] <src> ...]
        [-touchz <path> ...]
        [-truncate [-w] <length> <path> ...]
        [-usage [cmd ...]]
Generic options supported are
-conf <configuration file>     specify an application configuration file
-D <property=value>            use value for given property
-fs <local|namenode:port>      specify a namenode
-jt <local|resourcemanager:port>    specify a ResourceManager
-files <comma separated list of files>    specify comma separated files to be copied to the map reduce cluster
-libjars <comma separated list of jars>    specify comma separated jar files to include in the classpath.
-archives <comma separated list of archives>    specify comma separated archives to be unarchived on the compute machines.
The general command line syntax is
bin/hadoop command [genericOptions] [commandOptions]
```

获取更多具体的信息，可使用`hadoop fs -help COMMAND`获取详细内容。

这些命令常用于加载 ad hoc data (临时数据)，下载已处理过的数据，维护文件系统，查看文件夹内容

> 实例：在 HDFS 中创建 _/data/weblogs_ 文件夹并将 weblog_entries.txt 从本地文件系统复制到 HDFS。

> 1. 在 HDFS 中创建用于存放 _weblog_entries.txt_ 的目录：

>   ```
>   hadoop fs -mkdir /data/weblogs
>   ```

>   若提示：" mkdir: `/data/weblogs': No such file or directory ”，则先执行一遍创建 data 目录的命令就可以了`hadoop fs -mkdir /data`

> 2. 将 weblog_entries.txt 文件从本地文件系统复制到 HDFS 中新创建的目录中：

>   ```
>   hadoop fs -copyFromLocal weblog_entries.txt /data/weblogs
>   ```

> 3. 查看 /data/weblogs 目录和 weblog_entries.txt 的内容：

>   ```
>   hadoop fs -ls /data/weblogs
>   hadoop fs -cat /data/weblogs/weblog_entries.txt
>   ```

## Moving data efficiently between clusters using Distributed Copy

## Importing data from MySQL into HDFS using Sqoop

## Exporting data from HDFS into MySQL using Sqoop

## Configuring Sqoop for Microsoft SQL Server

## Exporting data from HDFS into MongoDB

## Importing data from MongoDB into HDFS

## Exporting data from HDFS into MongoDB using Pig

## Using HDFS in a Greenplum external table

## Using Flume to load data into HDFS
