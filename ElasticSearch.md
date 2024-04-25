#                                                 ElasticSearch





## 原理简述

源文档：https://blog.csdn.net/u011863024/article/details/115721328



![image-20231113102413087](C:\Users\32784\Desktop\笔记\image\image-20231113102413087.png)



查询过程：

a) 用户输入查询语句。

b) 对查询语句经过词法分析得到一系列词(Term) 。

c) 通过语法分析得到一个查询树。

d) 通过索引存储将索引读入到内存。

e) 利用查询树搜索索引，从而得到每个词(Term) 的文档链表，对文档链表进行交、差、并得到结果文档。

f) 将搜索到的结果文档对查询的相关性进行排序。

g) 返回查询结果给用户。



### 5.2 ES写数据

ES写数据包含两种情况，分别为写入一个新的文档和在原有文档的基础上进行数据的追加（覆盖原有的文档）。两者基本上没有什么区别，后者是把原来的文档进行删除，再重新写入。

ES写数据流程：

（1） 客户端选择一个ES节点发送写请求，ES节点接收请求变为协调节点。

（2） 协调节点判断写请求中如果没有指定文档id，则自动生成一个doc_id。协调节点对doc_id进行哈希取值，判断出文档应存储在哪个切片中。协调节点找到存储切片的对应节点位置，将请求转发给对应的node节点。

（3） Node节点的primary shard处理请求，并将数据同步到replica shard

（4） 协调节点发现所有的primary shard和所有的replica shard都处理完之后，就返回结果给客户端。





### 5.2 ES写数据

ES写数据包含两种情况，分别为写入一个新的文档和在原有文档的基础上进行数据的追加（覆盖原有的文档）。两者基本上没有什么区别，后者是把原来的文档进行删除，再重新写入。

ES写数据流程：

（1） 客户端选择一个ES节点发送写请求，ES节点接收请求变为协调节点。

（2） 协调节点判断写请求中如果没有指定文档id，则自动生成一个doc_id。协调节点对doc_id进行哈希取值，判断出文档应存储在哪个切片中。协调节点找到存储切片的对应节点位置，将请求转发给对应的node节点。

（3） Node节点的primary shard处理请求，并将数据同步到replica shard

（4） 协调节点发现所有的primary shard和所有的replica shard都处理完之后，就返回结果给客户端。

 

ES写数据底层原理：

（1）  数据先写入内存 buffer，然后每隔 1s，将数据 refresh 到操作系统缓存（os cache），生成新的segment。（os cache 中存储的数据能被搜索到）

（2）    写入 os cache 中的translog数据，默认每隔 5 秒刷一次到磁盘中去，如果translog 大到一定程度，或者默认每隔 30mins，会触发 commit 操作，将缓冲区的数据都 flush 到 segment file 磁盘文件中。

 

### 5.3 ES读数据

ES读数据是通过doc_id来进行查询，先根据doc_id判断出文档存储在哪个切片上，再从切片上把数据读取过来。

ES读数据流程：

（1） 客户端给任意一个节点发送请求，该节点变为协调节点

（2） 协调节点根据doc_id，进行哈希取值，判断出文档存储在哪个切片上。

（3） 协调节点将请求转发到对应的节点上，然后使用随机轮询算法（round-robin）,在切片和副本切片中随机选择一个，以使读请求负载均衡

（4） 接收请求的节点返回文档数据给协调节点，协调节点再返回数据给客户端。

 

### 5.4 ES检索关键词

ES检索关键词流程：

ES检索关键词是ES最常使用的做法，通过关键词，将包含关键词的文档全部搜索出来。

（1） 客户端向任意一个节点发送请求，该节点变为协调节点

（2） 协调节点将搜索请求转到所有的shard上

（3） 每个shard将自身的检索结果（搜索到的doc_id和分数）,返回给协调节点。

（4） 协调节点根据检索结果进行相关性排序，产出最终的结果。再把doc_id发送给各个节点，拉取文档数据，最终返回给客户端。

 

### 5.5 ES删数据

删除数据底层原理：

删除操作，是在commit 的时候会生成一个.del文件，里面将doc标识为deleted状态，搜索的时候根据.del文件就知道这个 doc 是否被删除了。





## 7.  ES常用API

ES Restful API GET、POST、PUT、DELETE、HEAD含义： 
1）GET：获取请求对象的当前状态。 
2）POST：改变对象的当前状态。 
3）PUT：创建一个对象。 
4）DELETE：销毁对象。 
5）HEAD：请求获取对象的基础信息。









## 08-入门-HTTP-索引-创建

对比关系型数据库，创建索引就等同于创建数据库。

在 Postman 中，向 ES 服务器发 PUT 请求 ： http://127.0.0.1:9200/shopping

请求后，服务器返回响应：

```json
{
    "acknowledged": true,//响应结果
    "shards_acknowledged": true,//分片结果
    "index": "shopping"//索引名称
}
```

1
2
3
4
5
后台日志：

[2021-04-08T13:57:06,954][INFO ][o.e.c.m.MetadataCreateIndexService] [DESKTOP-LNJQ0VF] [shopping] creating index, cause [api], templates [], shards [1]/[1], mappings []
1
如果重复发 PUT 请求 ： http://127.0.0.1:9200/shopping 添加索引，会返回错误信息 :

```json
k{
    "error": {
        "root_cause": [
            {
                "type": "resource_already_exists_exception",
                "reason": "index [shopping/J0WlEhh4R7aDrfIc3AkwWQ] already exists",
                "index_uuid": "J0WlEhh4R7aDrfIc3AkwWQ",
                "index": "shopping"
            }
        ],
        "type": "resource_already_exists_exception",
        "reason": "index [shopping/J0WlEhh4R7aDrfIc3AkwWQ] already exists",
        "index_uuid": "J0WlEhh4R7aDrfIc3AkwWQ",
        "index": "shopping"
    },
    "status": 400
}


```



## 查看所有索引

在 Postman 中，向 ES 服务器发 GET 请求 ： http://127.0.0.1:9200/_cat/indices?v

这里请求路径中的_cat 表示查看的意思， indices 表示索引，所以整体含义就是查看当前 ES服务器中的所有索引，就好像 MySQL 中的 show tables 的感觉，服务器响应结果如下 :

health status index    uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   shopping J0WlEhh4R7aDrfIc3AkwWQ   1   1          0            0       208b           208b
1
2
表头	含义
health	当前服务器健康状态： green(集群完整) yellow(单点正常、集群不完整) red(单点不正常)
status	索引打开、关闭状态
index	索引名
uuid	索引统一编号
pri	主分片数量
rep	副本数量
docs.count	可用文档数量
docs.deleted	文档删除状态（逻辑删除）
store.size	主分片和副分片整体占空间大小
pri.store.size	主分片占空间大小
查看单个索引
在 Postman 中，向 ES 服务器发 GET 请求 ： http://127.0.0.1:9200/shopping

返回结果如下：

```json
{
    "shopping": {//索引名
        "aliases": {},//别名
        "mappings": {},//映射
        "settings": {//设置
            "index": {//设置 - 索引
                "creation_date": "1617861426847",//设置 - 索引 - 创建时间
                "number_of_shards": "1",//设置 - 索引 - 主分片数量
                "number_of_replicas": "1",//设置 - 索引 - 主分片数量
                "uuid": "J0WlEhh4R7aDrfIc3AkwWQ",//设置 - 索引 - 主分片数量
                "version": {//设置 - 索引 - 主分片数量
                    "created": "7080099"
                },
                "provided_name": "shopping"//设置 - 索引 - 主分片数量
            }
        }
    }
}

```







## 删除索引

在 Postman 中，向 ES 服务器发 DELETE 请求 ： http://127.0.0.1:9200/shopping

返回结果如下：

```json
{
    "acknowledged": true
}
```


再次查看所有索引，GET http://127.0.0.1:9200/_cat/indices?v，返回结果如下：

health status index uuid pri rep docs.count docs.deleted store.size pri.store.size

成功删除。











## 10-入门-HTTP-文档-创建（Put & Post）

假设索引已经创建好了，接下来我们来创建文档，并添加数据。这里的文档可以类比为关系型数据库中的表数据，添加的数据格式为 JSON 格式

在 Postman 中，向 ES 服务器发 POST 请求 ： http://127.0.0.1:9200/shopping/_doc，请求体JSON内容为：

```json
{
    "title":"小米手机",
    "category":"小米",
    "images":"http://www.gulixueyuan.com/xm.jpg",
    "price":3999.00
}
```


注意，此处发送请求的方式必须为 POST，不能是 PUT，否则会发生错误 。

返回结果：

```json
{
    "_index": "shopping",//索引
    "_type": "_doc",//类型-文档
    "_id": "ANQqsHgBaKNfVnMbhZYU",//唯一标识，可以类比为 MySQL 中的主键，随机生成
    "_version": 1,//版本
    "result": "created",//结果，这里的 create 表示创建成功
    "_shards": {//
        "total": 2,//分片 - 总数
        "successful": 1,//分片 - 总数
        "failed": 0//分片 - 总数
    },
    "_seq_no": 0,
    "_primary_term": 1
}
```

上面的数据创建后，由于没有指定数据唯一性标识（ID），默认情况下， ES 服务器会随机生成一个。

如果想要自定义唯一性标识，需要在创建时指定： http://127.0.0.1:9200/shopping/_doc/1，请求体JSON内容为：

{
    "title":"小米手机",
    "category":"小米",
    "images":"http://www.gulixueyuan.com/xm.jpg",
    "price":3999.00
}

返回结果如下：

```json
{
    "_index": "shopping",
    "_type": "_doc",
    "_id": "1",//<------------------自定义唯一性标识
    "_version": 1,
    "result": "created",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 1,
    "_primary_term": 1
}
```


此处需要注意：如果增加数据时明确数据主键，那么请求方式也可以为 PUT。






## -入门-HTTP-查询-主键查询 & 全查询

查看文档时，需要指明文档的唯一性标识，类似于 MySQL 中数据的主键查询

在 Postman 中，向 ES 服务器发 GET 请求 ： http://127.0.0.1:9200/shopping/_doc/1 。

返回结果如下：

```json
{
    "_index": "shopping",
    "_type": "_doc",
    "_id": "1",
    "_version": 1,
    "_seq_no": 1,
    "_primary_term": 1,
    "found": true,
    "_source": {
        "title": "小米手机",
        "category": "小米",
        "images": "http://www.gulixueyuan.com/xm.jpg",
        "price": 3999
    }
}
```


查找不存在的内容，向 ES 服务器发 GET 请求 ： http://127.0.0.1:9200/shopping/_doc/1001。

返回结果如下：

```json
{
    "_index": "shopping",
    "_type": "_doc",
    "_id": "1001",
    "found": false
}

```

查看索引下所有数据，向 ES 服务器发 GET 请求 ： http://127.0.0.1:9200/shopping/_search。

返回结果如下：

```json
{
    "took": 133,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 2,
            "relation": "eq"
        },
        "max_score": 1,
        "hits": [
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "ANQqsHgBaKNfVnMbhZYU",
                "_score": 1,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 3999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "1",
                "_score": 1,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 3999
                }
            }
        ]
    }
}

```



## 入门-HTTP-全量修改 & 局部修改 & 删除

全量修改
和新增文档一样，输入相同的 URL 地址请求，如果请求体变化，会将原有的数据内容覆盖

在 Postman 中，向 ES 服务器发 POST 请求 ： http://127.0.0.1:9200/shopping/_doc/1

请求体JSON内容为:

{
    "title":"华为手机",
    "category":"华为",
    "images":"http://www.gulixueyuan.com/hw.jpg",
    "price":1999.00
}
1
2
3
4
5
6
修改成功后，服务器响应结果：

{
    "_index": "shopping",
    "_type": "_doc",
    "_id": "1",
    "_version": 2,
    "result": "updated",//<-----------updated 表示数据被更新
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 2,
    "_primary_term": 1
}

局部修改
修改数据时，也可以只修改某一给条数据的局部信息

在 Postman 中，向 ES 服务器发 POST 请求 ： http://127.0.0.1:9200/shopping/_update/1。

请求体JSON内容为:

```
{
	"doc": {
		"title":"小米手机",
		"category":"小米"
	}
}
：
```





```
{
    "_index": "shopping",
    "_type": "_doc",
    "_id": "1",
    "_version": 3,
    "result": "updated",//<-----------updated 表示数据被更新
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 3,
    "_primary_term": 1
}

```

在 Postman 中，向 ES 服务器发 GET请求 ： http://127.0.0.1:9200/shopping/_doc/1，查看修改内容：

```
{
    "_index": "shopping",
    "_type": "_doc",
    "_id": "1",
    "_version": 3,
    "_seq_no": 3,
    "_primary_term": 1,
    "found": true,
    "_source": {
        "title": "小米手机",
        "category": "小米",
        "images": "http://www.gulixueyuan.com/hw.jpg",
        "price": 1999
    }
}




```

## 删除

删除一个文档不会立即从磁盘上移除，它只是被标记成已删除（逻辑删除）。

在 Postman 中，向 ES 服务器发 DELETE 请求 ： http://127.0.0.1:9200/shopping/_doc/1

返回结果：

```json
{
    "_index": "shopping",
    "_type": "_doc",
    "_id": "1",
    "_version": 4,
    "result": "deleted",//<---删除成功
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 4,
    "_primary_term": 1
}

在 Postman 中，向 ES 服务器发 GET请求 ： http://127.0.0.1:9200/shopping/_doc/1，查看是否删除成功：

{
    "_index": "shopping",
    "_type": "_doc",
    "_id": "1",
    "found": false
}

```







## 13-入门-HTTP-条件查询 & 分页查询 & 查询排序

### 条件查询

假设有以下文档内容，（在 Postman 中，向 ES 服务器发 GET请求 ： http://127.0.0.1:9200/shopping/_search）：

```json
{
    "took": 5,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 6,
            "relation": "eq"
        },
        "max_score": 1,
        "hits": [
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "ANQqsHgBaKNfVnMbhZYU",
                "_score": 1,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 3999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "A9R5sHgBaKNfVnMb25Ya",
                "_score": 1,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "BNR5sHgBaKNfVnMb7pal",
                "_score": 1,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "BtR6sHgBaKNfVnMbX5Y5",
                "_score": 1,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "B9R6sHgBaKNfVnMbZpZ6",
                "_score": 1,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "CdR7sHgBaKNfVnMbsJb9",
                "_score": 1,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            }
        ]
    }
}

```

### URL带参查询

查找category为小米的文档，在 Postman 中，向 ES 服务器发 GET请求 ： http://127.0.0.1:9200/shopping/_search?q=category:小米，返回结果如下：

```json
{
    "took": 94,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 3,
            "relation": "eq"
        },
        "max_score": 1.3862942,
        "hits": [
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "ANQqsHgBaKNfVnMbhZYU",
                "_score": 1.3862942,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 3999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "A9R5sHgBaKNfVnMb25Ya",
                "_score": 1.3862942,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "BNR5sHgBaKNfVnMb7pal",
                "_score": 1.3862942,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            }
        ]
    }
}

```

上述为URL带参数形式查询，这很容易让不善者心怀恶意，或者参数值出现中文会出现乱码情况。为了避免这些情况，我们可用使用带JSON请求体请求进行查询。

### 请求体带参查询

接下带JSON请求体，还是查找category为小米的文档，在 Postman 中，向 ES 服务器发 GET请求 ： http://127.0.0.1:9200/shopping/_search，附带JSON体如下：

```json
{
	"query":{
		"match":{
			"category":"小米"
		}
	}
}
```


返回结果如下：

```json
{
    "took": 3,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 3,
            "relation": "eq"
        },
        "max_score": 1.3862942,
        "hits": [
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "ANQqsHgBaKNfVnMbhZYU",
                "_score": 1.3862942,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 3999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "A9R5sHgBaKNfVnMb25Ya",
                "_score": 1.3862942,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "BNR5sHgBaKNfVnMb7pal",
                "_score": 1.3862942,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            }
        ]
    }
}
```

### 带请求体方式的查找所有内容

查找所有文档内容，也可以这样，在 Postman 中，向 ES 服务器发 GET请求 ： http://127.0.0.1:9200/shopping/_search，附带JSON体如下：

```json
{
	"query":{
		"match_all":{}
	}
}

```

则返回所有文档内容：

```json
{
    "took": 2,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 6,
            "relation": "eq"
        },
        "max_score": 1,
        "hits": [
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "ANQqsHgBaKNfVnMbhZYU",
                "_score": 1,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 3999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "A9R5sHgBaKNfVnMb25Ya",
                "_score": 1,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "BNR5sHgBaKNfVnMb7pal",
                "_score": 1,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "BtR6sHgBaKNfVnMbX5Y5",
                "_score": 1,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "B9R6sHgBaKNfVnMbZpZ6",
                "_score": 1,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "CdR7sHgBaKNfVnMbsJb9",
                "_score": 1,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            }
        ]
    }
}
```



### 查询指定字段

如果你想查询指定字段，在 Postman 中，向 ES 服务器发 GET请求 ： http://127.0.0.1:9200/shopping/_search，附带JSON体如下：

```json
{
	"query":{
		"match_all":{}
	},
	"_source":["title"]
}

```

返回结果如下：

```json
{
    "took": 5,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 6,
            "relation": "eq"
        },
        "max_score": 1,
        "hits": [
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "ANQqsHgBaKNfVnMbhZYU",
                "_score": 1,
                "_source": {
                    "title": "小米手机"
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "A9R5sHgBaKNfVnMb25Ya",
                "_score": 1,
                "_source": {
                    "title": "小米手机"
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "BNR5sHgBaKNfVnMb7pal",
                "_score": 1,
                "_source": {
                    "title": "小米手机"
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "BtR6sHgBaKNfVnMbX5Y5",
                "_score": 1,
                "_source": {
                    "title": "华为手机"
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "B9R6sHgBaKNfVnMbZpZ6",
                "_score": 1,
                "_source": {
                    "title": "华为手机"
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "CdR7sHgBaKNfVnMbsJb9",
                "_score": 1,
                "_source": {
                    "title": "华为手机"
                }
            }
        ]
    }
}

```

### 分页查询

在 Postman 中，向 ES 服务器发 GET请求 ： http://127.0.0.1:9200/shopping/_search，附带JSON体如下：

```json
{
	"query":{
		"match_all":{}
	},
	"from":0,
	"size":2
}

```

返回结果如下：

```json
{
    "took": 1,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 6,
            "relation": "eq"
        },
        "max_score": 1,
        "hits": [
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "ANQqsHgBaKNfVnMbhZYU",
                "_score": 1,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 3999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "A9R5sHgBaKNfVnMb25Ya",
                "_score": 1,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            }
        ]
    }
}

```

### 查询排序

如果你想通过排序查出价格最高的手机，在 Postman 中，向 ES 服务器发 GET请求 ： http://127.0.0.1:9200/shopping/_search，附带JSON体如下：

```
{
	"query":{
		"match_all":{}
	},
	"sort":{
		"price":{
			"order":"desc"
		}
	}
}

```

返回结果如下：

```json
{
    "took": 96,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 6,
            "relation": "eq"
        },
        "max_score": null,
        "hits": [
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "ANQqsHgBaKNfVnMbhZYU",
                "_score": null,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 3999
                },
                "sort": [
                    3999
                ]
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "A9R5sHgBaKNfVnMb25Ya",
                "_score": null,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                },
                "sort": [
                    1999
                ]
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "BNR5sHgBaKNfVnMb7pal",
                "_score": null,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                },
                "sort": [
                    1999
                ]
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "BtR6sHgBaKNfVnMbX5Y5",
                "_score": null,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                },
                "sort": [
                    1999
                ]
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "B9R6sHgBaKNfVnMbZpZ6",
                "_score": null,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                },
                "sort": [
                    1999
                ]
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "CdR7sHgBaKNfVnMbsJb9",
                "_score": null,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                },
                "sort": [
                    1999
                ]
            }
        ]
    }
}

```







## 入门-HTTP-多条件查询 & 范围查询

### 多条件查询

假设想找出小米牌子，价格为3999元的。（must相当于数据库的&&）

在 Postman 中，向 ES 服务器发 GET请求 ： http://127.0.0.1:9200/shopping/_search，附带JSON体如下：

```json
{
	"query":{
		"bool":{
			"must":[{
				"match":{
					"category":"小米"
				}
			},{
				"match":{
					"price":3999.00
				}
			}]
		}
	}
}
```


返回结果如下：

```json
{
    "took": 134,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 1,
            "relation": "eq"
        },
        "max_score": 2.3862944,
        "hits": [
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "ANQqsHgBaKNfVnMbhZYU",
                "_score": 2.3862944,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 3999
                }
            }
        ]
    }
}

```

假设想找出小米和华为的牌子。（should相当于数据库的||）

在 Postman 中，向 ES 服务器发 GET请求 ： http://127.0.0.1:9200/shopping/_search，附带JSON体如下：

```json
{
	"query":{
		"bool":{
			"should":[{
				"match":{
					"category":"小米"
				}
			},{
				"match":{
					"category":"华为"
				}
			}]
		},
        "filter":{
            "range":{
                "price":{
                    "gt":2000
                }
            }
        }
	}
}

```

返回结果如下：

```json
{
    "took": 8,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 6,
            "relation": "eq"
        },
        "max_score": 1.3862942,
        "hits": [
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "ANQqsHgBaKNfVnMbhZYU",
                "_score": 1.3862942,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 3999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "A9R5sHgBaKNfVnMb25Ya",
                "_score": 1.3862942,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "BNR5sHgBaKNfVnMb7pal",
                "_score": 1.3862942,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "BtR6sHgBaKNfVnMbX5Y5",
                "_score": 1.3862942,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "B9R6sHgBaKNfVnMbZpZ6",
                "_score": 1.3862942,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "CdR7sHgBaKNfVnMbsJb9",
                "_score": 1.3862942,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            }
        ]
    }
}
```

### 范围查询

假设想找出小米和华为的牌子，价格大于2000元的手机。

在 Postman 中，向 ES 服务器发 GET请求 ： http://127.0.0.1:9200/shopping/_search，附带JSON体如下：

```json
{
	"query":{
		"bool":{
			"should":[{
				"match":{
					"category":"小米"
				}
			},{
				"match":{
					"category":"华为"
				}
			}],
            "filter":{
            	"range":{
                	"price":{
                    	"gt":2000
                	}
	            }
    	    }
		}
	}
}
```

返回结果如下：

```json
{
    "took": 72,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 1,
            "relation": "eq"
        },
        "max_score": 1.3862942,
        "hits": [
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "ANQqsHgBaKNfVnMbhZYU",
                "_score": 1.3862942,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 3999
                }
            }
        ]
    }
}

```





## 15-入门-HTTP-全文检索 & 完全匹配 & 高亮查询

### 全文检索

这功能像搜索引擎那样，如品牌输入“小华”，返回结果带回品牌有“小米”和华为的。

在 Postman 中，向 ES 服务器发 GET请求 ： http://127.0.0.1:9200/shopping/_search，附带JSON体如下：

```json
{
	"query":{
		"match":{
			"category" : "小华"
		}
	}
}
```


返回结果如下：

```json
{
    "took": 7,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 6,
            "relation": "eq"
        },
        "max_score": 0.6931471,
        "hits": [
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "ANQqsHgBaKNfVnMbhZYU",
                "_score": 0.6931471,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 3999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "A9R5sHgBaKNfVnMb25Ya",
                "_score": 0.6931471,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "BNR5sHgBaKNfVnMb7pal",
                "_score": 0.6931471,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "BtR6sHgBaKNfVnMbX5Y5",
                "_score": 0.6931471,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "B9R6sHgBaKNfVnMbZpZ6",
                "_score": 0.6931471,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "CdR7sHgBaKNfVnMbsJb9",
                "_score": 0.6931471,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            }
        ]
    }
}
```



### 完全匹配

在 Postman 中，向 ES 服务器发 GET请求 ： http://127.0.0.1:9200/shopping/_search，附带JSON体如下：

```json
{
	"query":{
		"match_phrase":{
			"category" : "为"
		}
	}
}

```

返回结果如下：

```json
{
    "took": 2,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 3,
            "relation": "eq"
        },
        "max_score": 0.6931471,
        "hits": [
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "BtR6sHgBaKNfVnMbX5Y5",
                "_score": 0.6931471,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "B9R6sHgBaKNfVnMbZpZ6",
                "_score": 0.6931471,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "CdR7sHgBaKNfVnMbsJb9",
                "_score": 0.6931471,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            }
        ]
    }
}

```

### 高亮查询

在 Postman 中，向 ES 服务器发 GET请求 ： http://127.0.0.1:9200/shopping/_search，附带JSON体如下：

```json
{
	"query":{
		"match_phrase":{
			"category" : "为"
		}
	},
    "highlight":{
        "fields":{
            "category":{}//<----高亮这字段
        }
    }
}
```

返回结果如下：

```json
{
    "took": 100,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 3,
            "relation": "eq"
        },
        "max_score": 0.6931471,
        "hits": [
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "BtR6sHgBaKNfVnMbX5Y5",
                "_score": 0.6931471,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                },
                "highlight": {
                    "category": [
                        "华<em>为</em>"//<------高亮一个为字。
                    ]
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "B9R6sHgBaKNfVnMbZpZ6",
                "_score": 0.6931471,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                },
                "highlight": {
                    "category": [
                        "华<em>为</em>"
                    ]
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "CdR7sHgBaKNfVnMbsJb9",
                "_score": 0.6931471,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                },
                "highlight": {
                    "category": [
                        "华<em>为</em>"
                    ]
                }
            }
        ]
    }
}




```



## 16-入门-HTTP-聚合查询

聚合允许使用者对 es 文档进行统计分析，类似与关系型数据库中的 group by，当然还有很多其他的聚合，例如取最大值max、平均值avg等等。

接下来按price字段进行分组：

在 Postman 中，向 ES 服务器发 GET请求 ： http://127.0.0.1:9200/shopping/_search，附带JSON体如下：

```json
{
	"aggs":{//聚合操作
		"price_group":{//名称，随意起名
			"terms":{//分组
				"field":"price"//分组字段
			}
		}
	}
}
```


返回结果如下：

```json
{
    "took": 63,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 6,
            "relation": "eq"
        },
        "max_score": 1,
        "hits": [
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "ANQqsHgBaKNfVnMbhZYU",
                "_score": 1,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 3999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "A9R5sHgBaKNfVnMb25Ya",
                "_score": 1,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "BNR5sHgBaKNfVnMb7pal",
                "_score": 1,
                "_source": {
                    "title": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "BtR6sHgBaKNfVnMbX5Y5",
                "_score": 1,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "B9R6sHgBaKNfVnMbZpZ6",
                "_score": 1,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "CdR7sHgBaKNfVnMbsJb9",
                "_score": 1,
                "_source": {
                    "title": "华为手机",
                    "category": "华为",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 1999
                }
            }
        ]
    },
    "aggregations": {
        "price_group": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 0,
            "buckets": [
                {
                    "key": 1999,
                    "doc_count": 5
                },
                {
                    "key": 3999,
                    "doc_count": 1
                }
            ]
        }
    }
}

```

上面返回结果会附带原始数据的。若不想要不附带原始数据的结果，在 Postman 中，向 ES 服务器发 GET请求 ： http://127.0.0.1:9200/shopping/_search，附带JSON体如下：

```json
{
	"aggs":{
		"price_group":{
			"terms":{
				"field":"price"
			}
		}
	},
    "size":0
}

```

返回结果如下：

```json
{
    "took": 60,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 6,
            "relation": "eq"
        },
        "max_score": null,
        "hits": []
    },
    "aggregations": {
        "price_group": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 0,
            "buckets": [
                {
                    "key": 1999,
                    "doc_count": 5
                },
                {
                    "key": 3999,
                    "doc_count": 1
                }
            ]
        }
    }
}
```


若想对所有手机价格求平均值。

在 Postman 中，向 ES 服务器发 GET请求 ： http://127.0.0.1:9200/shopping/_search，附带JSON体如下：

```json
{
	"aggs":{
		"price_avg":{//名称，随意起名
			"avg":{//求平均
				"field":"price"
			}
		}
	},
    "size":0
}
```


返回结果如下：

```json
{
    "took": 14,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 6,
            "relation": "eq"
        },
        "max_score": null,
        "hits": []
    },
    "aggregations": {
        "price_avg": {
            "value": 2332.3333333333335
        }
    }


```







## 入门-HTTP-映射关系

有了索引库，等于有了数据库中的 database。

接下来就需要建索引库(index)中的映射了，类似于数据库(database)中的表结构(table)。

创建数据库表需要设置字段名称，类型，长度，约束等；索引库也一样，需要知道这个类型下有哪些字段，每个字段有哪些约束信息，这就叫做映射(mapping)。

先创建一个索引：

PUT http://127.0.0.1:9200/user


返回结果：

```json
{
    "acknowledged": true,
    "shards_acknowledged": true,
    "index": "user"
}

```

### 创建映射

PUT http://127.0.0.1:9200/user/_mapping

```json
{
    "properties": {
        "name":{
        	"type": "text",
        	"index": true
        },
        "sex":{
        	"type": "keyword",
        	"index": true
        },
        "tel":{
        	"type": "keyword",
        	"index": false
        }
    }
}

```

返回结果如下：

```json
{
    "acknowledged": true
}

```

### 查询映射

#GET http://127.0.0.1:9200/user/_mapping

返回结果如下：

```json
{
    "user": {
        "mappings": {
            "properties": {
                "name": {
                    "type": "text"
                },
                "sex": {
                    "type": "keyword"
                },
                "tel": {
                    "type": "keyword",
                    "index": false
                }
            }
        }
    }
}

```

### 增加数据

#PUT http://127.0.0.1:9200/user/_create/1001

```json
{
	"name":"小米",
	"sex":"男的",
	"tel":"1111"
}
```


返回结果如下：

```json
{
    "_index": "user",
    "_type": "_doc",
    "_id": "1001",
    "_version": 1,
    "result": "created",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 0,
    "_primary_term": 1
}
```


查找name含有”小“数据：

#GET http://127.0.0.1:9200/user/_search

```json
{
	"query":{
		"match":{
			"name":"小"
		}
	}
}

```

返回结果如下：

```json
{
    "took": 495,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 1,
            "relation": "eq"
        },
        "max_score": 0.2876821,
        "hits": [
            {
                "_index": "user",
                "_type": "_doc",
                "_id": "1001",
                "_score": 0.2876821,
                "_source": {
                    "name": "小米",
                    "sex": "男的",
                    "tel": "1111"
                }
            }
        ]
    }
}

```

查找sex含有”男“数据：

#GET http://127.0.0.1:9200/user/_search

```json
{
	"query":{
		"match":{
			"sex":"男"
		}
	}
}
```


返回结果如下：

```json
{
    "took": 1,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 0,
            "relation": "eq"
        },
        "max_score": null,
        "hits": []
    }
}
```


找不想要的结果，只因创建映射时"sex"的类型为"keyword"。

"sex"只能完全为”男的“，才能得出原数据。

#GET http://127.0.0.1:9200/user/_search

```json
{
	"query":{
		"match":{
			"sex":"男的"
		}
	}
}

```

返回结果如下：

```json
{
    "took": 2,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 1,
            "relation": "eq"
        },
        "max_score": 0.2876821,
        "hits": [
            {
                "_index": "user",
                "_type": "_doc",
                "_id": "1001",
                "_score": 0.2876821,
                "_source": {
                    "name": "小米",
                    "sex": "男的",
                    "tel": "1111"
                }
            }
        ]
    }
}
```

### 查询电话_

_GET http://127.0.0.1:9200/user/_search

```json
{
	"query":{
		"match":{
			"tel":"11"
		}
	}
}
```


返回结果如下：

```json
{
    "error": {
        "root_cause": [
            {
                "type": "query_shard_exception",
                "reason": "failed to create query: Cannot search on field [tel] since it is not indexed.",
                "index_uuid": "ivLnMfQKROS7Skb2MTFOew",
                "index": "user"
            }
        ],
        "type": "search_phase_execution_exception",
        "reason": "all shards failed",
        "phase": "query",
        "grouped": true,
        "failed_shards": [
            {
                "shard": 0,
                "index": "user",
                "node": "4P7dIRfXSbezE5JTiuylew",
                "reason": {
                    "type": "query_shard_exception",
                    "reason": "failed to create query: Cannot search on field [tel] since it is not indexed.",
                    "index_uuid": "ivLnMfQKROS7Skb2MTFOew",
                    "index": "user",
                    "caused_by": {
                        "type": "illegal_argument_exception",
                        "reason": "Cannot search on field [tel] since it is not indexed."
                    }
                }
            }
        ]
    },
    "status": 400
}

```

报错只因创建映射时"tel"的"index"为false。


8-入门-JavaAPI-环境准备
新建Maven工程。

添加依赖：

```json
<dependencies>
    <dependency>
        <groupId>org.elasticsearch</groupId>
        <artifactId>elasticsearch</artifactId>
        <version>7.8.0</version>
    </dependency>
    <!-- elasticsearch 的客户端 -->
    <dependency>
        <groupId>org.elasticsearch.client</groupId>
        <artifactId>elasticsearch-rest-high-level-client</artifactId>
        <version>7.8.0</version>
    </dependency>
    <!-- elasticsearch 依赖 2.x 的 log4j -->
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-api</artifactId>
        <version>2.8.2</version>
    </dependency>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.8.2</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.9.9</version>
    </dependency>
    <!-- junit 单元测试 -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
</dependencies>
```



```json
HelloElasticsearch

import java.io.IOException;

import org.apache.http.HttpHost;
import org.elasticsearch.client.RestClient;
import org.elasticsearch.client.RestHighLevelClient;

public class HelloElasticsearch {

public static void main(String[] args) throws IOException {
	// 创建客户端对象
	RestHighLevelClient client = new RestHighLevelClient(
			RestClient.builder(new HttpHost("localhost", 9200, "http")));



		System.out.println(client);

// 关闭客户端连接
client.close();

}

}

```













## 18-入门-JavaAPI-环境准备

新建Maven工程。

添加依赖：

```xml
<dependencies>
    <dependency>
        <groupId>org.elasticsearch</groupId>
        <artifactId>elasticsearch</artifactId>
        <version>7.8.0</version>
    </dependency>
    <!-- elasticsearch 的客户端 -->
    <dependency>
        <groupId>org.elasticsearch.client</groupId>
        <artifactId>elasticsearch-rest-high-level-client</artifactId>
        <version>7.8.0</version>
    </dependency>
    <!-- elasticsearch 依赖 2.x 的 log4j -->
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-api</artifactId>
        <version>2.8.2</version>
    </dependency>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.8.2</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.9.9</version>
    </dependency>
    <!-- junit 单元测试 -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
</dependencies>

```



### 初始化



```java
import java.io.IOException;

import org.apache.http.HttpHost;
import org.elasticsearch.client.RestClient;
import org.elasticsearch.client.RestHighLevelClient;

public class HelloElasticsearch {


public static void main(String[] args) throws IOException {
	// 创建客户端对象
	RestHighLevelClient client = new RestHighLevelClient(
			RestClient.builder(new HttpHost("localhost", 9200, "http")));
//		...
		System.out.println(client);
	// 关闭客户端连接
	client.close();
}
}
```


### 19-入门-JavaAPI-索引-创建

```java
import org.apache.http.HttpHost;
import org.elasticsearch.action.admin.indices.create.CreateIndexRequest;
import org.elasticsearch.action.admin.indices.create.CreateIndexResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.client.RestClient;
import org.elasticsearch.client.RestHighLevelClient;

import java.io.IOException;

public class CreateIndex {
public static void main(String[] args) throws IOException {
    // 创建客户端对象
    RestHighLevelClient client = new RestHighLevelClient(
            RestClient.builder(new HttpHost("localhost", 9200, "http")));

    // 创建索引 - 请求对象
    CreateIndexRequest request = new CreateIndexRequest("user2");
    // 发送请求，获取响应
    CreateIndexResponse response = client.indices().create(request,
            RequestOptions.DEFAULT);
    boolean acknowledged = response.isAcknowledged();
    // 响应状态
    System.out.println("操作状态 = " + acknowledged);

    // 关闭客户端连接
    client.close();
}
```

}

后台打印：

```java
四月 09, 2021 2:12:08 下午 org.elasticsearch.client.RestClient logResponse
警告: request [PUT http://localhost:9200/user2?master_timeout=30s&include_type_name=true&timeout=30s] returned 1 warnings: [299 Elasticsearch-7.8.0-757314695644ea9a1dc2fecd26d1a43856725e65 "[types removal] Using include_type_name in create index requests is deprecated. The parameter will be removed in the next major version."]
操作状态 = true

Process finished with exit code 0
```



### 20-入门-JavaAPI-索引-查询 & 删除

查询

```java
import org.apache.http.HttpHost;

import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.client.RestClient;
import org.elasticsearch.client.RestHighLevelClient;
import org.elasticsearch.client.indices.GetIndexRequest;
import org.elasticsearch.client.indices.GetIndexResponse;

import java.io.IOException;

public class SearchIndex {
    public static void main(String[] args) throws IOException {
        // 创建客户端对象
        RestHighLevelClient client = new RestHighLevelClient(
                RestClient.builder(new HttpHost("localhost", 9200, "http")));
// 查询索引 - 请求对象
    GetIndexRequest request = new GetIndexRequest("user2");
    // 发送请求，获取响应
    GetIndexResponse response = client.indices().get(request,
            RequestOptions.DEFAULT);
    
    System.out.println("aliases:"+response.getAliases());
    System.out.println("mappings:"+response.getMappings());
    System.out.println("settings:"+response.getSettings());

    client.close();
}
}
```


后台打印：

```java
aliases:{user2=[]}
mappings:{user2=org.elasticsearch.cluster.metadata.MappingMetadata@ad700514}
settings:{user2={"index.creation_date":"1617948726976","index.number_of_replicas":"1","index.number_of_shards":"1","index.provided_name":"user2","index.uuid":"UGZ1ntcySnK6hWyP2qoVpQ","index.version.created":"7080099"}}

Process finished with exit code 0
```



### 删除

删除

```java
import org.apache.http.HttpHost;
import org.elasticsearch.action.admin.indices.delete.DeleteIndexRequest;
import org.elasticsearch.action.support.master.AcknowledgedResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.client.RestClient;
import org.elasticsearch.client.RestHighLevelClient;

import java.io.IOException;

public class DeleteIndex {
    public static void main(String[] args) throws IOException {
        RestHighLevelClient client = new RestHighLevelClient(
                RestClient.builder(new HttpHost("localhost", 9200, "http")));
        // 删除索引 - 请求对象
        DeleteIndexRequest request = new DeleteIndexRequest("user2");
        // 发送请求，获取响应
        AcknowledgedResponse response = client.indices().delete(request,RequestOptions.DEFAULT);
        // 操作结果
        System.out.println("操作结果 ： " + response.isAcknowledged());
        client.close();
    }
}
```


后台打印：

后台打印：

操作结果 ： true

Process finished with exit code 0







## 21-入门-JavaAPI-文档-新增 & 修改

重构
上文由于频繁使用以下连接Elasticsearch和关闭它的代码，于是个人对它进行重构。

```java
public class SomeClass {
    public static void main(String[] args) throws IOException {
        RestHighLevelClient client = new RestHighLevelClient(
                RestClient.builder(new HttpHost("localhost", 9200, "http")));
		

        ...
        
        client.close();
    }

}
```

重构后的代码：



```java
import org.elasticsearch.client.RestHighLevelClient;

public interface ElasticsearchTask {
void doSomething(RestHighLevelClient client) throws Exception;}
```





```java
public class ConnectElasticsearch{
public static void connect(ElasticsearchTask task){
    // 创建客户端对象
    RestHighLevelClient client = new RestHighLevelClient(
            RestClient.builder(new HttpHost("localhost", 9200, "http")));
    try {
        task.doSomething(client);
        // 关闭客户端连接
        client.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

}

接下来，如果想让Elasticsearch完成一些操作，就编写一个lambda式即可。



```java
public class SomeClass {
public static void main(String[] args) {
    ConnectElasticsearch.connect(client -> {
		//do something
    });
}
}


```


### 新增

```java
import com.fasterxml.jackson.databind.ObjectMapper;
import com.lun.elasticsearch.hello.ConnectElasticsearch;
import com.lun.elasticsearch.model.User;
import org.elasticsearch.action.index.IndexRequest;
import org.elasticsearch.action.index.IndexResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.common.xcontent.XContentType;

public class InsertDoc {
public static void main(String[] args) {
    ConnectElasticsearch.connect(client -> {
        // 新增文档 - 请求对象
        IndexRequest request = new IndexRequest();
        // 设置索引及唯一性标识
        request.index("user").id("1001");

        // 创建数据对象
        User user = new User();
        user.setName("zhangsan");
        user.setAge(30);
        user.setSex("男");

        ObjectMapper objectMapper = new ObjectMapper();
        String productJson = objectMapper.writeValueAsString(user);
        // 添加文档数据，数据格式为 JSON 格式
        request.source(productJson, XContentType.JSON);
        // 客户端发送请求，获取响应对象
        IndexResponse response = client.index(request, RequestOptions.DEFAULT);
        3.打印结果信息
        System.out.println("_index:" + response.getIndex());
        System.out.println("_id:" + response.getId());
        System.out.println("_result:" + response.getResult());
    });
}
}
```


```java
后台打印：

_index:user
_id:1001
_result:UPDATED

Process finished with exit code 0
```



### 修改



```java
import com.lun.elasticsearch.hello.ConnectElasticsearch;
import org.elasticsearch.action.update.UpdateRequest;
import org.elasticsearch.action.update.UpdateResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.common.xcontent.XContentType;

public class UpdateDoc {
public static void main(String[] args) {
    ConnectElasticsearch.connect(client -> {
        // 修改文档 - 请求对象
        UpdateRequest request = new UpdateRequest();
        // 配置修改参数
        request.index("user").id("1001");
        // 设置请求体，对数据进行修改
        request.doc(XContentType.JSON, "sex", "女");
        // 客户端发送请求，获取响应对象
        UpdateResponse response = client.update(request, RequestOptions.DEFAULT);
        System.out.println("_index:" + response.getIndex());
        System.out.println("_id:" + response.getId());
        System.out.println("_result:" + response.getResult());
    });
}}
```



后台打印：

```java
_index:user
_id:1001
_result:UPDATED

Process finished with exit code 0

```



## 22-入门-JavaAPI-文档-查询 & 删除

### 查询

```java
import com.lun.elasticsearch.hello.ConnectElasticsearch;
import org.elasticsearch.action.get.GetRequest;
import org.elasticsearch.action.get.GetResponse;
import org.elasticsearch.client.RequestOptions;

public class GetDoc {
public static void main(String[] args) {
    ConnectElasticsearch.connect(client -> {
        //1.创建请求对象
        GetRequest request = new GetRequest().index("user").id("1001");
        //2.客户端发送请求，获取响应对象
        GetResponse response = client.get(request, RequestOptions.DEFAULT);
        3.打印结果信息
        System.out.println("_index:" + response.getIndex());
        System.out.println("_type:" + response.getType());
        System.out.println("_id:" + response.getId());
        System.out.println("source:" + response.getSourceAsString());
    });
}
}


```

后台打印：

```java
index:user
_type:_doc
_id:1001
source:{"name":"zhangsan","age":30,"sex":"男"}

Process finished with exit code 0
```



### 删除

```java
import com.lun.elasticsearch.hello.ConnectElasticsearch;
import org.elasticsearch.action.delete.DeleteRequest;
import org.elasticsearch.action.delete.DeleteResponse;
import org.elasticsearch.client.RequestOptions;

public class DeleteDoc {
    public static void main(String[] args) {
        ConnectElasticsearch.connect(client -> {
            //创建请求对象
            DeleteRequest request = new DeleteRequest().index("user").id("1001");
            //客户端发送请求，获取响应对象
            DeleteResponse response = client.delete(request, RequestOptions.DEFAULT);
            //打印信息
            System.out.println(response.toString());
        });
    }
}
```

后台打印：

```java
DeleteResponse[index=user,type=_doc,id=1001,version=16,result=deleted,shards=ShardInfo{total=2, successful=1, failures=[]}]

Process finished with exit code 0


```







## 23-入门-JavaAPI-文档-批量新增 & 批量删除

### 批量新增

```java
import com.lun.elasticsearch.hello.ConnectElasticsearch;
import org.elasticsearch.action.bulk.BulkRequest;
import org.elasticsearch.action.bulk.BulkResponse;
import org.elasticsearch.action.index.IndexRequest;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.common.xcontent.XContentType;

public class BatchInsertDoc {

    public static void main(String[] args) {
        ConnectElasticsearch.connect(client -> {
            //创建批量新增请求对象
            BulkRequest request = new BulkRequest();
            request.add(new
                    IndexRequest().index("user").id("1001").source(XContentType.JSON, "name",
                    "zhangsan"));
            request.add(new
                    IndexRequest().index("user").id("1002").source(XContentType.JSON, "name",
                            "lisi"));
            request.add(new
                    IndexRequest().index("user").id("1003").source(XContentType.JSON, "name",
                    "wangwu"));
            //客户端发送请求，获取响应对象
            BulkResponse responses = client.bulk(request, RequestOptions.DEFAULT);
            //打印结果信息
            System.out.println("took:" + responses.getTook());
            System.out.println("items:" + responses.getItems());
        });
    }

}
```


后台打印

```java
took:294ms
items:[Lorg.elasticsearch.action.bulk.BulkItemResponse;@2beee7ff

Process finished with exit code 0
```

### 批量删除

```java
import com.lun.elasticsearch.hello.ConnectElasticsearch;
import org.elasticsearch.action.bulk.BulkRequest;
import org.elasticsearch.action.bulk.BulkResponse;
import org.elasticsearch.action.delete.DeleteRequest;
import org.elasticsearch.client.RequestOptions;

public class BatchDeleteDoc {
    public static void main(String[] args) {
        ConnectElasticsearch.connect(client -> {
            //创建批量删除请求对象
            BulkRequest request = new BulkRequest();
            request.add(new DeleteRequest().index("user").id("1001"));
            request.add(new DeleteRequest().index("user").id("1002"));
            request.add(new DeleteRequest().index("user").id("1003"));
            //客户端发送请求，获取响应对象
            BulkResponse responses = client.bulk(request, RequestOptions.DEFAULT);
            //打印结果信息
            System.out.println("took:" + responses.getTook());
            System.out.println("items:" + responses.getItems());
        });
    }
}
```

后台打印

```java
took:108ms
items:[Lorg.elasticsearch.action.bulk.BulkItemResponse;@7b02881e

Process finished with exit code 0
```






## 入门-JavaAPI-文档-高级查询-全量查询

先批量增加数据

```java
public class BatchInsertDoc {

    public static void main(String[] args) {
        ConnectElasticsearch.connect(client -> {
            //创建批量新增请求对象
            BulkRequest request = new BulkRequest();
            request.add(new IndexRequest().index("user").id("1001").source(XContentType.JSON, "name", "zhangsan", "age", "10", "sex","女"));
            request.add(new IndexRequest().index("user").id("1002").source(XContentType.JSON, "name", "lisi", "age", "30", "sex","女"));
            request.add(new IndexRequest().index("user").id("1003").source(XContentType.JSON, "name", "wangwu1", "age", "40", "sex","男"));
            request.add(new IndexRequest().index("user").id("1004").source(XContentType.JSON, "name", "wangwu2", "age", "20", "sex","女"));
            request.add(new IndexRequest().index("user").id("1005").source(XContentType.JSON, "name", "wangwu3", "age", "50", "sex","男"));
            request.add(new IndexRequest().index("user").id("1006").source(XContentType.JSON, "name", "wangwu4", "age", "20", "sex","男"));
            //客户端发送请求，获取响应对象
            BulkResponse responses = client.bulk(request, RequestOptions.DEFAULT);
            //打印结果信息
            System.out.println("took:" + responses.getTook());
            System.out.println("items:" + responses.getItems());
        });
    }

}
```

后台打印

took:168ms
items:[Lorg.elasticsearch.action.bulk.BulkItemResponse;@2beee7ff

Process finished with exit code 0

查询所有索引数据

```java
import com.lun.elasticsearch.hello.ConnectElasticsearch;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.SearchHits;
import org.elasticsearch.search.builder.SearchSourceBuilder;

public class QueryDoc {

    public static void main(String[] args) {
        ConnectElasticsearch.connect(client -> {
            // 创建搜索请求对象
            SearchRequest request = new SearchRequest();
            request.indices("user");
            // 构建查询的请求体
            SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
            // 查询所有数据
            sourceBuilder.query(QueryBuilders.matchAllQuery());
            request.source(sourceBuilder);
            SearchResponse response = client.search(request, RequestOptions.DEFAULT);
            // 查询匹配
            SearchHits hits = response.getHits();
            System.out.println("took:" + response.getTook());
            System.out.println("timeout:" + response.isTimedOut());
            System.out.println("total:" + hits.getTotalHits());
            System.out.println("MaxScore:" + hits.getMaxScore());
            System.out.println("hits========>>");
            for (SearchHit hit : hits) {
            //输出每条查询的结果信息
                System.out.println(hit.getSourceAsString());
            }
            System.out.println("<<========");
        });
    }

}
```

后台打印

```
took:2ms
timeout:false
total:6 hits
MaxScore:1.0
hits========>>
{"name":"zhangsan","age":"10","sex":"女"}
{"name":"lisi","age":"30","sex":"女"}
{"name":"wangwu1","age":"40","sex":"男"}
{"name":"wangwu2","age":"20","sex":"女"}
{"name":"wangwu3","age":"50","sex":"男"}
{"name":"wangwu4","age":"20","sex":"男"}
<<========

Process finished with exit code 0

```











## 入门-JavaAPI-文档-高级查询-分页查询 & 条件查询 & 查询排序

### 条件查询

```java
import com.lun.elasticsearch.hello.ConnectElasticsearch;
import com.lun.elasticsearch.hello.ElasticsearchTask;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.SearchHits;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.elasticsearch.search.sort.SortOrder;

public class QueryDoc {
    

	public static final ElasticsearchTask SEARCH_BY_CONDITION = client -> {
	    // 创建搜索请求对象
	    SearchRequest request = new SearchRequest();
	    request.indices("user");
	    // 构建查询的请求体
	    SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
	    sourceBuilder.query(QueryBuilders.termQuery("age", "30"));
	    request.source(sourceBuilder);
	    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
	    // 查询匹配
	    SearchHits hits = response.getHits();
	    System.out.println("took:" + response.getTook());
	    System.out.println("timeout:" + response.isTimedOut());
	    System.out.println("total:" + hits.getTotalHits());
	    System.out.println("MaxScore:" + hits.getMaxScore());
	    System.out.println("hits========>>");
	    for (SearchHit hit : hits) {
	        //输出每条查询的结果信息
	        System.out.println(hit.getSourceAsString());
	    }
	    System.out.println("<<========");
	};
	
	public static void main(String[] args) {
	    ConnectElasticsearch.connect(SEARCH_BY_CONDITION);
	}

}
```


后台打印

```
took:1ms
timeout:false
total:1 hits
MaxScore:1.0
hits========>>
{"name":"lisi","age":"30","sex":"女"}
<<========

```

### 分页查询

```java
import com.lun.elasticsearch.hello.ConnectElasticsearch;
import com.lun.elasticsearch.hello.ElasticsearchTask;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.SearchHits;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.elasticsearch.search.sort.SortOrder;

public class QueryDoc {
    

	public static final ElasticsearchTask SEARCH_BY_PAGING = client -> {
	    // 创建搜索请求对象
	    SearchRequest request = new SearchRequest();
	    request.indices("user");
	    // 构建查询的请求体
	    SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
	    sourceBuilder.query(QueryBuilders.matchAllQuery());
	    // 分页查询
	    // 当前页其实索引(第一条数据的顺序号)， from
	    sourceBuilder.from(0);
	
	    // 每页显示多少条 size
	    sourceBuilder.size(2);
	    request.source(sourceBuilder);
	    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
	    // 查询匹配
	    SearchHits hits = response.getHits();
	    System.out.println("took:" + response.getTook());
	    System.out.println("timeout:" + response.isTimedOut());
	    System.out.println("total:" + hits.getTotalHits());
	    System.out.println("MaxScore:" + hits.getMaxScore());
	    System.out.println("hits========>>");
	    for (SearchHit hit : hits) {
	        //输出每条查询的结果信息
	        System.out.println(hit.getSourceAsString());
	    }
	    System.out.println("<<========");
	};
	
	public static void main(String[] args) {
	    ConnectElasticsearch.connect(SEARCH_BY_CONDITION);
	}

}
```


后台打印

后台打印

```
took:1ms
timeout:false
total:6 hits
MaxScore:1.0
hits========>>
{"name":"zhangsan","age":"10","sex":"女"}
{"name":"lisi","age":"30","sex":"女"}
<<========
```



### 查询排序

```java
import com.lun.elasticsearch.hello.ConnectElasticsearch;
import com.lun.elasticsearch.hello.ElasticsearchTask;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.SearchHits;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.elasticsearch.search.sort.SortOrder;

public class QueryDoc {
    

	public static final ElasticsearchTask SEARCH_WITH_ORDER = client -> {
	    // 创建搜索请求对象
	    SearchRequest request = new SearchRequest();
	    request.indices("user");
	
	    // 构建查询的请求体
	    SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
	    sourceBuilder.query(QueryBuilders.matchAllQuery());
	    // 排序
	    sourceBuilder.sort("age", SortOrder.ASC);
	    request.source(sourceBuilder);
	    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
	    // 查询匹配
	    SearchHits hits = response.getHits();
	    System.out.println("took:" + response.getTook());
	    System.out.println("timeout:" + response.isTimedOut());
	    System.out.println("total:" + hits.getTotalHits());
	    System.out.println("MaxScore:" + hits.getMaxScore());
	    System.out.println("hits========>>");
	    for (SearchHit hit : hits) {
	    //输出每条查询的结果信息
	        System.out.println(hit.getSourceAsString());
	    }
	    System.out.println("<<========");
	};
	
	public static void main(String[] args) {
	    ConnectElasticsearch.connect(SEARCH_WITH_ORDER);
	}

}
```


后台打印

```
took:1ms
timeout:false
total:6 hits
MaxScore:NaN
hits========>>
{"name":"zhangsan","age":"10","sex":"女"}
{"name":"wangwu2","age":"20","sex":"女"}
{"name":"wangwu4","age":"20","sex":"男"}
{"name":"lisi","age":"30","sex":"女"}
{"name":"wangwu1","age":"40","sex":"男"}
{"name":"wangwu3","age":"50","sex":"男"}
<<========

```











## 26-入门-JavaAPI-文档-高级查询-组合查询 & 范围查询

### 组合查询

```java
import com.lun.elasticsearch.hello.ConnectElasticsearch;
import com.lun.elasticsearch.hello.ElasticsearchTask;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.index.query.BoolQueryBuilder;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.SearchHits;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.elasticsearch.search.sort.SortOrder;

public class QueryDoc {
    

	public static final ElasticsearchTask SEARCH_BY_BOOL_CONDITION = client -> {
	    // 创建搜索请求对象
	    SearchRequest request = new SearchRequest();
	    request.indices("user");
	    // 构建查询的请求体
	    SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
	    BoolQueryBuilder boolQueryBuilder = QueryBuilders.boolQuery();
	    // 必须包含
	    boolQueryBuilder.must(QueryBuilders.matchQuery("age", "30"));
	    // 一定不含
	    boolQueryBuilder.mustNot(QueryBuilders.matchQuery("name", "zhangsan"));
	    // 可能包含
	    boolQueryBuilder.should(QueryBuilders.matchQuery("sex", "男"));
	    sourceBuilder.query(boolQueryBuilder);
	    request.source(sourceBuilder);
	    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
	    // 查询匹配
	    SearchHits hits = response.getHits();
	    System.out.println("took:" + response.getTook());
	    System.out.println("timeout:" + response.isTimedOut());
	    System.out.println("total:" + hits.getTotalHits());
	    System.out.println("MaxScore:" + hits.getMaxScore());
	    System.out.println("hits========>>");
	    for (SearchHit hit : hits) {
	        //输出每条查询的结果信息
	        System.out.println(hit.getSourceAsString());
	    }
	    System.out.println("<<========");
	
	};
	
	public static void main(String[] args) {
	    ConnectElasticsearch.connect(SEARCH_BY_BOOL_CONDITION);
	}

}

```



```
后台打印

took:28ms
timeout:false
total:1 hits
MaxScore:1.0
hits========>>
{"name":"lisi","age":"30","sex":"女"}
<<========

Process finished with exit code 0
```



### 范围查询

范围查询

```java
import com.lun.elasticsearch.hello.ConnectElasticsearch;
import com.lun.elasticsearch.hello.ElasticsearchTask;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.index.query.BoolQueryBuilder;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.index.query.RangeQueryBuilder;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.SearchHits;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.elasticsearch.search.sort.SortOrder;

public class QueryDoc {
    

	public static final ElasticsearchTask SEARCH_BY_RANGE = client -> {
	    // 创建搜索请求对象
	    SearchRequest request = new SearchRequest();
	    request.indices("user");
	    // 构建查询的请求体
	    SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
	    RangeQueryBuilder rangeQuery = QueryBuilders.rangeQuery("age");
	    // 大于等于
	    //rangeQuery.gte("30");
	    // 小于等于
	    rangeQuery.lte("40");
	    sourceBuilder.query(rangeQuery);
	    request.source(sourceBuilder);
	    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
	    // 查询匹配
	    SearchHits hits = response.getHits();
	    System.out.println("took:" + response.getTook());
	    System.out.println("timeout:" + response.isTimedOut());
	    System.out.println("total:" + hits.getTotalHits());
	    System.out.println("MaxScore:" + hits.getMaxScore());
	    System.out.println("hits========>>");
	    for (SearchHit hit : hits) {
	    //输出每条查询的结果信息
	        System.out.println(hit.getSourceAsString());
	    }
	    System.out.println("<<========");
	};
	
	public static void main(String[] args) {
	    ConnectElasticsearch.connect(SEARCH_BY_RANGE);
	}

}
```


后台打印

```
took:1ms
timeout:false
total:5 hits
MaxScore:1.0
hits========>>
{"name":"zhangsan","age":"10","sex":"女"}
{"name":"lisi","age":"30","sex":"女"}
{"name":"wangwu1","age":"40","sex":"男"}
{"name":"wangwu2","age":"20","sex":"女"}
{"name":"wangwu4","age":"20","sex":"男"}
<<========

Process finished with exit code 0

```







## 27-入门-JavaAPI-文档-高级查询-模糊查询 & 高亮查询

### 模糊查询

```java
import com.lun.elasticsearch.hello.ConnectElasticsearch;
import com.lun.elasticsearch.hello.ElasticsearchTask;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.common.unit.Fuzziness;
import org.elasticsearch.index.query.BoolQueryBuilder;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.index.query.RangeQueryBuilder;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.SearchHits;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.elasticsearch.search.sort.SortOrder;

public class QueryDoc {
    

    public static final ElasticsearchTask SEARCH_BY_FUZZY_CONDITION = client -> {
        // 创建搜索请求对象
        SearchRequest request = new SearchRequest();
        request.indices("user");
        // 构建查询的请求体
        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
        sourceBuilder.query(QueryBuilders.fuzzyQuery("name","wangwu").fuzziness(Fuzziness.ONE));
        request.source(sourceBuilder);
        SearchResponse response = client.search(request, RequestOptions.DEFAULT);
        // 查询匹配
        SearchHits hits = response.getHits();
        System.out.println("took:" + response.getTook());
        System.out.println("timeout:" + response.isTimedOut());
        System.out.println("total:" + hits.getTotalHits());
        System.out.println("MaxScore:" + hits.getMaxScore());
        System.out.println("hits========>>");
        for (SearchHit hit : hits) {
            //输出每条查询的结果信息
            System.out.println(hit.getSourceAsString());
        }
        System.out.println("<<========");
    };


    public static void main(String[] args) {

//        ConnectElasticsearch.connect(SEARCH_ALL);
//        ConnectElasticsearch.connect(SEARCH_BY_CONDITION);
//        ConnectElasticsearch.connect(SEARCH_BY_PAGING);
//        ConnectElasticsearch.connect(SEARCH_WITH_ORDER);
//        ConnectElasticsearch.connect(SEARCH_BY_BOOL_CONDITION);
//        ConnectElasticsearch.connect(SEARCH_BY_RANGE);
        ConnectElasticsearch.connect(SEARCH_BY_FUZZY_CONDITION);
    }

}
```

后台打印

```
took:152ms
timeout:false
total:4 hits
MaxScore:1.2837042
hits========>>
{"name":"wangwu1","age":"40","sex":"男"}
{"name":"wangwu2","age":"20","sex":"女"}
{"name":"wangwu3","age":"50","sex":"男"}
{"name":"wangwu4","age":"20","sex":"男"}
<<========

Process finished with exit code 0
```

### 高亮查询

```java
import com.lun.elasticsearch.hello.ConnectElasticsearch;
import com.lun.elasticsearch.hello.ElasticsearchTask;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.common.unit.Fuzziness;
import org.elasticsearch.index.query.BoolQueryBuilder;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.index.query.RangeQueryBuilder;
import org.elasticsearch.index.query.TermsQueryBuilder;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.SearchHits;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.elasticsearch.search.fetch.subphase.highlight.HighlightBuilder;
import org.elasticsearch.search.fetch.subphase.highlight.HighlightField;
import org.elasticsearch.search.sort.SortOrder;

import java.util.Map;

public class QueryDoc {
    

    public static final ElasticsearchTask SEARCH_WITH_HIGHLIGHT = client -> {
        // 高亮查询
        SearchRequest request = new SearchRequest().indices("user");
        //2.创建查询请求体构建器
        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
        //构建查询方式：高亮查询
        TermsQueryBuilder termsQueryBuilder =
                QueryBuilders.termsQuery("name","zhangsan");
        //设置查询方式
        sourceBuilder.query(termsQueryBuilder);
        //构建高亮字段
        HighlightBuilder highlightBuilder = new HighlightBuilder();
        highlightBuilder.preTags("<font color='red'>");//设置标签前缀
        highlightBuilder.postTags("</font>");//设置标签后缀
        highlightBuilder.field("name");//设置高亮字段
        //设置高亮构建对象
        sourceBuilder.highlighter(highlightBuilder);
        //设置请求体
        request.source(sourceBuilder);
        //3.客户端发送请求，获取响应对象
        SearchResponse response = client.search(request, RequestOptions.DEFAULT);
        //4.打印响应结果
        SearchHits hits = response.getHits();
        System.out.println("took::"+response.getTook());
        System.out.println("time_out::"+response.isTimedOut());
        System.out.println("total::"+hits.getTotalHits());
        System.out.println("max_score::"+hits.getMaxScore());
        System.out.println("hits::::>>");
        for (SearchHit hit : hits) {
            String sourceAsString = hit.getSourceAsString();
            System.out.println(sourceAsString);
            //打印高亮结果
            Map<String, HighlightField> highlightFields = hit.getHighlightFields();
            System.out.println(highlightFields);
        }
        System.out.println("<<::::");
    };


    public static void main(String[] args) {
        ConnectElasticsearch.connect(SEARCH_WITH_HIGHLIGHT);
    }

}
```

后台打印

```
took::672ms
time_out::false
total::1 hits
max_score::1.0
hits::::>>
{"name":"zhangsan","age":"10","sex":"女"}
{name=[name], fragments[[<font color='red'>zhangsan</font>]]}
<<::::
```











### 入门-JavaAPI-文档-高级查询-最大值查询 & 分组查询

### 最大值查询

```java
import com.lun.elasticsearch.hello.ConnectElasticsearch;
import com.lun.elasticsearch.hello.ElasticsearchTask;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.common.unit.Fuzziness;
import org.elasticsearch.index.query.BoolQueryBuilder;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.index.query.RangeQueryBuilder;
import org.elasticsearch.index.query.TermsQueryBuilder;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.SearchHits;
import org.elasticsearch.search.aggregations.AggregationBuilders;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.elasticsearch.search.fetch.subphase.highlight.HighlightBuilder;
import org.elasticsearch.search.fetch.subphase.highlight.HighlightField;
import org.elasticsearch.search.sort.SortOrder;

import java.util.Map;

public class QueryDoc {
    

    public static final ElasticsearchTask SEARCH_WITH_MAX = client -> {
        // 高亮查询
        SearchRequest request = new SearchRequest().indices("user");
        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
        sourceBuilder.aggregation(AggregationBuilders.max("maxAge").field("age"));
        //设置请求体
        request.source(sourceBuilder);
        //3.客户端发送请求，获取响应对象
        SearchResponse response = client.search(request, RequestOptions.DEFAULT);
        //4.打印响应结果
        SearchHits hits = response.getHits();
        System.out.println(response);
    };
    
    public static void main(String[] args) {
        ConnectElasticsearch.connect(SEARCH_WITH_MAX);
    }

}
```

后台打印

```
{"took":16,"timed_out":false,"_shards":{"total":1,"successful":1,"skipped":0,"failed":0},"hits":{"total":{"value":6,"relation":"eq"},"max_score":1.0,"hits":[{"_index":"user","_type":"_doc","_id":"1001","_score":1.0,"_source":{"name":"zhangsan","age":"10","sex":"女"}},{"_index":"user","_type":"_doc","_id":"1002","_score":1.0,"_source":{"name":"lisi","age":"30","sex":"女"}},{"_index":"user","_type":"_doc","_id":"1003","_score":1.0,"_source":{"name":"wangwu1","age":"40","sex":"男"}},{"_index":"user","_type":"_doc","_id":"1004","_score":1.0,"_source":{"name":"wangwu2","age":"20","sex":"女"}},{"_index":"user","_type":"_doc","_id":"1005","_score":1.0,"_source":{"name":"wangwu3","age":"50","sex":"男"}},{"_index":"user","_type":"_doc","_id":"1006","_score":1.0,"_source":{"name":"wangwu4","age":"20","sex":"男"}}]},"aggregations":{"max#maxAge":{"value":50.0}}}

Process finished with exit code 0
```



### 分组查询

分组查询

```java
import com.lun.elasticsearch.hello.ConnectElasticsearch;
import com.lun.elasticsearch.hello.ElasticsearchTask;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.common.unit.Fuzziness;
import org.elasticsearch.index.query.BoolQueryBuilder;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.index.query.RangeQueryBuilder;
import org.elasticsearch.index.query.TermsQueryBuilder;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.SearchHits;
import org.elasticsearch.search.aggregations.AggregationBuilders;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.elasticsearch.search.fetch.subphase.highlight.HighlightBuilder;
import org.elasticsearch.search.fetch.subphase.highlight.HighlightField;
import org.elasticsearch.search.sort.SortOrder;

import java.util.Map;

public class QueryDoc {

	public static final ElasticsearchTask SEARCH_WITH_GROUP = client -> {
	    SearchRequest request = new SearchRequest().indices("user");
	    SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
	    sourceBuilder.aggregation(AggregationBuilders.terms("age_groupby").field("age"));
	    //设置请求体
	    request.source(sourceBuilder);
	    //3.客户端发送请求，获取响应对象
	    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
	    //4.打印响应结果
	    SearchHits hits = response.getHits();
	    System.out.println(response);
	};
	
	public static void main(String[] args) {
	    ConnectElasticsearch.connect(SEARCH_WITH_GROUP);
	}

}
```


后台打印

```java
{"took":10,"timed_out":false,"_shards":{"total":1,"successful":1,"skipped":0,"failed":0},"hits":{"total":{"value":6,"relation":"eq"},"max_score":1.0,"hits":[{"_index":"user","_type":"_doc","_id":"1001","_score":1.0,"_source":{"name":"zhangsan","age":"10","sex":"女"}},{"_index":"user","_type":"_doc","_id":"1002","_score":1.0,"_source":{"name":"lisi","age":"30","sex":"女"}},{"_index":"user","_type":"_doc","_id":"1003","_score":1.0,"_source":{"name":"wangwu1","age":"40","sex":"男"}},{"_index":"user","_type":"_doc","_id":"1004","_score":1.0,"_source":{"name":"wangwu2","age":"20","sex":"女"}},{"_index":"user","_type":"_doc","_id":"1005","_score":1.0,"_source":{"name":"wangwu3","age":"50","sex":"男"}},{"_index":"user","_type":"_doc","_id":"1006","_score":1.0,"_source":{"name":"wangwu4","age":"20","sex":"男"}}]},"aggregations":{"lterms#age_groupby":{"doc_count_error_upper_bound":0,"sum_other_doc_count":0,"buckets":[{"key":20,"doc_count":2},{"key":10,"doc_count":1},{"key":30,"doc_count":1},{"key":40,"doc_count":1},{"key":50,"doc_count":1}]}}}

Process finished with exit code 0
```









## 第3章 Elasticsearch环境

29-环境-简介
单机 & 集群
单台 Elasticsearch 服务器提供服务，往往都有最大的负载能力，超过这个阈值，服务器
性能就会大大降低甚至不可用，所以生产环境中，一般都是运行在指定服务器集群中。
除了负载能力，单点服务器也存在其他问题：

单台机器存储容量有限
单服务器容易出现单点故障，无法实现高可用
单服务的并发处理能力有限
配置服务器集群时，集群中节点数量没有限制，大于等于 2 个节点就可以看做是集群了。一
般出于高性能及高可用方面来考虑集群中节点数量都是 3 个以上

总之，集群能提高性能，增加容错。

集群 Cluster
**一个集群就是由一个或多个服务器节点组织在一起，共同持有整个的数据，并一起提供索引和搜索功能。**一个 Elasticsearch 集群有一个唯一的名字标识，这个名字默认就是”elasticsearch”。这个名字是重要的，因为一个节点只能通过指定某个集群的名字，来加入这个集群。

节点 Node
集群中包含很多服务器， 一个节点就是其中的一个服务器。 作为集群的一部分，它存储数据，参与集群的索引和搜索功能。

一个节点也是由一个名字来标识的，默认情况下，这个名字是一个随机的漫威漫画角色的名字，这个名字会在启动的时候赋予节点。这个名字对于管理工作来说挺重要的，因为在这个管理过程中，你会去确定网络中的哪些服务器对应于 Elasticsearch 集群中的哪些节点。

一个节点可以通过配置集群名称的方式来加入一个指定的集群。默认情况下，每个节点都会被安排加入到一个叫做“elasticsearch”的集群中，这意味着，如果你在你的网络中启动了若干个节点，并假定它们能够相互发现彼此，它们将会自动地形成并加入到一个叫做“elasticsearch”的集群中。

在一个集群里，只要你想，可以拥有任意多个节点。而且，如果当前你的网络中没有运
行任何 Elasticsearch 节点，这时启动一个节点，会默认创建并加入一个叫做“elasticsearch”的
集群。







## 30-环境-Windows集群部署

部署集群
一、创建 elasticsearch-cluster 文件夹

创建 elasticsearch-7.8.0-cluster 文件夹，在内部复制三个 elasticsearch 服务。

![img](C:\Users\32784\Desktop\笔记\image\6d6022b6d30a9e2c1a4e18092d5f130f.png)

二、修改集群文件目录中每个节点的 config/elasticsearch.yml 配置文件

node-1001 节点

```yml
#节点 1 的配置信息：
#集群名称，节点之间要保持一致
cluster.name: my-elasticsearch
#节点名称，集群内要唯一
node.name: node-1001
node.master: true
node.data: true
#ip 地址
network.host: localhost
#http 端口
http.port: 1001
#tcp 监听端口
transport.tcp.port: 9301
#discovery.seed_hosts: ["localhost:9301", "localhost:9302","localhost:9303"]
#discovery.zen.fd.ping_timeout: 1m
#discovery.zen.fd.ping_retries: 5
#集群内的可以被选为主节点的节点列表
#cluster.initial_master_nodes: ["node-1", "node-2","node-3"]
#跨域配置
#action.destructive_requires_name: true
http.cors.enabled: true
http.cors.allow-origin: "*"
```


node-1002 节点

```yml
#节点 2 的配置信息：
#集群名称，节点之间要保持一致
cluster.name: my-elasticsearch
#节点名称，集群内要唯一
node.name: node-1002
node.master: true
node.data: true
#ip 地址
network.host: localhost
#http 端口
http.port: 1002
#tcp 监听端口
transport.tcp.port: 9302
discovery.seed_hosts: ["localhost:9301"]
discovery.zen.fd.ping_timeout: 1m
discovery.zen.fd.ping_retries: 5
#集群内的可以被选为主节点的节点列表
#cluster.initial_master_nodes: ["node-1", "node-2","node-3"]
#跨域配置
#action.destructive_requires_name: true
http.cors.enabled: true
http.cors.allow-origin: "*"
```


node-1003 节点

```yml
#节点 3 的配置信息：
#集群名称，节点之间要保持一致
cluster.name: my-elasticsearch
#节点名称，集群内要唯一
node.name: node-1003
node.master: true
node.data: true
#ip 地址
network.host: localhost
#http 端口
http.port: 1003
#tcp 监听端口
transport.tcp.port: 9303
#候选主节点的地址，在开启服务后可以被选为主节点
discovery.seed_hosts: ["localhost:9301", "localhost:9302"]
discovery.zen.fd.ping_timeout: 1m
discovery.zen.fd.ping_retries: 5
#集群内的可以被选为主节点的节点列表
#cluster.initial_master_nodes: ["node-1", "node-2","node-3"]
#跨域配置
#action.destructive_requires_name: true
http.cors.enabled: true
http.cors.allow-origin: "*"

```

三、如果有必要，删除每个节点中的 data 目录中所有内容 。

启动集群
分别依次双击执行节点的bin/elasticsearch.bat, 启动节点服务器（可以编写一个脚本启动），启动后，会自动加入指定名称的集群。

测试集群
一、用Postman，查看集群状态

GET http://127.0.0.1:1001/_cluster/health
GET http://127.0.0.1:1002/_cluster/health
GET http://127.0.0.1:1003/_cluster/health
返回结果皆为如下：

```
{
    "cluster_name": "my-application",
    "status": "green",
    "timed_out": false,
    "number_of_nodes": 3,
    "number_of_data_nodes": 3,
    "active_primary_shards": 0,
    "active_shards": 0,
    "relocating_shards": 0,
    "initializing_shards": 0,
    "unassigned_shards": 0,
    "delayed_unassigned_shards": 0,
    "number_of_pending_tasks": 0,
    "number_of_in_flight_fetch": 0,
    "task_max_waiting_in_queue_millis": 0,
    "active_shards_percent_as_number": 100.0
}

```

status字段指示着当前集群在总体上是否工作正常。它的三种颜色含义如下：

green：所有的主分片和副本分片都正常运行。
yellow：所有的主分片都正常运行，但不是所有的副本分片都正常运行。
red：有主分片没能正常运行。
二、用Postman，在一节点增加索引，另一节点获取索引

向集群中的node-1001节点增加索引：

#PUT http://127.0.0.1:1001/user
1
返回结果：

```
{
    "acknowledged": true,
    "shards_acknowledged": true,
    "index": "user"
}

```

向集群中的node-1003节点获取索引：

#GET http://127.0.0.1:1003/user
1
返回结果：

```
{
    "user": {
        "aliases": {},
        "mappings": {},
        "settings": {
            "index": {
                "creation_date": "1617993035885",
                "number_of_shards": "1",
                "number_of_replicas": "1",
                "uuid": "XJKERwQlSJ6aUxZEN2EV0w",
                "version": {
                    "created": "7080099"
                },
                "provided_name": "user"
            }
        }
    }
}
```


如果在1003创建索引，同样在1001也能获取索引信息，这就是集群能力。









## 31-环境-Linux单节点部署

软件安装
一、下载软件

下载Linux版的Elasticsearch

二、解压软件

解压缩

tar -zxvf elasticsearch-7.8.0-linux-x86_64.tar.gz -C /opt/module

改名

mv elasticsearch-7.8.0 es



三、创建用户

因为安全问题， Elasticsearch 不允许 root 用户直接运行，所以要创建新用户，在 root 用户中创建新用户。

useradd es #新增 es 用户
passwd es #为 es 用户设置密码
userdel -r es #如果错了，可以删除再加
chown -R es:es /opt/module/es #文件夹所有者



四、修改配置文件

修改/opt/module/es/config/elasticsearch.yml文件。

加入如下配置

cluster.name: elasticsearch
node.name: node-1
network.host: 0.0.0.0
http.port: 9200
cluster.initial_master_nodes: ["node-1"]

修改/etc/security/limits.conf

在文件末尾中增加下面内容

每个进程可以打开的文件数的限制

es soft nofile 65536
es hard nofile 65536



修改/etc/security/limits.d/20-nproc.conf

在文件末尾中增加下面内容

每个进程可以打开的文件数的限制

es soft nofile 65536
es hard nofile 65536

操作系统级别对每个用户创建的进程数的限制

* hard nproc 4096

注： * 带表 Linux 所有用户名称


修改/etc/sysctl.conf

在文件中增加下面内容

一个进程可以拥有的 VMA(虚拟内存区域)的数量,默认值为 65536

vm.max_map_count=655360

重新加载

sysctl -p

启动软件
使用 ES 用户启动

```yml
cd /opt/module/es/
#启动
bin/elasticsearch
#后台启动
bin/elasticsearch -d  

```

启动时，会动态生成文件，如果文件所属用户不匹配，会发生错误，需要重新进行修改用户和用户组



关闭防火墙

```yml
#暂时关闭防火墙
systemctl stop firewalld
#永久关闭防火墙
systemctl enable firewalld.service #打开防火墙永久性生效，重启后不会复原
systemctl disable firewalld.service #关闭防火墙，永久性生效，重启后不会复原
```









2-环境-Linux集群部署
软件安装
一、下载软件

下载Linux版的Elasticsearch

二、解压软件

解压缩

tar -zxvf elasticsearch-7.8.0-linux-x86_64.tar.gz -C /opt/module

改名

mv elasticsearch-7.8.0 es-cluster

将软件分发到其他节点： linux2, linux3

三、创建用户

```yml
#因为安全问题， Elasticsearch 不允许 root 用户直接运行，所以要创建新用户，在 root 用户中创建新用户。

useradd es #新增 es 用户
passwd es #为 es 用户设置密码
userdel -r es #如果错了，可以删除再加
chown -R es:es /opt/module/es #文件夹所有者
```


四、修改配置文件

```
修改/opt/module/es/config/elasticsearch.yml 文件，分发文件。
```

加入如下配置

#集群名称

```
cluster.name: cluster-es
#节点名称， 每个节点的名称不能重复
node.name: node-1
#ip 地址， 每个节点的地址不能重复
network.host: linux1
#是不是有资格主节点
node.master: true
node.data: true
http.port: 9200

head 插件需要这打开这两个配置

http.cors.allow-origin: "*"
http.cors.enabled: true
http.max_content_length: 200mb
#es7.x 之后新增的配置，初始化一个新的集群时需要此配置来选举 master
cluster.initial_master_nodes: ["node-1"]
#es7.x 之后新增的配置，节点发现
discovery.seed_hosts: ["linux1:9300","linux2:9300","linux3:9300"]
gateway.recover_after_nodes: 2
network.tcp.keep_alive: true
network.tcp.no_delay: true
transport.tcp.compress: true
#集群内同时启动的数据任务个数，默认是 2 个
cluster.routing.allocation.cluster_concurrent_rebalance: 16
#添加或删除节点及负载均衡时并发恢复的线程个数，默认 4 个
cluster.routing.allocation.node_concurrent_recoveries: 16
#初始化数据恢复时，并发恢复线程的个数，默认 4 个
cluster.routing.allocation.node_initial_primaries_recove
```





修改/etc/security/limits.conf ，分发文件

在文件末尾中增加下面内容

es soft nofile 65536
es hard nofile 65536

修改/etc/security/limits.d/20-nproc.conf，分发文件

在文件末尾中增加下面内容

es soft nofile 65536
es hard nofile 65536
\* hard nproc 4096
\# 注： * 带表 Linux 所有用户名称

修改/etc/sysctl.conf

在文件中增加下面内容

vm.max_map_count=655360
1
重新加载

sysctl -p
1
启动软件

```
分别在不同节点上启动 ES 软件

cd /opt/module/es-cluster
#启动
bin/elasticsearch
#后台启动
bin/elasticsearch -d

```

测试集群

测试集群





32-环境-Linux集群部署
软件安装
一、下载软件

下载Linux版的Elasticsearch



二、解压软件

解压缩

tar -zxvf elasticsearch-7.8.0-linux-x86_64.tar.gz -C /opt/module



改名

mv elasticsearch-7.8.0 es-cluster
1
2
3
4
将软件分发到其他节点： linux2, linux3



三、创建用户

因为安全问题， Elasticsearch 不允许 root 用户直接运行，所以要创建新用户，在 root 用户中创建新用户。

useradd es #新增 es 用户
passwd es #为 es 用户设置密码
userdel -r es #如果错了，可以删除再加
chown -R es:es /opt/module/es #文件夹所有者
1
2
3
4
四、修改配置文件

修改/opt/module/es/config/elasticsearch.yml 文件，分发文件。

加入如下配置

```
#集群名称
cluster.name: cluster-es
#节点名称， 每个节点的名称不能重复
node.name: node-1
#ip 地址， 每个节点的地址不能重复
network.host: linux1
#是不是有资格主节点
node.master: true
node.data: true
http.port: 9200
```

head 插件需要这打开这两个配置

```
http.cors.allow-origin: "*"
http.cors.enabled: true
http.max_content_length: 200mb
#es7.x 之后新增的配置，初始化一个新的集群时需要此配置来选举 master
cluster.initial_master_nodes: ["node-1"]
#es7.x 之后新增的配置，节点发现
discovery.seed_hosts: ["linux1:9300","linux2:9300","linux3:9300"]
gateway.recover_after_nodes: 2
network.tcp.keep_alive: true
network.tcp.no_delay: true
transport.tcp.compress: true

#集群内同时启动的数据任务个数，默认是 2 个
cluster.routing.allocation.cluster_concurrent_rebalance: 16
#添加或删除节点及负载均衡时并发恢复的线程个数，默认 4 个
cluster.routing.allocation.node_concurrent_recoveries: 16
#初始化数据恢复时，并发恢复线程的个数，默认 4 个
cluster.routing.allocation.node_initial_primaries_recoveries: 16
```

修改/etc/security/limits.conf ，分发文件

在文件末尾中增加下面内容

es soft nofile 65536
es hard nofile 65536

修改/etc/security/limits.d/20-nproc.conf，分发文件

在文件末尾中增加下面内容

es soft nofile 65536
es hard nofile 65536
\* hard nproc 4096
\# 注： * 带表 Linux 所有用户名称

修改/etc/sysctl.conf

在文件中增加下面内容

vm.max_map_count=655360

重新加载

sysctl -p

启动软件
分别在不同节点上启动 ES 软件



cd /opt/module/es-cluster
#启动
bin/elasticsearch
#后台启动
bin/elasticsearch -d

测试集群







## 第4章 Elasticsearch进阶

33-进阶-核心概念


索引Index
一个索引就是一个拥有几分相似特征的文档的集合。比如说，你可以有一个客户数据的索引，另一个产品目录的索引，还有一个订单数据的索引。一个索引由一个名字来标识（必须全部是小写字母），并且当我们要对这个索引中的文档进行索引、搜索、更新和删除（CRUD）的时候，都要使用到这个名字。在一个集群中，可以定义任意多的索引。

能搜索的数据必须索引，这样的好处是可以提高查询速度，比如：新华字典前面的目录就是索引的意思，目录可以提高查询速度。

Elasticsearch 索引的精髓：一切设计都是为了提高搜索的性能。

### 类型Type

在一个索引中，你可以定义一种或多种类型。

一个类型是你的索引的一个逻辑上的分类/分区，其语义完全由你来定。通常，会为具
有一组共同字段的文档定义一个类型。不同的版本，类型发生了不同的变化。

版本	Type
5.x	支持多种 type
6.x	只能有一种 type
7.x	默认不再支持自定义索引类型（默认类型为： _doc）

### 文档Document

一个文档是一个可被索引的基础信息单元，也就是一条数据。

比如：你可以拥有某一个客户的文档，某一个产品的一个文档，当然，也可以拥有某个订单的一个文档。文档以 JSON（Javascript Object Notation）格式来表示，而 JSON 是一个到处存在的互联网数据交互格式。

在一个 index/type 里面，你可以存储任意多的文档。

字段Field
相当于是数据表的字段，对文档数据根据不同属性进行的分类标识。

映射Mapping
mapping 是处理数据的方式和规则方面做一些限制，如：某个字段的数据类型、默认值、分析器、是否被索引等等。这些都是映射里面可以设置的，其它就是处理 ES 里面数据的一些使用规则设置也叫做映射，按着最优规则处理数据对性能提高很大，因此才需要建立映射，并且需要思考如何建立映射才能对性能更好。

分片Shards
一个索引可以存储超出单个节点硬件限制的大量数据。比如，一个具有 10 亿文档数据
的索引占据 1TB 的磁盘空间，而任一节点都可能没有这样大的磁盘空间。 或者单个节点处理搜索请求，响应太慢。为了解决这个问题，**Elasticsearch 提供了将索引划分成多份的能力，每一份就称之为分片。**当你创建一个索引的时候，你可以指定你想要的分片的数量。每个分片本身也是一个功能完善并且独立的“索引”，这个“索引”可以被放置到集群中的任何节点上。

分片很重要，主要有两方面的原因：

允许你水平分割 / 扩展你的内容容量。
允许你在分片之上进行分布式的、并行的操作，进而提高性能/吞吐量。
至于一个分片怎样分布，它的文档怎样聚合和搜索请求，是完全由 Elasticsearch 管理的，对于作为用户的你来说，这些都是透明的，无需过分关心。

被混淆的概念是，一个 Lucene 索引 我们在 Elasticsearch 称作 分片 。 一个Elasticsearch 索引 是分片的集合。 当 Elasticsearch 在索引中搜索的时候， 他发送查询到每一个属于索引的分片（Lucene 索引），然后合并每个分片的结果到一个全局的结果集。

Lucene 是 Apache 软件基金会 Jakarta 项目组的一个子项目，提供了一个简单却强大的应用程式接口，能够做全文索引和搜寻。在 Java 开发环境里 Lucene 是一个成熟的免费开源工具。就其本身而言， Lucene 是当前以及最近几年最受欢迎的免费 Java 信息检索程序库。但 Lucene 只是一个提供全文搜索功能类库的核心工具包，而真正使用它还需要一个完善的服务框架搭建起来进行应用。

目前市面上流行的搜索引擎软件，主流的就两款： Elasticsearch 和 Solr,这两款都是基于 Lucene 搭建的，可以独立部署启动的搜索引擎服务软件。由于内核相同，所以两者除了服务器安装、部署、管理、集群以外，对于数据的操作 修改、添加、保存、查询等等都十分类似。

### 副本Replicas

在一个网络 / 云的环境里，失败随时都可能发生，在某个分片/节点不知怎么的就处于
离线状态，或者由于任何原因消失了，这种情况下，有一个故障转移机制是非常有用并且是强烈推荐的。为此目的， Elasticsearch 允许你创建分片的一份或多份拷贝，这些拷贝叫做复制分片(副本)。

复制分片之所以重要，有两个主要原因：

在分片/节点失败的情况下，提供了高可用性。因为这个原因，注意到复制分片从不与原/主要（original/primary）分片置于同一节点上是非常重要的。
扩展你的搜索量/吞吐量，因为搜索可以在所有的副本上并行运行。
总之，每个索引可以被分成多个分片。一个索引也可以被复制 0 次（意思是没有复制）或多次。一旦复制了，每个索引就有了主分片（作为复制源的原来的分片）和复制分片（主分片的拷贝）之别。

分片和复制的数量可以在索引创建的时候指定。在索引创建之后，你可以在任何时候动态地改变复制的数量，但是你事后不能改变分片的数量。

默认情况下，Elasticsearch 中的每个索引被分片 1 个主分片和 1 个复制，这意味着，如果你的集群中至少有两个节点，你的索引将会有 1 个主分片和另外 1 个复制分片（1 个完全拷贝），这样的话每个索引总共就有 2 个分片， 我们需要根据索引需要确定分片个数。

分配Allocation
将分片分配给某个节点的过程，包括分配主分片或者副本。如果是副本，还包含从主分片复制数据的过程。这个过程是由 master 节点完成的。

### 34-进阶-系统架构-简介


一个运行中的 Elasticsearch 实例称为一个节点，而集群是由一个或者多个拥有相同
cluster.name 配置的节点组成， 它们共同承担数据和负载的压力。当有节点加入集群中或者从集群中移除节点时，集群将会重新平均分布所有的数据。

当一个节点被选举成为主节点时， 它将负责管理集群范围内的所有变更，例如增加、
删除索引，或者增加、删除节点等。 而主节点并不需要涉及到文档级别的变更和搜索等操作，所以当集群只拥有一个主节点的情况下，即使流量的增加它也不会成为瓶颈。 任何节点都可以成为主节点。我们的示例集群就只有一个节点，所以它同时也成为了主节点。

作为用户，我们可以将请求发送到集群中的任何节点 ，包括主节点。 每个节点都知道
任意文档所处的位置，并且能够将我们的请求直接转发到存储我们所需文档的节点。 无论我们将请求发送到哪个节点，它都能负责从各个包含我们所需文档的节点收集回数据，并将最终结果返回給客户端。 Elasticsearch 对这一切的管理都是透明的。









## 5-进阶-单节点集群

我们在包含一个空节点的集群内创建名为 users 的索引，为了演示目的，我们将分配 3个主分片和一份副本（每个主分片拥有一个副本分片）。

```json
#PUT http://127.0.0.1:1001/users
{
    "settings" : {
        "number_of_shards" : 3,
        "number_of_replicas" : 1
    }
}
```


集群现在是拥有一个索引的单节点集群。所有 3 个主分片都被分配在 node-1 。

![img](C:\Users\32784\Desktop\笔记\image\54ee6753be248cc7d345b38a0eae7d96.png)

通过 elasticsearch-head 插件（一个Chrome插件）查看集群情况 。

![img](C:\Users\32784\Desktop\笔记\image\e8b15d0b243d486e91f478a220da63bf.png)

集群健康值:yellow( 3 of 6 )：表示当前集群的全部主分片都正常运行，但是副本分片没有全部处在正常状态。

3 个主分片正常。



![img](C:\Users\32784\Desktop\笔记\image\489b6de480112879a00067b793bde685.png)



![img](C:\Users\32784\Desktop\笔记\image\3ce9a78d26ee762f0a7a8abf7817a58e.png)



3 个副本分片都是 Unassigned，它们都没有被分配到任何节点。 在同 一个节点上既保存原始数据又保存副本是没有意义的，因为一旦失去了那个节点，我们也将丢失该节点 上的所有副本数据。



当前集群是正常运行的，但存在丢失数据的风险。

elasticsearch-head chrome插件安装

插件获取网址，下载压缩包，解压后将内容放入自定义命名为elasticsearch-head文件夹。

接着点击Chrome右上角选项->工具->管理扩展（或则地址栏输入chrome://extensions/），选择打开“开发者模式”，让后点击“加载已解压得扩展程序”，选择elasticsearch-head/_site，即可完成chrome插件安装。

36-进阶-故障转移
当集群中只有一个节点在运行时，意味着会有一个单点故障问题——没有冗余。 幸运的是，我们只需再启动一个节点即可防止数据丢失。当你在同一台机器上启动了第二个节点时，只要它和第一个节点有同样的 cluster.name 配置，它就会自动发现集群并加入到其中。但是在不同机器上启动节点的时候，为了加入到同一集群，你需要配置一个可连接到的单播主机列表。之所以配置为使用单播发现，以防止节点无意中加入集群。只有在同一台机器上
运行的节点才会自动组成集群。

如果启动了第二个节点，集群将会拥有两个节点 : 所有主分片和副本分片都已被分配 。

![img](C:\Users\32784\Desktop\笔记\image\bf76cb1bfbdf07555918d9055817ab44.png)

通过 elasticsearch-head 插件查看集群情况

集群健康值:green( 3 of 6 )：表示所有 6 个分片（包括 3 个主分片和 3 个副本分片）都在正常运行。

![img](C:\Users\32784\Desktop\笔记\image\18db400822b83e727d6206f486b7b2ea.png)



：3 个主分片正常。
：第二个节点加入到集群后， 3 个副本分片将会分配到这个节点上——每 个主分片对应一个副本分片。这意味着当集群内任何一个节点出现问题时，我们的数据都完好无损。所 有新近被索引的文档都将会保存在主分片上，然后被并行的复制到对应的副本分片上。这就保证了我们 既可以从主分片又可以从副本分片上获得文档。





36-进阶-故障转移
当集群中只有一个节点在运行时，意味着会有一个单点故障问题——没有冗余。 幸运的是，我们只需再启动一个节点即可防止数据丢失。当你在同一台机器上启动了第二个节点时，只要它和第一个节点有同样的 cluster.name 配置，它就会自动发现集群并加入到其中。但是在不同机器上启动节点的时候，为了加入到同一集群，你需要配置一个可连接到的单播主机列表。之所以配置为使用单播发现，以防止节点无意中加入集群。只有在同一台机器上
运行的节点才会自动组成集群。

如果启动了第二个节点，集群将会拥有两个节点 : 所有主分片和副本分片都已被分配 。

![img](C:\Users\32784\Desktop\笔记\image\bf76cb1bfbdf07555918d9055817ab44-169994797941638.png)



通过 elasticsearch-head 插件查看集群情况

![img](C:\Users\32784\Desktop\笔记\image\18db400822b83e727d6206f486b7b2ea-169994798505041.png)

集群健康值:green( 3 of 6 )：表示所有 6 个分片（包括 3 个主分片和 3 个副本分片）都在正常运行。
：3 个主分片正常。
：第二个节点加入到集群后， 3 个副本分片将会分配到这个节点上——每 个主分片对应一个副本分片。这意味着当集群内任何一个节点出现问题时，我们的数据都完好无损。所 有新近被索引的文档都将会保存在主分片上，然后被并行的复制到对应的副本分片上。这就保证了我们 既可以从主分片又可以从副本分片上获得文档。





## 7-进阶-水平扩容

怎样为我们的正在增长中的应用程序按需扩容呢？当启动了第三个节点，我们的集群将会拥有三个节点的集群 : 为了分散负载而对分片进行重新分配 。

![img](C:\Users\32784\Desktop\笔记\image\d527e26aa2bccdf54b11410024eadc92.png)

通过 elasticsearch-head 插件查看集群情况。

![img](C:\Users\32784\Desktop\笔记\image\6985fe14454c1269204478320d089bd7.png)

- 集群健康值:green( 3 of 6 )：表示所有 6 个分片（包括 3 个主分片和 3 个副本分片）都在正常运行。

![img](C:\Users\32784\Desktop\笔记\image\9494419153adb44bedb395ac5d7bc488.png)

Node 1 和 Node 2 上各有一个分片被迁移到了新的 Node 3 节点，现在每个节点上都拥有 2 个分片， 而不是之前的 3 个。 这表示每个节点的硬件资源（CPU, RAM, I/O）将被更少的分片所共享，每个分片 的性能将会得到提升。
分片是一个功能完整的搜索引擎，它拥有使用一个节点上的所有资源的能力。 我们这个拥有 6 个分 片（3 个主分片和 3 个副本分片）的索引可以最大扩容到 6 个节点，每个节点上存在一个分片，并且每个 分片拥有所在节点的全部资源。

但是如果我们想要扩容超过 6 个节点怎么办呢？

主分片的数目在索引创建时就已经确定了下来。实际上，这个数目定义了这个索引能够
存储 的最大数据量。（实际大小取决于你的数据、硬件和使用场景。） 但是，读操作——
搜索和返回数据——可以同时被主分片 或 副本分片所处理，所以当你拥有越多的副本分片
时，也将拥有越高的吞吐量。

在运行中的集群上是可以动态调整副本分片数目的，我们可以按需伸缩集群。让我们把
副本数从默认的 1 增加到 2。

```java
#PUT http://127.0.0.1:1001/users/_settings

{
    "number_of_replicas" : 2
}
1
```



users 索引现在拥有 9 个分片： 3 个主分片和 6 个副本分片。 这意味着我们可以将集群
扩容到 9 个节点，每个节点上一个分片。相比原来 3 个节点时，集群搜索性能可以提升 3 倍。

![img](C:\Users\32784\Desktop\笔记\image\97fd01e34e5d8df23d226c4fef157801.png)

通过 elasticsearch-head 插件查看集群情况



![img](C:\Users\32784\Desktop\笔记\image\8bf9dbf0cec5b7875bf8aa9d17a9a67c.png)

当然，如果只是在相同节点数目的集群上增加更多的副本分片并不能提高性能，因为每
个分片从节点上获得的资源会变少。 你需要增加更多的硬件资源来提升吞吐量。

但是更多的副本分片数提高了数据冗余量：按照上面的节点配置，我们可以在失去 2 个节点
的情况下不丢失任何数据。





## 38-进阶-应对故障

我们关闭第一个节点，这时集群的状态为:关闭了一个节点后的集群。

![img](C:\Users\32784\Desktop\笔记\image\44e841004be934e6bce08187ca3852bb.png)

![img](C:\Users\32784\Desktop\笔记\image\e956bda7e0d005699e27760d4193d101.png)

为什么我们集群状态是 yellow 而不是 green 呢？

虽然我们拥有所有的三个主分片，但是同时设置了每个主分片需要对应 2 份副本分片，而此
时只存在一份副本分片。 所以集群不能为 green 的状态，不过我们不必过于担心：如果我
们同样关闭了 Node 2 ，我们的程序 依然 可以保持在不丢任何数据的情况下运行，因为
Node 3 为每一个分片都保留着一份副本。

如果想回复原来的样子，要确保Node-1的配置文件有如下配置：

```
discovery.seed_hosts: ["localhost:9302", "localhost:9303"]
1
```

集群可以将缺失的副本分片再次进行分配，那么集群的状态也将恢复成之前的状态。 如果 Node 1 依然拥有着之前的分片，它将尝试去重用它们，同时仅从主分片复制发生了修改的数据文件。和之前的集群相比，只是 Master 节点切换了。



![img](C:\Users\32784\Desktop\笔记\image\37eea6a8dae7ba908312f2ebf0eced11.png)









## 39-进阶-路由计算 & 分片控制

### 路由计算

当索引一个文档的时候，文档会被存储到一个主分片中。 Elasticsearch 如何知道一个
文档应该存放到哪个分片中呢？当我们创建文档时，它如何决定这个文档应当被存储在分片 1 还是分片 2 中呢？首先这肯定不会是随机的，否则将来要获取文档的时候我们就不知道从何处寻找了。实际上，这个过程是根据下面这个公式决定的：

shard = hash(routing) % number_of_primary_shards
1
routing 是一个可变值，默认是文档的 _id ，也可以设置成一个自定义的值。 routing 通过hash 函数生成一个数字，然后这个数字再除以 number_of_primary_shards （主分片的数量）后得到余数 。这个分布在 0 到 number_of_primary_shards-1 之间的余数，就是我们所寻求的文档所在分片的位置。

![img](C:\Users\32784\Desktop\笔记\image\9c34e8603887c2bed475416e3b67cd9a.png)



这就解释了为什么我们要在创建索引的时候就确定好主分片的数量并且永远不会改变这个数量:因为如果数量变化了，那么所有之前路由的值都会无效，文档也再也找不到了。

所有的文档API ( get . index . delete 、 bulk , update以及 mget ）都接受一个叫做routing 的路由参数，通过这个参数我们可以自定义文档到分片的映射。一个自定义的路由参数可以用来确保所有相关的文档—一例如所有属于同一个用户的文档——都被存储到同一个分片中


![img](C:\Users\32784\Desktop\笔记\image\3940d6cdb197259368542b86384911a4.png)

分片控制
我们可以发送请求到集群中的任一节点。每个节点都有能力处理任意请求。每个节点都知道集群中任一文档位置，所以可以直接将请求转发到需要的节点上。在下面的例子中，如果将所有的请求发送到Node 1001，我们将其称为协调节点coordinating node。



当发送请求的时候， 为了扩展负载，更好的做法是轮询集群中所有的节点。
28





## 40-进阶-数据写流程

新建、索引和删除请求都是写操作， 必须在主分片上面完成之后才能被复制到相关的副本分片。

![img](C:\Users\32784\Desktop\笔记\image\418356a32516c222a8d366df021276c2.png)

在客户端收到成功响应时，文档变更已经在主分片和所有副本分片执行完成，变更是安全的。有一些可选的请求参数允许您影响这个过程，可能以数据安全为代价提升性能。这些选项很少使用，因为 Elasticsearch 已经很快，但是为了完整起见， 请参考下文：

consistency
即一致性。在默认设置下，即使仅仅是在试图执行一个写操作之前，主分片都会要求必须要有规定数量quorum（或者换种说法，也即必须要有大多数）的分片副本处于活跃可用状态，才会去执行写操作（其中分片副本 可以是主分片或者副本分片）。这是为了避免在发生网络分区故障（network partition）的时候进行写操作，进而导致数据不一致。 规定数量即： int((primary + number_of_replicas) / 2 ) + 1
consistency 参数的值可以设为：
one ：只要主分片状态 ok 就允许执行写操作。
all：必须要主分片和所有副本分片的状态没问题才允许执行写操作。
quorum：默认值为quorum , 即大多数的分片副本状态没问题就允许执行写操作。
注意，规定数量的计算公式中number_of_replicas指的是在索引设置中的设定副本分片数，而不是指当前处理活动状态的副本分片数。如果你的索引设置中指定了当前索引拥有3个副本分片，那规定数量的计算结果即：int((1 primary + 3 replicas) / 2) + 1 = 3，如果此时你只启动两个节点，那么处于活跃状态的分片副本数量就达不到规定数量，也因此您将无法索引和删除任何文档。
timeout
如果没有足够的副本分片会发生什么？Elasticsearch 会等待，希望更多的分片出现。默认情况下，它最多等待 1 分钟。 如果你需要，你可以使用timeout参数使它更早终止：100是100 毫秒，30s是30秒。
新索引默认有1个副本分片，这意味着为满足规定数量应该需要两个活动的分片副本。 但是，这些默认的设置会阻止我们在单一节点上做任何事情。为了避免这个问题，要求只有当number_of_replicas 大于1的时候，规定数量才会执行。







![img](C:\Users\32784\Desktop\笔记\image\7139df83ee6f7a59c5d3252d34cc8762.png)

在处理读取请求时，协调结点在每次请求的时候都会通过轮询所有的副本分片来达到负载均衡。在文档被检索时，已经被索引的文档可能已经存在于主分片上但是还没有复制到副本分片。 在这种情况下，副本分片可能会报告文档不存在，但是主分片可能成功返回文档。 一旦索引请求成功返回给用户，文档在主分片和副本分片都是可用的。






## 42-进阶-更新流程 & 批量操作流程

更新流程
部分更新一个文档结合了先前说明的读取和写入流程：

![img](C:\Users\32784\Desktop\笔记\image\ca41993144aee98311671278725437cd.png)

部分更新一个文档的步骤如下：

客户端向Node 1发送更新请求。
它将请求转发到主分片所在的Node 3 。
Node 3从主分片检索文档，修改_source字段中的JSON，并且尝试重新索引主分片的文档。如果文档已经被另一个进程修改,它会重试步骤3 ,超过retry_on_conflict次后放弃。
如果 Node 3成功地更新文档，它将新版本的文档并行转发到Node 1和 Node 2上的副本分片，重新建立索引。一旦所有副本分片都返回成功，Node 3向协调节点也返回成功，协调节点向客户端返回成功。
当主分片把更改转发到副本分片时， 它不会转发更新请求。 相反，它转发完整文档的新版本。请记住，这些更改将会异步转发到副本分片，并且不能保证它们以发送它们相同的顺序到达。 如果 Elasticsearch 仅转发更改请求，则可能以错误的顺序应用更改，导致得到损坏的文档。

批量操作流程
**mget和 bulk API的模式类似于单文档模式。**区别在于协调节点知道每个文档存在于哪个分片中。它将整个多文档请求分解成每个分片的多文档请求，并且将这些请求并行转发到每个参与节点。

协调节点一旦收到来自每个节点的应答，就将每个节点的响应收集整理成单个响应，返回给客户端。

![img](C:\Users\32784\Desktop\笔记\image\b90ea9c79138d8361ca339cff205fdb0.png)

用单个 mget 请求取回多个文档所需的步骤顺序:

客户端向 Node 1 发送 mget 请求。
Node 1为每个分片构建多文档获取请求，然后并行转发这些请求到托管在每个所需的主分片或者副本分片的节点上。一旦收到所有答复，Node 1 构建响应并将其返回给客户端。
可以对docs数组中每个文档设置routing参数。

bulk API， 允许在单个批量请求中执行多个创建、索引、删除和更新请求。

![img](C:\Users\32784\Desktop\笔记\image\83499315a7b8ab81471a88f3e142f0a8.png)

bulk API 按如下步骤顺序执行：

客户端向Node 1 发送 bulk请求。
Node 1为每个节点创建一个批量请求，并将这些请求并行转发到每个包含主分片的节点主机。
主分片一个接一个按顺序执行每个操作。当每个操作成功时,主分片并行转发新文档（或删除）到副本分片，然后执行下一个操作。一旦所有的副本分片报告所有操作成功，该节点将向协调节点报告成功，协调节点将这些响应收集整理并返回给客户端。
————————————————





## 43-进阶-倒排索引

分片是Elasticsearch最小的工作单元。但是究竟什么是一个分片，它是如何工作的？

传统的数据库每个字段存储单个值，但这对全文检索并不够。文本字段中的每个单词需要被搜索，对数据库意味着需要单个字段有索引多值的能力。最好的支持是一个字段多个值需求的数据结构是倒排索引。

倒排索引原理
Elasticsearch使用一种称为倒排索引的结构，它适用于快速的全文搜索。

见其名，知其意，有倒排索引，肯定会对应有正向索引。正向索引（forward index），反向索引（inverted index）更熟悉的名字是倒排索引。

所谓的正向索引，就是搜索引擎会将待搜索的文件都对应一个文件ID，搜索时将这个ID和搜索关键字进行对应，形成K-V对，然后对关键字进行统计计数。（统计？？下文有解释）

![img](C:\Users\32784\Desktop\笔记\image\cba02cc6d7c5f054dfe5d58fafac9a6a.png)

但是互联网上收录在搜索引擎中的文档的数目是个天文数字，这样的索引结构根本无法满足实时返回排名结果的要求。所以，搜索引擎会将正向索引重新构建为倒排索引，即把文件ID对应到关键词的映射转换为关键词到文件ID的映射，每个关键词都对应着一系列的文件，这些文件中都出现这个关键词。

![img](C:\Users\32784\Desktop\笔记\image\a1f52e96e0ac218b5024d708202afba4.png)

倒排索引的例子
一个倒排索引由文档中所有不重复词的列表构成，对于其中每个词，有一个包含它的文档列表。例如，假设我们有两个文档，每个文档的content域包含如下内容：

The quick brown fox jumped over the lazy dog
Quick brown foxes leap over lazy dogs in summer
为了创建倒排索引，我们首先将每个文档的content域拆分成单独的词（我们称它为词条或tokens )，创建一个包含所有不重复词条的排序列表，然后列出每个词条出现在哪个文档。结果如下所示：

![img](C:\Users\32784\Desktop\笔记\image\3cc642e9bae776c3e617f9d117d41e21.png)

现在，如果我们想搜索 quick brown ，我们只需要查找包含每个词条的文档：

![img](C:\Users\32784\Desktop\笔记\image\f26aaa01e011edfa68736956b2f1ddea.png)

两个文档都匹配，但是第一个文档比第二个匹配度更高。如果我们使用仅计算匹配词条数量的简单相似性算法，那么我们可以说，对于我们查询的相关性来讲，第一个文档比第二个文档更佳。

但是，我们目前的倒排索引有一些问题：

Quick和quick以独立的词条出现，然而用户可能认为它们是相同的词。

fox和foxes非常相似，就像dog和dogs；他们有相同的词根。

jumped和leap，尽管没有相同的词根，但他们的意思很相近。他们是同义词。

使用前面的索引搜索+Quick +fox不会得到任何匹配文档。(记住，＋前缀表明这个词必须存在）。

只有同时出现Quick和fox 的文档才满足这个查询条件，但是第一个文档包含quick fox ，第二个文档包含Quick foxes 。

我们的用户可以合理的期望两个文档与查询匹配。我们可以做的更好。

如果我们将词条规范为标准模式，那么我们可以找到与用户搜索的词条不完全一致，但具有足够相关性的文档。例如：

Quick可以小写化为quick。
foxes可以词干提取变为词根的格式为fox。类似的，dogs可以为提取为dog。
jumped和leap是同义词，可以索引为相同的单词jump 。
现在索引看上去像这样：

![img](C:\Users\32784\Desktop\笔记\image\19813d1918c89461303377444cf85c8c.png)

这还远远不够。我们搜索+Quick +fox 仍然会失败，因为在我们的索引中，已经没有Quick了。但是，如果我们对搜索的字符串使用与content域相同的标准化规则，会变成查询+quick +fox，这样两个文档都会匹配！分词和标准化的过程称为分析，这非常重要。你只能搜索在索引中出现的词条，所以索引文本和查询字符串必须标准化为相同的格式。








## 45-进阶-文档刷新 & 文档刷写 & 文档合并

![img](C:\Users\32784\Desktop\笔记\image\b3b31c1e592d5aa794e7c9fcb259c924.png)

![img](C:\Users\32784\Desktop\笔记\image\521c25f0f16247240234d1b8eb3c5f25.png)

### 近实时搜索

随着按段（per-segment）搜索的发展，一个新的文档从索引到可被搜索的延迟显著降低了。新文档在几分钟之内即可被检索，但这样还是不够快。磁盘在这里成为了瓶颈。提交（Commiting）一个新的段到磁盘需要一个fsync来确保段被物理性地写入磁盘，这样在断电的时候就不会丢失数据。但是fsync操作代价很大；如果每次索引一个文档都去执行一次的话会造成很大的性能问题。

我们需要的是一个更轻量的方式来使一个文档可被搜索，这意味着fsync要从整个过程中被移除。在Elasticsearch和磁盘之间是文件系统缓存。像之前描述的一样，在内存索引缓冲区中的文档会被写入到一个新的段中。但是这里新段会被先写入到文件系统缓存—这一步代价会比较低，稍后再被刷新到磁盘—这一步代价比较高。不过只要文件已经在缓存中，就可以像其它文件一样被打开和读取了。



![img](C:\Users\32784\Desktop\笔记\image\a679d4f5f4bfa6913a53316251beef2a.png)

Lucene允许新段被写入和打开，使其包含的文档在未进行一次完整提交时便对搜索可见。这种方式比进行一次提交代价要小得多，并且在不影响性能的前提下可以被频繁地执行。

![img](C:\Users\32784\Desktop\笔记\image\673d3a77e254fa3a5a6f5293ffb125ab.png)





在 Elasticsearch 中，写入和打开一个新段的轻量的过程叫做refresh。默认情况下每个分片会每秒自动刷新一次。这就是为什么我们说 Elasticsearch是近实时搜索：文档的变化并不是立即对搜索可见，但会在一秒之内变为可见。

这些行为可能会对新用户造成困惑：他们索引了一个文档然后尝试搜索它，但却没有搜到。这个问题的解决办法是用refresh API执行一次手动刷新：/usersl_refresh

尽管刷新是比提交轻量很多的操作，它还是会有性能开销。当写测试的时候，手动刷新很有用，但是不要在生产环境下每次索引一个文档都去手动刷新。相反，你的应用需要意识到Elasticsearch 的近实时的性质，并接受它的不足。

并不是所有的情况都需要每秒刷新。可能你正在使用Elasticsearch索引大量的日志文件，你可能想优化索引速度而不是近实时搜索，可以通过设置refresh_interval ，降低每个索引的刷新频率

```
{
    "settings": {
    	"refresh_interval": "30s"
    }
}

```

refresh_interval可以在既存索引上进行动态更新。在生产环境中，当你正在建立一个大的新索引时，可以先关闭自动刷新，待开始使用该索引时，再把它们调回来。

关闭自动刷新

```
PUT /users/_settings
{ "refresh_interval": -1 }

每一秒刷新

PUT /users/_settings
{ "refresh_interval": "1s" }
```


持久化变更
如果没有用fsync把数据从文件系统缓存刷（flush）到硬盘，我们不能保证数据在断电甚至是程序正常退出之后依然存在。为了保证Elasticsearch 的可靠性，需要确保数据变化被持久化到磁盘。在动态更新索引，我们说一次完整的提交会将段刷到磁盘，并写入一个包含所有段列表的提交点。Elasticsearch 在启动或重新打开一个索引的过程中使用这个提交点来判断哪些段隶属于当前分片。

持久化变更
如果没有用fsync把数据从文件系统缓存刷（flush）到硬盘，我们不能保证数据在断电甚至是程序正常退出之后依然存在。为了保证Elasticsearch 的可靠性，需要确保数据变化被持久化到磁盘。在动态更新索引，我们说一次完整的提交会将段刷到磁盘，并写入一个包含所有段列表的提交点。Elasticsearch 在启动或重新打开一个索引的过程中使用这个提交点来判断哪些段隶属于当前分片。

即使通过每秒刷新(refresh）实现了近实时搜索，我们仍然需要经常进行完整提交来确保能从失败中恢复。但在两次提交之间发生变化的文档怎么办?我们也不希望丢失掉这些数据。Elasticsearch 增加了一个translog ，或者叫事务日志，在每一次对Elasticsearch进行操作时均进行了日志记录。


整个流程如下:

一、一个文档被索引之后，就会被添加到内存缓冲区，并且追加到了 translog

![img](C:\Users\32784\Desktop\笔记\image\baeab48c8d6b87660ac4fb954e9c9731.png)

### 刷新（refresh）

使分片每秒被刷新（refresh）一次：

- 这些在内存缓冲区的文档被写入到一个新的段中，且没有进行fsync操作。
- 这个段被打开，使其可被搜索。
- 内存缓冲区被清空。

![img](C:\Users\32784\Desktop\笔记\image\17be4247e6b23f31b1e589c70d61e817.png)

三、这个进程继续工作，更多的文档被添加到内存缓冲区和追加到事务日志。

![img](C:\Users\32784\Desktop\笔记\image\4b5c4a3a3ffb4c84625bb283f6a67018.png)

四、每隔一段时间—例如translog变得越来越大，索引被刷新（flush）；一个新的translog被创建，并且一个全量提交被执行。

所有在内存缓冲区的文档都被写入一个新的段。

缓冲区被清空。

一个提交点被写入硬盘。

文件系统缓存通过fsync被刷新（flush） 。

老的translog被删除。

translog 提供所有还没有被刷到磁盘的操作的一个持久化纪录。当Elasticsearch启动的时候，它会从磁盘中使用最后一个提交点去恢复己知的段，并且会重放translog 中所有在最后一次提交后发生的变更操作。

translog 也被用来提供实时CRUD。当你试着通过ID查询、更新、删除一个文档，它会在尝试从相应的段中检索之前，首先检查 translog任何最近的变更。这意味着它总是能够实时地获取到文档的最新版本。




![img](C:\Users\32784\Desktop\笔记\image\11c7d2cc05244e669eb8402dd8049de9.png)

执行一个提交并且截断translog 的行为在 Elasticsearch被称作一次flush。分片每30分钟被自动刷新（flush)，或者在 translog 太大的时候也会刷新。

你很少需要自己手动执行flush操作，通常情况下，自动刷新就足够了。这就是说，在重启节点或关闭索引之前执行 flush有益于你的索引。当Elasticsearch尝试恢复或重新打开一个索引，它需要重放translog中所有的操作，所以如果日志越短，恢复越快。

translog 的目的是保证操作不会丢失，在文件被fsync到磁盘前，被写入的文件在重启之后就会丢失。默认translog是每5秒被fsync刷新到硬盘，或者在每次写请求完成之后执行（e.g. index, delete, update, bulk）。这个过程在主分片和复制分片都会发生。最终，基本上，这意味着在整个请求被fsync到主分片和复制分片的translog之前，你的客户端不会得到一个200 OK响应。

在每次请求后都执行一个fsync会带来一些性能损失，尽管实践表明这种损失相对较小（特别是 bulk 导入，它在一次请求中平摊了大量文档的开销）。

但是对于一些大容量的偶尔丢失几秒数据问题也并不严重的集群，使用异步的 fsync还是比较有益的。比如，写入的数据被缓存到内存中，再每5秒执行一次 fsync 。如果你决定使用异步translog 的话，你需要保证在发生 crash 时，丢失掉 sync_interval时间段的数据也无所谓。请在决定前知晓这个特性。如果你不确定这个行为的后果，最好是使用默认的参数{“index.translog.durability”: “request”}来避免数据丢失。




在每次请求后都执行一个fsync会带来一些性能损失，尽管实践表明这种损失相对较小（特别是 bulk 导入，它在一次请求中平摊了大量文档的开销）。

但是对于一些大容量的偶尔丢失几秒数据问题也并不严重的集群，使用异步的 fsync还是比较有益的。比如，写入的数据被缓存到内存中，再每5秒执行一次 fsync 。如果你决定使用异步translog 的话，你需要保证在发生 crash 时，丢失掉 sync_interval时间段的数据也无所谓。请在决定前知晓这个特性。如果你不确定这个行为的后果，最好是使用默认的参数{“index.translog.durability”: “request”}来避免数据丢失。





### 段合并

由于自动刷新流程每秒会创建一个新的段，这样会导致短时间内的段数量暴增。而段数目太多会带来较大的麻烦。每一个段都会消耗文件句柄、内存和 cpu运行周期。更重要的是，每个搜索请求都必须轮流检查每个段；所以段越多，搜索也就越慢。

Elasticsearch通过在后台进行段合并来解决这个问题。小的段被合并到大的段，然后这些大的段再被合并到更大的段。

段合并的时候会将那些旧的已删除文档从文件系统中清除。被删除的文档（或被更新文档的旧版本）不会被拷贝到新的大段中。

启动段合并不需要你做任何事。进行索引和搜索时会自动进行。

一、当索引的时候，刷新（refresh）操作会创建新的段并将段打开以供搜索使用。

二、合并进程选择一小部分大小相似的段，并且在后台将它们合并到更大的段中。这并不会中断索引和搜索。



![img](C:\Users\32784\Desktop\笔记\image\c907ca35bd7c0393d46aec2c7038af19.png)

合并大的段需要消耗大量的 I/O 和 CPU 资源，如果任其发展会影响搜索性能。 Elasticsearch在默认情况下会对合并流程进行资源限制，所以搜索仍然有足够的资源很好地执行。







## 46-进阶-文档分析

分析包含下面的过程：

将一块文本分成适合于倒排索引的独立的词条。
将这些词条统一化为标准格式以提高它们的“可搜索性”，或者recall。
分析器执行上面的工作。分析器实际上是将三个功能封装到了一个包里：

字符过滤器：首先，字符串按顺序通过每个 字符过滤器 。他们的任务是在分词前整理字符串。一个字符过滤器可以用来去掉 HTML，或者将 & 转化成 and。
分词器：其次，字符串被分词器分为单个的词条。一个简单的分词器遇到空格和标点的时候，可能会将文本拆分成词条。
Token 过滤器：最后，词条按顺序通过每个 token 过滤器 。这个过程可能会改变词条（例如，小写化Quick ），删除词条（例如， 像 a， and， the 等无用词），或者增加词条（例如，像jump和leap这种同义词）
内置分析器
Elasticsearch还附带了可以直接使用的预包装的分析器。接下来我们会列出最重要的分析器。为了证明它们的差异，我们看看每个分析器会从下面的字符串得到哪些词条：

"Set the shape to semi-transparent by calling set_trans(5)"
1
标准分析器
标准分析器是Elasticsearch 默认使用的分析器。它是分析各种语言文本最常用的选择。它根据Unicode 联盟定义的单词边界划分文本。删除绝大部分标点。最后，将词条小写。它会产生：

set, the, shape, to, semi, transparent, by, calling, set_trans, 5
1
简单分析器
简单分析器在任何不是字母的地方分隔文本，将词条小写。它会产生：

set, the, shape, to, semi, transparent, by, calling, set, trans
1
空格分析器
空格分析器在空格的地方划分文本。它会产生:

Set, the, shape, to, semi-transparent, by, calling, set_trans(5)
1
语言分析器
特定语言分析器可用于很多语言。它们可以考虑指定语言的特点。例如，英语分析器附带了一组英语无用词（常用单词，例如and或者the ,它们对相关性没有多少影响），它们会被删除。由于理解英语语法的规则，这个分词器可以提取英语单词的词干。

英语分词器会产生下面的词条：

set, shape, semi, transpar, call, set_tran, 5
1
注意看transparent、calling和 set_trans已经变为词根格式。

分析器使用场景
当我们索引一个文档，它的全文域被分析成词条以用来创建倒排索引。但是，当我们在全文域搜索的时候，我们需要将查询字符串通过相同的分析过程，以保证我们搜索的词条格式与索引中的词条格式一致。

全文查询，理解每个域是如何定义的，因此它们可以做正确的事：

当你查询一个全文域时，会对查询字符串应用相同的分析器，以产生正确的搜索词条列表。

当你查询一个精确值域时，不会分析查询字符串，而是搜索你指定的精确值。

测试分析器
有些时候很难理解分词的过程和实际被存储到索引中的词条，特别是你刚接触Elasticsearch。为了理解发生了什么，你可以使用analyze API来看文本是如何被分析的。在消息体里，指定分析器和要分析的文本。

#GET http://localhost:9200/_analyze

```
{
    "analyzer": "standard",
    "text": "Text to analyze"
}

```

结果中每个元素代表一个单独的词条：

```
{
    "tokens": [
        {
            "token": "text", 
            "start_offset": 0, 
            "end_offset": 4, 
            "type": "<ALPHANUM>", 
            "position": 1
        }, 
        {
            "token": "to", 
            "start_offset": 5, 
            "end_offset": 7, 
            "type": "<ALPHANUM>", 
            "position": 2
        }, 
        {
            "token": "analyze", 
            "start_offset": 8, 
            "end_offset": 15, 
            "type": "<ALPHANUM>", 
            "position": 3
        }
    ]
}

```

token是实际存储到索引中的词条。
start_ offset 和end_ offset指明字符在原始字符串中的位置。
position指明词条在原始文本中出现的位置。
指定分析器
当Elasticsearch在你的文档中检测到一个新的字符串域，它会自动设置其为一个全文字符串域，使用 标准 分析器对它进行分析。你不希望总是这样。可能你想使用一个不同的分析器，适用于你的数据使用的语言。有时候你想要一个字符串域就是一个字符串域，不使用分析，直接索引你传入的精确值，例如用户 ID 或者一个内部的状态域或标签。要做到这一点，我们必须手动指定这些域的映射。

（细粒度指定分析器）

## IK分词器

首先通过 Postman 发送 GET 请求查询分词效果

GET http://localhost:9200/_analyze

{
	"text":"测试单词"
}

ES 的默认分词器无法识别中文中测试、 单词这样的词汇，而是简单的将每个字拆完分为一个词。

```
{
    "tokens": [
        {
            "token": "测", 
            "start_offset": 0, 
            "end_offset": 1, 
            "type": "<IDEOGRAPHIC>", 
            "position": 0
        }, 
        {
            "token": "试", 
            "start_offset": 1, 
            "end_offset": 2, 
            "type": "<IDEOGRAPHIC>", 
            "position": 1
        }, 
        {
            "token": "单", 
            "start_offset": 2, 
            "end_offset": 3, 
            "type": "<IDEOGRAPHIC>", 
            "position": 2
        }, 
        {
            "token": "词", 
            "start_offset": 3, 
            "end_offset": 4, 
            "type": "<IDEOGRAPHIC>", 
            "position": 3
        }
    ]
}
```



这样的结果显然不符合我们的使用要求，所以我们需要下载 ES 对应版本的中文分词器。

IK 中文分词器下载网址

将解压后的后的文件夹放入 ES 根目录下的 plugins 目录下，重启 ES 即可使用。

我们这次加入新的查询参数"analyzer":“ik_max_word”。

GET http://localhost:9200/_analyze

```
{
	"text":"测试单词",
	"analyzer":"ik_max_word"
}
```



<h4 style="color:red">
    ik_max_word：会将文本做最细粒度的拆分。</br>
    ik_smart：会将文本做最粗粒度的拆
</h4>




使用中文分词后的结果为：

```
{
    "tokens": [
        {
            "token": "测试", 
            "start_offset": 0, 
            "end_offset": 2, 
            "type": "CN_WORD", 
            "position": 0
        }, 
        {
            "token": "单词", 
            "start_offset": 2, 
            "end_offset": 4, 
            "type": "CN_WORD", 
            "position": 1
        }
    ]
}
```


18
ES 中也可以进行扩展词汇，首先查询

```
#GET http://localhost:9200/_analyze

{
    "text":"弗雷尔卓德",
    "analyzer":"ik_max_word"
}
```

仅仅可以得到每个字的分词结果，我们需要做的就是使分词器识别到弗雷尔卓德也是一个词语。

```
{
    "tokens": [
        {
            "token": "弗",
            "start_offset": 0,
            "end_offset": 1,
            "type": "CN_CHAR",
            "position": 0
        },
        {
            "token": "雷",
            "start_offset": 1,
            "end_offset": 2,
            "type": "CN_CHAR",
            "position": 1
        },
        {
            "token": "尔",
            "start_offset": 2,
            "end_offset": 3,
            "type": "CN_CHAR",
            "position": 2
        },
        {
            "token": "卓",
            "start_offset": 3,
            "end_offset": 4,
            "type": "CN_CHAR",
            "position": 3
        },
        {
            "token": "德",
            "start_offset": 4,
            "end_offset": 5,
            "type": "CN_CHAR",
            "position": 4
        }
    ]
}

```

首先进入 ES 根目录中的 plugins 文件夹下的 ik 文件夹，进入 config 目录，创建 custom.dic文件，写入“弗雷尔卓德”。
同时打开 IKAnalyzer.cfg.xml 文件，将新建的 custom.dic 配置其中。
重启 ES 服务器 。



```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
	<comment>IK Analyzer 扩展配置</comment>
	<!--用户可以在这里配置自己的扩展字典 -->
	<entry key="ext_dict">custom.dic</entry>
	 <!--用户可以在这里配置自己的扩展停止词字典-->
	<entry key="ext_stopwords"></entry>
	<!--用户可以在这里配置远程扩展字典 -->
	<!-- <entry key="remote_ext_dict">words_location</entry> -->
	<!--用户可以在这里配置远程扩展停止词字典-->
	<!-- <entry key="remote_ext_stopwords">words_location</entry> -->
</properties>

```

GET http://localhost:9200/_analyze

{
	"text":"弗雷尔卓德",
	"analyzer":"ik_max_word"
}

返回结果如下：

```
{
    "tokens": [
        {
            "token": "弗雷尔卓德",
            "start_offset": 0,
            "end_offset": 5,
            "type": "CN_WORD",
            "position": 0
        }
    ]
}

```



### 自定义分析器

虽然Elasticsearch带有一些现成的分析器，然而在分析器上Elasticsearch真正的强大之处在于，你可以通过在一个适合你的特定数据的设置之中组合字符过滤器、分词器、词汇单元过滤器来创建自定义的分析器。在分析与分析器我们说过，一个分析器就是在一个包里面组合了三种函数的一个包装器，三种函数按照顺序被执行：

字符过滤器
字符过滤器用来整理一个尚未被分词的字符串。例如，如果我们的文本是HTML格式的，它会包含像<p>或者<div>这样的HTML标签，这些标签是我们不想索引的。我们可以使用html清除字符过滤器来移除掉所有的HTML标签，并且像把&Aacute;转换为相对应的Unicode字符Á 这样，转换HTML实体。一个分析器可能有0个或者多个字符过滤器。

分词器
一个分析器必须有一个唯一的分词器。分词器把字符串分解成单个词条或者词汇单元。标准分析器里使用的标准分词器把一个字符串根据单词边界分解成单个词条，并且移除掉大部分的标点符号，然而还有其他不同行为的分词器存在。

例如，关键词分词器完整地输出接收到的同样的字符串，并不做任何分词。空格分词器只根据空格分割文本。正则分词器根据匹配正则表达式来分割文本。

词单元过滤器
经过分词，作为结果的词单元流会按照指定的顺序通过指定的词单元过滤器。词单元过滤器可以修改、添加或者移除词单元。我们已经提到过lowercase和stop词过滤器，但是在Elasticsearch 里面还有很多可供选择的词单元过滤器。词干过滤器把单词遏制为词干。ascii_folding过滤器移除变音符，把一个像"très”这样的词转换为“tres”。

ngram和 edge_ngram词单元过滤器可以产生适合用于部分匹配或者自动补全的词单元。

自定义分析器例子
接下来，我们看看如何创建自定义的分析器：

#PUT http://localhost:9200/my_index

```
{
    "settings": {
        "analysis": {
            "char_filter": {
                "&_to_and": {
                    "type": "mapping", 
                    "mappings": [
                        "&=> and "
                    ]
                }
            }, 
            "filter": {
                "my_stopwords": {
                    "type": "stop", 
                    "stopwords": [
                        "the", 
                        "a"
                    ]
                }
            }, 
            "analyzer": {
                "my_analyzer": {
                    "type": "custom", 
                    "char_filter": [
                        "html_strip", 
                        "&_to_and"
                    ], 
                    "tokenizer": "standard", 
                    "filter": [
                        "lowercase", 
                        "my_stopwords"
                    ]
                }
            }
        }
    }
}

```


索引被创建以后，使用 analyze API 来 测试这个新的分析器：

GET http://127.0.0.1:9200/my_index/_analyze

```
{
    "text":"The quick & brown fox",
    "analyzer": "my_analyzer"
}
1
2
3
4
5
返回结果为：

{
    "tokens": [
        {
            "token": "quick",
            "start_offset": 4,
            "end_offset": 9,
            "type": "<ALPHANUM>",
            "position": 1
        },
        {
            "token": "and",
            "start_offset": 10,
            "end_offset": 11,
            "type": "<ALPHANUM>",
            "position": 2
        },
        {
            "token": "brown",
            "start_offset": 12,
            "end_offset": 17,
            "type": "<ALPHANUM>",
            "position": 3
        },
        {
            "token": "fox",
            "start_offset": 18,
            "end_offset": 21,
            "type": "<ALPHANUM>",
            "position": 4
        }
    ]
}

```





## 47-进阶-文档控制

### 文档冲突

当我们使用index API更新文档，可以一次性读取原始文档，做我们的修改，然后重新索引整个文档。最近的索引请求将获胜：无论最后哪一个文档被索引，都将被唯一存储在 Elasticsearch 中。如果其他人同时更改这个文档，他们的更改将丢失。

很多时候这是没有问题的。也许我们的主数据存储是一个关系型数据库，我们只是将数据复制到Elasticsearch中并使其可被搜索。也许两个人同时更改相同的文档的几率很小。或者对于我们的业务来说偶尔丢失更改并不是很严重的问题。

但有时丢失了一个变更就是非常严重的。试想我们使用Elasticsearch 存储我们网上商城商品库存的数量，每次我们卖一个商品的时候，我们在 Elasticsearch 中将库存数量减少。有一天，管理层决定做一次促销。突然地，我们一秒要卖好几个商品。假设有两个web程序并行运行，每一个都同时处理所有商品的销售。

![img](C:\Users\32784\Desktop\笔记\image\49ca2ec50db3ddd0fcd1f364ac600b96.png)

web_1 对stock_count所做的更改已经丢失，因为 web_2不知道它的 stock_count的拷贝已经过期。结果我们会认为有超过商品的实际数量的库存，因为卖给顾客的库存商品并不存在，我们将让他们非常失望。

变更越频繁，读数据和更新数据的间隙越长，也就越可能丢失变更。在数据库领域中，有两种方法通常被用来确保并发更新时变更不会丢失：

悲观并发控制：这种方法被关系型数据库广泛使用，它假定有变更冲突可能发生，因此阻塞访问资源以防止冲突。一个典型的例子是读取一行数据之前先将其锁住，确保只有放置锁的线程能够对这行数据进行修改。
乐观并发控制：Elasticsearch 中使用的这种方法假定冲突是不可能发生的，并且不会阻塞正在尝试的操作。然而，如果源数据在读写当中被修改，更新将会失败。应用程序接下来将决定该如何解决冲突。例如，可以重试更新、使用新的数据、或者将相关情况报告给用户。
乐观并发控制
Elasticsearch是分布式的。当文档创建、更新或删除时，新版本的文档必须复制到集群中的其他节点。Elasticsearch也是异步和并发的，这意味着这些复制请求被并行发送，并且到达目的地时也许顺序是乱的。Elasticsearch需要一种方法确保文档的旧版本不会覆盖新的版本。

当我们之前讨论index , GET和DELETE请求时，我们指出每个文档都有一个_version（版本号），当文档被修改时版本号递增。Elasticsearch使用这个version号来确保变更以正确顺序得到执行。如果旧版本的文档在新版本之后到达，它可以被简单的忽略。

我们可以利用version号来确保应用中相互冲突的变更不会导致数据丢失。我们通过指定想要修改文档的 version号来达到这个目的。如果该版本不是当前版本号，我们的请求将会失败。

老的版本es使用version，但是新版本不支持了，会报下面的错误，提示我们用if_seq _no和if _primary_term

创建索引

#PUT http://127.0.0.1:9200/shopping/_create/1001
1
返回结果

```json
{
    "_index": "shopping",
    "_type": "_doc",
    "_id": "1001",
    "_version": 1,
    "result": "created",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 10,
    "_primary_term": 15
}
```


更新数据

#POST http://127.0.0.1:9200/shopping/_update/1001

```json
{
    "doc":{
        "title":"华为手机"
    }
}

```

返回结果：

```json
{
    "_index": "shopping",
    "_type": "_doc",
    "_id": "1001",
    "_version": 2,
    "result": "updated",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 11,
    "_primary_term": 15
}
```



旧版本使用的防止冲突更新方法：

#POST http://127.0.0.1:9200/shopping/_update/1001?version=1

```json
{
    "doc":{
        "title":"华为手机2"
    }
}
```


返回结果：

```json
{
    "error": {
        "root_cause": [
            {
                "type": "action_request_validation_exception",
                "reason": "Validation Failed: 1: internal versioning can not be used for optimistic concurrency control. Please use `if_seq_no` and `if_primary_term` instead;"
            }
        ],
        "type": "action_request_validation_exception",
        "reason": "Validation Failed: 1: internal versioning can not be used for optimistic concurrency control. Please use `if_seq_no` and `if_primary_term` instead;"
    },
    "status": 400
}
```


新版本使用的防止冲突更新方法：

#POST http://127.0.0.1:9200/shopping/_update/1001?if_seq_no=11&if_primary_term=15

```json
{
    "doc":{
        "title":"华为手机2"
    }
}

```

返回结果：

```json
{
    "_index": "shopping",
    "_type": "_doc",
    "_id": "1001",
    "_version": 3,
    "result": "updated",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 12,
    "_primary_term": 16
}

```

### 外部系统版本控制

一个常见的设置是使用其它数据库作为主要的数据存储，使用Elasticsearch做数据检索，这意味着主数据库的所有更改发生时都需要被复制到Elasticsearch，如果多个进程负责这一数据同步，你可能遇到类似于之前描述的并发问题。

如果你的主数据库已经有了版本号，或一个能作为版本号的字段值比如timestamp，那么你就可以在 Elasticsearch 中通过增加 version_type=extermal到查询字符串的方式重用这些相同的版本号，版本号必须是大于零的整数，且小于9.2E+18，一个Java中 long类型的正值。

外部版本号的处理方式和我们之前讨论的内部版本号的处理方式有些不同，Elasticsearch不是检查当前_version和请求中指定的版本号是否相同，而是检查当前_version是否小于指定的版本号。如果请求成功，外部的版本号作为文档的新_version进行存储。



#POST http://127.0.0.1:9200/shopping/_doc/1001?version=300&version_type=external

```json
{
	"title":"华为手机2"
}
```



返回结果：

```json
{
    "_index": "shopping",
    "_type": "_doc",
    "_id": "1001",
    "_version": 300,
    "result": "updated",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 13,
    "_primary_term": 16
}

```



## 第5章 Elasticsearch Spring Data集成



Spring Data是一个用于简化数据库、非关系型数据库、索引库访问，并支持云服务的开源框架。其主要目标是使得对数据的访问变得方便快捷，并支持 map-reduce框架和云计算数据服务。Spring Data可以极大的简化JPA(Elasticsearch…)的写法，可以在几乎不用写实现的情况下，实现对数据的访问和操作。除了CRUD 外，还包括如分页、排序等一些常用的功能。

Spring Data 的官网

Spring Data 常用的功能模块如下：

Spring Data JDBC
Spring Data JPA
Spring Data LDAP
Spring Data MongoDB
Spring Data Redis
Spring Data R2DBC
Spring Data REST
Spring Data for Apache Cassandra
Spring Data for Apache Geode
Spring Data for Apache Solr
Spring Data for Pivotal GemFire
Spring Data Couchbase
Spring Data Elasticsearch
Spring Data Envers
Spring Data Neo4j
Spring Data JDBC Extensions
Spring for Apache Hadoop
Spring Data Elasticsearch 介绍
Spring Data Elasticsearch基于Spring Data API简化 Elasticsearch 操作，将原始操作Elasticsearch 的客户端API进行封装。Spring Data为Elasticsearch 项目提供集成搜索引擎。Spring Data Elasticsearch POJO的关键功能区域为中心的模型与Elastichsearch交互文档和轻松地编写一个存储索引库数据访问层。





创建Maven项目。

二、修改pom文件，增加依赖关系。



    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.6.RELEASE</version>
        <relativePath/>
    </parent>
    
    <groupId>com.lun</groupId>
    <artifactId>SpringDataWithES</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    
    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-test</artifactId>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
        </dependency>
    </dependencies>
    </project>



三、增加配置文件。

在 resources 目录中增加application.properties文件

es 服务地址

elasticsearch.host=127.0.0.1

es 服务端口

elasticsearch.port=9200

配置日志级别,开启 debug 日志

logging.level.com.atguigu.es=debug

四、Spring Boot 主程序。



```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
@SpringBootApplication
public class MainApplication {
    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class, args);
    }
}
```

五、数据实体类。

```java
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.ToString;
import org.springframework.data.annotation.Id;
import org.springframework.data.elasticsearch.annotations.Document;
import org.springframework.data.elasticsearch.annotations.Field;
import org.springframework.data.elasticsearch.annotations.FieldType;

@Data
@NoArgsConstructor
@AllArgsConstructor
@ToString
@Document(indexName = "shopping", shards = 3, replicas = 1)
public class Product {
    //必须有 id,这里的 id 是全局唯一的标识，等同于 es 中的"_id"
    @Id
    private Long id;//商品唯一标识

    /**
     * type : 字段数据类型
     * analyzer : 分词器类型
     * index : 是否索引(默认:true)
     * Keyword : 短语,不进行分词
     */
    @Field(type = FieldType.Text, analyzer = "ik_max_word")
    private String title;//商品名称
    
    @Field(type = FieldType.Keyword)
    private String category;//分类名称
    
    @Field(type = FieldType.Double)
    private Double price;//商品价格
    
    @Field(type = FieldType.Keyword, index = false)
    private String images;//图片地址

}
```

ElasticsearchRestTemplate是spring-data-elasticsearch项目中的一个类,和其他spring项目中的 template类似。
在新版的spring-data-elasticsearch 中，ElasticsearchRestTemplate 代替了原来的ElasticsearchTemplate。
原因是ElasticsearchTemplate基于TransportClient，TransportClient即将在8.x 以后的版本中移除。所以，我们推荐使用ElasticsearchRestTemplate。
ElasticsearchRestTemplate基于RestHighLevelClient客户端的。需要自定义配置类，继承AbstractElasticsearchConfiguration，并实现elasticsearchClient()抽象方法，创建RestHighLevelClient对象。
AbstractElasticsearchConfiguration源码：



```java
package org.springframework.data.elasticsearch.config;
import org.elasticsearch.client.RestHighLevelClient;
import org.springframework.context.annotation.Bean;
import org.springframework.data.elasticsearch.core.ElasticsearchOperations;
import org.springframework.data.elasticsearch.core.ElasticsearchRestTemplate;
import org.springframework.data.elasticsearch.core.convert.ElasticsearchConverter;

/**

 * @author Christoph Strobl

 * @author Peter-Josef Meisch

 * @since 3.2

 * @see ElasticsearchConfigurationSupport
   */
   public abstract class AbstractElasticsearchConfiguration extends ElasticsearchConfigurationSupport {

   //需重写本方法

public abstract RestHighLevelClient elasticsearchClient();

@Bean(name = { "elasticsearchOperations", "elasticsearchTemplate" })
public ElasticsearchOperations elasticsearchOperations(ElasticsearchConverter elasticsearchConverter) {
	return new ElasticsearchRestTemplate(elasticsearchClient(), elasticsearchConverter);
}
}
```




需要自定义配置类，继承AbstractElasticsearchConfiguration，并实现elasticsearchClient()抽象方法，创建RestHighLevelClient对象。

```java
import lombok.Data;
import org.apache.http.HttpHost;
import org.elasticsearch.client.RestClient;
import org.elasticsearch.client.RestClientBuilder;
import org.elasticsearch.client.RestHighLevelClient;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.elasticsearch.config.AbstractElasticsearchConfiguration;

@ConfigurationProperties(prefix = "elasticsearch")
@Configuration
@Data
public class ElasticsearchConfig extends AbstractElasticsearchConfiguration{

    private String host ;
    private Integer port ;
    //重写父类方法
    @Override
    public RestHighLevelClient elasticsearchClient() {
        RestClientBuilder builder = RestClient.builder(new HttpHost(host, port));
        RestHighLevelClient restHighLevelClient = new
                RestHighLevelClient(builder);
        return restHighLevelClient;
    }

}
```



```java
import com.lun.model.Product;
import org.springframework.data.elasticsearch.repository.ElasticsearchRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface ProductDao extends ElasticsearchRepository<Product, Long>{

}
```

51-框架集成-SpringData-集成测试-索引操作

```java
import com.lun.model.Product;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.elasticsearch.core.ElasticsearchRestTemplate;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest
public class SpringDataESIndexTest {
    //注入 ElasticsearchRestTemplate
    @Autowired
    private ElasticsearchRestTemplate elasticsearchRestTemplate;
    //创建索引并增加映射配置
    @Test
    public void createIndex(){
        //创建索引，系统初始化会自动创建索引
        System.out.println("创建索引");
    }

    @Test
    public void deleteIndex(){
        //创建索引，系统初始化会自动创建索引
        boolean flg = elasticsearchRestTemplate.deleteIndex(Product.class);
        System.out.println("删除索引 = " + flg);
    }

}
```



用Postman 检测有没有创建和删除。

#GET http://localhost:9200/_cat/indices?v 
1
52-框架集成-SpringData-集成测试-文档操作

```java
import com.lun.dao.ProductDao;
import com.lun.model.Product;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Sort;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.ArrayList;
import java.util.List;

@RunWith(SpringRunner.class)
@SpringBootTest
public class SpringDataESProductDaoTest {

    @Autowired
    private ProductDao productDao;
    /**
     * 新增
     */
    @Test
    public void save(){
        Product product = new Product();
        product.setId(2L);
        product.setTitle("华为手机");
        product.setCategory("手机");
        product.setPrice(2999.0);
        product.setImages("http://www.atguigu/hw.jpg");
        productDao.save(product);
    }
    //POSTMAN, GET http://localhost:9200/product/_doc/2
    
    //修改
    @Test
    public void update(){
        Product product = new Product();
        product.setId(2L);
        product.setTitle("小米 2 手机");
        product.setCategory("手机");
        product.setPrice(9999.0);
        product.setImages("http://www.atguigu/xm.jpg");
        productDao.save(product);
    }
    //POSTMAN, GET http://localhost:9200/product/_doc/2


    //根据 id 查询
    @Test
    public void findById(){
        Product product = productDao.findById(2L).get();
        System.out.println(product);
    }
    
    @Test
    public void findAll(){
        Iterable<Product> products = productDao.findAll();
        for (Product product : products) {
            System.out.println(product);
        }
    }
    
    //删除
    @Test
    public void delete(){
        Product product = new Product();
        product.setId(2L);
        productDao.delete(product);
    }
    //POSTMAN, GET http://localhost:9200/product/_doc/2
    
    //批量新增
    @Test
    public void saveAll(){
        List<Product> productList = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
            Product product = new Product();
            product.setId(Long.valueOf(i));
            product.setTitle("["+i+"]小米手机");
            product.setCategory("手机");
            product.setPrice(1999.0 + i);
            product.setImages("http://www.atguigu/xm.jpg");
            productList.add(product);
        }
        productDao.saveAll(productList);
    }
    
    //分页查询
    @Test
    public void findByPageable(){
        //设置排序(排序方式，正序还是倒序，排序的 id)
        Sort sort = Sort.by(Sort.Direction.DESC,"id");
        int currentPage=0;//当前页，第一页从 0 开始， 1 表示第二页
        int pageSize = 5;//每页显示多少条
        //设置查询分页
        PageRequest pageRequest = PageRequest.of(currentPage, pageSize,sort);
        //分页查询
        Page<Product> productPage = productDao.findAll(pageRequest);
        for (Product Product : productPage.getContent()) {
            System.out.println(Product);
        }
    }

}
```

53-框架集成-SpringData-集成测试-文档搜索

```java
import com.lun.dao.ProductDao;
import com.lun.model.Product;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.index.query.TermQueryBuilder;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.domain.PageRequest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest
public class SpringDataESSearchTest {

@Autowired
private ProductDao productDao;
/**
 * term 查询
 * search(termQueryBuilder) 调用搜索方法，参数查询构建器对象
 */
@Test
public void termQuery(){
    TermQueryBuilder termQueryBuilder = QueryBuilders.termQuery("title", "小米");
            Iterable<Product> products = productDao.search(termQueryBuilder);
    for (Product product : products) {
        System.out.println(product);
    }
}
/**
 * term 查询加分页
 */
@Test
public void termQueryByPage(){
    int currentPage= 0 ;
    int pageSize = 5;
    //设置查询分页
    PageRequest pageRequest = PageRequest.of(currentPage, pageSize);
    TermQueryBuilder termQueryBuilder = QueryBuilders.termQuery("title", "小米");
            Iterable<Product> products =
                    productDao.search(termQueryBuilder,pageRequest);
    for (Product product : products) {
        System.out.println(product);
    }
}
}
```





## 第6章 Elasticsearch优化

56-优化-硬件选择
Elasticsearch 的基础是 Lucene，所有的索引和文档数据是存储在本地的磁盘中，具体的路径可在 ES 的配置文件…/config/elasticsearch.yml中配置，如下：

#

Path to directory where to store the data (separate multiple locations by comma):

#
path.data: /path/to/data
#

Path to log files:

#
path.logs: /path/to/logs


磁盘在现代服务器上通常都是瓶颈。Elasticsearch重度使用磁盘，你的磁盘能处理的吞吐量越大，你的节点就越稳定。这里有一些优化磁盘I/O的技巧：

使用SSD就像其他地方提过的，他们比机械磁盘优秀多了。
使用RAID0。条带化RAID会提高磁盘IO，代价显然就是当一块硬盘故障时整个就故障了。不要使用镜像或者奇偶校验RAID，因为副本已经提供了这个功能。
另外，使用多块硬盘，并允许Elasticsearch 通过多个path data目录配置把数据条带化分配到它们上面。
不要使用远程挂载的存储，比如NFS或者SMB/CIFS。这个引入的延迟对性能来说完全是背道而驰的。
57-优化-分片策略
合理设置分片数
分片和副本的设计为 ES 提供了支持分布式和故障转移的特性，但并不意味着分片和副本是可以无限分配的。而且索引的分片完成分配后由于索引的路由机制，我们是不能重新修改分片数的。

可能有人会说，我不知道这个索引将来会变得多大，并且过后我也不能更改索引的大小，所以为了保险起见，还是给它设为 1000 个分片吧。但是需要知道的是，一个分片并不是没有代价的。需要了解：

一个分片的底层即为一个 Lucene 索引，会消耗一定文件句柄、内存、以及 CPU运转。

每一个搜索请求都需要命中索引中的每一个分片，如果每一个分片都处于不同的节点还好， 但如果多个分片都需要在同一个节点上竞争使用相同的资源就有些糟糕了。

用于计算相关度的词项统计信息是基于分片的。如果有许多分片，每一个都只有很少的数据会导致很低的相关度。

一个业务索引具体需要分配多少分片可能需要架构师和技术人员对业务的增长有个预先的判断，横向扩展应当分阶段进行。为下一阶段准备好足够的资源。 只有当你进入到下一个阶段，你才有时间思考需要作出哪些改变来达到这个阶段。一般来说，我们遵循一些原则：

控制每个分片占用的硬盘容量不超过 ES 的最大 JVM 的堆空间设置（一般设置不超过 32G，参考下文的 JVM 设置原则），因此，如果索引的总容量在 500G 左右，那分片大小在 16 个左右即可；当然，最好同时考虑原则 2。
考虑一下 node 数量，一般一个节点有时候就是一台物理机，如果分片数过多，大大超过了节点数，很可能会导致一个节点上存在多个分片，一旦该节点故障，即使保持了 1 个以上的副本，同样有可能会导致数据丢失，集群无法恢复。所以， 一般都设置分片数不超过节点数的 3 倍。
主分片，副本和节点最大数之间数量，我们分配的时候可以参考以下关系：

**节点数<=主分片数 *（副本数+1）**

推迟分片分配
对于节点瞬时中断的问题，默认情况，集群会等待一分钟来查看节点是否会重新加入，如果这个节点在此期间重新加入，重新加入的节点会保持其现有的分片数据，不会触发新的分片分配。这样就可以减少 ES 在自动再平衡可用分片时所带来的极大开销。

通过修改参数 delayed_timeout ，可以延长再均衡的时间，可以全局设置也可以在索引级别进行修改：

#PUT /_all/_settings

```
{
	"settings": {
		"index.unassigned.node_left.delayed_timeout": "5m"
	}
}
```



### 58-优化-路由选择

当我们查询文档的时候， Elasticsearch 如何知道一个文档应该存放到哪个分片中呢？它其实是通过下面这个公式来计算出来：

shard = hash(routing) % number_of_primary_shards
1
routing 默认值是文档的 id，也可以采用自定义值，比如用户 id。

不带routing查询
在查询的时候因为不知道要查询的数据具体在哪个分片上，所以整个过程分为2个步骤

分发：请求到达协调节点后，协调节点将查询请求分发到每个分片上。
聚合：协调节点搜集到每个分片上查询结果，在将查询的结果进行排序，之后给用户返回结果。
带routing查询
查询的时候，可以直接根据routing 信息定位到某个分配查询，不需要查询所有的分配，经过协调节点排序。向上面自定义的用户查询，如果routing 设置为userid 的话，就可以直接查询出数据来，效率提升很多。

### 59-优化-写入速度优化

ES 的默认配置，是综合了数据可靠性、写入速度、搜索实时性等因素。实际使用时，我们需要根据公司要求，进行偏向性的优化。

针对于搜索性能要求不高，但是对写入要求较高的场景，我们需要尽可能的选择恰当写优化策略。综合来说，可以考虑以下几个方面来提升写索引的性能：

加大Translog Flush，目的是降低Iops、Writeblock。
增加Index Refesh间隔，目的是减少Segment Merge的次数。
调整Bulk 线程池和队列。
优化节点间的任务分布。
优化Lucene层的索引建立，目的是降低CPU及IO。
优化存储设备
ES 是一种密集使用磁盘的应用，在段合并的时候会频繁操作磁盘，所以对磁盘要求较高，当磁盘速度提升之后，集群的整体性能会大幅度提高。

### 合理使用合并

Lucene 以段的形式存储数据。当有新的数据写入索引时， Lucene 就会自动创建一个新的段。

随着数据量的变化，段的数量会越来越多，消耗的多文件句柄数及 CPU 就越多，查询效率就会下降。

由于 Lucene 段合并的计算量庞大，会消耗大量的 I/O，所以 ES 默认采用较保守的策略，让后台定期进行段合并。

减少 Refresh 的次数
Lucene 在新增数据时，采用了延迟写入的策略，默认情况下索引的refresh_interval 为1 秒。

Lucene 将待写入的数据先写到内存中，超过 1 秒（默认）时就会触发一次 Refresh，然后 Refresh 会把内存中的的数据刷新到操作系统的文件缓存系统中。

如果我们对搜索的实效性要求不高，可以将 Refresh 周期延长，例如 30 秒。

这样还可以有效地减少段刷新次数，但这同时意味着需要消耗更多的 Heap 内存。

加大 Flush 设置
Flush 的主要目的是把文件缓存系统中的段持久化到硬盘，当 Translog 的数据量达到 512MB 或者 30 分钟时，会触发一次 Flush。

index.translog.flush_threshold_size 参数的默认值是 512MB，我们进行修改。

增加参数值意味着文件缓存系统中可能需要存储更多的数据，所以我们需要为操作系统的文件缓存系统留下足够的空间。

减少副本的数量
ES 为了保证集群的可用性，提供了 Replicas（副本）支持，然而每个副本也会执行分析、索引及可能的合并过程，所以 Replicas 的数量会严重影响写索引的效率。

当写索引时，需要把写入的数据都同步到副本节点，副本节点越多，写索引的效率就越慢。

如果我们需要大批量进行写入操作，可以先禁止Replica复制，设置
index.number_of_replicas: 0 关闭副本。在写入完成后， Replica 修改回正常的状态。

### 60-优化-内存设置

ES 默认安装后设置的内存是 1GB，对于任何一个现实业务来说，这个设置都太小了。如果是通过解压安装的 ES，则在 ES 安装文件中包含一个 jvm.option 文件，添加如下命令来设置 ES 的堆大小， Xms 表示堆的初始大小， Xmx 表示可分配的最大内存，都是 1GB。

确保 Xmx 和 Xms 的大小是相同的，其目的是为了能够在 Java 垃圾回收机制清理完堆区后不需要重新分隔计算堆区的大小而浪费资源，可以减轻伸缩堆大小带来的压力。

假设你有一个 64G 内存的机器，按照正常思维思考，你可能会认为把 64G 内存都给ES 比较好，但现实是这样吗， 越大越好？虽然内存对 ES 来说是非常重要的，但是答案是否定的！

因为 ES 堆内存的分配需要满足以下两个原则：

不要超过物理内存的 50%： Lucene 的设计目的是把底层 OS 里的数据缓存到内存中。Lucene 的段是分别存储到单个文件中的，这些文件都是不会变化的，所以很利于缓存，同时操作系统也会把这些段文件缓存起来，以便更快的访问。如果我们设置的堆内存过大， Lucene 可用的内存将会减少，就会严重影响降低 Lucene 的全文本查询性能。

堆内存的大小最好不要超过 32GB：在 Java 中，所有对象都分配在堆上，然后有一个 Klass Pointer 指针指向它的类元数据。这个指针在 64 位的操作系统上为 64 位， 64 位的操作系统可以使用更多的内存（2^64）。在 32 位
的系统上为 32 位， 32 位的操作系统的最大寻址空间为 4GB（2^32）。
但是 64 位的指针意味着更大的浪费，因为你的指针本身大了。浪费内存不算，更糟糕的是，更大的指针在主内存和缓存器（例如 LLC, L1 等）之间移动数据的时候，会占用更多的带宽。

最终我们都会采用 31 G 设置

-Xms 31g
-Xmx 31g
假设你有个机器有 128 GB 的内存，你可以创建两个节点，每个节点内存分配不超过 32 GB。也就是说不超过 64 GB 内存给 ES 的堆内存，剩下的超过 64 GB 的内存给 Lucene。



<div class="table-box"><table><thead><tr><th>参数名</th><th>参数值</th><th>说明</th></tr></thead><tbody><tr><td>cluster.name</td><td>elasticsearch</td><td>配置 ES 的集群名称，默认值是 ES，建议改成与所存数据相关的名称， ES 会自动发现在同一网段下的 集群名称相同的节点。</td></tr><tr><td>node.name</td><td>node-1</td><td>集群中的节点名，在同一个集群中不能重复。节点 的名称一旦设置，就不能再改变了。当然，也可以 设 置 成 服 务 器 的 主 机 名 称 ， 例 如 node.name:${HOSTNAME}。</td></tr><tr><td>node.master</td><td>true</td><td>指定该节点是否有资格被选举成为 Master 节点，默 认是 True，如果被设置为 True，则只是有资格成为 Master 节点，具体能否成为 Master 节点，需要通 过选举产生。</td></tr><tr><td>node.data</td><td>true</td><td>指定该节点是否存储索引数据，默认为 True。数据 的增、删、改、查都是在 Data 节点完成的。</td></tr><tr><td>index.number_of_shards</td><td>1</td><td>设置都索引分片个数，默认是 1 片。也可以在创建 索引时设置该值，具体设置为多大都值要根据数据 量的大小来定。如果数据量不大，则设置成 1 时效 率最高</td></tr><tr><td>index.number_of_replicas</td><td>1</td><td>设置默认的索引副本个数，默认为 1 个。副本数越 多，集群的可用性越好，但是写索引时需要同步的 数据越多。</td></tr><tr><td>transport.tcp.compress</td><td>true</td><td>设置在节点间传输数据时是否压缩，默认为 False， 不压缩</td></tr><tr><td>discovery.zen.minimum_master_nodes</td><td>1</td><td>设置在选举 Master 节点时需要参与的最少的候选 主节点数，默认为 1。如果使用默认值，则当网络 不稳定时有可能会出现脑裂。 合 理 的 数 值 为 (master_eligible_nodes/2)+1 ， 其 中 master_eligible_nodes 表示集群中的候选主节点数</td></tr><tr><td>discovery.zen.ping.timeout</td><td>3s</td><td>设置在集群中自动发现其他节点时 Ping 连接的超 时时间，默认为 3 秒。 在较差的网络环境下需要设置得大一点，防止因误 判该节点的存活状态而导致分片的转移</td></tr></tbody></table></div>









# ES设计



索引设计的重要性
好的索引设计在整个集群规划中占据举足轻重的作用，索引的设计直接影响集群设计的好坏和复杂度。必须充分结合业务场景的时间维度和空间维度，结合业务场景充分考量增、删、改、查等全维度设计的。完全基于“设计先行，编码在后”的原则，前期会花很长时间，为的是后期工作更加顺畅，避免不必要的返工。

PB 级别的大索引如何设计
说明
单纯的普通数据索引，如果不考虑增量数据，基本上普通索引就能够满足性能要求。

我们通常的操作就是：

步骤 1：创建索引；

步骤 2：导入或者写入数据；

步骤 3：提供查询请求访问或者查询服务。

大索引的缺陷
如果每天亿万+的实时增量数据，单个索引是无法满足要求的。

存储大小限制
单个分片（Shard）实际是 Lucene 的索引，单分片能存储的最大文档数是：2,147,483,519 (= Integer.MAX_VALUE - 128)。如下命令能查看全部索引的分隔分片的文档大小：

 

```
GET _cat/shards
app_index                       2 p STARTED      9443   2.8mb 127.0.0.1 Hk9wFwU
app_index                       2 r UNASSIGNED                          
app_index                       3 p STARTED      9462   2.7mb 127.0.0.1 Hk9wFwU
app_index                       3 r UNASSIGNED                          
app_index                       4 p STARTED      9520   3.5mb 127.0.0.1 Hk9wFwU
app_index                       4 r UNASSIGNED                          
app_index                       1 p STARTED      9453   2.4mb 127.0.0.1 Hk9wFwU
app_index                       1 r UNASSIGNED                          
app_index                       0 p STARTED      9365   2.3mb 127.0.0.1 Hk9wFwU
app_index                       0 r UNASSIGNED

```

性能
当然一个索引很大的话，数据写入和查询性能都会变差。

而高效检索体现在：基于日期的检索可以直接检索对应日期的索引，无形中缩减了很大的数据规模。

比如检索：“2021-02-01”号的数据，之前的检索会是在一个月甚至更大体量的索引中进行。

现在直接检索"index_2021-02-01"的索引,效率提升好几倍。

风险
一旦一个大索引出现故障，相关的数据都会受到影响。而分成滚动索引的话，相当于做了物理隔离。

PB 级索引设计
大索引设计建议：使用模板+Rollover+Curator动态创建索引。动态索引使用效果如下：

 

```
index_2019-01-01-000001
index_2019-01-02-000002
index_2019-01-03-000003
index_2019-01-04-000004
index_2019-01-05-000005

```

使用模板统一配置索引
统一管理索引，相关索引字段完全一致。

使用 Rollver 增量管理索引
目的：按照日期、文档数、文档存储大小三个维度进行更新索引。使用举例：

 

POST /logs_write/_rollover 

```json
{
  "conditions": {
    "max_age":   "7d",
    "max_docs":  1000,
    "max_size":  "5gb"
  }
}

```

![在这里插入图片描述](C:\Users\32784\Desktop\笔记\image\watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdWVyY2hvbmc=,size_16,color_FFFFFF,t_70.png)



索引更新的时机是：当原始索引满足设置条件的三个中的一个的时候，就会更新为新的索引。为保证业务的全索引检索，一般采用别名机制。

在索引模板设计阶段，模板定义一个全局别名：用途是全局检索，如图所示的别名：indexall。每次更新到新的索引后，新索引指向一个用于实时新数据写入的别名，如图所示的别名：indexlatest。同时将旧索引的别名 index_latest 移除。
别名删除和新增操作举例：

```
POST /_aliases
{
  "actions" : [
      { "remove" : { "index" : "index_2019-01-01-000001", "alias" : "index_latest" } },
      { "add" : { "index" : "index_2019-01-02-000002", "alias" : "index_latest" } }
  ]
}


```



使用 curator 高效清理历史数据
目的：按照日期定期删除、归档历史数据。

一个大索引的数据删除方式只能使用 delete_by_query，由于 ES 中使用更新版本机制。删除索引后，由于没有物理删除，磁盘存储信息会不减反增。有同学就反馈 500GB+ 的索引 delete_by_query 导致负载增高的情况。

而按照日期划分索引后，不需要的历史数据可以做如下的处理。

删除——对应 delete 索引操作。

压缩——对应 shrink 操作。

段合并——对应 force_merge 操作。

而这一切，可以借助：curator 工具通过简单的配置文件结合定义任务 crontab 一键实现。

注意：7.X高版本借助iLM实现更为简单。

举例，一键删除 30 天前的历史数据：




```
[root@localhost .curator]# cat action.yml 
  actions:
      1:
        action: delete_indices
        description: >-
          Delete indices older than 30 days (based on index name), for logstash-
          prefixed indices. Ignore the error if the filter does not result in an
          actionable list of indices (ignore_empty_list) and exit cleanly.
        options:
          ignore_empty_list: True
          disable_action: False
        filters:
        - filtertype: pattern
          kind: prefix
          value: logs_
        - filtertype: age
          source: name
          direction: older
          timestring: '%Y.%m.%d'
          unit: days
          unit_count: 30

```



如何设计分片数和副本数
分片/副本
1、分片：分片本身都是一个功能齐全且独立的“索引”，可以托管在集群中的任何节点上。

数据切分分片的主要目的：

（1）水平分割/缩放内容量 。

（2）跨分片（可能在多个节点上）分布和并行化操作，提高性能/吞吐量。

注意：分片一旦创建，不可以修改大小。

2、副本：它在分片/节点出现故障时提供高可用性。

副本的好处：因为可以在所有副本上并行执行搜索——因此扩展了搜索量/吞吐量。

注意：副本分片与主分片存储在集群中不同的节点。副本的大小可以通过：number_of_replicas动态修改。

分片和副本实战中设计
索引设置多少分片
Shard 大小官方推荐值为 20-40GB, 具体原理呢？Elasticsearch 员工 Medcl 曾经讨论如下：

Lucene 底层没有这个大小的限制，20-40GB 的这个区间范围本身就比较大，经验值有时候就是拍脑袋，不一定都好使。

Elasticsearch 对数据的隔离和迁移是以分片为单位进行的，分片太大，会加大迁移成本。

一个分片就是一个 Lucene 的库，一个 Lucene 目录里面包含很多 Segment，每个 Segment 有文档数的上限，Segment 内部的文档 ID 目前使用的是 Java 的整型，也就是 2 的 31 次方，所以能够表示的总的文档数为Integer.MAXVALUE - 128 = 2^31 - 128 = 2147483647 - 1 = 2,147,483,519，也就是21.4亿条。

同样，如果你不 forcemerge 成一个 Segment，单个 shard 的文档数能超过这个数。

单个 Lucene 越大，索引会越大，查询的操作成本自然要越高，IO 压力越大，自然会影响查询体验。

具体一个分片多少数据合适，还是需要结合实际的业务数据和实际的查询来进行测试以进行评估。

综合实战+网上各种经验分享，梳理如下：

第一步：预估一下数据量的规模。一共要存储多久的数据，每天新增多少数据？两者的乘积就是总数据量。

第二步：预估分多少个索引存储。索引的划分可以根据业务需要。

第三步：考虑和衡量可扩展性，预估需要搭建几台机器的集群。存储主要看磁盘空间，假设每台机器2TB，可用：2TB0.85(磁盘实际利用率）0.85(ES 警戒水位线）。

第四步：单分片的大小建议最大设置为 30GB。此处如果是增量索引，可以结合大索引的设计部分的实现一起规划。

前三步能得出一个索引的大小。分片数考虑维度：

1）分片数 = 索引大小/分片大小经验值 30GB 。

2）分片数建议和节点数一致。设计的时候1）、2）两者权衡考虑+rollover 动态更新索引结合。

每个 shard 大小是按照经验值 30G 到 50G，因为在这个范围内查询和写入性能较好。


索引设置多少副本

结合集群的规模，对于集群数据节点 >=2 的场景：建议副本至少设置为 1。

注意：

单节点的机器设置了副本也不会生效的。副本数的设计结合数据的安全需要。对于数据安全性要求非常高的业务场景，建议做好：增强备份（结合 ES 官方备份方案）。



mapping 如何设计
Mapping
Mapping 是定义文档及其包含的字段的存储和索引方式的过程。例如，使用映射来定义：

应将哪些字符串字段定义为全文检索字段；

哪些字段包含数字，日期或地理位置；

定义日期值的格式（时间戳还是日期类型等）；

用于控制动态添加字段的映射的自定义规则。

设计 Mapping 的注意事项
ES 支持增加字段 //新增字段

```
PUT new_index
  {
    "mappings": {
      "_doc": {
        "properties": {
          "status_code": {
            "type":       "keyword"
          }
        }
      }
    }
  }

```


ES 不支持直接删除字段

ES 不支持直接修改字段

ES 不支持直接修改字段类型 如果非要做灵活设计，ES 有其他方案可以替换，借助reindex。但是数据量大会有性能问题，建议设计阶段综合权衡考虑。

Mapping 字段的设置流程
索引分为静态 Mapping（自定义字段）+动态 Mapping（ES 自动根据导入数据适配）。

实战业务场景建议：选用静态 Mapping，根据业务类型自己定义字段类型。

好处：

可控；

节省存储空间（默认 string 是 text+keyword，实际业务不一定需要）。

设置字段的时候，务必过一下如下图示的流程。根据实际业务需要，主要关注点：


![在这里插入图片描述](C:\Users\32784\Desktop\笔记\image\watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdWVyY2hvbmc=,size_16,color_FFFFFF,t_70-17001048252753.png)





数据类型选型；

是否需要检索；

是否需要排序+聚合分析；

是否需要另行存储。

Mapping 建议结合模板定义
索引 Templates——索引模板允许您定义在创建新索引时自动应用的模板。模板包括settings和Mappings以及控制是否应将模板应用于新索引。

注意：模板仅在索引创建时应用。更改模板不会对现有索引产生影响。

第1部分也有说明，针对大索引，使用模板是必须的。核心需要设置的setting（仅列举了实战中最常用、可以动态修改的）如下：

index.numberofreplicas 每个主分片具有的副本数。默认为 1（7.X 版本，低于 7.X 为 5）。

index.maxresultwindow 深度分页 rom + size 的最大值—— 默认为 10000。

index.refresh_interval 默认 1s：代表最快 1s 搜索可见；

写入时候建议设置为 -1，提高写入性能；

实战业务如果对实时性要求不高，建议设置为 30s 或者更高。

包含 Mapping 的 template 设计万能模板

```json
PUT _template/test_template
{
  "index_patterns": [
    "test_index_*",
    "test_*"
  ],
  "settings": {
    "number_of_shards": 1, // 分片数
    "number_of_replicas": 1, // 副本数
    "max_result_window": 100000, //max_result_window: 表示一次查询最多可以返回 100,000 个文档
    "refresh_interval": "30s" // refresh_interval表示刷入系统文件缓冲区的时间，默认1s，对近实时性要求不高的可以选择30-60s
  },
  "mappings": {
    "properties": {
      "id": {
        "type": "long"
      },
      "title": {
        "type": "keyword"
      },
      "content": {
        "analyzer": "ik_max_word", // ik分词器
        "type": "text",
        "fields": { //fields 是一个映射定义中的关键字，用于指定为一个字段创建多个不同的表示形式，既可以是text分词查询也可以是keword精准查询
          "keyword": {
            "ignore_above": 256,
            "type": "keyword"
          }
        }
      },
      "available": {
        "type": "boolean"
      },
      "review": {//  表示 "review" 字段的数据类型是嵌套类型。嵌套类型允许在一个文档中嵌套另一个文档，这样可以建模复杂的结构，比如嵌套的对象或数组。
        "type": "nested",
        "properties": {
          "nickname": { //"nickname": 表示评论的昵称，数据类型是 "text"
            "type": "text"
          },
          "text": { // "text": 表示评论的文本内容，同样是 "text" 类型。
            "type": "text"
          },
          "stars": { // "stars": 表示评论的星级评分，数据类型是 "integer"。
            "type": "integer"
          }
        }
      },
      "publish_time": {
        "type": "date",
        "format": "yyyy-MM-dd HH:mm:ss||yyyy-MM-dd||epoch_millis"
      },
      "expected_attendees": {
        "type": "integer_range"
      },
      "ip_addr": { // 字段的映射定义指定了该字段的数据类型为 "ip"。这表示该字段用于存储 IP 地址。
        "type": "ip"
      },
      "suggest": { // 
        "type": "completion" // : "completion" 是 Elasticsearch 映射中的字段类型设置，用于定义自动补全（Autocomplete）字Completion suggester 是 Elasticsearch 提供的一个特殊类型的字段，用于支持自动补全和搜索建议功能。
      }
    }
  }
}
```









### 分词的选型

主要以 ik 来说明，最新版本的ik支持两种类型。ik_maxword 细粒度匹配，适用切分非常细的场景。ik_smart 粗粒度匹配，适用切分粗的场景。

分词选型
实际业务中：建议适用ik_max_word分词 + match_phrase短语检索。

原因：ik_smart有覆盖不全的情况，数据量大了以后，即便 reindex 能满足要求，但面对极大的索引的情况，reindex 的耗时我们承担不起。建议ik_max_word一步到位。

ik 要装集群的所有机器
安装在集群的所有节点上。

ik 匹配不到怎么办
方案1：扩充 ik 开源自带的词库+动态更新词库；原生的词库分词数量级很小，基础词库尽量更大更全，网上搜索一下“搜狗词库“。

动态更新词库：可以结合 mysql+ik 自带的更新词库的方式动态更新词库。

更新词库仅对新创建的索引生效，部分老数据索引建议使用 reindex 升级处理。

方案2：采用字词混合索引的方式，避免“明明存在，但是检索不到的”场景。

### 检索类型选型

前提：5.X 版本之后，string 类型不再存在，取代的是text和keyword类型。

text 类型作用：分词，将大段的文字根据分词器切分成独立的词或者词组，以便全文检索。

适用于：email 内容、某产品的描述等需要分词全文检索的字段；

不适用：排序或聚合（Significant Terms 聚合例外）

keyword 类型：无需分词、整段完整精确匹配。

适用于：email 地址、住址、状态码、分类 tags。

以一个实战例子说明：

```json
PUT zz_test
{
  "mappings": {
          "doc": {
    "properties": {
        "title": {
          "type": "text",
          "analyzer":"ik_max_word",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        }
      }
    }
  }
}
```
```json
GET zz_test/_mapping

PUT zz_test/doc/1
{
  "title":"锤子加湿器官方致歉,难产后临时推迟一个月发货遭diss耍流氓"
}
```

 



```json
POST zz_test/_analyze
{
  "text": "锤子加湿器官方致歉,难产后临时推迟一个月发货遭diss耍流氓",
  "analyzer": "ik_max_word"
}

```


ik_max_word的分词结果如下：

锤子、锤、子、加湿器、湿、器官、官方、方、致歉、致、歉、难产、产后、后、临时、临、时、推迟、迟、一个、 一个、 一、个月、 个、 月、 发货、发、货、遭、diss、耍流氓、耍、流氓、氓。

term 精确匹配
核心功能：不受到分词器的影响，属于完整的精确匹配。

应用场景：精确、精准匹配。

适用类型：keyword。

举例：term 最适合匹配的类型是 keyword，如下所示的精确完整匹配：

```json
POST zz_test/_search
{
  "query": {
    "term": {
      "title.keyword": "锤子加湿器官方致歉,难产后临时推迟一个月发货遭diss耍流氓"
    }
  }
}
```

注意：如下是匹配不到结果的。

POST zz_test/_search

```json
{
  "query": {
    "term": {
      "title": "锤子加湿器"
    }
  }
}

```

原因：对于 title 中的锤子加湿器，term 不会做分词拆分匹配的。且 ik_max_word 分词也是没有“锤子加湿器”这组关键词的。

prefix 前缀匹配
核心功能：前缀匹配。

应用场景：前缀自动补全的业务场景。


![在这里插入图片描述](C:\Users\32784\Desktop\笔记\image\watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdWVyY2hvbmc=,size_16,color_FFFFFF,t_70-17001050015936.png)



适用类型：keyword。

如下能匹配到文档 id 为 1 的文章。

```json
POST zz_test/_search
{
  "query": {
    "prefix": {
      "title.keyword": "锤子加湿器"
    }
  }
}
```





wildcard 模糊匹配
核心功能：匹配具有匹配通配符表达式 keyword 类型的文档。支持的通配符：*，它匹配任何字符序列（包括空字符序列）；？，它匹配任何单个字符。

应用场景：请注意，选型务必要慎重！此查询可能很慢多组关键次的情况下可能会导致宕机，因为它需要遍历多个术语。为了防止非常慢的通配符查询，通配符不能以任何一个通配符*或？开头。

适用类型：keyword。

如下匹配，类似 MySQL 中的通配符匹配，能匹配所有包含加湿器的文章。

```json
POST zz_test/_search
{
  "query": {
    "wildcard": {
      "title.keyword": "*加湿器*"
    }
  }
}

```

match 分词匹配
核心功能：全文检索，分词词项匹配。

应用场景：实际业务中较少使用，原因：匹配范围太宽泛，不够准确。

适用类型：text。

如下示例，title 包含"锤子"和“加湿器”的都会被检索到。

```json

POST zz_test/_search
{
  "profile": true, 
  "query": {
    "match": {
      "title": "锤子加湿器"
    }
  }
}

```



match_phrase 短语匹配
核心功能：match_phrase 查询首先将查询字符串解析成一个词项列表，然后对这些词项进行搜索; 只保留那些包含 全部 搜索词项，且 位置"position" 与搜索词项相同的文档。

应用场景：业务开发中 90%+ 的全文检索都会使用 match_phrase 或者 query_string 类型，而不是 match。

适用类型：text。

注意：

```json
POST zz_test/_analyze
{
  "text": "锤子加湿器",
  "analyzer": "ik_max_word"
}

```

分词结果：

锤子， 锤，子， 加湿器， 湿，器。而：id为1的文档的分词结果：锤子, 锤, 子, 加湿器, 湿, 器官。所以，如下的检索是匹配不到结果的。

```json
POST zz_test/_search
{
"query": {
  "match_phrase": {
    "title": "锤子加湿器"
  }
}
}
```


如果想匹配到，怎么办呢？这里可以字词组合索引的形式。

multi_match 多组匹配
核心功能：match query 针对多字段的升级版本。

应用场景：多字段检索。

适用类型：text。

举例：

```json
POST zz_test/_search
{
  "query": {
    "multi_match": {
      "query": "加湿器",
      "fields": [
        "title",
        "content"
      ]
    }
  }
}

```

query_string 类型
核心功能：支持与或非表达式+其他N多配置参数。

应用场景：业务系统需要支持自定义表达式检索。

适用类型：text。

```json
POST zz_test/_search
{
  "query": {
    "query_string": {
      "default_field": "title",
      "query": "(锤子 AND 加湿器) OR (官方 AND 道歉)"
    }
  }
}
```

bool 组合匹配
核心功能：多条件组合综合查询。

应用场景：支持多条件组合查询的场景。

适用类型：text 或者 keyword。一个 bool 过滤器由三部分组成 

```json
{
   "bool" : {
      "must" :     [],
      "should" :   [],
      "must_not" : [],
      "filter":    []
   }
}

```

must ——所有的语句都 必须（must） 匹配，与 AND 等价。

must_not ——所有的语句都 不能（must not） 匹配，与 NOT 等价。

should ——至少有一个语句要匹配，与 OR 等价。

filter——必须匹配，运行在非评分&过滤模式。


![在这里插入图片描述](C:\Users\32784\Desktop\笔记\image\watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdWVyY2hvbmc=,size_16,color_FFFFFF,t_70-17001051285109.png)

多表关联如何设计
为什么会有多表关联
主要原因：常规基于关系型数据库开发，多多少少都会遇到关联查询。而关系型数据库设计的思维很容易带到 ES 的设计中。

多表关联如何实现
方案一：多表关联视图，视图同步 ES
MySQL 宽表导入 ES，使用 ES 查询+检索。适用场景：基础业务都在 MySQL，存在几十张甚至几百张表，准备同步到 ES，使用 ES 做全文检索。

将数据整合成一个宽表后写到 ES，宽表的实现可以借助关系型数据库的视图实现。

宽表处理在处理一对多、多对多关系时，会有字段冗余问题，如果借助：logstash_input_jdbc，关系型数据库如 MySQL 中的每一个字段都会自动帮你转成 ES 中对应索引下的对应 document 下的某个相同字段下的数据。

步骤 1：提前关联好数据，将关联的表建立好视图，一个索引对应你的一个视图，并确认视图中数据的正确性。

步骤 2：ES 中针对每个视图定义好索引名称及 Mapping。

步骤 3：以视图为单位通过 logstash_input_jdbc 同步到 ES 中。

方案二：1 对 1 同步 ES
MySQL+ES 结合，各取所长。适用场景：关系型数据库全量同步到 ES 存储，没有做冗余视图关联。

ES 擅长的是检索，而 MySQL 才擅长关系管理。

所以可以考虑二者结合，使用 ES 多索引建立相同的别名，针对别名检索到对应 ID 后再回 MySQL 通过关联 ID join 出需要的数据。

方案三：使用 Nested 做好关联
适用场景：1 对少量的场景。

举例：有一个文档描述了一个帖子和一个包含帖子上所有评论的内部对象评论。可以借助 Nested 实现。

Nested 类型选型——如果需要索引对象数组并保持数组中每个对象的独立性，则应使用嵌套 Nested 数据类型而不是对象 Oject 数据类型。

当使用嵌套文档时，使用通用的查询方式是无法访问到的，必须使用合适的查询方式（nested query、nested filter、nested facet等），很多场景下，使用嵌套文档的复杂度在于索引阶段对关联关系的组织拼装。

方案四：使用 ES6.X+ 父子关系 Join 做关联

适用场景：1 对多量的场景。

举例：1 个产品和供应商之间是1对N的关联关系。

Join 类型：join 数据类型是一个特殊字段，用于在同一索引的文档中创建父/子关系。关系部分定义文档中的一组可能关系，每个关系是父名称和子名称。

当使用父子文档时，使用has_child 或者has_parent做父子关联查询。

方案三、方案四选型对比：
![在这里插入图片描述](C:\Users\32784\Desktop\笔记\image\watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdWVyY2hvbmc=,size_16,color_FFFFFF,t_70-170010517782912.png)

注意：方案三&方案四选型必须考虑性能问题。文档应该尽量通过合理的建模来提升检索效率。

Join 类型应该尽量避免使用。nested 类型检索使得检索效率慢几倍，父子Join 类型检索会使得检索效率慢几百倍。

尽量将业务转化为没有关联关系的文档形式，在文档建模处多下功夫，以提升检索效率。


![在这里插入图片描述](C:\Users\32784\Desktop\笔记\image\watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdWVyY2hvbmc=,size_16,color_FFFFFF,t_70-170010519196915.png)

# ES设计2

## 索引设计的重要性

　　首先索引创建后，索引的分片只能通过_split和_shrink接口对其进行成倍的增加和缩减，主要是因为es的数据是通过_routing分配到各个分片上面的，所以本质上是不推荐去改变索引的分片数量的，因为这样都会对数据进行重新的移动。还有就是索引只能新增字段，不能对字段进行修改和删除，缺乏灵活性，所以每次都只能通过_reindex重建索引了。还有就是一个分片的大小以及所以分片数量的多少严重影响到了索引的查询和写入性能。所以可想而知，设计一个好的索引能够减少后期的运维管理和提高不少性能。所以前期对索引的设计是相当的重要的。

- 好的索引设计在整个集群规划中占据举足轻重的作用，索引的设计直接影响集群设计的好坏和复杂度。
- 好的索引设计应该是充分结合业务场景的时间维度和空间维度，结合业务场景充分考量增、删、改、查等全维度设计的。
- 好的索引设计是完全基于“设计先行，编码在后”的原则，前期会花很长时间，为的是后期工作更加顺畅，避免不必要的返工

[回到顶部](https://www.cnblogs.com/zsql/p/14418018.html#_labelTop)

## 二、如何设计索引

　　在设计索引之前我们要明白索引有些什么内容，明白索引的构成，比如索引的基本配置setting，映射mapping，还有重要的分片，副本，模板，索引的生命周期等。知道这些之后就可以有针对性的设计了。首先要结合公司的业务场景，数据量的大小，每天增量如何，数据的特点，会不会对历史数据进行重新更新。数据存多久，是永久还是有一定的周期。数据需要准实时呢还是不需要准实时呢。所以清楚索引的构成和知道业务场景，才能够结合起来做更好的设计。



### 2.1、考虑索引的公共基本配置

由于elasticsearch7.x不允许把索引级别的设置配置在elasticsearch.yml中，所以需要对每个索引进行单独的配置，这样的话就比较麻烦，所以可以把这些公共的配置配置在索引模板中，这样就可以在新建索引的时候可以自动的设置到索引中，关于索引模板的操作可以看考：[聊聊elasticsearch7.8的模板和动态映射](https://www.cnblogs.com/zsql/p/14381646.html)

接下来看看一些常用索引级别的配置

[![复制代码](C:\Users\32784\Desktop\笔记\image\copycode.gif)](javascript:void(0);)

```
"number_of_replicas": 1, #推荐副本数为1
"max_result_window": 100000,
"refresh_interval": "30s", #这里对实时性要求不高，可以增加该值提高写入性能
"index.search.slowlog.threshold.query.warn": 10s,
"index.search.slowlog.threshold.query.info": 5s,
"index.search.slowlog.threshold.query.debug": 2s,
"index.search.slowlog.threshold.query.trace": 500ms,
"index.search.slowlog.threshold.fetch.warn": 1s,
"index.search.slowlog.threshold.fetch.info": 800ms,
"index.search.slowlog.threshold.fetch.debug": 500ms,
"index.search.slowlog.threshold.fetch.trace": 200ms,
"index.indexing.slowlog.threshold.index.warn": 10s,
"index.indexing.slowlog.threshold.index.info": 5s,
"index.indexing.slowlog.threshold.index.debug": 2s,
"index.indexing.slowlog.threshold.index.trace": 500ms
"dynamic": false #是否关闭动态字段映射，默认为true，这里选择个人选择禁用
```

[![复制代码](C:\Users\32784\Desktop\笔记\image\copycode.gif)](javascript:void(0);)

当然索引的配置还有很多其他的，可以根据实际情况进行调整，这样就可以把需要配置公共索引配置设计成索引模板：

[![复制代码](C:\Users\32784\Desktop\笔记\image\copycode.gif)](javascript:void(0);)

```
PUT _index_template/template_index
{
    "index_patterns": [
        "index-*"
    ],
    "template": {
        "settings": {
            "number_of_replicas": 1,
            "max_result_window": 100000,
            "refresh_interval": "30s",
            "index.search.slowlog.threshold.query.warn": "10s",
            "index.search.slowlog.threshold.query.info": "5s",
            "index.search.slowlog.threshold.query.debug": "2s",
            "index.search.slowlog.threshold.query.trace": "500ms",
            "index.search.slowlog.threshold.fetch.warn": "1s",
            "index.search.slowlog.threshold.fetch.info": "800ms",
            "index.search.slowlog.threshold.fetch.debug": "500ms",
            "index.search.slowlog.threshold.fetch.trace": "200ms",
            "index.indexing.slowlog.threshold.index.warn": "10s",
            "index.indexing.slowlog.threshold.index.info": "5s",
            "index.indexing.slowlog.threshold.index.debug": "2s",
            "index.indexing.slowlog.threshold.index.trace": "500ms"
        },
        "mappings": {
            "dynamic": false
        }
    },
    "priority": 10
}
```

[![复制代码](C:\Users\32784\Desktop\笔记\image\copycode.gif)](javascript:void(0);)

这样新建index-开头的索引的时候都会默认配置好如上的配置，这样就是考虑到基本设置公共化



### 2.2、索引命名规范

　　这部分主要说下索引命名规范，包括别名，通过别名可以使得索引的操作变得更加灵活，一个索引可以有多个别名，当然一个别名可以配置多个索引，这样的话就极大的增加了索引的的灵活性。在索引的命名上规定特殊字段开头，同样对其好进行权限控制，关于权限控制可以参考：[elasticsearch7.8权限控制和规划](https://www.cnblogs.com/zsql/p/14373370.html)

必须严格按照如下命名格式：（否则将无法使用，因为这里设置了相关权限）；

- **索引命名规范：index-{行业}-{业务}-{版本}**
- **别名命名规范：\**index\**-{行业}-{业务}**

 如果是索引拆分后（有多个索引），需要一个全局的读的别名对所有拆分后的所有进行命名，和一个最新索引写的别名（只对可更新的索引），如果这里没有描述清楚，请见**2.5大索引的设计**，两个别名可规范如下：

- 读别名：index-{行业}-{业务}-read
- 写别名：index-{行业}-{业务}-insert



### 2.3、mapping的设计

mapping设置主要就是怎么选择数据类型，分词等

中文分词：推荐使用："analyzer": "ik_max_word" ，这样可以更细粒度的进行中文分词

设置字段的时候，务必过一下如下图示的流程。根据实际业务需要，主要关注点：

- 数据类型选型；
- 是否需要检索；
- 是否需要排序+聚合分析；
- 是否需要另行存储

![img](C:\Users\32784\Desktop\笔记\image\1271254-20210219223103209-1834910904.png)

![img](C:\Users\32784\Desktop\笔记\image\1271254-20210219223221700-1168895403.png)

### 2.4、分片的设计

这个很重要，直接影响到后期的管理和性能。

Elasticsearch中的数据组织成索引。每一个索引由一个或多个分片组成。每个分片是Luncene索引的一个实例，你可以把实例理解成自管理的搜索引擎，用于在Elasticsearch集群中对一部分数据进行索引和处理查询。

分片设计原则

- 推荐每个分片的大小：20-40G，建议不超过30G，但是也会有特殊的情况，有些索引字段少，但是数据量大，这样的话也可以增加一些分片数
- 确保每个节点的分片数量保持在低于每1GB堆内存对应集群的分片在20-25之间。 因此，具有30GB堆内存的节点最多可以有600-750个分片
- 每个索引的分片：一般为节点数的1-3倍，假设我们有15个数据节点，则15*3*40G=1.8T，这样一个索引最多设置真的大，如果超过了，就需要参考大索引的设计
- 分片数量尽量为数据节点的倍数，这样就可以使得数据进来均衡，但是数据量极少的索引，根据情况进行分片数量的设计

下面写一个简单的参考表（都可以根据实际情况进行调整，只是个人的建议）：

<div class="table-wrapper"><table style="height: 188px; width: 613px" border="0">
<tbody>
<tr>
<td style="border: 1px solid rgba(52, 29, 226, 1); text-align: center"><span style="font-size: 15px">索引的大小</span></td>
<td style="border: 1px solid rgba(52, 29, 226, 1); text-align: center"><span style="font-size: 15px">分片数量设置</span></td>
</tr>
<tr>
<td style="border: 1px solid rgba(52, 29, 226, 1); text-align: center"><span style="font-size: 15px">0-20G</span></td>
<td style="border: 1px solid rgba(52, 29, 226, 1); text-align: center"><span style="font-size: 15px">2</span></td>
</tr>
<tr>
<td style="border: 1px solid rgba(52, 29, 226, 1); text-align: center"><span style="font-size: 15px">20-100G</span></td>
<td style="border: 1px solid rgba(52, 29, 226, 1); text-align: center"><span style="font-size: 15px">8</span></td>
</tr>
<tr>
<td style="border: 1px solid rgba(52, 29, 226, 1); text-align: center"><span style="font-size: 15px">100-400G</span></td>
<td style="border: 1px solid rgba(52, 29, 226, 1); text-align: center"><span style="font-size: 15px">15</span></td>
</tr>
<tr>
<td style="border: 1px solid rgba(52, 29, 226, 1); text-align: center"><span style="font-size: 15px">400-900G</span></td>
<td style="border: 1px solid rgba(52, 29, 226, 1); text-align: center"><span style="font-size: 15px">30</span></td>
</tr>
<tr>
<td style="border: 1px solid rgba(52, 29, 226, 1); text-align: center"><span style="font-size: 15px">900G-1.6T</span></td>
<td style="border: 1px solid rgba(52, 29, 226, 1); text-align: center"><span style="font-size: 15px">45</span></td>
</tr>
</tbody>
</table></div>

如上设置是基于15个数据节点进行配置的，基本都给增量预留了一些空间，最好是根据实际情况进行设定，如果一个索引已经很大了，上面的配置不能满足了的话需要对对索引进行拆分了，使用索引模板+Rollover+索引生命周期进行自动滚动，拆分索引。见2.5节









## 索引模板1

```json

COPYPUT _index_template/logstash-village
{
  "index_patterns": [
    "logstash-village-*"  // 可以通过"logstash-village-*"来适配创建的索引
  ],
  "template": {
    "settings": {
      "number_of_shards": "3", //指定模板分片数量
      "number_of_replicas": "2"  //指定模板副本数量
    },
    "aliases": {
      "logstash-village": {}  //指定模板索引别名
    },
    "mappings": {   //设置映射
      "dynamic": "strict", //禁用动态映射
      "properties": {
        "@timestamp": {
          "type": "date",
           "format": "strict_date_optional_time||epoch_millis||yyyy-MM-dd HH:mm:ss"
        },
        "@version": {
          "doc_values": false,
          "index": "false",
          "type": "integer"
        },
        "name": {
          "type": "keyword"
        },
        "province": {
          "type": "keyword"
        },
        "city": {
          "type": "keyword"
        },
        "area": {
          "type": "keyword"
        },
        "addr": {
          "type": "text",
          "analyzer": "ik_smart"
        },
        "location": {
          "type": "geo_point"
        },
        "property_type": {
          "type": "keyword"
        },
        "property_company": {
          "type": "text",
          "analyzer": "ik_smart"
        },
        "property_cost": {
          "type": "float"
        },
        "floorage": {
          "type": "float"
        },
        "houses": {
          "type": "integer"
        },
        "built_year": {
          "type": "integer"
        },
        "parkings": {
          "type": "integer"
        },
        "volume": {
          "type": "float"
        },
        "greening": {
          "type": "float"
        },
        "producer": {
          "type": "keyword"
        },
        "school": {
          "type": "keyword"
        },
        "info": {
          "type": "text",
          "analyzer": "ik_smart"
        }
      }
    }
  }
}


```



<table><thead><tr><th>参数名称</th><th>参数介绍</th></tr></thead><tbody><tr><td>index_patterns</td><td>必须配置，用于在创建期间匹配索引名称的通配符（*）表达式数组</td></tr><tr><td>template</td><td>可选配置，可以选择包括别名、映射或设置配置</td></tr><tr><td>composed_of</td><td>可选配置，组件模板名称的有序列表。组件模板按指定的顺序合并，这意味着最后指定的组件模板具有最高的优先级</td></tr><tr><td>priority</td><td>可选配置，创建新索引时确定索引模板优先级的优先级。选择具有最高优先级的索引模板。如果未指定优先级，则将模板视为优先级为0（最低优先级）</td></tr><tr><td>version</td><td>可选配置，用于外部管理索引模板的版本号</td></tr><tr><td>_meta</td><td>可选配置，关于索引模板的可选用户元数据，可能有任何内容</td></tr></tbody></table>













# ElasticSearch : 索引模板和滚动索引实现

[AntBlack](https://juejin.cn/user/3790771822007822/posts)

2023-04-021,560

> **首先分享之前的所有文章 , 欢迎点赞收藏转发三连下次一定 >>>>** 😜😜😜
> **文章合集 :** 🎁 [juejin.cn/post/694164…](https://juejin.cn/post/6941642435189538824)
> **Github :** 👉 [github.com/black-ant](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fblack-ant)
> **CASE 备份 :** 👉 [gitee.com/antblack/ca…](https://link.juejin.cn/?target=https%3A%2F%2Fgitee.com%2Fantblack%2Fcase)

## 一. 前言

ES 最近有需求涉及到滚动索引方面的概念，但是这一块一直没有太系统的了解。这一篇索性把相关的节点进行深度的学习。

针对版本 ：**ES 7**

**吐槽一下 ： ES 通病，每次更新后会改来改去。你像MySQL , 更新后 SQL 至少不会有什么减少，ES每次更新，就像重做了一下，API变了不说，各种Maven版本还不能互相适配**

## 二. 索引

在ES中，数据存储在索引中。索引是一种类似于数据库中表的数据结构，它包含了一系列的文档，每个文档有一个唯一的标识符，称为文档ID

通常一个索引包含 ：分片 ， 映射， 分析器等多个部分组成。我们可以通过 API 创建一个索引 ：

```java
java复制代码PUT /my_index 
{
    "settings": {
        "number_of_shards": 1,
        "number_of_replicas": 0
    },
    "mappings": {
        "properties": {
            "title": {
                "type": "text"
            }
        }
    }
}
```

建立索引的时候可以为其配置很多 Setting 和 properties ， 这些配置可以帮助我们更好的使用索引。

但是这衍生一个问题 ，如果索引特别多或者需要滚动索引的时候，每一次都需要配置，这个时候就需要我们使用索引模板了》》》

## 三. 索引模板

> 作用

索引模板定义了设置和映射，你可以在创建新索引时**自动应用**。Elasticsearch根据与索引名称相匹配的索引模式，**将模板应用于新的索引**。

索引模板只在创建索引时应用。**对索引模板的改变不影响现有的索引**。在创建索引的API请求中指定的设置和映射会**覆盖索引模板**中指定的任何设置或映射

### 3.1 索引模板的创建

```java
java复制代码PUT _template/test_template
{
    "order": 0,
    "index_patterns": [
        "test_*"
    ],
    "settings": {
        "index": {
            // 分片数量
            "number_of_shards": "6",
            // 刷新间隔
            "refresh_interval": "10s"
        }
    },
    "mappings": {
        "properties": {
            "createTime": {
                "type": "date",
                "format": "yyyy-MM-dd HH:mm:ss"
            },
            "requestId": {
                "type": "keyword"
            },
            "title": {
                "type": "text",
                // 分词器
                "analyzer": "standard",
                "fields": {
                    "keyword": {
                        "type": "keyword"
                    }
                }
            }
        }
    },
    "aliases": {}
}
```

在上面的例子中，`my_template` 是索引模板的名称，`my_index_*` 是新创建的索引的名称，`number_of_shards` 是设置每个新索引的分片数，`timestamp` 是要插入的时间戳字段。

其他核心的字段 ：

1. `template`: 匹配模板名称的模式，可以使用通配符来匹配多个模板。
2. `order`: 定义模板的匹配顺序。较低的数字优先匹配。
3. `settings`: 指定模板的索引设置，例如副本数量、分片数量和分配策略。
4. `mappings`: 指定模板的索引映射，即文档类型和字段。
5. `aliases`: 定义与索引相关联的别名。
6. `version`: 定义模板的版本号，用于在更新模板时进行冲突检测。
7. `index_patterns`: 指定要应用模板的索引模式，支持使用通配符指定多个索引。
8. `composed_of`: 定义其他 `_template` API 模板的组合。
9. `priority`: 定义模板的优先级。较高的数字优先匹配。
10. `metadata`: 定义模板的任意元数据。

[Create or update index template API | Elasticsearch Guide [8.6\] | Elastic](https://link.juejin.cn/?target=https%3A%2F%2Fwww.elastic.co%2Fguide%2Fen%2Felasticsearch%2Freference%2Fcurrent%2Findices-templates-v1.html)

### 3.2 索引模板 Setting

setting 用于定义策略，包括分片，刷新等等 ，它拥有如下核心配置 ：

- index.number_of_shards：设置每个新创建索引的主分片数量。默认为 1
- index.number_of_replicas：设置每个新创建索引的副本分片数量。默认为 1
- index.codec：设置新创建索引所使用的编解码器
- index.routing.allocation.total_shards_per_node：设置每个节点最多可以容纳的主分片和副本分片总数
- index.routing.allocation.require：设置新创建索引需要符合的节点筛选条件
- index.lifecycle.name：设置新创建索引要使用的生命周期策略
- index.refresh_interval：设置新创建索引的刷新间隔时间
- index.max_result_window：设置新创建索引可返回的最大搜索结果数
- index.mapping.ignore_malformed：设置是否忽略在文档中出现的字段映射错误
- index.analysis.analyzer：设置新创建索引中的分析器
- index.analysis.filter：设置新创建索引中的分析过滤器

### 3.3 索引映射 ：mapping

- `type`：指定字段的数据类型
- `index`：指定字段是否索引，可以是 analyzed、not_analyzed、no 或 false
- `store`：指定字段是否存储，可以是 true 或 false
- `analyzer`：指定字段分析器的名称，可以是内置分析器或自定义分析器
- `search_analyzer`：指定查询时使用的分析器名称，可以是内置分析器或自定义分析器
- `normalizer`：指定字段规范化器的名称，用于在查询和聚合时规范化字段值
- `copy_to`：指定一个或多个字段，将该字段的内容复制到指定的字段中，以便在查询和聚合时使用
- `fields`：为字段定义多个属性，例如不同的分析器、不同的索引设置等
- `format`：指定日期类型的格式化方式

**注意 ：还是由于版本的问题，导致每个版本的写法不一样，可能有的还会过时删除！！**

## 四. 业务功能

### 4.1 创建滚动索引

**背景** ： ES 记录业务审计日志，每天生成大量的记录，导致单个索引过大

> 前置知识点 ：

```java
java复制代码// 索引别名 : 指向一个或多个索引的可读写名称，它们可以被用来代替实际的索引名称
- 帮助实现索引的无缝切换，同时也可以减少修改客户端查询的需要
- 切换集群，切换测试时都可以快速修改

// ES Rollover 特性
- 允许你在索引到达一定大小或者时间上限时自动滚动到一个新的索引中
- 新索引可以是一个完全相同的结构，也可以是不同的
- 可以是在同一个 Elasticsearch 集群中的不同节点上，也可以是在不同的集群上
```

> 创建流程 ：

- S1 : 创建一个索引模板 ， 为模板定义一个 index_patterns
- S2 : 创建一个索引，并且为索引定义一个别名
- S3 : 触发索引的滚动，同时通过别名进行业务操作

```java
java复制代码// S1 : 创建模板
- 创建 ：此处略，创建方式就是上面的模板创建案例
- 查看创建的模板 ： GET _template/test_template


// S2 : 索引创建过程 (这里的实际格式为 <test-{now/d}-000001>)
PUT %3Ctest_%7Bnow%2Fd%7D-000001%3E
{
  "aliases": {
    "test_rollover": {
      "is_write_index": true
    }
  }
}
GET %3Ctest_%7Bnow%2Fd%7D-000001%3E

// S3: 为其触发手动滚动规则 （此处是有3条文档则滚动一次，此处没有数据，所以无法滚动）
POST /test_rollover/_rollover
{
    "conditions": {
        "max_docs": 3 
    }
}

// S4 : 插入文档
POST /test_rollover/_doc
{
  "businessInfo":"123"
}


// S5 : 查询当前文档数 （这里创建后已经有了多条，但是索引只有一个）
GET /test_rollover/_doc/_search
POST /test_rollover/_rollover
{
    "conditions": {
        "max_docs": 3 
    }
}

>> 再次触发滚动后，发现已经创建了新的
```

> 核心重点

- is_write_index 很重要，他将决定翻滚之后的旧索引是否还能被查询到
  - 未配置 ：别名会指向新索引，并从旧索引中移除别名 ，通过别名无法查询到旧的索引
  - 配置为 true : 别名会同时指向新旧索引 , 旧索引上的别名is_write_index会被设置为 false，仅可读
- conditions 有哪些
  - `max_docs`: 索引中文档的最大数量，超过该数量将触发rollover
  - `max_age`: 索引的最大存储时间，超过该时间将触发rollover（30d / 12h 等）
  - `max_size`: 索引的最大存储大小，超过该大小将触发rollover（5gb / 100mb 等）
  - `max_primary_shard_size`: 主分片的最大存储大小，超过该大小将触发rollover
  - `max_num_segments`: 索引的最大段数，超过该数量将触发rollover
  - `min_index_age`: 索引的最小存储时间，必须等待一段时间后才能rollover
  - `min_doc_count`: 索引中文档的最小数量，必须达到一定数量后才能rollover
- 自动化
  - 通过 _rollover 手动触发
  - 通过 Index Lifecycle Management 做生命周期控制



**注意！`POST _rollover`这个动作，并不是[ElasticSearch](https://so.csdn.net/so/search?q=ElasticSearch&spm=1001.2101.3001.7020)内建的定时机制！也就是说，除非你定期执行`POST /logs_write/_rollover`，否则当前的index即使过期了或者文档数超了，也不会rollover。**







`%7Bnow%2Fd%7D` 是 URL 编码形式，表示一个 Elasticsearch Date Math 表达式，用于动态地插入当前日期。该表达式具体含义如下：

- `%7B`: URL 编码中的 `{` 符号。
- `now`: 代表当前时间。
- `%2F`: URL 编码中的 `/` 符号。
- `d`: 表示日期。

所以，`%7Bnow%2Fd%7D` 实际上是 Elasticsearch 的 Date Math 表达式，表示当前日期。当索引被创建时，Elasticsearch 会将这个表达式替换为实际的当前日期。

例如，如果今天是2023年11月16日，那么 `%7Bnow%2Fd%7D` 将被解析为 `2023.11.16`，从而生成类似于 `logs-2023.11.16-000001` 的索引名称。这样的机制允许你在索引名称中包含当前日期信息，使索引名称更具有动态性。

因此，`%7Bnow%2Fd%7D` 不是固定的值，而是一个动态的表达式，根据实际情况在索引创建时被解析为当前日期。








如果你希望创建只有年份的索引，可以使用 Elasticsearch 的 Date Math 表达式，将只有年份的部分作为常量插入到索引名称中。以下是一个示例，演示如何在索引名称中包含只有年份的信息：

1. **创建带有年份的索引：**

```
jsonCopy codePUT /index-%7Bnow%2FY%7D-01
{
  "aliases": {
    "index-current": {}
  }
}
```

上述请求中，`%7Bnow%2FY%7D` 是一个 Elasticsearch 的 Date Math 表达式，表示当前年份。这个表达式在索引创建时会被解析为实际的当前年份。 `-01` 部分表示版本迭代号，你可以根据需要进行调整。

1. **使用 Rollover API 触发索引切割：**

```
jsonCopy codePOST /index-current/_rollover 
{
  "conditions": {
    "max_age": "1d",
    "max_docs": 1000
  }
}
```

在这里，`index-current` 是索引的别名，通过 Rollover API 触发索引切割。如果索引的最大生命周期超过1天，或者文档数超过1000，则执行 Rollover 操作，创建新的索引。

这样，你就可以实现只有年份的索引，并在每年创建新的索引。例如，今年的索引为 `index-2023-01`，明年的索引将为 `index-2024-01`，依此类推。



在数学日期表达式中，now就是现在的时间，比如,我写下这篇博客的时间是`2016.03.17 20:39:00`。

- now/d，就是向一天取整,即`2016.03.17 00:00:00`。
- now/M，就是向一个月取整，即`2016.03.01 00:00:00`

它还支持加减法，比如

- now+1h，就是`2016.03.17 21:39:00`
- now-1d，就是`2016.03.16 20:39:00`

了解日期表达式的用法，在使用elasticsearch时是很必要的。

### 索引数据的例子

```javascript
curl -XPOST 'localhost:9200/<test-\{now%2FM\}>/type/1?pretty' -d '{"name":"xing1",age:20}'
{
  "_index" : "test-2016.03.01",
  "_type" : "type",
  "_id" : "1",
  "_version" : 1,
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "created" : true
}
```

复制

注意：

- 1 正常的日期表达式格式为 now/d，但是符号`/`必须经过编码才行
- 2 大括号需要进行转义









## 日期数学表达式的例子

比如现在的时间是`2024年3月22日中午12点.utc`

注意，如果是中国的时间需要加上8个小时！

<table><thead><tr><th style="text-align: left;"><div><div class="table-header"><p>表达式</p></div></div></th><th style="text-align: left;"><div><div class="table-header"><p>表示的值</p></div></div></th></tr></thead><tbody><tr><td style="text-align: left;"><div><div class="table-cell"><p>&lt;test-{now/d}&gt;</p></div></div></td><td style="text-align: left;"><div><div class="table-cell"><p>test-2024.03.22</p></div></div></td></tr><tr><td style="text-align: left;"><div><div class="table-cell"><p>&lt;test-{now/M}&gt;</p></div></div></td><td style="text-align: left;"><div><div class="table-cell"><p>test-2024.03.01</p></div></div></td></tr><tr><td style="text-align: left;"><div><div class="table-cell"><p>&lt;test-{now/M{YYYY.MM}}&gt;</p></div></div></td><td style="text-align: left;"><div><div class="table-cell"><p>test-2024.03</p></div></div></td></tr><tr><td style="text-align: left;"><div><div class="table-cell"><p>&lt;test-{now/M-1M{YYYY.MM}}&gt;</p></div></div></td><td style="text-align: left;"><div><div class="table-cell"><p>test-2024.02</p></div></div></td></tr><tr><td style="text-align: left;"><div><div class="table-cell"><p>&lt;test-{now/d{YYYY.MM.dd\|+12:00}}&gt;</p></div></div></td><td style="text-align: left;"><div><div class="table-cell"><p>test-2024.03.23</p></div></div></td></tr></tbody></table> 





















# 相关性算分

![image-20231223154216340](C:\Users\32784\Desktop\笔记\image\image-20231223154216340.png)





Function Score  Query





![image-20231223155632998](C:\Users\32784\Desktop\笔记\image\image-20231223155632998.png)







# 查询

## 复合查询

![image-20231223160347374](C:\Users\32784\Desktop\笔记\image\image-20231223160347374.png)





## 搜索结果处理



![image-20231223161150054](C:\Users\32784\Desktop\笔记\image\image-20231223161150054.png)

  



![image-20231223164930983](C:\Users\32784\Desktop\笔记\image\image-20231223164930983.png)





![image-20231223165102976](C:\Users\32784\Desktop\笔记\image\image-20231223165102976.png)





![image-20231223165157232](C:\Users\32784\Desktop\笔记\image\image-20231223165157232.png)



![image-20231223165724898](C:\Users\32784\Desktop\笔记\image\image-20231223165724898.png) 





## RestClient 查询文档

![image-20231223165920550](C:\Users\32784\Desktop\笔记\image\image-20231223165920550.png)



### 精确查询&组合查询

![image-20231223171929010](C:\Users\32784\Desktop\笔记\image\image-20231223171929010.png)



![image-20231223171943548](C:\Users\32784\Desktop\笔记\image\image-20231223171943548.png)





![image-20231223172719477](C:\Users\32784\Desktop\笔记\image\image-20231223172719477.png)





![image-20231223172929517](C:\Users\32784\Desktop\笔记\image\image-20231223172929517.png)







## 聚合

![image-20231224143053088](C:\Users\32784\Desktop\笔记\image\image-20231224143053088.png)

![image-20231224143425730](C:\Users\32784\Desktop\笔记\image\image-20231224143425730.png)





![image-20231224143710749](C:\Users\32784\Desktop\笔记\image\image-20231224143710749.png)



RestAPI实现聚合



![image-20231224144243946](C:\Users\32784\Desktop\笔记\image\image-20231224144243946.png)



![image-20231224144339284](C:\Users\32784\Desktop\笔记\image\image-20231224144339284.png)





## 自动补全

###  拼音分词器

![image-20231224152555317](C:\Users\32784\Desktop\笔记\image\image-20231224152555317.png)

![image-20231224152724388](C:\Users\32784\Desktop\笔记\image\image-20231224152724388.png)



创建倒排索引映射时应该用自定义分词器，拼音分词器不适用于搜索

![image-20231224153042845](C:\Users\32784\Desktop\笔记\image\image-20231224153042845.png)

![image-20231224153639780](C:\Users\32784\Desktop\笔记\image\image-20231224153639780.png)





### completion suggester查询

![image-20231224154632841](C:\Users\32784\Desktop\笔记\image\image-20231224154632841.png)

![image-20231224153923095](C:\Users\32784\Desktop\笔记\image\image-20231224153923095.png)



![image-20231224154111484](C:\Users\32784\Desktop\笔记\image\image-20231224154111484.png)



![image-20231224154416758](C:\Users\32784\Desktop\笔记\image\image-20231224154416758.png)



![image-20231224164218317](C:\Users\32784\Desktop\笔记\image\image-20231224164218317.png)





![image-20231224164657799](C:\Users\32784\Desktop\笔记\image\image-20231224164657799.png)





# 数据同步

![image-20231224172351059](C:\Users\32784\Desktop\笔记\image\image-20231224172351059.png)



![image-20231224172412335](C:\Users\32784\Desktop\笔记\image\image-20231224172412335.png)



![image-20231224172626347](C:\Users\32784\Desktop\笔记\image\image-20231224172626347.png)



![image-20231224172718610](C:\Users\32784\Desktop\笔记\image\image-20231224172718610.png)



![image-20231224172846940](C:\Users\32784\Desktop\笔记\image\image-20231224172846940.png) 

![image-20231224173558259](C:\Users\32784\Desktop\笔记\image\image-20231224173558259.png)



# 集群

![image-20231224185914582](C:\Users\32784\Desktop\笔记\image\image-20231224185914582.png)



![image-20231224190331139](C:\Users\32784\Desktop\笔记\image\image-20231224190331139.png)



## 脑裂问题

![image-20231224190514152](C:\Users\32784\Desktop\笔记\image\image-20231224190514152.png)



![image-20231224190942030](C:\Users\32784\Desktop\笔记\image\image-20231224190942030.png)

![image-20231224194121009](C:\Users\32784\Desktop\笔记\image\image-20231224194121009.png)

![image-20231224194515551](C:\Users\32784\Desktop\笔记\image\image-20231224194515551.png)

![image-20231224194852925](C:\Users\32784\Desktop\笔记\image\image-20231224194852925.png)

![image-20231224194957201](C:\Users\32784\Desktop\笔记\image\image-20231224194957201.png)

![image-20231224195112109](C:\Users\32784\Desktop\笔记\image\image-20231224195112109.png)
