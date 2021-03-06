#### Callback函数浅谈

**什么是callback**

- 回调函数就是一个通过函数指针调用的函数。如果你把函数的指针（地址）作为参数传递给另一个函数，当这个指针被用为调用它所指向的函数时，我们就说这是回调函数。回调函数不是由该函数的实现方直接调用，而是在特定的事件或条件发生时由另外的一方调用的，用于对该事件或条件进行响应。

这个解释看上去很复杂，于是找到了知乎上一个更好的解释

你到一个商店买东西，刚好你要的东西没有货，于是你在店员那里留下了你的电话，过了几天店里有货了，店员就打了你的电话，然后你接到电话后就到店里去取了货。在这个例子里，你的电话号码就叫回调函数，你把电话留给店员就叫登记回调函数，店里后来有货了叫做触发了回调关联的事件，店员给你打电话叫做调用回调函数，你到店里去取货叫做响应回调事件。回答完毕。

在Javascript中:

**函数A作为参数(函数引用)传递到另一个函数B中，并且这个函数B执行函数A。我们就说函数A叫做回调函数。如果没有名称(函数表达式)，就叫做匿名回调函数。<font color="#f00">实际上，也就是把函数作为参数传递。</font>**

Javscript Callback

把上面那些复杂的解释都丢到垃圾桶里吧~，看看Callback是什么

**一、Callback是什么**

在jQuery中， hide的方法大概是这样子的
````
$(selector).hide(speed, callback)
````
使用的时候
````
$('#element').hide(1000, function() {
  // callback function
})
````
我们只需要在里面写一个简单的函数
````
$('#element').hide(1000, function() {
  console.log('Hide')
})
````
**注意**：在这其中，Callback 函数在当前动画 100% 完成之后执行。然后我们就可以看到真正的现象，当id为element的元素隐藏后，会在console中输出Hide。
就也就意味着:<font color="#f00">Callback实际上是，当一个函数执行完后，现执行的那个函数就是所谓的callback函数。</font>

**二、Callback作用**

正常情况下函数都是按顺序执行的，然而Javascript是一个事件驱动的语言。
````
function hello(){
  console.log('hello')
}
function world(){
  console.log('world')
}
hello()  
world()
````
所以正常情况下都会按顺序执行的，然而当执行hello事件的时间比较长时。
````
function hello(){
  setTimeout( function(){
    console.log( 'hello' )
  }, 1000 )
}
function world(){
  console.log('world')
}
hello()
world()
````
那么这个时候就不是这样的，这时会输出world，再输出hello，故而我们需要callback。

**三、Callback实例**

一个简单地例子如下
````
function add_callback(p1, p2 ,callback) {
  var my_number = p1 + p2
  callback(my_number)
}
add_callback(5, 15, function(num){
console.log("call " + num)
})
````
在例子中我们有一个add_callback的函数，接收三个参数:前两个是要相加的两个参数，第三个参数是回调函数。当函数执行时，返回相加结果，并在控制台中输出'call 20'。