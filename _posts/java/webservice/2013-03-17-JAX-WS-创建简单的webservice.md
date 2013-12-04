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
![]({{ ASSET_PATH }}/img/39102de9-613b-3e9f-ac62-f26946fe4072.png)
为了方便传送过来【以下内容来自http://blog.csdn.net/kkdelta/article/details/4017747】

>1、客户端开发者通过URL得到WSDL文件。(通过HTTP访问可以得到,http://<endpoint-address>?wsdl) 
2、客户端根据WSDL的描述,通过HTTP POST发送SOAP消息给服务器端。 
3、服务器端Listener接受到SOAP请求消息,对JAVA来说,通常是一个servlet或者EJB。Listener把消息转发给 Dispatcher,有时候listener和Dispatcher也可能是同一个类。Dispatcher会把请求消息交给WebService的运行终端。 
4、这时候,服务器端会将HTTP request转成服务器端的消息类型,形成javax.xml.ws.handler.MessageContext,并处理SOAP消息的头信息,如mustUnderstand。 
5、如果在服务器端配置了handler,会调用handler的handleMessage方法,通常用handler来保存消息,解密或者保证消息到达的顺序。handler通过在@HandlerChain标注配置handlers.xml文件内容为： 

	<handler-chains xmlns="http://java.sun.com/xml/ns/javaee">   
	  <handler-chain>   
		   <handler>   
			   <handler-name>WSSOAPHandler</handler-name>   
			   <handler-class>com.cxf.test.WSSOAPHandler</handler-class>   
			   </handler>   
		   </handler-chain>   
	   <handler-chain>   
	   <handler>   
		   <handler-name>WSHandler</handler-name>   
		   <handler-class>com.cxf.test.WSHandler</handler-class>   
		 </handler>   
	   </handler-chain>   
	</handler-chains>   
 
6、SOAP消息被反序列化为JAVA对象,传到真正的实现业务的类。 
7、调用真正的业务方法,执行后利用JAXB注解序列化成SOAP返回消息。 
8、如果配置了handler,会调用handler的handleMessage方法。如果业务方法有异常抛出,把异常转为SOAP fault 消息。 
9、Listener通过HTTP把response返回给客户端。 
总体来讲:整个过程是一个Requset XML(SOAP)-->JAXB-->JAVA Object-->JAXB-->Response XML(SOAP)的过程 

## 从一个例子开始 ##
只需要几个简单的annotation即可发布一个基于JAX-WS的webservice服务：

	@WebService  
	public interface HelloJAXWS {  
	  
		public String sayHello();  
	}  
	  
	@WebService  
	public class HelloJAXWSImpl implements HelloJAXWS {  
	  
		@WebMethod  
		public String sayHello() {  
			return "Hello, JAX-WS";  
		}  
	  
	}  
	
在一个main函数中发布该服务：

	Endpoint.publish("http://localhost:8080/service/helloJaxWs", new HelloJAXWSImpl());  

这样我们就可以在浏览器通过上面的URL访问：http://localhost:8080/service/helloJaxWs?wsdl如果看到返回的xml格式的文档表示我们就发布成功。