# css-loader

- css-loader 使你能够使用类似@import和url（...）的方法实现require的功能
- style-loader 将所有的计算后的样式加入页面中

{ test: /.css$/, use: [ { loader: 'style-loader' }, { loader: 'css-loader', options: { root: '/', //修改css中url指向的根目录, 默认值为/, 对于绝对路径, css-loader默认是不会对它进行处理的 modules: false, //开启css-modules模式, 默认值为flase localIdentName: '[name]-[local]-[hash:base64:5]', //设置css-modules模式下local类名的命名 minimize: false, //压缩css代码, 默认false camelCase: false, //导出以驼峰化命名的类名, 默认false import: true, //禁止或启用@import, 默认true url: true, //禁止或启用url, 默认true sourceMap: false, //禁止或启用sourceMap, 默认false importLoaders: 0, //在css-loader前应用的loader的数目, 默认为0 alias: {} //起别名, 默认{} } } ] }

[

e^{i\pi} + 1

]
