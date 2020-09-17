## Redis

### 数据结构
最大限制512MB
空字符串是有效的key
常用的key pattern `object-type:id:field` -> `User:01:name`
lists底层是链表(双向？) 适合数组插入
sorted sets 适合快速取
LRange 对list结果分页 ???

### redis 命令
set当key存在时会失败 / get
mset / mget  (pipeline)
del 删除key
type 返回value存储类型
expire key 5

##### 对lists, sets, sorted sets的操作
1. 向聚合数据添加元素时, 如果目标key不存在, 就在添加元素前创建空的聚合数据类型。
2. 在collection中删除元素之后，该collection为空，则redis会自动删除key
3. 对不存在的key调用只读操作，如llen返回0(等同一个空collection)

#### String
二进制安全的字符串 ??

与C语言String的区别
* 提供方便好用的操作数组的api
* 提供数组相关metadata，防止反复的遍历数组(Redis是单线程) 
* 提供冗余，防止反复的新增和释放内存

#### Hash
hash便于表示object
```
hmset user:01 name yisen
```

#### Lists
Linked list

#### Sets
Set

#### Sorted sets
每个element都关联到score floating number value.
所有的element都通过score进行排序


#### encoding

* int 8 bit
* embstr 39 bits
* raw > 39

#### hash (key,value)
hash便于表示一个object  
```
hmset user:01 name heyan
```

* hashtable
* ziplist

#### string
二进制安全的字符串

* 跟C语言的String的区别

* raw
* int
* emstr
#### list // array
* linkedlist
* ziplist

#### set //


* hashtable
* intset

#### zset
* skiplist
* ziplist

//TODO redis 内部数据类型转换(自动？？？)

### redis-cli

#### set
`set key value`
`setex` update
`setnx` create (分布式锁http://redis.io/topics/distlock)

* 批量设置
`mset k v k v`

#### get
* 批量查找
`mget k k k `


`keys *` //list all keys
`del key`
`dbsize` //count of keys
`exists key`
`expire key seconds`

`object encoding`//lookup data type

### boolean过滤器
