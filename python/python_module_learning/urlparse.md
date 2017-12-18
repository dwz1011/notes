# urlparse学习

- urlparse模块主要是把url拆分为6部分，并返回元组
- 可以把拆分后的部分再组成一个url

********

**urlparse函数**

用法：

	urlparse.urlparse(urlstring[, scheme[, allow_fragments]])

将urlstring解析成6个部分，并返回元组(scheme, netloc, path, parameters, query, fragment)

例：

	>>> import urlparse
	>>> url=urlparse.urlparse('http://www.baidu.com/index.php?username=guol')
	>>> print url
	ParseResult(scheme='http', netloc='www.baidu.com', path='/index.php', params='', query='username=guol', fragment='')
	>>> print url.netloc
	www.baidu.com
	>>> print url.scheme
	http
	>>> print url.path
	/index.php

**urlunparse函数**

用法：

	urlparse.urlunparse(parts)
	
从一个元组构建一个url

例：
	
	>>> import urlparse
	>>> url=urlparse.urlparse('http://www.baidu.com/index.php?username=guol')
	>>> print url
	ParseResult(scheme='http', netloc='www.baidu.com', path='/index.php', params='', query='username=guol', fragment='')
	>>> u=urlparse.urlunparse(url)
	>>> print u
	http://www.baidu.com/index.php?username=guol
	
**urlsplit函数**

用法：

	urlparse.urlsplit(urlstring[, scheme[, allow_fragments]])
	
分析urlstring，返回一个包含5个字符串项目的元组：协议、位置、路径、查询、片段
`urlsplit()和urlparse()差不多，不过urlsplit()不切分URL的参数`

例：

	>>> import urlparse
	>>> url=urlparse.urlparse('http://www.baidu.com/index.php?username=guol')
	>>> print url
	ParseResult(scheme='http', netloc='www.baidu.com', path='/index.php', params='', query='username=guol', fragment='')
	>>> url=urlparse.urlsplit('http://www.baidu.com/index.php?username=guol')
	>>> print url
	SplitResult(scheme='http', netloc='www.baidu.com', path='/index.php', query='username=guol', fragment='')

**urlunsplit函数**

用法：
	
	urlparse.urlunsplit(parts)
	
urlunsplit使用urlsplit()返回的值组合成一个url

**urljoin函数**

用法：
 	
	urlparse.urljoin(base, url[, allow_fragments])
 
主要是拼接URL，它以base作为其基地址，然后与url中的相对地址相结合组成一个绝对URL地址
 
*如果基地址并非以字符/结尾的话，那么URL基地址最右边部分就会被这个相对路径所替换

例：

	>>> import urlparse
	>>> urlparse.urljoin('http://www.oschina.com/tieba','index.php')
	'http://www.oschina.com/index.php'
	>>> urlparse.urljoin('http://www.oschina.com/tieba/','index.php')
	'http://www.oschina.com/tieba/index.php'


