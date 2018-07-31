### 常用的数组方法
#### 一、数组的方法有数组原型方法，也有从object对象继承来的方法，这里我们只介绍数组的原型方法，数组原型方法主要有以下这些：

- join()
- push()和pop()
- shift() 和 unshift()
- sort()
- reverse()
- concat()
- slice()
- splice()
- indexOf()和 lastIndexOf() （ES5新增）
- forEach() （ES5新增）
- map() （ES5新增）
- filter() （ES5新增）
- every() （ES5新增）
- some() （ES5新增）
- reduce()和 reduceRight() （ES5新增）
  
**1、join()**
- join(separator): 将数组的元素组起一个字符串，以separator为分隔符，省略的话则用默认用逗号为分隔符，该方法只接收一个参数：即分隔符。
- <font color="#dd0000">改方法不改变原数组</font>
````
var arr = [1,2,3];
console.log(arr.join()); // 1,2,3
console.log(arr.join("-")); // 1-2-3
console.log(arr); // [1, 2, 3]（原数组不变）
````
**2、push()和pop()**
- push(): 可以接收任意数量的参数，把它们逐个添加到数组末尾，并返回修改后数组的长度。 
- pop()：数组末尾移除最后一项，减少数组的 length 值，然后返回移除的项。
- *<font color="#dd0000">注意：两者都会改变原数组</font>*
````
var arr = ["Lily","lucy","Tom"];
var count = arr.push("Jack","Sean");
console.log(count); // 5
console.log(arr); // ["Lily", "lucy", "Tom", "Jack", "Sean"]
var item = arr.pop();
console.log(item); // Sean
console.log(arr); // ["Lily", "lucy", "Tom", "Jack"]
````
**3、shift() 和 unshift()**
- shift()：删除原数组第一项，并返回删除元素的值；如果数组为空则返回undefined 。 
- unshift:将参数添加到原数组开头，并返回数组的长度 。

*<font color="#dd0000">说明这组方法和上面的push()和pop()方法正好对应，一个是操作数组的开头，一个是操作数组的结尾，且也会改变原数组</font>*
````
var arr = ["Lily","lucy","Tom"];
var count = arr.unshift("Jack","Sean");
console.log(count); // 5
console.log(arr); //["Jack", "Sean", "Lily", "lucy", "Tom"]
var item = arr.shift();
console.log(item); // Jack
console.log(arr); // ["Sean", "Lily", "lucy", "Tom"]
````
**4、sort()**
- sort()：按升序排列数组项——即最小的值位于最前面，最大的值排在最后面。
- 会改变原数组
- *<font color="#dd0000">在排序时，sort()方法会调用每个数组项的 toString()转型方法，然后比较得到的字符串，以确定如何排序。即使数组中的每一项都是数值， sort()方法比较的也是字符串</font>*
````
var arr1 = ["a", "d", "c", "b"];
console.log(arr1.sort()); // ["a", "b", "c", "d"]
arr2 = [13, 24, 51, 3];
console.log(arr2.sort()); // [13, 24, 3, 51]
console.log(arr2); // [13, 24, 3, 51](原数组被改变)
````
为了解决上述问题，sort()方法可以接收一个比较函数作为参数，以便我们指定哪个值位于哪个值的前面。比较函数接收两个参数，如果第一个参数应该位于第二个之前则返回一个负数，如果两个参数相等则返回 0，如果第一个参数应该位于第二个之后则返回一个正数。以下就是一个简单的比较函数：
````
function compare(value1, value2) {
  if (value1 < value2) {
    return -1;
  } else if (value1 > value2) {
    return 1;
  } else {
    return 0;
  }
}
arr2 = [13, 24, 51, 3];
console.log(arr2.sort(compare)); // [3, 13, 24, 51]
````
如果需要通过比较函数产生降序排序的结果，<font color="#dd0000">只要交换比较函数返回的值即可：</font>
````
function compare(value1, value2) {
  if (value1 < value2) {
    return 1;
  } else if (value1 > value2) {
    return -1;
  } else {
    return 0;
  }
}
arr2 = [13, 24, 51, 3];
console.log(arr2.sort(compare)); // [51, 24, 13, 3]
````
**5、reverse()**
- reverse()：反转数组项的顺序。
- <font color="#dd0000">原数组改变</font>
````
var arr = [13, 24, 51, 3];
console.log(arr.reverse()); //[3, 51, 24, 13]
console.log(arr); //[3, 51, 24, 13]
````
**6、concat()**
- concat() ：将参数添加到原数组中。这个方法会先创建当前数组一个副本，然后将接收到的参数添加到这个副本的末尾，最后返回新构建的数组。在没有给 concat()方法传递参数的情况下，它只是复制当前数组并返回副本。
- <font color="#dd0000">不修改原数组</font>
````
var arr = [1,3,5,7];
var arrCopy = arr.concat(9,[11,13]);
console.log(arrCopy); //[1, 3, 5, 7, 9, 11, 13]
console.log(arr); // [1, 3, 5, 7]
````
从上面测试结果可以发现：传入的不是数组，则直接把参数添加到数组后面，如果传入的是数组，则将数组中的各个项添加到数组中。但是如果传入的是一个二维数组呢？
````
var arrCopy2 = arr.concat([9,[11,13]]);
console.log(arrCopy2); //[1, 3, 5, 7, 9, Array[2]]
console.log(arrCopy2[5]); //[11, 13]
````
*注意：上述代码中，arrCopy2数组的第五项是一个包含两项的数组，也就是说concat方法只能将传入数组中的每一项添加到数组中，如果传入数组中有些项是数组，那么也会把这一数组项当作一项添加到arrCopy2中。*