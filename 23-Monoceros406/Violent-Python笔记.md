---
title: Violent_Python笔记
date: 2023-10-15 21:06:56
tags: Python
mathjax: true
---

# Violent Python笔记

本笔记为Python2代码。

## 入门

### 抓取banner

```python
import socket
socket.setdefaulttimeout(2)
s=socket.socket()
s.connet(("192.168.95.148",21))
ans=s.recv(1024)#1024B数据
```

### UNIX口令破解机

```python
import crypt
crypt.crypt("egg","HX")
```

### zip口令破解机

```python
import zipfile
zFile=zipfile.ZipFile("*.zip")
try:
    zFile.extractall(pwd="...")
except Exception,e:
    print e
```

### 多线程

```python
from threading import Thread
def extractFile(zFile,password):
    #...
def main():
    passFile=open('*.txt')
    for line in passFile.readlines():
        t=Thread(target=extractFile,args=(zFile,password))
        t.start()
if __name__=='__main__'
    main()
```

### 命令行提示

```python
import optparse
parser=optparse.OptionParser("usage%prog"+"-f <zipfile> -d <dictionary>")
parser.add_option('-f',dest='zname',type='string',help='specify zip file')
parser.add_option('-d',dest='dname',type='string',help='specify dictionary file')(options,args)=parse_args()
if (options.zname==None)|(options.dname==None):
    print parser.usage
    exit(0)
else:
    zname=options.zname
    dname=options.dname
```

## 渗透测试

### 端口扫描器+信号量

```python
import socket
from socket import *
from threading import *
screenLock=Semaphore(value=1)
def connScan(tgtHost,tgtPort):
    try:
        connSkt=socket(AF_INET,SOCK_STREAM)
        connSkt.connet((tgtHost,tgtPort))
        connSkt.send('ViolentPython\r\n')
        results=connSkt.recv(100)
        screenLock.acquire()
        #该tcp端口开放
    except:
        screenLock.acquire()
        #未开放
    finally:
        screenLock.release()
        connSkt.close()
def portScan(tgtHost,tgtPorts):
    try:
        tgtIP=gethostbyname(tgtHost)
    except:
        #无法识别主机
        return
    try:
        tgtName=gethostbyaddr(tgtIP)
        #能得到主机名
    except:
        #只能得到IP
    setdefaulttimeout(1)
    for tgtPort in tgtPorts:
        connScan(tgtHost,int(tgtPort))
```

### nmap

```python
import nmap
def nmapScan(tgtHost,tgtPort):
    nmScan=nmap.PortScanner()
    nmScan.scan(tgtHost,tgtPort)
    state=nmScan[tgtHost]['tcp'][int(tgtPort)]['state']
def main():
    for tgtPort in tgtPorts:
        nmapScan(tgtHost,tgtPort)
```

## 取证

### 分析无线访问热点

```bash
reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged" /s
```

脚本：

```python
//python3
from winreg import *
import ctypes
def val2addr(val):
    addr=''
    if val==None:
        return "Unknow"
    for ch in val:
        addr+='%02x'%ch
    addr=addr.strip(' ').replace(' ',':')[0:17]
    return addr
def printNets():
    net="SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\NetworkList\\Signatures\\Unmanaged"
    key=OpenKey(HKEY_LOCAL_MACHINE,net)
    print('\n[*]Networks you have joined.')
    for i in range(100):
        try:
            guid=EnumKey(key,i)
            netKey=OpenKey(key,str(guid))
            (n,addr,t)=EnumValue(netKey,5)
            (n,name,t)=EnumValue(netKey,4)
            macAddr=val2addr(addr)
            netName=str(name)
            print('[+]'+netName+'/'+macAddr)
            CloseKey(netKey)
        except:
            break
def main():
    printNets()
if __name__=="__main__":
    main()
```

### 恢复回收站内容

```python
import os
import optparse
from _winreg import *
def sid2user(sid):
    try:
        key=OpenKey(HKEY_LOCAL_MACHINE,"SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList"+'\\'+sid)
        (value,type)=QueryValueEx(key,'ProfileimagePath')
        user=value.split('\\')[-1]
        return user
   except:
    	return sid
def returnDir():
    return 'C:\\$Recycle.Bin\\'
def findRecycled(recycleDir):
    dirList=os.listdir(recycleDir)
    for sid in dirList:
        files=os.listdir(recycleDir+sid)
        user=sid2user(sid)
        print '\n[*] Lising Files For User: '+str(user)
        for file in files:
            print '[+]Found File: '+str(file)
def main():
    recycledDir=returnDir()
    findRecycled(recycledDir)
if __name__=='__main__':
    main()
```

### 解析PDF元数据

> 2010年12月10日，黑客组织匿名者发布了一条消息，解释了他们发起最近一次代号为“复仇行动”的攻击的大致动机。由于被那些放弃支持维基解密网站的公司所激怒，匿名者组织号召要通过对涉及的一些机构进行DDoS以实现报复。这个稿子上既没有签名，也没有标注信息来源，只是以PDF文件的形式被发布出来。但是创建这个文档所用的程序在PDF元数据中记录了文档作者的名字——Alex Tapanaris先生。几天后，希腊警方就逮捕了Tapanaris先生。
>
> https://www.wired.com/images_blogs/threatlevel/2010/12/ANONOPS_The_Press_Release.pdf

```python
import pyPdf
from pyPdf import PdfFileReader
def printMeta(fileName):
    pdfFile=PdfFileReader(file(fileName,'rb'))
    docInfo=pdfFile.getDocumentInfo()
    print '[*] PDF MetaData For: '+str(fileName)
    for metaItem in docInfo:
        print '[+] '+metaItem+':'+docInfo[metaItem]
```

### 蓝牙搜寻

```python
import time
from bluetooth import *
alreadyFound=[]
def findDevs():
    foundDevs=discover_devices(lookup_names=True)
    for (addr,name) in foundDevs:
        if addr not in alreadyFound:
            print '[*] Found Bluetooth Device: '+str(name)
            print '[+] MAC address: '+str(addr)
            alreadFound.append(addr)
while True:
    findDevs()
    time.sleep(5)
```

