## Nginx-proxy 实现代理跨域
作为一个纯前端，以前经常遇到因为浏览器同源策略（必须得相同的协议，相同的域名，相同的端口号）所产生的跨域问题，错误如图![](../Iamge/err.png)。讲真，对于一个前端菜鸟来讲，感觉真的像一道过不去的坎儿，只能跪舔后台，让他们解决。直到遇到nginx了，才让我有了尊严，自己就能完美解决。好了，言归正传，回归正题。
### 1.nginx是什么呢？
 nginx是一个高性能的web服务器，常用作反向代理服务器。。。(用途太多，这里只讲其解决跨域问题的能力，so省略1w字)，
 ### 2.nginx解决跨域的原理
 nginx作为反向代理服务器，就是把http请求转发到另一个或者一些服务器上。通过把本地一个url前缀映射到要跨域访问的web服务器上，就可以实现跨域访问。对于浏览器来说，访问的就是同源服务器上的一个url。而nginx通过检测url前缀，把http请求转发到后面真实的物理服务器。并通过rewrite命令把前缀再去掉。这样真实的服务器就可以正确处理请求，并且并不知道这个请求是来自代理服务器的。
 简单说，nginx服务器欺骗了浏览器，让它认为这是同源调用，从而解决了浏览器的跨域问题。又通过重写url，欺骗了真实的服务器，让它以为这个http请求是直接来自与用户浏览器的。
 ### 3.为什么选nginx反向代理服务器来解决跨域问题呢？
 用nginx反向代理实现跨域，只需要修改nginx的配置即可解决跨域问题，支持所有浏览器，支持session，不需要修改任何代码，并且不会影响服务器性能。就是这么简单、强大、高效!
 ### 4.具体操作
#### a.首先找到nginx配置文件nginx.conf
```javascript
server {
  listen 8080; #监听8080端口，可以改成其他端口
  server_name localhost; #当前服务的域名

  #charset koi8-r;

  #access_log logs/host.access.log main;

  location / {
    root html;
    index index.html index.htm
  }
}
```
如果www.a.com访问www.b.com
```javascript
location /api {
  proxy_pass http://www.b.com #真实要访问的服务器
}
```