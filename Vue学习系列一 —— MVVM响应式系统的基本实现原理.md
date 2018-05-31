### MVVM是什么
MVVM是Model-View-ViewModel的简写。它模式是MVC—>MVP—>MVVM的进化版。     
Model负责用JavaScript对象表示，View负责UI界面显示，两者做到了最大限度的分离。     
而把Model和View关联起来的就是ViewModel。ViewModel负责把Model的数据同步到View显示出来，还负责把View的界面修改同步回Model更新数据。


### 主流MVVM框架和实现做法  

+ 脏值检查（angular.js） 
+ 发布者-订阅者模式+数据劫持（vue.js）

**脏值检查**: angular.js 是通过脏值检测的方式来比对数据是否有变更而决定是否更新视图。   
原理是，拷贝一份`copy_viewModel`在内存中,用户操作导致`viewModel`发生改变的行为时，框架都会把`copy_viewModel`和最新的`viewModel`进行深度比较，一旦发现有属性发生变化，则重新渲染与之绑定的DOM节点。  
最简单的方式就是通过`setInterval()`定时轮询检测数据变动，angular触发时进入脏值检测。但只限 **指定的事件** (如：用户点击，输入操作，ajax请求，setInterval，setTimeout等...)，否则需手动调用`apply`函数去强制执行一次脏检查。   

**数据劫持**: vue.js 则是采用数据劫持结合发布者-订阅者模式的方式，通过`Object.defineProperty()`来劫持各个属性的`setter`，`getter`在数据变动时发布消息给订阅者，触发相应的监听回调，而产生更新数据和视图。


### vue数据双向绑定原理

![官网数据绑定说明图](http://upload-images.jianshu.io/upload_images/1947234-08ef4df6f7f188fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

原理图告诉我们，data属性定义了getter、setter对属性进行劫持，当属性值改变是就会notify通知watch对象，而watch对象则会重新触发组件呈现功能，继而更新view上的DOM节点树。
反之，view上输入数据时，也会触发data变更，也会触发订阅者watch更新,这样子model数据就可以实时更新view上的数据变化。这样一个过程就是vue的数据双向绑定了。


vue是通过数据劫持的方式来做数据绑定的，其中最核心的方法便是通过`Object.defineProperty()`来实现对属性的劫持，达到监听数据变动的目的。

#### Object.defineProperty
`Object.defineProperty`是ES5一个方法，可以直接在一个对象上定义一个新属性，或者修改一个已经存在的属性，并返回这个对象，对象里目前存在的属性描述符有两种主要形式：**数据描述符**和 **存取描述符**。  
**数据描述符**是一个拥有可写或不可写值的属性。  
**存取描述符**是由一对getter-setter函数功能来描述的属性。   
**描述符必须是两种形式之一；不能同时是两者。即：有值和可写，或者可get和set**  
属性描述符包括：
+ Configurable(可配置性相当于属性的总开关，只有为true时才能设置，而且不可逆)、
+ Enumerable(是否可枚举，为false时for..in以及Object.keys()将不能枚举出该属性)、
+ Writable(是否可写，为false时将不能够修改属性的值)、
+ Value(属性的值,默认为undefined)、
+ Get(一个给属性提供getter的方法)、
+ Set(一个给属性提供setter的方法)、

```
var Book = {}
Object.defineProperty(Book, 'name', {
  get: function () {
    return '《' + name + '》'
  },
  set: function (value) {
    name = value;
    console.log('你取了一个书名叫做' + value);
  }
})

console.log(Book.name);  // 《》
Book.name = 'vue权威指南';  // 你取了一个书名叫做vue权威指南
console.log(Book.name);  // 《vue权威指南》
```

#### 实现过程

我们已经知道怎么实现数据的双向绑定，首先要对数据进行劫持监听，所以我们需要设置一个监听器`Observer`，用来监听所有属性。如果属性发上变化了，就需要告诉订阅者`Watcher`看是否需要更新。因为订阅者是有很多个，所以我们需要有一个消息订阅器`Dep`来专门收集这些订阅者，然后在监听器`Observer`和订阅者`Watcher`之间进行统一管理的。接着，我们还需要有一个指令解析器`Compile`，对每个节点元素进行扫描和解析，将相关指令对应初始化成一个订阅者`Watcher`，并替换模板数据或者绑定相应的函数，此时当订阅者`Watcher`接收到相应属性的变化，就会执行对应的更新函数，从而更新视图。   
因此接下去我们执行以下4个步骤，实现数据的双向绑定：

1. 实现一个监听器`Observer`，用来劫持并监听所有属性，如果有变动的，就拿到最新值并通知订阅者。
2. 实现一个订阅者`Watcher`，连接`Observer`和`Compile`。可以订阅并收到每个属性的变化通知并执行指令绑定的相应函数，从而更新视图。
3. 实现一个解析器`Compile`，可以扫描和解析每个节点的相关指令，并根据初始化模板替换数据，以及绑定相应的更新函数。
4. mvvm入口函数，整合以上三者。

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <h2>{{title}}</h2>
        <input v-model="name">
        <h1>{{name}}</h1>
        <button v-on:click="clickMe">click me!</button>
        <p>aaaa{{xxx}}zzzz</p>
    </div>
    <!-- <h1 id="name">{{name}}</h1> -->
</body>

</html>
<script>
    /**** 
     * Observer
     * 
     * */
    //初始化数据监听器
    function observe(data) {
        //验证传入的参数格式
        if (!data || typeof data !== 'object') {
            return;
        }
        // var dep = new Dep(); //创建订阅器Dep
        // console.log(dep)
        //遍历所有属性
        Object.keys(data).forEach(function (key) {
            defineReactive(data, key, data[key])//所有数据，单个键，单个值
            console.log(data)
            console.log(key)
            console.log(data[key])
        })
        console.log(Object.keys(data))
    }

    //监听所有属性
    function defineReactive(data, key, val) {
        observe(val); // 递归遍历所有子属性
        var dep = new Dep();//创建订阅器Dep
        Object.defineProperty(data, key, {
            enumerable: true, // 可枚举
            configurable: false, // 可配置
            get: function () {//返回它本身
                console.log(Dep)
                console.log(Dep.target)
                if (Dep.target) { // 判断是否需要添加订阅者
                    dep.addSub(Dep.target); // 在这里添加一个订阅者
                }
                return val;
            },
            set: function (newVal) {//返回更新值
                val = newVal;
                console.log('属性' + key + '已经被监听了，现在值为：“' + newVal.toString() + '”');
                console.log(dep)
                dep.notify(); // 如果数据变化，通知所有订阅者
            }
        })
    }
    console.log(Dep)
    Dep.target = null;

    //订阅器容器
    function Dep() {
        this.subs = [];
    }

    //订阅器原型方法
    Dep.prototype = {
        //添加进订阅器容器
        addSub: function (sub) {
            this.subs.push(sub);
        },
        //通知所有订阅者
        notify: function () {
            this.subs.forEach(function (sub) {
                console.log(sub)
                sub.update();
            });
        }
    };
    /**** 
     * Watcher
     * 
     * */
    //初始化Watcher订阅者
    function Watcher(vm, exp, cb) {//实例本身， 模板键值，模板值重新赋值方法
        console.log(vm)
        console.log(exp)
        console.log(cb)
        this.cb = cb;
        this.vm = vm;
        this.exp = exp;
        this.value = this.get();  // 将自己添加到订阅器的操作
    }

    Watcher.prototype = {
        update: function () {
            this.run();
        },
        run: function () {
            var value = this.vm.data[this.exp];
            var oldVal = this.value;
            if (value !== oldVal) {
                this.value = value;
                this.cb.call(this.vm, value, oldVal);//实例的赋值方法call到订阅者
            }
        },
        //让实例设置的属性强制映射到结构树上
        get: function () {
            console.log(Dep.target)
            console.log(Dep)
            Dep.target = this;  // 缓存自己
            var value = this.vm.data[this.exp]  // 强制执行监听器里的get函数
            Dep.target = null;  // 释放自己
            return value;
        }
    };
    /**** 
     * Compile
     * 
     * */
    function Compile(el, vm) {//dom节点，实例对象
        this.vm = vm;
        this.el = document.querySelector(el);
        this.fragment = null;
        this.init();
    }
    Compile.prototype = {
        // 初始化
        init: function () {
            if (this.el) {
                this.fragment = this.nodeToFragment(this.el);
                this.compileElement(this.fragment);
                this.el.appendChild(this.fragment);//挂载点载入模板碎片
            } else {
                console.log('Dom元素不存在');
            }
        },
        //创建一个fragment片段，用于解析的dom节点
        nodeToFragment: function (el) {
            var fragment = document.createDocumentFragment();//创建fragment-DOM模板碎片
            var child = el.firstChild;
            while (child) {
                // 将Dom元素移入fragment中
                fragment.appendChild(child);
                child = el.firstChild
            }
            return fragment;
        },
        //获取起始节点下所有节点并且递归遍历所有符合{{}}的指令
        compileElement: function (el) {
            var childNodes = el.childNodes;
            var self = this;
            //数组分割的方法作用于起始节点下所有节点并遍历每个节点执行对应方法
            [].slice.call(childNodes).forEach(function (node) {
                var reg = /\{\{(.*)\}\}/;//{{}}指令的正则
                var text = node.textContent;//节点的内容

                //v-model指令和事件指令的解析编译
                if (self.isElementNode(node)) {
                    self.compile(node);
                } else if (self.isTextNode(node) && reg.test(text)) {  // 判断是否是符合这种形式{{}}的指令
                    self.compileText(node, reg.exec(text)[1]);
                }

                if (node.childNodes && node.childNodes.length) {
                    self.compileElement(node);  // 继续递归遍历子节点
                }
            });
        },
        // 执行v-model指令和事件指令的解析编译
        compile: function (node) {
            var nodeAttrs = node.attributes;//获取该元素上的长度
            var self = this;
            //遍历该元素上的所有属性
            Array.prototype.forEach.call(nodeAttrs, function (attr) {
                var attrName = attr.name;
                if (self.isDirective(attrName)) {
                    var exp = attr.value;//指定model的value值
                    var dir = attrName.substring(2);
                    if (self.isEventDirective(dir)) {  // 事件指令
                        self.compileEvent(node, self.vm, exp, dir);
                    } else {  // v-model 指令
                        self.compileModel(node, self.vm, exp, dir);
                    }
                    node.removeAttribute(attrName);
                }
            });
        },
        //执行{{}}的节点的值
        compileText: function (node, exp) {//每个符合{{}}的节点，{{}}里面的内容值
            var self = this;
            var initText = this.vm[exp];
            this.updateText(node, initText);  // 将初始化的数据初始化到视图中
            new Watcher(this.vm, exp, function (value) { // 生成订阅器并绑定更新函数
                self.updateText(node, value);
            });
        },
        //执行事件的节点的值
        compileEvent: function (node, vm, exp, dir) {
            var eventType = dir.split(':')[1];
            var cb = vm.methods && vm.methods[exp];

            if (eventType && cb) {
                node.addEventListener(eventType, cb.bind(vm), false);
            }
        },
        //执行模块的节点的值
        compileModel: function (node, vm, exp, dir) {
            var self = this;
            var val = this.vm[exp];
            this.modelUpdater(node, val);
            new Watcher(this.vm, exp, function (value) {
                self.modelUpdater(node, value);
            });

            node.addEventListener('input', function (e) {
                var newValue = e.target.value;
                if (val === newValue) {
                    return;
                }
                self.vm[exp] = newValue;
                val = newValue;
            });
        },
        //更新文本
        updateText: function (node, value) {
            node.textContent = typeof value == 'undefined' ? '' : value;
        },
        //更新模块
        modelUpdater: function (node, value, oldValue) {
            node.value = typeof value == 'undefined' ? '' : value;
        },
        // 判断是是不是v-指令
        isDirective: function (attr) {
            return attr.indexOf('v-') == 0;
        },
        // 判断是是不是on:事件指令
        isEventDirective: function (dir) {
            return dir.indexOf('on:') === 0;
        },
        // 判断元素节点 元素类型等于1
        isElementNode: function (node) {
            return node.nodeType == 1;
        },
        // 判断文本节点
        isTextNode: function (node) {
            return node.nodeType == 3;
        }
    }
    /**** 
     * Observer和Watcher
     * 
     * */
    function SelfVue(options) {// 整个实例对象   //data, el, exp 所有数据，选中元素，模板键值
        var self = this;
        this.vm = this;
        this.data = options.data;
        this.methods = options.methods;
        //赋值时，属性的绑定做一层封装
        Object.keys(this.data).forEach(function (key) {
            self.proxyKeys(key);  // 绑定代理属性
        });
        //劫持并监听所有属性
        observe(this.data);
        //解析器解析挂载点的指令
        new Compile(options.el, this.vm)//挂载点，实例对象
        options.mounted.call(this); // 所有事情处理好后执行mounted函数

        // el.innerHTML = this.data[exp];  // 初始化模板数据的值 // 内容为设置的键值
        // console.log(el.innerHTML)
        // console.log(this)
        // new Watcher(this, exp, function (value) {//selfvue本身，模板键值，模板值为监听的新值
        //     el.innerHTML = value;
        // });
        return this;
    }
    //让selfVue的属性代理为访问selfVue.data的属性
    SelfVue.prototype = {
        proxyKeys: function (key) {
            var self = this;
            Object.defineProperty(this, key, {
                enumerable: false,
                configurable: true,
                get: function proxyGetter() {
                    return self.data[key];
                },
                set: function proxySetter(newVal) {
                    self.data[key] = newVal;
                }
            });
        }
    }
    /**** 
     * 实例
     * 
     * */
    var selfVue = new SelfVue({
        el: '#app',
        data: {
            title: 'hello world',
            name: 'null',
            xxx: 'cjh'
        },
        methods: {
            clickMe: function () {
                this.title = 'hello world';
            }
        },
        mounted: function () {
            window.setTimeout(() => {
                this.title = '你好';
            }, 2000);
        }
    });

    // window.setTimeout(function () {
    //     selfVue.title = '你好';
    // }, 2000);
    // window.setTimeout(function () {
    //     selfVue.name = 'canfoo';
    // }, 2500);

    // //实例
    // var ele = document.querySelector('#name');
    // var selfVue = new SelfVue({
    //     name: 'hello world'
    // }, ele, 'name');
    // console.log(ele)
    // console.log('name')

    // window.setTimeout(function () {
    //     console.log('name值改变了');
    //     selfVue.name = 'canfoo';
    // }, 2000);


    // //实例
    // var library = {
    //     book1: {
    //         name: ''
    //     },
    //     book2: ''
    // };
    // observe(library);
    // library.book1.name = 'vue权威指南'; // 属性name已经被监听了，现在值为：“vue权威指南”
    // library.book2 = '没有此书籍';  // 属性book2已经被监听了，现在值为：“没有此书籍”
    // console.log(library)
</script>
```






##### 参考链接：

[深入响应式原理](https://cn.vuejs.org/v2/guide/reactivity.html)   
[剖析Vue原理&实现双向绑定MVVM](https://segmentfault.com/a/1190000006599500)    
[《响应式系统的基本原理》.js](https://github.com/answershuto/VueDemo/blob/master/%E3%80%8A%E5%93%8D%E5%BA%94%E5%BC%8F%E7%B3%BB%E7%BB%9F%E7%9A%84%E5%9F%BA%E6%9C%AC%E5%8E%9F%E7%90%86%E3%80%8B.js)    
[JavaScript实现MVVM之我就是想监测一个普通对象的变化](http://hcysun.me/2016/04/28/JavaScript%E5%AE%9E%E7%8E%B0MVVM%E4%B9%8B%E6%88%91%E5%B0%B1%E6%98%AF%E6%83%B3%E7%9B%91%E6%B5%8B%E4%B8%80%E4%B8%AA%E6%99%AE%E9%80%9A%E5%AF%B9%E8%B1%A1%E7%9A%84%E5%8F%98%E5%8C%96/)

