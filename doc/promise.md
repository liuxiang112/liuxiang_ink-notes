#### Promise 的基础用法
**一、Promise 的含义**

Promise 是异步编程的一种解决方案，比传统的解决方案–回调函数和事件－－更合理和更强大。它由社区最早提出和实现，ES6将其写进了语言标准，统一了语法，原生提供了Promise
所谓Promise ，简单说就是一个容器，里面保存着某个未来才回结束的事件(通常是一个异步操作）的结果。从语法上说，Promise是一个对象，从它可以获取异步操作的消息。 
Promise 对象的状态不受外界影响！！！

三种状态:
- pending：进行中
- fulfilled :已经成功
- rejected 已经失败
- 
*状态改变：Promise对象的状态改变，只有两种可能：*
- 从pending变为fulfilled
- 从pending变为rejected。
  
**这两种情况只要发生，状态就凝固了，不会再变了，这时就称为resolved（已定型）**

**二、基本用法**

ES6规定，Promise对象是一个构造函数，用来生成Promise实例
````
const promise = new Promise(function(resolve, reject) {
    if (/*异步操作成功*/) {
        resolve(value);
    } else{
        reject(error);
    }
})
````
<font color="#dd0000">总结：</font>
- resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去； 
- reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

**Promise 实例生成以后，可以用then 方法分别指定resolved状态和rejected状态的回调函数。**
````
promise.then(function(value) {
  //success
}, function(error) {
  //failure
});
````
**三、例子:**
````
function timeout(ms){
  return new Promise((resolve, reject) => {
    setTimeout(resolve, ms, 'done');
  });
}
timeout(100).then((value) => {
  console.log(value);
});
````
````
let promise = new Promise(function(resolve, reject) {
  console.log('Promise');
  resolve();
});
promise.then(function() {
  console.log('resolved');
});
console.log('Hi!');

//Promise
//Hi!
//resolved
````
//异步加载图片:如下
````
function loadImageAsync(url) {
  return new Promise(function(resolve, reject){
    const image = new Image();
    image.onload = function() {
      resolve(image);
    };
    image.onerror = function(){
      reject(new Error('error');
    };
    image.src = url;
  });
}
````
下面是一个用Promise对象实现的 Ajax 操作的例子。
````
const getJSON = function(url) {
  const promise = new Promise(function(resolve, reject) {
    const handler = function() {
      if (this.readyState !== 4) {
        return;
      }
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };
    const client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();
  });
  return promise;
};

getJSON("/posts.json").then(function(json) {
  console.log('Contents: ' + json);
}, function(error) {
  console.error('出错了', error);
});
````