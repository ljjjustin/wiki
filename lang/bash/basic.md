<link rel="stylesheet" type="text/css" href="/auto-number.css">

# Bash基础

## 语法

### 变量
### 表达式
### 函数

## 常见操作
### 字符串处理

#### 字符串比较


#### 字符串长度

通过变量表达式`${#变量名}`

```
$ str1="abcde"
$ echo ${#str1}
5
```

#### 提取子字符串

`${变量名:起始位置:长度}`
提取从起始位置开始，指定长度的子字符串；不影响原变量

```
$ str="helloworld"
$ echo ${str:2:3}
llo
```
`${变量名#正则表达式}`
删除变量头部匹配正则表达式的子串，匹配时是最短匹配；不影响原变量

```
$ fullpath='/tmp/test/aaa.txt'
$ echo ${fullpath#/}  # 删除变量开头的 /
tmp/test/aaa.txt

$ echo ${fullpath#/*}
tmp/test/aaa.txt
```
如上面的例子所示，因为采用了最短匹配，所以`/`后面的星号没有起作用。

`${变量名%子字符串正则表达式}`
删除变量尾部匹配正则表达式的子串，匹配时是最短匹配；不影响原变量

```
$ fullpath='/tmp/test/aaa.txt'
$ echo ${fullpath%.txt}  # 删除.txt后缀
/tmp/test/aaa

$ echo ${fullpath%/*}    # 删除变量尾部第一个/后面所有的内容，类似dirname的功能
/tmp/test

$ echo ${fullpath%*}
/tmp/test/aaa.txt
```
上面最后一个例子，星号是匹配0个、1个或多个任意字符，因为最短匹配，所以，这里不会匹配任何字符。


`${变量名##正则表达式}`
删除变量头部匹配正则表达式的子串，匹配时是最短匹配；不影响原变量

```
$ fullpath='/tmp/test/aaa.txt'
$ echo ${fullpath##/}     # 删除变量开头的 /
tmp/test/aaa.txt

$ echo ${fullpath##/*aaa} # 删除变量开头的 / 与 aaa 之间所有的字符
.txt

$ echo ${fullpath##/*}    # 删除变量开头的 / 到行尾所有的字符
```
`##`区别于`#`的地方就是采用最长匹配还是最短匹配。


`${变量名%%正则表达式}`
删除变量尾部匹配正则表达式的子串，匹配时是最短匹配；不影响原变量

```
$ fullpath='/tmp/test/aaa.txt'
$ echo ${fullpath%%/aaa.*}     # 删除变量尾部匹配 /aaa.* 的子串
tmp/test/aaa.txt

$ echo ${fullpath##/*}         # 删除变量尾部匹配 /* 的子串

```
上面最后的例子中，因为`/*`匹配了所有内容，所以删除之后，就是空字符串了

#### 字符串替换

${变量/查找/替换值}
用`替换值`替换变量中匹配`查找`部分的字符串；不影响原变量；

```
$ fullpath='/tmp/test/aaa/bbb/aaa.txt'
$ echo ${fullpath/aaa/bbb}
/tmp/test/bbb/bbb/aaa.txt

$ echo ${fullpath/aaa/bbb/ccc}
/tmp/test/bbb/ccc/bbb/aaa.txt

$ echo ${fullpath/aaa/bbb/ccc/ddd}
/tmp/test/bbb/ccc/ddd/bbb/aaa.txt
```
如上面的例子所示，变量的替换只会进行一次，只有第一个匹配的子串被替换。

#### join字符串
通过某个字符来连接一组字符串是一个非常常见的操作，join字符串一个非常便捷的方法就是临时改变`IFS`
具体如下：

```
$ joinstr() { local IFS=$1; shift; echo "$*"; }
$ joinstr / aa bb cc dd
aa/bb/cc/dd
```
### 文件操作

