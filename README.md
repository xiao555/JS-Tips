<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [JS-Tips](#js-tips)
  - [在数组中插入项 Insert item inside an Array](#%E5%9C%A8%E6%95%B0%E7%BB%84%E4%B8%AD%E6%8F%92%E5%85%A5%E9%A1%B9-insert-item-inside-an-array)
    - [在一个已经存在的数组中添加一项](#%E5%9C%A8%E4%B8%80%E4%B8%AA%E5%B7%B2%E7%BB%8F%E5%AD%98%E5%9C%A8%E7%9A%84%E6%95%B0%E7%BB%84%E4%B8%AD%E6%B7%BB%E5%8A%A0%E4%B8%80%E9%A1%B9)
    - [在尾部添加元素](#%E5%9C%A8%E5%B0%BE%E9%83%A8%E6%B7%BB%E5%8A%A0%E5%85%83%E7%B4%A0)
      - [在移动端的表现](#%E5%9C%A8%E7%A7%BB%E5%8A%A8%E7%AB%AF%E7%9A%84%E8%A1%A8%E7%8E%B0)
        - [Android(v4.2.2)](#androidv422)
        - [Chrome Mobile(v33.0.0)](#chrome-mobilev3300)
        - [Safari Mobile(v9)](#safari-mobilev9)
        - [最终胜利者](#%E6%9C%80%E7%BB%88%E8%83%9C%E5%88%A9%E8%80%85)
    - [在头部添加元素](#%E5%9C%A8%E5%A4%B4%E9%83%A8%E6%B7%BB%E5%8A%A0%E5%85%83%E7%B4%A0)
      - [在移动端的表现](#%E5%9C%A8%E7%A7%BB%E5%8A%A8%E7%AB%AF%E7%9A%84%E8%A1%A8%E7%8E%B0-1)
        - [Android(v4.2.2)](#androidv422-1)
        - [Chrome Mobile(v33.0.0)](#chrome-mobilev3300-1)
        - [Safari Mobile(v9)](#safari-mobilev9-1)
        - [最终胜利者](#%E6%9C%80%E7%BB%88%E8%83%9C%E5%88%A9%E8%80%85-1)
      - [在桌面的表现](#%E5%9C%A8%E6%A1%8C%E9%9D%A2%E7%9A%84%E8%A1%A8%E7%8E%B0)
        - [Chrome (v48.0.2564)](#chrome-v4802564)
        - [Firefox (v44)](#firefox-v44)
        - [IE (v11)](#ie-v11)
        - [Opera (v35.0.2066.68)](#opera-v350206668)
        - [Safari (v9.0.3)](#safari-v903)
    - [在中间添加元素](#%E5%9C%A8%E4%B8%AD%E9%97%B4%E6%B7%BB%E5%8A%A0%E5%85%83%E7%B4%A0)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# JS-Tips
A JS tip per day！ --- translate from http://www.jstips.co/

## 在数组中插入项 Insert item inside an Array

在一个已经存在的数组中插入一项是一个日常任务。你可以用push向数组的尾部添加元素，用unshift添加到头部，或者用splice添加到中间。

### 在一个已经存在的数组中添加一项

一个已经存在的数组中插入一项是一个日常任务。你可以用push向数组的尾部添加元素，用unshift添加到头部，或者用splice添加到中间。
这些是众所周知的方法，但是这并不意味着没有一个更高性能的方案。我们现在开始讲一讲：

### 在尾部添加元素

用push()可以轻松的在数组尾部添加元素，但是可以用另一种方法实现。

```js
var arr = [1,2,3,4,5];
var arr2 = [];

arr.push(6);
arr[arr.length] = 6;
arr2 = arr.concat([6]); //concat() 方法用于连接两个或多个数组。
```
两种方法都像第一种一样修改了原始的数组。不相信我吗？点击链接[jsfiddle](https://jsfiddle.net/xiao555/79abzm6t/)

#### 在移动端的表现

##### Android(v4.2.2)
  1. `arr.push(6);` and `arr[arr.length] = 6;` 性能相同 // 3 319 694 ops/sec
  2. `arr2 = arr.concat([6]);` 比另外两种慢一些

##### Chrome Mobile(v33.0.0)
  1. `arr[arr.length] = 6;` // 6 125 975 ops/sec
  2. `arr.push(6);` 慢66.74 %
  3. `arr2 = arr.concat([6]);` 慢87.63%

##### Safari Mobile(v9)
  1. `arr[arr.length] = 6;` // 7 452 898 ops/sec
  2. `arr.push(6);` 慢 40.19 %
  3. `arr2 = arr.concat([6]);` 慢 49.78%

##### 最终胜利者

  1. arr[arr.length] = 6; // with an average of 42 345 449 ops/sec
  2. arr.push(6); // 34.66 % slower
  3. arr2 = arr.concat([6]); // 85.79 % slower

### 在头部添加元素

现在如果我们想在数组头部添加一项

```js
var arr = [1,2,3,4,5];

arr.unshift(0);
[0].concat(arr);
```
这里有个小细节: unshift 编辑了原始的数组, concat 返回了一个新的数组。[jsfiddle](https://jsfiddle.net/xiao555/wq7fvbm9/)

#### 在移动端的表现

##### Android(v4.2.2)
  1. `[0].concat(arr);` // 1 808 717 ops/sec
  2. `arr.unshift(0);` 97.85 % slower

##### Chrome Mobile(v33.0.0)
  1. `[0].concat(arr);` // 1 269 498 ops/sec
  2. `arr.unshift(0);` 99.86 % slower

##### Safari Mobile(v9)
  1. `arr.unshift(0);` // 3 250 184 ops/sec
  2. `[0].concat(arr);` 33.67 % slower

##### 最终胜利者

  1. [0].concat(arr); // with an average of 4 972 622 ops/sec
  2. arr.unshift(0); // 64.70 % slower

#### 在桌面的表现

##### Chrome (v48.0.2564)
1. [0].concat(arr); // 2 656 685 ops/sec
2. arr.unshift(0); 96.77 % slower

##### Firefox (v44)
1. [0].concat(arr); // 8 039 759 ops/sec
2. arr.unshift(0); 99.72 % slower

##### IE (v11)
1. [0].concat(arr); // 3 604 226 ops/sec
2. arr.unshift(0); 98.31 % slower

##### Opera (v35.0.2066.68)
1. [0].concat(arr); // 4 102 128 ops/sec
2. arr.unshift(0); 97.44 % slower

##### Safari (v9.0.3)
1. arr.unshift(0); // 12 356 477 ops/sec
2. [0].concat(arr); 15.17 % slower

### 在中间添加元素

用splice很容易在数组中间添加项 ，这是性能最好的方法

```js
var items = ['one', 'two', 'three', 'four'];
items.splice(items.length / 2, 0, 'hello'); //arrayObject.splice(index,howmany,item1,.....,itemX)
// index 必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置。
// howmany 必需。要删除的项目数量。如果设置为 0，则不会删除项目。
// item1, ..., itemX 可选。向数组添加的新项目。
```
我尝试在不同的浏览器和操作系统上运行这个测试，我希望这些tips对你们有用，激励你执行自己的测试！



