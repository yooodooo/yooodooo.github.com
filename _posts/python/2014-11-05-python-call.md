---
layout: post
title: python中魔术方法call的使用
tagline: [python] 
group: python
categories: [Python]
---
{% include codepiano/setup %}

最近工作中使用到了Python的MagicMethod `__call()__`将他的一些使用方法记录下来。简单地讲，该函数可以将对象的实例作为一个函数来调用,并且可以传入参数等操作。下面见一个例子

    class User(object):
        
        def __init__(self, name, age):
            self._name = name
            self._age = age
            
        def __call__(self):
            print "name: %s, age: %s" % (self._name, self._age)
    
    
    def test_user_call():
        u = User('robin', 'aa')
        u()

在这个对象中实现了函数`__call()__`，该函数打印了私有变量。在函数`test_user_call`中对该对象的实例进行了调用:`u()`那么将打印如下结果

    name: robin, age: aa



http://www.rafekettler.com/magicmethods.html

