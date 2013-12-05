---
layout: post
title: Python 内置函数
tagline: [python] 
group: Python
---
{% include JB/setup %}

python提供了大量内置的函数，对这样常用的函数我进行了简单的分类，主要包括以下几类：

## 集合类 ##
<table>
<tbody>
<tr><td>dict(**kwarg)</td><td>创建一个字典</td></tr>
<tr><td>list([iterable])</td><td>一个可变序列</td></tr>
<tr><td>set([iterable])</td><td>创建一个set对象，可通过迭代器对元素进行访问</td></tr>
<tr><td>tuple([iterable])</td><td>一个不可变的序列</td></tr>
<tr><td>range(start, stop[, step])</td><td>从list，tuple等返回一个不可变的序列</td></tr>
</tbody>
</table>


## 数字、计算、进制 ##
<table>
<tbody>
<tr><td>abs(x)</td><td>返回数字的绝对值，数字可以是int,float如果是复数返回模</td></tr>
<tr><td>bin()</td><td>返回一个integer的二进制字符串</td></tr>
<tr><td>bool([x])</td><td>将一个值转换为Boolean类型</td></tr>
<tr><td>complex([real[, imag]])</td><td>创建一个复数: 1+2j</td></tr>
<tr><td>float([x])</td><td>将一个字符串或数字转换为浮点数</td></tr>
<tr><td>int([x=0[,base=10]])</td><td>将字符串或数字转换为整型</td></tr>
<tr><td>len(s)</td><td>返回对象的长度，可以是序列(string,tuple,list)或字典</td></tr>
<tr><td>min(iterable, *[, key])</td><td>返回迭代器中最小的</td></tr>
<tr><td>max()</td><td>创建一个复数</td></tr>
</tbody>
</table>

- `bool([x])` 在转换时将`None,False,0,0.0,0j,[],{},'',()`都为`False`
- `float([x])`接收字符串，数字，符号(`+,-`)以及(`Infinity,inf,nan`)

		float() #0.0
		float('+1.23') #1.23
		float('1') #1.0
		float('   -12345\n') #-12345.0
		float('+1E6') #1000000.0
		float('-Infinity')#inf
		float('aa')#raise ValueError




## 对象属性 ##
<table>
<tbody>
<tr><td>classmethod(function)</td><td>创建</td></tr>
<tr><td>delattr()</td><td>一个</td></tr>
<tr><td>getattr()</td><td>创建</td></tr>
<tr><td>hasattr()</td><td>一个</td></tr>
<tr><td>isinstance()</td><td>序列</td></tr>
<tr><td>issubclass()</td><td>序列</td></tr>
<tr><td>object()</td><td>序列</td></tr>
<tr><td>setattr()</td><td>序列</td></tr>
<tr><td>staticmethod()</td><td>序列</td></tr>
</tbody>
</table>


## 文件目录 ##
<table>
<tbody>
<tr><td>dict(**kwarg)</td><td>创建一个字典</td></tr>
<tr><td>list([iterable])</td><td>一个可变序列</td></tr>
<tr><td>set([iterable])</td><td>创建一个set对象，可通过迭代器对元素进行访问</td></tr>
<tr><td>tuple([iterable])</td><td>一个不可变的序列</td></tr>
<tr><td>range(start, stop[, step])</td><td>从list，tuple等返回一个不可变的序列</td></tr>
</tbody>
</table>