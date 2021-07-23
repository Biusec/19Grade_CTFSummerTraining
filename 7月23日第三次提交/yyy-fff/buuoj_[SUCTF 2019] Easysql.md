### buuoj_[SUCTF 2019] Easysql

首先随便测试几个数据看看情况

![](https://pic.imgdb.cn/item/60faa5765132923bf8ea96d5.jpg)

![](https://pic.imgdb.cn/item/60faa59c5132923bf8eaef35.jpg)

![](https://pic.imgdb.cn/item/60faa5b15132923bf8eb2315.jpg)

看样子没有报错注入可以用，查看源码也没看出什么东西

但是试了下堆叠注入发现可行

![](https://pic.imgdb.cn/item/60faa5e85132923bf8eba9e4.jpg)

但是后面就卡住了，用御剑扫了下也没发现什么东西，burp抓包也没看出什么花样

看了大佬的wp，发现还是水太深，我把握不住

通过输入非零字符返回1，就可以猜到语句含有||运算符，太强了，随后猜测语句是这样子

```
select $_GET['query'] || flag from flag 
```

通过输入*，1就可以达到构造语句

```
select *,1|| flag from flag -->select *,1 from flag 
```

即直接读出flag中所有内容

![](https://pic.imgdb.cn/item/60faa9435132923bf8f3f22c.jpg)

但是据说还有一种解法，输入

```
1;set sql_mode=pipes_as_concat;select 1
```

> sql_mode：是一组mysql支持的基本语法及校验规则
> PIPES_AS_CONCAT：将“||”视为字符串的连接操作符而非或运算符，这和Oracle数据库是一样的，也和字符串的拼接函数Concat相类似

学废了直接