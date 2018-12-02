# 简单的HTTP协议

## 客户端和服务端的通信

一般请求都是有客户端发出的，而服务器回复的是响应。服务器相对来说是比较被动的。

客户端，请求方法 ：GET 、地址： / 、协议版本 ：HTTP1.1 、请求首部：Host:xxx等。服务器返回的信息：HTTP协议版本号，状态码以及结果短语、响应首部（响应时间之后的）和资源主体（html开始）。

## HTTP协议是无状态的

前后分离之后，引入了token机制，为什么要这么做？本质上还是因为HTTP协议是无状态的。所谓无状态，就是通信的双方没有记住通信历史，协议本身是不保留一切请求和响应的信息。这也是确保HTTP协议有能够处理大量事务且具有可伸缩性的原因。

## HTTP 方法

**GET** ：获取资源。

**POST** ：传输实体主体，主要目的是传输。

PUT ： 传输文件，保存到指定的位置

HEAD ： 获得报文首部

DELETE ：删除文件

OPTIONS ：查询支持的方法

TRACE ：追踪路径

CONNECT ：要求使用隧道协议连接代理

## 状态码

| 1**  | 信息，服务器收到请求，需要请求者继续执行操作   |
| ---- | ---------------------------------------------- |
| 2**  | 成功，操作被成功接收并处理                     |
| 3**  | 重定向，需要进一步的操作以完成请求             |
| 4**  | 客户端错误，请求包含语法错误或无法完成请求     |
| 5**  | 服务器错误，服务器在处理请求的过程中发生了错误 |

| 200  | OK                    | 请求成功。一般用于GET与POST请求                              |
| ---- | --------------------- | ------------------------------------------------------------ |
| 301  | Moved Permanently     | 永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替 |
| 302  | Found                 | 临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI |
| 304  | Not Modified          | 未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源 |
| 404  | Not Found             | 服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面 |
| 500  | Internal Server Error | 服务器内部错误，无法完成请求                                 |
| 502  | Bad Gateway           | 作为网关或者代理工作的服务器尝试执行请求时，从远程服务器接收到了一个无效的响应 |

# AJAX

AJAX即“Asynchronous Javascript And XML”（**异步**JavaScript和XML），是指一种创建交互式网页应用的网页开发技术。Ajax不是一种新的编程语言，而是使用现有标准的新方法。**AJAX可以在不重新加载整个页面的情况下，与服务器交换数据。**这种异步交互的方式，使用户单击后，不必刷新页面也能获取新数据。使用Ajax，用户可以创建接近本地桌面应用的直接、高可用、更丰富、更动态的Web用户界面。

利用AJAX可以做：

- 注册时，输入用户名自动检测用户是否已经存在。
- 登陆时，提示用户名密码错误

### 单线程的JavaScript

说起异步，就要先说说JavaScript运行机制。我们知道，JavaScript是单线程执行的，意味着同一个时间点，只有一个任务在运行。单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。如果前一个任务耗时很长，后一个任务就不得不一直等着。

从诞生起，JavaScript就是单线程，这已经成了这门语言的核心特征，将来也不会改变。

### 为什么需要异步？

单线程的好处是实现起来比较简单，执行环境相对单纯；坏处是只要有一个任务耗时很长，后面的任务都必须排队等着，会拖延整个程序的执行。常见的浏览器无响应（假死），往往就是因为某一段Javascript代码长时间运行（比如死循环），导致整个页面卡在这个地方，其他任务无法执行。

为了解决这个问题，Javascript语言将任务的执行模式分成两种：同步（Synchronous）和异步（Asynchronous）。

### JavaScript执行机制

在谈异步之前，先来说说JavaScript的执行机制，看下面这段代码

```
function foo () {
    return foo();
}
foo();
```

stack是一种数据结构，数据先入后出，后入先出。执行JavaScript的call stack，也是如此。

```
var i = 0;
function inc() {
    i++;
    inc();
}
inc();
```

### 异步的实现

webApi，任务队列，event loop

### webApi

1. dom事件
2. 定时器setTimeout,setInterval
3. XMLHttpRequest

**DOM0级事件**

```
element.onclick=function(){
    console.log("clicked");
}
window.onload=function(){
    console.log("loaded");
}
```

**DOM2级事件**

```
element.addEventListener("click",function(){
    console.log("clicked");
});
```

**定时器**

```
console.log(1);
setTimeout(() => {console.log('after 0ms')} ,0);
console.log(2);
console.log(3);
```

### ajax伪造

iframe就是我们常用的iframe标签：<iframe>。iframe标签是框架的一种形式，也比较常用到，iframe一般用来包含别的页面，例如我们可以在我们自己的网站页面加载别人网站或者本站其他页面的内容。iframe标签的最大作用就是让页面变得美观。iframe标签的用法有很多，主要区别在于对iframe标签定义的形式不同，例如定义iframe的长宽高。

因此，iframe标签具有局部加载内容的特性，所以可以使用其来伪造Ajax请求。

```
<!DOCTYPE html>
<html>
    <head lang="en">
        <meta charset="UTF-8">
        <title>伪造AJAX</title>
    </head>
    <body>
        <div>
            <p>请输入要加载的地址：<span id="currentTime"></span></p>
            <p>
                <input id="url" type="text" />
                <input type="button" value="提交" onclick="LoadPage();">
            </p>
        </div>
        <div>
            <h3>加载页面位置：</h3>
            <iframe id="iframePosition" style="width: 100%;height: 500px;"></iframe>
        </div>
        <script type="text/javascript">
            window.onload= function(){
                var myDate = new Date();
                document.getElementById('currentTime').innerText = myDate.getSeconds();
 
            };
            function LoadPage(){
                var targetUrl =  document.getElementById('url').value;
                document.getElementById("iframePosition").src = targetUrl;
            }
        </script>
    </body>
</html>
```

### 原生ajax

 Ajax的核心是XMLHttpRequest对象(XHR)。XHR为向服务器发送请求和解析服务器响应提供了接口。能够以异步方式从服务器获取新数据。

**一、XMLHttpRequest对象**

Ajax的核心是XMLHttpRequest对象(XHR)。XHR为向服务器发送请求和解析服务器响应提供了接口。能够以异步方式从服务器获取新数据。

**XHR的主要方法有：**

1. void open(String method,String url,Boolen async) 用于创建请求 参数： method： 请求方式（字符串类型），如：POST、GET、DELETE... url： 要请求的地址（字符串类型） async： 是否异步（布尔类型）
2. void send(String body)用于发送请求参数： body： 要发送的数据（字符串类型）
3. void setRequestHeader(String header,String value)用于设置请求头参数： header： 请求头的key（字符串类型） vlaue： 请求头的value（字符串类型）
4. String getAllResponseHeaders()获取所有响应头返回值： 响应头数据（字符串类型）
5. String getResponseHeader(String header)获取响应头中指定header的值参数： header： 响应头的key（字符串类型）返回值： 响应头中指定的header对应的值
6. void abort()终止请求

**XHR的主要属性有：**

1. Number readyState状态值（整数），可以确定请求/响应过程的当前活动阶段

   0：未初始化。未调用open()方法1：启动。已经调用open()方法，未调用send()方法2：发送。已经调用send()方法，未接收到响应3：接收。已经接收到部分数据4：完成。已经接收到全部数据，可以在客户端使用

2. Function onreadystatechange 当readyState的值改变时自动触发执行其对应的函数（回调函数）

3. String responseText 作为响应主体被返回的文本（字符串类型）

4. XmlDocument responseXML 服务器返回的数据（Xml对象）

5. Number states 状态码（整数），如：200、404...

6. String statesText 状态文本（字符串），如：OK、NotFound...

### GET

GET用于向服务器查询某些信息：

向url后添加需要访问的数据

如：

```
https://a.com/student?a=1&b=2
这里的?后面的是要发送的数据，用&连接
```

```
xmlhttp.open("GET","xxx",true);
xmlhttp.send();
```

```
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
</head>
<body>


    <h1>Ajax请求</h1>
    <input type="button" onclick="XmlGetRequest();" value="Get发送请求" />
    <script type="text/javascript">


        function GetXHR(){
            var xhr = null;
            if(XMLHttpRequest){
                xhr = new XMLHttpRequest();
            }else{
                xhr = new ActiveXObject("Microsoft.XMLHTTP");
            }
            return xhr;
        }
        function XmlGetRequest(){
            var xhr = GetXHR();
            // 定义回调函数
            xhr.onreadystatechange = function(){
                if(xhr.readyState == 4){
                    // 已经接收到全部响应数据，执行以下操作
                    var data = xhr.responseText;
                    console.log(data);
                }
            };
            xhr.open('get', "xxx", true);
            xhr.send();
        }
    </script>
</body>
</html>
```

### POST

```
xmlhttp.open("POST","xxx",true);
xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
xmlhttp.send("a=1;b=2");
post在send中发送请求数据，和get中的?a=1&b=2对应
```

```
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>POST</title>
</head>
<body>


    <h1>Ajax请求</h1>
    <input type="button" onclick="XmlPostRequest();" value="Post发送请求" />


    <script type="text/javascript">


        function GetXHR(){
            var xhr = null;
            if(XMLHttpRequest){
                xhr = new XMLHttpRequest();
            }else{
                xhr = new ActiveXObject("Microsoft.XMLHTTP");
            }
            return xhr;
        }
        function XmlPostRequest(){
            var xhr = GetXHR();
            // 定义回调函数
            xhr.onreadystatechange = function(){
                if(xhr.readyState == 4){
                    // 已经接收到全部响应数据，执行以下操作
                    var data = xhr.responseText;
                    console.log(data);
                }
            };
            // 指定连接方式和地址
            xhr.open('POST', "xxx", true);
            // 设置请求头
            xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded; charset-UTF-8');
            // 发送请求
            xhr.send('n1=1;n2=2;');
        }
    </script>
</body>
</html>


post
```

### GET和POST的区别

- GET在浏览器回退时是无害的，而POST会再次提交请求。
- GET产生的URL地址可以被Bookmark(书签)，而POST不可以
- GET请求会被浏览器主动cache，而POST不会，除非手动设置。
- GET请求只能进行url编码，而POST支持多种编码方式。
- GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
- GET请求在URL中传送的参数是有长度限制的，而POST么有。
- 对参数的数据类型，GET只接受ASCII字符，而POST没有限制。
- GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。
- GET参数通过URL传递，POST放在Request body中。

### 跨域

 

```
Failed to load file:///C:/Users/admin/Desktop/work/test.json: Cross origin requests are only supported for protocol schemes: http, data, chrome, chrome-extension, https.
```

**为什么会跨域**

协议，域名，端口相同，视为同一个域，一个域内的脚本仅仅具有本域内的权限，可以理解为本域脚本只能读写本域内的资源，而无法访问其它域的资源。这种安全限制称为同源策略。          同源策略保证了资源的隔离。一个网站的脚本只能访问自己的资源，就像操作系统里进程不能访问另一个进程的资源一样，如果没有同源策略，你在网站浏览，跳转其他网页，然后这个网页就可以跨域读取你网站中的信息，这样整个Web世界就无隐私可言了。这就是同源策略的重要性，它限制了这些行为。当然，在同一个域内，客户端脚本可以任意读写同源内的资源，前提是这个资源本身是可读可写的。

   通俗的讲，浏览器有一个很重要的安全机制，即为同源策略：不用域的客户端脚本在无明确授权的情况下不能读取对方资源，跨域也就是不同源。

 这里要提一点，访问本地计算机中的文件，使用的是**file协议**

**怎么解决跨域问题**

- 前端人员使用的一般是JSONP进行跨域。

- 项目中使用nginx反向代理。

  **proxy_pass**

- 修改谷歌浏览器的配置。

- **本地服务器**(canvas)

- **CORS**

 

**使用jsonp解决跨域**

实现原理：<script>  标签是不受同源策略的限制的，它可以载入任意地方的 JavaScript 文件，而并不要求同源。所以 JSONP 的理念就是，我和服务端约定好一个函数名，当我请求文件的时候，服务端返回一段 JavaScript。这段 JavaScript 调用了我们约定好的函数，并且将数据当做参数传入。非常巧合的一点（其实并不是），JSON 的数据格式和 JavaScript 语言里对象的格式正好相同。所以在我们约定的函数里面可以直接使用这个对象

```
<script type="text/javascript">
        var localHandler = function(data){
                alert('我是：' + data.result);
        };
</script>
<script type="text/javascript" src="./test2.js"></script>


localHandler({"result":"我是远程js带来的数据"});
```

```
//PHP jsonp -> 字符拼接，了解即可
$jsonp = $_GET['callback']
$result = $jsonp . '({"xxx":"xx"})'
```

> 
>
> level 1:

 使用get请求此接口[https://api.tumiv.com/v2/cqupt/student](https://api.tumiv.com/v2/cqupt/student?page=2&year=2015)

 要求：

| key  | type   | value       |
| ---- | ------ | ----------- |
| page | string | 1           |
| year | string | 2015(固定） |

- 对page进行更改，访问page=1到page=10，对请求到的数据进行展示，**不要求界面美观**,但是也别太丑

- 数据展示为分页展示，点击下一页后访问，如，第一页为page=1，第二页为page=2，页数标明清楚，

  上一页，下一页为两个button

- 不能使用jq封装的ajax，axios，fetch

> 
>
> level 2:

 自己封装ajax，封装后能直接进行使用，使用方法如下，并使用自己封装的ajax完成上面的level 1

```
ajax({
      type: "",
      url: "",
      data: ,
      success: 
      error:
});
```

 使用fetch对上面接口访问，只需要返回正确的数据即可

**加分项**：可在移动设备上查看，要求界面不崩，可使用rem作为单位(关于rem自行百度)