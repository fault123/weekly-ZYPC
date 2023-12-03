---
title: C++查缺补漏
date: 2023-11-06 15:37:41
tags: C++
---

# C++查缺补漏

## 面向对象

### 省略号参数

```c++
#include <cstdarg>
void OutputInfo(int num,...){
    va_list arguments;
    va_start(arguments,num);
    while(num--){
        char* pchData=va_arg(arguments,char*);
        int iData=va_arg(arguments,int);
        printf("%s\n",pchData);
        printf("%d\n",iData);
    };
    va_end(arguments);
};
int main(void){
    OutputInfo(2,"beijing",2008,"olympic games",2008);
    return 0;
};
```

### 函数指针

```c++
typedef int (*ptfun)(int,int);
int Invoke(int x,int y,ptfun fun){
    return fun(x,y);
};
int sum(int x,int y){
    return x+y;
};
int sub(int x,int y){
    return x-y;
};
int mul(int x,int y){
    return x*y;
};
int divi(int x,int y){
    return x/y;
};
int main(int argc,char* argv[]){
    ptfun pfun;
    pfun=sum;
    int ret=Invoke(20,10,pfun);
    pfun=mul;
    ret=Invoke(20,10,pfun);
    return 0;
};
```

函数指针数组：

```c++
int sum(int x,int y){
    return x+y;
};
int sub(int x,int y){
    return x-y;
};
int mul(int x,int y){
    return x*y;
};
int divi(int x,int y){
    return x/y;
};
int main(int argc,char* argv[]){
    int (*ptfun[4])(int,int);
    ptfun[0]=sum,
    ptfun[1]=sub,
    ptfun[2]=mul,
    ptfun[3]=divi;
    for(int i=0;i<4;i++){
        int ret=ptfun[i](30,10);
        printf("%d\n",ret);
    };
    return 0;
};
```

## 文件与注册表

### INI文件操作

```c++
BOOL WritePrivateProfileString(LPCTSTR IpAppName,LPCTSTR IpKeyName,LPCTSTR IpString,LPCTSTR IpFileName);
/*
	写入字符串数据
	IpAppName 节名 不存在将创建一个节名
	IpKeyName 键名 不存在则创建 如果为NULL节及节下所有项目将被删除
	IpString 写入键值的数据
	IpFileName INI文件名称
*/

DWORD GetPrivateProfileString(LPCTSTR IpAppName,LPCTSTR IpKeyName,LPCTSTR IpDefault,LPTSTR IpReturnedString,DWORD nSize,LPCTSTR IpFileName);
/*
	获取字符串数据
	IpAppName 节名 NULL复制所有节名到缓冲区
	IpKeyName 键名 NULL将IpAppName下所有键名复制到缓冲区
	IpDefault 默认值
	IpReturnedString 缓冲区
	nSize 字符为单位表示缓冲区大小
	IpFileName 文件名
*/

UINT GetPrivateProfileInt(LPCTSTR IpAppName,LPCTSTR IpKeyName,INT nDefault,LPCTSTR IpFileName);//读整型
DWORD GetPrivateProfileSectionNames(LPTSTR IpszReturnBuffer,DWORD nSize,LPCTSTR IpFileName);//返回所有节名
DWORD GetPrivateProfileSecion(LPCTSTR IpAppName,LPTSTR IpReturnedString,DWORD nSize,LPCTSTR IpFileName);//返回指定节下所有键名和键值
```
