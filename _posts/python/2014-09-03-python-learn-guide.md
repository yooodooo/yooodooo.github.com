---
layout: post
title: Python入门变量函数与常见数据结构
tagline: [python] 
group: python
categories: [Python]
---
{% include codepiano/setup %}

变量与数据类型

> 变量的声明

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

