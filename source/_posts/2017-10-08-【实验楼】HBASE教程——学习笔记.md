---
title: 【实验楼】HBASE教程——学习笔记
date: 2017-10-08 18:08:55
---

## HBase环境搭建与配置

HBase解压即可使用。

【注意】伪分布模式下，HBase需要与Hadoop版本匹配，可以看HBase的lib里Hadoop的jar文件版本。

需要配置`hbase-site.xml`，可以使用自带的Zookeeper。

单机模式配置如下：

``` xml
<configuration>
    <property>
        <name>hbase.rootdir</name>
        <value>file:///tmp/hbase-${user.name}/hbase</value>
    </property>
</configuration>
```

<!-- more -->

伪分布模式配置如下：

``` xml
<configuration>
    <property>
        <name>hbase.rootdir</name>
        <value>hdfs://localhost:9000/hbase</value>
    </property>
    <property>
        <name>hbase.cluster.distributed</name>
        <value>true</value>
    </property>
</configuration>
```

配置`hbase-env.sh`，需要设置`JAVA_HOME`和`HBASE_MANAGES_ZK`。使用自带的Zookeeper时，`HBASE_MANAGES_ZK=true`

启动HBase：`start-hbase.sh`，停止HBase：`stop-hbase.sh`

注意：若是伪分布模式，需要先启动HDFS。停止时先停HBase。

进入HBase Shell：`hbase shell`

## HBase基本操作

显示帮助：`help`

显示状态：`status`

退出HBase：`quit`

创建新表：`create '表名', '列族名'`

列举表信息：`list`、`list '表名'`

获取表描述：`describe '表名'`

删除表：删除表之前，先`disable`表，再`drop`表

检查表是否存在：`exists '表名'`

禁用表：`disable '表名'`，启用表：`enable '表名'`

向表中插入数据：`put '表名', '行', '列族:列', '值'`

一次性扫描全表数据：`scan '表名'`

获取一行数据：`get '表名', '行'`

## HBase应用程序开发

`org.apache.hadoop.hbase`包下常用类：

- HBaseConfiguration   HBase配置

- HColumnDescriptor   列族描述符，指定列族相关信息

- HTableDescriptor  表名描述符，指定表相关信息

`org.apache.hadoop.hbase.client`包下常用类：

- HBaseAdmin  HBase管理（判断表是否存在、创建表等）

- HTable  表

- Get  获取数据

- Put 添加数据

- Scan

- Result

- ResultScanner

创建表：

``` java
HBaseAdmin admin = new HBaseAdmin(conf);
HTableDescriptor tabDesc = new HTableDescriptor(tableName);
tabDesc.addFamily(new HColumnDescriptor(columnFamily));
admin.createTable(tabDesc);
```

``` java
Table tab = new HTable(conf, tableName);
Put p = new Put(Bytes.toBytes(row));
p.add(Bytes.toBytes(columnFamily), Bytes.toBytes(column), Bytes.toBytes(data));
tab.put(p);
```

获取、扫描数据：

``` java
HTable tab = new HTable(conf, tableName);
// get
Get g = new Get(Bytes.toBytes(row));
Result r= tab.get(g);
// scan
Scan s = new Scan();
ResultScanner rs = tab.getScanner(s);
for(Result rst:rs){
    ; // do something
}
```

删除表：

``` java
if(admin.tableExists(tableName)){
    admin.disableTable(tableName);
    admin.deleteTable(tableName);
}
```

