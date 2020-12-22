# 工具篇

## clush

### 配置文件路径

clush会依次在下面的位置查找配置文件：
* /etc/clustershell/clush.conf
* $XDG_CONFIG_HOME/clustershell/clush.conf
* $HOME/.config/clustershell/clush.conf (only if $XDG_CONFIG_HOME is not defined)
* $HOME/.local/etc/clustershell/clush.conf

### 配置通过密码登录

修改上述任一配置文件，在`[Main]`下面配置`ssh_path`选项，例如：

```
ssh_path: /usr/local/bin/sshpass -p xxxx /usr/bin/ssh
```
如果密码保存在文件中，可以修改sshpass的参数指定，例如：

```
ssh_path: /usr/local/bin/sshpass -f /path/to/pass /usr/bin/ssh
```
