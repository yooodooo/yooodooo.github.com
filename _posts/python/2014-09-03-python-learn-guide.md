---
layout: post
title: Python入门 变量、函数与常见数据结构
tagline: [python] 
group: python
categories: [Python]
---
{% include codepiano/setup %}

变量与数据类型
--

> 变量的声明

```python
a = 1
b = -100
c = 12.3
d = 341341325234234523433333333333338888888888888L
e = 1235324523452346 * 999999999999999999999999

my_name = 'python'
_my_name = "Life is short, I use Python"
_my_name2 = 'I\'m "OK"!'

a1,b1,c1 = (1,2,3)
a2,b2,c2 = [1,2,3]
```
对于批量的命名需要注意变量个数和序列个数保存一致，否则会抛出`ValueError`更多用法
```python
a,b,c,d,e='hello'  ##必须为strings,files,iterators
_,age,_=['robin',10,('aaa',222,'ccc')] ##只关注age
```
或者
```python
a,b,*c=[1,2,3,4,5] #c=[3,4,5]
a,*b,c=[1,2,3,4,5] #b=[2,3,4]
```

> bool

采用`True, False`表示
```python
>>> True
True
>>> False
False
>>> 3 > 2
True
>>> 3 > 5
False
```
> 布尔运算

```python
True and True ##True
True and False ## False
False and False ## False

True or True ##True
True or False ##True
False or False ##False

not True ## False
not False ## True
```
---
数字与运算
--

> 类型

- int 整数，正整数或负整数 `type(10)--> <type 'int'>`
- long 长整数，无限大小。可以l或L结尾 `type(3452345234523452345234) --> <type 'long'>`
- float 浮点数 `type(2.1)--><type 'float'>, type(32.3e18) --> <type 'float'>`
- complex 复数 `type(1+2j)--><type 'complex'>`

> 算术运算

```python
7 + 2  >> 9
7 - 2  >> 5
7 * 2  >> 14
7 / 2  >> 3
7 % 2  >> 1   
```
注意`/`是取整，可以用浮点数除法或`from __future__ import division`在python3中处理为一样
```python
7.0 / 2  >> 3.5  
7 / 2.0  >> 3.5 
7. /2    >> 3.5
```
或  
```python
>>> from __future__ import division
>>> 7/2
3.5
```
再介绍一种运算`//`返回除法的整数部分
```python
7 // 2   >> 3
7.0 // 2 >> 3.0
```

> 比较、逻辑、布尔运算

常用比较运算(`==, !=, >, <, >=, <=`)  
布尔运算 `and, or, not`
```python
>>> 4>3 and 4>5
False
>>> 4>3 or 4>5
True
>>> not 4>3
False
>>> not 4>5
True
```
当然在python中比较并不只是这么简单，下面来个链式比较：
```python
>>> x = 5
>>> 1 < x < 10
True
>>> x < 10 < x*10 < 100
True
```
这才是你希望的书写方式
> 不存在整数溢出问题

```python
>>>123456789870987654321122343445567678890098*123345566778999009987654333238766544
15227847719352756287004435258757627686452840919334677759119167301324455281312L
```

> 神奇的`*`

```python
>>> a = 'abc'
>>> a*4
'abcabcabcabc'
>>> b = (1,2)
>>> b*2
(1, 2, 1, 2)
>>> c = [1,2]
>>> c*3
[1, 2, 1, 2, 1, 2]
```
---
字符串
--

> 申明

```python
a0 = "Hello"
a1 = 'world'
//运算
a = a0 + a1
a*2

//转义或doc方式
b = "what's your name?"
c = 'what\'s your name'
d = """what's your name?"""

//unicode
>>> e = u'你好'
>>> e
u'\xc4\xe3\xba\xc3'
```

> 字符串连接

```python
a = "my name is %s, and %d years old"%('abc',1)
b = "my name is %s"%'abc'
```

> 常用方法

```python
a = 'Hello World'
len(a)
//大小写转换    
a.upper()
a.lower()
a.capitalize()
//下标与数量   
a.index('e')
a.count('l')
a.strip();
a.rstrip()
```
> 分组、切片与其他

索引
```python
name = 'Hello'
name[0]  --> H
name[-2] --> l
name[6]  -->IndexError
```
切片
```python
name[0:3] --> 'Hel'
name[3:]  --> 'lo'
name[:]   --> 'Hello'
name[0:3:2] --> 'Hl' 步长为2
name[::-1]  --> 'olleH'
```
其他
```python
list(name) --> ['H', 'e', 'l', 'l', 'o']返回用"".join([])
>>> 'a' in name
False
>>> 'a' not in name
True
```
---
函数
--
> 函数定义

```python
import random

def guess_a_number(x):
    a = random.randrange(1, 100)
    if x > a:
        print 'Larger'
    elif x == a:
        print 'equal'
    else:
        pass
```
几点说明
- 用关键字`def`申明，不使用`{}`用缩进代替，注意后面的冒号(`:`)
- 注意参数个数和类型
- `pass`表示空实现

是否可以返回多个参数?答案是肯定的
```python
def return_multi_param(x):
    return x + 1, x + 2
    
a,b = return_multi_param(1)
```

> 可变参数

```python
def foo(name, *args, **kwds):  
    print "name= ", name  
    print "args= ", args  
    print "kwds= ",kwds  
```
可简单理解`*args`为可变列表, `**kwds`为可变字典
```python
foo("python", "10", "chengdu", a="aaaaaaaaa", b="vvvvvvvvvvvvv")
## outputs
name=  python  
args=  ('10', 'chengdu')  
kwds=  {'a': 'aaaaaaaaa', 'b': 'vvvvvvvvvvvvv'}   
```

> 默认参数

```python
def foo(name = 'abc'):  
    print "name= ", name 
## outputs
foo()         #name= abc
foo("lala")   #name= lala
```
**注意**如果默认参数是可变参数，那么可能导致和你预期不一样的结果
```
def foo(bar=[]):
    bar.append('bar')
    return bar
foo() >>['bar']
foo() >>['bar', 'bar']
foo() >>['bar', 'bar', 'bar']
```
这里调用三次后结果并不是预期那样：一个函数参数的默认值，仅仅在该函数定义的时候，被赋值一次。如此，只有当函数foo()第一次被定义的时候，才讲参数bar的默认值初始化到它的默认值（即一个空的列表）。当调用foo()的时候（不给参数bar），会继续使用bar最早初始化时的那个列表。
当然也有方式来避免这种方式
```
def foo(bar=[]):
    if not bar:
        bar = []
    bar.append('bar')
    return bar
```
更多信息见http://blog.jobbole.com/40088/

> 函数作为参数传递

```python
def get_text(name):
    return "lorem ipsum, {0} dolor sit amet".format(name)

def p_decorate(func):
    def func_wrapper(name):
        return "<p>{0}</p>".format(func(name))
    return func_wrapper

my_get_text = p_decorate(get_text)
## outputs
<p>lorem ipsum, John dolor sit amet</p>
```
这也可以用语法糖`@`来修饰
```python
@p_decorate 
def get_text(name):
    return "lorem ipsum, {0} dolor sit amet".format(name)
```
这样就和普通的调用一样即可`get_text("John")`。这就是Python中的修饰器，更多信息可参考
- http://thecodeship.com/patterns/guide-to-python-function-decorators/
- http://simeonfranklin.com/blog/2012/jul/1/python-decorators-in-12-steps

---
几个内置函数
--
> lambda函数

lambda函数即匿名函数,没有函数名通常直接调用。考虑下面这个普通函数:
```python
def squ(x):
    return x*x 
```
采用关键字`lambda`的实现
```python
g = lambda x:x*x
```

> filter(function, sequence)

对sequence中的item依次执行function(item)，将执行结果为True的item组成一个List/String/Tuple（取决于sequence的类型）返回

过滤序列中大于5小于10的数
```python
def largerThan(x):
    return x > 5 and x < 10
    
if __name__ == '__main__':
    x = filter(largerThan, [1,6,4,8,11])
    print x
```
通常与lambda结合使用
```python
filter(lambda i:i > 5 and i < 10, [1, 6, 4, 8, 11])
```
返回的结果与sequence的类型有关
```python
filter(lambda i:i > 5 and i < 10, [1, 6, 4, 8, 11])
filter(lambda i:i > 5 and i < 10, (1, 6, 4, 8, 11,))
filter(lambda x: x != 'a' and x != 'c', 'abcdefadc')
```

> map(function, sequence)

对sequence中的item依次执行function(item)，将执行结果组成一个List返回.与filter很类似,比较下两者的用法
```python
filter(lambda i:i > 5 and i < 10, [1, 6, 4, 8, 11])-->[6, 8]
map(lambda i:i > 5 and i < 10, [1, 6, 4, 8, 11])-->[False, True, False, True, False]
```
当然也支持多种参数的输入
```python
map(lambda i:i * i, [1, 6, 4, 8, 11])
map(lambda i: i + i, 'abcd')
```
可以看到map返回的序列和远序列的长度一致,只是类型会根据条件变化.同样支持多个序列
```python
map(lambda x, y: x + y, range(8), range(8))
```
如果函数为`None`时,返回序列
```python
map(None, range(3), range(3)) -->[(0, 0), (1, 1), (2, 2)]
map(None, range(2), range(3)) -->[(0, 0), (1, 1), (None, 2)]
```

> reduce(function, sequence, starting_value)

对sequence中的item顺序迭代调用function，如果有starting_value，还可以作为初始值调用.需要注意与上诉用法的区别，是对列表中的元素依次调用对应的function
```python
reduce(lambda x, y:x + y, range(4)) --> 0+1+2+3
reduce(lambda x, y:x + y, ['a','b','c','d'])
reduce(lambda x, y:x + y, range(4), 10) --> 0+1+2+3+10
```

---
数据结构
--

> 序列

序列是Python中最基本的元素，列表中的每个元素下标从0开始
```python
a = [1, 'abc', 2, 3]
b = [1, 2, [1, 2]]
```
##### 索引与分片
```python
a[0]       --> 1
a[4]       --> IndexError
a[1:3]     --> ['abc', 2]
a[::-1]    --> [3, 2, 'abc', 1]
a[0:20]    --> [1, 'abc', 2, 3]
```
##### 追加元素
这里主要介绍3种方式
```python
a = [1,'a',2,'c']

a.append(3)       --> a = [1, 'a', 2, 'c', 3]

a[len(a):] = [3]   --> a = [1, 'a', 2, 'c', 3, 3]
a[len(a):] = 'abc' --> a = [1, 'a', 2, 'c', 3, 3, 'a', 'b', 'c']

a.extend([1,2,3])
a.extend('abc')

a.insert(0,'a')
a.insert(16,'a')
```
注意对于`a[len(a):] = [3]`这种追加方式需要注意   
- 不能使用`a[len(a)+1]`来指定下标，这样会抛出`IndexError`
- 赋予的新值必须是可迭代的,如`a[len(a):] = 3`就会抛出`TypeError`，当然也支持多个元素的
`a[len(a):] = [3, 4, 5]`

对于`extend`参数也是接受可迭代的类型如列表`[1,2,3]`或者字符串`abc`。而对于`insert(i, x)`则是对指定的下标插入元素，注意下标可以大于列表的实际长度，但是也只是在列表后面追加，相应的长度增加1。


##### 常用方法
```python
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
```

##### 列表循环
对于列表的循环，我们通常采用这种方式
```python
a = [1, 'a', 2, 'b']
for i in a:
    print i
```
或者
```python
for i in range(len(a)):
    print a[i]
```
如果对列表只是一些简单的运算，可以考虑使用**列表推导**
```python
[x * x for x in range(3)]-->[0, 1, 4]
```
也可以用`if`进行过滤
```python
[x * x for x in range(3) if x % 2 == 0]-->[0, 4]
```
对多个`for`语句进行
```python
[(x, y) for x in range(3) for y in range(3)]
```
有时我们希望将列表处理为一个枚举，可采用列表推导和函数`enumerate([])`
```python
a = ['a', 'b', 'c', 'd', 'e']
b = [(index, item) for index, item in enumerate(a)]
outputs
[(0, 'a'), (1, 'b'), (2, 'c'), (3, 'd'), (4, 'e')]
```
这里有必要介绍下函数`range(start,end,step)`

##### range(start,end,step)
返回指定范围的列表，包括起止范围和步长
```python
range(5)     --> [0, 1, 2, 3, 4]
range(0,5)   --> [0, 1, 2, 3, 4]
range(0,5,2) --> [0, 2, 4]
```
##### 与str的区别
通过上面的讲解我们会发现列表和字符串存在一定的相似地方，如索引，下标操作等。如：
```python
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
```
以上操作都很类似只是结果返回不一样。当然也有不同的地方，列表本质是可变长的，因此对列表的操作中如`append, insert`等就不适合字符串。当然两者也可以很方便转换
```python
list(a) --> ['a', 'b', 'c']
''.join(b) --> abc
```

> 元组 

不可变序列即为元组
```python
(1,2,3)
('a','b','c')
(1,)
```
如果只有一个元素需要有末尾的逗号`(,)`注意比较
```python
3 * (10 + 2)
3 * (10 + 2,)
```
可以通过函数`tuple`来实现列表和元组的转换
```python
tuple('1bs3')
tuple([1, 2, 3, 4])
tuple((1, 2, 3,))
tuple([1, 2, 3, 4, [1, 2]])
```
注意上面最后元素是可以包含可变元素的。对其中可变元素`[1,2]`操作与普通的列表没什么差别
```python
t = tuple([1, 2, 3, 4, [1, 2]])
t[4].insert(0, 0)
print t --> (1, 2, 3, 4, [0, 1, 2])
```
元组的循环可以使用上面的列表介绍的方法，但是由于元组不可变那么改变元组的方法是不可用的。会抛出异常`AttributeError`。

> 集合(set)

与其他语言一样，是一个无序不可重复的元素集。因此不支持下标、分片等类序列操作的方法

##### 创建集合
```python
s0 = {'h', 'e', 'l', 'l', 'o'}
s1 = set('hello')
s2 = set(['h', 'e', 'l', 'l', 'o'])
```
都返回`set(['h', 'e', 'l', 'o'])`与期望的一样不包括重复元素。同样作为集合元素也不能包含可变元素。像这样使用没问题`set(((1, 2), (3,)))`而`set([1, 2, 3, [1, 2]])`则会抛出异常`TypeError`。虽然集合不能包含可变元素，但作为本身是可变的(注意与元组比较)。
```python
s1.add('a')         --> set(['a', 'h', 'e', 'l', 'o'])
s1.add(['a','b'])   --> TypeError不支持可变元素
```
当然也有不可变的集合`frozenset`
```python
>>> cities = frozenset(["Frankfurt", "Basel","Freiburg"])
>>> cities.add("Strasbourg")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'frozenset' object has no attribute 'add'
```
##### 常用方法
一张图来回忆下集合的常用操作

![sets_with_notations.png](C:/Users/dong/Desktop/sets_with_notations.png "")
```python
s0.remove('h')   -->s0: set(['e', 'l', 'o'])
s0.remove('hs')  -->不存在的元素抛出KeyError

s0.discard('h')  -->s0: set(['e', 'l', 'o'])
s0.discard('hs') -->不存在的元素不出错原来一致

s0.clear()       -->s0: set([])
```
交集(`difference()`)
```python
>>> x = {"a","b","c","d","e"}
>>> y = {"b","c"}
>>> z = {"c","d"}
>>> x.difference(y)
set(['a', 'e', 'd'])
>>> x.difference(y).difference(z)
set(['a', 'e'])
```
也可以更简单的处理方式
```python
>>> x - y
set(['a', 'e', 'd'])
>>> x - y - z
set(['a', 'e'])
```
补集(`difference_update`)
```python
x = {"a","b","c","d","e"}
y = {"b","c"}
x.difference_update(y)
x --> set(['a', 'e', 'd'])
z = x - y
z --> set(['a', 'e', 'd'])
```
或者
```python
x = {"a", "b", "c", }
y = {"c", "d", "e"}
print x - y
print x | y
print x & y

outputs
set(['a', 'b'])
set(['a', 'c', 'b', 'e', 'd'])
set(['c'])
```

> 字典

字典与java中的map作用类似首先来看如何创建一个字典
```python
d1 = {}
d2 = {'name':'python', 'version':'2.7.6'}
d3 = dict((['name', 'python'], ['version', 2.7]))
d4 = dict(name='python', version=2.7)
d5 = {}.fromkeys(('x', 'y'), -1)
```
对`fromkeys`接收两个参数，前面作为key后面相应的为value，如果只有一个参数那么就是这样：
`d5 = {}.fromkeys(('x', 'y')) #{'x': None, 'y': None}`

##### 遍历访问
```python
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
```
以上的遍历是通过key,value或iter的访问。当然也支持对单个key的访问，如`d2['name']或d2.get(`name`)`对于后者也可以传入默认的value，如果key不存在时返回该值，此时该字典并不改变
```python
d2.get('age', 10) >> 10
print d2          >> {'version': '2.7.6', 'name': 'python'}
```

##### 转换
常见的转换，可以将字典转换为列表(list)和迭代器(iterator)
```python
dict={'name':'python', 'version':'2.7.6'}
#list
dict.items()  >>[('version', '2.7.6'), ('name', 'python')]
dict.keys()   >>['version', 'name']
dict.values() >>['2.7.6', 'python']
#iterator
dict.iteritems()
dict.iterkeys()
dict.itervalues()
```

##### 常见方法

- **dict.copy()**  #返回字典(浅复制)的一个副本:原字典发生改变不影响新的字典
- **dict.clear()** #清空字典中所有元素：{}
- **dict1.has_key("name")** #判断是否有键，返回True或False
- **dict.pop(key[, default])** #如果字典中key存在，删除并返回`dic[key]`,如果key不存在，`default`没赋值将抛出`KeyError`

    ```python
    dict = {'name':'python', 'version':'2.7.6'}
    dict.pop("name")
    dict.pop("name","python")
    dict.pop("name")
    ```
- **dict.setdefault(key,default=None)** 和方法`set()`相似，如果字典中不存在key 键，由`dict[key]=default`为它赋值
- **dict.update(dict2)** 将字典dict2 的键-值对添加到字典dict

##### 创建进阶的字典

针对字典中key是列表，可这样操作`dict = {'name':['python','java']}`可用`setdefault()`来实现
```python
dict = {};
dict.setdefault("name",[]).append("python");
dict.setdefault("name",[]).append("java");
```
或者用集合中的`defaultdict`
```python
from collections import defaultdict
d = defaultdict(list);
d['name'].append('python')
d['name'].append('java')
```
上面使用的集合模块同样提供了有序集合`OrderedDict`
```python
d = OrderedDict()
d['name'] = 'python'
d['age'] = 20
```

##### 字典的计算

- 几种计算
    ```python	
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
    ```	
	上面用到了几个内置函数：`zip`,`min`,`max`,`sorted`:  
	`zip`:接受可迭代的参数转换为元组或列表，如果长度不一样返回最小的列
	```python	
	d = {
         'iphone5':5000,
         'g4':4000
        }
	zip(d, d.values())
	zip([1,2],[1,2,3])
    ```
- 找到相同的
	```python	
	a = {'x':1,'y':2,'z':3}
    b = {'a':4,'y':2,'z':4}
    set(a.keys()) & set(b.keys()) #{'y', 'z'}
    set(a.keys()) - set(b.keys()) #{'x'}
    set(a.items()) & set(b.items()) #{('y', 2)}
	```	
- 根据条件生产新的字典
	```python	
	a = {'x':1, 'y':2, 'z':3}
	{key:a[key] for key in set(a.keys()) - {'z'}} #{'x': 1, 'y': 2}
	```
	生成新的字典中将不包含`key：z`

参考
---

- [https://docs.python.org/2.7/](https://docs.python.org/2.7/)
- [http://blog.jobbole.com/68256/](http://blog.jobbole.com/68256/) 
- [http://blog.jobbole.com/69834/](http://blog.jobbole.com/69834/) 
- [http://www.python-course.eu/course.php](http://www.python-course.eu/course.php)  
- [http://blog.segmentfault.com/qiwsir](http://blog.segmentfault.com/qiwsir)
- [http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000)