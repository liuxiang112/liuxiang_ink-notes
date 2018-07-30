### 函数节流和去抖
#### 一、前言
以下场景往往由于事件频繁被触发，因而频繁执行DOM操作、资源加载等重行为，导致UI停顿甚至浏览器崩溃。

  1. window对象的resize、scroll事件
  2. 拖拽时的mousemove事件
  3. 射击游戏中的mousedown、keydown事件
  4. 文字输入、自动完成的keyup事件
  5. 点击事件，频繁请求

  实际上对于window的resize事件，实际需求大多为停止改变大小n毫秒后执行后续处理；而其他事件大多的需求是以一定的频率执行后续处理。针对这两种需求就出现了debounce和throttle两种解决办法
  #### 二、什么是debounce
  1.定义
      
      如果用手指一直按住一个弹簧，它将不会弹起直到你松手为止。也就是说当调用动作n毫秒后，才会执行该动作，若在这n毫秒内又调用此动作则将重新计算执行时间。      
**接口定义**
````
/**
* 空闲控制 返回函数连续调用时，空闲时间必须大于或等于 delay，method 才会执行
* @param delay   {number}    空闲时间，单位毫秒
* @param method {function}  请求关联函数，实际应用需要调用的函数
* @return {function}    返回客户调用函数
*/
debounce(method, delay)
````
2.简单实现
````
function debounce (method, delay) {
  let timer = null
  return function () {
    let that = this
    let args = arguments
    clearTimeout(timer)
    timer = setTimeout(function () {
      method.apply(that, args)
    }, delay)
  }
}
````
*简单的说，去抖就是当频繁的调用函数的时候，清除定时器，在函数不被调用的时候触发事件*
#### 三、什么是throttle
1.定义
      
      如果将水龙头拧紧直到水是以水滴的形式流出，那你会发现每隔一段时间，就会有一滴水流出。也就是会说预先设定一个执行周期，当调用动作的时刻大于等于执行周期则执行该动作，然后进入下一个新周期。
**接口定义**
````
/**
* 频率控制 返回函数连续调用时，action 执行频率限定为 次 / delay
* @param delay  {number}    延迟时间，单位毫秒
* @param method {function}  请求关联函数，实际应用需要调用的函数
* @return {function}    返回客户调用函数
*/
throttle(delay,action)
````
简单实现
````
function throttle (method, delay) {
  let starttime = new Date()
  return function () {
    let currenttime = new Date()
    if (currenttime - starttime >= delay){
      method.apply(this, arguments)
      starttime = currenttime 
    }
  }
}
````
*简单的说，节流就是按照某个频率一直触发事件（函数预先设定一个执行周期，当调用动作的时刻大于等于执行周期则执行该动作，然后进入下一个新周期）*