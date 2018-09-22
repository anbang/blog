VScode默认使用cmd作为默认终端；

不怎么喜欢用；

我改成了

`git-bash.exe` 作为默认的终端

方法如下：

把下面的设置，配置到覆盖默认的设置

```bash
"terminal.external.windowsExec": "D:\\git\\Git\\git-bash.exe",
"terminal.integrated.shell.windows": "D:\\git\\Git\\git-bash.exe"
```

重启下就可以了