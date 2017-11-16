>**AJAX**即“Asynchronous Javascript And XML”（异步JavaScript和XML），是指一种创建交互式网页应用的网页开发技术。

####Ajax的核心是*XMLHttpRequest*对象
**关键流程：**
1. 发送异步请求
2. 接受响应
3. 执行回调

JQ对Ajax进行了封装:
+ 第一层：$.ajax()
+ 第二层：$.get() , $.post() , load()
+ 第三层：$.getScript() , $getJSON()

***
**$.ajax()方法：**

    
    语法：$.ajax({name:value, name:value, ... })

+ async  /  布尔值，表示请求是否异步处理。默认是 true。
+ beforeSend(xhr)  /  发送请求前运行的函数。
+ cache  /  布尔值，表示浏览器是否缓存被请求页面。默认是 true。
+ complete(xhr,status)  /  请求完成时运行的函数（在请求成功或失败之后均调用，即在 success 和 error 函数之后）。
+ contentType  /  发送数据到服务器时所使用的内容类型。默认是："application/x-www-form-urlencoded"。
+ context  /  为所有 AJAX 相关的回调函数规定 "this" 值。
+ data  /  规定要发送到服务器的数据。
+ dataFilter(data,type)  /  用于处理 XMLHttpRequest 原始响应数据的函数。
+ dataType  /  预期的服务器响应的数据类型。
+ error(xhr,status,error)  /  如果请求失败要运行的函数。
+ global  /  布尔值，规定是否为请求触发全局 AJAX 事件处理程序。默认是 true。
+ ifModified  /  布尔值，规定是否仅在最后一次请求以来响应发生改变时才请求成功。默认是 false。
+ jsonp  /  在一个 jsonp 中重写回调函数的字符串。
+ jsonpCallback  /  在一个 jsonp 中规定回调函数的名称。
+ password  /  规定在 HTTP 访问认证请求中使用的密码。
+ processData  /  布尔值，规定通过请求发送的数据是否转换为查询字符串。默认是 true。
+ scriptCharset  /  	规定请求的字符集。
+ success(result,status,xhr)  /  当请求成功时运行的函数。
+ timeout  /  设置本地的请求超时时间（以毫秒计）。
+ traditional  /  布尔值，规定是否使用参数序列化的传统样式。
+ type  /  规定请求的类型（GET 或 POST）。
+ url  /  	规定发送请求的 URL。默认是当前页面。
+ username  /  规定在 HTTP 访问认证请求中使用的用户名。
+ xhr  /  用于创建 XMLHttpRequest 对象的函数。

***
**load()方法：**

    语法：load( [url [,data] [,callback] )

    url：请求页面的URL地址
    data：发送服务器的数据
    callback：请求完成是的回调函数，无论请求是否成功或者失败

回调函数有三个参数
+ data / 返回的内容
+ textStatus / 请求的状态
+ XMLHttpRequest  / XHR对象

>**load()方法，无论Ajax请求是否成功，只要请求完全(complete)后，回调函数(callback)就被触发。**

>**load()方法与load()事件是有区别的，虽然名字一样，但是调用的时候是根据参数的进行判别的。**

***
**$.get()方法：**


    语法：$.get( [url [,data] [,callback] [,type] )

数据返回成功，就可以调用回调函数。

回调函数有两个参数
+ data / 返回的内容
+ textStatus / 请求的状态

***
**$.post()方法：**


    语法：$.post( [url [,data] [,callback] [,type] )

数据返回成功，就可以调用回调函数。

回调函数有两个参数
+ data / 返回的内容
+ textStatus / 请求的状态

>**$.post方式更$.get基础一样。但是还是有区别的。**
1. GET会把参数放在URL后面传递，POST则作为HTTP消息的实体内容。
2. GET会限制传输的大小（不大于2KB），而POST理论上是无限制的。
3. GET数据会被缓存，有安全问题。而POST没有。
4. GET和POST在服务器的获取方式不同。

***
**$.getScipt()方法：**


    语法：$.getScipt( [url  [,callback])


与load()方法相同，不过是用来加载和调用一个JS文件。

回调函数有两个参数
+ url  / 链接地址
+ callback / 回调函数

***
**$.getJSON()方法：**


    语法：$.getJSON( [url [,data] [,callback])

与$.getScipt()方法相同，不过是用来加载一个JSON文件。

回调函数有两个参数
+ url  / 链接地址
+ data / 传递的数据
+ callback / 回调函数

>附加一张ajax的脑图：

![JQuery.Ajax.png](http://upload-images.jianshu.io/upload_images/1947234-067ea8890f9de880.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
