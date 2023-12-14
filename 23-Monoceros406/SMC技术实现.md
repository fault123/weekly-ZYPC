---
title: SMC技术实现
date: 2023-11-10 18:32:49
tags: SMC
mathjax: true
---

# SMC技术实现

丢Viisual Studio2022里报了俩错，不会修。丢Dev-C++不报错，但是Release版还保留着原变量名、函数名，搞不懂。

```c++
//flag{smc_is_good!}
#include <cstdio>
#include <windows.h>
using namespace std;
char input[50];
unsigned long long encrypt[]={0x53e5e8325de5938b,0x57e5c0dede2de472,0x9b6ba65e9b6b0093,0x9b6ba15c9b6bac5f,0x9b6bbb5a9b6ba75d,0x9b6bad589b6bb35b,0x9b6b9f569b6ba359,0x9b6bb3549b6ba957,0x9b6ba7529b6b9f55,0x9b6baf509b6baf53,0x9b6be14e9b6ba451,0x35adc0dede16bd4f,0x1ee84b960dce88fd,0xbde5d068d17dc196,0x1c9540db9a1bcf1d,0x35adc0dede15c7aa,0xa0bc3b5ddf6e43d3,0x5de5c0dedeac7806,0xdeadc01d83f6e81a};
void desmc(void){
    for(int i=0;i<19;i++)
        encrypt[i]^=0xDEADC0DEDEADC0DE;
    return;
};
int main(void){
    scanf("%256s",&input);
    desmc();
    PDWORD lpflOldProtect=(PDWORD)malloc(sizeof(DWORD));
    printf("Checking your flag~\n");
    VirtualProtect(encrypt,sizeof(encrypt),PAGE_EXECUTE,lpflOldProtect);
    bool (*chk)(const char *);
    chk=(bool(*)(const char*))encrypt;
    if(chk(input))
        printf("Yes\n");
    else
        printf("No\n");
    system("pause");
    VirtualProtect(encrypt,sizeof(encrypt),PAGE_READWRITE,lpflOldProtect);
    free(lpflOldProtect);
    return 0;
};
```

先写一份能跑的chk函数代码，在IDA Pro里取消定义，再用LazyIDA将数据导出。把数据加密后放到encrypt数组里。
