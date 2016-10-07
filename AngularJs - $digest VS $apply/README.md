# AngularJs - $digest VS $apply

JavaScript 模块和构建步骤越来越多而且复杂，但是新框架里的boilerplate是什么呢？

AngularJs 一个最值得赞赏的特性就是双向数据绑定。为了使他工作AngularJs通过循环($digest)评估了model和view之间的改变. 为了进一步了解这个框架是怎么工作的你需要了解这些观念。

当一个事件结束后 Angular 评估每个 watcher。 这就是所谓的$digest 循环。有时候你不得不手动的强制他开始一个新的循环，而且必须选择正确的选项，因为从性能上说这是最有影响的时期。

## $apply

这个核心的方法使你开始明确的消化循环。 这意味着所有的watcher都已经检查过；整个应用开始$digest loop. 在内部，执行了一个可选的函数参数以后，他叫做 $rootScope.$digest();

## $digest

既然$digest 方法为当前范围和他的子集开始 $digest 循环。你应该关注父级范围不会被检查，也不会被影响

## 建议

  * 只有当浏览器DOM事件在AngularJs范围之外被触发时用$apply 和 $digest
  * 向$apply传入一个函数参数，这是一种错误处理机制，允许digest cycle里的集成的变化

```js
  $scope.$apply(() => {
    $scope.tip = 'Javascript Tip';
  });
```
 * 如果你只需要更新当前范围或者他的孩子，用$digest, 并在整个应用层面防止一个新的digest cycle。更高性能是本质。
 * $apply 对机器来说是一个顽固的进程，当有很多绑定的时候会引发性能问题
 * 如果你用的Angular JS 1.2.x 以上，使用 $evalAsync, 一个会在当前循环或者下一个评估表达式的核心方法。这能增强你的应用的性能。
