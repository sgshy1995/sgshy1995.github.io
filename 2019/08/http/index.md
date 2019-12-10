# HTTP的请求与响应以及使用Chrome的查看方式


HTTP 的作用就是指导浏览器和服务器如何进行沟通。

今天，我们就HTTP的请求与响应，做出简短的介绍。
## HTTP 请求 

这里使用 curl 命令来实现请求。

请求示例1：

```txt
curl -s -v -H "TEST: test" -- "https://www.baidu.com"
```

其中`-H "TEST: test"`可以删除。
我们可以看一下请求结果。

![请求](/images/http/1.png)

请求示例2：

```txt
curl -X POST -d "1234567890" -s -v -H "Test: test" -- "https://www.baidu.com"
```

该请求可以将 '1234567890' 字符串请求上传至服务器。可以看一下请求结果。

![请求](/images/http/2.png)

以第一个命令为例，请求的内容为：(都只截取了其中以>开头的请求内容)

```
GET / HTTP/1.1
Host: www.baidu.com
User-Agent: curl/7.54.0
Accept: */*
TEST: test
```

可以看出请求的格式为：
```
1 动词 路径 协议/版本
2 Key1: value1
2 Key2: value2
2 Key3: value3
2 Content-Type: application/x-www-form-urlencoded
2 Host: www.baidu.com
2 User-Agent: curl/7.54.0
3 
4 要上传的数据
```
请求最多包含四部分，最少包含三部分。（也就是说第四部分可以为空）
**第三部分永远都是一个回车（\\n）!**

动词有 **GET、POST、PUT、PATCH、DELETE、HEAD、OPTIONS** 等，
这里的路径包括「查询参数」，但不包括「锚点」。
如果你没有写路径，那么路径默认为 "/"。
第 2 部分中的 Content-Type 标注了第 4 部分的格式。

## 用Chrome开发者工具查看 HTTP 请求内容

打开 Network，在地址栏输入网址。查看「request」，点击「view source」
可以看到请求的前三部分。
如果有请求的第四部分，那么在「FormData」或「Payload」里面可以看到。

![请求](/images/http/3.png)

## HTTP 响应

以上面两个请求为示例，我们截取得到的响应 (以<开头)。

第一个：

![请求](/images/http/4.jpeg)

他的响应内容是：

```
HTTP/1.1 200 OK
Accept-Ranges: bytes
Cache-Control: private, no-cache, no-store, proxy-revalidate, no-transform
Connection: Keep-Alive
Content-Length: 2443
Content-Type: text/html
Date: Wed, 05 Dec 2018 12:10:46 GMT
Etag: "58860429-98b"
Last-Modified: Mon, 23 Jan 2017 13:24:57 GMT
Pragma: no-cache
Server: bfe/1.0.8.18
Set-Cookie: BDORZ=27315; max-age=86400; domain=.baidu.com; path=/
    
<!DOCTYPE html> ... 省略
```

第二个：

![请求](/images/http/5.jpeg)

他的响应内容是：

```
HTTP/1.1 302 Found
Connection: Keep-Alive
Content-Length: 17931
Content-Type: text/html
Date: Wed, 05 Dec 2018 12:42:04 GMT
Etag: "54d9748e-460b"
Server: bfe/1.0.8.18
    
<html> ... 省略
```

可以看出响应的格式为：

```
1 协议/版本号 状态码 状态解释
2 Key1: value1
2 Key2: value2
2 Content-Length: 17931
2 Content-Type: text/html
3
4 要下载的内容
```

状态码是服务器对浏览器说的话，可以查阅或记忆。状态解释没什么用。

第 2 部分中的 Content-Type 标注了第 4 部分的格式。

第 2 部分中的 Content-Type 遵循 MIME 规范。
