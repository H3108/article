> 最近的一个项目中使用到了swipe.js这个插件
感觉非常的好用的和简洁
重点是比swiper小超多
官方网站：
http://swipejs.com/
https://github.com/bradbirdsall/Swipe

#####简介

swipe.js是一个轻量级的移动滑动组件，支持触摸移动，支持响应式页面。
下面是从GitHub上翻译的用法和文档

####用法
Swipe只需添加很简单的一段代码即可，如下
```
<div id='slider' class='swipe'>
  <div class='swipe-wrap'>
    <div class='wrap'></div>
    <div class='wrap'></div>
    <div class='wrap'></div>
  </div>
</div>
```
以上代码是最初需要的结构--一系列元素包裹在两个容器中，你可以在wrap中添加任何你想要的内容。最外面的div（即slide）需要设置一下的js函数：

```window.mySwipe = Swipe(document.getElementById('slider'));```

同样的，Swipe需要往样式表中添加一些代码

```
.swipe {
  overflow: hidden;
  visibility: hidden;
  position: relative;
}
.swipe-wrap {
  overflow: hidden;
  position: relative;
}
.swipe-wrap > div {
  float:left;
  width:100%;
  position: relative;
}
```
#### 配置选项
* Swipe可以扩展可选参数-通过设置对象的键值对:
* startSlide Integer (默认:0) - Swipe开始的索引
* speed Integer (默认:300) - 前进和后台的速度，单位毫秒.
* auto Integer - 自动滑动 (time in milliseconds between slides)
* continuous Boolean (默认:true) -是否可以循环播放（注：我设置为false好像也是循环的）
* disableScroll Boolean (默认:false) - 停止触摸滑动
* stopPropagation Boolean (默认:false) -停止事件传播
* callback Function - 回调函数，可以获取到滑动中图片的索引.
* transitionEnd Function - 在最后滑动转化是执行.

#### 举例
```
window.mySwipe = new Swipe(document.getElementById('slider'), {
  startSlide: 2,
  speed: 400,
  auto: 3000,
  continuous: true,
  disableScroll: false,
  stopPropagation: false,
  callback: function(index, elem) {},
  transitionEnd: function(index, elem) {}
});
```
#### Swipe API
swipe扩展了几个函数，以便于更好的通过脚本来控制滑动。
+ prev() 上一页
+ next() 下一页
+ getPos() 返回当前div（class='wrap'的div）的索引
+ getNumSlides() 返回滑块总数（貌似无效）
+ slide(index, duration) 设置滑到的索引 (duration: 转化的速度，单位毫秒)
