# KCTF-1.马到成功

1.jadx打开，类中有一个String类成员a，onCreate方法中调用了其构造函数，参数为“65544231587a52794d3138316458417a636c396d4e44553343673d3d”

![1-1](https://github.com/OWORD/ctfimg/raw/main/KCTF/1.马到成功/img/1-1.png)

2.将a的构造函数抄下来，运行得到”eTB1XzRyM181dXAzcl9mNDU3Cg==“，猜测是base64加密，解密后得到flag：y0u_4r3_5up3r_f457