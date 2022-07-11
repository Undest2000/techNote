## ZDialog

#### 重要：子窗口的父对象为 ownerDialog

内容来源：http://www.cool80.com/Demonstrate/465/

#### 提取自[ZCMS](http://www.zving.com/products/zcms/)的弹出框：

1. 代替window.open、window.alert、window.confirm；提供良好的用户体验；
2. 水晶质感，设计细腻，外观漂亮；
3. 兼容ie6/7/8、firefox2/3、Opera；弹出框在ie6下不会被select控件穿透；
4. 无外部css文件，引用Dialog.js即可使用；
5. 对iframe下的应用作了充分考虑，适合复杂的系统应用；
6. Dialog显示的内容（三种）：1、指向一个URL的iframe窗口；2、页面内隐藏的元素中的html内容；3、直接输出一段html内容；
7. 按ESC键可关闭弹出框；

#### 主调函数参数说明：

Dialog.open({ID,Title,URL,InnerHtml,InvokeElementId,Width,Height,Top,Left,Drag,OKEvent,ShowButtonRow,MessageTitle,Message,AutoClose,OnLoad})  

- ID：窗口id号，可省略。每个窗口的id必须是唯一的不能重复。
- Title：窗口标题。如不写此项默认值为""。
- URL： 窗口内容页地址，或使用相对路径或绝对路径，注意如果使用[http://www.host.com](http://www.2ky.cn/Pri_upfile/txdemo/0811/zDialog/zDialogDemo.html#;)形式的绝对地址，则http://不能省略。
- InnerHtml： 窗口内容html代码，用于直接输出html内容，注意不要让生成的内容因为不适当的宽度或定位方式而破坏了Dialog的外观。
- InvokeElementId： 本页面内隐藏的元素的id，用于显示页面内隐藏的元素中的html内容，注意不要让内容因为不适当的宽度或定位方式而破坏了Dialog的外观。
- Width：窗口宽度（dialog内容区宽度），值为数值型，默认值为窗口可见宽的40%。
- Height：窗口高度（dialog内容区高度），值为数值型，默认值为窗口可见宽的20%。
- Left：窗口距浏览器左边距离，值为数值型或字符串型（当使用百分比时为字符串型），如Left:"0%",Top:"0%"为左上，Left:"50%",Top:"50%"为居中，Left:"100%",Top:"100%"为右下。
- Top：窗口距浏览器顶端距离，值为数值型或字符串型（百分比）。
- Drag：是否允许拖动窗口，值为布尔型(true|false)，默认值为true，注意需要页面引用了Drag.js。
- OKEvent：点击确定按钮后执行的函数。
- CancelEvent：点击取消按钮或点击关闭按钮后执行的函数，默认为关闭本Dialog。
- ShowButtonRow：是否不显示按钮栏，值为布尔型(true|false)，默认值为false，当定义了OKEvent或调用了addButton时自动设为true。
- MessageTitle,Message：自定义的窗口说明栏中的小标题和说明。
- ShowMessageRow：是否显示窗口说明栏，值为布尔型(true|false)，默认值为false，当定义了MessageTitle或Message时自动设为true。
- AutoClose：是否自行关闭，值为数值型，默认值为false。
- OnLoad：窗口内容载入完成后执行的程序，值为函数型。

### 1. 普通窗口

   

    Dialog.open({URL:"test.html"});

### 2. 设定了高宽和标题的普通窗口

   

    var diag = new Dialog();
    
    diag.Width = 600;
    
    diag.Height = 300;
    
    diag.Title = "设定了高宽和标题的普通窗口";
    
    diag.URL = "test.html";
    
    diag.show();

### 3. 内容页为外部连接的窗口

   

    var diag = new Dialog();
    
    diag.Width = 900;
    
    diag.Height = 400;
    
    diag.Title = "内容页为外部连接的窗口";
    
    diag.URL = "http://demo.zving.com/";
    
    diag.show();

### 4. 内容页为html代码的窗口

   

    var diag = new Dialog();
    
    diag.Width = 300;
    
    diag.Height = 100;
    
    diag.Title = "内容页为html代码的窗口";
    
    diag.InnerHtml='<div style="text-align:center;color:red;font-size:14px;">直接输出html，使用 <b>InnerHtml</b> 属性。</div>'
    
    diag.OKEvent = function(){diag.close();};//点击确定后调用的方法
    
    diag.show();

### 5. 内容页为隐藏的元素的html内容

   

    var diag = new Dialog();
    
    diag.Width = 300;
    
    diag.Height = 150;
    
    diag.Title = "内容页为隐藏的元素的html";
    
    diag.InvokeElementId="forlogin"
    
    diag.OKEvent = function(){$id("username").value||Dialog.alert("用户名不能为空");$id("userpwd").value||Dialog.alert("密码不能为空")};//点击确定后调用的方法
    
    diag.show();

| 用户登陆 |
| --- | --- |
| 用户名 |     |
| 密　码 |     |

### 6. 在调用页面按钮关闭弹出窗口

     

    var diag = new Dialog();
    
    diag.Modal = false;
    
    diag.Title = "弹出没有遮罩层的窗口";
    
    diag.URL = "test.html";
    
    diag.show();

关闭窗口按钮代码：  Dialog.close();

### 7. 在指定位置弹出窗口

   

    var diag = new Dialog();
    
    diag.Width = 200;
    
    diag.Height = 100;
    
    diag.Modal = false;
    
    diag.Title = "在指定位置弹出窗口";
    
    diag.Top="100%";
    
    diag.Left="100%";
    
    diag.URL = "test.html";
    
    diag.show();

注：可使用数字或百分比（带百分比符号的字符串）来定义相对于当前窗口的位置，换算效果同css中用百分比定义背景图位置，如Left:"0%",Top:"0%"为左上，Left:"50%",Top:"50%"为居中，Left:"100%",Top:"100%"为右下。

### 8. 返回值到调用页面

     

    var diag = new Dialog();
    
    diag.Title = "返回值到调用页面";
    
    diag.URL = "test.html";
    
    diag.OKEvent = function(){$id('getval').value = diag.innerFrame.contentWindow.document.getElementById('a').value;diag.close();};
    
    diag.show();
    
    var doc=diag.innerFrame.contentWindow.document;
    
    doc.open();
    
    doc.write('<html><body><input id="a" type="text"/>请在文本框里输入一些值</body></html>') ;
    
    doc.close();

### 9. 代替window.alert及window.confirm

     

    Dialog.alert("提示：你点击了一个按钮");
    
    Dialog.confirm('警告：您确认要XXOO吗？',function(){Dialog.alert("yeah，周末到了，正是好时候")});

注：Dialog.alert(msg, func, w, h)第二个参数为点击“确定”按钮后执行的函数。  
Dialog.confirm(msg, funcOK, funcCal, w, h)第二个参数为点击“确定”按钮后执行的函数，第三个参数为点击“取消”按钮后执行的函数。

### 10. 创建其它按钮

   

    var diag = new Dialog();
    
    diag.Title = "创建其它按钮";
    
    diag.URL = "test.html";
    
    diag.show();
    
    diag.addButton("next","下一步",function(){
    
        var doc=diag.innerFrame.contentWindow.document;
    
        doc.open();
    
        doc.write('<html><body>进入了下一步</body></html>') ;
    
        doc.close();
    
    })

### 11. 带有内容说明栏的新窗口

   

    var diag = new Dialog();
    
    diag.Title = "带有说明栏的新窗口";
    
    diag.Width = 900;
    
    diag.Height = 400;
    
    diag.URL = "http://demo.zving.com/";
    
    diag.MessageTitle = "泽元网站内容管理系统";
    
    diag.Message = "泽元网站内容管理系统是一个基于J2EE及AJAX技术的企业级网站内容管理系统";
    
    diag.show();

### 12. 显示窗体内容页面标题

   

    var diag = new Dialog();
    
    diag.URL = "http://demo.zving.com/";
    
    diag.show();

注：如果窗体内为iframe内容页，并且没有设置Title属性，并且引用页和当前页在同一个域内，则显示显示窗体内容页面标题。

### 13. 在弹窗的内容载入完成后，执行方法

   

    var diag = new Dialog();
    
    diag.OnLoad=function(){alert("页面载入完成")};
    
    diag.URL = "http://demo.zving.com/";
    
    diag.show();

注：如果窗体内为iframe内容页，要在载入完成后对内容页作操作，必须考虑访问权限，如引用页和当前页应在同一个域内。

### 14. 点击取消及关闭时执行方法

   

    var diag = new Dialog();
    
    diag.Title = "点击取消或关闭按钮时执行方法";
    
    diag.CancelEvent=function(){alert("点击取消或关闭按钮时执行方法");diag.close();};
    
    diag.URL = "test.html";
    
    diag.show();

### 15. 不允许拖拽

   

    var diag = new Dialog();
    
    diag.Drag=false;
    
    diag.URL = "test.html";
    
    diag.show();

### 16. 动态改变窗口大小

   

    var diag = new Dialog();
    
    diag.Title = "修改中窗体尺寸";
    
    diag.URL = "javascript:void(document.write(\'这是弹出窗口中的内容\'))";
    
    diag.OKEvent = function(){
    
        var doc=diag.innerFrame.contentWindow.document;
    
        doc.open();
    
        doc.write('<html><body>窗口尺寸改为600*300</body></html>') ;
    
        doc.close();
    
        diag.setSize(600,300);
    
    };
    
    diag.show();
    
    diag.okButton.value="改变窗口大小"

### 17. 弹出窗口自动关闭

   

    var diag = new Dialog();
    
    diag.AutoClose=5;
    
    diag.ShowCloseButton=false;
    
    diag.URL = "javascript:void(document.write(\'这是弹出窗口中的内容\'))";
    
    diag.show();

注：AutoClose为自动关闭时间，单位秒

### 18. 设置确定按钮及取消按钮的属性

   

    var diag = new Dialog();
    
    diag.Title="设置确定按钮及取消按钮的属性";
    
    diag.ShowButtonRow=true;
    
    diag.URL = "test.html";
    
    diag.show();
    
    diag.okButton.value=" OK ";
    
    diag.cancelButton.value="Cancel";

### 19. 窗体内的按钮操作父[Dialog](https://so.csdn.net/so/search?q=Dialog&spm=1001.2101.3001.7020)

   

    var diag = new Dialog();
    
    diag.Title = "窗体内的按钮操作父Dialog";
    
    diag.URL = "test.html";
    
    diag.show();
    
    var doc=diag.innerFrame.contentWindow.document;
    
    doc.open();
    
    doc.write('<html><body><input type="button" id="a" value="修改父Dialog尺寸" \
    οnclick="parentDialog.setSize(function(min,max){return Math.round(min+(Math.random()*(max-min)))}(300,800))" \
    /><input type="button" id="b" value="关闭父窗口" οnclick="parentDialog.close()" /></body></html>') ;
    
    doc.close();
