### 简单了解下纯函数
#### 一、why纯函数(Pure Functions)
当我们的程序变得庞大的时候, 将不可避免地引发一些bugs。我们不能保证杜绝bug产生, 但是我们可以通过某些编程方式来减少一些错误的发生。
纯函数就是其中一种,它也是函数式编程中一部分。那它为什么可以起到减少bug的作用呢, 原因就在于能被称之为纯函数而制定的一些原则，我们来简单看下

#### 二、3个原则：
变量都只在函数作用域内获取, 作为的函数的参数传入
不会产生副作用(side effects), 不会改变被传入的数据或者其他数据
相同的输入保证相同的输出(same input -> same ouput)
比如以下这个pureAdd函数就是纯函数, x和y都是函数参数,处在函数作用域内
````
function pureAdd(x, y) {
  return x + y;
}
````
但是一下就是这个impureAdd就是不是
````
let x = 1;
function impureAdd(y) {
  return x + y;
}
````
因为函数内需要的x需要从函数外部的去获取, 这样的也就导致了函数的相同的输入不能保证有相同的输出, 如下
````
let x = 1;
console.log(impureAdd(3)) // 4
let x = 2;
console.log(impureAdd(3)) // 5
````
还有纯函数不得改变传入的值, 比较容易出错的就是引用类型作为参数传入
````
function mutateObject(obj) {
  obj['newkey'] = 'newValue';
}
var o = {};
mutateObject(o);
console.log(o) //{newkey: "newValue"}
````
还有使用一些mutator methods如数组的push,shift,splice等等
````
function firstThree(arr) {
  return arr.splice(0,3);
}
````
#### 三、<font color='red'>纯函数的一些优点</font>

**1.1容易可测试(testable)**：
因为相同的输入必定是相同的输出，因此结果可以缓存(cacheable)

**1.2**自我记录(Self documenting)：
因为需要的变量都是参数，参数命名良好的情况下即便很久以后再去看这个函数依旧可以很容易知道这个函数需要哪些参数

**1.3**因为不用担心有副作用(side-effects),因此可以更好地工作
