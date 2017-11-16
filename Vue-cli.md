> Vue-cli是Vue的脚手架工具 。
\
\*  “脚手架”是一种元编程的方法，用于构建基于数据库的应用。许多MVC框架都有运用这种思想。程序员编写一份specification（规格说明书），来描述怎样去使用数据库；而由（脚手架的）编译器来根据这份specification生成相应的代码，进行增、删、改、查数据库的操作。我们把这种模式称为"脚手架"

######  安装node环境

![node.png](http://upload-images.jianshu.io/upload_images/1947234-6485f656c872fba6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######  安装 vue 和 vue-cli  
```  
npm install --global vue-cli  
``` 

![vue.png](http://upload-images.jianshu.io/upload_images/1947234-39aa051c654c1cbe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######  vue list 查看可用的模板
```  
vue list
``` 

![vue list.png](http://upload-images.jianshu.io/upload_images/1947234-0196e52ac9135e86.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######  安装webpack模板（可以安装其他模板的）
```  
vue init webpack my-project
``` 

### 项目配置

![image.png](http://upload-images.jianshu.io/upload_images/1947234-ec88fec979966427.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 配置解释：
```
  A newer version of vue-cli is available.

  latest:    2.9.1
  installed: 2.8.2

? Project name my-project                    // 项目名称
? Project description A Vue.js project       // 项目描述,会在README.md文件生成输入的内容
? Author H3108 <jackie3108@126.com>          // 作者,如果有git，就是读取git的User信息
? Vue build standalone
? Install vue-router? Yes                    // 否安装Vue路由
? Use ESLint to lint your code? Yes          // ESLint管理代码(ES6代码风格检查器)
? Pick an ESLint preset Standard             // ESlint-类型
? Setup unit tests with Karma + Mocha? Yes   // 使用单元测试工具karma和mocha 默认是
? Setup e2e tests with Nightwatch? Yes       // 使用e2e测试框架 NightWatch 默认是

   vue-cli · Generated "my-project".

   To get started:

     cd my-project
     npm install
     npm run dev

   Documentation can be found at https://vuejs-templates.github.io/webpack
```
###### 项目目录：
进入项目之后安装依赖
```
cd sell
npm install / cnpm install
```

![my-project.png](http://upload-images.jianshu.io/upload_images/1947234-5b0888b8b49f1bca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
/build    webpack配置文件
/config   项目配置文件
/dist     发布目录
/node_modules    依赖目录
/src      存放的项目代码
/static   直接引用文件
/test     单元测试文件
.babelrc babel  配置文件，把我们ES2105的代码通过它编译成ES5的。 
.editorconfig   编辑器配置 
.eslintignore   忽略语法检查的目录文件配置 
.eslintrc.js    eslint的配置文件 
.gitignore      配置git仓库的忽略 
.postcssrc.js   PostCSS的配置文件
index.html      项目入口模板文件 
package.json    node配置文件
```
###### 项目运行:
```
npm run dev / cnpm run dev
```
[http://localhost:8080/#/](http://localhost:8080/#/) 

浏览器会打开localhost的8080端口。项目运行成功。
_______
End.
