---
layout: post
title: Python 模块与导入
tagline: [python] 
group: Python
---
{% include JB/setup %}

## 模块与导入 ##

最开始，我们使用python的解释器编写代码，但是如果一旦关闭，那么我们写的函数，变量都将丢失。后来我们将一定功能的内容用文本保存，并用*.py保存。这就是脚本，也达到了重复利用的目的。而这样的文件我们通常也称为模块(modules),用来组织不同功能的列表。这与java中的package，c++中的namespace作用类似。而在python中我们使用import关键字来处理。



1. 首先我们写一个文件`sample_module.py`包括如下内容：

		def meth1(name):
			print(name)
			
		def meth2(name):
			print('hello, ', name)
		
	如果在其他模块中需要用到这里面的函数，可以通过两种方式：  
	- `import sample_module`
		
		这种方法引入新模块的内容存在于该模块的上下文空间中而不是当前执行模块的空间中，考虑：
		
			meth1('python')
			sample_module.meth1('python')
		
		前者将会抛出`NameError: name 'meth1' is not defined`,而后者能正常调用
	- `from sample_module import *`
	
		与上面不同的是这里将引入的模块导入了新模块的上下文中，可以直接调用如`meth1('python')`。这里采用了通配的`*`来导入，将会导入所有的方法，当然也可以指定需要的方法
	
			from sample_module import meth1,meth2
	
		针对导入多个模块如果存在重名的方法，可以用`as`来通过别名来区分
	
			import sample_module as sm
			from sample_module import meth1 as m,meth2 as mm

2. 接下来看看导入后当前默认上下文发生的变化，我们可以通过`dir()`查看：

		>>> dir()
		['__builtins__', '__doc__', '__loader__', '__name__', '__package__']
		>>> import sys
		>>> dir()
		['__builtins__', '__doc__', '__loader__', '__name__', '__package__', 'sys']
		
		>>> import sys as SYS
		>>> dir()
		['SYS', '__builtins__', '__doc__', '__loader__', '__name__', '__package__']
	
		>>> from sample_module import meth1 as m,meth2 as mm
		>>> dir()
		['__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', 'm', 'mm']

3. 插入与删除
	
	可以通过`del`对导入的模块进行删除
	
		>>> del SYS
		>>> dir()
		['__builtins__', '__doc__', '__loader__', '__name__', '__package__']
		
	通过`sys.path`动态导入模块
	
		>>> import sys
		>>> sys.path.insert(0,'c:/sa')
		>>> sys.path
		['c:/sa', '', 'C:\\Python33\\Lib\\idlelib', 'C:\\Windows\\system32\\python33.zip', 'C:\\Python33\\DLLs', 'C:\\Python33\\lib', 'C:\\Python33', 'C:\\Python33\\lib\\site-packages']
		>>> import sa
		>>> sa.meth1('aaa')
		aaa
	
	通过sys.path.insert(index, src_path)导入了目录c:/sa,这样该路径下的文件就作为模块导入了path中,通过import就导入了需要的模块,需要注意的是重名的问题。类似的还有
	
		>>sys.path.append("c:/sa")  	#加到路径最后
		>>sys.path.insert(0,"c:/sa")  	#加到路径最前
		>>sys.path.remove("c:/sa")  	#删除路径
	
	




	






