---
layout: post
title: Thrift入门
tagline: [thrift] 
group: thrift
---
{% include codepiano/setup %}

## 一、什么是thrift ##
Thrift的[官网](http://thrift.apache.org/download/)。Thrift是由 Facebook 开发的远程服务调用框架 Apache Thrift，它采用接口描述语言定义并创建服务，支持可扩展的跨语言服务开发，所包含的代码生成引擎可以在多种语言中，如`C++, Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, Smalltalk`等创建高效的、无缝的服务，其传输数据采用二进制格式，相对 XML 和 JSON 体积更小，对于高并发、大数据量和多语言的环境更有优势
 
## 二、安装 ##
这里是windows下的安装过程

1、下载
[主页](https://dist.apache.org/repos/dist/release/thrift/0.9.0/thrift-0.9.0.tar.gz)下载最新的release包和windows下的编译文件，该文件负责将idl文件编译成各种语言的代码：https://dist.apache.org/repos/dist/release/thrift/0.9.0/thrift-0.9.0.exe
 
2、作为java的实现语言
如果是maven管理需要加入依赖：

	<dependency>  
	  <groupId>org.apache.thrift</groupId>  
	  <artifactId>libthrift</artifactId>  
	  <version>0.9.0</version>  
	</dependency>  
 
3、作为python的实现语言
在windows下需要语言安装python是必须的，同时需要有安装模块的工具，这里用setuptools：根据具体的平台找到相应的工具，[这里](http://pypi.python.org/packages/2.7/s/setuptools/setuptools-0.6c11.win32-py2.7.exe#md5=57e1e64f6b7c7f1d2eddfc9746bbaf20)下载后安装，将python安装目录下的script设置为windows的PATH变量，如

接下来进入到解压后thrift的release包目录，如`F:\thrift\thrift-0.9.0\lib\py`，在命令控制台运行：

	setup.py install  
	
即完成了安装
 
## 三、简单的实例 ##

根据IDL语言编写thrift的描述文件Hello.thrift

	namespace java org.java.codelib.thrift.sample  
	  
	service Hello {  
		i32 add(1:i32 para1, 2:i32 para2),  
		void sayHello(1:string name);  
	}  
	
其中

- namespace指明了包结构，这里会生成的java包为`org.java.codelib.thrift.sample`
- service相当于java中的接口描述，会将Hello生成具体语言的接口
- add和sayHello声明了两个方法：其中add接收两个参数并返回结果，而sayHello接收string类型的参数，这里的参数类型string、i32等与java中的String、int类似
利用thrift-0.9.0.exe文件编译成java实现代码：`thrift-0.9.0.exe --gen java Hello.thrift`
	
这样在目录下生成了gen-java的文件夹，包含了java的实现代码Hello.java
接下来需要实现Hello.java文件中的Hello.Iface 接口:

	public class HelloImpl implements Hello.Iface {  
	  
		@Override  
		public int add(int para1, int para2) throws TException {  
			return para1 + para2;  
		}  
	  
		@Override  
		public void sayHello(String name) throws TException {  
			System.out.println("Hello Thrift From " + name);  
		}  
	  
	}  

创建服务端代码：

	public class HelloServiceServer {  
	  
		/** 
		 * @param args 
		 * @author Administrator 
		 * @date 2013-1-26 
		 */  
		public static void main(String[] args) {  
			try {  
				// 设置服务端口为 7911   
				TServerSocket serverTransport = new TServerSocket(7911);  
				// 设置协议工厂为 TBinaryProtocol.Factory   
				Factory proFactory = new TBinaryProtocol.Factory(true, true);  
				// 关联处理器与 Hello 服务的实现  
				Hello.Processor<HelloImpl> processor = new Hello.Processor<HelloImpl>(new HelloImpl());  
	  
				Args arg = new Args(serverTransport);  
				arg.processor(processor);  
				arg.protocolFactory(proFactory);  
				TServer server = new TSimpleServer(arg);  
				server.serve();  
				System.out.println("Start server on port 7911...");  
			} catch (TTransportException e) {  
				e.printStackTrace();  
			}  
		}  
	  
	}  
	
下面是客户端的代码：

	public class HelloServiceClient {  
	  
		/** 
		 * @param args 
		 * @author Administrator 
		 * @date 2013-1-26 
		 */  
		public static void main(String[] args) {  
			TTransport transport;  
			try {  
				transport = new TSocket("localhost", 7911);  
				TProtocol protocol = new TBinaryProtocol(transport);  
				Hello.Client client = new Hello.Client(protocol);  
				transport.open();  
				System.out.println(client.add(12, 20));  
				client.sayHello("robin");  
				transport.close();  
			} catch (TTransportException e) {  
				e.printStackTrace();  
			} catch (TException e) {  
				e.printStackTrace();  
			}  
		}  
	  
	}  
	
先运行服务端的代码，再运行客户端就可以在控制台看到相应的输出。这样就完成了一个thrift的简单实例
 
## 四、Thrift架构简介 ##

这是thrift的架构图：
 
- 黄色部分是用户实现的业务逻辑，
- 褐色部分是根据Thrift 定义的服务接口描述文件生成的客户端和服务器端代码框架，
- 红色部分是根据Thrift 文件生成代码实现数据的读写操作。
- 红色部分以下是 Thrift 的传输体系、协议以及底层 I/O 通信，使用 Thrift 可以很方便的定义一个服务并且选择不同的传输协议和传输层而不用重新生成代码

下面是thrift的网络协议栈：

	+-------------------------------------------+  
	| Server                                    |  
	| (single-threaded, event-driven etc)       |  
	+-------------------------------------------+  
	| Processor                                 |  
	| (compiler generated)                      |  
	+-------------------------------------------+  
	| Protocol                                  |  
	| (JSON, compact etc)                       |  
	+-------------------------------------------+  
	| Transport                                 |  
	| (raw TCP, HTTP etc)                       |  
	+-------------------------------------------+  
	  
从上面可以看到，主要包括几个部分：

## Transport ##

常见的传输层有

- `TSocket` —— 使用阻塞式 I/O 进行传输，是最常见的模式
- `TNonblockingTransport` —— 使用非阻塞方式，用于构建异步客户端

		TNonblockingServerTransport serverTransport = new TNonblockingServerSocket(10005);  
		TCompactProtocol.Factory proFactory = new TCompactProtocol.Factory();  
		Hello.Processor<HelloImpl> processor = new Hello.Processor<HelloImpl>(new HelloImpl());  
		  
		Args args = new org.apache.thrift.server.TNonblockingServer.Args(serverTransport);  
		args.processor(processor);  
		args.protocolFactory(proFactory);  
		  
		TServer server = new TNonblockingServer(args);  
		System.out.println("Start server on port 10005 ...");  
		server.serve();  
 
- `TFramedTransport` —— 使用非阻塞方式，按块的大小进行传输，类似于 Java 中的 NIO。若使用 TFramedTransport 传输层，其服务器必须修改为非阻塞的服务类型。如服务端采用
- `TnonblockingTransport` 见上面的代码，下面是客户端的写法：

		TTransport transport = new TFramedTransport(new TSocket("localhost", 10005));  
		TProtocol protocol = new TBinaryProtocol(transport);  
		  
		Hello.Client client = new Hello.Client(protocol);  
		transport.open();  
		client.add(12, 20);  
		client.sayHello("robin");  
		transport.close();  
 
 
## Protocol ##
协议定义了内存和网络传输格式之间的映射，也可理解为不同传输结构见的解码和编码的约定。在thrift中常用的有`JSON, XML, plain text, compact binary` 等。

1. `binary`: 相当简单的二进制编码：将filed和对应的value合并在一起简单的二进制编码TBinaryProtocol

	Server：
		
		// 设置协议工厂为 TBinaryProtocol.Factory   
		Factory proFactory = new TBinaryProtocol.Factory(true, true);  
	
	Client：
	 
		TProtocol protocol = new TBinaryProtocol(transport);  
 
2. `compact`：https://issues.apache.org/jira/browse/THRIFT-110高效率的、密集的二进制编码格式进行数据传输

	server：
	
		TCompactProtocol.Factory proFactory = new TCompactProtocol.Factory(); 
	 
	Client：
	
		TCompactProtocol protocol = new TCompactProtocol(transport);  
 
3. `json`TJSONProtocol 使用 JSON 的数据编码协议进行数据传输与上面的一样：
	
		TJSONProtocol.Factory proFactory = new TJSONProtocol.Factory();  
		TJSONProtocol protocol = new TJSONProtocol(transport);  

## Processor ##

一个Processor类似一个管道，主要处理输入输出流。在thrift中输入、输出流由Protocol对象来表示，Processor接口非常简单：

	interface TProcessor {  
		bool process(TProtocol in, TProtocol out) throws TException  
	}  
 

Thrift服务的特定processor的实现在编译时(生成具体语言的代理实现)生成。Processor主要的流程是读取网络中传送的输入流，处理读取的stream(用户实现的handler)并将相应结构返回给调用方

## Server ##
一个服务就是讲上述几个特性整合起来，具体的描述如下:

- Create a transport
- Create input/output protocols for the transport
- Create a processor based on the input/output protocols
- Wait for incoming connections and hand them off to the processor