---
title: C#笔记
date: 2023-10-31 14:15:31
tags: C#
mathjax: true
---

# C#笔记

##  入门

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
namespace Test{
    class Program{
        static void Main(string[] args){
            string s=string.Empty;
            Console.Title="Title"; //控制台标题
            Console.WriteLine("Hello World!");
            s=Console.ReadLine();
        }
    }
}
```

### Char类常用方法

IsDigit IsLetter IsLetterOrDigit IsLower IsNumber IsPunctuation IsSeparator IsUpper IsWhiteSpace Parse ToLower ToString ToUpper TryParse

### 多级转义

```csharp
Console.WriteLine(@"..."); //不需要转义字符进行转义
```

### 动态常量

```csharp
class Program{
    readonly int Price;
    Programe(){
        Price=368;
    }
    static void Main(string[] args){}
}
```

### Convert类常用方法

ToBoolean ToByte ToChar ToDateTime ToDecimal ToDouble ToInt32 ToInt64 ToSByte ToSingle ToString ToUInt32 ToUInt64

## 数组

### 一维数组

```csharp
int[] arr=new int[5];
for(int i=0;i<arr.Length;i++){
    arr[i]=i+1;
}
```

### 二维数组

以下两种方法均可。

```csharp
int[,] a=new int[2,4];

int[][] a=new int[2][];
a[0]=new int[2];
a[1]=new int[3];
```

### foreach

```csharp
string[] roles={"a","b",...};
foreach(string role in roles){
    Console.WriteLine(role+" ");
}
```

### List

```csharp
List<string> list=new List<string>();
list.Add("...");
```



### Sort

从小到大，只能为一维数组。

```csharp
public static void Sort(Array array)
public static void Sort(Array array,int index,int length)
/*
    index:排序范围内的起始索引
    length:排序范围内的元素数
*/

int[] arr=new int[]{3,9,27,6,18,12,21,15};
Array.Sort(arr);
```

### Reverse

用法同上。

## 字符串

### IndexOf

```csharp
public int IndexOf(char value)
public int IndexOf(string value)
public int IndexOf(char value,int startIndex)
public int IndexOf(string value,int startIndex)
public int IndexOf(char value,int startIndex,int count)
public int IndexOf(string value,int startIndex,int count)
/*
    value:要检索的字符或字符串
    startindex:搜索起始位置
    count:要检查的字符位置数
*/

string str="We are the world";
int size=str.IndexOf('e'); //1

string str="We are the world";
int firstIndex=str.IndexOf("r");
int secondIndex=str.IndexOf("r",firstIndex+1);
int thirdIndex=str.IndexOf("r",secondIndex+1);
```

### LastIndexOf

用法同上。

### StartsWith

```csharp
public bool StartsWith(string value)
public bool StartsWith(string value,bool ignoreCase,CultureInfo culture)
/*
    value:要比较的字符串
    ignoreCase:忽略大小写为true，否则false
    culture:确定如何对字符串与value进行比较的区域性信息
*/

string str="Keep on ...";
bool result=str.StartsWith("keep",true,null);
```

### Equals

```csharp
public bool Equals(string value)
public static bool Equals(string a,string b)

string pwd=Console.ReadLine();
if(pwd.Equals("aaa")){
    //...
}
```

### ToUpper/ToLower

```csharp
Console.WriteLine(str.ToUpper());
Console.WriteLine(str.ToLower());
```

### Format

```csharp
Console.WriteLine(string.Format("aaa{0:D}",1234));//1234
Console.WriteLine(string.Format("aaa{0:C}",5201));//货币形式 不包括小数 0表示第1个参数 ￥5,201.00
Console.WriteLine(string.Format("aaa{0:E}",120000.1));//科学计数法 1.200001E+005
Console.WriteLine(string.Format("aaa{0:N0}",12800));//分隔符显示 第2个0表示保留0位小数点 12,800
Console.WriteLine(string.Format("aaa{0:F2}",Math.PI));//小数点后2位 3.14
Console.WriteLine(string.Format("aaa{0:X4}",33));//16进制 0021
Console.WriteLine(string.Format("aaa{0:P0}",0.01))//百分号 1%
int money=1298;
Console.WriteLine(money.ToString("C"));
    
DateTime strDate=DateTime.Now;
Console.WriteLine(string.Format("{0:d}",strDate));//短日期
Console.WriteLine(string.Format("{0:D}",strDate));//长日期
Console.WriteLine(string.Format("{0:f}",strDate));//长日期短时间
Console.WriteLine(string.Format("{0:F}",strDate));//长日期长时间
Console.WriteLine(string.Format("{0:g}",strDate));//短日期短时间
Console.WriteLine(string.Format("{0:G}",strDate));//短日期短时间
Console.WriteLine(string.Format("{0:M}",strDate));//月日 或m
Console.WriteLine(string.Format("{0:t}",strDate));//短时间
Console.WriteLine(string.Format("{0:T}",strDate));//长时间
Console.WriteLine(string.Format("{0:Y}",strDate));//年月 或y
DateTime dTime=DateTime.Now;
Console.WriteLine(dTime.ToString("Y"));
```

### Substring

```csharp
public string Substring(int startIndex);
public string Substring(int stratIndex,int length);

string strFile="Program.cs";
string strFileName=strFile.Substring(0,strFile.IndexOf('.'));
string strExtension=strFile.Substring(strFile.IndexOf('.'));
```

### Split

```csharp
char[] separator={','};
string[] splitStrings=str.Split(separator,StringSplitOptions.RemoveEmptyEntries);
for(i=0;i<splitStirng.Length;i++){
    Console.WriteLine(splitStirng[i]);
}
```

### Trim

```csharp
string shortStr=str.Trim();
```

### Replace

```csharp
string strOld="...";
string strNew1=strOld.Replace(',','*');
string strNew2=strOld.Replace("one","One");
```

### StringBuilder

```csharp
StringBuilder SBuilder=new StringBuilder("...");
int Num=368;
SBuilder.Append("...");//追加
SBuilder.AppendFormat("{0:C0}",Num);
SBuilder.Insert(0,"...");//开头追加
SBuilder.Remove(14,SBuilder.Length-14);//删除索引14以后的字符串
SBuilder.Replace("...","...");
Console.WriteLine(SBuilder);
```

## Windows控件

### Label

```csharp
label1.Text="...";
label1.Visible=true;
```

### TextBox

```csharp
textBox1.ReadOnly=true;
textBox1.PasswordChar='*';
textBox1.Multiline=true;
label1.Text=textBox1.Text;
```

### MessageBox

```csharp
DialogResult ret=MessageBox.Show("aaa","bbb",MessageBoxButtons.YesNo,MessageBoxIcon.Warning);
```

MessageBoxButtons枚举值：MessageBoxButtons.OK MessageBoxButtons.OKCancel MessageBoxButtons.AbortRetryIgnore MessageBoxButtons.YesNoCancel MessageBoxButtons.YesNo MessageBoxButtons.RetryCancel

MessageBoxIcon枚举值：MessageBoxIcon.None MessageBoxIcon.Question MessageBoxIcon.Exclaimation MessageBoxIcon.Asterisk MessageBoxIcon.Stop MessageBoxIcon.Error MessageBoxIcon.Warning MessageBoxIcon.Information

DialogResult枚举值：DialogResult.None DialogResult.OK DialogResult.Cancel DialogResult.Abort DialogResult.Retry DialogResult.Ignore DialogResult.Yes DialogResult.No

### OpenFileDialog

```csharp
openFileDialog1.InitialDirectory="C:\\";//初始目录
openFileDialog1.Filter="bmp文件(*.bmp)|*.bmp|gir文件(*.gif)|*.gif";//文件名筛选器
openFileDialog1.ShowDialog();
string strName=openFileDialog1.Filename;
```

| 其他属性         | 描述                                       |
| ---------------- | ------------------------------------------ |
| AddExtension     | 用户省略扩展名时是否在文件名中添加扩展名。 |
| DefaultExt       | 默认文件扩展名。                           |
| FileNames        | 获取所有选定文件的文件名。                 |
| Multiselect      | 是否允许选择多个文件。                     |
| RestoreDirectory | 对话框关闭前是否还原当前目录。             |

| 其他方法 | 描述                       |
| -------- | -------------------------- |
| OpenFile | 以只读模式打开选择的文件。 |

### SaveFileDialog

同上。

### FolderBrowserDialog

```csharp
folderBrowserDialog1.ShowNewFolderButton=false;//不显示新建文件夹按钮
if(folderBrowserDialog1.ShowDialog()==DialogResult.OK){
    textBox1.Text=folderBrowserDialog1.SelectedPath;
}
```

| 其他属性    | 说明                 |
| ----------- | -------------------- |
| Description | 显示的说明文本。     |
| RootFolder  | 开始浏览的根文件夹。 |

## I/O

```csharp
using System.IO;
```

### File

#### Exists

确定指定的文件是否存在。权限不够也返回false。

```csharp
bool ret=File.Exists("C:\\Test.txt");
```

#### Create

创建文件。

```csharp
File.Create("C:\\Test.txt");
```

#### Copy

```csharp
File.Copy("C:\\Test.txt","D:\\Test.txt");
```

#### Move

```csharp
File.Move("C:\\Test.txt","D:\\Test.txt");
```

#### Delete

```csharp
File.Delete("C:\\Test.txt");
```

### FileInfo

#### Exists

```csharp
FileInfo finfo=new FileInfo("C:\\Test.txt");
if(finfo.Exists){
    //...
}
```

#### Create

```csharp
FileInfo finfo=new FileInfo("C:\\Test.txt");
finfo.Create();
```

#### CopyTo

```csharp
FileInfo finfo=new FileInfo("C:\\Test.txt");
finfo.CopyTo("D:\\Test.txt",true);//true表示存在则改写
```

#### MoveTo

```csharp
FileInfo finfo=new FileInfo("C:\\Test.txt");
finfo.MoveTo("D:\\Test.txt");
```

#### Delete

```csharp
FileInfo finfo=new FileInfo("C:\\Test.txt");
finfo.Delete();
```

#### 详细信息

```csharp
private void button1_Click(object sender,EventArgs e){
    if(openFileDialog1.ShowDialog()==DialogResult.OK){
        textBox1.Text=openFileDialog1.Filename;
        FileInfo finfo=new FileInfo(textBox1.Text);
        string strCTime=finfo.CreateionTime.ToShortDateString();
        string strLATime=finfo.LastAccessTime.ToShortDateString();
        string strLWTime=finfo.LastWriteTime.ToShortDateString();
        string strName=finfo.Name;
        string strFName=finfo.FullName;
        string strDName=finfo.DirectoryName;
        string strISRead=finfo.IsReadOnly.ToString();
        long lgLength=finfo.Length;
    }
}
```

### Directory

#### Exists

```csharp
Directory.Exists("C:\\Test");
```

#### CreateDirectory

```csharp
DirectoryInfo.CreateDirectory("C:\\Test")
```

#### Move

只能在同一盘根目录下。

```csharp
Directory.Move("C:\\Test","C:\\aaa\\Test");
```

#### Delete

```csharp
Directory.Delete("C:\\Test");
```

### DirectoryInfo

#### Exists

```csharp
DirectoryInfo dinfo=new DirectoryInfo("C:\\Test");
if(dinfo.Exists){
    //...
}
```

#### CreateDirectory

```csharp
DirectoryInfo dinfo=new DirectoryInfo("C:\\Test");
dinfo.Create();
```

#### MoveTo

只能在同一盘根目录下。

```csharp
DirectoryInfo dinfo=new DirectoryInfo("C:\\Test");
dinfo.MoveTo("C:\\aaa\\Test");
```

#### Delete

```csharp
DirectoryInfo dinfo=new DirectoryInfo("C:\\Test");
dinfo.Delete();
```

### FileSystemInfo

```csharp
DirectoryInfo dinfo=new DirectoryInfo("...");
FileSystemInfo[] fsinfos=dinfo.GetFileSystemInfos();
foreach(FileSystemInfo fsinfo in fsinfos){
    if(fsinfo is DirecotryInfo){
        DirectoryInfo dirinfo=new DirectoryInfo(fsinfo.Fullname);
        //...
    }
    else{
        FileInfo finfo=new FileInfo(fsinfo.FullName);
        //...
    }
}
```

### StreamWriter/StreamReader

```csharp
StreamWriter sw=new StreamWriter("...",true);
sw.WriteLine("...");
sw.Close();

StreamReader sr=new StreamReader("...");
string str=sr.ReadToEnd();
sr.Close();
```

## GDI+

### Image

```csharp
private void Form1_Paint(object sender,PaintEventArgs e){
    Image myImage=Image.FromFile("*.jpg");
    Graphics myGraphics=this.CreateGraphics();
    myGraphics.DrawImage(myImage,50,20,90,92);
}
```

### Bitmap

绘制出的图像可以自动刷新。

```csharp
Bitmap bmp=new Bitmap(120,80);
Graphics g=Graphics.FromImage(bmp);
Pen myPen=new Pen(Color.Green,3);
g.DrawEllipse(myPen,50,10,120,80);
this.BackgroundImage=bmp;
```

## Socket

```csharp
using System.Net;
```

### Dns/IPAddress/IPHostEntry

```csharp
string localname=Dns.GetHostName();//获取本机名
IPAddress[] ips=Dns.GetHostAddresses(localname);//获取本机所有IP
foreach(IPAddress ip in ips)
    if(!ip.IsIPv6SiteLocal)//不是IPv6地址
        localip=ip.ToString();//获取本机IP地址

try{
    IPHostEntry host=Dns.GetHostEntry("192.168.1.50");
    name=host.HostName.ToString();//目标主机名
}
catch(Exception ex{
    MessageBox.Show(ex.Message);
}
```

### TcpClient/TcpListener实战

Server:

```csharp
namespace Server{
    class Program{
        static void Main(){
            int port=888;
            TcpClient tcpClient;
            IPAddress[] serverIP=Dns.GetHostAddresses("127.0.0.1");
            IPAddress localAddress=serverIP[0];
            TcpListener tcpListener=new TcpListener(localAddress,port);
            tcpListener.Start();
            //服务器启动成功
            while(true){
                try{
                    tcpClient=tcpListener.AcceptTcpClient();
                    NetworkStream networkStream=tcpClient.GetStream();
                    BinaryReader reader=new BinaryReader(networkStream);
                    BinaryWriter writer=new BinaryWriter(networkStream);
                    while(true){
                        try{
                            string strReader=reader.ReadString();
                            string[] strReaders=strReader.Split(new char[]{' '});//strReaders[0]为客户端IP [1]为消息
                            //...
                            writer.Write("...");//发送消息
                        }
                        catch{
                            break;
                        }
                    }
                }
                catch{
                    break;
                }
            }
        }
    }
}
```

Client:

```csharp
namespace Client{
    class Program{
        static void Main(string[] args){
            TcpClient tcpClient=new TcpClient();
            tcpClient.Connect("127.0.0.1",888);//连接服务器IP地址及端口
            if(tcpClient!=null){
                //连接成功
                NetworkStream networkStream=tcpClient.GetStream();
                BinaryReader reader=new BinaryReader(networkStream);
                BinaryWriter writer=new BinaryWriter(networkStream);
                string localip="127.0.0.1";
                IPAddress[] ips=Dns.GetHostAddresses(Dns.GetHostName());
                foreach(IPAddress ip in ips)
                    if(!ip.IsIPv6SiteLocal)
                        localip=ip.ToString();
                writer.Write(localip+" ...");//发送消息
                while(true){
                    try{
                        string strReader=reader.ReadString();
                        if(strReader!=null)
                            Console.WriteLine(strReader);//接收到的消息
                    }
                    catch{
                        break;
                    }
                }
            }
            //连接失败
        }
    }
}
```

### UdpClient

Server:

```csharp
namespace Server{
    class Program{
        static UdpClient udp=new UdpClient();
        static void Main(string[] args){
            udp.Connect("127.0.0.1",888);
            while(true){
                Thread thread=new Thread(()=>{
                    try{
                        Byte[] sendBytes=Encoding.Default.GetBytes("("+DateTime.Now.ToLongTimeString()+")...");//发送消息
                        udp.Send(sendBytes,sendByes.Length);
                    }
                    catch(Exception ex){
                        Console.WriteLine(ex.Message);
                    }
                });
                thread.Start();
                Thread.Sleep(1000);
            }
        }
    }
}
```

Client:

```csharp
namespace Client{
    public partial class Form1:Form{
        public Form1(){
            InitializeComponent();
            CheckForIllegalCrossThreadCalls=false;//其他线程中可以调用主窗体控件
        }
        bool flag=true;
        UdpClient udp;
        Thread thread;
        private void button1_Click(object sender,EventArgs e){
            udp=new UdpClient(888);
            flag=true;
            IPEndPoint ipendpoint=new IPEndPoint(IPAddress.Any,888);
            thread=new Thread((=>{
                while(flag){
                    try{
                        if(udp.Available<=0)//是否有网络数据
                            continue;
                        if(udp.Client==null)
                            return;
                        byte[] bytes=udp.Recieve(ref ipendpoint);
                        string str=Encoding.Default.GetString(bytes);//接收到的消息
                    }
                    catch(Exception ex){
                        MessageBox.Show(ex.Message);
                    }
                    Thread.Sleep(1000);
                }
            }));
            thread.Start();
        }
        private void button2_Click(object sender,EventArgs e){
            flag=false;
            if(thread.ThreadState==ThreadState.Running)
                thread.Abort();
            udp.Close();
        }
    }
}
```

## Thread

```csharp
using System.Threading;
```

### Start

```csharp
public partial class Form1:Form{
    public Form1(){
        InitializeComponent();
        CheckForIllegalCrossThreadCalls=false;
    }
    void Roll(){
        Thread.Sleep(1000);//线程休眠1000ms
        th2.Join();//线程th1暂停th2开始
        //th2.Join(1000); 等待线程th2退出或1000ms后自动退出
        lock(this){//线程同步法一
            //...
        }
        Monitor.Enter(this);//法二
        //...
        Monitor.Exit(this);
        Mutex myMutex=new Mutex(this);//法三
        myMutex.WaitOne();
        //...
        myMutex.ReleaseMutex();
        //...
    }
    private void Form1_Load(object sender,EventArgs e){
        Thread th1=new Thread(new ThreadStart(Roll));
        th1.Name="线程一";
        th1.Priority=ThreadPriority.Lowest;//BelowNormal Normal AboveNormal Highest
        th1.Start();
        //Thread th2=...;
        //th2.Start();
        th1.Suspend();//挂起
        th1.Resume();//恢复挂起的进程
    }
     private void Form1_FormClosing(object sender,EventArgs e){
         if(th1.ThreadState==ThreadState.Running)
             th1.Abort();
     }
}
```

