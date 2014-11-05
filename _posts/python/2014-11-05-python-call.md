---
layout: post
title: Python中魔术方法call的使用
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

当然函数`__call()__`中可以传入参数，也可以调用其他函数

    def __call__(self, msg):
        self.say_hello(msg)
        
    def say_hello(self, msg):
        print "User[name=%s, age=%s] ...%s" % (self._name, self._age, msg)
        
那么这个时候调用只需要传入参数如`u('Hello')`即可：

    User[name=robin, age=aa] ...Hello
    
以上都是一些通常的使用方法，当然我们并不满足这个层面的使用。再见下面的例子，结合高价函数的使用实现了一个简单的AOP功能

    from decorator import decorator
    class UserMethodDecrator(object):
        
        def __init__(self, name, age):
            self._name = name
            self._age = age
            
        def __call__(self, method):
            self.method = method
            return decorator(self.lookup, method)
    
        def lookup(self, method, *args, **kwargs):
            print "something happen before"
            print args
            print kwargs
            value = self.method(*args, **kwargs)
            print "something happen after"
            return value
    
    @UserMethodDecrator('robin','aaa')        
    def do_something(name):
        print "%s is doing something..."%(name,)
        return "HELLO"
    
    if __name__ == '__main__':
        print do_something('LUCK')
   
在执行函数`do_something`的时候，使用了高阶函数`UserMethodDecrator`(这里是对象当作函数使用，当然函数也一样没问题，只是如果需要植入的业务太复杂可能需要好好封装)。在该函数执行前后进行了一些操作，并将结果返回。整个执行的结果如下

    something happen before
    ('LUCK',)
    {}
    LUCK is doing something...
    something happen after
    HELLO

当然这里涉及了`decorator`的用法，见[这里](http://thecodeship.com/patterns/guide-to-python-function-decorators/)

- [更多MagicMethod的使用](http://www.rafekettler.com/magicmethods.html)

