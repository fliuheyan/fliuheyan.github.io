## Transaction Isolation
多个事务并发访问时，事务之间是隔离的，一个事务不应该影响其它事务运行效果。这指的是在并发环境中，当不同的事务同时操纵相同的数据时，每个事务都有各自的完整数据空间


### READ UNCOMMITTED (读未提交)
该隔离级别的事务会读到其它未提交事务的数据，此现象也称之为 脏读.

### READ COMMITTED（读提交）
`一个事务可以读取另一个已提交的事务`，多次读取会造成不一样的结果，此现象称为不可重复读问题，Oracle 和 SQL Server 的默认隔离级别。


### REPEATABLE READ（可重复读）
Mysql默认,select 的结果是事务开始时时间点的状态

问题： 会有`幻读`. InnoDB 引擎可以通过 next-key locks 机制

* 幻读举例:
1. tty1 `begin transaction`
2. tty2 `begin transaction` 
3. tty1 `insert into table(id) values(1) and commit`
4. 这个时候对于tty2来讲，是看不到table当中id=1的这条记录的，但是如果tty2这个时候insert了id=1就会发生主键冲突

### SERIALIZABLE（序列化）
避免了脏读、不可重读复读和幻读问题
