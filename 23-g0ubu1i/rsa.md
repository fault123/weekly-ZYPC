# RSA解题

## 一、素数分解

题目中给出的n可直接分解

工具：yafu http://www.factordb.com/

```python
from Crypto.Util.number import *

flag= ···
n= ···
e = ···

q = ···
p = ···

phi = (p-1)*(q-1)
d = inverse(e,phi)
m = pow(flag,d,n)
print(long_to_bytes(m))
```



## 二、低加密指数攻击

e很小。使用中国剩余定理

```PYTHON
from gmpy2 import*
from Crypto.Util.number import*
from libnum import*

e=9

def CRT(a,n):
    sum = 0
    N = reduce(lambda x,y:x*y,n)   # ni 的乘积,N=n1*n2*n3

    for n_i, a_i in zip(n,a):    # zip()将对象打包成元组
        N_i = N // n_i           #Mi=M/ni
        sum += a_i*N_i*gmpy2.invert(N_i,n_i)   #sum=C1M1y1+C2M2y2+C3M3y3
    return sum % N 

n = [···]
c = [···]
x = CRT(c,n)

m = gmpy2.iroot(x,e)[0]
print(n2s(int(m)))
```

## 三、共模攻击

当给出n被不同的e加密后的c时，使用扩展欧几里得定理

```python
from Crypto.Util.number import *
from gmpy2 import *

n1 = ```
e1 = ```

n2 = ```
e2 = ```

c1=```
c2=```

s,s1,s2 = gmpy2.gcdext(e1,e2)

m=(pow(c1,s1,n1)*pow(c2,s2,n1))%n1

print(long_to_bytes(m))
```

## 四、共享素数

题目中对明文进行了两次的加密，分别使用两次的n，要通过密文解出明文m，那么就得知道两次加密的解密钥d，要求出解密钥d就得需要知道p，q通过求逆元的方法求出d。但是n1和n2的值非常大，通过爆破求因式分解显示不太可能。因此可以看看两个n之间是否存在共用的素数。

```python
from Crypto.Util.number import *
from gmpy2 import *

n1 = ···
n2 = ···
e = 65537
c = ···

p = gmpy2.gcd(n1,n2)
q1 = n1 // p
q2 = n2 // p
phi1 = (p-1)*(q1-1)
phi2 = (p-1)*(q2-1)
d1 = gmpy2.invert(e,phi1)
d2 = gmpy2.invert(e,phi2)
m = pow(c,d2,n2)
m = pow(m,d1,n1)
print(long_to_bytes(m))
```

## 五、低解密指数攻击

当e非常大时，d会很小，维纳攻击，涉及数论的勒让德定理https://www.cnblogs.com/yuanzhimengbian/p/15876619.html

```python
def RSA_wiener (n,e,c):
    #连分数逼近，并列出逼近过程中的分子与分母
    def lian_fen(x,y):
        res = []
        while y:
            res.append(x//y)
            x,y = y,x%y
        resu = []
        for j in range(len(res)):
            a,b = 1,0
            for i in res[j::-1]:
                b,a = a,a*i+b
            resu.append((a,b))
        if resu[0] == (0,1):
            resu.remove((0,1))
        return resu[:-1]
    lianfen = lian_fen(e,n)
    def get_pq(a,b,c):
        par = isqrt((n-phi+1)**2-4*n)
        x1,x2 = (-b + par) // (2 * a), (-b - par) // (2 * a)
        return x1,x2
    for (k,d) in lianfen:
        phi = (e*d-1)//k
        p,q = get_pq(1,n-phi+1,n)
        if p*q == n:
            p,q = abs(int(p)),abs(int(q))
            d = invert(e,(p-1)*(q-1))
            break
    return long_to_bytes(pow(c,d,n))
```

## 六、已知p高位攻击

已知p的高位

```sage
from sage.all import *
n = 113432930155033263769270712825121761080813952100666693606866355917116416984149165507231925180593860836255402950358327422447359200689537217528547623691586008952619063846801829802637448874451228957635707553980210685985215887107300416969549087293746310593988908287181025770739538992559714587375763131132963783147
p4 = 7117286695925472918001071846973900342640107770214858928188419765628151478620236042882657992902
#p去0的剩余位
e = 65537
pbits = 512
kbits = pbits - p4.nbits()
print(p4.nbits())
p4 = p4 << kbits
PR.<x> = PolynomialRing(Zmod(n))
f = x + p4
roots = f.small_roots(X=2^kbits, beta=0.4)
if roots:
    p = p4+int(roots[0])
print("n: "+str(n))
print("p: "+str(p))
print("q: "+str(n//p))
```

## 七、e与phi不互素

```python
from Crypto.Util.number import *
from gmpy2 import *
p= 86053582917386343422567174764040471033234388106968488834872953625339458483149
q= 72031998384560188060716696553519973198388628004850270102102972862328770104493
c= 3939634105073614197573473825268995321781553470182462454724181094897309933627076266632153551522332244941496491385911139566998817961371516587764621395810123
e = 74
phi = (p-1)*(q-1)
g = GCD(e,phi)
d = inverse(e//g,phi)
m = pow(c,d,p*q)
m = gmpy2.iroot(m,g)[0]
print(long_to_bytes(m))
```

