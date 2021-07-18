# Bugku-6.Timer

1.该程序是一个计时器程序，从200000开始倒数，猜测倒数结束后显示flag

![6-1](https://github.com/OWORD/ctfimg/raw/main/Bugku/6.Timer/img/6-1.jpg)

2.了解到一个分析apk很方便的工具：jadx，用jadx打开apk后，查看MainActivity，验证了之前的猜想，并且最终flag的生成需要一个参数k，k在每次计时器计数时都要调用is2()函数修改，最终调用stringFromJNI2()函数获取flag，而stringFromJNI2()是一个native函数，就是它的实现在so库内，因此我们可以模拟一个is2函数算出计时结束时的k值，再修改smali代码，使其直接显示flag

![6-2](https://github.com/OWORD/ctfimg/raw/main/Bugku/6.Timer/img/6-2.png)

3.计算k值的代码如下

```c++
#include <stdio.h>
bool Is2(int param)
{
    if(param <= 3)
        return (param > 1);
    if(param % 2 == 0 || param % 3 == 0)
        return false;
    int b = 5;
    while(true)
    {
        if(b * b <= param){
            if(param % b == 0 || param % (b + 2) == 0)
                return false;
            b += 6;
            continue;
        }
        return true;
    }
}

int main(void)
{
    unsigned int k = 0;
    for(unsigned int i = 200000; i > 0; i--)
    {
        if(Is2(i))
            k += 100;
        else
            k--;
    }
    printf("%u", k);
    return 0;
}
```

计算得到k=1616384

4.需要修改的smali代码在MainActivity$1.smali文件中，首先找到"the flag is"，相关代码就在周围

![6-4](https://github.com/OWORD/ctfimg/raw/main/Bugku/6.Timer/img/6-4.png)

首先可以在上方发现一条if-gtz指令，意为大于0则跳转，为判断剩余时间的代码，我们改为if-lez，小于等于0跳转。其次在下方有一条iget指令，将k的值赋给v3寄存器，下面调用stringFromJNI2()，因此v3为stringFromJNI2()的参数，我们将iget这行指令改为计算得到的k值：const v3, 1616384，保存

5.修改完成后，apktool回编译：

```shell
java -jar apktool.jar b Timer Timer.apk
```

再进行签名，这里记录下签名的过程，首先使用keytool生成存储着密钥和证书的.keystore文件： 

```shell
keytool -genkey -keystore ./Timer.keystore -alias Timer -keyalg RSA -validity 10000
```

命令解释：

Keytool 选项 描述
-genkey 产生一个键值对（公钥和私钥）
-v 允许动作输出
-alias 键的别名。只有前八位字符有效。
-keyalg 产生键的加密算法。支持DSA和RSA。
-keysize 产生键的长度。如果不支持，keytool用默认值1024 bits.通常我们用2048 bits 或更长的key。
-dname 专有名称，描述谁创建的密钥。该值被用作自签名证书的颁发者和主题字段。注意你可以不在命令行指定。如果没有指定keytool会提示你(CN,
OU, and so on)。
-keypass 键的密码。 主要为了安全起见，如果没提供，keytool会提示你输入。
-validity 键的有效期，单位：天
-keystore.keystore 用于存储私钥的文件。

再使用jarsigner对apk签名：

```shell
jarsigner -verbose -keystore ./Timer.keystore -signedjar ./Timer2.apk ./Timer.apk Timer
```

-keystore 使用的keystore文件

-signedjar 目标生成的apk文件

最后的Timer为别名

6.生成apk后，安装打开，即可得到flag，按题目要求换为flag{}格式，则flag：flag{Y0vAr3TimerMa3te7}

![6-6](https://github.com/OWORD/ctfimg/raw/main/Bugku/6.Timer/img/6-6.jpg)