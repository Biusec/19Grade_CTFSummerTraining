# Bugku-16.easy-100

1.评论区说这题有手就行😅，对于我这种刚刚接触Java的小白还是有些困难的，首先jadx打开，搜索输入错误时的字符串“Oh no”，找到相关代码在d类，调用了MainActivity的a函数，第一个参数是MainActivity的string成员v，第二个参数是我们输入的内容，这个函数的返回值直接决定了我们的flag是否正确。

![16-1](https://github.com/OWORD/ctfimg/raw/main/Bugku/16.easy-100/img/16-1.png)

2.接下来看MainActivity类，其String成员v在onCreate函数中被间接赋值，值在url.png这张图片中，从第144字节起往后数16个，为“this_is_the_key.”，MainActivity类的a函数调用了c类的a函数，并将返回值与一串字节组成的字符串相比较。

![16-2](https://github.com/OWORD/ctfimg/raw/main/Bugku/16.easy-100/img/16-2.png)

3.接下来顺藤摸瓜去看c类，c类重载有2个a函数，一个public的有两个String参数，一个private的有一个String参数，private看样子只是对这个字符串进行简单的变换处理，public中申请了一个a类的对象，并将str参数经private的a函数处理后传给了a类对象的a函数，紧接着又把strt2转为字节传给了a类的b函数，并返回一个字符串

![16-3](https://github.com/OWORD/ctfimg/raw/main/Bugku/16.easy-100/img/16-3.png)

4.接着看a类，a类就是比较核心的类了，其有一个SecretKeySpec类的成员a，为密钥类，有一个Cipher类的成员b，为加解密类，在a函数中，是对其成员分别进行初始化设置，由于其参数一定不为空，则可知加密方法为AES加密，ECB模式，PKCS5Padding填充方式。b函数则为执行加密。

![16-4](https://github.com/OWORD/ctfimg/raw/main/Bugku/16.easy-100/img/16-4.png)

5.梳理代码，可以理解代码大致逻辑：该程序以经c类private的a函数处理后的“this_is_the_key.”为密钥，对我们的输入进行加密，要求加密后的字节数组必须是MainActivity类的a函数中的字节数组，我们自己将c类private的a函数抄下来运行，可以得到真正的密钥是“htsii__sht_eek.y”，因此，我们只需以此为密钥，对字节数组进行解密即可得到flag，为：LCTF{1t's_rea1ly_an_ea3y_ap4}