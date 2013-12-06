---
layout: post
title: python requests
tagline: [python] 
group: python
---
{% include JB/setup %}

## 说明 ##

> Requests allow you to send HTTP/1.1 requests. You can add headers, form data, multipart files, and parameters with simple Python dictionaries, and access the response data in the same way. It's powered by httplib and urllib3, but it does all the hard work and crazy hacks for you.

## 安装 ##

有[多种](http://www.python-requests.org/en/latest/user/install/)方式安装,这里采用`setuptools`

	git clone git://github.com/kennethreitz/requests.git
	python setup.py install
	
## 快速入门 ##

用Requests来创建一个HTTP请求很简单

	import requests
	r = requests.get('https://www.google.com/search?q=python')

这里创建了一个get请求，同时返回了一个`Response`对象.当然也可以传递参数的方式发送请求

	queryparams = {'q':'python'}
	r = requests.get('https://www.google.com/search', params=queryparams)

当然对于其他的HTTP请求类型也同样支持：

	r = requests.post('https://www.somesite.com/post')
	r = requests.put('https://www.somesite.com/put')
	r = requests.delete('https://www.somesite.com/delete')
	r = requests.head('https://www.somesite.com/head')
	r = requests.options('https://www.somesite.com/options')

## Response ##

`Response`表示了一个HTTP响应对象，可以通过text的方式返回`r.text`，也可以是二进制的内容`r.content`。同样也可以返回原生的内容`r.raw`.此时是`requests.packages.urllib3.response.HTTPResponse`对象。

	r = requests.get('https://www.google.com/search?q=python',stream=True)
	r.raw
	>>> <requests.packages.urllib3.response.HTTPResponse object at 0x02367470>
	
	with open('sss.html','wb') as fd:
		for chunk in r.iter_content(1000):
			fd.write(chunk)

上面的例子将请求的内容原生返回并写入了指定的文件。对应返回的内容，也可以指定编码方式`r.encoding`.下表展示了`Response`中的属性和方法

<table>

<tr>
	<td>status_code</td>
	<td>返回的HTTP状态码</td>
</tr>
<tr>
	<td>headers</td>
	<td>以字典的形式返回响应的Headers,如`r.headers['content-type']`</td>
</tr>
<tr>
	<td>raw</td>
	<td>类文件的形式返回请求对象，需要设置请求参数`stream=True`</td>
</tr>
<tr>
	<td>url</td>
	<td>请求后的最终url</td>
</tr>
<tr>
	<td>encoding</td>
	<td>设置解析`r.text`的编码</td>
</tr>
</table>