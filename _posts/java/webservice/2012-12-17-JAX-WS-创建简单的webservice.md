---
layout: post
title: JAX-WS 创建简单的webservice
tagline: [webservice] 
group: webservice
---
{% include JB/setup %}

## 什么是JAX-WS ##
JAX-WS (JavaTM API for XML-Based Web Services)规范是一组XML web services的JAVA API。JAX-WS允许开发者可以选择RPC-oriented或者message-oriented 来实现自己的web services。JAX-WS2.0 (JSR 224)是Sun新的web services协议栈，是一个完全基于标准的实现。在binding层，使用的是the Java Architecture for XML Binding (JAXB, JSR 222)，在parsing层，使用的是the Streaming API for XML (StAX, JSR 173)，同时它还完全支持schema规范。在JDK6.0之后已经整合到SDK中，所以我们以下的实例只需要在JDK6后即可，不需要再添加额外的依赖包
这是网上找到的JAX-WS的原理图
