##### 官方解释：
Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

###### 状态管理模式
状态自管理应用包含以下几个部分：

+ state，驱动应用的数据源；
+ view，以声明方式将 state 映射到视图；
+ actions，响应在 view 上的用户输入导致的状态变化。

![单向数据流.png](http://upload-images.jianshu.io/upload_images/1947234-ac8aa954deb3a334.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当应用遇到**多个组件共享**状态时，**单向数据流**的简洁性很容易被破坏：

+ 多个视图依赖于同一状态。
+ 来自不同视图的行为需要变更同一状态。

[ 
例如：
在 template 中我们可以方便的访问项目列表并且进行过滤、排序等操作，
不过如果我们在另一个列表中也需要来展示相同的数据信息，继续按照这种方式实现的话我们不得不重新加载一遍数据，
而且如果用户在本地修改了某个列表数据，那么如何同步两个组件中的列表信息。

因为vue是组件化,多组件共享时，会遇到多个视图公用一个数据或者状态，多个方法或者动作共同改变同一个数据或者状态，所以会导致单向数据流被破坏而且代码混乱无法维护，所以才会有vuex出现。
]

**所以vuex就是把组件的共享状态抽取出来，用一个全局单例模式进行管理**

![vuex示意图.png](http://upload-images.jianshu.io/upload_images/1947234-1ad27c7e74cf5619.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上述是vuex官方解释，没听懂，没关系，咱们在抽象一下。

##### 抽象解释：
我们可以把
**vuex想象成一个“公共的属性和方法库”，把所有公共的地方合并在一起的大对象。**
或者把
**vuex想象成为了方便前端数据的操作而建立的一个“前端数据库”**

好，先酝酿一下。
我们来试着实现一个迷你的vuex。

因为模块间是不共享作用域的，那么B模块想要拿到A模块的数据，要怎么办？

>我们会定义一个全局变量，叫**store**吧，也就是**window.store**。
然后我们要把A模块要共享的数据作为属性挂到B模块上。
这样我们在B模块中通过**window.store**就获取到这个数据了。
之后我们就获取到了A模块和B模块共享的数据，叫做**state**。
虽然B模块拿到了A模块的数据**state**，但是这个数据不是一成不变的，
A模块是要操作这个数据的。
所以说我们就要在**state**这个数据改变时通知一下B模块
那写个自定义事件吧。。。

好了，这就是一个迷你的vuex。而vuex也就帮你做这点事儿的。

#### Vuex API 

###### Vuex 中 Store 的模板化定义如下：
```
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
const store = new Vuex.Store({
  state: {
  },
  actions: {
  },
  mutations: {
  },
  getters: {
  },  
  modules: {
    
  }
})
export default store

```
###### 上述代码中包含了定义 Vuex Store 时关键的 5 个属性：

+ state: state 定义了应用状态的数据结构，同样可以在这里设置默认的初始状态。
```
state: {
  projects: [],
  userProfile: {}
}
```
+ actions:Actions 即是定义提交触发更改信息的描述，常见的例子有从服务端获取数据，在数据获取完成后会调用store.commit()来调用更改 Store 中的状态。可以在组件中使用dispatch来发出 Actions。
```
actions: {
    LOAD_PROJECT_LIST: function ({ commit }) {
      axios.get('/secured/projects').then((response) => {
        commit('SET_PROJECT_LIST', { list: response.data })
      }, (err) => {
        console.log(err)
      })
    }
  }
```
+ mutations: 调用 mutations 是唯一允许更新应用状态的地方。
```
mutations: {
    SET_PROJECT_LIST: (state, { list }) => {
      state.projects = list
    }
  }
```
+ getters: Getters 允许组件从 Store 中获取数据，譬如我们可以从 Store 中的 projectList 中筛选出已完成的项目列表：
```
getters: {
 completedProjects: state => {
  return state.projects.filter(project => project.completed).length
 }
}
```
+ modules: modules 对象允许将单一的 Store 拆分为多个 Store 的同时保存在单一的状态树中。随着应用复杂度的增加，这种拆分能够更好地组织代码，更多细节参考[这里](http://link.zhihu.com/?target=http%3A//vuex.vuejs.org/en/modules.html)。


