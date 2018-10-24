### 原生ajax
**一、原理**
1. 创建XMLHttpRequest异步对象；
2. 使用open（）方法设置和服务器的交互信息；
3. 设置发送的数据，开始和服务器交互；
4. 注册事件；
5. 更新视图。
6. 
**二、实现**

get请求
````
function ajax (url) {
    let xml
    let result
    if (window.XMLHttpRequest) {
        xml = new XMLHttpRequest()
    } else {
        xml = new ActiveXObject('Microsoft.XMLHTTP')
    }
    xml.open('GET', url, true)
    xml.send()
    xml.onreadystatechange = function () {
        if (xml.readyState == 4 && xml.status == 200) {
            result = xml.responseText
        }
    }
    return result
}
````

psot请求：必须使用色图Request Header()设置请求头，且在send()分分钟设置要发送的数据
````
xml.open('POST', url, true)
xml.setRequestHeader("Content-type","application/x-www-form-urlencoded")
xml.send('name=Linda&age=23')
````
为了方便使用,我们可以把他封装进方法里面,要用的时候,直接调用就好了
````
function ajax (method, data, url, success) {
    let xml
    if (window.XMLHttpRequest) {
        xml = new XMLHttpRequest()
    } else {
        xml = new ActiveXObject('Microsoft.XMLHTTP)
    }
    if (method === 'GET') { // get请求
        if (data) {
            url += '?'
            url += data
        }
        xml.open(method, url, true)
        xml.send()
    } else { // post请求
        xml.open(method, url, true)
        xml.setRequestHeader("Content-type", "application/x-www-form-urlencoded")
        if (data) {
            xml.send(data)
        } else {
            xml.send()
        }
    }
    xml.onreadychange = function () {
        if (xml.readyState == 4 && xml.status == 200) {
            // console.log(ajax.responseText)

            // 将数据让外面可以使用
            // return xml.responseText

            // 当 onreadystatechange 调用时说明数据回来了
            // xml.responseText

            // 如果说外面可以传入一个 function作为参数 success
            success(xml.responseText)
        }
    }
}
````

**三、笔记**

xmlhttp.readyState的值及解释：

1. 0：请求未初始化（还没有调用 open()）。
2. 1：请求已经建立，但是还没有发送（还没有调用 send()）。
3. 2：请求已发送，正在处理中（通常现在可以从响应中获取内容头）。
4. 3：请求在处理中；通常响应中已有部分数据可用了，但是服务器还没有完成响应的生成。
5. 4：响应已完成；您可以获取并使用服务器的响应了。

xmlhttp.status的值及解释：
1. 1xx:信息响应类，表示接收到请求并且继续处理
2. 2xx:处理成功响应类，表示动作被成功接收、理解和接受
3. 3xx:重定向响应类，为了完成指定的动作，必须接受进一步处理
4. 4xx:客户端错误，客户请求包含语法错误或者是不能正确执行
5. 5xx:服务端错误，服务器不能正确执行一个正确的请求

xml.readyState==4 && xml.status==200的解释：请求完成并且成功返回
