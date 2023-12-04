---
title: Javascript操作BOM
date: 2023-10-15 20:11:06
tags: Javascript
mathjax: true
---

# Javascript操作BOM

## 基本操作

```javascript
var a="...";
function f(){
    alert(a);
};
alert(window.a);
window.f();
var user=prompt("...");
var ok=confim("...");
```

## 实例1

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <style type="text/css">
            #alert_box{
                position:absolute;
                left:50%;
                top:50%;
                width:400px;
                height:200px;
                display:none;
            }
            #alert_box dl{
                position:absolute;
                left:-200px;
                top:-100px;
                width:400px;
                height:200px;
                border:solid 1px #999;
                border-radius:8px;
                overflow:hidden;
            }
            #alert_box dt{
                background-color:#CCC;
                height:30px;
                text-align:center;
                line-height:30px;
                font-weight:bold;
                font-size:15px;
            }
            #alert_box dd{
                padding:6px;
                margin:0;
                font-size:12px;
            }
        </style>
    </head>
    <body>
        <script>
            window.alert=function(title,info){
                var box=documnet.getElementById("alert_box");
                var html='<dl><dt>'+title+'</dt><dd>'+info+'</dd></dl>';
                if(box)
                    box.innerHTML=html,
                    box.style.display="block";
                else{
                    var div=document.createElement("div");
                    div.id="alert_box",
                    div.style.display="block",
                    document.body.appendChild(div);
                    div.innerHTML=html;
                };
            };
            alert("标题","内容");
        </script>
    </body>
</html>
```

## 窗口操作

```javascript
window.open(URL,name,features,replace);
/*
    URL:可选字符串，省略或空不会显示任何文档
    name:新窗口名称 可选字符串
        用作<a>和<form>属性target的值
        如果已经存在，则返回窗口引用，忽略features
    features:可选字符串
        channelmode=yes|no|1|0 剧院模式 no
        directories=yes|no|1|0 添加目录按钮 yes
        fullscreen=yes|no|1|0 全屏（同时剧院） no
        height:文档显示区高度，单位px
        left:x坐标，单位px
        location=yes|no|1|0 显示地址字段 yes
        menubar=yes|no|1|0 显示菜单栏 yes
        resizable=yes|no|1|0 是否可调节尺寸 yes
        scrollbars=yes|no|1|0 显示滚动条 yes
        status=yes|no|1|0 状态栏 yes
        toolbar=yes|no|1|0 浏览器工具栏 yes
        top:y坐标 单位px
        width:文档显示区宽度 单位px
    replace:可选布尔
*/
myWindow=window.open();
myWindow.document.write("...");//新窗口
myWindow.focus();
myWindow.opener.document.write("...")//原窗口
myWindow.opener=null;//打开指定URL，这里是新标签页
myWindow.close();
//实例：
var url="url1";
var features="heiht=500, width=800, top=100, left=100,toolbar=no, menubar=no,scrollbars=no, resizable=no, location=no, status=no";
document.write('<a href="url2" target="newW">...</a>');
var me=window.open(url,"newW",features);
setTimeout(function(){
    if(me.closed)
        ...;
    else
        me.close();
});
//探测浏览器是否禁止弹窗
var error=false;
try{
    var w=window.open("https://www.baidu.com/","_blank");
    if(w==null)
        error=true;
}
catch(ex){
    error=true;
};
if(error)
    ...;//禁止
```

## 框架集

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <title>...</title>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
        <title>文档标题</title>
    </head>
    <frameset rows="50%,50%" cols="*" frameborder="yes" border="1" framespacing="0">
        <frameset rows="*" cols="33%,*,33%" framespacing="0" frameborder="yes" border="1">
            <frame src="left.htm" name="left" id="left"/>
            <!--
                访问方法：
                window.frames[0]
                window.frames["left"]
                top.frames[0]
                top.frames["left"]
                frames[0]
                frames["left"]
            -->
            <frame src="middle.htm" name="middle" id="middle"/>
            <frame src="right.htm" name="right" id="right"/>
        </frameset>
        <frame src="bottom.htm" name="bottom" id="bottom"/>
    </frameset>
    <noframes>
        <body></body>
    </noframes>
</html>
<!--在right.htm中-->
<script>
    window.onload=function(){
        document.body.onclick=f;
    };
    var f=function(){
        parent.frames[2].document.body.style.backgroundColor="red";
    };
</script>
<!--在left.htm中-->
<script>
    function left(){
        ...;
    };
</script>
<!--在middle.htm中-->
<script>
    window.onload=function(){
        document.body.onclick=f;
    };
    var f=function(){
        parent.frames[0].left();
    };
</script>
```

## 获取窗口左、上位置

```javascript
/*
    IE Safari Opera Chrome支持screenLeft screenTop
    Firefox Safari Chrome支持screenX screenY
*/
var leftPos=(typeof window.screenLeft=="number")?window.screenLeft:window.screenX;
var topPos=(typeof window.screenTop=="number")?window.screenTop:window.screenY;
window.moveTo(0,0);
window.moveBy(0,100);
//Opera IE7+被禁用 不适用框架
```

## 获取页面视图大小

```javascript
/*
    IE9+ Safari Firefox:outerWidth outerHeight窗口尺寸，innerWidth innerHeight页面视图去掉边框
    Opera:outerWidth outerHeight视图容器尺寸，innerWidth innerHeight页面视图去掉边框
    Chrome:outerWidth outerHeight innerWidth innerHeight都为视图大小
    IE8-用DOM
    IE Firefox Safari Opera Chrome:document.documentElement.clientWidth document.documentElement.clientHeight页面视图信息
    IE6怪异模式:document.body.clientWidth document.body.clientHeight页面视图信息
    Chrome怪异模式:document.documentElement.clientWidth document.documentElement.clientHeight ocument.body.clientWidth document.body.clientHeight都行
*/
var pageWidth=window.innerWidth,pageHeight=window.innerHeight;
if(typeof pageWidth!="number"){
    if(document.compatMode=="CSS1Compat")
        pageWidth=document.documentElement.clientWidth,
        pageHeight=document.documentElement.clientHeight;
    else
        pageWidth=document.body.clientWidth,
        pageHeight=document.body.clientHeight;
};
```

## 窗口控制

```javascript
window.onload=function(){
    timer=window.setInterval("jump()",1000);
};
function jump(){
    window.resizeTo(200,200);
    x=Math.ceil(Math.random()*1024),
    y=Math.ceil(Math.random()*760);
    window.moveTo(x,y);
};
```

## setTimeout()方法

```javascript
var o=document.getElementsByTagName("body")[0].childNodes;
for(var i=0;i<o.length;i++){
    o[i].onmouseover=function(i){
        return function(){
            f(o[i]);
        };
    }(i);
    o[i].onmouseout=function(i){
        return function(){
            clearTimeout(o[i].out);
        };
    }(i);
};
function f(o){
    o.out=setTimeout(function(){
        alert(o.tagName);
    },500);
};
```

## setInterval()方法

```javascript
var t=document.getElementByTagName("input")[0],i=1,out=setInterval(f,1000);
function f(){
    t.value=i++;
    if(i>10)
        clearTimeout(out);
};
```

## navigator对象

```javascript
/*
    属性：
    appCodeName appMinorVersion appName appVersion browserLanguage cookieEnabled cpuClass onLine platform systemLanguage userAgent userLanguage
*/
var s=window.navigator.userAgent;
//检测浏览器
var ua=navigator.userAgent.toLowerCase();
var info={
    ie:/msie/.test(ua)&&!/opera/.test(ua),
    op:/opera/.test(ua),
    sa:/version.*safari/.test(ua),
    ch:/chrome/.test(ua),
    ff:/gecko/.test(ua)&&!/webkit/.test(ua)
};//info.ie op sa ff ch哪个为true则是哪个
//获取IE版本号
function getIEVer(){
    var ua=navigator.userAgent,b=ua.indexOf("MSIE ");
    if(b<0)
        return 0;
    return parseFloat(ua.substring(b+5,ua.indexOf(";",b)));
};
//获取Firefox版本号
function getFFver(){
    var ua=navigator.userAgent,b=ua.indexOf("Firefox/");
    if(b<0)
        return 0;
    return parseFloat(ua.substring(b+8,ua.lastIndexOf("\.")));
};
//检测操作系统
var isWin=(navigator.userAgent.indexOf("Win")!=-1);
var isMac=(navigator.userAgent.indexOf("Mac")!=-1);
var isUnix=(navigator.userAgent.indexOf("X11")!=-1);
var isLinux=(navigator.userAgent.indexOf("Linux")!=-1);
//检测插件
function hasPlugin(name){
    name=name.toLowerCase();
    for(var i=0;i<navigator.mimeTypes.length;i++)
        if(navigator.mimeTypes[i].name.toLowerCase().indexOf(name)>-1)
            return true;
    return false;
};
//IE中通过COM标识符检测插件
function hasIEPlugin(name){
    try{
        new ActiveXObject(name);
        return true;
    }
    catch(ex){
        return false;
    };
};
```

## location对象

```javascript
/*
    属性：
    href:完整
    protocol:http:
    host:www.mysite.cn:80
    hostname:www.mysite.cn
    port:80
    pathname:news/index.asp
    search:?id=123&name=location
    hash:#top
*/
var queryString=function{
    var q=location.search.substring(1),a=q.split("&"),o={};
    for(var i=0;i<a.length;i++){
        var n=a[i].indexOf("=");
        if(n==-1)
            continue;
        var v1=a[i].substring(0,n),v2=a[i].substring(n+1);
        o[v1]=unescape(v2);
    };
    return 0;
};
var f1=queryString();
for(var i in f1){
    alert(i+"="+f1[i]);
};
//直接跳转
location.hash="#top";
location="http://...";
location.href="http://";
//获取网页名称
var p=location.pathname;
var n=p.substring(p.lastIndexOf(".")+1);
```

## history对象

```javascript
/*
    back()
    forward()
    go:若参数为：
        正整数 等价于history.forward()
        负整数 等价于history.back()
        0 等价于刷新页面
        字符串 访问第一个检索到的包含该字符串的URL
*/
   /访问框架的历史记录
frames[n].history.back();
frames[n].history.forward();
frames[n].history.go(m);
```

## screen对象

```javascript
/*
    属性：
    availHeight 显示屏幕高、宽（除Windows任务栏）
    availWidth
    bufferDepth 调色板比特深度
    colorDepth 目标设备、缓冲器调色板比特深度
    deviceXDPI 显示屏幕每英寸水平、垂直点数
    deviceYDPI
    fontSmoothingEnabled 是否启用字体平滑
    height 显示屏幕高、宽度
    width
    logicalXDPI 显示屏幕每英寸水平、垂直方向常规点数
    logicalYDPI
    pixelDepth 显示屏幕颜色分辨率 单位bit/px
    updateInterval 设置或返回屏幕刷新率
*/
//窗口居中显示
function center(url){
    var w=screen.availWidth/2,h=screen.availHeight/2,t=(screen.availHeight-h)/2,l=(screen.availWidth-w)/2,p="top="+t+",left="+l+",width="+w+",height="+h,win=window.ope(url,"url",p);
    win.focus();
};
```

## document对象

```html
<!--法1-->
<img name="img" src="#"/>
<form name="form" method="post" action="#"></form>
<script>
    alert(document.img.src);
    alert(document.form.action);
</script>
<!--法2-->
<img src="#"/>
<form method="post" action="#"></form>
<script>
    alert(document.images[0].src);
    alert(document.forms[0].action);
</script>
<!--法3-->
<img name="img" src="#"/>
<form name="form" method="post" action="#"></form>
<script>
    alert(document.images["img"].src);
    alert(document.forms["form"].action);
</script>
<script>
    //动态生成文档内容-框架集
    window.onload=function(){
        document.body.onclick=f;
    };
    function f(){
        parent.frames[1].document.open();
        parent.frames[1].document.write('...');
        parent.frames[1].document.close();
    };
</script>
```

## 远程交互

```html
<!--iframe_main.html-->
<html>
<head>
    <title>交互页面</title>
    <script>
        function hideIframe(url){
            var hideFrame=null;
            hideFrame=document.createElement("iframe"),
            hideFrame.name="hideFrame",
            hideFrame.id="hideFrame",
            hideFrame.style.height="0px",
            hideFrame.style.width="0px",
            hideFrame.style.position="absolute",
            hideFrame.style.visibility="hidden";
            document.body.appendChild(hideFrame);
            setTimeout(function(){
                frames["hideFrame"].location.href=url;
            },10);
        };
        function request(){
            var user=document.getElementById("user"),pass=document.getElementById("pass"),s="iframe_server.html?user="+user.value+"&pass="+pass.value;
            hideIframe(s);
        };
        function callback(b,n){
            if(b&&n){
                var e=document.getElementsByTagName("body")[0];
                e.innerHTML="<h1>"+n+"</h1><p>欢迎登录</p>";
            }
            else{
                alert("用户名密码错误");
                var user=parent.document.getElementById("user"),pass=parent.document.getElementsById("pass");
                user.value=pass.value="";
            };
        };
        window.onload=function(){
            var b=document.getElementById("submit");
            b.onclick=request;
        };
    </script>
</head>
<body>
    <h1>用户登录</h1>
    用户名<input name="" id="user" type="text"><br/><br/>
    密码<input name="" id="pass" type="password"><br/><br/>
    <input name="submit" type="button" id="submit" value="提交"/>
</body>
</html>
<!--iframe_server.html-->
<html>
    <head>
        <title>服务器端响应处理页面</title>
        <script>
            window.onload=function(){
                var query=location.search.substring(1),a=query.split("&"),o={};
                for(var i=0;i<a.length;i++){
                    var pos=a[i].indexOf("=");
                    if(pos==-1)
                        continue;
                    var name=a[i].substring(0,pos),value=a[i].substring(pos+1);
                    o[name]=unescape(value);
                };
                var n,b;
                ((0["user"])&&o["user"]=="admin")?(n=o["user"]):(n=null);
                ((0["pass"])&&o["pass"]=="123456")?(b=true):(b=false);
                parent.callback(b,n);
            };
        </script>
    </head>
    <body>
        <h1>服务器端响应处理页面</h1>
    </body>
</html>
```

