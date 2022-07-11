### Bootstrap实现标签卡高亮切换

```javascript
function goToPage(id){
 $("#" + id ).addClass("active").siblings().removeClass("active");
}
```

siblings()返回同级元素
这个方法来实现选项卡高亮的功能
每个选项卡后面都加了onclick="goToPage('divxx')"

```html
<ul class="nav nav-tabs flex-column">
  <li class="nav-item">
    <a id="div01" data-toggle="tab" class="nav-link active" href="#" onclick="goToPage('div01')">Active</a>
  </li>
  <li class="nav-item">
    <a id="div02" data-toggle="tab" class="nav-link" href="#" onclick="goToPage('div02')">Link</a>
  </li>
  <li class="nav-item">
    <a id="div03" data-toggle="tab" class="nav-link" href="#" onclick="goToPage('div03')">Link</a>
  </li>
  <li class="nav-item">
    <a id="div04" data-toggle="tab" class="nav-link" href="#" onclick="goToPage('div04')">Link</a>
  </li>
</ul>
```
