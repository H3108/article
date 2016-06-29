>对于js的调试，一般我们经常用alert()或者console.log()进行调试。但是alert()会让程序中断，而console.log()则不会。虽然用的很多，但是我就只知道console.log()而已。今天专门去研究一下这个东西**console**。

######JS原生中默认是没有console对象,这是宿主对象（也就是游览器）提供的内置对象。 用于访问调试控制台, 在不同的浏览器里效果可能不同。
比如：现在大部分的游览器都是带有调试功能的。而低版本的IE就没有，所以在低版本IE的window中，console对象并不存在。【所以还需要低版本支持的朋友要注意】。

打印window对象中的console：


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-a3143305020b9e67.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 ######接下来打印一下console都有一些什么功能：
这是谷歌和火狐`for(var i in console){console.log(i)}`出来，对应游览器所支持的console方法：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-bcaa0a54184e4421.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-97cac607fab9df20.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####console 的方法
*******
#####console.log(),console.debug(),console.info()
`console.log`方法用于在控制台输出信息。它可以接受多个参数，逗号分隔。它会自动在每次输出的结尾，添加换行符。没有返回值回会返回`undefined`。【console.log大家用的很熟的】

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-a76fa1f98e4e54aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果第一个参数是格式字符串（使用了格式占位符），console.log方法将依次用后面的参数替换占位符，然后再进行输出。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-3f392b4a58a0c308.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

占位符格式如下表：

| 模式 | 类型 |
| ------------- |:-------------:|
| %s | 字符串 | 
| %d,%i | 整数 |
| %f| 浮点数 |
| %o | 对象超链接 |
| %c | CSS格式化样式 |


`console.log`方法和`console.debug()`与`console.info()`，几乎用法完全一样，唯一不同的就是现实时候的表现形式了。
*注意一点的是：IE不支持debug()方法*

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-72522c009077889a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#####console.assert()
接收至少两个参数，第一个参数的值或返回值为`false`的时候，将会在控制台上抛出一个异常并将其余参数作为异常描述输出.

        console.assert(false,123) //抛出错误，并且输出，返回undefined
        console.assert(true,123) //没有错误，返回undefined


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-9244ce012afc5c9c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#####console.count()
`console.count()`方法用于计数，输出它被调用了多少次。

    (function() {
     for (var i = 0; i < 5; i++) { 
            console.count('count'); 
     }
    })();

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-a8f4934714cb431f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`console.count()`方法里面可以传入一个字符串作为参数，作为标签，对执行次数进行分类

    function greet(user) { 
      console.count(user); 
      return "hi " + user;
    }
    greet('bob')
    greet('alice')
    greet('bob')

      

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-e26749e7cb835d50.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#####console.clear()
`console.clear()`清空控制台内容。


#####console.dir()
`console.dir()`方法用来对一个对象进行检查，并以易于阅读和打印的格式显示。
    
    var obj = {
        name: 'c',
        age: '20',
        type: '1'
      };
      console.dir(obj);

      var arr = [1,2,3]
      console.dir(arr)

      var s = 'sdfs'
      console.dir(s)

      var n = '123'
      console.dir(n)

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-9b7fe5d69db01f48.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-380ed15a0e135b77.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#####console.error(),console.warn()
`console.error(),console.warn()`方法用于输出错误和警告信息，用法和常见的`console.log`方法一样，不同点在于输出时候的表现形式。一个是黄色的警告形式一个是红色的错误形式。而`console.error()`方法会标记为错误的地方。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-fdf9ae9b454e59e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#####console.table()
`console.table()`方法可以将传入的对象或数组这些复合数据以表格形式输出。

      var arr= [ 
             { num: "1"},
             { num: "2"}, 
             { num: "3" }
        ];
      console.table(arr);

      var obj= {
           a:{ num: "1"},
           b:{ num: "2"},
           c:{ num: "3" }
      };
      console.table(obj);

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-83304750427dc623.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-812270e5dfdbca79.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#####console.time(),console.timeEnd()
`console.time(),console.timeEnd()`方法计算一个操作的执行的时间`console.time()`是开始，`console.timeEnd()`是结束。可以传一个参数，参数为计时器的名称。

    console.time('计时器1');
        for (var i = 0; i < 100; i++) {
            for (var j = 0; j < 100; j++) {}
       }
    console.timeEnd('计时器1');
    console.time('计时器2');
        for (var i = 0; i < 1000; i++) {
            for (var j = 0; j < 1000; j++) {}
       }
    console.timeEnd('计时器2');

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-7c62be89609ddc33.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#####console.group(),console.groupCollapsed(),console.groupend()
`console.group()和console.groupCollapsed()`这两个方法用于将显示的信息分组，可以把信息进行折叠和展开。他们都可以传递一个参数，参数默认是分组名。
用法都是一样的。唯一区别就是`console.group()`是默认展开的，而`console.groupCollapsed()`默认是收起的。

       console.group('第一层');
	    console.group('第二层');
                  
                  console.log('error');
                  console.error('error');
                  console.warn('error');

	    console.groupEnd(); 
       console.groupEnd(); 

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-4c262db546c4e424.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    console.groupCollapsed('第一层');
	    console.groupCollapsed('第二层');

		    console.log('error');
		    console.error('error');
		    console.warn('error');

    	console.groupEnd(); 
    console.groupEnd(); 


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-f29610a6ea357e55.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#####console.profile(),console.profileEnd()
`console.profile(),console.profileEnd()`方法就比较高大上点了，用来新建一个性能测试器，可以评估某段代码的性能，可以传一个参数，为生成的性能测试器的名字。

    function profile() { 
        for (var i = 0; i < 10000; i++) { 
            i++;
        }
    }
    console.profile('性能分析');
        profile();
    console.profileEnd();

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-097b7b6cd11fe16c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

***
> **profile在哪里呢。打开控制台。Profile就是了。也可手动添加**

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-0510fe261f99125e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*****

#####console.trace()
`console.trace()`方法用来追踪函数的调用过程。在复杂的架构中可以查找到对应的调用路径。
 
    function d(a) { 
        console.trace();
        return a;
    }
    function b(a) { 
        return c(a);
    }
    function c(a) { 
        return d(a);
     }
    var a = b('123');

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-6a3c5a771f5c3487.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

****
PS：很多互联网公司都会在页面中写类似的推广呀，招聘呀。。。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1947234-a972a42a28e03c7a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

试着在你的网站写写类似的语句，说不定哪天，那个程序媛审查一下元素，就和你去拯救世界了呢。O(∩_∩)O~

    var cons = console;
    if (cons) { 
          cons.log('想和我共同保护世界，维护世界的和平吗？\n'); 
          cons.log("请在邮件中注明：%c来自:浩3108的简书", "color:red;font-weight:bold;");
    }


>暂时告一段。
