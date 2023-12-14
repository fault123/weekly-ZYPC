---
title: Python爬虫笔记
date: 2023-10-15 21:06:24
tags: 爬虫
mathjax: true
---

# Python爬虫笔记

## urllib

### GET请求

```python
import urllib.request
url='https://...'
response=urllib.request.urlopen(url=url)
response.status #返回请求码 200
response.getheaders() #返回响应头信息
    [('Connection','close'),('Content-Length','48955'),('Server','nginx'),...]
response.read().decode('utf-8') #返回HTML代码
```

### POST请求

```python
import urllib.request
import urllib.parse
url='...'
data=bytes(urllib.parse.urlencode({'hello':'python'}),encoding='utf-8')
response=urllib.request.urlopen(url=url,data=data)
print(response.read(),decode('utf-8')) #读取HTML
```

### timeout参数

```python
import urllib.request
import urllib.error
import socket
url='...'
try:
    response=urllib.request.urlopen(url=url,timeout=0.1)
    ...
except urllib.error.URLError as error:
    if isinstance(error.reason,socket.timeout):
        #超时
```

### 设置请求头

```python
import urllib.request
import urllib.parse
url='...'
headers={'User-Agent':'Mozilla/5.0 ...'}
data=bytes(urllib.parse.urlencode({'hello':'python'}),encoding='utf-8') #POST
r=urllib.request.Request(url=url,data=data,headers=headers,method='POST')
response=urllib.request.urlopen(r)
print(response.read().decode('utf-8'))
```

### 获取cookie

```python
import urllib.request
import urllib.parse
import urllib.cookiejar
import json
url='...'
data=bytes(urllib.parse.urlencode({'username':'mrsoft','password':'mrsoft'}),encoding='utf-8')
cookie_file='cookie.txt'
cookie=http.cookiejar.LWPCookieJar(cookie_file)
cookie=http.cookiejar.CookieJar()
cookie_processor=urllib.request.HTTPCookieProcessor(cookie)
cookie.load(cookie_file,ignore_expires=True,ignore_discard=True)
opener=urllib.request.build_opener(cookie_processor)
response=opener.open(url,data=data)
response=json.loads(response.read().decode('utf-8'))['msg']
if response=='登录成功！':
    for i in cookie:
        print(i.name+'='+i.value)
```

### 设置代理IP

```python
import urllib.request
url="..."
proxy_handler=urllib.request.ProxyHandler({'https':'58.220.95.114:10053'})
opener=urllib.request.build_opener(proxy_handler)
response=opener.open(url,timeout=2)
print(response.read().decode('utf-8'))
```

### 异常处理

```python
import urllib.request
import urllib.error
try:
    response=urllib.request.urlopen('...')
except urllib.error.HTTPError as error:
    print(error.code)#状态码
    print(error.reason)#Not Found
    print(error.headers)#请求头
except urllib.error.URLError as error:
    print(error.reason)
```

### 解析链接

#### 拆分url

```python
import urllib.parse
parse_result=urllib.parse.urlparse('https://doc.python.org/3/library/urllib.parse.html')
print(parse_result.scheme)#https
print(parse_result.netloc)#docs.python.org
print(parse_result.path)#/3/library/urllib.parse.html
print(parse_result.params)#参数
print(parse_result.query)#查询条件
print(parse_result.fragment)#片段标识符
```

#### 组合url

```python
import urllib.parse
list_url=['https','docs.python.org','/3/library/urllib.parse.html','','','']
tuple_url=('https','docs.python.org','/3/library/urllib.parse.html','','','')
dict_url={'scheme':'https','netloc':'docs.python.org','path':'/3/library/urllib.parse.html','params':'','query':'','fragment':''}
print(urllib.parse.urlunparse(list_url))#相同方法
print(urllib.parse.urlunparse(tuple_url))
print(urllib.parse.urlunparse(dict_url))
```

#### 连接url

```python
import urllib.parse
base_url='...'
print(urllib.parse.urljoin(base_url,'../../ *.html'))#如果第二个参数为完整url，直接反应对应的值
```

#### url编码解码

```python
import urllib.parse
params={'name':'Jack','country':'中国','age':30}
print(urllib.parse.urlencode(params))#name=...&country=...&age=...
print(urllib.parse.quote('中国'))#直接编码
print(urllib.parse.unquote(u))#解码
```

#### 参数转为字典类型

```python
import urllib.parse
url=''
q=urllib.parse.urlsplit(url).query
q_dict=urllib.parse.parse_qs(q)
```

#### 参数转为列表类型

```python
import urllib.parse
str_params='...'
list_params=urllib.parse.parse_qsl(str_params)
```

## urllib3

### GET

```python
import urllib3
urllib3.disable_warnings()#关闭ssl警告
url1='...'
url2='...'
http=urllib3.PoolManager()
r1=http.request('GET',url1)
r2=http.request('GET',url2)
```

### POST

```python
import urllib3
urllib3.disable_warnings()
url='...'
params={'':'',...}
http=urllib3.PoolManager()
r=http.request('POST',url,fields=params)
print(r.data.decode('utf-8'))
print(r.data.decode('unicode_escape'))#中文
```

### 重试请求

```python
#...
r=http.request('POST',url)#默认3次
r=http.request('POST',url,retries=5)#重试5次
r=http.request('POST',url,retries=False)#关闭重试
#...
```

### 获取响应头

```python
response_header=r.info()
for key in response_header.keys():
    print(key,':',response_header.get(key))
```

### json信息

```python
j=json.loads(r.data.decode('unicode_escape'))
print(j.get('...'))
print(j.get('...').get('...'))
```

### 二进制数据

```python
r=http.request('GET',url)
f=open('*.png','wb+')
f.write(r.data)
f.close()
```

### 设置请求头

```python
header={'User-Agent':'Mozilla/5.0 (Wind...'}
r=http.request('GET',url,headers=headers)
```

### 设置超时

```python
r=http.request('GET',url,timeout=0.01)
###
import urllib3
from urllib3 import timeout
urllib3.disable_warnings()
timeout=Timeout(connect=0.5,read=0.1)
http=urllib3.PoolManager(timeout=timeout)
http.request('GET','http://...')
#或
http=urllib3.PoolManager()
http.request('GET','...',timeout=timeout)
```

### 设置代理

```python
import urllib3
url='...'
headers={'User-Agent':'...'}
proxy=urllib3.ProxyManager('http://...:80',headers=headers)
r=proxy.request('get',url,timeout=2.0)
print(r.data.decode())
```

### 上传文件

#### fields上传

```python
import urllib3
with open('text.txt') as f:
    data=f.read()
http=urllib3.PoolManager()
r=http.request('POST','...',fields={'filefield':('example.txt',data),})
```

#### body上传

```python
import urllib3
with open('*.jpg','rb') as f:
    data=f.read()
http=urllib3.PoolManager()
r=http.request('POST','...',body=data,headers={'Content-Type':'image/jpeg'})
```

## request

### GET

```python
import requests
response=requests.get('...')
print(response.status_code)
print(response.url)
print(response.headers)
print(response.cookies)
```

### 对响应结果编码

```python
response.encoding='utf-8'
print(response.text)
```

### 爬取二进制数据

```python
import requests
response=requests.get('.../*.png')
with open("*.png",'wb') as f:
    f.write(response.content)
```

### GET请求

```python
import requests
data={'name':'Micheal',...}
response=requests.get('...',params=data)
response__dict=json.loads(response.text)
```

### POST请求

```python
import requests
data={'1':'...',...}
response=requests.post('...',data=data)
#...
```

### 元组、列表、字典转json

```python
data=...
data=json.dumps(data)
```

### 添加headers

```python
import requests
url=''
headers={'User-Agent':'...'}
response=requests.get(url,headers=headers)
print(response.status_code)
```

### 验证cookies

```python
import requests
from lxml import etree
cookies='...'
headers={
    'Host':'...',
    ...
};
cookies_jar=requests.cookies.RequestsCookieJar()
for cookie in cookies.split(';'):
    key,value=cookie.split('=',1)
    cookies_jars.set(key,value)
response=requests.get('https://...',headers=headers,cookies=cookies_jar)
if response.status_code==200:
    html=etree.HTML(response.text)
    name=html.xpath('//*[@id="db-global-nav"]/div/div[1]/ul/li[2]/a/span[1]/text()')
    print(name[0]) #输出账号
```

### 会话请求

相当于再开一个选项卡，一个负责登录，一个负责获取登录后数据

```python
import requests
s=requests.Session()
data={'username':'...','password':'...'}#登录信息
response=s.post('http://.../chklogin.html',data=data)
response2=s.get('http://...')
print(response.text,response2.text)
```

### 验证请求

```python
import requests
from requests.auth import HTTPBasicAuth
url='...'
ah=HTTPBasicAuth('username','password')
response=requests.get(url=url,auth=ah)
if response.status_code==200:
    print(response.text)
```

### 设置超时

```python
import requests
try:
    response=requests.get('...',timeout=0.1)
    ...
except Exception as e:
    print(str(e))
```

### 识别异常

```python
import requests
from requests.exceptions import ReadTimeout,HTTPError,RequestException
try:
    response=requests.get('...',timeout=0.1)
    print(response.status_code)
except ReadTimeout: #超时异常
    ...
except HTTPError: #HTTP异常
    ...
except RequestException: #请求异常
    ...
```

### 上传文件

```python
import requests
bd=open('*.png','rb')
file={'file':bd}
response=requests.post('...',files=file)
print(response.text)
```

### 设置代理

```python
import requests
headers={'User-Agent':'Mozilla'
                      '...'}
proxy={'http':'http://*.*.*.*:3000',
       'https':'...'}
try:
    response-requests.get('http://...',headers=headers,proxies=proxy,verify=False,timeout=3)
    print(response.status_code)
except Exception as e:
    print(e)
```

### 获取免费代理IP

```python
import requests
from lxml import etree
import pandas as pd
ip_list=[]
def get_ip(url,headers):
    response=requests.get(url,headers=headers)
    response.encoding='utf-8'
    if response.status_code==200:
        html=etree.HTML(response.text)
        li_all=html.xpath('//li[@class="f-list col-lg-12 col-md-12 col-sm-12 col-xs-12"]')
        for i in li_all:
            ip=i.xpath('span[@class="f-address"]/text()')[0]
            port=i.xpath('span[@class=f-address]/text()')[0]
            ip_list.append(ip+':'+port)
headers={'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64)'
                      'AppleWebkit/537.36 (KHTML, like Gecko)'
                      'Chrome/72.0.3626.121 Safari/537.36'}
if __name__=='__main__':
    ip_table=pd.DataFrame(columns=['ip'])
    for i in range(1,5):
        url='https://.../{page}.html'.format(page=i)
        get_ip(url,headers)
    ip_table['ip']=ip_list
    ip_table.to_excel('ip.xlsx',sheet_name='data')
```

### 检测代理IP是否有效

```python
import requests
import pandas
from lxml import etree
ip_table=pandas.read_excel('ip.xlsx')
ip=ip_table['ip']
headers={'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64)'
                      'AppleWebkit/537.36 (KHTML, like Gecko)'
                      'Chrome/72.0.3626.121 Safari/537.36'}
for i in ip:
    proxies={'http':'http://{ip}'.format(ip=i),
             'https':'https://{ip}'.format(ip=i)}
    try:
        response=requests.get('http://202020.ip138.com/',headers=headers.proxies=proxies,timeout=2)
        if response.status_code==200:
            response.encoding='utf-8'
            html=etree.HTML(response.text)
            info=html.xpath('/html/body/p[1]//text()')
            print(info) #\r\n您的iP地址是：[*.*.*.*] 来自：*省*市 ***（运营公司）\r\n
    except Exception as e:
        pass
```

## requests_cache

### 打印当前版本

```python
import requests_cache
version=requests_cache.__version__
print(version)
```

### 缓存设置

```python
"""
    install_cache(cache_name='cache',backend=None,expire_after=None,allowable_codes=(200,),allowable_methods=('GET',),session_factory=<class 'requests_cache.core.CacheSession'>,**backend_options)
        cache_name 缓存文件名 默认cache
        backend 储存机制 默认None 默认使用sqlite储存
            memory 内存
            sqlite
            mongo
            redis
        expire_after 缓存有效时间 默认None 永久有效
        allowable_codes 设置状态码 默认200
        allowable_methods 设置请求方式 默认GET 表示只有GET请求才可以生成缓存
        session_factory 设置缓存执行的对象 需要实现CachedSession类
        **backend_options 如果存储方式为sqlite、mongo、redis 则设置连接方式
"""
import requests_cache
import requests
requests_cache.install_cache()
requests_cache.clear()
url='...'
r=requests.get(url)
print(r.from_cache) #False不存在缓存
r=requests.get(url)
print(r.from_cache) #True存在缓存
```

### 缓存应用

```python
import requests_cache
import time
requests_cache.install_cache()
requests_cache.clear()
def make_throttle_hook(timeout=0.1):
    def hook(response,*args,**kwargs):
        print(response.text)
        if not getattr(response,'from_cache',False):
            time.sleep(timeout) #等待2秒
        else:
            print(response.from_cache) #存在缓存
        return response
    return hook
if __name__=='__main__':
    requests_cache.install_cache()
    requests_cache.clear()
    s=requests_cache.CachedSession()
    s.hooks={'response':make_throttle_hook(2)} #配置钩子函数
    s.get('http://...') #不存在缓存
    s.get('http://...') #存在缓存
```

## requests-html

### get()请求

```python
from requests_html import HTMLSession
session=HTMLSession()
url='http://...'
r=session.get(url)
print(r.html)
```

### post()请求

```python
data={
    'user':'admin',
    'password':123456
}
r=session.post('http://...',data=data)
if r.status_code==200:
    print(r.text)
```

### 随机请求头

```python
from requests_html import UserAgent,HTMLSession
session=HTMLSession()
ua=UserAgent().random
r=session.get('http://...',headers={'user-agent':ua})
if r.status_code==200:
    print(r.text)
```

### 爬取新闻

```python
from requests_html import HTMLSession,UserAgent
session=HTMLSession()
ua=UserAgent().random
r=session.get('http://...',headers={'user-agent':ua})
r.encoding='gb2312'
if r.status_code==200:
    li_all=r.html.xpath('.//ul[@class="tj3_1"]/li')
    for li in li_all:
        news_title=li.find('a')[0].text
        new_href='http://'+li.find('a[href]')[0].attrs.get('href').lstrip('.')
        news_time=li.find('font')[0].text
        #...
```

### find()方法

```python
li_all=html.find('li',containing='新冠病毒')
```

### search()方法

```python
for li in r.html.find('li',containing='新冠病毒'):
    a=li.search('<a href="{}">{}</a>')
    news_title=a[1]
    news_href='http://...'+a[0]
    news_time=li.search('<font>{}</font>')[0]
```

### search_all()方法

```python
import re
class_tj3_1=r.html.xpath('.//ul[@class="tj3_1"]')
li_all=class_tj3_1[0].search_all('<li>{}</li>')
for li  in li_all:
    if '新冠病毒' in li[0]:
        a=re.findall('<font>(.*?)</font><a href="(.*?)">(.*?)</a>',li[0])
        news_title=a[0][2]
        news_href='http://...'+a[0][1]
        news_time=a[0][0]
```

## 正则表达式

```python
import re
match=re.search('模式字符串','匹配字符串',re.I) #re.I 可选 不区分大小写
```

```
(\d?)+ 多个数字可有可无
\s 空格
([\u4e00-\u9fa5]?)+ 多个汉字可有可无
\b 匹配字符串开头处、结尾处、空格、标点符号、换行
.*(\d+) 贪婪匹配
```

## 视频下载

```python
video_response=requests.get(url=video_url,headers=headers)
data=video_response.content
file=open('*.mp4','wb')
file.write(data)
file.close()
```

## XPath解析

### parse()方法-解析HTML文件

```python
from lxml import etree
parser=etree.HTMLParser()
html=etree.parse('demo.html',parser=parser)
html_txt=etree.toString(html,encoding='utf-8')
print(html_txt.decode('utf-8'))
```

### HTML()方法-解析字符串型HTML代码

```python
from lxml import etree
html_str="""
            ...
         """
html=etree.HTML(html_str)
html_txt=etree.toString(html,encoding='utf-8')
print(html_txt.decode('utf-8'))
```

### 解析HTML代码

```python
from lxml import etree
import requests
from requests.auth import HTTPBasicAuth
url='http://...'
ah=HTTPBasicAuth('admin','admin') #用户名密码
response=requests.get(url=url,auth=ah)
if response.status_code==200:
    html=etree.HTML(response.text)
    html_txt=etree.tostring(html,encoding='utf-8')
    print(html_txt.decode('utf-8'))
```

### 获取节点

```python
#获取所有节点
from lxml import etree
html_str="""
            ...
         """
html=etree.HTML(html_str)
node_add=html.xpath('//*')
print([i.tag for i in node_all])
#获取所有"li"节点
li_all=html.xpath('//li')
#获取"li"节点的直接子节点
a_all=html.xpath('//li/a')
#获取"li"节点的子孙节点
a_all=html.xpath('//li//a')
#获取"a"节点的父节点
a_all_parent=html.xpath('//a/..')
a_all_parent=html.xpath('//a/parent::*')
```

### 获取节点文本信息

```python
html=etree.HTML(html_str)
a_text=html.xpath('//a/text()') #获取所有"a"节点的文本信息
print(a_text)
```

### 属性匹配

```python
div_one=html.xpath('//div[@class="level one"]/text()')
div_all=html.xpath('//div[contains(@class,"level")]/text()')
```

### 获取属性

```python
li_all=html.xpath('//div/li/a/@title')
print(li_all)
```

### 按序获取属性

```python
li=html.xpath('//div/li[last()]/a/@title') #最后一个
li=html.xpath('//div/li[position()=1]/a/@title') #第一个
li=html.xpath('//div/li[last()-1]/a/@title') #倒数第二个
li=html.xpath('//div/li[position()>1]/a/@title') #位置大于1
```

### 节点轴

```python
li=html.xpath('//li[2]/ancestor::*') #li[2]所有祖先节点名称
li=html.xpath('//li[2]/ancestor::body') #li[2]指定祖先节点名称
li=html.xpath('//li[2]/ancestor::*[@class="video_scroll"]') #li[2]指定属性的祖先节点名称
li=html.xpath('//li[2]/a/attribute::*') #li[2]所有属性值
li=html.xpath('//li[2]/child::*') #li[2]所有子节点名称
li=html.xpath('//li[2]/descendant::*') #li[2]所有子孙节点名称
li=html.xpath('//li[2]/following::*') #li[2]之后的所有节点名称
li=html.xpath('//li[2]/following-sibling::*') #li[2]之后所有同级节点名称
li=html.xpath('//li[2]/preceding::*') #li[2]之前所有节点名称
```

## Beautiful Soup

### 解析HTML代码

```python
from bs4 import BeautifulSoup
html_doc="""
            ...
         """
soup=BeautifulSoup(html_doc,features='lxml')
print(soup)
#从文件解析
soup=BeautifulSoup(open('index.html'),'lxml')
print(soup.prettify()) #格式化
```

### 获取节点

```python
#获取节点内容
print(soup.head)
print(soup.body)
print(soup.title)
print(soup.p) #只打印第一个节点
#获取节点名称
print(soup.head.name)
print(soup.body.name)
print(soup.title.name)
print(soup.p.name)
#获取节点属性
print(soup.meta.attrs) #返回字典
print(soup.link.attrs)
print(soup.div.attrs)
#获取节点文本内容
print(soup.title.string)
print(soup.h3.string)
```

### 关联获取

```python
#获取子节点
print(soup.head.contents) #列表形式
for i in soup.head.children: #可迭代对象形式
    print(i)
#获取孙节点
for i in soup.body.descendants:
    print(i)
#获取父节点
print(soup.title.parent) #打印父节点内容
for i in soup.title.parents:
    prnt(i.name) #从内向外打印，最后[document]代表整个HTML文档
#兄弟节点
print(soup.p.next_sibling)
print(soup.p.previous_sibling)
print(list(soup.p.next_siblings))
print(list(soup.p.previous_siblings))
```

### find()方法获取内容

```python
#name参数
print(soup.find_all(name='p')[0])
#attr参数
print(soup.find_all(attrs={'value':'1'}))
print(soup.find_all(class_='p-1'))
print(soup.find_all(value='3'))
#text参数
print(soup.find_all(text='...'))
print(soup.find_all(text=re.compile('...')))
#其他方法
find()
find_all()
find_parent()
find_parents()
find_next_sibling()
find_next_siblings()
find_previous_sibling()
find_previous_siblings()
find_next()
find_all_next()
find_previous()
find_all_previous()
```

### CSS选择器

```python
print(soup.select('p')) #打印所有p节点内容
print(soup.select('p')[1]) #打印所有p节点中第2个p节点
print(soup.select('html head title')) #打印逐层获取的title节点
print(soup.select('.test_2')) #打印类名为test_2对应的节点
print(soup.select('#class_1')) #打印id为class_1对应的节点
#其他方法
soup.select('div[class="test_1"]')[0].select('p')[0] #嵌套获取名为test_1对应div中第一个p节点
soup.select('p')[0]['value'] #获取p节点中第一个节点value属性对应的值
soup.select('p')[0].attrs['value']
soup.select('p')[0].get_text() #获取p中第一个节点内文本
soup.select('p')[0].string
soup.select('p')[1:] #获取p节点中第二个以后的p节点
soup.select('.p-1,.p-5') #获取class为p-1与p-5对应的节点
soup.select('a[href]') #获取存在href属性的节点
soup.select('p[value="1"]') #获取属性value="1"的p节点
#select_one()方法
soup.select_one('a') #获取a节点中第一个a节点内容
```



