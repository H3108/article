```
render: h => h(App)
```

```
// ES5  
(function (h) {  
  return h(App);  
});  
  
// ES6  
h => h(App); 

// JS
render: function (createElement) {
    return createElement(
      'h' + this.level,   // tag name 标签名称
      this.$slots.default // 子组件中的阵列
    )
}

```
