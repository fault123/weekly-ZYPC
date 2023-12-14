---
title: Delphi开发指南
date: 2023-10-31 10:51:52
tags: Delphi
mathjax: true
---

# Delphi开发指南

## 常用系统函数与过程

### 数值运算函数

| 数学运算函数                                                 | 参数类型      | 函数值类型    | 功能描述                               |
| ------------------------------------------------------------ | ------------- | ------------- | -------------------------------------- |
| Abs(x)                                                       | Integer或Real | Integer或Real |                                        |
| Arctan(x)、Cos(x)、Sin(x)、Pi、Sqrt(x)、Power(x,y)、Ln(x)、Log10(x)、Exp(x)、Frac(x)、Int(x) | Real          | Real          |                                        |
| Round(x)、Trunc(x)                                           | Real          | Int64         |                                        |
| Odd(x)                                                       | Integer       | Boolean       |                                        |
| Random[(x)]                                                  | Integer       | 不定          | 有参数返回$[0,x)$，无参数返回$[0,1)$。 |

### 字符处理函数

| 函数/过程          | 功能描述                                      |
| ------------------ | --------------------------------------------- |
| UpperCase(s)       |                                               |
| LowerCase(s)       |                                               |
| CompareStr(s1,s2)  | 区分大小写，s1>s2返回值>0，否则<0，相等=0。   |
| CompareText(s1,s2) | 不区分大小写，同上。                          |
| Concat(s1,s2,...)  |                                               |
| Pos(s1,s)          | s1在s中起始位置，不包含返回0。                |
| Length(s)          |                                               |
| Copy(s,n,m)        | 截取s中从n开始，m个字符长的子字符串。         |
| IntToStr(x)        |                                               |
| FloatToStr(x)      |                                               |
| StrToInt(s)        |                                               |
| StrToFloat(s)      |                                               |
| IntToHex(d,h)      |                                               |
| Chr(x)             |                                               |
| Format(s,x)        | 例如：`Format('s% abc d%',['xxx',98])`        |
| Str(x,s)           | x转换成字符串放入参数v。                      |
| V(s,v,c)           | s转换成数值放入v，根据c的值判断是否转换成功。 |

### 日期时间函数

| 函数/过程                          | 参数类型  | 函数值类型 | 功能描述                                                     |
| ---------------------------------- | --------- | ---------- | ------------------------------------------------------------ |
| Now                                |           | Double     | 整数部分表示从1899年12月30日以来经过的天数，小数表示经过的时间与24小时之比。 |
| Date                               |           | TdateTime  |                                                              |
| Time                               |           | TdateTime  |                                                              |
| DateToStr(date)                    | TdateTime | String     |                                                              |
| TimeToStr(time)                    | TdateTime | String     |                                                              |
| DateTimeToStr(datetime)            | TdateTime | String     |                                                              |
| EncodeDate(year,month,day)         | Integer   | TdateTime  |                                                              |
| EncodeTime(hour,min,sec,msec)      | Integer   | TdateTime  |                                                              |
| DayOfWeek(date)                    |           | Integer    | 返回整数1~7，1星期日，7星期六                                |
| FormatDateTime(format,datetime)    | TdateTime | String     |                                                              |
| Decodedate(date,year,month,day)    |           |            |                                                              |
| DecodeTime(Time,hour,min,sec,msec) |           |            |                                                              |

### 顺序函数

| 函数    | 功能描述     |
| ------- | ------------ |
| Ord(x)  | x的序数      |
| Pred(x) | x的前驱值    |
| Succ(x) | x的后驱值    |
| Low(x)  | 第一个元素   |
| High(x) | 最末一个元素 |

## TForm方法

### Create

创建窗体，引发OnCreate事件。需要用Show使之可见。

### Close

关闭一个显示中的窗体，调用CloseQuery判断是否可以关闭，可以则引发OnClose事件关闭窗体。

### CloseQuery

判断是否可以被关闭，返回Boolean。

### Release

从内存中彻底清除。

### Show

显示窗体，引发OnShow事件。

### ShowModal

显示一个模式窗体，引发OnShow事件。

### Print

打印窗体。

## 弹出对话框

### ShowMessage()

```pascal
ShowMessage(<信息内容>);
```

使用硬回车（#13），高度宽度随内容增加而增加。

### ShowMessageFmt()

```pascal
ShowMesage(<信息内容>,<参数组>);
```

### MessageDlg

```pascal
<Word变量>=MessageDlg(<信息内容>,<类型>,<按钮组>,HelpCtx);
```

<类型>取值：

| 取值           | 说明                     |
| -------------- | ------------------------ |
| mtWarning      | 警告                     |
| mtError        | 错误                     |
| mtInformation  | 信息                     |
| mtConfirmation | 确认                     |
| mtCustom       | 不含图标，标题为程序名称 |

<按钮组>取值：

| 取值               | 返回值 |
| ------------------ | ------ |
| mbYes              | 6      |
| mbNo               | 7      |
| mbOK               | 1      |
| mbCancel           | 2      |
| mbHelp             |        |
| mbAbort            | 3      |
| mbRetry            | 4      |
| mbIgnore           | 5      |
| mbAll              | 8      |
| mbNoToAll          | 9      |
| mbYesToAll         | 10     |
| mbYesNoCancel      |        |
| mbOKCancel         |        |
| mbAbortRetryIgnore |        |

HelpCtx指定单击Help或按F1键时显示的帮助主题。

### MessageDlgPos

同上，多了两个参数X、Y，指定显示位置坐标。

### CreateMessageDialog

```psacal
<TForm变量>=CreateMessageDialog(<信息内容>,<类型>,<按钮组>);
```

需要使用窗体ShowModal方法弹出。

### InputBox

```pascal
<变量>=InputBox(<对话框标题>,<信息内容>,<默认内容>);
```

## 实例

```pascal
procedure TForm1.Button1Click(Sender:TObject);
var
	sum:int64;
	StrMsg:string;
begin
	if(Sender as TButton).Tag=1 then
	begin
		sum:=StrToInt(Edit1.Text)+StrToInt(Edit2.Text);
		StrMsg:=IntToStr(sum);
		ShowMessage(StrMsg);
	end
	else
		if(Sender as TButton).Tag=2 then
		begin
			sum:=StrToInt(Edit1.Text)*StrToInt(Edit2.Text);
			StrMsg:=IntToStr(sum);
			ShowMessage(StrMsg);
		end;
end;
procedure TForm1.Button2Click(Sender:TObject);
begin
	Form1.ButtonClick(Sender);
end;
procedure TForm1.Button3Click(Sender.TObject);
begin
	close;
end;
```
