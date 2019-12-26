## Sqoop简单教程

### Sqoop基本命令

```bash
$ sqoop help
$ sqoop help 命令名字
```

### Sqoop和MySQL

#### 1. 查询MySQL所有的数据库

```bash
$ sqoop list-databases \
--connect jdbc:mysql://mysql-host:3306/ \
--username root \
--password password
```

#### 2. 查询指定数据库中所有数据表

```bash
$ sqoop list-tables \
--connect jdbc:mysql://mysql-host:3306/mydatabase \
--username root \
--password password
```

### Sqoop和HDFS

#### 1. MySQL数据导入到HDFS

``` bash
$ sqoop import \
--connect jdbc:mysql://mysql-host:3306/mydatabase \
--username root \
--password password \
--table mytable \                # 待导入的表
--delete-target-dir \            # 目标目录存在则先删除
--target-dir /sqoop \            # 导入的目标目录
--fields-terminated-by '\t'  \   # 指定导出数据的分隔符
-m 3                             # 指定并行执行的 map tasks 数量
```

#### 2. HDFS数据导出到MySQL

```bash
$ sqoop export  \
    --connect jdbc:mysql://mysql-host:3306/mydatabase \
    --username root \
    --password password \
    --table mytable \        # 导出数据存储在MySQL的mytable的表中
    --export-dir /sqoop  \
    --input-fields-terminated-by '\t'\
    --m 3 
```

### Sqoop和Hive

#### 1. MySQL数据导入到Hive

```bash
$ sqoop import \
--connect jdbc:mysql://mysql-host:3306/mydatabase \
--username root \
--password password \
--table mytable \ # 待导入的表
--delete-target-dir \ # 如果临时目录存在删除
--target-dr /sqoop_hive \ # 临时目录位置
--hive-database sqoop_test \ # 导入到hive的sqoop数据库，需预先创建
--hive-import \ # 导入到hive
--hive-overwrite \ # 覆盖重写
-m 3 # 并行度
```

#### 2. Hive数据导出到MySQL

```bash
# 查看hive表在HDFS的储存位置
hive> desc formatted mytable;
```

```bash
sqoop export \
--connect jdbc:mysql://mysql-host:3306/mydatabase \
--username root \
--password password \
--table mytable \ 
--export-dir /user/hive/warehouse/sqoop_test.db/mytable \ 
-input-field-terminated-by '\001' \
--m 3
```

### 参考

[1] https://juejin.im/post/5d85939ef265da03ec2e9f50

[2] https://blog.bcmeng.com/post/sqoop.html

[3] [https://blog.csdn.net/sl1992/article/details/53887108#2hive%E8%A1%A8%E6%95%B0%E6%8D%AE%E5%AF%BC%E5%87%BA%E5%88%B0mysql%E8%A1%A8%E4%B8%AD](https://blog.csdn.net/sl1992/article/details/53887108#2hive表数据导出到mysql表中)


