<link rel="stylesheet" type="text/css" href="/auto-number.css">

# awk基础

## 常见问题

### 带缓存输出

awk的print函数在输出时是先输出到buffer，这使得awk输出到标准输出时和输出到管道时表现不一样。例如：
```
while true
do
    clush -l root  -g sql_px uptime
        sleep 1
done |  awk '{printf("{\"load\":%f}\n",$NF)}'
```
通过上面的命令，可以持续的观察到`sql_px`这组服务器的15分钟的平均负载。
但是如果将命令的输出改为管道，如下所示：
```
while true
do
    clush -l root  -g sql_px uptime
        sleep 1
done |  awk '{printf("{\"load\":%f}\n",$NF)}' | jq
```
上面的命令将awk的输出作为`jq`的输入，通过`jq`来格式化输出，但这时候，好久都看不到任何输出。

经过一番查找，发现原来是awk的输出具有缓冲效果，也就是说awk的输出如果是管道，awk不会立即将内容输出到管道，而是缓存起来，积累到一定的量再输出，从而导致管道后续的没有没有拿到输入。

那有什么解决办法呢？我尝试了两种方法，最后都有效。

方法1：将awk的print函数的输出重定位到文件，然后通过tail读文件。具体如下：

```
while true
do
    clush -l root  -g sql_px uptime
        sleep 1
done |  awk '{printf("{\"load\":%f}\n",$NF) > "bbb"}'

tail -f bbb | jq
```
上面的方法从实际测试效果看是没有问题的。不过原理上来说，还是有风险。

方法2：在print函数后面调用fflush，强制输出。具体如下：
```
#!/bin/bash

while true
do
    clush -l root  -g sql_px uptime
        sleep 1
done |  awk '{printf("{\"load\":%f}\n",$NF); fflush("")}' | jq
```
因为调用了`fflush`函数，所以，awk的输出会立即执行。这个方法相对稳妥。
