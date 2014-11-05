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
