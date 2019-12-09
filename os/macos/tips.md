<link rel="stylesheet" type="text/css" href="/auto-number.css">

# MacOS Tips

## 系统配置

* 禁止后台下载更新
```bash
sudo defaults write /Library/Preferences/com.apple.SoftwareUpdate AutomaticDownload -boolean FALSE
```

## 软件配置
### Java Home路径

在我的电脑上（mojave 10.14.6）安装了`jre-8u211-macosx-x64.dmg`，但是我命令行执行`java -version`的时候系统报错，说找不到java runtime。

```
No Java runtime present, requesting install.
```

用`java_home`找了一下，确实找不到：

```bash
$ /usr/libexec/java_home -V
Unable to find any JVMs matching version "(null)".
Matching Java Virtual Machines (0):

Default Java Virtual Machines (0):

No Java runtime present, try --request to install.
```

在网上找了一番，是说，JRE可能在两个位置：

* /Library/Java/JavaVirtualMachines
* /Library/Internet Plug-Ins/JavaAppletPlugin.plugin/Contents

于是分别在两个位置找了一下，发现果然在`Internet Plug-Ins`目录下，然后修改bash配置文件，添加如下的内容：

```bash
export JAVA_HOME="/Library/Internet Plug-Ins/JavaAppletPlugin.plugin/Contents/Home"
export PATH=$JAVA_HOME/bin:$PATH
```

然后重新source一下，再次执行`java -version`，结果就正常了。
