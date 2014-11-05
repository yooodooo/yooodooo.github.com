---
layout: post
title: Python入门指南(一)
tagline: [python] 
group: python
categories: [Python]
---
{% include codepiano/setup %}

## 变量与数据类型

1、 变量的声明

作为弱类型语言，并不需要在变量声明时指定具体的类型。下面是一个声明的例子

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

对于批量的命名需要注意变量个数和序列个数保存一致，否则会抛出`ValueError`更多用法

    a,b,c,d,e='hello'  ##必须为strings,files,iterators
    _,age,_=['robin',10,('aaa',222,'ccc')] ##只关注age

或者

    a,b,*c=[1,2,3,4,5] #c=[3,4,5]
    a,*b,c=[1,2,3,4,5] #b=[2,3,4]

2、 bool

采用`True, False`表示

    >>> True
    True
    >>> False
    False
    >>> 3 > 2
    True
    >>> 3 > 5
    False

3、 布尔运算

    True and True ##True
    True and False ## False
    False and False ## False
    
    True or True ##True
    True or False ##True
    False or False ##False
    
    not True ## False
    not False ## True

## 数字与运算


1、 类型

- int 整数，正整数或负整数 `type(10)--> <type 'int'>`
- long 长整数，无限大小。可以l或L结尾 `type(3452345234523452345234) --> <type 'long'>`
- float 浮点数 `type(2.1)--><type 'float'>, type(32.3e18) --> <type 'float'>`
- complex 复数 `type(1+2j)--><type 'complex'>`

2、 算术运算

    7 + 2  >> 9
    7 - 2  >> 5
    7 * 2  >> 14
    7 / 2  >> 3
    7 % 2  >> 1   

注意`/`是取整，可以用浮点数除法或`from __future__ import division`在python3中处理为一样

    7.0 / 2  >> 3.5  
    7 / 2.0  >> 3.5 
    7. /2    >> 3.5

或  

    >>> from __future__ import division
    >>> 7/2
    3.5

再介绍一种运算`//`返回除法的整数部分

    7 // 2   >> 3
    7.0 // 2 >> 3.0


3、 比较、逻辑、布尔运算

常用比较运算(`==, !=, >, <, >=, <=`)  
布尔运算 `and, or, not`

    >>> 4>3 and 4>5
    False
    >>> 4>3 or 4>5
    True
    >>> not 4>3
    False
    >>> not 4>5
    True

当然在python中比较并不只是这么简单，下面来个链式比较：

    >>> x = 5
    >>> 1 < x < 10
    True
    >>> x < 10 < x*10 < 100
    True

这才是你希望的书写方式

4、 不存在整数溢出问题

    >>>123456789870987654321122343445567678890098*123345566778999009987654333238766544
    15227847719352756287004435258757627686452840919334677759119167301324455281312L
    

5、 神奇的`*`

    >>> a = 'abc'
    >>> a*4
    'abcabcabcabc'
    >>> b = (1,2)
    >>> b*2
    (1, 2, 1, 2)
    >>> c = [1,2]
    >>> c*3
    [1, 2, 1, 2, 1, 2]


## 字符串

1、 申明

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


2、 字符串连接

    a = "my name is %s, and %d years old"%('abc',1)
    b = "my name is %s"%'abc'

3、 常用方法

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

4、 分组、切片与其他

索引

    name = 'Hello'
    name[0]  --> H
    name[-2] --> l
    name[6]  -->IndexError

切片

    name[0:3] --> 'Hel'
    name[3:]  --> 'lo'
    name[:]   --> 'Hello'
    name[0:3:2] --> 'Hl' 步长为2
    name[::-1]  --> 'olleH'

其他

    list(name) --> ['H', 'e', 'l', 'l', 'o']返回用"".join([])
    >>> 'a' in name
    False
    >>> 'a' not in name
    True

## 函数

1、 函数定义

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

2、 可变参数

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

3、 默认参数

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

4、 函数作为参数传递

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


## 几个内置函数

1、 lambda函数

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

2、 map(function, sequence)

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

3、 reduce(function, sequence, starting_value)

对sequence中的item顺序迭代调用function，如果有starting_value，还可以作为初始值调用.需要注意与上诉用法的区别，是对列表中的元素依次调用对应的function
```python
reduce(lambda x, y:x + y, range(4)) --> 0+1+2+3
reduce(lambda x, y:x + y, ['a','b','c','d'])
reduce(lambda x, y:x + y, range(4), 10) --> 0+1+2+3+10
```
