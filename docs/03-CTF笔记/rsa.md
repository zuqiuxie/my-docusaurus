# RSA

> - p 和 q ：大整数N的两个因子（factor）
> - N：大整数N，我们称之为模数（modulus)
> - e 和 d：互为模反数的两个指数（exponent）
> - c 和 m：分别是密文和明文，这里一般指的是一个十进制的数

### **信息传递的过程：**

A 要 向 B 传递信息 m

首先 B  要把  公钥 （n, e）传递给  A

然后  A 拿着公钥  进行 公钥加密算法  将 明文 m  变成 密文 c

接着  A 把 生成的 密文 c  传递 给 B

最后  B 再利用 私钥 （n，d）进行 私钥解密算法 还原出来  明文 m

rsa加密的过程：
随便找出两个 整数 q 和 p （q，p互素，即：公因数只有1）
求出n = q * p  
φ(n)= （p-1）*（q-1） 欧拉公式
公钥 e ：   随机取，要求 ：e 和 φ(n) 互素（公因数只有 1）；   1<  e < φ(n)）；
私钥 d ：  ed  ≡  1 （mod  φ(n) ）   （ed 除以 φ(n) 的 余数 为  1 ）

![image-20211008145908649](https://tva1.sinaimg.cn/large/008i3skNgy1gv7wvcwk52j607j06jmx202.jpg)



>### n = q * p 
>
>### s= （p-1）*（q-1)
>
>### d=gmpy2.invert(e, s)
>
>> 问题描述，求d
>> n=pq
>> phi =(p-1)(q-1)
>> ed=1 mod phi
>>
>> 常用的库
>> import libnum  libnum.n2s(n)数字转字符串 libnum.s2n(s)
>> gmpy2.mpz(n)初始化一个大整数
>> n=invert(m,phi)求mod phi的逆元
>> pow(m,e,n)求c^d mod n
>> gmpy2.is_prime(n) 素性检测
>> gmpy2.gcd(a,b)  欧几里得算法，最大公约数
>> gmpy2.gcdext(a,b)  扩展欧几里得算法
>> gmpy2.iroot(x,n) x开n次根



## 一、已知p q e 求d(私钥) 及已知公钥 q p 求私钥d

```python
import gmpy2

p = 38456719616722997
q = 44106885765559411
e = 65537

s = (p - 1) * (q - 1)
d = gmpy2.invert(e, s)
print("dec: " + str(d))
print ("hex: " +  hex(d))
```

> [已知pqe 求d.py](../../../../../PycharmProjects/pythonProject3/rsa 练习/已知pqe 求d.py) 

## 二、已知c n e 求m(明文)  及已知公钥(n,e)求私钥d

```py
import libnum
from Crypto.Util.number import long_to_bytes

c = 0xdc2eeeb2782c
n = 322831561921859
e = 23


q = 13574881  #使用pyfactor 分解因式n
p = 23781539


d = libnum.invmod(e, (p - 1) * (q - 1))
m = pow(c, d, n)#解密
print("m的值为:")
print(long_to_bytes(m))
```

> [已知c n e 求m.py](../../../../../PycharmProjects/pythonProject3/rsa 练习/已知c n e 求m.py) 

## 三、已知(p+1)(q+1) p+q 求明文m  

>(p+1)(q+1) p+q   ----->  q*p   s=(q-1)(q-1)
>
>使用数学变换

```py
import libnum
from Crypto.Util.number import long_to_bytes
x = 0x1232fecb92adead91613e7d9ae5e36fe6bb765317d6ed38ad890b4073539a6231a6620584cea5730b5af83a3e80cf30141282c97be4400e33307573af6b25e2ea + 1
y = 0x5248becef1d925d45705a7302700d6a0ffe5877fddf9451a9c1181c4d82365806085fd86fbaab08b6fc66a967b2566d743c626547203b34ea3fdb1bc06dd3bb765fd8b919e3bd2cb15bc175c9498f9d9a0e216c2dde64d81255fa4c05a1ee619fc1fc505285a239e7bc655ec6605d9693078b800ee80931a7a0c84f33c851740
c = 0x50ae00623211ba6089ddfae21e204ab616f6c9d294e913550af3d66e85d0c0693ed53ed55c46d8cca1d7c2ad44839030df26b70f22a8567171a759b76fe5f07b3c5a6ec89117ed0a36c0950956b9cde880c575737f779143f921d745ac3bb0e379c05d9a3cc6bf0bea8aa91e4d5e752c7eb46b2e023edbc07d24a7c460a34a9a
e = 0xe6b1bee47bd63f615c7d0a43c529d219
#已知

n = y-x
phi = n-x+2
d = libnum.invmod(e,phi)
m = pow(c,d,n)
print(long_to_bytes(m))
```

>  [babyRSA.py](../../../../../PycharmProjects/pythonProject3/rsa 练习/babyRSA.py) 

## 四、Rabin加密 及RSA 中e=2

##### Rabin加密是一种基于模平方和模平方根的非对称加密算法。

a=x^2 mod m    称a为x模m时的平方，x为a模m时的平方根。

##### 1、加密过程

设私钥p q为两个素数，公钥n=p*q，对于明文m和密文c，定义一下加密过程：

c=m^2 mod n

##### 2、解密过程

根据以下公式计算出mp和mq:

mp = c^((1/4)(p+1)) mod p

mq = c^((1/4)(q+1)) mod q


根据以下公式推导出一个可用的yp和yq:

yp * p + yq * q = 1

根据以下公式计算最终结果：

r = (yp * p * mq + yq * q * mp) mod n

-r = n - r

s = (yp * p * mq - yq * q * mp) mod n

-s = n - s

可以证明每一个密文对应四个原文，而真正的原文一般需要根据验证码来对应。

CTF 中应用Rabin加密的题：https://www.jarvisoj.com/challenges (hardRSA)

解题思路：先用openssl解出参数，发现e=2，则说明用了Rabin加密，于是找脚本爆破即可。

```c = int(open('flag.enc','r').read().encode('hex'),16)
#print c
 
p = 275127860351348928173285174381581152299
q = 319576316814478949870590164193048041239
n = p*q
 
mp = pow(c,(p+1)/4,p)
mq = pow(c,(q+1)/4,q)
yp = gmpy.invert(p,q)
yq = gmpy.invert(q,p)
 
a = (yp*p*mq + yq*q*mp)%n
b = n - int(a)
c = (yp*p*mq - yq*q*mp)%n
d = n - int(c)
```

## 五、模不互素 知道两个n和一个e

这个题目就是两个`n`然后共用一个`e`，之后是两个密文
适用情况：存在两个或更多模数 ，且`gcd(N1,N2)!=1`。

多个模数`n`共用质数，则可以很容易利用欧几里得算法求得他们的质因数之一`gcd(N1,N2)`，然后这个最大公约数可用于分解模数分别得到对应的`p`和`q`，即可进行解密，解题脚本如下。

```python
n1=123213123
n2=8282828282
e = 0x10001

p1 = gmpy2.gcd(n1, n2)

p2=n1/p1
p3=n2/p1

d1 = gmpy2.invert(e, s1)
d2 = gmpy2.invert(e, s2)

m1 = pow(c1, d1, n1)
m2 = pow(c2, d2, n2)
```

### 六、共模攻击

一个`n`两个`e`，两个密文，然后`e1`与`e2`互质适用情况
明文`m`、模数`n`相同，公钥指数`e`、密文`c`不同，`gcd(e1,e2)==1`
对同一明文的多次加密使用相同的模数和不同的公钥指数可能导致共模攻击，题目内容及解题脚本如下。

```
#!/usr/bin/python
#coding:utf-8
import re
import gmpy2
import libnum
from Crypto.Util.number import long_to_bytes

e1 = 17
e2 = 65537
n = 
c1=
c2=


_, r, s = gmpy2.gcdext(e1, e2)
print(_, r, s)

# print(long_to_bytes(pow(c1, r, n)))
m = pow(c1, r, n) * pow(c2, s, n) % n

print(long_to_bytes(m))
```

### 七、低解密指数攻击 公钥 (e,n)偏弱  `e`过大或者过小

在`RSA`中`d`也称为揭秘指数，当`d`比较小的时候，`e`就显得特别大了。

> ##### 使用情况`e`过大或过小，在`e`过大或者过小的情况下，可使用算法快速推断出`d`的值，进而求出`m`。

```
#!/usr/bin/python
#coding:utf-8

import gmpy2
from Crypto.PublicKey import RSA
import ContinuedFractions, Arithmetic
from Crypto.Util.number import long_to_bytes

def wiener_hack(e, n):
    frac = ContinuedFractions.rational_to_contfrac(e, n)
    convergents = ContinuedFractions.convergents_from_contfrac(frac)
    for (k, d) in convergents:
        if k != 0 and (e * d - 1) % k == 0:
            phi = (e * d - 1) // k
            s = n - phi + 1
            discr = s * s - 4 * n
            if (discr >= 0):
                t = Arithmetic.is_perfect_square(discr)
                if t != -1 and (s + t) % 2 == 0:
                    print("Hacked!")
                    return d
    return False
def main():

	#已知
    n = 
    e = 
    c = 
    
    
    d = wiener_hack(e, n)
    m = pow(c,d,n)
    print long_to_bytes(m)
    
if __name__=="__main__":
    main()
```

### 八、知道只知道e，n切n无法分解，e非常大

这是在`buu`遇到的一个`rsa`很有意思，需要用到工具`rsa-wiener-attack-master`
题目给了一个`py`文件

```python
N = 101991809777553253470276751399264740131157682329252673501792154507006158434432009141995367241962525705950046253400188884658262496534706438791515071885860897552736656899566915731297225817250639873643376310103992170646906557242832893914902053581087502512787303322747780420210884852166586717636559058152544979471
e = 46731919563265721307105180410302518676676135509737992912625092976849075262192092549323082367518264378630543338219025744820916471913696072050291990620486581719410354385121760761374229374847695148230596005409978383369740305816082770283909611956355972181848077519920922059268376958811713365106925235218265173085
# d=8920758995414587152829426558580025657357328745839747693739591820283538307445
import hashlib
import wiener_hack
d=wiener_hack.wiener_hack(e,N)
print(d)
flag = "flag{" + hashlib.md5(hex(d).encode()).hexdigest() + "}"
print(flag)
```





