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

	>>> import requests
	>>> r = requests.get('https://www.google.com/search?q=python')

这里创建了一个get请求，同时返回了一个`Response`对象.当然对于其他的HTTP请求类型也同样支持：

	r = requests.post('https://www.somesite.com/post')
	r = requests.put('https://www.somesite.com/put')
	r = requests.delete('https://www.somesite.com/delete')
	r = requests.head('https://www.somesite.com/head')
	r = requests.options('https://www.somesite.com/options')



 
