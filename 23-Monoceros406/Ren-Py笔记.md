---
title: Ren'Py笔记
date: 2023-10-15 20:50:22
tags: Ren'Py
mathjax: true
---

# Ren'Py笔记

```
script.rpy
options.rpy
gui.rpy
screens.rpy
```

```python
label start:
    ...
    return

menu:
    "...":
        jump game
    "...":
        jump book
label game:
    ...
label book:
    ...

default book=True
$ book=True
if book:
    ...
else:
    ...

say:
    "角色" "正在说的话"
    "旁白的话" #"用\"
    define s=Character('名字',color="#C8FFC8")
    s "正在说的话"

scene:
    scene bg meadow #bg meadow.png 只能JPG JPEG PNG WEBP
    scene bg meadow with fade
        #None 无
        #dissolve 溶解
        #fade 褪色

show:
    show sylvie green smile #sylvie green smile.png 只能PNG WEBP
    show sylvie green smile with fade #同scene
    show sylvie green smile at right
        #left 左
        #right 右
        #center 水平居中 图像底部与界面底部相接
        #truecenter 水平垂直居中

hide:
    hide sylvie

play:
    play music "..." #opus ogg-vorbis mp3
    play music "..." fadeout 1.0 fadein 1.0
    play music illurock #默认game/audio/illurock.ogg
    play sound "..." #不会循环

queue:
    queue music "..."

stop:
    stop music
    stop music fadeout 1.0

pause:
    pause #等待单击
    pause 3.0 #暂停对应秒数
```

## Options.rpy

```python
define config.name=_('...')#游戏标题
define gui.show_name=False#画面主菜单中隐藏标题、版本
define config.version="1.0"#版本
define gui.about=_("...")#关于界面的附加信息，\n\n换行，也用"""可实现多行
```

```
按钮分类：
    button 基础按钮，对用户行为引导
    choice_button 游戏内菜单的单项选择按钮
    quick_button 游戏内快速进入游戏菜单按钮
    navigation_button 主、游戏菜单用于引导至其他界面、开始游戏按钮
    page_button 读、存档界面用于翻页按钮
    slot_button 存档槽位按钮（缩略图、时间、可选名字）
    radio_button 多组单项选择按钮
    check_button 勾选项按钮
    test_button 环境设定界面上用于音频回放的按钮，垂直高度与滑块一致
    help_button 选择需要何种帮助的按钮
    confirm_button 选择“是”或“否”确认界面按钮
    nvl_button NVL模式下菜单选项的按钮
```

```
GUI图像放在game/gui下
gui/main_menu.png 主菜单
gui/game_menu.png 游戏菜单
    关于界面背景两种都可以
gui/window_icon.png 运行时窗口图标
gui/textbox.png 文本框
#文件名前加按钮名称前缀可用于对应类型按钮，如check_*.png
gui/button/choice_idle_background.png 未取到焦点选项按钮背景
gui/button/choice_hover_background.png 取到焦点选项按钮背景
gui/button/idle_background.png 未获取焦点按钮背景
gui/button/hover_background.png 获取焦点按钮背景
gui/button/selected_idle_background.png 被选择、未获取焦点按钮背景（必须存在gui/button/idle_background.png）
gui/button/selected_hover_background.png 被选择、获取焦点按钮背景（必须存在gui/button/hover_background.png）
gui/button/check或radio_foreground.png check button或radio button未被选择的选项
gui/button/check或radio_selected_foreground.png check button或radio button被选中的选项
gui/button/slot_idel_background.png 未获焦点存档槽位背景
gui/button/slot_hover_background.png 获焦点存档槽位背景
gui/overlay/main_menu.png 主菜单叠加
gui/overlay/game_menu.png 游戏菜单叠加图片
gui/overlay/confirm.png 选择确认界面暗化背景的叠加图片
gui/frame.png 主Frame窗口背景
空闲和指针悬停状态下垂直或水平滑块的背景图片:
    gui/slider/horizontal_idle_bar.png
    gui/slider/horizontal_hover_bar.png
    gui/slider/vertical_idle_bar.png
    gui/slider/vertical_hover_bar.png
用于Bar的Thumb图片：
    gui/slider/horizontal_idle_thumb.png
    gui/slider/horizontal_hover_thumb.png
    gui/slider/vertical_idle_thumb.png
    gui/slider/vertical_hover_thumb.png
未获取焦点、鼠标悬停状态下垂直滚动条背景图片：
    gui/scrollbar/horizontal_idle_bar.png
    gui/scrollbar/horizontal_hover_bar.png
    gui/scrollbar/vertical_idle_bar.png
    gui/scrollbar/vertical_hover_bar.png
Scrollbar的Thumb图片：
    gui/scrollbar/horizontal_idle_thumb.png
    gui/scrollbar/horizontal_hover_thumb.png
    gui/scrollbar/vertical_idle_thumb.png
    gui/scrollbar/vertical_hover_thumb.png
水平和垂直Bar的填充图片：
    gui/bar/left.png
    gui/bar/bottom.png
    gui/bar/right.png
    gui/bar/top.png
gui/skip.png 跳过提示背景图
gui/notify.png 通知提示背景图
gui/namebox.png 名字框
```



## gui.rpy

```python
define gui.text_size=22#字号 默认22
define gui.text_color="#402000"#对话文本颜色
define gui.text_font="*.ttf"#对话、菜单、输入等字体，*.ttf放在game中
define gui.text_size=33#对话文本字号
define gui.name_text_size=45#角色名字号
define gui.textbox_height=278#文本框高，与gui/textbox.png高度一致
define gui.choice_button_text_idle_color='#888888'#未取到焦点选项按钮文本颜色
define gui.choice_text_hover_color='#0066CC'#取到焦点选项按钮文本颜色
define gui.accent_color='#000060'#标题、标签强调色
define gui.idle_color='#606060'#大多数按钮未获取焦点、未被选择时颜色
define gui.idle_small_color='#404040'#鼠标悬停在存档槽日期名字、快捷菜单按钮文字颜色
define gui.hover_color='#3284D6'#鼠标悬停的按钮文本、滑块、滚动条
define gui.selected_color='#555555'#被选择的按钮文本
define gui.insensitive_color='#8888887F'#不接受用户输入的按钮文本
define gui.interface_text_color='#404040'#帮助关于文本
define gui.muted_color='#6080D0'
define gui.hover_muted_color='#8080F0'#当重新生成图片，启动器图片无法马上生效时，条、滚动条、滑块无法正确战术数值或可视区域某些部分颜色
define gui.interface_text_font="*.ttf"#主菜单、游戏菜单、按钮
define gui.glyph_font="*.ttf"#跳过提示的箭头标志
define gui.interface_text_size=36#按钮文本默认字号
define gui.label_text_size=45#标签部分文本字号
define gui.notify_text_size=24#通知文本字号
define gui.title_text_size=75#游戏标题字号
define gui.frame_borders=Borders(15,15,15,15)# Borders(left,top,right,bottom)
define gui.confirm_frame_borders=Borders(60,60,60,60)#确认提示界面Frame
define gui.frame_title=True#确认提示界面边条、中心被True:复制码放；False:拉伸
#button前加上前缀可配置特定种类按钮，如：navigation_button_...
define gui.button_width=None
define gui.button_height=64#按钮宽度、高度，如果为None，系统基于Borders和文本尺寸自己决定
define gui.button_borders=Borders(10,10,10,10)#按钮borders
define gui.button_tile=True#True为Tile形式码放，False则使用缩放
define gui.button_text_font=gui.interface_font
define gui.button_text_size=gui.interface_text_size
define gui.button_text_idle_color=gui.idle_color
define gui.button_text_hover_color=gui.hover_color
define gui.button_text_selected_color=gui.accent_color
define gui.button_text_insensitive_color=gui.insensitive_color
define gui.button_text_xalign=0.0#垂直方向对齐方式，0.0左对齐 0.5中央对齐 1.0右对齐
define gui.button_image_extension=".png"#按钮图像扩展名 可以.webp
define gui.slot_button_width=414
define gui.slot_button_height=309#存档槽位按钮宽度高度
define gui.slot_button_borders=Borders(15,15,15,15)#存档槽位Borders
define config.thumbnail_width=384
define config.thumbnail_height=216#存档缩略图宽高
define gui.file_slot_cols=3
define gui.file_slot_rows=2#存档槽位行列数
define gui.slider_size=64#水平滑动块的高度或垂直滑块的宽度
define gui.slider_tile=True#True:Tile码放，False:缩放
define gui.slider_borders=Borders(6,6,6,6)
define gui.vslider_borders(6,6,6,6)#Frame包含Bar图片时使用的Borders
define gui.scrollbar_size=24#水平滚动条的高度，垂直滚动条的宽度
define gui.scrollbar_tile=True
define gui.scrollbar_borders=Borders(10,6,10,6)
define gui.vscrollbar_borders=Borders(10,6,10,6)
define gui.unscrollable="hide"#无法滚动时隐藏
define gui.bar_tile=False#True:Tile码放 False:Scale缩放
define gui.bar_border=Borders(10,10,10,10)
define gui.vbar_borders=Borders(10,10,10,10)
define gui.skip_frame_borders=Borders(24,8,75,8)
define gui.notify_frame_borders=Borders(24,8,60,8)
define gui.skip_ypos=15#从窗口顶算起，跳过提示的垂直位置
define gui.notify_ypos=68#从窗口顶算起，通知提示的垂直位置
define gui.name_xpos=360
define gui.name_ypos=0#距离左侧、顶部距离，居中可填0.5、像素数、可负数
define gui.name_xalign=0.0#水平对齐，0,0左对齐，0.5居中，1.0右对齐
define gui.namebox_width=None
define gui.namebox_height=None
define gui.namebox_borders=Borders(5,5,5,5)
define gui.namebox_tile=False
define gui.dialogue_xpos=402
define gui.dialogue_ypos=75
define gui.dialogue_width=1116#每行内容最大宽度(px)
define gui.dialogue_text_xalign=0.0
define gui.history_height=210#历史层高
```

未完待续...
