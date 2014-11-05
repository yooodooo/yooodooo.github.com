---
layout: post
title: Python���� �����������볣�����ݽṹ
tagline: [python] 
group: python
categories: [Python]
---
{% include codepiano/setup %}

��������������
--

> ����������

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
����������������Ҫע��������������и�������һ�£�������׳�`ValueError`�����÷�
```python
a,b,c,d,e='hello'  ##����Ϊstrings,files,iterators
_,age,_=['robin',10,('aaa',222,'ccc')] ##ֻ��עage
```
����
```python
a,b,*c=[1,2,3,4,5] #c=[3,4,5]
a,*b,c=[1,2,3,4,5] #b=[2,3,4]
```

> bool

����`True, False`��ʾ
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
> ��������

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
����������
--

> ����

- int ������������������ `type(10)--> <type 'int'>`
- long �����������޴�С������l��L��β `type(3452345234523452345234) --> <type 'long'>`
- float ������ `type(2.1)--><type 'float'>, type(32.3e18) --> <type 'float'>`
- complex ���� `type(1+2j)--><type 'complex'>`

> ��������

```python
7 + 2  >> 9
7 - 2  >> 5
7 * 2  >> 14
7 / 2  >> 3
7 % 2  >> 1   
```
ע��`/`��ȡ���������ø�����������`from __future__ import division`��python3�д���Ϊһ��
```python
7.0 / 2  >> 3.5  
7 / 2.0  >> 3.5 
7. /2    >> 3.5
```
��  
```python
>>> from __future__ import division
>>> 7/2
3.5
```
�ٽ���һ������`//`���س�������������
```python
7 // 2   >> 3
7.0 // 2 >> 3.0
```

> �Ƚϡ��߼�����������

���ñȽ�����(`==, !=, >, <, >=, <=`)  
�������� `and, or, not`
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
��Ȼ��python�бȽϲ���ֻ����ô�򵥣�����������ʽ�Ƚϣ�
```python
>>> x = 5
>>> 1 < x < 10
True
>>> x < 10 < x*10 < 100
True
```
�������ϣ������д��ʽ
> �����������������

```python
>>>123456789870987654321122343445567678890098*123345566778999009987654333238766544
15227847719352756287004435258757627686452840919334677759119167301324455281312L
```

> �����`*`

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
�ַ���
--

> ����

```python
a0 = "Hello"
a1 = 'world'
//����
a = a0 + a1
a*2

//ת���doc��ʽ
b = "what's your name?"
c = 'what\'s your name'
d = """what's your name?"""

//unicode
>>> e = u'���'
>>> e
u'\xc4\xe3\xba\xc3'
```

> �ַ�������

```python
a = "my name is %s, and %d years old"%('abc',1)
b = "my name is %s"%'abc'
```

> ���÷���

```python
a = 'Hello World'
len(a)
//��Сдת��    
a.upper()
a.lower()
a.capitalize()
//�±�������   
a.index('e')
a.count('l')
a.strip();
a.rstrip()
```
> ���顢��Ƭ������

����
```python
name = 'Hello'
name[0]  --> H
name[-2] --> l
name[6]  -->IndexError
```
��Ƭ
```python
name[0:3] --> 'Hel'
name[3:]  --> 'lo'
name[:]   --> 'Hello'
name[0:3:2] --> 'Hl' ����Ϊ2
name[::-1]  --> 'olleH'
```
����
```python
list(name) --> ['H', 'e', 'l', 'l', 'o']������"".join([])
>>> 'a' in name
False
>>> 'a' not in name
True
```
---
����
--
> ��������

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
����˵��
- �ùؼ���`def`��������ʹ��`{}`���������棬ע������ð��(`:`)
- ע���������������
- `pass`��ʾ��ʵ��

�Ƿ���Է��ض������?���ǿ϶���
```python
def return_multi_param(x):
    return x + 1, x + 2
    
a,b = return_multi_param(1)
```

> �ɱ����

```python
def foo(name, *args, **kwds):  
    print "name= ", name  
    print "args= ", args  
    print "kwds= ",kwds  
```
�ɼ����`*args`Ϊ�ɱ��б�, `**kwds`Ϊ�ɱ��ֵ�
```python
foo("python", "10", "chengdu", a="aaaaaaaaa", b="vvvvvvvvvvvvv")
## outputs
name=  python  
args=  ('10', 'chengdu')  
kwds=  {'a': 'aaaaaaaaa', 'b': 'vvvvvvvvvvvvv'}   
```

> Ĭ�ϲ���

```python
def foo(name = 'abc'):  
    print "name= ", name 
## outputs
foo()         #name= abc
foo("lala")   #name= lala
```
**ע��**���Ĭ�ϲ����ǿɱ��������ô���ܵ��º���Ԥ�ڲ�һ���Ľ��
```
def foo(bar=[]):
    bar.append('bar')
    return bar
foo() >>['bar']
foo() >>['bar', 'bar']
foo() >>['bar', 'bar', 'bar']
```
����������κ���������Ԥ��������һ������������Ĭ��ֵ�������ڸú��������ʱ�򣬱���ֵһ�Ρ���ˣ�ֻ�е�����foo()��һ�α������ʱ�򣬲Ž�����bar��Ĭ��ֵ��ʼ��������Ĭ��ֵ����һ���յ��б���������foo()��ʱ�򣨲�������bar���������ʹ��bar�����ʼ��ʱ���Ǹ��б�
��ȻҲ�з�ʽ���������ַ�ʽ
```
def foo(bar=[]):
    if not bar:
        bar = []
    bar.append('bar')
    return bar
```
������Ϣ��http://blog.jobbole.com/40088/

> ������Ϊ��������

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
��Ҳ�������﷨��`@`������
```python
@p_decorate 
def get_text(name):
    return "lorem ipsum, {0} dolor sit amet".format(name)
```
�����ͺ���ͨ�ĵ���һ������`get_text("John")`�������Python�е���������������Ϣ�ɲο�
- http://thecodeship.com/patterns/guide-to-python-function-decorators/
- http://simeonfranklin.com/blog/2012/jul/1/python-decorators-in-12-steps

---
�������ú���
--
> lambda����

lambda��������������,û�к�����ͨ��ֱ�ӵ��á��������������ͨ����:
```python
def squ(x):
    return x*x 
```
���ùؼ���`lambda`��ʵ��
```python
g = lambda x:x*x
```

> filter(function, sequence)

��sequence�е�item����ִ��function(item)����ִ�н��ΪTrue��item���һ��List/String/Tuple��ȡ����sequence�����ͣ�����

���������д���5С��10����
```python
def largerThan(x):
    return x > 5 and x < 10
    
if __name__ == '__main__':
    x = filter(largerThan, [1,6,4,8,11])
    print x
```
ͨ����lambda���ʹ��
```python
filter(lambda i:i > 5 and i < 10, [1, 6, 4, 8, 11])
```
���صĽ����sequence�������й�
```python
filter(lambda i:i > 5 and i < 10, [1, 6, 4, 8, 11])
filter(lambda i:i > 5 and i < 10, (1, 6, 4, 8, 11,))
filter(lambda x: x != 'a' and x != 'c', 'abcdefadc')
```

> map(function, sequence)

��sequence�е�item����ִ��function(item)����ִ�н�����һ��List����.��filter������,�Ƚ������ߵ��÷�
```python
filter(lambda i:i > 5 and i < 10, [1, 6, 4, 8, 11])-->[6, 8]
map(lambda i:i > 5 and i < 10, [1, 6, 4, 8, 11])-->[False, True, False, True, False]
```
��ȻҲ֧�ֶ��ֲ���������
```python
map(lambda i:i * i, [1, 6, 4, 8, 11])
map(lambda i: i + i, 'abcd')
```
���Կ���map���ص����к�Զ���еĳ���һ��,ֻ�����ͻ���������仯.ͬ��֧�ֶ������
```python
map(lambda x, y: x + y, range(8), range(8))
```
�������Ϊ`None`ʱ,��������
```python
map(None, range(3), range(3)) -->[(0, 0), (1, 1), (2, 2)]
map(None, range(2), range(3)) -->[(0, 0), (1, 1), (None, 2)]
```

> reduce(function, sequence, starting_value)

��sequence�е�item˳���������function�������starting_value����������Ϊ��ʼֵ����.��Ҫע���������÷��������Ƕ��б��е�Ԫ�����ε��ö�Ӧ��function
```python
reduce(lambda x, y:x + y, range(4)) --> 0+1+2+3
reduce(lambda x, y:x + y, ['a','b','c','d'])
reduce(lambda x, y:x + y, range(4), 10) --> 0+1+2+3+10
```

---
���ݽṹ
--

> ����

������Python���������Ԫ�أ��б��е�ÿ��Ԫ���±��0��ʼ
```python
a = [1, 'abc', 2, 3]
b = [1, 2, [1, 2]]
```
##### �������Ƭ
```python
a[0]       --> 1
a[4]       --> IndexError
a[1:3]     --> ['abc', 2]
a[::-1]    --> [3, 2, 'abc', 1]
a[0:20]    --> [1, 'abc', 2, 3]
```
##### ׷��Ԫ��
������Ҫ����3�ַ�ʽ
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
ע�����`a[len(a):] = [3]`����׷�ӷ�ʽ��Ҫע��   
- ����ʹ��`a[len(a)+1]`��ָ���±꣬�������׳�`IndexError`
- �������ֵ�����ǿɵ�����,��`a[len(a):] = 3`�ͻ��׳�`TypeError`����ȻҲ֧�ֶ��Ԫ�ص�
`a[len(a):] = [3, 4, 5]`

����`extend`����Ҳ�ǽ��ܿɵ������������б�`[1,2,3]`�����ַ���`abc`��������`insert(i, x)`���Ƕ�ָ�����±����Ԫ�أ�ע���±���Դ����б��ʵ�ʳ��ȣ�����Ҳֻ�����б����׷�ӣ���Ӧ�ĳ�������1��


##### ���÷���
```python
a = [1, 2, 1, 'a', 'b']
len(a)  -->5 //����
a.count(1) --> 2 //Ԫ�ظ���
a.count(3) --> 0 //Ԫ�ز����ڲ�������0

a.index(1) --> 0 //Ԫ�����б��е��±꣬������ص�һ��
a.index(3) -->   //Ԫ�ز��������쳣ValueError 

a.remove(1) --> ���Ԫ��ֻɾ����һ��
a.remove(3) --> �����ڵ�Ԫ���׳��쳣ValueError 

a.pop(1)    --> ɾ��ָ���±��Ԫ�أ����ظ�Ԫ������Ϊ2
a.pop(10)   --> �±�Խ���׳�IndexError
```

##### �б�ѭ��
�����б��ѭ��������ͨ���������ַ�ʽ
```python
a = [1, 'a', 2, 'b']
for i in a:
    print i
```
����
```python
for i in range(len(a)):
    print a[i]
```
������б�ֻ��һЩ�򵥵����㣬���Կ���ʹ��**�б��Ƶ�**
```python
[x * x for x in range(3)]-->[0, 1, 4]
```
Ҳ������`if`���й���
```python
[x * x for x in range(3) if x % 2 == 0]-->[0, 4]
```
�Զ��`for`������
```python
[(x, y) for x in range(3) for y in range(3)]
```
��ʱ����ϣ�����б���Ϊһ��ö�٣��ɲ����б��Ƶ��ͺ���`enumerate([])`
```python
a = ['a', 'b', 'c', 'd', 'e']
b = [(index, item) for index, item in enumerate(a)]
outputs
[(0, 'a'), (1, 'b'), (2, 'c'), (3, 'd'), (4, 'e')]
```
�����б�Ҫ�����º���`range(start,end,step)`

##### range(start,end,step)
����ָ����Χ���б�������ֹ��Χ�Ͳ���
```python
range(5)     --> [0, 1, 2, 3, 4]
range(0,5)   --> [0, 1, 2, 3, 4]
range(0,5,2) --> [0, 2, 4]
```
##### ��str������
ͨ������Ľ������ǻᷢ���б���ַ�������һ�������Ƶط������������±�����ȡ��磺
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
���ϲ�����������ֻ�ǽ�����ز�һ������ȻҲ�в�ͬ�ĵط����б����ǿɱ䳤�ģ���˶��б�Ĳ�������`append, insert`�ȾͲ��ʺ��ַ�������Ȼ����Ҳ���Ժܷ���ת��
```python
list(a) --> ['a', 'b', 'c']
''.join(b) --> abc
```

> Ԫ�� 

���ɱ����м�ΪԪ��
```python
(1,2,3)
('a','b','c')
(1,)
```
���ֻ��һ��Ԫ����Ҫ��ĩβ�Ķ���`(,)`ע��Ƚ�
```python
3 * (10 + 2)
3 * (10 + 2,)
```
����ͨ������`tuple`��ʵ���б��Ԫ���ת��
```python
tuple('1bs3')
tuple([1, 2, 3, 4])
tuple((1, 2, 3,))
tuple([1, 2, 3, 4, [1, 2]])
```
ע���������Ԫ���ǿ��԰����ɱ�Ԫ�صġ������пɱ�Ԫ��`[1,2]`��������ͨ���б�ûʲô���
```python
t = tuple([1, 2, 3, 4, [1, 2]])
t[4].insert(0, 0)
print t --> (1, 2, 3, 4, [0, 1, 2])
```
Ԫ���ѭ������ʹ��������б���ܵķ�������������Ԫ�鲻�ɱ���ô�ı�Ԫ��ķ����ǲ����õġ����׳��쳣`AttributeError`��

> ����(set)

����������һ������һ�����򲻿��ظ���Ԫ�ؼ�����˲�֧���±ꡢ��Ƭ�������в����ķ���

##### ��������
```python
s0 = {'h', 'e', 'l', 'l', 'o'}
s1 = set('hello')
s2 = set(['h', 'e', 'l', 'l', 'o'])
```
������`set(['h', 'e', 'l', 'o'])`��������һ���������ظ�Ԫ�ء�ͬ����Ϊ����Ԫ��Ҳ���ܰ����ɱ�Ԫ�ء�������ʹ��û����`set(((1, 2), (3,)))`��`set([1, 2, 3, [1, 2]])`����׳��쳣`TypeError`����Ȼ���ϲ��ܰ����ɱ�Ԫ�أ�����Ϊ�����ǿɱ��(ע����Ԫ��Ƚ�)��
```python
s1.add('a')         --> set(['a', 'h', 'e', 'l', 'o'])
s1.add(['a','b'])   --> TypeError��֧�ֿɱ�Ԫ��
```
��ȻҲ�в��ɱ�ļ���`frozenset`
```python
>>> cities = frozenset(["Frankfurt", "Basel","Freiburg"])
>>> cities.add("Strasbourg")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'frozenset' object has no attribute 'add'
```
##### ���÷���
һ��ͼ�������¼��ϵĳ��ò���

![sets_with_notations.png](C:/Users/dong/Desktop/sets_with_notations.png "")
```python
s0.remove('h')   -->s0: set(['e', 'l', 'o'])
s0.remove('hs')  -->�����ڵ�Ԫ���׳�KeyError

s0.discard('h')  -->s0: set(['e', 'l', 'o'])
s0.discard('hs') -->�����ڵ�Ԫ�ز�����ԭ��һ��

s0.clear()       -->s0: set([])
```
����(`difference()`)
```python
>>> x = {"a","b","c","d","e"}
>>> y = {"b","c"}
>>> z = {"c","d"}
>>> x.difference(y)
set(['a', 'e', 'd'])
>>> x.difference(y).difference(z)
set(['a', 'e'])
```
Ҳ���Ը��򵥵Ĵ���ʽ
```python
>>> x - y
set(['a', 'e', 'd'])
>>> x - y - z
set(['a', 'e'])
```
����(`difference_update`)
```python
x = {"a","b","c","d","e"}
y = {"b","c"}
x.difference_update(y)
x --> set(['a', 'e', 'd'])
z = x - y
z --> set(['a', 'e', 'd'])
```
����
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

> �ֵ�

�ֵ���java�е�map������������������δ���һ���ֵ�
```python
d1 = {}
d2 = {'name':'python', 'version':'2.7.6'}
d3 = dict((['name', 'python'], ['version', 2.7]))
d4 = dict(name='python', version=2.7)
d5 = {}.fromkeys(('x', 'y'), -1)
```
��`fromkeys`��������������ǰ����Ϊkey������Ӧ��Ϊvalue�����ֻ��һ��������ô����������
`d5 = {}.fromkeys(('x', 'y')) #{'x': None, 'y': None}`

##### ��������
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
���ϵı�����ͨ��key,value��iter�ķ��ʡ���ȻҲ֧�ֶԵ���key�ķ��ʣ���`d2['name']��d2.get(`name`)`���ں���Ҳ���Դ���Ĭ�ϵ�value�����key������ʱ���ظ�ֵ����ʱ���ֵ䲢���ı�
```python
d2.get('age', 10) >> 10
print d2          >> {'version': '2.7.6', 'name': 'python'}
```

##### ת��
������ת�������Խ��ֵ�ת��Ϊ�б�(list)�͵�����(iterator)
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

##### ��������

- **dict.copy()**  #�����ֵ�(ǳ����)��һ������:ԭ�ֵ䷢���ı䲻Ӱ���µ��ֵ�
- **dict.clear()** #����ֵ�������Ԫ�أ�{}
- **dict1.has_key("name")** #�ж��Ƿ��м�������True��False
- **dict.pop(key[, default])** #����ֵ���key���ڣ�ɾ��������`dic[key]`,���key�����ڣ�`default`û��ֵ���׳�`KeyError`

    ```python
    dict = {'name':'python', 'version':'2.7.6'}
    dict.pop("name")
    dict.pop("name","python")
    dict.pop("name")
    ```
- **dict.setdefault(key,default=None)** �ͷ���`set()`���ƣ�����ֵ��в�����key ������`dict[key]=default`Ϊ����ֵ
- **dict.update(dict2)** ���ֵ�dict2 �ļ�-ֵ����ӵ��ֵ�dict

##### �������׵��ֵ�

����ֵ���key���б�����������`dict = {'name':['python','java']}`����`setdefault()`��ʵ��
```python
dict = {};
dict.setdefault("name",[]).append("python");
dict.setdefault("name",[]).append("java");
```
�����ü����е�`defaultdict`
```python
from collections import defaultdict
d = defaultdict(list);
d['name'].append('python')
d['name'].append('java')
```
����ʹ�õļ���ģ��ͬ���ṩ�����򼯺�`OrderedDict`
```python
d = OrderedDict()
d['name'] = 'python'
d['age'] = 20
```

##### �ֵ�ļ���

- ���ּ���
    ```python	
    d = {
    	 'iphone5':5000,
    	 'g4':4000,
    	 'mi3':1999,
    	 'mi2s':1699
    	 }
    min(d)  //key������ĸ˳�����е���С
    min(d.values())  # values��С
    			
    min(zip(d.values(), d.keys()))  # ��С�����У�����values����
    max(zip(d.values(), d.keys()))
    sorted(zip(d.values(), d.keys()))  # ����values����
    min(d, key=lambda k:d[k])#��ȡvalues��С��key
    ```	
	�����õ��˼������ú�����`zip`,`min`,`max`,`sorted`:  
	`zip`:���ܿɵ����Ĳ���ת��ΪԪ����б�������Ȳ�һ��������С����
	```python	
	d = {
         'iphone5':5000,
         'g4':4000
        }
	zip(d, d.values())
	zip([1,2],[1,2,3])
    ```
- �ҵ���ͬ��
	```python	
	a = {'x':1,'y':2,'z':3}
    b = {'a':4,'y':2,'z':4}
    set(a.keys()) & set(b.keys()) #{'y', 'z'}
    set(a.keys()) - set(b.keys()) #{'x'}
    set(a.items()) & set(b.items()) #{('y', 2)}
	```	
- �������������µ��ֵ�
	```python	
	a = {'x':1, 'y':2, 'z':3}
	{key:a[key] for key in set(a.keys()) - {'z'}} #{'x': 1, 'y': 2}
	```
	�����µ��ֵ��н�������`key��z`

�ο�
---

- [https://docs.python.org/2.7/](https://docs.python.org/2.7/)
- [http://blog.jobbole.com/68256/](http://blog.jobbole.com/68256/) 
- [http://blog.jobbole.com/69834/](http://blog.jobbole.com/69834/) 
- [http://www.python-course.eu/course.php](http://www.python-course.eu/course.php)  
- [http://blog.segmentfault.com/qiwsir](http://blog.segmentfault.com/qiwsir)
- [http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000)