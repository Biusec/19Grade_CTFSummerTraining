1.

![image-20210716094307856](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210716094307856.png)

 ![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713201510404-1214007146.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)

提示：有输入验证且只允许一位输入的限制条件

解题方法：

a)   修改最大长度；

b)   寻找判断处的条件；

![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713201541488-1884021618.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image003.png)

 ![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713201551181-783044083.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image004.png)

 

2.

![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713201606964-500947104.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image005.png)![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713201614898-570335584.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image006.png)

以GET方式获取参数

is_numeric（）函数是判断是否为数字或者数字字符串

所以不能是数字或者数字字符串，但是下面$num == 1，要求为数字1

所以构造1+任意字符 比如1awd

![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713201640764-513943207.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image007.png)

补充：

1. 弱类型语言与强类型语言：

　　　　强类型：指任何变量在使用的时候必须要指定这个变量的类型，而且在程序的运行过程中这个变量只能存储这个类型的数据。因此，对于强类型语言，一个变量不经过强制转换，它永远是这个数据类型，不允许隐式的类型转换。常见的有C++,Pyton、Java等。

　　　　弱类型则是与强类型定义相反。常见：PHP、VB。

　2. PHP中 “== ” 与 “=== ”的区别：

 　　“ == ”：只是检测左右两边的值是否相等。在1w232 == 1中，1w232被强制转换成了整型1，则两者相等。

 　　“=== ”：操作符除了检测左右两边的值是否相等外，还检测他们的类型是否相等。

参照：

PHP的比较运算符：https://www.php.net/manual/zh/language.operators.comparison.php

PHP的类型比较表：https://www.php.net/manual/zh/types.comparisons.php

 

3.

![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713201715600-66894302.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image008.png)

 ![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713201724079-1114493536.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image009.png)

 4.

![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713201736050-821818293.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image010.png)

![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713201743577-859677634.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image011.png)

![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713201752732-2131289269.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image012.png)

![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713201807934-1181465709.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image013.png)

参照：

http://fairysoftware.com/post_content_type.html

 

5.

![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713201849081-1139117977.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image014.png)

直接查看源代码即可

 

6.

![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713202136251-2057192113.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image015.png)

网页不停闪烁

 ![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713202155453-261363255.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image016.png)

 多次抓包

![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713202212261-929555104.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image017.png)

 

7.

![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713202343344-2001012390.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image018.png)

查看源代码，发现底部一串Unicode编码，解码即可http://tool.chinaz.com/tools/unicode.aspx

<!-- &#102;&#108;&#97;&#103;&#123;&#56;&#97;&#48;&#49;&#49;&#50;&#56;&#49;&#54;&#53;&#50;&#51;&#51;&#57;&#51;&#97;&#56;&#50;&#97;&#99;&#49;&#99;&#99;&#99;&#99;&#55;&#98;&#53;&#101;&#52;&#100;&#97;&#125; -->

![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713202417591-105382720.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image019.png)

 

8.

![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713202227164-739788506.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image020.png)

 ![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713202236554-925931337.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image021.png)

![https://img2020.cnblogs.com/blog/2146423/202107/2146423-20210713202243991-918835222.png](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image022.png)

参照：

https://blog.csdn.net/qq_38603541/article/details/85106740

https://blog.csdn.net/weixin_43578492/article/details/94844543

https://shentuzhigang.blog.csdn.net/article/details/95034922

 

 