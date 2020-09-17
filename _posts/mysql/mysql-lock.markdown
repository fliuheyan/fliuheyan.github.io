## Mysql 锁
* 共享锁
`select * from tableName where... lock in share mode`

* 独占锁
`select * from tableName where... for update`


InnoDB 意向锁


### 行锁的算法
InnoDB 存储引擎使用三种行锁的算法用来满足相关事务隔离级别的要求。

Record Locks

该锁为索引记录上的锁，如果表中没有定义索引，InnoDB 会默认为该表创建一个隐藏的聚簇索引，并使用该索引锁定记录。

Gap Locks

该锁会锁定一个范围，但是不括记录本身。可以通过修改隔离级别为 READ COMMITTED 或者配置 innodb_locks_unsafe_for_binlog 参数为 ON 。

Next-key Locks

该锁就是 Record Locks 和 Gap Locks 的组合，即锁定一个范围并且锁定该记录本身。InnoDB 使用 Next-key Locks 解决幻读问题。需要注意的是，如果索引有唯一属性，则 InnnoDB 会自动将 Next-key Locks 降级为 Record Locks。举个例子，如果一个索引有 1, 3, 5 三个值，则该索引锁定的区间为 (-∞,1], (1,3], (3,5], (5,+ ∞) 。

### 死锁
InnoDB 引擎采取的是 wait-for graph 等待图的方法来自动检测死锁，如果发现死锁会自动回滚一个事务。

#### 锁优化
1. 合理设计索引，让 InnoDB 在索引键上面加锁的时候尽可能准确，尽可能的缩小锁定范围，避免造成不必要的锁定而影响其他 Query 的执行。
2. 尽可能减少基于范围的数据检索过滤条件，避免因为间隙锁带来的负面影响而锁定了不该锁定的记录。
3. 尽量控制事务的大小，减少锁定的资源量和锁定时间长度。
4. 在业务环境允许的情况下，尽量使用较低级别的事务隔离，以减少 MySQL 因为实现事务隔离级别所带来的附加成本。
