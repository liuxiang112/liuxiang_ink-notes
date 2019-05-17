### export default 和 export 区别

**一、总结：**
- A.export与export default均可用于导出常量、函数、文件、模块等
- B.你可以在其它文件或模块中通过import+(常量 | 函数 | 文件 | 模块)名的方式，将其导入，以便能够对其进行使用
- C.<font color="#dd000">在一个文件或模块中，export、import可以有多个，export default仅有一个</font>
- D.通过export方式导出，在导入时要加{ }，export default则不需要.
  
**二、例子**

1. export
````
//demo1.js
export const str = 'hello world'
export function f(a){ return a+1}

或者

const str1 = 'hello'
function test1() {}
export { str1, test1 }

或者

const str2 = 'hello2'
function test2() {}
export str2
export test2
````
对应的导入方式：
````
//demo2.js
import { str, f } from 'demo1' //也可以分开写两次，导入的时候带花括号

或者

import { str } from 'demo1'
import { f } from 'demo1'

或者

import * from 'demo1'
````
1. export default
````
//demo1.js
export default const str = 'hello world'

或者

const str1 = 'hello'
export default str1
````
对应的导入方式：
````
//demo2.js
import str from 'demo1' //导入的时候没有花括号

或者

import * from 'demo1'
````
- E.**使用export default命令，为模块指定默认输出，这样就不需要知道所要加载模块的变量名**
  
如下：
````
//a.js
let sex = "boy";
export default sex（sex不能加大括号）
//原本直接export sex外部是无法识别的，加上default就可以了.但是一个文件内最多只能有一个export default。
````
*其实此处相当于为sex变量值"boy"起了一个系统默认的变量名default，自然default只能有一个值，所以一个文件内不能有多个export default。*

本质上，a.js文件的export default输出一个叫做default的变量，然后系统允许你为它取任意名字。所以可以为import的模块起任何变量名，且不需要用大括号包含！！！
````
// b.js
import any from "./a.js"
import any12 from "./a.js" 
console.log(any,any12)   // boy,boy
````
