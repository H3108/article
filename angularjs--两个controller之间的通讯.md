因为作用域是有层次的，所以可以利用作用域链传递事件。

传递事件有2种方式： * $broadcast: 触发的事件要通知整个事件系统（允许任意作用域处理这个事件）就要向下传播。 * $emit: 如果要提醒一个全局模块，需要通知更高层次的作用域时（例如$rootscope）需要把事件向上传递。

作用域上使用$on进行事件监听。


```
   app.controller('ParentController', function($scope) { 
         $scope.$on('$fromSubControllerClick', function(e,data){
             console.log(data); // hello
         });
     });
    app.controller('ChildController', function($scope) {
        $scope.sayHello = function() {
            $scope.$emit('$fromSubControllerClick','hello');
        };
    });
    //HTML
     <div ng-controller="ParentController">
       <div ng-controller="ChildController">
         <a ng-click="sayHello()">Say hello</a>
       </div>
     </div>

```
在这里想要说的另外一个问题就是事件传播的性能问题，$broadcast+$on的方式回通知所有的子作用域，这里就会有性能问题，所以推荐使用$emit+$on的方式,为了进一步提升性能，定义的事件处理函数要在作用域销毁时一起释放掉。

使用$emit+$on的方式需要我们将事件监听绑定在$rootScope上，例如：
```
angular
    .module('MyApp')
    .controller('MyController', ['$scope', '$rootScope', function MyController($scope, $rootScope) {
            var unbind = $rootScope.$on('someComponent.someCrazyEvent', function(){
                console.log('foo');
            });
            $scope.$on('$destroy', unbind);
        }
    ]);
```
但是这种方式有点繁琐，定义多个事件处理函数时整个人都不好了，所以我们来改进一下

利用装饰器来定义一个新的事件绑定函数：
```
angular
    .module('MyApp')
    .config(['$provide', function($provide){
        $provide.decorator('$rootScope', ['$delegate', function($delegate){
            Object.defineProperty($delegate.constructor.prototype, '$onRootScope', {
                value: function(name, listener){
                    var unsubscribe = $delegate.$on(name, listener);
                    this.$on('$destroy', unsubscribe);
                    return unsubscribe;
                },
                enumerable: false
            });
            return $delegate;
        }]);
    }]);
```
那么我们在控制器中定义事件处理函数时：
```
angular
    .module('MyApp')
    .controller('MyController', ['$scope', function MyController($scope) {
            $scope.$onRootScope('someComponent.someCrazyEvent', function(){
                console.log('foo');
            });
        }
    ]);
```





网址：http://www.tuicool.com/articles/InuMF3J









