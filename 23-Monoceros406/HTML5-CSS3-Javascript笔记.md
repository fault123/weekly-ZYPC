---
title: HTML5 & CSS3 & Javascript笔记
date: 2023-10-14 19:20:38
tags: HTML5 CSS3 Javascript
mathjax: true
---

# HTML5 & CSS3 & Javascript笔记

## ???

vscode:Ctrl+Shift+F9

```bash
hh -decompile <输出文件夹路径> <要反编译的CHM文件全路径名>
```

## HTML4文档基本结构

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML1.0 Transitional//EN" "http://www.w1.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w1.org/1999/xhtml">
    <head>
        <meta http-equiv="Content-Type" content="text/html;charset=utf-8"/>
        <title>文档标题</title>
    </head>
    <body>
    </body>
</html>
```

## XHTML文档基本结构

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <meta http-equiv="Content-Type" content="text/html;charset=gb2313"/>
        <title>文档标题</title>
    </head>
    <body>
    </body>
</html>
```

## HTML5文档基本结构

```html
<!DOCTYPE html>
<!--使用工具时声明方法：<!DOCTYPE HTML SYSTEM "about:legacy-compact">-->
<meta charset="UTF-8">
<title>title</title>
<h1>h1</h1>
<p>p</p>
<br/>
```

## 结构元素

```html
<div></div> 定义包含框、容器
<ol></ol> 一定排序进行列表
<ul></ul> 没排序的列表
<li> 每条列表项
<dl></dl> 定义方式列表
<dt> 列表中词条
<dd> 解式定义的词条
<del></del> 删除文本
<ins></ins> 插入文本
<h1></h1>~<h6></h6> 标题
<p> 段落结构
<hr/> 水平线
```

## 内容元素

```html
<span></span> 行内包含框
<a></a> 超链接
<abbr></abbr> 缩写词
<adress></adress> 地址
<dfn></dfn> 术语，斜体
<kbd></kbd> 键盘键
<samp></samp> 样本
<var></var> 变量
<tt></tt> 打印机字体
<code></code> 源代码
<pre></pre> 预定义格式文本
<blockquote></blockquote> 大块内容应用
<cite></cite> 引文
<q></q> 引用短语
<strong></strong> 重要文本
<em></em> 文本定义为重要
```

## 修饰元素

```html
<b></b> 视觉提醒，粗体
<i></i> 语气强调，斜体
<big></big> 较大文本
<small></small> 旁注，缩小
<sup></sup> 上标
<sub></sub> 下标
<bdi></bdi> <bdo></bdo> 文本显示方向
<br/> 换行
<u></u> 非文本注解，下划线
```

## 已废弃

```html
<center></center> 居中
<font></font> 文字样式大小颜色
<s></s> <strike></strike> 删除线
```

## 颜色

>#C99E8C 脏橘
>#465E65 月蓝
>#467897 海蓝
>#E7CD79 栀子黄
>#BE98AA 珊瑚粉红
>#3E3F4C 蓝莓
>#BCCF90 青豆绿
>#F6A09A 水蜜桃粉
>#C8B7A6 莫兰迪咖
>#758D71 莫兰迪绿
>#800020 勃艮第红
>#DCD2C6 米白
>#8D5130 茶褐色
>#015467 孔雀蓝
>#8FD1E1 天青色
>#FEDC5E 黄栗留
>#002FA7 克莱因蓝
>#C8C7C5 雾灰

## 其他不允许结束标记的

```html
<area/><base/><col/><command/><embed/><img/><input/><keygen/><link/><meta/><param/><source/><track/><wbr/>
```

## 其他可省结束标记的

```html
<rt><rp><optgroup><option><colgroup><thead><tbody><tfoot><tr><td><th>
```

## 可全省的

```html
<html></html> <head></head> <body></body> <colgroup></colgroup> <tbody></tbody>
```

## 核心属性

> class:类规则、样式规则
>
> id:唯一标识
>
> style:样式声明
>
> html,head,title,base,meta,param,script,style无

## 语言属性

>lang:语言代码、编码
>
>dir:文本方向
>
>  ltr：左向右 rtl：右向左
>
>frameset,frame,iframe,br,hr,base,param,script无

```html
<html xmlns="http://www.w3.org/1999/xhtml" dir="ltr" xml:lang="zh-CN">
<body id="myid" lang="en-us"></body>
</html>
```

## 键盘属性

> accesskey:Alt+accesskey为快捷键，IE需配合Enter确定
>
> tabindex:定义用Tab键访问顺序

```html
<a href="..." accesskey="a">按Alt+A打开</a>
<a href="..." tabindex="1"></a>
```

## 内容属性

> alt:替换文本
>
>   当图像无法显示、不支持、无法下载时提供的文本说明
>
> title:提示文本
>
> longdesc:大段描述信息
>
> cite:引用信息
>
> datetime:日期时间

## 其他属性

> rel:原文档到目标文档
>
> rev:目标文档到原文档

## HTML5新增

```html
<hgroup></hgroup>
<video scr="..." controls="controls"></video>
<audio src="..."></audio>
<embed scr="..."/> <!--各种多媒体：Midi Wav AIFF AU MP3 SWF等-->
<mark></mark><!--高亮显示-->
<dialog open>对话框窗口</dialog>
<bdi>文本方向，脱离周围文本方向设置</bdi>
<figure>
    <figcaption>定义figure标题</figcaption>
</figure>
<time></time>
<canvas id="..." width="..." height="..."></canvas>
<output></output>
<source>
<menu>菜单列表</menu>
<ruby>汉<rt><rp>（</rp>han<rp>）</rp></rt></ruby><!--ruby：中文注音、字符 rt：解释发音 rp不支持ruby显示的内容-->
<wbr><!--软换行：没必要换行不换，宽度不够换行-->
<!--<command onclick="cut()" label="cut">命令按钮-->
<details>
    <summary>标题</summary>
    细节信息
</details>
<datalist>可选数据列表</datalist>
<datagrid>同上 树形</datagrid>
<!--<keygen>密钥-->
<progress></progress>
<meter value="3" min="0" max="10">十分之三</meter>
<meter value="0.6">60%</meter><!--度量给定范围内数据-->
<video width="320" height="240" controls="controls">
    <source scr="..." type="..."/>
    <track kind="subtitles" src="subs_chi.srt" srclang="zh" label="Chinese"><!--媒体播放器中文本轨道-->
</video>
```

## 表单元素

```html
<input type="..."/>
```

> tel search url email dataetime date month week time datetime-local number range color

## HTML5全局属性

```
contentEditable:
    true或false 可编辑的
contextmenu=...:
    右键上下文菜单 只Firefox
data-*属性:
    ?
draggable:
    true或false或auto（默认行为）
    是否可被拖动
    只IE9+ Firefox Opera Chrome Safari
dropzone:
    copy（产生副本）或move（移动到新位置）或link（产生链接）
    拖动数据时...
    都不支持
hidden:
    true（不可见）或false（可见）
    IE不支持
spellcheck:
    true（检查）或false
    拼写语法检查：input文本值、<textarea>、可编辑元素
    IE10+ Firfox Opera Chrome Safari
translate:
    yes或no
    都不支持
```

## CSS导入

```html
行内：
<p style="background-color:#66CC99">...</p>
内嵌：
<style type="text/css">
    p{
        background-color:#66CC99;
        background-position:2px -31px;
        border:solid 2px red;/*边框样式*/
        border-bottom:3px dashed #009933; /*下边框线*/
        color:#FF0000;
        font-size:18px; /*大小18px*/
        font-weight:bold;/*加粗 normal取消*/
        font-style:italic;/*斜体*/
        font-family:"黑体";
        height:50px;/*标签高*/
        line-height:25px;/*行高 160%表示字体大小1.6倍*/
        margin:0 auto;/*火狐居中*/
        margin-bottom:20px;/*段落下边距20px*/
        margin-
        min-width:776px;
        padding:10px 15px;/*间距*/
        text-align:left; /*左对齐 center:IE居中*/
        text-decoration:underline;/*下划线、none无*/
        text-indent:2em;/*首行缩进2文字*/
        width:500px;/*段落宽度500px，否则居中看不见效果*/
    };
</style>
<p>...</p>
链接：
<link href="*.css" type="text/css" rel="stylesheet"/>
导入：
<style type="text/css">
    @import *.css; /*等价写法*/
    @import '*.css';
    @import "*.css";    
    @import url(*.css);
    @import url('*.css');
    @import url("*.css");
</style>
```

## charset

```html
@charset "gb2312";
```

## 建立网页布局 头部尾部宽度

```html
<style type="text/css">
    .header{width:960px;}
    .footer{width:960px;}
</style>

<head>
    <style type="text/css">
        p{/*标签选择器*/
            ...
        }
        .style1{/*类选择器*/
            ...;
            ... !important;/*最大优先级*/
        }
        #imp{/*ID选择器*/
            ...
        }
        *{/*通配选择器*/
            ...
        }
    </style>
</head>
<body>
    <p class="style1">
        ...
    </p>
</body>
```

## 优先级加权值

标签选择器、伪元素/对象选择器：1

类选择器、属性选择器：10

ID选择器：100

!important：1000

其他选择器（通配选择器等）：0

注意：被继承为0，内联优先，相同时选最后

```html
<div id="..." class="..." style="...">style内联优先</div>
```

## 选择器

```css
a{...}
a:link{...}/*正常*/
a:visited{...}/*访问过*/
a:hover{
    zoom:1;/*解决IE无法显示*/
    ...
}/*鼠标悬停*/
a:active{...}/*单击与释放之间*/
a:focus{...}/*输入焦点时*/
```

## CSS

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <script type="text/javascript" src="IE8.js"></script>
        <style type="text/css">
            h1,h2,h3,h4,h5,h6{ /*分组选择器*/
                background-color:#99CC33;
                margin:0;
                margin-bottom:10px;
            }
            h1+h2,h2+h3,h4+h5{/*相邻选择器：同级相邻，顺序一致，作用于后选择符*/
                color:#0099FF;
            }
            body>h6,h1>span,h4>span{/*子选择器：前者的子标签（后者），作用于后选择符*/
                font-size:20px;
            }
            h2 span,h3 span{/*包含选择器：同上，子孙辈皆可*/
                padding:0 20px
            }
            /*
                p~h3{ 兄弟选择器：同相邻选择器，顺序不要求一致
                    ...
                }
            */
            h5 span[class],h6 span[class]{
                background-color:#CC0033;
            }
        </style>
    </head>
    <body>
        <h1>h1元素<span>span元素</span></h1>
        <h2>h2元素<span>span元素</span></h2>
        <h3>h3元素<span>span元素</span></h3>
        <h4>h4元素<span>span元素</span></h4>
        <h5>h5元素<span class="S1">span元素</span></h5>
        <h6>h6元素<span class="S2">span元素</span></h6>
    </body>
</html>
```

## 伪类选择器

```css
E[attr]{} 有就行，两个及以上时同时具有
E[attr="value"]{} 具有相同值
E[attr~="value"]{} 与列表中某一个值匹配，列表用空格隔开
E[attr^="value"]{} 以...开头
E[attr$="value"]{} 以...结尾
E[attr*="value"]{} 有字串就行
E[attr|="value"]{} 等于value或以value开头
结构伪类选择器：
    :first-child{} 某个元素的第一个子元素 IE6不支持
    :last-child{}
    :nth-child(n){}
        当n=0时没有选择元素，否则选择第n个元素，直接填"n"表示全选，不可直接写负数。
        填"2n"或"even"选择第2、4...个，填"2n-1"或"odd"选择第1、3...个。
        填"n*length"：n的倍数选择；填"n+length"：>=length元素；填"-n+length"：<=length元素；填"n*length+1"：隔几选一。
        IE6~8、FF3及以下不支持
    :nth-last-child(n){} 同上，从后面计算
    :nth-of-type(){} IE6~8、FF3及以下不支持
    :nth-last-of-type(){} 同上，从后面计算
    :first-of-type{}和:last-of-type{}
    :only-child{} 父元素唯一一个子元素 IE6~8不支持
    :only-of-type{} 父元素类型唯一的子元素 IE6~8、FF3及以下不支持
    :empty{} 选择没有任何内容的元素 IE6~8不支持
    :first-letter{} 首字
否定伪类选择器：
    :not(...){} 过滤 IE6~8不支持
状态伪类选择器：IE6~8不支持
    :enable{} 匹配可用UI元素（form内表单元素）
    :disabled{} 匹配不可用UI元素 disabled="disabled"（或enable）
    :check{} 匹配指定范围可用UI元素 checked="checked"（或unchecked）
目标伪类选择器：
    :target{} 当在地址栏输入URL+#(id)匹配该元素时有效 IE8及以下不支持
```

## HTML标签

```html
<h1>一级标题</h1>
<h2>二级标题</h2>
...
<h6>六级标题</h6>

<p>
    段落文本
</p>

<cite title="参考文献">http://...</cite>
<blockquote cite="http://...">
    <p>
        大段引用
    </p>
</blockquote>
<q>行内引用</q>

<em>强调斜体</em>
<strong>强调粗体</strong>
<i>斜体</i>
<big>大一号字体，最大7号</big>
<small>小一号字体，最小1号</small>
<sup>上标</sup>
<sub>下标</sub>
<br/>换行

<pre><<!--预定义格式源代码，保留源代码显示中空格大小-->
    <code>代码字体</code>
    <tt>打印机字体</tt>
    <kbd>键盘字体</kbd>
    <dfn>定义的术语</dfn>
    <var>变量字体</var>
    <samp>代码范例</samp>
</pre>

<abbr title="Abbreviation">abbr缩写</abbr>
<acronym title="Hypertext Markup Language">HTML首字母缩写</acronym> <!--HTML5不支持-->
<!--IE6及以下版本：-->
<span title="...">
    <abbr title="...">
        ...
    </abbr>
</span>

<del datetime="2014-8-8"><!--YYYY-MM-DD-->
    ...
</del>
<ins cite="http://..." datetime="2014-8-1">
    ...
</ins>

<bdo dir="rtl">...</bdo><!--方向显示，dir:ltr或rtl-->

<article>
    <mark>高亮显示</mark> <!--IE8及更早不支持-->
</article>

<section>
    <p>
        进度
    </p>
    <progress id="progress" max="100"><!--支持：Firefox8+ Opera11+ IE10+ Chrome6+ Safari5.2+-->
        <!--
            max:一共多少工作量
            value:已经完成多任务
        -->
        <span>0</span>%
    </progress>
    <input type="button" onclick="click1()" value="进度"/>
</section>
<script>
    function click1(){
        var progress=document.getElementById('progress');
        progress.getElementsByTagName('span')[0].textContent="0";
        for(var i=0;i<=100;i++)
            updateProgress(i);
    }
    function updateProgress(newValue){
        var progress=document.getElementById('progress');
        progress.value=newValue;
        progress.getElementsByTagName('span')[0].textContent=newValue;
    }
</script>

<meter value="3" min="0" max="10">30%</meter>
<meter value="0.6">60%</meter>
<!--
    value:实际值，默认0，可以浮点小数
    min:允许的最小值，默认0，>=0
    max:允许的最大值，默认1，若<min则把min视为max
    low:范围下限，<=high，若<min则把min视为low
    high:范围上限，若<low则把low视为high，若>max则把max视为high
    optimum:最佳值，min~max之间，可>high
    form:所属表单
    支持：Safari5.2+ Chrome6+ Opera11+ Firefox 16+
-->

<article>
    <header>
        <h1>...</h1>
        <time datetime="2016-12-30" pubdate>2016-12-30</time>
        <!--
            datetime:定义日期时间
                2017-11-13
                2017-11-13T20:00
                2017-11-13T20:00Z
                2017-11-13T20:00+09:00
            pubdate:布尔值，True:网页或<article>发布日期，False:更正日期
        -->
    </header>
    <p>...</p>
    <footer>
        ...
    </footer>
</article>

<footer>
    <section>
        <address>
            <!--联系信息-->
            <a title="作者" href="https://...">
                ...
            </a>
        </address>
        <p>
            发布于：
            <time datetime="2017-6-1">...</time>
        </p>
    </section>
</footer>

<bdi>从周围文本方向设置中隔离出来</bdi>
<!--支持：Firefox、Chrome-->

<wbr>软换行，IE不支持

<ruby>
    少<rt>shao</rt>小<rt>xiao</rt>...
    <rp>不支持时的显示内容</rp>
    <!--支持：IE9+ Firefox Opera Chrome Safari-->
</ruby>
<!------------------------------------------------------------>
<!--
    * 代表出现0次或以上。 
    + 代表出现1次或以上。 
    ? 代表是可选的，即出现0次或1次。 
    {A} 代表出现A次。 
    {A,B} 代表出现 A 次以上 B 次以下，其中B可以省略为 {A,}，代表至少出现A次，无上限。 
    # 代表出现1次以上，以逗号隔开，可以选择后面跟大括号的形式，精确表示重复多少次，如<length>#{1,4}。 
    ! 代表出现产生一个值，即使组内的值都可以省略，但至少有一个值不能呗省略，如[ A? B? C? ]!。 
-->
```

## CSS元素

```css
body{
    /**/font-family:name(字体名，有空格加""),...
        serif 衬线
        sans-serif 无衬线
        cursive 草体
        fantasy 奇异
        monospace 等宽
    font-size:...
        xx-small x-small small medium large x-large xx-large
        larger smaller:根据父对象调整，单位em，默认16px
        length:百分数（基于父对象）、浮点+单位，不可为负
        单位：in(inch) cm mm pt(point印刷点数) pc(1pica=12point) px em(1倍父辈字体大小) ex(1倍父辈字体x高度)
    color:字体颜色
        gray
        #666666
        rgb(120,120,120)
        rgb(50%,50%,50%)
        rgba(255,0,0,5) rgb+Alpha通道
            前三个取值0~255 第四个0~1
        hsl(0,100%,100%) 色相[-360,360] 饱和度[0%,100%] 亮度[0%,100%]
            色调：0或360或-360：红 60：黄 120：绿 180：青 240：蓝 300：洋红
        hsla(0,100%,100%,5) hsl+Alpha
        initial和-moz-initial 各种属性使用默认值;
    font-weight:...
        lighter normal(默认400) bolder bold(700)
        100 200 ... 900
    font-style:...
        normal(默认) italic(斜体) oblique(倾斜)
    text-decoration:...
        none(默认) blink(闪烁) underline(下划线) line-through(贯穿线) overline(上划线)
    font-variant:...
        normal(默认) small-caps(小型大写字母)
    text-transform:...
        none(默认) capitalize(单词第一个字母大写) uppercase(所有大写) lowercase
    /**/text-align:...;
        left(左对齐) right(右对齐) center(居中对齐) justify(两端对齐) start end match-parent justify-all
        例如：强制两端对齐，不够一行加空格，仅IE
            text-align:justify;
            text-justify:distribute-all-lines;
    /**/vertical-align:...
        auto:根据layout-flow对齐
        baseline:默认
        sub:对齐下标
        super:对齐上标
        top:内容对象顶端对齐
        text-top:文本与对象顶端对齐
        middle:内容与对象中部对齐
        bottom:内容与对象底端对齐
        text-bottom:文本与对象顶端对齐
        length:浮点+单位、百分数，可负，基线0或0%，偏移量;
        下 bottom middle sub baseline=text-bottom text-top super top 上
        /*仅单元格、图像，文字需要：*/
            vertical-align:...;
            display:table-cell;
    letter-spacing:...
        字距，默认normal;
    /**/word-spacing:...
        词距，默认normal，中文无效;
    /**/line-height:行高
        浮点+单位、百分比，允许负 normal默认1.2em;
    text-indent:首行缩进
        浮点+单位、百分比，可负;
    /**/text-shadow:阴影
        默认none
        语法：length1 length2 (length3) color
            length1、length2为阴影的水平、垂直偏移值，可负
            length3可选，模糊值，不可负
            color 阴影颜色
        可用","隔开，定义多色阴影，必须指定偏移
        支持：Safari Firefox Chrome Opera等
        例如：火焰文字
            background:#000;
            color:red;
            text-shadow:0 0 4px white,
                        0 -5px 4px #FF3,
                        2px -10px 6px #FD3,
                        -2px -15px 11px #F80,
                        2px -25px 18px #F20;
        例如：立体文字
            background:#CCC;
            color:#D1D1D1;
            text-shadow:-1px -1px white,
                        1px 1px #333;
        例如：描边文字
            text-shadow:-1px 0 black,
                        0 1px black,
                        1px 0 black,
                        0 -1px black;
        例如：外发光文字
            text-shadow:0 0 0.2em #F87,
                        0 0 0.2em #F87;
    /**/text-overflow:溢出文本
        clip(不显示"..."只裁切) ellipsis(显示"..."最后一个字符) ellipsis-word(显示"..."最后一个词)
        配合使用：
            white-space:nowrap;/*禁止换行*/
            overflow:hidden;/*禁止文本溢出显示*/
            -o-text-overflow:ellipsis;/*兼容Opera*/
            text-overflow:ellipsis;/*兼容IE Safari(Webkit)*/
            -moz-binding:url('ellipsis.xml#ellipsis');/*兼容Firefox*/
    /**/word-wrap:换行
        normal连续文本换行 break-word边界内换行，如果需要则词内换行
        配合使用，确保所有文本正常显示：
            word-wrap:break-word;
            overflow:hidden;
        Firefox Opera消极支持
    /**/line-break:日文换行 仅IE;
    word-break:换行
        break-all(非亚洲语言文本行任意子内断开) keep-all(中韩日不允许字断开)
        支持：IE Chrome Safari
    white-space:格式化文本
        nowrap(强制同一行) pre(预定义文本格式)
    /**/content:动态内容
        normal(默认) string(文本) attr()(元素属性值) url()(外部资源) counter()(计数器) none(无)
        例如：
        /* <style type="text/css"> */
            ol{
                list-style:none;
            }
            li:before{
                color:#F00;
                font-family:Times New Roman;
            }
            li{
                counter-increment:a 1;
            }
            li:before{
                content:counter(a)".";
            }
            li li{
                counter-increment:b 1;
            }
            li li:before{
                content:counter(a)"."counter(b)".";
            }
            li li li{
                counter-increment:c 1;
            }
            li li li:before{
                content:counter(a)"."counter(b)"."counter(c)".";
            }
        /* </style> */
        <ol>
            <li>一级列表项目1
                <ol>
                    <li>二级列表项目1</li>
                    <li>二级列表项目2
                        <ol>
                            <li>三级列表项目1</li>
                            <li>三级列表项目2</li>
                        </ol>
                    </li>
                </ol>
            </li>
            <li>一级列表项目2</li>
        </ol>
    /**/@font-face:{
        /* <style type="text/css">
            @font-face{外部自定义字体
                font-family:...;
                font-style:...;
                font-variant:...;
                font-weight:...;
                font-stretch:...; 横向拉伸变形
                font-size:...;
                src:url(*.eot); 兼容IE
                src:local("..."),url(*.ttf) format("truetype"); 兼容非IE
                IE5+:*.eot(Embedded Open Type) 其他:*.ttf(TrueType) *.otf(OpenType)
            }
            h1{
                font-family:...;
            }
        </style> */
    /**/margin:...;
        块状元素居中：
            margin-left:auto;
            margin-right:auto;
        margin:0 auto;/*网页在标准浏览器对齐*/
    visibility:...;
        hidden 不可见，占据原本位置
    display:...;
        none:不可见也不占位 block:块状显示 table-cell;
    /**/:边框阴影;
        box-shadow:rgba(0,0,0,0.1) 0px 0px 8px;
        -moz-box-shadow:rgba(0,0,0,0.1) 0px 0px 8px;/*兼容Mozilla:FF等*/
        -webkit-box-shadow:rgba(0,0,0,0.1) 0px 0px 8px;/*兼容Webkit:Chrome、Safari等*/
}
div{
    width:...;
    height:...;
    /**/border:...;
        none清除边框
        样式、颜色、宽度，没有顺序，例如：border:solid red 150px;
    border-color:...;
        顶右底左：border-color:red blue green yellow;
    border-width:...;
        顺序同上：border-width:10px 20px 30px 40px;
    /**/border-style:...;
        dotted点线 dashed虚线 solid实线 double双线 groove立体凹槽 ridge立体凸槽 inset立体凹边 outset立体凸边
        顺序同上：border-style:solid dashed dotted double;
        双线框：两条单线+空隙=边框宽
            3px=1px*2(内外)+1px(空隙)
            4px=2px(外)+1px*2(空隙、内)
            5px=2px*2(内外)+1px(空隙) ...
        其他相关属性：
            border-top-style:...;
            border-right-style:...;
            border-bottom-style:...;
            border-left-style:...;
    /**/opacity:...;/*不透明度*/
        0~1 1不透明，支持：Firefox Safari Opera Chrome IE8+
        filter:alpha(opacity=0~100);/*100不透明 早期IE*/
        -moz-opacity:...;/*Firefox*/
    /**/border-radius:...;/*圆角*/
        一个参数：圆角半径
        两个参数用"/"分隔：水平、垂直半径
        默认none，如果有0则无圆角，参数为浮点+单位、%
        其他：
            border-top-right-radius:...;
            border-bottom-right-radius:...;
            border-bottom-left-radius:...;
            border-top-left-radius:...;
        -moz-border-radius:...;/*兼容Gecko*/
        -webkit-border-radius:..;/*兼容Webkit*/
    background:...;
    background-position:...;
    background-attachment:...;
        fixed(相对窗体固定) scroll(相对元素固定) local(相对元素内容固定)
    /**/background-origin:...;/*Presto IE*/
        border-box(边框开始) padding-box(默认，补白区域开始) content-box(仅内容)
        其他：
            -moz-background-origin:...;/*Mozilla Gecko*/
            -webkit-background-origin:...;/*Webkit*/
    /**/background-clip:...;/*图片裁剪 Presto IE9+*/
        border-box padding-box content-box text
        其他：
            -webkit-background-clip:...;/*Webkit*/
            -moz-background-clip:...;/*Mozilla Gecko（Firefox不支持text值）*/
    /**/background-size:...;
        auto(默认) cover(正好完全覆盖) contain(宽或高正好覆盖)/*按宽高比例放缩*/
        <length>(浮点+单位，不可负) <percentage>0%~100%不可负
            可1~2个值，width height，一个时默认另一个为auto
        其他：
            -webkit-background-size:...;
            -moz-backkground-size:...;
    /**/background-break:...;/*都不支持*/
        bounding-box(内联元素平铺) each-box(每一行平铺) continuous(下一行紧接着上一行)
        替换：
            -moz-background-inline-policy:...;
    /**/-webkit-gradient(<type>,<point>[,<radius>]?,<point>[,<radius>]?[,<stop>]*)
        <type> =linear|radial
        <point> =[<length>|<percentage>|left|center|right]&&[<length>|<percentage>|left|center|right]/*起始、终止点*/
        <radius> =<length>/*raidal时长度*/
        <stop> =[from(<color>)]||[to(<color>)]||[color-stop(<length>|<percentage>,<color>)]/*渐变色和步长*/
        例如：
            background:-webkit-gradient(linear,left top,left bottom,from(blue),to(red));
            background:-webkit-gradient(linear,left top,left bottom,from(blue),to(red),color-stop(50%,green));
            background:-webkit-gradient(linear,left top,left bottom,from(blue),to(red),color-stop(0.4,#FFF),color-stop(0.6,#000));
            background:-webkit-gradient(radial,200 100,10,200 100,100,from(red),to(green),color-stop(90%,blue));
            background:-webkit-gradient(radial,120 100,10,200 100,100,from(red),to(green));
            球形效果：background:-webkit-gradient(radial,180 80,10,200 100,90,from(#00C),to(rgba(1,159,98,0)),color-stop(98%,#0CF));
    /**/-moz-linear-gradient([<point>||<angle>,]?<stop>,<stop>[,<stop>]*)
        <point> =<length>|<percentage>|left|center|right /*起始点*/
        <stop> =[<color>]&&[<length>|<percentage>]?/*步长*/
        例如：
            background:-moz-linear-gradient(red,blue);
            background:-moz-linear-gradient(top left,red,blue);
            background:-moz-linear-gradient(left,red,orange,yellow,green,blue,indigo,violet);
    /**/-moz-radial-gradient([<position>||<angle>,]?[<shape>||<size>,]?<stop>,<stop>[,<stop>]*)
        <point>、<stop>同上
        <shape> =circle|ellipse
        <size>圆半径或椭圆轴长
        例如：
            background:-moz-radial-gradient(red,yellow,blue);
            background:-moz-radial-gradient(red 20%,yellow 30%,blue 40%);
            background:-moz-radial-gradient(bottom left,red,yellow,blue 80%);
            background:-moz-radial-gradient(left,circle,red,yellow,blue 50%);
    /**/box-shadow:...;
        2~4个参数+颜色，参数为：水平偏移，垂直偏移，阴影大小，阴影扩展
        可参数前加"inset"表示内阴影
        -moz-box-shadow:...;/*兼容Gechko*/
        -webkit-box-shadow:...;/*兼容Webkit*/
    /**/linear-gradient();
        linear-gradient([[<angle>|to<side-or-corner>],]?<color-stop>[,<color-stop>]+);
            <side-or-corner> =[left|right]||[top|bottom]
            <color-stop> =<color>[<length>|<percentage>]? <stop>步长
            <angle>可以是deg、grad、rad单位
        例如：
            background:linear-gradient(to bottom,#00ADEE,#0078A5);
            background:-webkit-gradient(linear,left top,left bottom,from(#00ADEE),to(#0078A5));
            background:-moz-linear-gradient(top,#00ADEE,#0078A5);
    radial-gradient();
        radial-gradient([[<shape>||<size>][at <position>]?,|at <position>,]?<color-stop>[,<color-stop>]+)
            <shape> =circle|ellipse
            <size>:圆半径、椭圆轴长
            <position> =[<length>|<percentage>|left|center|right]?[<length>|<percentage>|top|center|bottom]?
                只指定一个，另一个默认center;
    background-repeat:repeat-x|repeat-y|[repeat|space|round|no-repeat]{1,2};
}
body{
    /**/cursor:...;/*光标*/
        crosshair 十字
        pointer 指针
        move 十字表示可移动
        e-resize ne-resize nw-resize n-resize se-resize sw-resize s-resize w-resize 正在移动某个边，框的移动开始于
        text 文本I型
        wait 手表或沙漏
        help ？或气球
        hand 手型，IE专用=pointer
}
html>/**/body ...{
    /*仅非IE中被解析...*/
}
body{
    list-style-type:...;
    list-style:...;
    list-style-position:...;
    list-style-image:...;
    z-index:...;
    /**/display:inline;
        /*
            IE6中，设置浮动+左右外补白，会产生双倍间距的bug，利用此属性解决
        */
}
/*优先权：最大 td tr thead/tbody/tfoot col colgroup table 最小*/
body{
    border-collapse:...;
    border-spacing:...;
    /**/caption-side:...;
        IE也有left right 解析差异大
    /**/empty-cells:...;
        可视：&nbsp;和其他空白字符
        无可视：ASCⅡ码 \0D回车 \0A换行 \09制表 \20空格
    table-layout:...;
}
body{
    box-sizing:...;
    ...
}
```

## 图片

```html
<img src="..." alt="替代文本"/>
    alt
    src
    height
    width:宽、高度 单位px或%
    ismap:定义为服务器端图像映射
    usemap:定义为客户器端图像映射
    longdesc:包含长图像描述文档URL
    border=0 清除边框
```

## 链接

```html
<a href="#">链接文本</a>
<!--
    download 规定被下载目标 强制下载方式（布尔） 只Firefox Chrome
    href
    hreflang 被链接文档语言
    media 规定被链接文档是为何种媒介/设备优化的
    rel 当前与被链接关系
    target 何处打开：_blank新标签页中显示
    type MIME类型
-->
href="mailto:邮件地址?subject=主题"
```

## Map

```html
<img src=".../*.jpg" width="..." height="..." border="0" usemap="#Map">
<map name="Map">
    <area shape="circle" coords="..." href="...">
    <area shape="poly" coords="..." href="...">
    <!--
        其他：
            nohref 排除某个区域
            shape:default rect(矩形) circ(圆) poly(多边形)
            target:_blank _parent _self _top
    -->
</map>
```

## inframe

```html
<inframe src="...">
    <!--
        frameborder 1、0
        height
        longdesc
        marginheight
        marginwidth
        name
        sandbox:"" allow-froms、allow-same-orgin、allow-scripts、allow-top-navigation
        scrolling:yes、no、auto
        seamless:布尔
        src
        srcdoc:HTML内容
        width
    -->
</inframe>
```

## 列表

```html
<ul>
    <!--无序列表-->
    <li>...</li>
    ...
</ul>
<ol>
    <!--有序列表-->
    <!--
        reversed 布尔 倒序
        start 起始值
        type:1 A a I i
    -->
    <li>...</li>
    ...
</ol>
<dl>
    <!--标识定义列表-->
    <dt>标识词条</dt>
        <dd>标识解释</dd>
</dl>
```

## menu

```html
<menu type="toolbar">
<!--
    其他：
        label:可见标签
        type:list(默认) context(必须在用户能与命令交互前被激活) toolbar(允许用户立即与命令交互)
-->
    <command onclick="JS脚本" icon="图像URL" type="checkbox或command或radio" radiogroup="组名" label="可见label">...</command>
    <!--只有IE9、最新Firefox支持-->
    <!--
        其他：
            checked 布尔 是否选中 仅radio或checkbox
            disabled 布尔 是否可用
    -->
    <menuitem>右键菜单</menuitem>
    <!--
        其他：
            checked
            default(默认) 设置为默认
            desabled
            icon
            open 布尔 details是否可见
            label 必需，命令、菜单项名称
            radiogroup 仅radio
            type:checkbox command radio
    -->
</menu>
<!--举例：在任意元素上绑定上下文菜单-->
<h1 contextmenu="menu元素的id">...</h1>
```

## table

```html
<table>
    <caption>标题，紧随table后</caption>
    <tr><!--定义一行-->
        <th>表头 1 （粗体、居中）</th>
        <th>表头 2</th>
    </tr>
    <tr>
        <td>数据 1 （左对齐，普通）</td>
        <td>&nbsp;</td>
    </tr>
</table>
<table width="100%" border="1">
    <caption>...</caption>
    <col align="left或center或right"/><!--第一列文本对齐方式，其他：justify(两端对齐) char(对齐指定字符)-->
    <thead>
        <tr>
            ...
        </tr>
    </thead>
    <tfoot>
        <tr>
            <td colspan="2">合并单元格页脚</td>
        </tr>
    </tfoot>
    <tbody>
        ...
    </tbody>
</table>
<table width="100%" border="1">
    <colgroup span="2" class="..."></colgroup><!--从左往右选择2列设为class=...-->
    <colgroup class="..."></colgroup><!--其次1列设为class=...-->
    ...
    <!--等价于：-->
    <colgroup>
        <col span="2" class="..."/>
        <col class="..."/>
    </colgroup>
</table>
<table>
    <!--
        其他：
        border 整数，单位px
        cellpadding
        cellspacing =CSS中margin
        width
        frame:框线void above below hsides(顶底) lhs(左) rhs(右) vsides(左右) box=border(所有)
        rules:内边线none groups(分组内边线) rows(每行水平线) cols(每列垂直线) all
        summary:摘要
    -->
</table>
<td></td>或<th></th>
<!--
    其他属性：
    abbr
    align:right left cneter justify char
    axis:每个tr中对单元格分类，没有浏览器支持
    char:定义哪个字符对齐
    charoff:对齐字符偏移量
    colspan,rowspan:可跨列、行数
    headers:相关表头
    scope:col colgroup row rowgroup 没用
    valign:top middle bottom baseline
-->
```

## form

```html
<form action="#" method="get" id="form1" name="form1">
    <!--<form>、<input>、<select>、<button>共有元素：name、id-->
    <!--
        <form>属性：
            accept-charset:服务器可处理表单数据字符集
            action:URL 向何处发送表单数据
            autocomplete:on/off 是否启用自动完成
            enctype:如何编码
                application/x-www-form-urlencoded:默认 标准编码格式
                multipart/form-data:非文本内容必须 发送文件必须
                text/plain:纯文本 发送邮件必须
            method:HTTP请求方式
                get:数据量少，效率高，地址栏可看到提交的查询字符串
                post:数据量大，删除、添加时用，发送文件必须
            novalidate:布尔 提交时不验证
            target:_blank、_self、_parent、_top、framename
    -->
    <p>用户名：
        <input name="" type="text"/>
        <!--
            type形式：
                submit 提交按钮
                reset 重设按钮
                image 图像按钮
                button 普通按钮
                text(默认) 单行文本框
                checkbox
                file
                hidden
                password:设置value仍是*或·
                radio
                email:自动检查格式
                url:同上
                number:
                    min、max、step(只能是倍数)、value(默认)
                range:
                    属性同上
                日期时间类：
                    date、month、week、time(属性同上，微调5秒)、datetime、datetime-local
                search:
                    IE、Chrome、Opera支持"x"按钮，Firefox不支持
                tel
                color:IE、Safari不支持
        -->
    </p>
    <p>密码：
        <input name="" type="text"/>
        <!--
            <input>属性：
                accept:设置上传文件类型
                alt:替代文本
                autocomplete:on/off 是否使用自动完成功能
                autofocus:布尔 输入字段在页面加载时是否获得焦点，不适用type="hidden"
                checked:布尔 设置此元素首次加载时被选中
                disabled:布尔 禁用
                form:设置所属表单，多个用空格隔开
                formaction:URL 覆盖表单action属性，适用type="submit"或"image"
                formenctype:覆盖表单enctype属性，适用同上
                formmethod:覆盖表单method属性，适用同上
                formnovalidate:布尔 同上novalidate
                formtarget:同上target
                height:高度，适用image
                list:引用包含输入字段的预定义选项的datalist
                max、min:最大、小值
                maxlength:最大长度
                multiple:允许一个以上值
                name
                pattern:如pattern="[0-9]"表示必须0~9数字
                placeholder:提示
                readonly:只读
                required:必需
                size:宽度
                src:图像URL
                step:合法数字间隔
                type
                value:默认值，提示用户输入格式
                width:宽度，适用image
        -->
    </p>
    <p>
        <input type="submit" value="提交"/>
    </p>
    <textarea name="" cols="40" rows="6">
        <!--
            其他属性：
                readonly布尔
                disabled布尔
                wrap:
                    soft 提交时不换行3
                    hard 提交时包含换行符，必须设置cols
        -->
        内容
    </textarea>
</form>
<form>
    <label>
        <input type="radio或checkbox" name="..." value="男"/>男
    </label>
    <label>
        <input type="radio" name="..." value="女"/>女
    </label>
</form>
<form>
    <select name="...">
        <!--
            其他属性：
                size:下拉菜单项目数
                multiple:定义可以多选，按Shift多选
        -->
        <optgroup label="...">
            <option value="..." selected="selected">...</option>
            <option value="...">...</option>
        </optgroup>
        ...
    </select>
</form>
<form>
    <fieldset>
        <legend>...</legend>
        ...
    </fieldset>
    <label for="username">用户名：
        <!--for属性：与表单绑定，用户单击提示信息时激活对应表单-->
        <input type="text" id="username" name="username" accesskey="a" tabindex="1"/>
        <!--
            属性：
                accesskey:
                    IE:Alt+accesskey
                    Firefox:Alt+Shift+accesskey
                tabindex:
                    默认0 禁用-1 越小优先级越高
                    多个相同时，按照元素出现先后顺序
                    disabled的tabindex无效
        -->
    </label>
    <keygen name="security"/>
</form>
<form action="*.asp" method="get">
    <!--仅Opera支持-->
    <input type="url" list="url_list" name="weblink"/>
    <datalist id="url_list">
        <option label="..." value="http://..."/>
        ...
    </datalist>
</form>
```

## article

```html
<article>
    <header>
        <hgroup>
            标题
        </hgroup>
    </header>
    <aside>
        侧边栏
    </aside>
    <main>
        <section>
            内容
        </section>
    </main>
    <footer>
        脚注
    </footer>
</article>
<nav draggable="true">  
    <a href="*">...</a>
</nav>
```

## JS引用

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script>
            //公共函数变量
            function hello(user){
                return "<h1>Hello,"+user+"</h1>";
            };
        </script>
    </head>
    <script>
        var user="World";
    </script>
    <body>
        <script>
            //加载期间、DOM对象初始化、DOM相关全局引用赋值
            document.write(hello(user));
        </script>
    </body>
</html>
<script src="*.js"></script>
```

## JS

```javascript
loop:for(var j=1;j<6;j++){
    document.write("<br>"+j+":");
    for(var i=1;i<6;i++){
        if(i==3)
            continue loop;
        document.write(i);
    };
};
var object={
    name1:value1,
    name2:value2,
    name3:value3
};
var me={
    name:"张三",
    say:function(){
        return "Hi,world!";
    }
};
document.write("<h1>"+me.name+":"+me.say()+"</h1>");
var me=["张三",function(){return "Hi,world!";}]
document.write("<h1>"+me[0]+":"+me[1]()+"</h1>");
```

```javascript
Math.E
Math.LN10
Math.LN2
Math.LOG10E
Math.LOG2E
Math.PI
Math.SQRT1_2
Math.SQRT2
Number.MAX_VALUE
Number.MIN_VALUE
Number.NaN（等于NaN）
Number.NEGATIVE_INFINITY
Number.POSITIVE_INFINITY
Infinity
```



```javascript
var age=prompt('...',"");
alert("...");
for(var i in document)
    document.write(...);
try{
    var err=new Error("...");
    throw err;
}
catch(ex){
    alert(ex.name+"\n"+ex.message);
}
finally{
    ...
}
with(document.getElementById("...").style){
    borderColor="red";
    borderWidth="1px";
    ...
}
var str="123.30";
var a=parseInt(str);
var b=parseFloat(str);
var a=100;
var c=!!a;
c=c+"";//"true"
a=a*1;//1
function add(a,b){
    if(add.length!=arguments.length)//形参!=实参
        throw new Error("...");
    else
        ...
};
try{
    alert(add(2));
}
catch(ex){
    alert(ex.message);
}
var z=function(x,y){
    return (x+y)/2;
}(23,35);
var a=function(f,x,y){
    return f(x,y);
};
var b=function(x,y){
    return x+y;
};
alert(a(b,3,4));
//闭包函数
function a(){
    var n=0;
    function b(m){
        n+=m;
        return n;
    };
    return b;
};
var a=new Array(1,2,3,"4","5");
var a=new Array(6);
var a=[1,2,3,"4","5"];
a.push(1,2,3);
a.pop();
a.unshift(1,2,3);//数组前加
a.shift();//前删1元素
a=a.splice(2,2);//截取从第2个开始2个
a=a.splice(2,2,7,8,9);//把从第2个开始2个替换成7,8,9
a=a.reverse();
a=a.concat(4,5);//类似push
a=a.concat([4,5],[1,[2,3]]);//添加4,5,1,2,3
a=a.slice(2,5);//取[2,5)
var f=function(x,y){
    return y-x;
};
var b=a.sort(f);//从大到小
a=a.join("-");//返回string
var s=a.split("-");
var b=str.replace("原","替换后");
var s="javascript";
//$$为符号$
var b=s.replace(/(java)(script)/,"$2-$1");//script-java
var b=s.replace(/.*/,"$&$&");//javascriptjavascript
var b=s.replace(/script/,"$&!=$`");//javascript!=java
var b=s.replace(/java/,"$&$' is");//javascript is script
var f=function($1){
    return $1.substring(0,1).toUpperCase()+$1.substring(1);
};
alert(s.replace(/(\b\w+\b)/g,f));
var f=function($1,$2,$3){
    return $2.toUpperCase()+$3;
};
var f=function(){
    //arguments.length为个数
    return arguments[1].toUpperCase()+arguments[2];
};
var a=s.replace(/\b(\w)(\w*)\b/g,f);
var a=s.match(/\d+\g);
for(var i=0;i<a.length;i++)
    sum+=parseFloat(a[i]);
```

```javascript
function typeOf(o){
    var _toString=Object.prototype.toString;
    var _type={
        "undefined":"undefined",
        "number":"number",
        "boolean":"boolean",
        "string":"string",
        "[object Function]":"function",
        "[object RegExp]":"regexp",
        "[object Array]":"array",
        "[object Date]":"date",
        "[object Error]":"error"
    };
    return _type[typeof o]||_type[_toString.call(o)]||(o?"object":"null");
};
```

