---
layout: post
title: Python list
tagline: [python] 
group: Python
categories: [Python]
---
{% include codepiano/setup %}

## 序列 ##
序列是python中基本的数据结构，序列中的每个元素被分配了一个序号即元素的位置，也即索引，下表从0开始。如下面是一个序列

    user1 = ['robin',30]

这其中元素的类型并不像java那样严格的规定，甚至还可以保护多个序列

	user2 = ['summer',27]
	user = [user1, user2]#[['robin', 30], ['summer', 27]]

这实际上也是我们常说的二维数组了

## 序列通用操作 ##
下面介绍序列的通用操作，主要包括索引(indexing)，分片(sliceing)，加(adding)，乘(multiplying)

1、索引  
这就是元素都有下标，从`0`开始，反转的时候从`-1`开始

	name = 'hello'
	name[0]
	name[-1]
	name[6]

需要注意，如果下标超过序列长度，那么将抛出错误，如：

	Traceback (most recent call last):
	  File "F:\git\pyl\pyl\src\com\python\learn\chap03\seq.py", line 15, in <module>
	    name[6]
	IndexError: string index out of range

2、分片  
分片就是截取序列的部分元素，主要通过冒号相隔两个索引来实现

	numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
	numbers[1:3] //-->[2,3]
	numbers[-3:-1] //-->[8, 9]

与索引一样，下标从`0`开始，反转从`-1`开始，我们来看看上面最后一个，明明是`-1`为什么不是`[8, 9,10]`，可能会想到这样

    numbers[-3:0]

但事实上这样输出为`[]`因为分片最左边的索引比他右边的晚出现在序列中，那么结果就是一个空的序列。但如果分片需要包括序列结尾的元素，只需要将最后一个索引置空即可

    numbers[-3:]

这是一个很酷的功能，比如
  
    numbers[:3]//--->[1,2,3]
    numbers[:]//--->[1,2,3,4,5,6,7,8,9,10]

其实上面的例子都包含了一个隐含的条件，那就是步长为1，当然你也可以设定需要的步长(但步长不能为0，否则将抛出`ValueError`)。可以参考如下几个例子的输出

    numbers[0:10]
	numbers[0:10:1]
	numbers[::1]
	numbers[0:10:2]

这里有一个很酷的应用

    name = 'abcdefghi'
    name[::-1]

看结果是什么？是的，这就是一个字符串反转的用法！

3、运算

- add
	
	同一类型的序列可进行相加

		[1, 2, 3] + [4, 5, 6]
		[1, 2, 3] + 'hello'//TypeError

- multiply

	乘法=重复次数

    在列表的运算中，乘法将使元素重复相应的次数

		[1, 2] * 5 //--->[1, 2, 1, 2, 1, 2, 1, 2, 1, 2]
        python' * 2 //--->pythonpython
        [None] * 10 //长度为10的空列表

- in
   
    `in`是用来检测是否包含指定元素的
  
		'h' in 'hello' //--->True
		[1, 2] in [[1, 2], [2, 1]] //--->True

- max

	`len`,`max`,`min`分别是序列的长度、序列中元素的最大值、最小值

		len([1, 2, 3, 4])
		max([1, 2, 3, 4])
		max(12,23,45)
		min(12,-23,45)

	如果序列不是数字呢？

		min('b','a','c')
		min(u'爱','a','c')

	取元素的ASCII码

## 列表(list) ##
列表是序列中很常见的结构，他是可变的，有许多有用的方法

- list

	他将字符串转换为列表

		list('123')
		list('abc')
	
	当然如果再转回来可以`''.join(['a','b',c])`

- modify

		x = [1, 2, 3]
	    x[1] = 1

- delete

		x = [1, 2, 3]
		del x[1]

- split

		a = [1, 2, 3, 4]
		a[2:]=list('hello') //[1, 2, 'h', 'e', 'l', 'l', 'o']

	或者一个更酷的

	    numbers = [1, 5]
		numbers[1:1] = [2, 3, 4]//[1,2,3,4,5]

	当然也可以用来作为删除元素

		numbers[1:4] = [] //[1, 5]

- append

	列表末尾追加新的对象，注意一下两个操作的结果

		numbers.append('a')
		numbers.append(['a'])

- count

	统计某个元素在列表中出现的个数

		['1','a','2','1'].count('1')
		['1','a','2','1'].count('1c')

- extend

	末尾最近新的序列

		a = [1, 2, 3]
		b = [4, 5, 6]
		a.extend(b)

	此时`a=[1,2,3,4,5,6]`而`b`未变。注意区别`+`操作

		a = [1, 2, 3]
		b = [4, 5, 6]
		c = a+b

	此时`c=[1,2,3,4,5,6]`而`a,b`未变

- index

	指定元素在列表中索引的位置

		[1, 2, 3, 4].index(1)

- insert

	在指定的位置插入元素，注意并不替换该位置原来值

		c = [1, 2, 3, 4]
		c.insert(2, 'a')

	如果指定位置超过实际长度，那么只是在最后追加

		c.insert(5, 'a')//[1,2,3,4,'a']

- pop

	移除列表中的元素(默认最后一个)，通过index来指定

		[1,2,3].pop();
		[1,2,3].pop(1);
		[1,2,3].pop(3);//IndexError

- remove

	移除列表中某个元素第一个匹配的项，如果不存在将会抛出`ValueError`

		[1,2,1,2].remove(1)
		[1,2].remove(3)

- reverse

	反转列表
	
		x=[1, 2, 3, 4]
		x.reverse();

	可以通过这个实现字符串的反转

		a = list('abcbd sad')
		a.reverse()
		''.join(a)

- sort

	该方法在原列表进行排序(不会返回副本)

		x=[1,2,4,6,3]
		x.sort()//--->x=[1,2,3,4,6]

	而如果采取这样
		
		x=[1,2,4,6,3]
		y=x.sort()//--->x=[1,2,3,4,6],y=None

	如果需要副本，可以用`sorted`

		x = [1, 2, 4, 6, 3]
		y = sorted(x)

	此时x保留原样，而y已经是排序后的输出。`sort()`有另外两个可选的参数:key排序函数，reverse指定是否倒序

		b=["aacs","aaa","tgdfs","f"]
		b.sort(key=len)

		c=[1,6,3,4,2]
		c.sort(reverse=True)

## 元组 ##
	
是不可变的序列，即不能修改。通常用`()`来表示，如

	(1,2,3)
	('a','b','c')

如果只包含一个元素，则需要在后面加逗号(，)

	(42,)

比较

	3 * (10 + 2)
	3 * (10 + 2,)

可以通过函数`tuple`来实现对元组的转换

	tuple('1bs3')
	tuple([1, 2, 3, 4])
	tuple((1, 2, 3,))
