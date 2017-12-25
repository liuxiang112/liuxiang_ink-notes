## koa: 下一代web应用
### [官方文档](https://koa.bootcss.com/)

### 我的理解
在nodejs上做web应用开发，一切的一切都从

```http.createServer(handle).listen(80)```

这行代码开始，其中handle是一个事件解析函数，```handle(req, res)```，req就是来自客户端的请求(不仅仅是浏览器，也可能是服务器或者任何能发出HTTP请求的实体), res就是由node返回给请求的响应。整个基于HTTP的服务，搭建起了node的web应用

#### 从express说起