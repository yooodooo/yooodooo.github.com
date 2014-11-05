---
layout: post
title: Python入门指南(二)
tagline: [python] 
group: python
categories: [Python]
---
{% include codepiano/setup %}

## 数据结构

1、 序列

序列是Python中最基本的元素，列表中的每个元素下标从0开始

    a = [1, 'abc', 2, 3]
    b = [1, 2, [1, 2]]

索引与分片

    a[0]       --> 1
    a[4]       --> IndexError
    a[1:3]     --> ['abc', 2]
    a[::-1]    --> [3, 2, 'abc', 1]
    a[0:20]    --> [1, 'abc', 2, 3]

追加元素  
这里主要介绍3种方式

    a = [1,'a',2,'c']
    
    a.append(3)       --> a = [1, 'a', 2, 'c', 3]
    
    a[len(a):] = [3]   --> a = [1, 'a', 2, 'c', 3, 3]
    a[len(a):] = 'abc' --> a = [1, 'a', 2, 'c', 3, 3, 'a', 'b', 'c']
    
    a.extend([1,2,3])
    a.extend('abc')
    
    a.insert(0,'a')
    a.insert(16,'a')

注意对于`a[len(a):] = [3]`这种追加方式需要注意   
- 不能使用`a[len(a)+1]`来指定下标，这样会抛出`IndexError`
- 赋予的新值必须是可迭代的,如`a[len(a):] = 3`就会抛出`TypeError`，当然也支持多个元素的
`a[len(a):] = [3, 4, 5]`

对于`extend`参数也是接受可迭代的类型如列表`[1,2,3]`或者字符串`abc`。而对于`insert(i, x)`则是对指定的下标插入元素，注意下标可以大于列表的实际长度，但是也只是在列表后面追加，相应的长度增加1。


常用方法

    a = [1, 2, 1, 'a', 'b']
    len(a)  -->5 //长度
    a.count(1) --> 2 //元素个数
    a.count(3) --> 0 //元素不存在不报错返回0
    
    a.index(1) --> 0 //元素在列表中的下标，多个返回第一个
    a.index(3) -->   //元素不存在抛异常ValueError 
    
    a.remove(1) --> 多个元素只删除第一个
    a.remove(3) --> 不存在的元素抛出异常ValueError 
    
    a.pop(1)    --> 删除指定下标的元素，返回该元素这里为2
    a.pop(10)   --> 下标越界抛出IndexError

列表循环  
对于列表的循环，我们通常采用这种方式

    a = [1, 'a', 2, 'b']
    for i in a:
        print i

或者

    for i in range(len(a)):
        print a[i]

如果对列表只是一些简单的运算，可以考虑使用**列表推导**

    [x * x for x in range(3)]-->[0, 1, 4]

也可以用`if`进行过滤

    [x * x for x in range(3) if x % 2 == 0]-->[0, 4]

对多个`for`语句进行

    [(x, y) for x in range(3) for y in range(3)]

有时我们希望将列表处理为一个枚举，可采用列表推导和函数`enumerate([])`

    a = ['a', 'b', 'c', 'd', 'e']
    b = [(index, item) for index, item in enumerate(a)]
    outputs
    [(0, 'a'), (1, 'b'), (2, 'c'), (3, 'd'), (4, 'e')]

这里有必要介绍下函数`range(start,end,step)`

range(start,end,step)  
返回指定范围的列表，包括起止范围和步长

    range(5)     --> [0, 1, 2, 3, 4]
    range(0,5)   --> [0, 1, 2, 3, 4]
    range(0,5,2) --> [0, 2, 4]

与str的区别  
通过上面的讲解我们会发现列表和字符串存在一定的相似地方，如索引，下标操作等。如：

    a = 'abc'
    b = ['a', 'b', 'c']
    
    a[0:1]
    b[0:1]
    
    a[::-1]
    b[::-1]
    
    a+"d"
    b + ['d']
    
    a*3
    b*3

以上操作都很类似只是结果返回不一样。当然也有不同的地方，列表本质是可变长的，因此对列表的操作中如`append, insert`等就不适合字符串。当然两者也可以很方便转换

    list(a) --> ['a', 'b', 'c']
    ''.join(b) --> abc


## 元组 

不可变序列即为元组

    (1,2,3)
    ('a','b','c')
    (1,)

如果只有一个元素需要有末尾的逗号`(,)`注意比较

    3 * (10 + 2)
    3 * (10 + 2,)

可以通过函数`tuple`来实现列表和元组的转换

    tuple('1bs3')
    tuple([1, 2, 3, 4])
    tuple((1, 2, 3,))
    tuple([1, 2, 3, 4, [1, 2]])

注意上面最后元素是可以包含可变元素的。对其中可变元素`[1,2]`操作与普通的列表没什么差别

    t = tuple([1, 2, 3, 4, [1, 2]])
    t[4].insert(0, 0)
    print t --> (1, 2, 3, 4, [0, 1, 2])

元组的循环可以使用上面的列表介绍的方法，但是由于元组不可变那么改变元组的方法是不可用的。会抛出异常`AttributeError`。

## 集合(set)

与其他语言一样，是一个无序不可重复的元素集。因此不支持下标、分片等类序列操作的方法

1、 创建集合

    s0 = {'h', 'e', 'l', 'l', 'o'}
    s1 = set('hello')
    s2 = set(['h', 'e', 'l', 'l', 'o'])

都返回`set(['h', 'e', 'l', 'o'])`与期望的一样不包括重复元素。同样作为集合元素也不能包含可变元素。像这样使用没问题`set(((1, 2), (3,)))`而`set([1, 2, 3, [1, 2]])`则会抛出异常`TypeError`。虽然集合不能包含可变元素，但作为本身是可变的(注意与元组比较)。

    s1.add('a')         --> set(['a', 'h', 'e', 'l', 'o'])
    s1.add(['a','b'])   --> TypeError不支持可变元素

当然也有不可变的集合`frozenset`

    >>> cities = frozenset(["Frankfurt", "Basel","Freiburg"])
    >>> cities.add("Strasbourg")
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    AttributeError: 'frozenset' object has no attribute 'add'

2、 常用方法
一张图来回忆下集合的常用操作

![sets_with_notations.png](C:/Users/dong/Desktop/sets_with_notations.png "")

    s0.remove('h')   -->s0: set(['e', 'l', 'o'])
    s0.remove('hs')  -->不存在的元素抛出KeyError
    
    s0.discard('h')  -->s0: set(['e', 'l', 'o'])
    s0.discard('hs') -->不存在的元素不出错原来一致
    
    s0.clear()       -->s0: set([])

交集(`difference()`)

    >>> x = {"a","b","c","d","e"}
    >>> y = {"b","c"}
    >>> z = {"c","d"}
    >>> x.difference(y)
    set(['a', 'e', 'd'])
    >>> x.difference(y).difference(z)
    set(['a', 'e'])

也可以更简单的处理方式

    >>> x - y
    set(['a', 'e', 'd'])
    >>> x - y - z
    set(['a', 'e'])
    
补集(`difference_update`)

    x = {"a","b","c","d","e"}
    y = {"b","c"}
    x.difference_update(y)
    x --> set(['a', 'e', 'd'])
    z = x - y
    z --> set(['a', 'e', 'd'])
    
或者

    x = {"a", "b", "c", }
    y = {"c", "d", "e"}
    print x - y
    print x | y
    print x & y
    
    outputs
    set(['a', 'b'])
    set(['a', 'c', 'b', 'e', 'd'])
    set(['c'])


## 字典

字典与java中的map作用类似首先来看如何创建一个字典

    d1 = {}
    d2 = {'name':'python', 'version':'2.7.6'}
    d3 = dict((['name', 'python'], ['version', 2.7]))
    d4 = dict(name='python', version=2.7)
    d5 = {}.fromkeys(('x', 'y'), -1)

对`fromkeys`接收两个参数，前面作为key后面相应的为value，如果只有一个参数那么就是这样：
`d5 = {}.fromkeys(('x', 'y')) #{'x': None, 'y': None}`

1、 遍历访问

    d2 = {'name':'python', 'version':'2.7.6'}
    for key in d2:
        print "key=%s, value=%s" % (key, d2[key])
    
    for key in d2.iterkeys():
        print "key=%s, value=%s" % (key, d2[key])
    
    for key in d2.keys():
        print "key=%s, value=%s" % (key, d2[key])
    
    for key in iter(d2):
        print "key=%s, value=%s" % (key, d2[key])
    
    for key, item in d2.items():
        print "key=%s, value=%s" % (key, item) 

以上的遍历是通过key,value或iter的访问。当然也支持对单个key的访问，如`d2['name']或d2.get(`name`)`对于后者也可以传入默认的value，如果key不存在时返回该值，此时该字典并不改变

    d2.get('age', 10) >> 10
    print d2          >> {'version': '2.7.6', 'name': 'python'}

2、 转换
常见的转换，可以将字典转换为列表(list)和迭代器(iterator)

    dict={'name':'python', 'version':'2.7.6'}
    #list
    dict.items()  >>[('version', '2.7.6'), ('name', 'python')]
    dict.keys()   >>['version', 'name']
    dict.values() >>['2.7.6', 'python']
    #iterator
    dict.iteritems()
    dict.iterkeys()
    dict.itervalues()

3、 常见方法

- **dict.copy()**  #返回字典(浅复制)的一个副本:原字典发生改变不影响新的字典
- **dict.clear()** #清空字典中所有元素：{}
- **dict1.has_key("name")** #判断是否有键，返回True或False
- **dict.pop(key[, default])** #如果字典中key存在，删除并返回`dic[key]`,如果key不存在，`default`没赋值将抛出`KeyError`


    dict = {'name':'python', 'version':'2.7.6'}
    dict.pop("name")
    dict.pop("name","python")
    dict.pop("name")
    
- **dict.setdefault(key,default=None)** 和方法`set()`相似，如果字典中不存在key 键，由`dict[key]=default`为它赋值
- **dict.update(dict2)** 将字典dict2 的键-值对添加到字典dict

4、 创建进阶的字典

针对字典中key是列表，可这样操作`dict = {'name':['python','java']}`可用`setdefault()`来实现

    dict = {};
    dict.setdefault("name",[]).append("python");
    dict.setdefault("name",[]).append("java");

或者用集合中的`defaultdict`

    from collections import defaultdict
    d = defaultdict(list);
    d['name'].append('python')
    d['name'].append('java')

上面使用的集合模块同样提供了有序集合`OrderedDict`

    d = OrderedDict()
    d['name'] = 'python'
    d['age'] = 20


5、 字典的计算

- 几种计算

        d = {
        	 'iphone5':5000,
        	 'g4':4000,
        	 'mi3':1999,
        	 'mi2s':1699
        	 }
        min(d)  //key按照字母顺序排列的最小
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
        set(a.keys()) & set(b.keys()) #{'y', 'z'}
        set(a.keys()) - set(b.keys()) #{'x'}
        set(a.items()) & set(b.items()) #{('y', 2)}

- 根据条件生产新的字典

    	a = {'x':1, 'y':2, 'z':3}
    	{key:a[key] for key in set(a.keys()) - {'z'}} #{'x': 1, 'y': 2}

	生成新的字典中将不包含`key：z`

参考
---

- [https://docs.python.org/2.7/](https://docs.python.org/2.7/)
- [http://blog.jobbole.com/68256/](http://blog.jobbole.com/68256/) 
- [http://blog.jobbole.com/69834/](http://blog.jobbole.com/69834/) 
- [http://www.python-course.eu/course.php](http://www.python-course.eu/course.php)  
- [http://blog.segmentfault.com/qiwsir](http://blog.segmentfault.com/qiwsir)
- [http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000)