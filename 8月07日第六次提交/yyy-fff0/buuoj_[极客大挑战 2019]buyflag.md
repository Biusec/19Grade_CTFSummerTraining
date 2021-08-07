### buuoj_[极客大挑战 2019]buyflag

首先进入界面查看源码，在pay.php界面发现源码

```
<!--
	~~~post money and password~~~
if (isset($_POST['password'])) {
	$password = $_POST['password'];
	if (is_numeric($password)) {
		echo "password can't be number</br>";
	}elseif ($password == 404) {
		echo "Password Right!</br>";
	}
}
-->
```

可以看到这个代码，要求password==404，同时要求密码不能是数字，但是看这里是双等于号，那么可以考虑弱比较，即令password==404%0a

而且post内容包括money和password，上面也说了buyflag的money==100000000，所以构造以下playload，但是没有回显，抓包看看

![](https://pic.imgdb.cn/item/610e81b25132923bf8d09f43.jpg)

可以看到这里user=0，我估计要改成1

然后post数据获得回显

![](https://pic.imgdb.cn/item/610e81fd5132923bf8d10bf0.jpg)

说数字太长，考虑用科学计数法

![](https://pic.imgdb.cn/item/610e823e5132923bf8d165dc.jpg)

完成

