### bugku_game1

点进页面，F12查看源码，然后发现了这样的代码

![image.png](https://i.loli.net/2021/07/13/lzJYGOiZRUmwKp6.png)

明显有个score.php的界面，可以看见sign部分就是base64编码

但是随手构造了一句

```
score.php?score=99999&ip=117.174.212.63&sign=OTk5OTk=
```

发现不行

然后进行了一场游戏

发现有一项这样的请求

![image.png](https://i.loli.net/2021/07/13/QmOgsWHNCi1lBVK.png)

发现好像是base64编码的结构错了，修改了一下重新试试

```
score.php?score=99999&ip=117.174.212.63&sign=zMOTk5OTk===
```

成功了

