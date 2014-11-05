---
layout: post
title: Thrift idl描述和跨语言的web服务
tagline: [thrift] 
group: thrift
---
{% include codepiano/setup %}

从上文的描述我们知道需要两个步骤：

## 一、编写idl描述性 ##
thrift采用`IDL（Interface Definition Language）`来定义通用的服务接口，并通过生成不同的语言代理实现来达到跨语言、平台的功能。在thrift的IDL中，我们需要关注一下几点：

1. <strong>基本类型</strong>

与java中的`char，int，long`等基本类型一样，IDL中也有用来描述基本类型的定义

- bool 表示一个布尔值，取true或false
- byte 表示一个带符号的字节
- i16 表示一个带符号的16位整形
- i32 表示一个带符号的32位整形
- i64 表示一个带符号的64位整形
- double 表示一个带符号的64位整形
- string 表示一个不可知编码的文本或二进制串
 
2. <strong>结构</strong>

定义了一个通用的对象以此来跨语言。主要采用struct来描述。如下我们定义了一个User对象：

	struct User {  
	    1: i16 gender = 1,  
	    2: string username,  
	    3: string password,  
	    4: i32 id  
	}  

上面的结构定义与C语言的结构描述很相同，每个字段都有一个唯一标识：1,2,3,4(处于版本管理的原因，推荐使用上唯一标识)。同时还可以有默认值。可以将结构的fields设置为optional，即如果在没有设置值时将不会序列化
 
 
3. <strong>容器</strong>

thrift的容器是强类型容器，能够与常用语言中的容器想对应，并可使用java泛型的方式进行标注。在thrift中提供了三种容器：

- `list<type>` 一个有序元素列表。可翻译为`java ArrayList`或STL的`vector`，或者脚本语言中的原生数组，可包含重复数据
- `set<type>` 一个无序不重复元素集。与STL的`set`，java的`HashSet`，Python的`set`，或者PHP/Ruby中的原生`dictionary`
- `map<type1,type2>` 一个主键唯一键值映射表，翻译为STL的`map`，java `HashMap`，Python `dictionary`
 
4. <strong>异常</strong>

与结构的声明一致，唯一不同的是用`exception`关键字

	exception NotFoundException{  
	    1:i16 errorType,  
	    2:string message  
	}  
 
 
5. <strong>服务</strong>

一个服务的定义在语义上相当于面向对象编程中的一个接口。服务的定义如下：
	
	service <name> {  
	  <returntype> <name> (<arguments>)[throws (<exceptions>)]  
	  ...  
	}  
	
在方法的声明中有一个`oneway`修饰符，表示客户端只会触发一个请求，而不会监听任何响应，oneway的方法必须是void：
	
	oneway void zip()  
	
一个例子
	
	service UserService{  
	  void saveUser(1:User user),  
	  User get(1:i32 id) throws (1:NotFoundException nfe)  
	}  
	
同时service也可以从另外的service继承，需要使用关键字extends，这与java的继承关键字一样：
	
	service Calculator extends shared.SharedService   
 
## 二、其他的说明 ##

1. <strong>include</strong>通过include引用其他的thrift文件，默认在当前路径下寻找，也可以在相对路径下寻找，需要通过编译参数`-I`来设置
 
2. <strong>namespace</strong>与java的package作用一样：

		namespace java org.java.codelib.thrift.sa  
		namespace python org.java.codelib.thrift.sa  

	上面指定了java和python的package，在只有`--gen java/python`时将会生产响应的package
 
3. 指定常量

		const i32 INT32CONSTANT = 9853  
		const map<string,string> MAPCONSTANT = {'hello':'world', 'goodnight':'moon'}  

	或枚举：
	
		enum Operation {  
		  ADD = 1,  
		  SUBTRACT = 2,  
		  MULTIPLY = 3,  
		  DIVIDE = 4  
		}  

## 三、一个例子 ##

下面的例子是thrift分发包中的tutorial，这里对package做了些修改，其他保持不变。这里分别生成python和java的代码，服务端采用python客户端用java来实现跨语言的调用

1. shared.thrift

		namespace java org.java.codelib.thrift.sa  
		namespace py shared  
		  
		struct SharedStruct {  
		    1: i32 key  
		    2: string value  
		}  
		  
		service SharedService {  
		    SharedStruct getStruct(1: i32 key)  
		}  
 
2. tutorial.thrift
 
		include "shared.thrift"  
		  
		namespace java org.java.codelib.thrift.sa  
		namespace py tutorial  
		  
		typedef i32 MyInteger  
		  
		const i32 INT32CONSTANT = 9853  
		const map<string,string> MAPCONSTANT = {'hello':'world', 'goodnight':'moon'}  
		  
		enum Operation {  
		    ADD = 1,  
		    SUBTRACT = 2,  
		    MULTIPLY = 3,  
		    DIVIDE = 4  
		}  
		  
		struct Work {  
		    1: i32 num1 = 0,  
		    2: i32 num2,  
		    3: Operation op,  
		    4: optional string comment,  
		}  
		  
		exception InvalidOperation {  
		    1: i32 what,  
		    2: string why  
		}  
		  
		service Calculator extends shared.SharedService {  
		  
		    void ping(),  
		  
		    i32 add(1:i32 num1, 2:i32 num2),  
		  
		    i32 calculate(1:i32 logid, 2:Work w) throws (1:InvalidOperation ouch),  
		  
		    oneway void zip()  
		}  
 
3. 生成python和java的代理实现代码

		thrift>thrift-0.9.0.exe --gen py shared.thrift  
		thrift>thrift-0.9.0.exe --gen py tutorial.thrift  
		thrift>thrift-0.9.0.exe --gen java shared.thrift  
		thrift>thrift-0.9.0.exe --gen java tutorial.thrift  

4. `PythonServer.py`作为服务端python的实现：

		import sys  
		sys.path.append('../gen-py')  
		  
		from tutorial import Calculator  
		from tutorial.ttypes import *  
		  
		from shared.ttypes import SharedStruct  
		  
		from thrift.transport import TSocket  
		from thrift.transport import TTransport  
		from thrift.protocol import TBinaryProtocol  
		from thrift.server import TServer  
		  
		class CalculatorHandler:  
		  def __init__(self):  
		    self.log = {}  
		  
		  def ping(self):  
		    print 'ping()'  
		  
		  def add(self, n1, n2):  
		    print 'add(%d,%d)' % (n1, n2)  
		    return n1+n2  
		  
		  def calculate(self, logid, work):  
		    print 'calculate(%d, %r)' % (logid, work)  
		  
		    if work.op == Operation.ADD:  
		      val = work.num1 + work.num2  
		    elif work.op == Operation.SUBTRACT:  
		      val = work.num1 - work.num2  
		    elif work.op == Operation.MULTIPLY:  
		      val = work.num1 * work.num2  
		    elif work.op == Operation.DIVIDE:  
		      if work.num2 == 0:  
		        x = InvalidOperation()  
		        x.what = work.op  
		        x.why = 'Cannot divide by 0'  
		        raise x  
		      val = work.num1 / work.num2  
		    else:  
		      x = InvalidOperation()  
		      x.what = work.op  
		      x.why = 'Invalid operation'  
		      raise x  
		  
		    log = SharedStruct()  
		    log.key = logid  
		    log.value = '%d' % (val)  
		    self.log[logid] = log  
		  
		    return val  
		  
		  def getStruct(self, key):  
		    print 'getStruct(%d)' % (key)  
		    return self.log[key]  
		  
		  def zip(self):  
		    print 'zip()'  
		  
		handler = CalculatorHandler()  
		processor = Calculator.Processor(handler)  
		transport = TSocket.TServerSocket(port=9090)  
		tfactory = TTransport.TBufferedTransportFactory()  
		pfactory = TBinaryProtocol.TBinaryProtocolFactory()  
		  
		server = TServer.TSimpleServer(processor, transport, tfactory, pfactory)  
		  
		# You could do one of these for a multithreaded server  
		#server = TServer.TThreadedServer(processor, transport, tfactory, pfactory)  
		#server = TServer.TThreadPoolServer(processor, transport, tfactory, pfactory)  
		  
		print 'Starting the server...'  
		server.serve()  
		print 'done.'  

5. `CalculatorClient`作为java的客户端实现：

		public class CalculatorClient {  
		  
		    public static void main(String[] args) {  
		        TTransport transport;  
		        try {  
		            transport = new TSocket("localhost", 9090);  
		            TProtocol protocol = new TBinaryProtocol(transport);  
		            Calculator.Client client = new Calculator.Client(protocol);  
		            transport.open();  
		            System.out.println(client.add(12, 20));  
		            transport.close();  
		        } catch (TTransportException e) {  
		            e.printStackTrace();  
		        } catch (TException e) {  
		            e.printStackTrace();  
		        }  
		    }  
		  
		}  

6. 更多例子见thrift分发包下的tutorial
