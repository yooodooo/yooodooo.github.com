---
layout: post
title: Python dictionary
tagline: [python] 
group: Python
---
{% include codepiano/setup %}

## 1. 创建字典 ##

	>>> dict1={}
	>>> dict2={'name':'python','version':'3.3.2'}
	>>> dict3 = dict((['name','python'],['version',3.2]))
	>>> dict4=dict(name='python',version=2.3)
	>>> dict5 = {}.fromkeys(('x', 'y'), -1) #{'x': -1, 'y': -1}

`fromkeys`最多接收2个参数，如上面`dict5`所示，如果只有一个参数，那么相应的值为`None`

	>>> dict5 = {}.fromkeys(('x', 'y')) #{'x': None, 'y': None}

## 2. 访问值 ##

遍历访问：

	dict2={'name':'python','version':'3.3.2'}
	for key in dict2:
	    print "key=%s, value=%s" % (key, dict2[key])
	    
	for key in dict2.iterkeys():
	    print "key=%s, value=%s" % (key, dict2[key])
	    
	for key in dict2.keys():
	    print "key=%s, value=%s" % (key, dict2[key])
	    
	for key in iter(dict2):
	    print "key=%s, value=%s" % (key, dict2[key])
	    
	for key,item in dict2.items():
	    print "key=%s, value=%s" % (key, item)

以上都是通过对key或迭代器等进行遍历。也可以通过key对指定的键进行访问，如

	dict2['name']  #python
	dict2.get('name')

其中get还接收一个default值，仅当该key不存在时，作为默认返回。但是此时的字典并没改变
	
	dict2.get("desc",'use python')

此时返回了`use python`，但是此时dict2并没有改变


## 3. 转换 ##

常见的转换，可以将字典转换为列表(list)和迭代器(iterator)
	
	dict={'name':'python', 'version':'3.3.2'}
    #list
    dict.items()
    dict.keys()
    dict.values()
    #iterator
    dict.iteritems()
    dict.iterkeys()
    dict.itervalues()

## 4. 常见方法 ##

- `dict.copy()` #返回字典(浅复制)的一个副本:原字典发生改变不影响新的字典
- `dict.clear()`#清空字典中所有元素：{}
- `dict1.has_key("name")`#判断是否有键，返回`True`或`False`
- `dict.pop(key[, default])`#如果字典中key存在，删除并返回`dic[key]`,如果key不存在，default没赋值将抛出`KeyError`

	    dict = {'name':'python', 'version':'3.3.2'}
	    dict.pop("name")
	    dict.pop("name","python")
	    dict.pop("name")

- `dict.setdefault(key,default=None)` 和方法set()相似，如果字典中不存在key 键，由`dict[key]=default` 为它赋值
- `dict.update(dict2)` 将字典dict2 的键-值对添加到字典dict 


## 5. more ##

几种进阶的字典操作
	
- key对应多个value。其实这种场景的本质是value是一个列表，如：
	
		dict = {'name':['python','java']}

	可以用`setdefault()`方法来实现：

	    dict = {};
	    dict.setdefault("name",[]).append("python");
	    dict.setdefault("name",[]).append("java");

	或者用集合中的`defaultdict`
	
		from collections import defaultdict
		d = defaultdict(list);
		d['name'].append('python')
		d['name'].append('java')

- 有序的字典:集合模块同样提供了`OrderedDict`

		d = OrderedDict()
		d['name'] = 'python'
		d['age'] = 2

	这样的顺序是按照输入的key排列

- 对字典进行计算
	
	- 几种计算
	
			d = {
				 'iphone5':5000,
				 'g4':4000,
				 'mi3':1999,
				 'mi2s':1699
				 }
			min(d)  # key按照字母顺序排列的最小
			min(d.values())  # values最小
			
			min(zip(d.values(), d.keys()))  # 最小的序列，按照values排序。
			max(zip(d.values(), d.keys()))
			sorted(zip(d.values(), d.keys()))  # 按照values排序
			min(d, key=lambda k:d[k])#获取values最小的key
	
		上面用到了几个内置函数：`zip`,`min`,`max`,`sorted`:
	
		`zip`:接受可迭代的参数转换为元组或列表，如果长度不一样返回最小的列
	
			d = {
		         'iphone5':5000,
		         'g4':4000
		        }
			zip(d, d.values())
			zip([1,2],[1,2,3])

	- 找到相同的
	
			a = {'x':1,'y':2,'z':3}
			b = {'a':4,'y':2,'z':4}
			a.keys() & b.keys() #{'y', 'z'}
			a.keys() - b.keys() #{'x'}
			a.items() & b.items() #{('y', 2)}
		
	- 根据条件生产新的字典
	
			{key:a[key] for key in a.keys() - {'z'}} #{'x': 1, 'y': 2}
		
		生成新的字典中将不包含`key：z`