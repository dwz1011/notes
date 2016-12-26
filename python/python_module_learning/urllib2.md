# python2.7之urllib2

[官方文档](https://docs.python.org/2/library/urllib2.html)

###urlopen

	urlopen(url, data, timeout,....)
	
（1）第一个参数url即为URL，第一个参数URL是必须要传送的

（2）第二个参数data是访问URL时要传送的数据，data默认为空None

（3）第三个timeout是设置超时时间，timeout默认60s（socket._GLOBAL_DEFAULT_TIMEOUT）

	例：
	import urllib2
	url = 'https://www.baidu.com/'
	html = urllib2.urlopen(url)
	response = html.read()		

返回对象的参数： 
  
- **geturl()**	返回对象的url
- **info()**		返回HTTP的头部内容
- **getcode()**	返回响应的HTTP状态代码
- **read()**		返回整个html

###HTTPError

处理HTTP错误

	例：
	若请求的url不存在，通过urllib2.HTTPEerror来捕捉错误信息
	import urllib2
	url = "http://www.jnrain.com/go"
	try:
	    response = urllib2.urlopen(url)
	    print response.info()
	    print response.read()
	except urllib2.HTTPError, e:
	    print e.getcode()
	    print e.geturl()
	    
###Request

urlopen 还可以使用request对象来作为参数，首先要通过Resquest类来创建一个request对象
	
	例：
	import urllib2
	headers = {
		'User-Agent': user_agent,
	}
	url = 'http://www.jnrain.com/wForum/logon.php'
	request = urllib2.Request(url, headers=headers)
	#添加头部其他信息
	request.add_header(key, value)
	try:    
	    response = urllib2.urlopen(request)
	except urllib2.URLError, e:
	    print e.code
	headers = response.info()
	data = response.read()
	
**Request Objects**

- add_data()	
- get_method()
- has_data()
- get_data()
- add_header()
- add\_unredirected_header()
- has_header()
- get\_full_url()
- get_type()
- get_host()
- get_selector()
- get_header()
- header_items()
- set_proxy()
- get\_origin\_req_host()
- is_unverifiable()	

	
##Openers和Handlers

当获取一个URL，使用一个opener(一个urllib2.OpenerDirector的实例，urllib2.OpenerDirector可能名字可能有点让人混淆。)，正常情况下，我们使用默认opener -- 通过urlopen,但能够创建个性的openers，Openers使用处理器handlers，所有的“繁重”工作由handlers处理。每个handlers知道如何通过特定协议打开URLs，或者如何处理URL打开时的各个方面，例如HTTP重定向或者HTTP cookies。
如果你希望用特定处理器获取URLs你会想创建一个openers，例如获取一个能处理cookie的opener，或者获取一个不重定向的opener。
要创建一个opener，实例化一个OpenerDirector，然后调用不断调用.add_handler(some_handler_instance).
同样，可以使用build_opener，这是一个更加方便的函数，用来创建opener对象，他只需要一次函数调用。
build_opener默认添加几个处理器，但提供快捷的方法来添加或更新默认处理器。
其他的处理器handlers你或许会希望处理代理，验证，和其他常用但有点特殊的情况。
install_opener 用来创建（全局）默认opener。这个表示调用urlopen将使用你安装的opener。
Opener对象有一个open方法，该方法可以像urlopen函数那样直接用来获取urls：通常不必调用install_opener，除了为了方便。

	例：
	import urllib2
	import urllib2  
	httpHandler = urllib2.HTTPHandler(debuglevel=1)  
	httpsHandler = urllib2.HTTPSHandler(debuglevel=1)  
	opener = urllib2.build_opener(httpHandler, httpsHandler)  
	urllib2.install_opener(opener)  
	response = opener.open('http://www.baidu.com') 
	等同于：
	response = urllib2.urlopen('http://www.baidu.com')

###OpenerDirector

实例方法：

**1.add_handler**

- protocol_open
- http_error_typr
- protocol_error
- protocol_request
- protocol_response

**2.open**

**3.error**

####OpenerDirector的操作类

一个管理很多处理类（Handler）的类，这些Handler类都对应处理相应的协议，或者特殊功能

处理类：

**BaseHandler**

**HTTPErrorProcessor**

**HTTPDefaultErrorHandler**

**HTTPRedirectHandler**

**ProxyHandler**

**AbstractBasicAuthHandler**

**HTTPBasicAuthHandler**

**ProxyBasicAuthHandler**

**AbstractDigestAuthHandler**

**ProxyDigestAuthHandler**

**AbstractHTTPHandler**

**HTTPHandler**

**HTTPCookieProcessor**

**UnknownHandler**

**FileHandler**

**FTPHandler**

**CacheFTPHandler**



















