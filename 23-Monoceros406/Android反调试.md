---
title: Android反调试
date: 2023-11-14 15:02:39
tags: Android
mathjax: true
---

# Android反调试

```java
protected void onCreate(Bundle arg4){
    ApplicationInfo v1=this.getApplicationInfo();
    int v2=v1.flags&2;
    v1.flag2=v2;
    if(v2!=0){
        Process.killProcess(Process.myPid());
    }
}
```

