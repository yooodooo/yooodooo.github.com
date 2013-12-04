---
layout: post
title: JAX-WS �����򵥵�webservice
tagline: [webservice] 
group: webservice
---
{% include JB/setup %}

## ʲô��JAX-WS ##
JAX-WS (JavaTM API for XML-Based Web Services)�淶��һ��XML web services��JAVA API��JAX-WS�������߿���ѡ��RPC-oriented����message-oriented ��ʵ���Լ���web services��JAX-WS2.0 (JSR 224)��Sun�µ�web servicesЭ��ջ����һ����ȫ���ڱ�׼��ʵ�֡���binding�㣬ʹ�õ���the Java Architecture for XML Binding (JAXB, JSR 222)����parsing�㣬ʹ�õ���the Streaming API for XML (StAX, JSR 173)��ͬʱ������ȫ֧��schema�淶����JDK6.0֮���Ѿ����ϵ�SDK�У������������µ�ʵ��ֻ��Ҫ��JDK6�󼴿ɣ�����Ҫ����Ӷ����������
���������ҵ���JAX-WS��ԭ��ͼ
![]({{ ASSET_PATH }}/img/39102de9-613b-3e9f-ac62-f26946fe4072.png)
Ϊ�˷��㴫�͹�����������������http://blog.csdn.net/kkdelta/article/details/4017747��

>1���ͻ��˿�����ͨ��URL�õ�WSDL�ļ���(ͨ��HTTP���ʿ��Եõ�,http://<endpoint-address>?wsdl) 
2���ͻ��˸���WSDL������,ͨ��HTTP POST����SOAP��Ϣ���������ˡ� 
3����������Listener���ܵ�SOAP������Ϣ,��JAVA��˵,ͨ����һ��servlet����EJB��Listener����Ϣת���� Dispatcher,��ʱ��listener��DispatcherҲ������ͬһ���ࡣDispatcher���������Ϣ����WebService�������նˡ� 
4����ʱ��,�������˻ὫHTTP requestת�ɷ������˵���Ϣ����,�γ�javax.xml.ws.handler.MessageContext,������SOAP��Ϣ��ͷ��Ϣ,��mustUnderstand�� 
5������ڷ�������������handler,�����handler��handleMessage����,ͨ����handler��������Ϣ,���ܻ��߱�֤��Ϣ�����˳��handlerͨ����@HandlerChain��ע����handlers.xml�ļ�����Ϊ�� 

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
 
6��SOAP��Ϣ�������л�ΪJAVA����,����������ʵ��ҵ����ࡣ 
7������������ҵ�񷽷�,ִ�к�����JAXBע�����л���SOAP������Ϣ�� 
8�����������handler,�����handler��handleMessage���������ҵ�񷽷����쳣�׳�,���쳣תΪSOAP fault ��Ϣ�� 
9��Listenerͨ��HTTP��response���ظ��ͻ��ˡ� 
��������:����������һ��Requset XML(SOAP)-->JAXB-->JAVA Object-->JAXB-->Response XML(SOAP)�Ĺ��� 

## ��һ�����ӿ�ʼ ##
ֻ��Ҫ�����򵥵�annotation���ɷ���һ������JAX-WS��webservice����

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
	
��һ��main�����з����÷���

	Endpoint.publish("http://localhost:8080/service/helloJaxWs", new HelloJAXWSImpl());  

�������ǾͿ����������ͨ�������URL���ʣ�http://localhost:8080/service/helloJaxWs?wsdl����������ص�xml��ʽ���ĵ���ʾ���Ǿͷ����ɹ���

�����������ͻ��˴��룬��JDK��Ҳ�ṩ����Ӧ�Ĺ���������wsdl���ɿͻ��˴��������wsimport������ͨ��wsimport -help���鿴��Ӧ�Ĳ�����������������¼�������

	wsimport -verbose -p com.sample.sws.ws -keep http://localhost:8080/service/helloJaxWs?wsdl  
	-verbose ��ʾ�������Ϣ  
	-p ָ���ͻ���package  
	-keep �������ɿͻ��˵�Դ�ļ��ͱ���õ�class�ļ�  

��ָ����package�¾Ϳ��Կ������ɵ�service�����࣬��Ȼ��������������֮ǰ`@WebService()`��Ĳ������ã�������server�˵ķ������������£�

	@WebService(serviceName = "helloJaxWsService", endpointInterface = "org.ws.server.ws.chap1.impl.HelloJAXWSImpl")  
	@SOAPBinding(style = Style.DOCUMENT)  

���ɵĿͻ��˴�����Ҳ�Ǹ��������serviceName��HelloJaxWsService
�����ǿͻ��˵��õĴ���

	HelloJAXWSImpl hellpJaxWs = new HelloJaxWsService().getHelloJAXWSImplPort();  
	System.out.println(hellpJaxWs.sayHello());  

�����������һ������webservice->����->���ɿͻ��˴���->�ͻ��˵��õ���������

## ���ݸ��Ӷ��� ##
������2�е������Ƿ���String������ʵJAX-WSԭ���Ը��Ӷ����кܺõ�֧�֣�������һЩҵ���ϵ�PO��List����ȡ���������һ�����ӣ�

	@WebService  
	public interface JaxWsService {  
	  
		public Address getAddress(String id);  
	  
		public Person getPerson(String id);  
	  
		List<Address> getAddressList(Address address);  
	}  
	
����������Address��Person��������Person����һ��List<Address>���ԣ�

	public class Person implements Serializable {  
		private static final long serialVersionUID = 8336803120311071811L;  
	  
		private String            username;  
		private Date              birthday;  
		private List<Address>     addresses;  
		   //����ʡ������  
	}  
	  
	public class Address implements Serializable {  
	  
		private static final long serialVersionUID = 5038515089191304705L;  
		private String            street;  
		private String            city;  
		private int               port;  
		   //����ʡ������  
	}  
	
�ڶ�����Ľӿ����򵥵�ʵ�֣�����������������ɿͻ��˴�����ü��ɴﵽԤ�ڵ�Ч�������ﲻ��׸�����ӿɼ����룬������Ҫ˵������JAX-WS�жԴ������ͨ������Ҫ���κδ����ɴ���
 
## wsimport��wsgen ##

����������λ��$JAVA_HOME$/binĿ¼
wsimport ǰ���Ѿ��õ�������ߣ���������һ�����ڵ�WSDL�ĵ������ɿͻ��˴��롣ͨ��wsimport��ѯ��������http://jax-ws.java.net/jax-ws-ea3/docs/wsimport.html
wsgen ���߻�����������һ��Web���������ɶ�Ӧ��WSDL�ĵ�

	@WebService  
	public class JaxWsWebServiceImpl implements JaxWsWebService {  
	  
		@WebMethod  
		public @WebResult  
		String getMessage() {  
			return "Hello";  
		}  
	  
	}  

	H:\git\webservice\sws\sws-server\target\classes>wsgen -verbose -keep -cp "." -wsdl org.ws.server.ws.chap8.impl.JaxWsWebServiceImpl  
	����6���ļ���  
	JaxWsWebServiceImplService.wsdl  
	JaxWsWebServiceImplService_schema1.xsd  
	org\ws\server\ws\chap8\impl\jaxws\GetMessage.java  
	org\ws\server\ws\chap8\impl\jaxws\GetMessage.class  
	org\ws\server\ws\chap8\impl\jaxws\GetMessageResponse.java  
	org\ws\server\ws\chap8\impl\jaxws\GetMessageResponse.class  
	
������ܼ�[����]() 

## Endpoint.publish��̽ ##

����������֪������һ��webservice����ֻ��Ҫͨ��

	public static Endpoint publish(String address, Object implementor)  

���ɣ���ʵ�����������JavaSE6�ṩ��һ����������HttpServer��λ��rt.jar�µ�com.sun.net.httpserver������ɼ�����ͬʱ���ﻹ��һ��Сʵ��