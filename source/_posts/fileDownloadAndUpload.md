---
title: 前端实现文件的下载和上传
date: 2019-04-06 14:03:39
categories: JavaScript
tags: 
  - JavaScript
---

### 前言

最近项目中遇到文件的上传和下载，这部分的知识之前也是比较陌生的，借此机会熟悉了这方面的知识。对此做一个简单的总结。

### 文件下载

> 前端实现文件下载的方式主要是 3 种 **window.open()**、 **form表单** 、**a标签**

#### 方式1 通过 window.open() 

```js
  function windowOpen(){
    // window.open('./static/thumb3.jpg')
    window.open('./static/wendang.doc')
  }
```
具体表现

> img: 打开新网页，显示图片  

> word文档：下载该文件

#### 方式2 通过 form表单提交 

```js
  function formDownload(){
    var form = document.createElement('form')
    form.method = 'get'
    form.style.display = 'none'
    // form.action = './static/thumb3.jpg'
    form.action = './static/wendang.doc'
    document.body.appendChild(form)
    form.submit()
    document.body.removeChild(form)
  }
```

具体表现
> img: form 未设置 **target** 则在当前 url 显示图片  

> word文档：下载该文件

注：以上两种方式若请求的 url 对应的是 img、txt 等格式时，浏览器直接打开相应的文件，而不是下载

#### 方式3 通过 a标签

```js
  function aDownload(){
    var a = document.createElement('a')
    // a.href = './static/wendang.doc'
    a.href = './static/thumb3.jpg'    
    a.download = 'thumb'
    a.click()
  }
```
具体表现
> 未设置 download 属性，和上面两种表现形式一样  
> 设置了 download 属性，图片也是会下载的

### 文件上传

我在项目中使用的是 **type="file"** 的 input 标签，然后使用 formData 传输数据。
示例代码如下：

html 部分

```html
  <form class="layui-form" action="" id="formload">
    <div class="layui-form-item">
      <label class="layui-form-label">文件</label>
      <div class="layui-input-block">
          <input type="file" name="file" id="file" required lay-verify="required" 
              autocomplete="off">
      </div>
    </div>
  </form>  
```

js 部分 文件上传

```js
  var formData = new FormData($('#formload')[0])
  $.ajax({
    url: api + '/web/file/uploadFile',
    type: 'post',
    cache: false,
    data: formData,
    processData: false,
    contentType: false,
    error: function(){
      // 失败回调
    },
    success: function(res){
      // 成功回调
    }
  })
```
> 文件对象的属性：lastModified、type、name 和 size，可根据需求自定义检查文档格式  

> 文件对象 file 可通过 elem.files[0] 或者 elem.files.items[0] 获得。
其中 elem 为 type="file" 的 input 对象

检查文档的代码

```js
  function checkoutFile(file){
    const accept = ['.doc', '.docx']
    const index = file.name.lastIndexOf('.')
    const flag = true
    // 文件格式不支持
    if(index < 0 || accept.indexOf(file.name.substr(index).toLowerCase() < 0) {
      flag = false 
    }
    // 文件太大
    if(file.size > 2 * 1024 * 1024) {
      flag = false
    }
    return true
  }
```

### 总结

> 前面提到的 下载 给定的都是文件地址，但项目中往往为了安全，后台返回的是 文件流。  

> 针对这种情况：  

> 之前的 form表单提交方式同样适用（我在项目中用过）  
> 这种方式有一个缺点，拿不到后端处理这个过程的时机，无法根据回调函数做交互以及进度提示  

> 还有一个是 html5 提供的新方式 [借助 blob 实现文本信息的下载](https://juejin.im/post/5bd1b0aa6fb9a05d2c43f004)。感兴趣的可自行查阅 
 
