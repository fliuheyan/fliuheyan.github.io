## Elasticsearch

### Concept
* Index (database) ->* type(table)-> * Document(Row) -> * Field(Column)

一般的搜索格式为
```
GET /{index}/{type}/_search
{
    "query": {
        "match": {
            "field": text
        } 
    }
}
```


* Aggregation
功能更加强大的group by

### 文档更新
#### 文档mata data

* _index

* _type 代表文档的类

* _id 文档唯一ID

#### reindex

#### replace

#### 增量更新
update API. Doc全部为immutale的

#### 全量更新

### 批量操作

`mget` API. 批量get
`bulk` API  batch create, delete, update 5-15MB

### Cluster
Indexpiliang持有数据在每个shard中的地址

每台机器是一个node，一个node可以拥有多个shard.
node之前采用了master slave的配置
slave节点的shard则为
分shard，每一个shard就是一个Lucene instance,所有document均匀分布在shard当中，然后做load balance


### Shard的生命周期，如何做同步

### Monitor

heart beat url `/_cluster/health`
