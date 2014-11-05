---
layout: post
title: python requests
tagline: [python] 
group: python
categories: [Python]
---
{% include codepiano/setup %}

## 说明 ##

> Requests allow you to send HTTP/1.1 requests. You can add headers, form data, multipart files, and parameters with simple Python dictionaries, and access the response data in the same way. It's powered by httplib and urllib3, but it does all the hard work and crazy hacks for you.

## 安装 ##

有[多种](http://www.python-requests.org/en/latest/user/install/)方式安装,这里采用`setuptools`

{% highlight python %}
git clone git://github.com/kennethreitz/requests.git
python setup.py install
{% endhighlight %}

## 快速入门 ##

用Requests来创建一个HTTP请求很简单

{% highlight python %}
import requests
r = requests.get('https://www.google.com/search?q=python')
{% endhighlight %}

这里创建了一个get请求，同时返回了一个`Response`对象.当然也可以传递参数的方式发送请求

{% highlight python %}
queryparams = {'q':'python'}
r = requests.get('https://www.google.com/search', params=queryparams)
{% endhighlight %}

当然对于其他的HTTP请求类型也同样支持：

{% highlight python %}
r = requests.post('https://www.somesite.com/post')
r = requests.put('https://www.somesite.com/put')
r = requests.delete('https://www.somesite.com/delete')
r = requests.head('https://www.somesite.com/head')
r = requests.options('https://www.somesite.com/options')
{% endhighlight %}

## Response ##

`Response`表示了一个HTTP响应对象，可以通过text的方式返回`r.text`，也可以是二进制的内容`r.content`。同样也可以返回原生的内容`r.raw`.此时是`requests.packages.urllib3.response.HTTPResponse`对象。

	r = requests.get('https://www.google.com/search?q=python',stream=True)
	r.raw
	>>> <requests.packages.urllib3.response.HTTPResponse object at 0x02367470>
	
	with open('sss.html','wb') as fd:
		for chunk in r.iter_content(1000):
			fd.write(chunk)

上面的例子将请求的内容原生返回并写入了指定的文件。对应返回的内容，也可以指定编码方式`r.encoding`.下表展示了`Response`中的属性和方法

<table class="table table-striped table-bordered">
<tr><td>status_code</td><td>返回的HTTP状态码</td></tr>
<tr><td>headers</td><td>以字典的形式返回响应的Headers,如<code>r.headers['content-type']</code></td></tr>
<tr><td>raw</td><td>类文件的形式返回请求对象，需要设置请求参数<code>stream=True</code></td></tr>
<tr><td>url</td><td>请求后的最终url</td></tr>
<tr><td>encoding</td><td>设置解析 <code>r.text</code>的编码</td></tr>
<tr><td>cookies</td><td>服务端响应的Cookies,CookieJar对象返回,如<code>r.cookies['cookie_name']</code></td></tr>
<tr><td>iter_content(chunk_size=1, decode_unicode=False)</td><td>迭代原生的相应数据，见上例</td></tr>
<tr><td>iter_lines(chunk_size=ITER_CHUNK_SIZE, decode_unicode=None)</td><td>与<code>iter_content</code>类似，只是按行读取。在返回大量数据时减少内存占用</td></tr>
<tr><td>content()</td><td>用byte返回响应的内容</td></tr>
<tr><td>text</td><td>用unicode返回响应的内容,可通过<code>r.encoding</code>设置编码</td></tr>
<tr><td>json(**kwargs)</td><td>json格式返回</td></tr>
</table>

## 再看Request ##

从前面知道提供了`get,post,put`等7个方法。其实这些都依赖于一个入口`requests.request(method, url, **kwargs)`来看看一个方法：

	def get(url, **kwargs):
	    kwargs.setdefault('allow_redirects', True)
	    return request('get', url, **kwargs)

而在主要的入口方法`request(method, url, **kwargs)`中有如下的参数：

<table class="table table-striped table-bordered">
<tr><td>method</td><td>必选</td><td>HTTP方法,<code>get,post,put,delete</code></td></tr>
<tr><td>url</td><td>必选</td><td>当前请求的URL</td></tr>
<tr><td>params</td><td>可选</td><td>当前请求的查询参数(get),字典或byte格式</td></tr>
<tr><td>data</td><td>可选</td><td>Form表单的请求参数(post),可以是字典、byte或文件</td></tr>
<tr><td>headers</td><td>可选</td><td>请求的headers</td></tr>
<tr><td>cookies</td><td>可选</td><td>请求的cookies</td></tr>
<tr><td>files</td><td>可选</td><td>迭代原生的相应数据，见上例</td></tr>
<tr><td>auth</td><td>可选</td><td>Auth tuple to enable Basic/Digest/Custom HTTP Auth</td></tr>
<tr><td>timeout</td><td>可选</td><td>超时设置</td></tr>
<tr><td>allow_redirects</td><td>可选</td><td>Boolean. Set to True if POST/PUT/DELETE redirect following is allowed</td></tr>
<tr><td>proxies</td><td>可选</td><td>Dictionary mapping protocol to the URL of the proxy</td></tr>
<tr><td>verify</td><td>可选</td><td> if True, the SSL cert will be verified. A CA_BUNDLE path can also be provided</td></tr>
<tr><td>stream</td><td>可选</td><td>if False, the response content will be immediately downloaded</td></tr>
<tr><td>cert</td><td>可选</td><td>if String, path to ssl client cert file (.pem). If Tuple, (‘cert’, ‘key’) pair</td></tr>
</table>