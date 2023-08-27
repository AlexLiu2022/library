---
{"dg-publish":true,"dg-path":"  Elasticsearch.md","permalink":"/elasticsearch/","tags":["CS/microservices/middlewares/elastic-stack"],"created":"2022-09-10T15:28:01.621+08:00","updated":"2023-08-27T05:14:16.758+08:00"}
---


elasticsearch是elastic stack的核心，负责存储、搜索、分析数据

# Introduction

## ES的发展

Lucene是一个Java语言的搜索引擎类库，是Apache公司的顶级项目，由Doug Cutting于1999年研发

Lucene的优势：
- 易扩展
- 高性能（基于倒排索引）

Lucene的缺点：

- 只限于java语言开发
- 学习曲线陡峭
- 不支持水平扩展

2004年Shay Banon基于Lucene开发了Compass
2010年Shay Banon重写了Compass,取名为Elasticsearch
 
相比与lucene,elasticsearch具备下列优势：
- 支持分布式，可水平扩展
- 提供Restful接口，可被任何语言调用

## 正向索引和倒排索引

elasticsearch采用倒排索引：

- 文档（document):每条数据就是一个文档
- 词条(term):文档按照语义分成的词语

## 与MySQL概念对比

### 文档
elasticsearch是面向文档存储的，可以是数据库中的一条商品数据，一个订单信息
文档数据会被序列化为json格式后存储在elasticsearch中

 ### 索引 
索引(index):相同类型的文档的集合
映射（mapping)索引中文档的字段约束信息，类似表的结构约束


### 示意图

| MySQL  | Elasticsearch | 说明                                                                          |
| ------ | ------------- | ----------------------------------------------------------------------------- |
| Table  | Index         | 索引(index),就是文档的集合，类似数据库的表(table)                             |
| Row    | Document      | 文档(Document),就是一条条的数据，类似数据库中的行(Row),文档都是JSON格式       |
| Column | Field         | 字段(Field),就是JS0N文档中的字段，类似数据库中的列(Column)                    |
| Schema | Mapping       | Mapping(映射)是索引中文档的约束，例如字段类型约束。类似数据库的表结构(Schema) |
| SQL    | DSL           | DSL是elasticsearch提供的JS0N风格的请求语句，用来操作elasticsearch,实现CRUD    |

### 架构

MySQL:擅长事务类型操作，可以确保数据的安全和一致性
Elasticsearch:擅长海量数据的搜索、分析、计算

![](https://gcore.jsdelivr.net/gh/AlexLiu2022/resources/img/diagram-of-mysql-work-with-es.png)

