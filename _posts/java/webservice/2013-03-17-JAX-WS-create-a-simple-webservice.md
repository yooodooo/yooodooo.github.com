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

下面来看看客户端代码，在JDK中也提供了相应的工具来根据wsdl生成客户端代码这就是wsimport，可以通过wsimport -help来查看相应的参数。这里采用了以下几个参数

	wsimport -verbose -p com.sample.sws.ws -keep http://localhost:8080/service/helloJaxWs?wsdl  
	-verbose 显示编译的信息  
	-p 指定客户端package  
	-keep 保存生成客户端的源文件和编译好的class文件  

在指定的package下就可以看到生成的service代理类，当然具体的类名会根据之前`@WebService()`后的参数设置，比如我server端的服务类声明如下：

	@WebService(serviceName = "helloJaxWsService", endpointInterface = "org.ws.server.ws.chap1.impl.HelloJAXWSImpl")  
	@SOAPBinding(style = Style.DOCUMENT)  

生成的客户端代理类也是根据上面的serviceName来HelloJaxWsService
下面是客户端调用的代码

	HelloJAXWSImpl hellpJaxWs = new HelloJaxWsService().getHelloJAXWSImplPort();  
	System.out.println(hellpJaxWs.sayHello());  

这样就完成了一个创建webservice->发布->生成客户端代码->客户端调用的整个过程

## 传递复杂对象 ##
从上面2中的例子是返回String对象，其实JAX-WS原生对复杂对象都有很好的支持，如我们一些业务上的PO或List对象等。下面来看一个例子：

	@WebService  
	public interface JaxWsService {  
	  
		public Address getAddress(String id);  
	  
		public Person getPerson(String id);  
	  
		List<Address> getAddressList(Address address);  
	}  
	
这里申明了Address和Person对象，其中Person包含一个List<Address>属性：

	public class Person implements Serializable {  
		private static final long serialVersionUID = 8336803120311071811L;  
	  
		private String            username;  
		private Date              birthday;  
		private List<Address>     addresses;  
		   //以下省略若干  
	}  
	  
	public class Address implements Serializable {  
	  
		private static final long serialVersionUID = 5038515089191304705L;  
		private String            street;  
		private String            city;  
		private int               port;  
		   //以下省略若干  
	}  
	
在对上面的接口做简单的实现，按照上面的流程生成客户端代码调用即可达到预期的效果。这里不在赘述例子可见代码，这里想要说明的是JAX-WS中对大多数普通对象不需要做任何处理即可传输
 
## wsimport与wsgen ##

这两个工具位于$JAVA_HOME$/bin目录
wsimport 前面已经用到这个工具，用来解析一个存在的WSDL文档，生成客户端代码。通过wsimport查询参数或者http://jax-ws.java.net/jax-ws-ea3/docs/wsimport.html
wsgen 工具还可以用来从一个Web服务中生成对应的WSDL文档

	@WebService  
	public class JaxWsWebServiceImpl implements JaxWsWebService {  
	  
		@WebMethod  
		public @WebResult  
		String getMessage() {  
			return "Hello";  
		}  
	  
	}  

	H:\git\webservice\sws\sws-server\target\classes>wsgen -verbose -keep -cp "." -wsdl org.ws.server.ws.chap8.impl.JaxWsWebServiceImpl  
	生成6个文件：  
	JaxWsWebServiceImplService.wsdl  
	JaxWsWebServiceImplService_schema1.xsd  
	org\ws\server\ws\chap8\impl\jaxws\GetMessage.java  
	org\ws\server\ws\chap8\impl\jaxws\GetMessage.class  
	org\ws\server\ws\chap8\impl\jaxws\GetMessageResponse.java  
	org\ws\server\ws\chap8\impl\jaxws\GetMessageResponse.class  
	
详情介绍见[这里]() 

## Endpoint.publish初探 ##

从上面我们知道发布一个webservice我们只需要通过

	public static Endpoint publish(String address, Object implementor)  

即可，其实这后面依赖于JavaSE6提供了一个轻量级的HttpServer。位于rt.jar下的com.sun.net.httpserver，具体可见这里同时这里还有一个小实例