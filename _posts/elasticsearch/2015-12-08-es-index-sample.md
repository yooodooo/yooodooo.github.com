---
layout: post
title: Elasticsearch索引基本操作
tagline: [elasticsearch] 
group: elasticsearch
categories: [elasticsearch]
---
{% include codepiano/setup %}

在介绍索引之前，需要介绍下数据在Elasticsearch中的存储方式：数据是以文档的形式存储，文档本质上来讲是一个JSON对象，不同的是包括两部分：  
- 元数据(metadata),用来描述文档的信息。如`_index,_type,_id,_version,_source`等  
- 实际存储对象的JSON格式,主要在`_source`中体现

一个典型的索引文档

    {
      "_index" :   "website",
      "_type" :    "blog",
      "_id" :      "123",
      "_version" : 1,
      "found" :    true,
      "_source" :  {
          "title": "My first blog entry",
          "text":  "Just trying this out...",
          "date":  "2014/01/01"
      }
    }

文档的元数据中有3个重要的属性：`_index`,`_type`,`_id`Elasticsearch通过这三个属性来唯一确定一个文档。与关系型数据库相比，对应数据库(`_index`)和表(`_type`)

###新增索引###

索引的过程就是将文档存储的过程。前面已经说了，需要通过`_index`、`_type`、`_id`来唯一确定一个索引，其API如下

    PUT /{index}/{type}/{id}  {JSON}

如

    PUT /website/blog/123
    {
      "title": "My first blog entry",
      "text":  "Just trying this out...",
      "date":  "2014/01/01"
    }

返回

    {
       "_index":    "website",
       "_type":     "blog",
       "_id":       "123",
       "_version":  1,
       "created":   true
    }
    
每个文档中都有一个版本号(`_version`),而且在每次对该文档进行操作如更新、删除该版本号都会增加。上面的API指定了id，适用索引对象会自动生成id。如果不提供该编号，ES也会默认返回一个UUIDS的编号，同样API会发生变化： 

    POST /{index}/{type}/ {JSON}
    
请注意比较两种方式的Method和URL的区别

    PUT  /{index}/{type}/{id}   {JSON}
    POST /{index}/{type}/       {JSON}
    
###检查索引是否存在###

采用HEAD请求，同时返回HTTP状态码来确定

    HEAD /website/blog/123
    
存在返回200，不存在404

    HTTP/1.1 200 OK
    Content-Type: text/plain; charset=UTF-8
    Content-Length: 0
    
或者

    HTTP/1.1 404 Not Found
    Content-Type: text/plain; charset=UTF-8
    Content-Length: 0
    
###重建索引###

和创建索引采用相同的API

    PUT /{index}/{type}/{id}  {JSON}

不同的是，返回版本号发生变化，`_create`属性为`false`:

    {
      "_index" :   "website",
      "_type" :    "blog",
      "_id" :      "123",
      "_version" : 2,
      "created":   false
    }   

由此看来，如果创建用该API存在弊端: 不知道该id对应的文档是否已经创建。通常在创建的场景是如果已经存在那么就不要创建成功，而不是覆盖。可以通过参数来达到：

    PUT /website/blog/123?op_type=create
    PUT /website/blog/123/_create
    
这样如果实际创建成功返回`201 Created`否则返回`409`
