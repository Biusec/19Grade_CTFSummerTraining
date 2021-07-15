### simple sst1_1

首先开启场景，发现这个界面是这样的

![image.png](https://i.loli.net/2021/07/12/ejvEDJCiF58zWOQ.png)

意思是需要一个名为flag的参数。查看页面源代码，有一句被注释的话，很明显flag就是SECRET_KEY

``` 
<!-- You know, in the flask, We often set a secret_key variable.-->
```

因为对flask框架已经忘得差不多了，所以查了一下

> 在flask框架中，使用session时设置密钥SECRET_KEY，通常使用SECRET_KEY
>
> app.config['SECRET_KEY']='xxx'
>
> 但是通常会单独使用一个config.py存放

对于这种的，可以直接构造playload:?flag={{ config.SECRET_KEY}}就可访问

至于为什么使用{{ }}

> 有一部分模板渲染使用双大括号
>
> 包括 Mako Jinja2 Twig等