## koa: 下一代web应用
### [官方文档](https://koa.bootcss.com/)

### 我的理解
在nodejs上做web应用开发，一切的一切都从

```http.createServer(handle).listen(80)```

这行代码开始，其中handle是一个事件解析函数，```handle(req, res)```，req就是来自客户端的请求(不仅仅是浏览器，也可能是服务器或者任何能发出HTTP请求的实体), res就是由node返回给请求的响应。整个基于HTTP的服务，搭建起了node的web应用

#### 从express说起
```javascript
const express = require('express');
const app = express();
app.get('/', (req, res) => {
  console.log(req.header);
  res.json()
})
```
通过一个函数，入参为```req, res```，调用node的方法对这个请求做出一些处理.通过express提供的```res.json(), res.send(), res.render()```等方法结束当前请求。乍一看挺美好的,但是我们知道node是基于异步的，如果需要返回的信息存在多次异步操作，代码就会变成这样

```javascript
app.use('/', (req, res) => {
  mysql.query(req.body, (err, result) => {
    mysql.query(result.query, (err, result2) => {
      mysql.query(result2.query, (err, result3) => {

        .......

        mysql.query(resultN.query, (err,, resultN) => {
          res.json(resultN);
        })

        .......

      })
    })
  })
})
```
这样就形成复杂的嵌套回调，如果中间还涉及到其他的操作，则会使代码的逻辑更加复杂。当然express同样可以使用```async```的写法

```javascript
app.use('/', async(req, res) => {
  let result = await mysql.query(req.body); // await只接收Promise对象，这里假设已处理
  res.json(result);
})
```
并且我们知道，无论express和koa都是基于中间件的模式，对于express来说，中间件属于这种模型

```[middleware1, middleware2, middleware2, ..., middleware3]```

一个请求会经过多个中间件的处理，有一些中间件是全局的，会加工所有的请求。默认的情况下，express的中间件逻辑是线型的，就是说一个请求经过```middleware1```以后便不会再被这个中间件处理，会执行之后中间件，直到被express终结然后返回。当然，koa出来以后，经过改造，express本身也可以实现洋葱圈模型
```javascript
app.use(async (req, res, next) => {
  console.log('进入路由前');
  await next();
  console.log('进入路由后');
})

app.get('/', async(req, res) => {
  console.log('进入路由');
  res.send('Hello world');
})
```
#### 进入koa
koa的出现说白了就是为了解决express的回调地狱问题。koa也分为两个版本，koa1通过Generator函数+yield语句+Promise 解决异步的流程问题，这种方式不易于理解。koa2通过async/await+Promise解决异步流程问题，async/await说白了就是Generator的语法糖。把原来需要手动通过Generator函数生成的next对象，然后调用的模式改为了自动处理。

koa对于中间件采取的是洋葱模型
![洋葱模型](https://camo.githubusercontent.com/d80cf3b511ef4898bcde9a464de491fa15a50d06/68747470733a2f2f7261772e6769746875622e636f6d2f66656e676d6b322f6b6f612d67756964652f6d61737465722f6f6e696f6e2e706e67)

即koa中没有类似express中能够终止请求的方法。一个请求在koa中会穿越一个中间件两次，一次进入，一次出来。当判断没有中间件的时候这次请求就结束了。看一看之前express中的代码，在koa中的写法
```javascript
app.use(async (ctx, next) => {
  console.log('进入路由前');
  await next();
  console.log('进入路由后');
})

app.get('/', async ctx => {
  console.log('进入路由');
  ctx.body = await mysql.query(param);
})
```
koa中入参没有了```req, res```，取而代之的是```ctx, next```，```ctx```是上下文对象，所谓的上下文就是koa把request, response请求封装到了ctx中，就是说 ctx.request 等价于 express 中的req, ctx.response 等价于 express 中的 res,但没有结束请求的相关方法。ctx本身也代理了他们的一些属性，比如ctx.header 等价于 ctx.request.header。值得注意的是，进入koa的请request的一些属性也会变成response的一些属性，比如```ctx.header```，也会变为response的header，通过```let header = ctx.header```进行读操作，实际取的是request中的值，```ctx.header['token'] = 'Sunshine'```,实际是对response的响应头赋值。

其中```next```是一个回调方法，表示跳转到下一个中间件。在koa中并不一定会指向所有的中间件，koa会把请求经过的中间件以一个栈存储起来，检测到其中一个中间件没有调用```next```的时候，就开始出栈，知道栈为空以后就结束了此次请求。这就是所谓的洋葱圈模型。

以上说的是KOA2的版本，KOA1因为使用generator，所以没有ctx上下文对象，取而代之的this。我是觉得这样很怪异。所以要用就用最新的吧

#### 附上一个快速开箱的KOA框架实例
[koa-for-rest](https://github.com/hstarorg/koa-for-rest)