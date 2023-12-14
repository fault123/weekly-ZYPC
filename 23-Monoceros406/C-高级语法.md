---
title: C++高级语法
date: 2023-10-14 19:16:39
tags: C++
mathjax: true
---

# C++高级语法

## 联合体

```c++
union infounion{
    int age;
    char name[32],address[];
}uinfo;
```

## enum枚举类型

```c++
enum fourseasons={spring=1,summer,automn,winter};
```

## 显式转换

```c++
#include <cstdlib>
char *str1="1234",*str2="12.345";
printf("%d %f\n",atoi(str1),atof(str2));
```

## 指针

```c++
int i,*p;
p=&i,i=2;
printf("%d=%d\n",i,*p);
//-----------------------------
int num[5]={12,34,56,78,90},*p;
p=num;
printf("%d\n",*(p+2));//56
p=num+4;
printf("%d\n",*(p-2));//56
p=num;
for(register int i=1;i<=5;i++)
    printf("%d\n",*p++);
p=num+4;
for(register int i=1;i<=5;i++)
    printf("%d\n",*p--);
//------------------------------
int num[3][3]={
    12,34,56,
    78,90,33,
    20,18,57
},(*pnum)[3];
pnum=num;
printf("%d %d %d %d\n",*pnum[0],*(pnum[0]+2),*pnum[2],*(pnum[2]+2));//12 56 20 57
```

## OOP

```c++
#include <cstdio>
using namespace std;
class ccalc{
    public:
        int inumber1,inumber2,irest;
        char oper;
        inline void setnum1(const int num){
            inumber1=num;
            return void();
        };
        inline int getnum1(void){
            return inumber1;
        };
        inline void setnum2(const int num){
            inumber2=num;
            return void();
        };
        inline int getnum2(void){
            return inumber2;
        };
        inline void setoperator(const char ch){
            oper=ch;
            return void();
        };
        inline char getoperator(void){
            return oper;
        };
        inline int calcrest(void);
};
inline int ccalc::calcrest(void){
    switch(oper){
        case '+':
            irest=inumber1+inumber2;
            break;
        case '-':
            irest=inumber1-inumber2;
            break;
        case '*':
            irest=inumber1*inumber2;
            break;
        case '/':
            irest=inumber1/inumber2;
            break;
        default:
            return -1;
    };
    return irest;
};
class cube:public ccalc{
    public:
        inline int getcube(void){
            return inumber1*inumber1*inumber1;
        };
        inline int getsquare(void){
            return inumber1*inumber1;
        };
};
int main(void){
    cube m_calc;
    m_calc.setnum1(1);
    m_calc.setnum2(2);
    m_calc.setoperator('+');
    printf("%d %c %d=%d\n",m_calc.getnum1(),m_calc.getoperator(),m_calc.getnum2(),m_calc.calcrest());
    return 0;
};
```

```c++
#include <cstdio>
using namespace std;
class ctest{
    public:
        int m_num;
        inline void setnum(const int a);
        inline int getnum(void);
};
inline void ctest::setnum(const int a){
    m_num=a;
    return void();
};
inline int ctest::getnum(void){
    return m_num;
};
inline void shownum(ctest *test){
    printf("%d %d\n",test->m_num,test->getnum());
    return void();
};
int main(void){
    ctest *pt1=new ctest;
    pt1->setnum(15);
    shownum(pt1);
    return 0;
};
```

```c++
#include <cstdio>
using namespace std;
class cstu{
    public:
        int num1,num2;
        cstu(void){
            this->setnum(11,22);
            this->getnum();
        };
        inline void setnum(const int a,const int b){
            this->num1=a,
            (*this).num2=b;
            return void();
        };
        inline void getnum(void){
            printf("%d %d\n",this->num1,(*this).num2);
            return void();
        };
};
int main(void){
    cstu s;
    s.setnum(10,20);
    s.getnum();
    return 0;
};
```

```c++
#include <cstdio>
using namespace std;
class ctest{
    public:
        static inline int add(const int a,const int b){
            return a+b;
        };
        inline int sub(const int a,const int b){
            return a-b;
        };//普通函数中无法调用静态变量
        inline int mul(const int a,const int b){
            return a*b;
        };
        static inline int div(const int a,const int b){
            return a/b;
        };
        static inline void setnum(void){
            num1=11,
            num2=22;
        };
        static int num1,num2;
};
inline void teststatic(void){
    printf("%d %d\n",ctest::add(10,20),ctest::div(50,10));
    return void();
};
int main(void){
    teststatic();
    ctest t1;
    printf("%d %d\n",t1.sub(10,20),t1.mul(50,10));
    return 0;
};
```

```c++
#include <cstdio>
using namespace std;
class student{
    public:
        int val;
        student(void){
            val=0;
        };
        student(const int num){
            val=num;
        };
};
int main(void){
    student s;
    printf("%d\n",s.val);
    student arr(4);
    printf("%d\n",arr.val);
    student *ptr1=new student;
    printf("%d\n",ptr1->val);
    student *ptr2=new student(10);
    printf("%d\n",ptr2->val);
    return 0;
};
```

```c++
#include <cstdio>
#include <cstring>
using namespace std;
class cstu{
    public:
        char val[16];
        cstu(void){};
        cstu(const int a){
            sprintf(val,"%d",a);
            printf("%s\n",val);
        };
        cstu(const double f){
            sprintf(val,"%f",f);
            printf("%s\n",val);
        };
        cstu(const char *str){
            strcpy(val,str);
            printf("%s\n",val);
        };
};
int main(void){
    cstu s,sint(10),sfloat(1.234),sstr("helloworld");
    return 0;
};
```

```c++
#include <cstdio>
using namespace std;
class ctest{
    public:
        int a;
        ctest(const int b){
            a=b;
        };
        ctest(const ctest& c){
            a=c.a;
        };
        inline void show(void){
            printf("%d\n",a);
            return void();
        };
};
int main(void){
    ctest a(123);
    ctest b=a;//浅拷贝
    b.show();
    return 0;
};
```

```c++
#include <cstdio>
using namespace std;
class ctest{
    private:
        int *p;
    public:
        ctest(void){
            p=new int(100);
        };
        ctest(const ctest& t){
            p=new int;
            *p=*(t.p);
        };//深拷贝
        ~ctest(void){
            if(p!=NULL)
                delete p;
        };
};
int main(void){
    ctest test1;
    ctest test2(test1);
    return 0;
};
```

```c++
#include <cstdio>
using namespace std;
class y;
class x{
    public:
        void disp(y py,char *name);
};
class y{
    friend void x::disp(y py,char *name);
    friend void puty(y& yc,char *name);
    private:
        int num;
        inline void dispy(char *name){
            printf("%s %d\n",name,num);
            return void();
        };
    public:
        y(const int n){
            num=n;
        };
};
inline void x::disp(y py,char *name){
    py.dispy(name);
    return void();
};
inline void puty(y& yc,char *name){
    yc.dispy(name);
    return void();
};
int main(void){
    y y1(100),y2(200);
    x x;
    x.disp(y1,"y1");
    x.disp(y2,"y2");
    puty(y1,"y1");
    puty(y2,"y2");
    return 0;
};
```

