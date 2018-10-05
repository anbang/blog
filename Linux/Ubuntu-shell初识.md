在linux早期,可以用来工作的只有shell;

在图形化之前,与unix系统进行交互的唯一方式就是借助由shell所提供的`文本命令行界面`(command line interface,CLI),CLI只能接受文本输入,也只能显示出文本和基本的图形输出

现在的linux发行版默认都是进入图形化界面(客户端),怎么进入CLI就是一个问题了;

我用的是ubuntu18

### 控制台终端访问CLI

在linux早期,在启动系统时候,只会在显示器上看到一个登录提示符';

在ubuntu18中,切换非常方便;

- 切换到CLI ,需要按下 `ctrl + alt + F3/F4/F5/F6`;
- 切换到图形化,需要按下 `ctrl + alt + F2`

`ctrl + alt + F1`是切换到除当前用户外的,其他用户,就是选择用户的那个界面;

文本模式的虚拟控制台采用全屏的方式显示文本登录界面;第一次进入,显示如下
```
Ubuntu 18.04.1 LTS x tty3
xlogin:
```
这时候输入账号密码即可进入(输入密码时,续集控制台什么都不显示,没有点号星号之类的东西);

注意上面的`tty3`,这个词代表这是虚拟控制台3,可以用过`ctrl+alt+F3`这个快捷键来进入,tty代表 `teletypewriter`电传打字机,表示用于发送消息的机器;

`注意`:在Linux虚拟控制台中无法运行任何图形化程序;

但是Ubuntu中,可以轻松切换CLI和GUI;

### 改变CLI的颜色

其实这是很无聊的,但是也是可以的

逆色显示

```
setterm inversescreen on
```

正常显示
```
setterm inversescreen off
```

设置前景色,设置后景色

```
setterm -background white
setterm -foreground black/red/green/yellow/blue/magenta/cyan/white
```