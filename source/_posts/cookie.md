---
title: 聊一聊 cookie
date: 2019-04-05 17:42:04
categories: JavaScript
tags: 
  - JavaScript
---

### 说明

最近在项目中遇到 **请求后台接口时的身份验证** 问题，需要使用 cookie 存储用户登录信息，便于每次在请求后台接口时，带上 cookie 达到身份的校验。借此机会，恶补了一下 cookie 方面的知识

### cookie 的工作方式

**cookie** 是浏览器提供的功能，它其实是存储在浏览器中的纯文本，浏览器的安装目录下会专门有一个 cookie 文件夹来存放各个域下的设置的 **cookie**

每当发送请求时，浏览器会自动检查是否有相应的 **cookie**, 有则添加到 **request header** 中的 cookie 字段中。

由于 **cookie** 存放的数据每次都会放到 http 请求头中，如果其中的数据没有必要发送给服务器，无疑增加了网络开销；但是这些数据每个请求到需要发送给服务器，那么免去了重复添加请求参数的操作。所以每个请求都需要携带的数据特别适合存放在 **cookie** 中（例如身份认证）

### cookie 的格式

JS 原生的方法 **document.cookie** 能够获取到 **cookie** 。获取到的是一个字符串，这个字符串是有格式的, 由键值对 **key=value** 构成, 键值对之间有一个 **分号** 和一个 **空格** 隔开

### cookie 的属性

#### expires
> expires: 用来设置 **cookie什么时间内有效**。**expires** 是 **cookie** 的失效时间, 格式必须是 **GMT** 格式的时间(可以通过 **new Date().toGMTString()** 或者 **new Date().toUTCString()** 来获得)  
> 如 **expires=Thu, 25 Feb 2016 04:18:00 GMT** 表示 **cookie** 将 在2016年2月25日4:**18分之后失效，对于失效的cookie** 浏览器会清空。如果没有设置该选项，则默认有效期为**session**，即会话 cookie。这种**cookie**在浏览器关闭后就没有了。

#### domain 和 path

> domain: 是域名  若未设置，默认为 设置该cookie的网页所在的域名
> path: 是路径 若未设置，默认为 设置该cookie的网页所在的目录
> 两者加起来限制 cookie 能够被哪些URL访问

#### secure

> 用来设置**cookie**只在确保安全的请求中才会发送。当请求是HTTPS或者其他安全协议时，包含 **secure** 选项的 **cookie** 才能被发送至服务器。  
> 默认 secure 为空，所以默认按情况下，不管是 https 还是 http 请求, cookie 都能被发送到服务端
> 注：在http协议的网页中是无法设置secure类型cookie的。

#### httpOnly

> 这个选项用来设置cookie是否能通过 js 去访问  
> 默认 httpOnly 为空,所以默认情况下 js 可以访问（读取、修改、删除）这个cookie 的
> 这种类型的 cookie 只能通过 服务端来设置  
> 这个选项是 限制客户端去访问cookie 保障安全

### 如何设置 cookie 

#### 服务端设置 cookie

  无论是请求资源文件还是发送一个 ajax 请求，服务端都会返回response。而response-header中有一个叫做 set-cookie，是服务端专门用来设置 cookie 的。

  注意：
  > 一个set-Cookie字段只能设置一个cookie，当你要想设置多个 cookie，需要添加同样多的set-Cookie字段  
  > 服务端可以设置cookie 的所有选项：expires、domain、path、secure、HttpOnly

#### 客户端设置 cookie

  通过 js 来设置 cookie

  ```js
    document.cookie = "name=jack; ";
    document.cookie = "age=12; expires=Thu, 26 Feb 2116 11:50:25 GMT; domain=baidu.com; path=/";
  ```

  下面的写法 无法创建多个 cookie ，只是添加了 "name=jack"，若是想添加多个 cookie 那么需要重复执行document.cookie = "key=name"。如上面所示

  ```js
    document.cookie = "name=jack; age=12; ";
  ```

### 如何修改、删除

#### 修改 cookie

  要想修改一个**cookie**，只需要重新赋值就行，旧的值会被新的值覆盖。但要注意一点，在设置新**cookie**时，**path/domain**这几个选项一定要旧**cookie** 保持一样。否则不会修改旧值，而是添加了一个新的 **cookie**。

#### 删除 cookie

  删除一个cookie 也挺简单，也是重新赋值，只要将这个新cookie的expires 选项设置为一个过去的时间点就行了。但同样要注意，path/domain/这几个选项一定要旧 cookie 保持一样。

### 跨域请求中的 cookie

> 在发同域请求时，浏览器会将cookie自动加在request header中。但大家是否遇到过这样的场景：在发送跨域请求时，cookie并没有自动加在request header中。

解决办法：

> 在跨域请求中，client端必须手动设置xhr.withCredentials=true，且server端也必须允许request能携带认证信息（即response header中包含Access-Control-Allow-Credentials:true），这样浏览器才会自动将cookie加在request header中。  

> 注意： 一旦跨域request能够携带认证信息，server端一定不能将Access-Control-Allow-Origin设置为*，而必须设置为请求页面的域名。
