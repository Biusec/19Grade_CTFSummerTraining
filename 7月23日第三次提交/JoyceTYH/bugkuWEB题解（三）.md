**1.game1**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723205952966.png" alt="image-20210723205952966" style="zoom:50%;" />

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723205855148.png" alt="image-20210723205855148" style="zoom:50%;" />

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723205934099.png" alt="image-20210723205934099" style="zoom:50%;" />

查看源代码：

![img](C:\Users\TYH\Desktop\1a85e264aa99fddcca978ccca633c08f.png)

发现唯一加密的是一个sign,去原页面查看源码搜索sign,其值为zM+base64(score)+==

brupsuite拦截请求修改报文提交结果,获得flag

![img](https://ctf.bugku.com/upload/tmp/20210426/5097c306166881f5c2ff245d001be55e.png)



-----



**2.你从哪里来**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723210711295.png" alt="image-20210723210711295" style="zoom:50%;" />

![image-20210723210320091](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723210320091.png)

![image-20210723210617006](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723210617006.png)

[参照]: https://www.sojson.com/blog/58.html



-----



**3.login1**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723212907063.png" alt="image-20210723212907063" style="zoom:50%;" />

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723211742931.png" alt="image-20210723211742931" style="zoom:50%;" />

SQL约束攻击真正利用起来，存在以下的约束条件：

1. 服务端没有对用户名长度进行限制。如果服务端限制了用户名长度就不能导致数据库截断，也就没有利用条件。 
2. 登陆验证的SQL语句必须是用户名和密码一起验证。如果是验证流程是先根据用户名查找出对应的密码，然后再比对密码的话，那么也不能进行利用。因为当使用 Dumb为用户名来查询密码的话，数据库此时就会返回两条记录，而一般取第一条则是目标用户的记录，那么你传输的密码肯定是和目标用户密码匹配不上的。
3. 验证成功后返回的必须是用户传递进来的用户名，而不是从数据库取出的用户名。因为当我们以用户Dumb和密码123456登陆时，其实数据库返回的是我们 自己的用户信息，而我们的用户名其实是[admin ]，如果此后的业务逻辑以该用户名为准，那么就不能达到越权的目的了。  

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723213047730.png" alt="image-20210723213047730" style="zoom:50%;" />

[参照]: https://www.cnblogs.com/vincy99/p/9642941.html
[参照]: https://blog.csdn.net/wy_97/article/details/77972375



-----



**4.前女友**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723213648968.png" alt="image-20210723213648968" style="zoom:50%;" />

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723213551029.png" alt="image-20210723213551029" style="zoom:67%;" />

[参照]: https://blog.csdn.net/weixin_43578492/article/details/95743049
[参照]: https://blog.csdn.net/CliffordR/article/details/82972869
[参照]: https://blog.csdn.net/m0_37444209/article/details/92020514



-----



**5.eval**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723215706281.png" alt="image-20210723215706281" style="zoom:67%;" />



![image-20210723215714205](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723215714205.png)

[参照]: https://blog.csdn.net/zyl_wjl_1413/article/details/83753231
[参照]: https://blog.csdn.net/huangming1644/article/details/82818482
[参照]: https://blog.csdn.net/baidu_35297930/article/details/82874813?spm=1001.2014.3001.5501



-----



**6.Simple_SSTI_1**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723214312232.png" alt="image-20210723214312232" style="zoom:67%;" />

![image-20210723214328855](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723214328855.png)

[参照]: https://blog.csdn.net/u011377996/article/details/86776181
[参照]: https://blog.csdn.net/qq_37865996/article/details/102365374
[参照]: https://blog.csdn.net/jiuyongpinyin/article/details/117355063



-----



**7.Simple_SSTI_2**

![image-20210723220100185](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723220100185.png)



![image-20210723220116833](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723220116833.png)

![20210312000927585](C:\Users\TYH\Desktop\20210312000927585.png)

[参照]: https://xz.aliyun.com/t/3679#toc-9
[参照]: https://www.freebuf.com/column/187845.html



-----



**8.秋名山车神**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723220736129.png" alt="image-20210723220736129" style="zoom:67%;" />

![image-20210723220722669](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210723220722669.png)

[参照]: https://blog.csdn.net/weixin_43578492/article/details/101701339
[参照]: https://blog.csdn.net/weixin_44953600/article/details/107930323



