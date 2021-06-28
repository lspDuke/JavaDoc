# MySql笔记

## 引擎

1. **MyISAM**

   > 不支持事务、也不支持外键，优势是访问速度快，对事务完整性没有 要求或者以select，insert为主的应用基本上可以用这个引擎来创建表

2. **InnoDB**

   > 该存储引擎提供了具有提交、回滚和崩溃恢复能力的事务安全。但是对比MyISAM引擎，写的处理效率会差一些，并且会占用更多的磁盘空间以保留数据和索引。
   > InnoDB存储引擎的特点：支持自动增长列，支持外键约束

**强烈建议：**InnoDB

## 隔离级别

### 未提交读(read uncommitted)

​		A事务已执行，但未提交；B事务查询到A事务的更新后数据；A事务回滚；---出现脏数据

### 已提交读(read committed)

​		A事务执行更新；B事务查询；A事务又执行更新；B事务再次查询时，前后两次数据不一致；---不可重复读

### 可重复读(repeatable read)

​		A事务无论执行多少次，只要不提交，B事务查询值都不变；B事务仅查询B事务开始时那一瞬间的数据快照；

### 串行化(serializable)

​		不允许读写并发操作，写执行时，读必须等待；

###　MySql默认级别：(RR)可重复读

###　疑问

1. 为什么是可重复读？

   > 在MySql5.0以前binlog只有statement一种模式。在这种模式下选(RC)已提交读会出现主从复制不一致的现象，所有选择了(RR)可重复读

2. 其它数据库是默认级别是什么？

   > Oracle，SqlServer 都是选择的(RC)已提交读

3. 项目中应该选什么级别,为什么？

   > (RC)已提交读。
   >
   > (1)  在RR隔离级别下，存在间隙锁，导致出现死锁的几率比RC大的多！
   >
   > (2)  在RR隔离级别下，条件列未命中索引会锁表！而在RC隔离级别下，只锁行

4. 在RC级别下，主从复制用什么binlog模式？

   > row模式

5. binlog在版本5.0之后有几种模式？

   > statement: 记录的是修改SQL语句
   > row：          记录的是每行实际数据的变更
   > mixed：      statement和row模式的混合

   | statement | 记录的是修改SQL语句        | 日志文件小，节约IO，提高性能   | 准确性差，对一些系统函数不能准确复制或不能复制，如now()、uuid()等 |
   | --------- | -------------------------- | :----------------------------- | ------------------------------------------------------------ |
   | row       | 记录的是每行实际数据的变更 | 准确性强，能准确复制数据的变更 | 日志文件大，较大的网络IO和磁盘IO                             |
   | mixed     | statement和row模式的混合   | 准确性强，文件大小适中         | 有可能发生主从不一致问题                                     |

**强烈建议：**选**RC**且**binlog**用**row**模式

##　MySql操作

1. 数据库级别

   ```mysql
   -- 查看当前事物级别：
   SELECT @@tx_isolation;
   -- 设置mysql的隔离级别：
   set session transaction isolation level 设置事务隔离级别
   
   -- 设置read uncommitted级别：
   set session transaction isolation level read uncommitted;
   
   -- 设置read committed级别：
   set session transaction isolation level read committed;
   
   -- 设置repeatable read级别：
   set session transaction isolation level repeatable read;
   
   -- 设置serializable级别：
   set session transaction isolation level serializable;
   ```

   

2. binlog

   ```mysql
   -- 查看 binlog 模式
   show variables like 'binlog_format'
   -- 查看　同步功能
   show variables like 'log_bin';
   
   --　开启binlog 一般修改/etc/my.inf文件中
   #log_bin
   log-bin = mysql-bin #开启binlog
   binlog-format = ROW #选择row模式
   server_id = 12345 #配置mysql replication需要定义，不能和canal的slaveId重复
   
   show binary logs   #获取binlog文件日志列表
   show master status  # 查看当前正在写入的binlog文件
   show master logs  # 查看master上的binlog文件
   show binlog events  #查看第一个binlog文件内容
   show binlog events 'mysql-bin.000002'  # 查看指定binlog文件内容， 如：查看mysql-bin.000002文件内容
   ```

   

##　表操作

###　1.新增

```mysql
INSERT INTO 表名(字段1,字段2,字段...)　VALUE(数据1,数据2,数据...);
```

### 2.删除

```mysql
DELETE FROM 表名 WHERE 条件;
```

### 3.修改

```mysql
UPDATE 表名 SET 字段=xx WHERE 条件;
```

### 4. 查询

```
SELECT 字段1,字段2,字段... FROM 表名;
```

## 慢查询日记操作

```mysql
# 查看是否开启慢查询日志show variables like "slow_query_log";

# 查看是否设置了把没有索引的记录到慢查询日志show variables like "log_queries_not_using_indexes";

# 查看是否设置慢查询的 SQL 执行时间show variables like "long_query_time";

# 查看慢查询日志记录位置show variables like "slow_query_log_file";

# 开启慢查询日志set global slow_query_log=on

# 设置没有索引的记录到慢查询日志set global log_queries_not_using_indexes=on

# 设置到慢查询日志的 SQL 执行时间set global long_query_time=0

# 查看慢查询日志（在 Linux 终端下执行）tail -50 /usr/local/var/mysql/luyiyuandeMacBook-Pro-slow.log;
```

