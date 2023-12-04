---
title: Javascript操作DOM
date: 2023-10-15 20:10:05
tags: Javascript
mathjax: true
---

# Javascript操作DOM

*该笔记暂未上线*

```javascript
//nodeType属性
function count(n){
    var num=0;
    if(n.nodeType==1)
        num++;//n.nodeName为标签名
    var son=n.childNodes;
    for(var i=0;i<son.length;i++)
        num+=count(son[i]);
    return num;
};
console.log(count(document));//元素总数
```

