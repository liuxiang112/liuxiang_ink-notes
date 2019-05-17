### JS 中的require 和 import 区别

在研究react和webpack的时候，经常看到在js文件中出现require，还有import，这两个都是为了JS模块化编程使用。CSS的是@import

1. ES6 模块的设计思想，是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。

Require是CommonJS的语法，CommonJS的模块是对象，输入时必须查找对象属性。
````
// CommonJS模块
let { stat, exists, readFile } = require('fs');

// 等同于
let _fs = require('fs');
let stat = _fs.stat;
let exists = _fs.exists;
let readfile = _fs.readfile;
````

**above**：整体加载fs模块（即加载fs所有方法），生成一个对象"_fs"，然后再从这个对象上读取三个方法，这叫“运行时加载”，因为只有运行时才能得到这个对象，不能在编译时做到静态化。ES6模块不是对象，而是通过export命令显示指定输出代码，再通过import输入。
```
import { stat, exists, readFile } from 'fs';
```
**above**：从fs加载“stat, exists, readFile” 三个方法，其他方法不加载，

2. ES6模块默认使用严格模式，无论是否声明“use strict”，ES6 模块之中，顶层的this指向undefined，即不应该在顶层代码使用this。Module 主要由两个命令组成，import和export，export用于规定模块的对外接口，import命令用于输入其他模块提供的功能。

3. Export

模块是独立的文件，该文件内部的所有的变量外部都无法获取。如果希望获取某个变量，必须通过export输出，
```
// profile.js
export var firstName = 'Michael';
export var lastName = 'Jackson';
export var year = 1958;
```
或者用更好的方式：用大括号指定要输出的一组变量

```
// profile.js
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export {firstName, lastName, year};
```
除了输出变量，还可以输出函数或者类（class），
```
export function multiply(x, y) {
  return x * y;
};
```
还可以批量输出，同样是要包含在大括号里，也可以用as重命名：

```
function v1() { ... }
function v2() { ... }

export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};
```
**Attention**:export 命令规定的是对外接口，必须与模块内部变量建立一一对应的关系
```
// 写法一
export var m = 1;

// 写法二
var m = 1;
export {m};

// 写法三
var n = 1;
export {n as m};


// 报错
export 1;

// 报错
var m = 1;
export m;
```
报错的写法原因是：没有提供对外的接口，第一种直接输出1，第二种虽然有变量m，但还是直接输出1，导致无法解构。同样的，function和class的输出，也必须遵守这样的写法。

```
// 报错
function f() {}
export f;

// 正确
export function f() {};

// 正确
function f() {}
export {f};
```

**And**：export语句输出的接口，都是和其对应的值是动态绑定的关系，即通过该接口取到的都是模块内部实时的值。

位置：export模块可以位于模块中的任何位置，但是必须是在模块顶层，如果在其他作用域内，会报错。
```
function foo() {
  export default 'bar' // SyntaxError
}
foo()
```

**注意**：import和export命令只能在模块的顶层，不能在代码块之中。否则会语法报错。这样的设计，可以提高编译器效率，但是没有办法实现运行时加载。因为require是运行时加载，所以import命令没有办法代替require的动态加载功能。所以引入了import()函数。完成动态加载。