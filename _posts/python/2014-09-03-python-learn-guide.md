---
layout: post
title: Python���ű��������볣�����ݽṹ
tagline: [python] 
group: python
categories: [Python]
---
{% include codepiano/setup %}

��������������

> ����������

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

����������������Ҫע��������������и�������һ�£�������׳�`ValueError`�����÷�

    a,b,c,d,e='hello'  ##����Ϊstrings,files,iterators
    _,age,_=['robin',10,('aaa',222,'ccc')] ##ֻ��עage

����

    a,b,*c=[1,2,3,4,5] #c=[3,4,5]
    a,*b,c=[1,2,3,4,5] #b=[2,3,4]

