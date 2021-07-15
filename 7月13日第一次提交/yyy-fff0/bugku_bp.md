### bugku_bp 

打开一看，熟悉的登陆界面，而且看题目提示，猜测是首位字母为z的6位数密码

随便输入几个密码，没猜到，用bp先抓包然后发送

![image.png](https://i.loli.net/2021/07/12/GgbMPdsT2QwYJUD.png)

得到了这样一串代码

```
console.log()	#在控制台上输出信息
```

可以看见，这段代码的意思就是，当r.code != ‘bugku10000' 的时候，就获得了正确密码，因此可以设置正则匹配来爆破

![image.png](https://i.loli.net/2021/07/12/saPovc42duSwCq5.png)

![image.png](https://i.loli.net/2021/07/12/ylQNfIq7FMRP3tJ.png)

![image.png](https://i.loli.net/2021/07/12/Eop86H957GWTiMX.png)

看到一共有六千多万种可能。。。感觉两小时估计跑不出来

最后猜了个密码，得到flag