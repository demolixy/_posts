
---
title: ES
---

[TOC]

## 1 介绍
* 分布式 可扩展 实时的搜索引擎
* 接近实时```GET```请求，适合作为```NoSQL```数据存储，但是缺乏```分布式事务```
* 支持多种客户端
* 强大的统计功能
* ```RESTful``` ```Api``` 接口

## 2 使用场景
* 全文检索
* 结构化检索
* 数据分析
* ```NoSQL``` ```Json``` 文档数据库
### 2.1 材料分析使用场景
* 文档数据存储（存储文本的结构化数据）
* 文本搜索（关键字搜索）
* 数据统计（用户使用习惯统计）

## 3 如何部署
### 3.1 服务端
支持linux和window平台部署
* 硬件要求
    > 内存要求比较高，排序和聚合比较好内存

    > 硬盘尽可能快

    > cpu要求不高，核数多，支持的并发就多
* 操作系统
    > 推荐部署在linux下。 （磁盘阵列）???
* JDK
    > JDK 1.7 + 
* 部署配置说明
    > ```1.7.5``` 部署说明

```
cluster.name: clfx-test
配置es的集群名称，默认是elasticsearch，es会自动发现在同一网段下的es，如果在同一网段下有多个集群，就可以用这个属性来区分不同的集群。
node.name: "data-0"
节点名，默认随机指定一个name列表中名字，该列表在es的jar包中config文件夹里name.txt文件中，其中有很多作者添加的有趣名字。
node.master: true
指定该节点是否有资格被选举成为node，默认是true，es是默认集群中的第一台机器为master，如果这台机挂了就会重新选举master。
node.data: true
指定该节点是否存储索引数据，默认为true。
index.number_of_shards: 5
设置默认索引分片个数，默认为5片。
index.number_of_replicas: 1
设置默认索引副本个数，默认为1个副本。
path.conf: /path/to/conf
设置配置文件的存储路径，默认是es根目录下的config文件夹。
path.data: /path/to/data
设置索引数据的存储路径，默认是es根目录下的data文件夹，可以设置多个存储路径，用逗号隔开，例：
path.data: /path/to/data1,/path/to/data2
path.work: /path/to/work
设置临时文件的存储路径，默认是es根目录下的work文件夹。
path.logs: /path/to/logs
设置日志文件的存储路径，默认是es根目录下的logs文件夹
path.plugins: /path/to/plugins
设置插件的存放路径，默认是es根目录下的plugins文件夹
bootstrap.mlockall: true
设置为true来锁住内存。因为当jvm开始swapping时es的效率会降低，所以要保证它不swap，可以把ES_MIN_MEM和 ES_MAX_MEM两个环境变量设置成同一个值，并且保证机器有足够的内存分配给es。同时也要允许elasticsearch的进程可以锁住内存，linux下可以通过`ulimit -l unlimited`命令。
network.bind_host: 192.168.0.1
设置绑定的ip地址，可以是ipv4或ipv6的，默认为0.0.0.0。 
network.publish_host: 192.168.0.1
设置其它节点和该节点交互的ip地址，如果不设置它会自动判断，值必须是个真实的ip地址。
network.host: 192.168.0.1
这个参数是用来同时设置bind_host和publish_host上面两个参数。
transport.tcp.port: 9300
设置节点间交互的tcp端口，默认是9300。
transport.tcp.compress: true
设置是否压缩tcp传输时的数据，默认为false，不压缩。
http.port: 9200
设置对外服务的http端口，默认为9200。
http.max_content_length: 100mb
设置内容的最大容量，默认100mb
http.enabled: false
是否使用http协议对外提供服务，默认为true，开启。
gateway.type: local
gateway的类型，默认为local即为本地文件系统，可以设置为本地文件系统，分布式文件系统，Hadoop的HDFS，和amazon的s3服务器。
gateway.recover_after_nodes: 1
设置集群中N个节点启动时进行数据恢复，默认为1。
gateway.recover_after_time: 5m
设置初始化数据恢复进程的超时时间，默认是5分钟。
gateway.expected_nodes: 2
设置这个集群中节点的数量，默认为2，一旦这N个节点启动，就会立即进行数据恢复。
cluster.routing.allocation.node_initial_primaries_recoveries: 4
初始化数据恢复时，并发恢复线程的个数，默认为4。
cluster.routing.allocation.node_concurrent_recoveries: 2
添加删除节点或负载均衡时并发恢复线程的个数，默认为4。
indices.recovery.max_size_per_sec: 0
设置数据恢复时限制的带宽，如入100mb，默认为0，即无限制。
indices.recovery.concurrent_streams: 5
设置这个参数来限制从其它分片恢复数据时最大同时打开并发流的个数，默认为5。
discovery.zen.minimum_master_nodes: 1
设置这个参数来保证集群中的节点可以知道其它N个有master资格的节点。默认为1，对于大的集群来说，可以设置大一点的值（2-4）
discovery.zen.ping.timeout: 3s
设置集群中自动发现其它节点时ping连接超时时间，默认为3秒，对于比较差的网络环境可以高点的值来防止自动发现时出错。
discovery.zen.ping.multicast.enabled: false
设置是否打开多播发现节点，默认是true。
discovery.zen.ping.unicast.hosts: ["host1", "host2:port", "host3[portX-portY]"]
设置集群中master节点的初始列表，可以通过这些节点来自动发现新加入集群的节点。

```
* 分词插件
    > IK分词

```http://172.16.33.29:9201/_analyze?text=%E6%9D%90%E6%96%99%E5%88%86%E6%9E%90```

### 3.2 客户端
客户端采用1.7.5
* Java
```
            Settings settings = ImmutableSettings.settingsBuilder()
                    .put("cluster.name", clusterName)
                    .put("client.transport.sniff", true).build();
            Esclient esClient = new TransportClient(settings)
                    .addTransportAddress(new InetSocketTransportAddress(ip,
                            port));
```

* HTTP

```
PUT /megacorp/employee/1
{
    "first_name" : "John",
    "last_name" :  "Smith",
    "age" :        25,
    "about" :      "I love to go rock climbing",
    "interests": [ "sports", "music" ]
}
```
## 4 存储结构
### 4.1 document
* 文档
    > 一个文档是可以被索引的最基本的单位。
    > 文档相当于关系型数据库中的row，也就是一行行的数据。
    > 例如一个用户可以是一个文档，一个产品可以是一个文档，一条订单记录可以是一个文档。一般用作json格式来展示。

### 4.2 index
* 索引
    > index 是一些列具有相同特性的文档的集合。类似于关系型数据库中的库。
    > 举例说明，你可以为客户数据创建索引，可以为产品目录创建索引，也可以为其他任何数据创建索引。
    > 需要注意的是，index name 必须为小写字母 ，通常索引的名称会用在索引数据，搜索，更新或者删除数据的地方。

### 4.3 type
* 类型
    > 在一个索引中，你可以定义多个 type ，type 是索引中的逻辑分类。当然这个type的意义完全取决于你。
    > type 相当于关系型数据数据库中的表。
    > 举例来说，在一个博客系统中，你可以定义一个 user type,可以定义一个 blog type,还可以定义一个 comment type。

### 4.4 Cluser
* 聚集
    > cluser 也称聚集，是由一个或者多个 node 组成的集合，保存了你所有的数据并且为所有节点提供索引及搜索的能力。cluser 的名称唯一。在聚集中，你可以有一个或者多个 node
### 4.5 Node
* 节点
    > 一个 node 是一个单个服务器，属于 cluser 的一部分，提供了索引及搜索的能力。和 cluser 一样, node 也是通过名字来确保唯一性的。当 node 启动时，会生成一个随机的字符作为 node 的名字。
    > node 可以通过配置进而加入到特定的 cluser 中。默认的话会被加入到 elasticsearch 这个 cluster。

### 4.6 Shards
* 碎片
    > 一个索引可能会存储大量的数据，进而会让单个节点超出硬件能承受范围。举例来说，存储了10亿文档的单个节点，会占用1TB磁盘空间，并且会导致查询的时候速度很慢。
    > 为了解决这个问题，Elasticsearch 提供了 碎片 也就是 shards 对 index 进行划分成更小的部分。 当你创建 index 的时候，你可以简单地指定你想要的碎片数量。每一个碎片具有和 index 完全相同的功能。

    碎片最主要的两个作用是：

    >它允许你水平地切割你的容量体积，它允许你并行地分发作业，提高系统的性能

### 4.7 Replicas
* 副本
    > 因为各种原因，所以数据丢失等问题会时有发生，碎片也可能会丢失，为了防止这个问题，所以你可以将一个或多个索引碎片复制到所谓的复制碎片，简称为副本

    副本最主要的两个作用是：

    > 它提供了高可用性,以防碎片/节点失败。之于这点，所以副本的永远不要和原始碎片分布在同一个节点上。
    > 它可以扩展系统的吞吐量，因为搜索可以在所有副本执行。=
    > 总的来说，每个 index 可以被分布到不同的碎片中，节点 可以有0个或者多个副本。一旦复制了，那么 index 就拥有一个主要的碎片和一个复制的碎片。碎片的数量和副本的数量可以在创建索引之前定义，在索引创建之后，你可以动态地改变副本的数量，但是却不能改变碎片的数量。
    > 默认情况下，Elasticsearch 为每个索引分配了5个主碎片和1个副本，这意味着在你的集群中，如果至少有两个节点，那么每个索引将有5个主碎片和5个复制碎片，总共10碎片/索引。

## 5 API
### 5.1 增
```
        JSONObject json = new JSONObject();
        json.put("name", "张三2222");
        json.put("sex", "男");
        json.put("age", "25");
        instance.getElasticWriter().index("lixiangyangtest", "qaht_type_wsinfo", "1113333333333333", json.toJSONString(), true);
```

### 5.2 删
```
                esInstance.getElasticWriter().delete("qaht_index_backfill", "clfx_qaht_type_backfill_field",
                    hit.getId());
```

### 5.3 改
```
查完在存储

```

### 5.4 查
```
            GetResponse response = esinstace.getElasticReader().searchByID(index, wstype, cBh);
            String jsonStr = response.getSourceAsString();
```

## 6 开发建议
### 6.1 索引

* 功能描述_index_v版本号
* 所有索引必须要创建别名，创建别名之后不能修改
* 时间类型用long类型的存储
* 最好index里面只创建一个type
* 创建索引时，必须要创建mapping，指定好每一个字段的属性

### 6.2 搜索Api

* org.elasticsearch.index.query.QueryBuilders.termQuery(String, String)

term是代表完全匹配，即不进行分词器分析，文档中必须包含整个搜索的词汇，但是ES存储field的分词属性不能设置

* matchPhraseQuery
```
{
  "query": {
    "match_phrase": {
        "content" : {
            "query" : "我的宝马多少马力"
        }
    }
  }
}
```
完全匹配

* matchQuery
```
{
  "query": {
    "match": {
        "content" : {
            "query" : "我的宝马多少马力"
        }
    }
  }
}
```
上面的查询匹配就会进行分词，比如"宝马多少马力"会被分词为"宝马 多少 马力", 所有有关"宝马 多少 马力", 那么所有包含这三个词中的一个或多个的文档就会被搜索出来。

* boolQuery
    > must 相当于SQl里面的AND 

    > should 相当于SQL里面的OR

## 7 问题

* 重迁索引
    > 

* 分布式事务
    > [对单个文件的变更是可以支持事务的](https://www.elastic.co/guide/cn/elasticsearch/guide/current/concurrency-solutions.html)

    > 但是包含多个文档的变更不支持

* 版本升级
    > 进行数据迁移，数据量大怎么迁移