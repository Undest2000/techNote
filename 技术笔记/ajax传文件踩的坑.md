AJAX 传 MutipartFile

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
processData设置为false。因为data值是FormData对象，不需要对数据做处理。

标签添加enctype="multipart/form-data"属性。
cache设置为false，上传文件不需要缓存。
contentType设置为false。因为是由表单构造的FormData对象，且已经声明了属性enctype="multipart/form-data"，所以这里设置为false。

不用AJAX的话直接用input type="submit"上传即可

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

formData包装键值对  如果form已经标了enctype="multipart/form-data",那么调用form的submit方法 再用request.open("post","url") request.send(formData)发送ajax

request是一个XMLHTMLrequest对象
