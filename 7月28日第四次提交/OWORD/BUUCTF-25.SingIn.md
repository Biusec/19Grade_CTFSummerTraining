# BUUCTF-25.SingIn

1.IDA打开，f5，shift+f12找字符串，定位得到以下代码：

```c
__int64 __fastcall main(int a1, char **a2, char **a3)
{
  char v4[16]; // [rsp+0h] [rbp-4A0h] BYREF
  char v5[16]; // [rsp+10h] [rbp-490h] BYREF
  char v6[16]; // [rsp+20h] [rbp-480h] BYREF
  char v7[16]; // [rsp+30h] [rbp-470h] BYREF
  char v8[112]; // [rsp+40h] [rbp-460h] BYREF
  char v9[1000]; // [rsp+B0h] [rbp-3F0h] BYREF
  unsigned __int64 v10; // [rsp+498h] [rbp-8h]

  v10 = __readfsqword(0x28u);
  puts("[sign in]");
  printf("[input your flag]: ");
  __isoc99_scanf("%99s", v8);
  str2asciistr(v8, (__int64)v9);
  __gmpz_init_set_str(v7, "ad939ff59f6e70bcbfad406f2494993757eee98b91bc244184a377520d06fc35", 16LL);
  __gmpz_init_set_str(v6, v9, 16LL);
  __gmpz_init_set_str(v4, "103461035900816914121390101299049044413950405173712170434161686539878160984549", 10LL);
  __gmpz_init_set_str(v5, "65537", 10LL);
  __gmpz_powm(v6, v6, v5, v4);
  if ( (unsigned int)__gmpz_cmp(v6, v7) )
    puts("GG!");
  else
    puts("TTTTTTTTTTql!");
  return 0LL;
}
```

2.str2asciistr函数是我自己改的名字，它的作用是把一串字符串转为16进制的ASCII码字符串，名字里带有gmp的函数是gmp库的，其为一个c的大数库，用于运算c不支持的大数字，在python平台有gmpy2。__gmpz_init_set_str函数为初始化gmp大数，第一个参数是返回的数的地址，第二个参数为要初始的大数的字符串地址，第三个参数为传入的大数的进制。该程序初始化了四个数。

__gmpz_powm函数类似于pow

该程序意思是输入flag，将其转为16进制ASCII码，并用于初始化一个大数v6，使得v6 ^ v5 mod v4 == v7，可以看出这是RSA加密的思路，那么，我们对v4，也就是RSA种的n进行质因分解，具体可用yafu或http://www.factordb.com/index.php，yafu可用指令factor(number)。得到p，q后，就可对n进行逆元运算得到私钥d，继而解得flag，具体代码如下

```python
import gmpy2
p = gmpy2.mpz("366669102002966856876605669837014229419")
q = gmpy2.mpz("282164587459512124844245113950593348271")
c = gmpy2.mpz("ad939ff59f6e70bcbfad406f2494993757eee98b91bc244184a377520d06fc35", 16)
e = gmpy2.mpz("65537")
n = gmpy2.mpz("103461035900816914121390101299049044413950405173712170434161686539878160984549")
phin = (p - 1) * (q - 1)
d = gmpy2.invert(e, phin)
#m = c^d mod n
m = pow(c, d, n)
print(hex(m))
#0x73756374667b50776e5f405f68756e647265645f79656172737d
#suctf{Pwn_@_hundred_years}
```

