- Reading and writing data to HDFS
- Compressing data using LZO
- Reading and writing data to SequenceFiles
- Using Apache Avro to serialize data
- Using Protocol Buffers to serialize data
- Setting the replication factor for HDFS
- Srtting the block size for HDFS

# Introduction

Keywords: _block replication_ _replication factor_ _data locality_

HDFS consists of three services:

HDFS Application   | Purpose
------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
NameNode           | 维护集群中所有block locations的数据字典（catalog）
Secondary NameNode | 定期与NameNode同步块索引（block index）。在同步期间，Secondary NameNode检索当前NameNode image和edit logs，并将它们合并，然后发送合并后的image给NameNode。Secondary NameNode不是NameNode的热备份。It cannot be used in the event of a NameNode failure.
DataNode           | 管理从NameNode接收到的data blocks. 它并不知道集群中的其他DataNode，只和NameNode进行通信

# Reading and writing data to HDFS
