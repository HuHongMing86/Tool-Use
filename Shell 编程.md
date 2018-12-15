# Shell 编程

## 王道烩 2018.12.14

### Shell 脚本

Shell是一个用C语言写的程序，使用户使用Linux的桥梁。

Shell的编程环境需要一个文本编辑器和一个解释执行的脚本解释器。常用的Shell很多：

- ```/usr/bin/sh或/bin/sh```
- ``` /bin/bash```
- ``` /usr/bin/csh```

一般使用的是bash。`#!`告诉系统其后路径所指定的程序是解释此脚本文件的Shell程序。

### SHell变量

定义变量时，变量名不加美元符号，但是变量名和等号之间不能有空格。

但是使用变量的时候需要在变量前面加上美元符号。

```bash
your_name="wangdh"
echo $your_name
echo $(your_name)
```

**花括号是可选的，但是为了可读性，最好加上。**

已定义的变量可以重新定义，使用`readonly`命令能够将变量定义为只读变量，其值不能被修改。`unset`命令可以删除变量，但是不能删除只读变量。

```bash
myUrl="fdafdsa"
readonly myUrl
```

#### 变量类型会有三种变量；

- 局部变量：仅在当前shell实例中有效，局部变量在脚本或命令中定义。
- 环境变量：所有程序都能够访问环境变量。
- shell变量：shell程序设置的特殊变量，一部分是环境变量，一部分是局部变量。

#### Shell字符串

shell常用的数据类型是字符串和数字。字符串可以用单引号也可以用双引号。

其中双引号可以有变量，可以出现转义字符：

```bash
your_name="wangdh"
greeting="hello ${your_name} !"
```

#### 获取字符串长度

```bash
string="abcd"
echo ${#string}
```

提取字符串

```bash
string="hello, world !"
echo ${string:1:4}  # 输出ello
```

查找子字符串

```bash
string="hello, world!"
echo `expr index $string io` 
```

#### Shell 数组

bash支持一维数组，没有限定大小。从0开始编号。括号表示数组，数组元素用空格分割开。

`数组名=(值1 值2 值3 ... 值n)`

```bash
array_naem=(value0 balue1 value2)
${array_name[0]}
```

#### Shell注释

以#开头的都是注释，如果是多行注释，那么可以使用一对花括号括起来，定义成一个函数，没有地方调用这个函数，这段代码就不会执行。



### Shell传递参数

在执行脚本时，可以向脚本传递参数。脚本内获得参数的方法是`$n`，n为数字，1为第一个参数，0位命令本身。

```bash
echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```



还有几个比较特别的参数：

| 参数 | 说明                                                  |
| ---- | ----------------------------------------------------- |
| $#   | 传递到J脚本的参数的个数                               |
| $*   | 以一个字符串显示所有向脚本传递的参数                  |
| $$   | 脚本运行的当前进程的ID号                              |
| $!   | 后台运行的最后一个进程的ID号                          |
| $@   | 与$*相同，但是使用时加引号，在引号中返回每个参数      |
| $?   | 显示最后命令的退出状态，0表示没有错误，其他表示有错误 |

**$*与$@的区别**

- 都是引用所有的参数。
- 只有在双引号中能够体现出来，假设在脚本运行时写了三个参数1、2、3，则“*”等价于“1 2 3”(传递了一个参数)，“@”等价于“1” “2” “3” ,(传递了三个参数)。

```bash
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

echo "-- \$* 演示 ---"
for i in "$*"; do
    echo $i
done

echo "-- \$@ 演示 ---"
for i in "$@"; do
    echo $i
done
```

执行脚本输出结果：

```bash
$ chmod +x test.sh 
$ ./test.sh 1 2 3
-- $* 演示 ---
1 2 3
-- $@ 演示 ---
1
2
3
```

### Shell 数组

Shell中只支持一维数组，初始化不需要制定数组的大小。下标由0开始，用括号表示，元素用空格分割开.

读取数组一般格式是：

```bash
${array_name[index]}
```

```bash
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

my_array=(A B "C" D)

echo "第一个元素为: ${my_array[0]}"
echo "第二个元素为: ${my_array[1]}"
echo "第三个元素为: ${my_array[2]}"
echo "第四个元素为: ${my_array[3]}"
```

@或*可以获得数组中所有元素：

```bash
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

my_array[0]=A
my_array[1]=B
my_array[2]=C
my_array[3]=D

echo "数组的元素为: ${my_array[*]}"
echo "数组的元素为: ${my_array[@]}"
```

```bash
$ chmod +x test.sh 
$ ./test.sh
数组的元素为: A B C D
数组的元素为: A B C D
```

### Shell 基本运算符

expr是一款计算表达式工具：

```bash
val=`expr 2 + 2`
echo $val
```

**注意：**

- 表达式和运算符之间要有空格，2 + 2

关系运算符

-eq -ne -gt -lt -ge -le

布尔运算符

！ -a -o

逻辑运算符

&&  ||

字符串运算符

=  !=  -z   -n  str

文件测试运算符

-w -x 

### Shell echo命令

echo 用于字符串的输出。`read`命令从标准输入中读取一行，并赋给指定的Shell变量。

反引号``用于指定命令。

`test`命令用于检查某个条件是否成立。与[]相同，但是[] 两边要加空格。

### Shell 流程控制

```bash
if condition
then
    command1 
    command2
    ...
    commandN 
fi
```

```bash
if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi
```

```bash
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

```bash
while condition
do
    command
done
```

```bash
until condition
do
    command
done
```

```bash
case 值 in
模式1)
    command1
    command2
    ...
    commandN
    ;;
模式2）
    command1
    command2
    ...
    commandN
    ;;
esac

echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac
```

break跳出循环，continue跳出当次循环。

### Shell 函数

```bash
[ function ] funname [()]

{

    action;

    [return int;]

}
```

- funciton 可用可无
- 参数返回：如果有return 可以返回，后面跟数值0-255， 如果不加，将最后一条命令的结果返回。

函数使用参数用$n 来使用。还可以使用$#等参数。

### Shell 输入输出重定向

`command > file`输出重定向到file。

`command < file `输入重定向到file。

\>> 表示追加。

**文件描述符0表示标准输入(STDIN)，1表示标准输出(STDOUT)，2表示标准错误输出(STDERR)。**

 

一般情况下，每个Unix/Linux命令运行时都会打开三个文件；

- 标准输入文件：stdin的文件描述符为0。

- 标准输出文件：stdout的文件描述符为1

- 标准错误文件：stderr的文件描述符为2

  stdout有缓冲区，stderr没有缓冲区，默认都是输出到屏幕上。

如果希望将stdout和stderr合并后输出到file，可以这样写：

```bash
$ command > file 2>&1

或者

$ command >> file 2>&1
```

#### /dev/null 文件

这是一个特殊的文件，写到它内容都会被丢弃，如果尝试从该文件中读取内容，那么什么都读不到。将命令的输出重定向到它，可以起到禁止输出的作用。

```bash
$ command > /dev/null 2>&1
```

### Shell 文件包含

Shell 可以包含外部脚本，方便地封装一些公用的代码作为一个独立的文件。文件包含的语法格式如下：

```bash
. filename   # 注意点号(.)和文件名中间有一空格

或

source filename
```

