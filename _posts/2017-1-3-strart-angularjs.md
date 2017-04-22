---
layout: post
title:  "AngularJS启动流程"
date:   2017-01-3 03:06:05
categories: AngularJs
---


* content
{:toc}



## 启动流程
### 步骤一  
(1)用自执行函数的形式让整个代码加载完成之后立即执行  
```js
     (function(window,document,undefined))
```    
(2)在window上面暴露一个唯一的全局对象angular  
```js
    /** @name angular */
    //如果window.angular已经有值了，就把原有的赋值给前面angular，如果没有则用window.angular空对象赋值
    //给angular
    angular = window.angular || (window.angular = {}),
    angularModule,    
    uid = 0;
``` 
  &nbsp;(3)获取其他工具模块  
```js
   function publishExternalAPI(angular) {
  extend(angular, {
    'bootstrap': bootstrap,
    'copy': copy,
    'extend': extend,
    'equals': equals,
    'element': jqLite,
    'forEach': forEach,
    'injector': createInjector,
    'noop': noop,
    'bind': bind,
    .....
```
### 步骤二
&nbsp;&nbsp;(1)检查是不是多次导入Angular:window.angular.bootstrap(通过检查指定的元素上是否已经存在injector进行判断)  
```js
   if (window.angular.bootstrap) {
    //AngularJS is already loaded, so we can return here...
    console.log('WARNING: Tried to load angular more than once.');
    return;
  }
```  
#### Angular的三种启动方式
##### 方式一:自动启动  
angular会自动找到ng-app,将它作为启动点，自动启动  
  
```html  
  <!DOCTYPE html>
  <html ng-app="myModule">
  <head>
    <title>New Page</title>
    <meta charset="utf-8" />
    <script type="text/javascript" src="../../vendor/bower_components/angular/angular.min.js">
    </script>
    <script type="text/javascript" src="./02.boot1.js"></script>
  </head>
  <body>
    <div ng-controller="MyCtrl">
        <span>{{Name}}</span>
    </div>
  </body>
  </html>  
```   
 
**JS代码**         
angular会自动找到ng-app,将它作为启动点，自动启动    
```js
   var myModule = angular.module("myModule", []);
    myModule.controller('MyCtrl', ['$scope',
    function($scope) {
        $scope.Name = "Puppet";
    }
   ]);
```
##### 方式二:手动启动  
 &nbsp; 在没有ng-app的情况下,只需要在JS中添加一段注册代码即可  
```js  
   <body>
    <div ng-controller="MyCtrl">
        <span>{{Name}}</span>
    </div>
</body>
```  
 
**JS代码**  
```js  
   var myModule = angular.module("myModule", []);
   myModule.controller('MyCtrl', ['$scope',
    function($scope) {
        $scope.Name = "Puppet";
    }
]);
    angular.element(document).ready(function() {
        angular.bootstrap(document, ['myModule']);
});  
```  
##### 方式三:多个ng-app
&nbsp;ng中，angular的ng-app是无法嵌套使用的，在不嵌套的情况下有多个ng-app，他默认只会启动第一个ng-app，第二个第三个需要手动启动(注意，不要手动启动第一个，虽然可以运行，但会抛异常)  
  
```js       
/** 第一个App @type {[type]} */
var myModule1 = angular.module("myModule1", []);
myModule1.controller('MyCtrl', ['$scope',
    function($scope) {
        $scope.Name = "Puppet";
    }
]);
// angular.element(document).ready(function() {
//     angular.bootstrap(app1, ['MyModule1']);
// });
/**
 * 第二个APP
 * @type {[type]}
 */
var myModule2 = angular.module("myModule2", []);
myModule2.controller('MyCtrl', ['$scope',
    function($scope) {
        $scope.Name = "Vincent";
    }
]);
angular.element(document).ready(function() {
    angular.bootstrap(app2, ['myModule2']);
});  
```    
### 步骤三  
尝试绑定jQuery,如果发现导入了jQuery,则使用导入的jQuery,否则，使用Angular自己封装的JQLite  
```js
   bindJQuery();
```    
```js  
   jQuery = window.jQuery;
   if (jQuery && jQuery.fn.on) {
    jqLite = jQuery;
    extend(jQuery.fn, {
      scope: JQLitePrototype.scope,
      isolateScope: JQLitePrototype.isolateScope,
      controller: JQLitePrototype.controller,
      injector: JQLitePrototype.injector,
      inheritedData: JQLitePrototype.inheritedData
    });
```
### 步骤四
#### 发布ng提供的API
publishExternalAPI(angular);
```js
//构建模块加载器
angularModule = setupModuleLoader(window);
  try {
    angularModule('ngLocale');
  } catch (e) {
    angularModule('ngLocale', []).provider('$locale', $LocaleProvider);
  }
```
#### 模块加载器的实现原理
```js  
function setupModuleLoader(window) {
  var $injectorMinErr = minErr('$injector');
  var ngMinErr = minErr('ng');
  function ensure(obj, name, factory){
  return obj[name] || (obj[name] = factory());
  }
  var angular = ensure(window, 'angular', Object);
  // We need to expose `angular.$$minErr` to modules such as `ngResource` that reference it during bootstrap
  angular.$$minErr= angular.$ $minErr || minErr;
  //把module方法放到angular的全局对象上，ensure就是一个属性拷贝的过程
return ensure(angular, 'module', function(){
    //模块缓存
    var modules = {};
}  
```
#### 把工具函数加载到模块里
```js
return function module(name, requires, configFn) {
......
}
```
#### 构建内置模块 ng
publishExternalAPI(angular)
```js
  angularModule('ng', ['ngLocale'], ['$provide',
    function ngModule($provide) {
      // $ $sanitizeUriProvider needs to be before $compileProvider as it is used by it.
      $provide.provider({
        $ $sanitizeUri: $$SanitizeUriProvider
      });
      $provide.provider('$compile', $CompileProvider).
        directive({
            a: htmlAnchorDirective,
            input: inputDirective,
            ......
```

#### 总结  
1.工具函数拷贝到angular全局对象上；  
2.调用setupModuleLoader方法创建模块定义和加载工具（挂在全局对象window.angular上）；  
3.构建内置模块ng；  
4.创建ng内置的directive和provider；  
5.两个重要的provider：$parse 和 $rootScope；  
### 步骤五
#### 初始化Angular-查找ng-app
```js
  jqLite(document).ready(function() {  
    angularInit(document, bootstrap);  
  });  
```
#### bootatrap
创建injector,拉起内核和启动模块，调用compile服务(一个ng-app只有一个injector)  
```js
function bootstrap(element, modules, config){
....
}
```