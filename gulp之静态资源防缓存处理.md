　　最近，因为校友网项目开始有些规模了。开始就要考虑对静态资源进行工程自动化的管理。一讲到前端的自动化工具，大家或许都会想到Grunt，Gulp，或者百度的FIS。这三个都有各自的特点，大家可以依据自己的喜好，选择工具。至于为什么选择Gulp，因为Grunt的gruntfile配置真的很头大好吗！简直看到头晕晕，但是还是有不少人喜欢这种方式的。然后FIS真心很强大，你所需要的，基本它都提供了，并且做得很好很简单，如果你急于马上使用可以赶紧去看看。而我为什么不用呢，感觉可能是因为，有点黑盒子？哈哈哈....不说了，让我们赶紧看看今天的主角——Gulp。
　　**定义：[gulp.js](http://gulpjs.com/) 是一种基于流的，代码优于配置的新一代构建工具。**
**　　**关于还要安装Node，怎么样用npm加载需要的Node模块，就不再赘述啦。当然使用yeoman来搭建手脚架是最快的，有兴趣的可以看看，慕课网里有噢。接下来看看我们的案例。我们的需求是，为了防止客户端的静态资源缓存，我们需要每次更新css或js的时候，通过md5或时间戳等方式重新命名静态资源。让客户端可以重新请求资源，而不是从缓存里取。然后html模板里的src也要做相应的修改。当然，这里还有个附加的需要就是，静态资源需要自行优化（压缩合并）。
静态资源优化
静态资源重命名
修改静态资源的引用路径

　　若是我们手动修改，这会有多麻烦呢？大家可以想一想，我们先用工具压缩了资源，然后手动更改名字，再打开相应的页面，改路径。这样一直枯燥的重复，不仅容易出错，而且尼玛工作量很大好嘛？！程序员自有懒人在，我们就站在懒巨人的肩膀上，沐浴春风。
　　那在gulpfile中，我们要用到的插件有哪些呢？
　　![](http://upload-images.jianshu.io/upload_images/1947234-d61676f60c412067.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
　　require完之后，我们就可以开始编写一个简单实用的版本控制的自动化工具了。
　　我们一步一步来，我们需要产出一个静态资源路径，我们首先要清空里面曾经的资源，防止有冗余。那我们就定义了一个clean任务，然后将src需要清理的文件夹引入，然后执行clean。src的第二个参数的{read:false}，是不读取文件加快程序。
　　![](http://upload-images.jianshu.io/upload_images/1947234-2748621aaa0356f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)　　
　　清理完之后，就可以对静态资源进行优化处理。那我们定义两个任务，一个是css的，一个是js的。在css里，还可以编译less或sass，这里我就没有做。然后js里同样可以编译coffee。我们来仔细看看下面的程序，首先任务名是css，然后src引入css文件，执行csso的压缩优化，然后重命名为*.min.css。接下来就是到了添加版本号，并将经过优化和版本控制的css输出到dist文件夹里。最后再用rev.manifest，将对应的版本号用json表示出来，这里可以参照下面第二张图。这样通过hash来精确定位到html模板中需要更改的部分。
　　![](http://upload-images.jianshu.io/upload_images/1947234-140315c2e2d56694.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
　　![](http://upload-images.jianshu.io/upload_images/1947234-aefa8534573bc81b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
　　接着便是最后一步，改变引用路径，这里我们将这个任务命名为rev。***记得rev任务要在生成mainfest之后进行，可以用gulp[]任务依赖来进行流程控制。***
　　![](http://upload-images.jianshu.io/upload_images/1947234-6d45256b74cf88a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
　　src引入一个数组，前一个是引入刚才生成的json文件，后一个是需要更改的html模板，当然我这里是jsp。然后replaceReved: true就可以成功替换了。最后将替换过的文件输出即可，这里我输出到了原来引入的路径，这样就可以成功替换了。如果你在开发的时候需要不断调试，还可以加上gulp.watch，实时监控文件变化，然后动态做出响应。当然还是推荐开发与上线分开不同的文件夹进行管理。（可是我这坑爹的组长没有！妈蛋！）最后来看一下按照这样执行的大概结果。
　　![](http://upload-images.jianshu.io/upload_images/1947234-b026b69d4b90d2a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
　　这是成功生成到dist的静态资源，都加上了相应的后缀。***这里要着重强调，修改的仅仅是后缀名，而之前的路径是不会帮你修改的。也就是说，你的原来的Html引入静态资源的时候，路径就要预先写好，然后rev来帮你修改后缀。至于是不是有更好的方法，我暂时还不清楚～～***
![](http://upload-images.jianshu.io/upload_images/1947234-223622a059626018.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
　　这是html模板上的引用路径做了对应的变化。
 
　　是不是讲的十分不清楚呢？！哈哈哈，我跳过了一些的步骤 ，我只是想推荐大家gulp-rev配合gulp-rev-collector，这个自动化静态资源版本控制工具用gulp是可以做到的！所以要是大家不大会使用gulp的可以先自行寻找教程，最后再回来实现一遍。
