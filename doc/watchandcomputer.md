### computed和watch的使用场景

从作用机制和性质上看待methods,watch和computed的关系

一、首先要说，methods,watch和computed都是以函数为基础的，但各自却都不同 
而从作用机制和性质上看,methods和watch/computed不太一样，所以我接下来的介绍主要有两个对比： 
- 1.methods和(watch/computed)的对比 
- 2.watch和computed的对比 
  
二、作用机制上 
- 1.watch和computed都是以Vue的依赖追踪机制为基础的，它们都试图处理这样一件事情：当某一个数据（称它为依赖数据）发生变化的时候，所有依赖这个数据的“相关”数据“自动”发生变化，也就是**自动调用**相关的函数去实现数据的变动。 
- 2.methods:methods里面是用来定义函数的，很显然，它需要**手动调用**才能执行。而不像watch和computed那样，“自动执行”预先定义的函数 
- 
**<font color="#dd0000">【总结】</font>**：methods里面定义的函数，是需要主动调用的，而和watch和computed相关的函数，会自动调用,完成我们希望完成的作用 

三、从性质上看 
- 1.methods里面定义的是函数，你显然需要像”fuc()”这样去调用它（假设函数为fuc） 
- 2.<font color="#dd0000">computed是计算属性，事实上和和data对象里的数据属性是同一类的（使用上）</font>, 
例如：
````
computed:{
  fullName: function () { return this.firstName + this.lastName }
}
````
**你在取的时候，用this.fullName去取用，就和取data一样（不要当成函数调用！！）**
- 3.watch:类似于监听机制+事件机制： 
例如：
````
watch: {
    firstName: function (val) { this.fullName = val + this.lastName }
}
````
firstName的改变是这个特殊“事件”被触发的条件，而firstName对应的函数就相当于监听到事件发生后执行的方法 

**watch和computed的对比**

**<font color="#dd0000">【总结】</font>**：首先它们都是以Vue的依赖追踪机制为基础的，*它们的共同点是：都是希望在依赖数据发生改变的时候，被依赖的数据根据预先定义好的函数，发生“自动”的变化*。我们当然可以自己写代码完成这一切，但却很可能造成写法混乱，代码冗余的情况，Vue为我们提供了这样一个方便的接口，统一规则 
*但watch和computed也有明显不同的地方：watch和computed各自处理的数据关系场景不同*

- 1.watch擅长处理的场景：一个数据影响多个数据（监听一个数据的变化做一些操作）
- 2.computed擅长处理的场景：一个数据受多个数据影响（通过某些数据的变化，定义一个数据）