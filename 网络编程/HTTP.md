# HTTP

提出问题:

    网络协议分层 是什么
    HTTP 是什么?
    HTTP 一次完整的流程是怎么样的?
    GET 和 POST 的区别

## 网络协议分层(经典五层模型)

|层级|作用&功能|具体协议|  
|---|---|---|
|应用层|规定应用程序的数据格式,对接受到的传输层的数据 进行解读 |HTTP FTP|
|传输层|建立"端口到端口"的通信 |TCP UDP|
|网络层|为不同主机提供通信服务(主机到主机)使用ip来解决mac地址效率低的问题|IP|
|数据链路层|确定电信号的分组方式 (数据包)||
|物理层|就是把电脑连接起来的物理手段,(使用光缆,双绞线), 负责传送0和1的电信号.||

## Http

    HTTP（全称超文本传输协议，英文全称HyperText Transfer Protocol）是互联网上应用最为广泛的一种网络协议.

## Http 报文详解

    HTTP 在应用层交互数据的方式 = 报文
    分为: 请求报文 & 响应报文

### 请求报文:

    请求报文由 请求行 . 请求头 & 请求体 组成

    请求行:包含 请求方法 + 请求路径 + 协议版本
    请求头:声明 客户端、服务器 / 报文的部分信息
    请求体:存放 需发送给服务器的数据信息

``` js
GET /barite/account/stock/groups HTTP/1.1
QUARTZ-SESSION: MC4xMDQ0NjA3NTI0Mzc0MjAyNg.VPXuA8rxTghcZlRCfiAwZlAIdCA
DEVICE-TYPE: ANDROID
API-VERSION: 15
Host: shitouji.bluestonehk.com
Connection: Keep-Alive
Accept-Encoding: gzip
User-Agent: okhttp/3.10.0
```

### 响应报文: 

    响应报文由 状态行 . 响应头 & 响应体 组成

    状态行:包含 请求方法 + 请求路径 + 协议版本
    响应头:声明 客户端、服务器 / 报文的部分信息
    响应体:存放 需发送给服务器的数据信息

``` js
HTTP/1.1 200 OK
Server: nginx/1.6.3
Date: Mon, 15 Oct 2018 03:30:28 GMT
Content-Type: application/json;charset=UTF-8
Pragma: no-cache
Cache-Control: no-cache
Expires: Thu, 01 Jan 1970 00:00:00 GMT
Content-Encoding: gzip
Transfer-Encoding: chunked
Proxy-Connection: Keep-alive

{"errno":0,"dialogInfo":null,"body":{"list":[{"flag":2,"group_id":1557,"group_name":"港股","count":1},{"flag":3,"group_id":1558,"group_name":"美股","count":7},{"flag":1,"group_id":1556,"group_name":"全部","count":8}]},"message":"success"}
```

请求头和响应头部分字段

* Host：指定服务器域名，可用来区分访问一个服务器上的不同服务
* Connection：keep-alive表示要求服务器不要关闭TCP连接，close表示明确要求关闭连接，默认值是keep-alive
* Accept-Encoding：说明自己可以接收的压缩方式
* User-Agent：用户代理，是服务器能识别客户端的操作系统（Android、IOS、WEB）及相关的信息。作用是帮助服务器区分客户端，并且针对不同客户端让用户看到不同数据，做不同操作。
* Content-Type：服务器告诉客户端数据的格式，常见的值有text/plain，image/jpeg，image/png，video/mp4，application/json，application/zip。这些数据类型总称为MIME TYPE。
* Content-Encoding：服务器数据压缩方式
* Transfer-Encoding：chunked表示采用分块传输编码，有该字段则无需使用Content-Length字段。
* Content-Length：声明数据的长度，请求和回应头部都可以使用该字段。

## HTTP 工作流程

    1）建立 TCP 连接:
        在HTTP工作开始之前,客户端首先要通过网络与服务器建立连接,该连接是通过 TCP 来完成的,该协议与 IP 协议共同构建 Internet,即著名的 TCP/IP 协议族,因此 Internet 又被称作是 TCP/IP 网络.HTTP 是比 TCP 更高层次的应用层协议,根据规则,只有低层协议建立之后,才能进行高层协议的连接,因此,首先要建立 TCP 连接,一般 TCP 连接的端口号是80;

    2）客户端向服务器发送请求命令:
        一旦建立了TCP连接,客户端就会向服务器发送请求命令;
        例如:GET/sample/hello.jsp HTTP/1.1;

    3）客户端发送请求头信息:
        客户端发送其请求命令之后,还要以头信息的形式向服务器发送一些别的信息,之后客户端发送了一空白行来通知服务器,它已经结束了该头信息的发送;

    4）服务器应答:
        客户端向服务器发出请求后，服务器会客户端返回响应;
        例如: HTTP/1.1 200 OK
        响应的第一部分是协议的版本号和响应状态码;

    5）服务器返回响应头信息:
        正如客户端会随同请求发送关于自身的信息一样,服务器也会随同响应向用户发送关于它自己的数据及被请求的文档;

    6）服务器向客户端发送数据:
        服务器向客户端发送头信息后,它会发送一个空白行来表示头信息的发送到此为结束,接着,它就以 Content-Type 响应头信息所描述的格式发送用户所请求的实际数据;

    7）服务器关闭 TCP 连接:
        一般情况下,一旦服务器向客户端返回了请求数据,它就要关闭 TCP 连接,然后如果客户端或者服务器在其头信息加入了这行代码 Connection:keep-alive,TCP 连接在发送后将仍然保持打开状态,于是,客户端可以继续通过相同的连接发送请求.保持连接节省了为每个请求建立新连接所需的时间,还节约了网络带宽.

## 请求方法

    常见的有GET 和 POST 2种

### GET 和 POST 区别

|区别|GET|POST|
|---|---|---|
|编码|只能进行url编码|支持多种编码格式|
|长度限制|在 URL 中传送的参数是有长度限制的|没有限制|
|参数类型|只接受ASCII字符|没有限制|
|安全性|不安全 参数直接暴露在 URL 上|在请求体中 相对安全|
|参数位置|放置在 URL 中|放在 请求体 中|
|数据包数量|产生一个TCP数据包|产生两个TCP数据包|

    对于GET方式的请求,浏览器会把http header和data一并发送出去,服务器响应200(返回数据);

    对于POST,浏览器先发送header,服务器响应100 continue,浏览器再发送data,服务器响应200 ok(返回数据).

POST先要建立连接才会接着传输数据,这样就避免了服务端突然关闭连接造成的资源消耗