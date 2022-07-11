AJAX 传 MutipartFile(文件)

使用FormData对象，是可以使用一系列的键值对来模拟一个完整的表单，然后使用XMLHttpRequest发送这个"表单"。

html:

```html
<form id="upladForm" enctype="multipart/form-data">
    <input id="file" type="file" name="file"/>
    <button id="upload" type="button">upload</button>
</form>
```

```javascript
$.ajax({
    url: "/upload",
    type: 'POST',
    cache: false,
    data: new FormData($("#uploadForm")[0]),
    processData: false,
    contentType: false,
    success: function (result) {
    },
    error: function (err) {
    }
});
```

这里要注意几点：
1.processData设置为false。因为data值是FormData对象，不需要对数据做处理。

2.form标签添加enctype="multipart/form-data"属性。
3.cache设置为false，上传文件不需要缓存。
4.contentType设置为false。因为是由表单构造的FormData对象，且已经声明了属性enctype="multipart/form-data"，所以这里设置为false。

5.不用AJAX的话直接用input type="submit"上传即可

依赖包：

```xml
<dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
            <version>2.6</version>
</dependency>

<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
<dependency>
            <groupId>commons-fileupload</groupId>
            <artifactId>commons-fileupload</artifactId>
            <version>1.3.3</version>
</dependency>
```

步骤：

formData包装键值对 后，再用request.open("post","url")   request.send(formData)发送ajax请求（也可以像上面一样用$.ajax()），注意request是一个XMLHTMLrequest对象

```javascript
var formData = new FormData();

formData.append("username", "Groucho");
formData.append("accountnum", 123456); //数字 123456 会被立即转换成字符串 "123456"

// HTML 文件类型 input，由用户选择
formData.append("userfile", fileInputElement.files[0]);

// JavaScript file-like 对象
var content = '<a id="a"><b id="b">hey!</b></a>'; // 新文件的正文
var blob = new Blob([content], { type: "text/xml"});

formData.append("webmasterfile", blob);

var request = new XMLHttpRequest();
request.open("POST", "http://foo.com/submitform.php");
request.send(formData);
```

更加具体查看：

[FormData 对象的使用 - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData/Using_FormData_Objects#%E4%B8%8D%E4%BD%BF%E7%94%A8formdata%E5%AF%B9%E8%B1%A1%E9%80%9A%E8%BF%87ajax%E6%8F%90%E4%BA%A4%E8%A1%A8%E5%8D%95%E5%92%8C%E4%B8%8A%E4%BC%A0%E6%96%87%E4%BB%B6)
