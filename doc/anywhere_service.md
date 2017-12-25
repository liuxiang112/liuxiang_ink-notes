
## anywhere服务器：随起随用的静态文件服务器
##### 1.安装
```javascript
npm install anywhere -g
```
##### 2.运行（想要以某个路径作为静态文件服务器的根目录分享，只需在该目录下执行）
```javascript
anywhere
```
**注意**: 
1. 默认不添加-s命令，会在命令敲击后，同时打开浏览器访问[http://localhost:8000]()这个路径
2. 命令：anywhere -p 8000 指定静态服务器端口为8000
3. anywhere -s 静默执行，不打开浏览器 
