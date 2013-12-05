---
layout: post
title: Python 可变参数args与kwds
tagline: [python] 
group: Python
---
{% include JB/setup %}

以前在用到mechanize和调用win32接口时遇到大量签名为*args, **kwds的方法，如：
mechanize的模块_form.py中有如下代码：

	def ParseString(text, base_uri, *args, **kwds):  
		fh = StringIO(text)  
		return ParseFileEx(fh, base_uri, *args, **kwds)  
	  
	def ParseFileEx(file, base_uri,  
					select_default=False,  
					form_parser_class=FormParser,  
					request_class=_request.Request,  
					entitydefs=None,  
					encoding=DEFAULT_ENCODING,  
	  
					# private  
					_urljoin=urlparse.urljoin,  
					_urlparse=urlparse.urlparse,  
					_urlunparse=urlparse.urlunparse,  
					):  
				
在简单的查阅后这个和java中的可变参数类似，如:

	public void foo(String ...name){}  
	
其中*args是一个可变长度的list，而**kwds为可变长度的字典。当然既然都是可变的，这两个作为方法签名也可以没有。虽然如此，在使用过程中遇到了不少问题，特将做的一些测试记录下来。
为了模拟上面的调用，定义了两个方法：

	def foo(name, *args, **kwds):  
		print "name= ", name  
		print "args= ", args  
		print "kwds= ",kwds  
		bar(name, *args, **kwds)  
		  
		  
	def bar(name, age, address,a="aaa", b="bbb"):  
		print "name= ",name  
		print "age= ", age  
		print "a= ", a  
		print "b= ", b  
		
可以想到，其中name为不变参数是不可缺少的，那么接下来的参数为list和dict，于是有了以下：

	foo("nico", ("10", "chengdu"), {"a":"aaaaaaaaa", "b":"vvvvvvvvvvvvv"})  
	
天真的以为对应了三个参数，可是打印的结果:

	name=  nico  
	args=  (('10', 'chengdu'), {'a': 'aaaaaaaaa', 'b': 'vvvvvvvvvvvvv'})  
	kwds=  {}  

可是与期望的不一样，而且除去第一个参数作为name的值，其他的都当做list处理了，经过处理，按照如下传递：

	foo("nico", "10", "chengdu", a="aaaaaaaaa", b="vvvvvvvvvvvvv")  
	
相应的结果为：

	foo  
	**********  
	name=  nico  
	args=  ('10', 'chengdu')  
	kwds=  {'a': 'aaaaaaaaa', 'b': 'vvvvvvvvvvvvv'}  
	  
	bar  
	**********  
	name=  nico  
	age=  10  
	a=  aaaaaaaaa  
	b=  vvvvvvvvvvvvv  
	
达到预期的效果了，虽然传递的形式觉得有点怪异。同时从对bar的签名中可以看出：

	bar(name, *args, **kwds)  
	def bar(name, age, address,a="aaa", b="bbb"):  
 
这里有签名age、address对应*args。a,b对应**kwds。也就是说list长度只能为2，dict的长度也只能为2。

	foo("nico", "10", a="aaaaaaaaa", b="vvvvvvvvvvvvv")   
	foo("nico", "10", "chengdu", a="aaaaaaaaa", b="vvvvvvvvvvvvv", c="aa")  

无一例外，都抛出异常
 
好了，这是网上找到的更多信息

	def foo(*args, **kwargs):  
		print 'args = ', args  
		print 'kwargs = ', kwargs  
		print '---------------------------------------'  
	  
	if __name__ == '__main__':  
		foo(1,2,3,4)  
		foo(a=1,b=2,c=3)  
		foo(1,2,3,4, a=1,b=2,c=3)  
		foo('a', 1, None, a=1, b='2', c=3)  
		
输出

	args =  (1, 2, 3, 4)   
	kwargs =  {}   
	---------------------------------------   
	args =  ()   
	kwargs =  {'a': 1, 'c': 3, 'b': 2}   
	---------------------------------------   
	args =  (1, 2, 3, 4)   
	kwargs =  {'a': 1, 'c': 3, 'b': 2}   
	---------------------------------------   
	args =  ('a', 1, None)   
	kwargs =  {'a': 1, 'c': 3, 'b': '2'}   
	---------------------------------------  