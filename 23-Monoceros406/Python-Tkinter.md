---
title: Python-Tkinter
date: 2023-10-15 20:51:21
tags: Tkinter
mathjax: true
---

# Python-Tkinter

```python
from tkinter import *
from tkinter.ttk import *
win=Tk()
win2=Toplevel() #弹出顶层窗口
win.mainloop()

win.title("...")#窗口标题
win.geometry("300x300+0+0")#窗口大小300x300 左上角(0,0)
win.geometry("300x300-0-0")#同理右下角
win.configure(bg="yellow"或"#a7ea90")#窗口背景颜色
win.maxsize(int,int)#窗口最大尺寸
"""int""" win.winfo_screenwidth()#屏幕宽度
"""int""" win.winfo_screenheight()#屏幕高度
#其他：
win.minsize()
win.resizable(True,True)#第一个是否可改宽度，第二个高度 True可以False无法
win.state("zoomed")#最大化
win.iconify()#最小化
win.iconbitmap()#设置默认图标

#Widget公共属性
"""
    foreground/fg 文字颜色
    background/bg 背景颜色
    width 宽度
    height 高度
    anchor 文字输出位置 默认center
        nw n      ne
        w  center e
        sw s      se
    padx 文字距离组件边缘水平间距
    pady 同上 垂直间距
    font 文字样式
        size
        family 字体
        weight 如：bold
        slant 如：italic
        underline True或False
        overstrike 删除线 True或False

        例如：font="华文新魏 16 bold"
    relief 边框样式
        solid raised sunken flat groove ridge
    cursor 鼠标悬停样式
        spider等
"""

#pack()方法
"""
    side
        top 上->下
        bottom 下->上
        left 左->右
        right 右->左
    padx pady 组件边界据窗口边界
    ipadx ipady 同Widge属性中padx pady
    fill 完全填充空白空间
        x 水平
        y 垂直
        both 水平垂直
        none 默认 不填充
    expand 是否填满整个空间 True或False
    anchor 组件位置 参数同Widge
    before/after 位于指定组件前/后面 ?
"""

#grid()方法
"""
    row column 组件所在行列
    rowspan columnspan 横/纵向合并行/列数
    sticky 对齐 N上 S下 W左 E右 （非字符串入参）
        N+S 拉长高度 顶底对齐
        N+S+E 拉长高度 顶底对齐 右切齐
        N+S+W 拉长高度 顶底对齐 左切齐
        E+W 拉长宽度 左右对齐
        N+S+E+W 拉长高度 顶底对齐 左右切齐
    padx pady 同pack()
"""
win.rowconfigure(0,weight=1) #第0行 改变组件所占空间随窗口缩放的比例=1 ?
win.columnconfigure(1,weight=1) #第1列 ?

#place()方法
"""
    x y 距离窗口左侧/顶部的水平/垂直距离
    width height 组件宽/高度
    relx rely 距离容器左侧/顶部相对距离 0~1
    relwidth relheight 相对父容器宽/高度 0~1
"""
```

## Label组件

```python
"""
    text
    justify 多行时最后一行文本对齐方式 center left等
    image
    compound 有文字图片
        top 图片在文字上
        bottom -下
        left -左
        right -右
        center 重叠，文字在图片上层
    wraplength 自动换行的像素数
"""
label=Label(win,text="...",bg="#yellow"等).pack()#添加文字
label.config("...") #修改文字
info=label.cget("text") #获取内容
img=PhotoImage(file="...")
Label(win,image=img).pack()
#当需要.jpg时：
from PIL import Image,ImageTk
image=Image.open("...")
img=ImageTk.PhotoImage(image)
Label(win,image=img).pack()
```

## Entry组件

```python
"""
    show 输入的内容隐藏
"""
Entry(win,show="*").pack()
#get()
def show():
    str=entry.get()
    print(str)
entry=Entry(win).pack()
Button(win,command=show).pack()
###############
entry.insert(index,str)#从index处添加str index可以是INSERT（光标处）
entry.delete(first,end)#删除first~end所有字符串 删除所有(0,END)
```

## Text组件

```python
text=Text(win)
text.insert(INSERT,...)
photo=PhotoImage(file='...')
text.image_create(END,image=photo)
text.window_create("2.end",window=bt) #将按钮组件等放置在text组件中
print(text.get(1.2,1.6)) #获取第一行第3列~第一行；第7列字符
    #行.列
    #insert 插入光标的位置
    #2.end 第2行最后一个字符位置
    #2.1+2 chars 第2行第4个字符位置
    #2.3-2 chars 第2行第2个字符位置
"""
    其他方法：
    delete() 删除内容
    get() 获取内容
    mark_set() 添加标记
"""
text=Text(win,...,undo=True,autoseparators=False)
text.edit_undo() #撤销
text.edit_redo() #恢复
text.edit_separator() #设置撤销、恢复的分割线
text.bind('<Key>',callback) #callback为函数名
    #可填：任意键<Key> <Control-Z> <Control-Y>
```

## Spinbox组件

```python
"""
    属性：
    activebackground 处于active的背景颜色
    buttonbackground 箭头背景颜色
    buttoncursor 鼠标悬停在箭头上样式
    command 通过箭头调节调用函数
    disabledbackground disabled状态时背景颜色
    disabledforeground disabled状态时前景颜色
    exportselection 是否可以复制到剪切板
    increment 单击箭头递增/减的数值
    justify 文本对其方式
    readonlybackground readonly状态时背景颜色
    state 可选值normal disabled readonly
    textvariable 关联变量
"""
Spinbox(win,from_=n1,to=n2) #当可选为数字时：n1~n2
Spinbox(win,values=(...,...)) #指定可选值
#获取值 法1
    def typ():
        ...#val.get()
    val=StringVar()
    val.set("...") #默认值
    Spinbox(win,values=("...","...",...),textvariable=val,command=typ).grid(row=0,column=1,pady=10)
#获取值 法2
    def pay():
        ...#int(num.get())
    num=Spinbox(win,from_=1,to=5,command=pay)
    num.grid(row=1,column=1,pady=10)
"""
    方法：
    get()
    insert(index,text) text插入到index
    selection('from',index) 选中范围
    selection('to',index)
    selection('range',start,end)
    selection_element(element=None)
"""
```

## Scale组件

```python
Scale(win,from_=1,to=50,resolution=1,orient=HORIZONTAL)
    #orient:HORIZONTAL水平 VERTICAL垂直
"""
    属性：
    activebackground 鼠标悬停在滑块上，尺度条背景色
    command
    digits 尺度数值
    label 标签 水平左上 垂直右上
    length 相对父容器高度 0~1
    repeatdelay 按住滑块多久可以拖动
    showvalue 0为不显示尺度条数值
    tickinterval 尺度条每单元长度数值
    touchcolor 刻度尺颜色
    variable 设置/获取刻度尺数值
"""
```

## Button组件

```python
Button(win,text="...",command=callback)
img=PhotoImage(file='*.png')
butback=Button(win,image=img)
"""
    属性：
    activebackground 激活时背景颜色
    activeforeground 激活时前景颜色
    bd 边框宽 默认2px
    command
    image
    state 状态NORMAL ACTIVE DISABLED
    wraplength 限制每行字符数量
    text
    underline 例如值为1表示第2跟字符带下划线
"""
```

## Radiobutton组件

```python
vali=IntVar()
vali.set("...") #默认选中的选项
radio1=Radiobutton(win,variable=vali,value="...",text="...").pack()
radio2=Radiobutton(win,variable=vali,value="...",text="...").pack()
#vali.get()返回选中的value
"""
    Radiobutton特殊属性：
    image
    text
    compound 与Label相同
    cursor
    indicatoron 是否绘制按钮前小圆圈
    selectcolor 选择框颜色
    selecteimage 被选中时状态
    state 单选按钮状态
    value 按钮值
    variable 设置/获取当前选中按钮
"""
```

## Checkbutton组件

```python
val1=IntVar()
checkbox1=Checkbutton(win,variable=val1,text="...").pack()
val2=IntVar()
checkbox2=Checkbutton(win,variable=val2,text="...").pack()
#val1.get() 选中为1 未选中为0
```

## Listbox组件

```py
listbox=Listbox(win,...)
"""
    特殊属性：
    listvariable 指向StringVar变量，存放所有项目
    selectbackground 选项被选中时背景颜色
    selectmode 单选single 单选+拖动鼠标/方向键改变选项browse 多选multiple 通过<Shift>、<Ctrl>或拖动鼠标多选extended
    takefocus 是否可通过<Tab>转移焦点
    xscrollcommand 添加水平滚动条
    yscrollcommand 添加垂直滚动条
"""
listbox.insert(END,"...") #添加选项
def typeIn(event):
    #listbox.get(listbox.curselection())
    ...
val=StringVar()
val.set("选项1 选项2 ...")
listbox=Listbox(win,...,listvariable=val)
listbox.bind("<Double-Button-1>",typeIn) #左键双击 左键单击为<Button-1>
"""
    其他方法：
    insert(index,text) 添加选项
    delete(index)或delete(start,end) 删除选项
    selection_set(index)或selection_set(start,end) 选中选项
    selection_get(index) 获取某项内容
    size() 列表框组长度
    selection_includes() 判断某项是否被选中
"""
```

## Scrollbar组件

```python
def gettext(event):
    index1=fruites.curselection()
    for item in index1:
        str+=fruites.get(item)
scr1=Scrollbar(win)
fruites=Listbox(win,yscrollcommand=scr1.set,...)
fruites.pack(...)
fruites.bind("<<ListboxSelect>>",gettext)
scr1.pack(...)
scr1.config(command=fruites.yview)
```

## OptionMenu组件

```python
val=StringVar()
fruits=("选项1","选项2",...)
optionmenu=OptionMenu(win,va,*fruits) #或：optionmenu=OptionMenu(win,val,"选项1","选项2",...)
optionmenu.set(fruits[0]) #设置默认选中
optionmenu.pack(...)
items=optionmenu.get() #获取选中
```

## Combobox组件

```python
from tkinter import *
from tkinter.ttk import *
val=StringVar()
fruits=("选项1","选项2",...)
combobox1=Combobox(win,textvariable=val,values=fruits)
combobox1["values"]=("选项1","选项2",...)
combobox1.current(0) #默认第一个
combobox1.set("...") #设置当前值
str=combobox1.get()
combobox.bind("<<ComboboxSelected>>",getMon)
```

## Frame组件

```python
box=Frame(win)
box.pack()
```

## LabelFrame组件

```python
labelframe=LabelFrame(win,text="...")
labelframe.pack()
```

## PaneWindow组件

```python
panewindow=PanedWindow(win,sashrelief=SUNKEN,...)
"""
    其他属性：
    bg或background
    borderwidth
    handlepad 手柄位置
    handlesize 手柄边长
    orient:HORIZZONTAL VERTICAL
    sashrelief:relief sunken raised groove ridge
    showhandle
    width:面板整体宽度 忽略则由子组件决定
"""
panewindow.pack()
btn1=Button(panewindow,...)
panewindow.add(btn1)
```

## Notebook组件

```python
from tkinter import *
from tkinter.ttk import *
note=Notebook(win,...)
pane=Frame() #Frame或LabelFrame
note.add(pane,text="...")
note.pack()
```

## Message组件

```py
mess=Message(win,text="...",variable=val,...)
"""
    其他属性：
    anchor
    aspect:组件宽/高，百分比 默认150 设置width时无效
    cursor
    font
    justify:LEFT CENTER RIGHT
    relief
    takefocus:TRUE接受输入焦点
    text
    textvariable:StringVar()
    width:宽 单位字符
"""
val=StringVar()
val.set("...")
```

## Messagebox组件

```python
from tkinter.messagebox import *
"""
    参数：
    1. 标题
    2. 内容
    
    通用属性：
    default:默认按钮，默认第一个
    icon:INFO ERROR QUESTION WARNING
    parent:关闭时指向的父窗口
"""
showinfo("...","...",...)
showwarning("...","...",...)
showerror("...","...",...)
boo=askokcancel("...","...",...) #True确定
boo=askyesno("...","...",...) #True是
boo=askyesnocancel("...","...",...) #True是 False否 None取消
boo=askretrycancel("...","...",...) #True重试
```

## Menu组件

```python
menu=Menu(win,...)
menu.add_command(label="...",command=help)
win.config(menu=menu1)
#常见方法
menu.add_command(option) #添加命令菜单项
menu.add_cascade(option) #添加父菜单
menu.add_checkbutton(option) #添加多选按钮菜单项
menu.add_radiobutton(option) #添加单选按钮菜单项
menu.add_separator(option) #添加分割线
menu.delete(index1,index2) #删除index1~index2所有菜单项
menu.entrycget(index,option) #获得index的值
menu.entryconfig(index,option) #设置index的值
menu.index(index) #返回index对应选项的序号
menu.insert(index,itemType,option) #插入菜单项
menu.insert_cascade(index,option) #添加父菜单
menu.insert_checkbutton(index,option) #添加复选框
menu.insert_radiobutton(index,option) #添加单选按钮
menu.insert_command(index,option) #添加子菜单
menu.insert_separator(index,option) #添加分割线
menu.invoke(index) #调用指定菜单选项
menu.post(x,y) #指定位置弹出菜单
menu.type(index) #获得指定项类型 "command" "cascade" "checkbutton" "radiobutton" "separator"
menu.unpost() #移除弹出菜单
menu.yposition(index) #返回指定菜单项的垂直偏移位置
"""
    option属性：
    postcommand 菜单被打开时调用的方法
    tearoff 是否能从窗口分离 默认True
    cursor
    tearoffcommand 被分离时执行的方法
    background/bg
    selectcolor 当为单/多选按钮时选中标志的颜色
    activebackground active状态背景色
    activeborderwidth active状态边框宽
    activeforeground active抓过你太前景色
    bordewidth/bd 指定边框宽
    disabledforeground disabled前景色
    font
    foreground/fg
    relief
    title 被分离菜单标题，默认父菜单名字
"""
#二级菜单
menu1=Menu(win)
menu2_1=Menu(menu1,tearoff=False)
menu1.add_cascade(label="城市",menu=menu2_1)
menu2_1.add_command(label="北京",accelerator="Ctrl+Up")
menu2_1.add_separator()
menu1.add_command(label="退出",command=win.quit)
win.config(menu=menu1)
win.bind_all("<Control-Up>",max_win)
```

## Treeview组件

```python
from tkinter.ttk import *
tree=Treeview(win,columns=("hero","type","operate"),show="headings",displaycolumns=(0,1,2))
"""
    参数：
    columns:值为list
    displaycolumns:是否显示、显示顺序 "#all"表示全部显示
    height:可显示几行数据
    padding:标题栏内容距组件边缘间距
    selectmode:extend通过Ctrl+鼠标选择（默认） browse只能选一行 none不能改变选择
    show:tree headings显示所有列 tree显示图标栏 headings显出除第一列外其他列
"""
tree.heading("hero",text="英雄",anchor="center")
...
tree.insert("父对象",插入位置,ID,...)
tree.insert("",END,values=("A","a","5"))
"""
    属性：
    text 子项目名称
    image 图表
        image=PhotoImage(file="*.png)
    values 值，未赋值为空列，超过长度会被截
    open 展开或关闭
    tags 与item关联的标记
"""
#ID用法：
tree.heading("#0",text="皇帝")
tree.insert("",0,"wei",text="魏")
shu=tree.insert("",1,text="蜀")
tree.insert("wei",0,text="曹操")
tree.insert(shu,0,text="刘备")
...
tree.pack()
"""
    虚拟事件：
    <<TreeviewSelect>> 选项发生变化时
    <<TreeviewOpen>> items的open=True时
    <<TreeviewClose>> items的open=False时
    
    常用方法：
    bbox(item,column=None)
    get_children(item=None)
    set_children(item,*newchildren)
    column(column,option=none,**kw)
    delete(*item)
    detach(*item)
    exists(item)
    focus(item=None)
    heading(column,option=None,**kw)
    insert(parent,index,iid=none,**kw)
    item(item.option=None,**kw)
    selection()
    selection_set(*item)
    selection_add(*item)
    selection_remove(*item)
    selection_toggle(*item)
    set(item,column=None,value=None)
"""
```

## Progressbar组件

```python
"""
    其他属性：
    orient:horizontal或vertical
    length:长或高度
    mode:determinate到终点后在从起点开始（默认） indeterminate起点终点之间
    maximum:最大值默认100
    value:当前值 当mode="indeterminate",value="maximum"时，指针在最右侧
    variable
    
    常用方法：
    start(interval=None) 开始递增，每隔interval时间，默认50ms
    step(amount=None) 步进值，默认1.0
    stop() 停止
"""
from tkinter.ttk import *
vari=IntVar()
vari.set(0)
pro=Progressbar(win,mode="determinate",variable=vari,max=50,length=200)
pro.grid(...)
vari.set(val)
pro.start()
pro.step(5)
pro.stop()
```

## Canvas组件

```python
canvas=Canvas(win,width=200,height=200,bg="#EFEFA3").pack()
"""
    属性：
    bd:边框宽 默认2px
    bg
    confine:如果默认True，画布不能滚动到可滑动区域外
    cursor
    height
    width
    highlightcolor:高亮边框颜色
    relief
    scrollregion:tuple(w,n,e,s) 左上右下可滑动最大区域
    xscrollincrement:水平/垂直滚动时，请求滚动的数量值
    yscrollincrement
    xscrollcommand:绑定水平/垂直滚动条
    yscrollcommand
"""
line1=canvas.create_line(x1,y1,x2,y2,...,xn,yn,...) #绘制线条
"""
    arrow:是否添加箭头，默认无，值：FIRST起始端右箭头 LAST末端右箭头 BOTH两端都有箭头
    arrowshap:(d1,d2,d3) 箭头形状，三角形底、斜边、高
    capstyle:终点样式，默认BUTT，其他:PROJECTING ROUND
    dash:(x1,x2) 设置为虚线，且值为x1px实线与x2px空白交替出现
    dashoffset:同dash x1,x2交换
    fill:颜色
    joinstyle:线条焦点颜色 ROUND BEVEL MITER
    stipple:绘制位图线条
    width:线宽
"""
rect=canvas.create_rectangle(x1,y1,x2,y2,...) #绘制矩形
canvas.move(rect,x1,x2) #将rect向右、下移动距离
"""
    属性：
    dash dashoffset stipple width
    outline:轮廓颜色
    fill:填充颜色
"""
cir1=canvas.create_oval(x1,y2,x2,y2,...) #绘制椭圆
arc1=canvas.create_arc(x1,y1,x2,y2,extend=120,style=ARC,...) #绘制圆弧 起点、终点、extend角度
    #style属性：ARD弧 CHORD弧+弦 PIESLICE扇形
arc2=canvas.create_arc(x1,y1,x2,y2,extent=120,start=startanfle,width=2,style=PIESLICE) #绘制扇形
poly1=canvas.create_polygon(x1,y1,x2,y2,...,...) #绘制多边形
text=canvas.create_text(x,y,text=str,...) #绘制文字
"""
    常用属性：font fill justify等
"""
canvas.delete("all") #清除指定内容，"all"表示所有
bird1=PhotoImage(file="*.png")
bird=canvas.create_image(x,y,image=bird1,...) #绘制图像
    #常用参数：anchor
#拖动效果：
    def draw(event):
        canvas.coords(bird,event.x,event.y) #改变坐标
    canvas.bind("<B1-Motion>",draw)
canvas.move(ID,x,y) #向右、下移动
```

## 事件

```python
#鼠标事件
"""
    <Button-1> 单击左键
    <Button-2> 单击中间
    <Button-3> 单击右键
    <Button-4> 向上滚轮
    <Button-5> 向下滚轮
    <B1-Motion> 按左键+拖动
    <B2-Motion> 按中间+拖动
    <B3-Motion> 按右键+拖动
    <ButtonRelease-1> 释放左键
    <ButtonRelease-2> 释放中间
    <ButtonRelease-3> 释放右键
    <Double-Button-1> 双击左键
    <Double-Button-2> 双击中间
    <Double-Button-3> 双击右键
    <Enter> 鼠标进入
    <Leave> 鼠标移出
"""

#键盘事件
"""
    <Return> 回车键
    <space> 空格键
    <Key> 按下某键，键值作为event传递
    <Shift-Up>
    <Alt-Up>
    <Control-Up>
"""

button=Button(win,text="...",command=...)
button.bind("<Button-1>",bg,add="+") #当add="+"为添加绑定 当add=""为替换
button.bind("<Button-1>",font,add="+")

label.unbind("<Button-1>")
```

## 文件对话框

```python
from tkinter.filedialog import *
file=askopenfilename(title="标题：选择文件",filetype=[("png格式图片文件","*.png")]) #选择单个文件
file=askopenfilenames(title="标题：选择文件",filetype=[("png格式图片文件","*.png")]) #可选多个文件
filetype=[('Python Files','*.py *.pyw'),('Text Files','*.txt'),('All Files','*.*')]
b=asksaveasfilename(defaultextension='.py',filetypes=filetype,initialdir='D:\\code',initialfile='Test',title"另存为")#保存文件 默认Test.py
b=askopenfile(title="打开文件",filetypes=[("text文本文件","*.txt")])
b.name #返回文件名
b=askdirectory(title="选择或新建文件夹")
```

