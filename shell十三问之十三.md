---
title: shell十三问之十三  [转载]
date: 2017-04-02 20:49:59
tags: [linux,shell]
categories: shell十三问
---


 
<blockquote class="blockquote-center">[^ ] 跟[! ]差在哪？ (wildcard)
</blockquote>


这个题目说穿了，
就是要探讨Wildcard与Regular Expression的差别的。
这也是很多初学shell的朋友很容易混淆的地方。

<!-- more -->

首先，让我们回到十三问之第2问，
再一次将我们提到的command line format 温习一次：

```shell
command_name options arguments
```

同时，也再来理解一下，我在第5章所提到的变量替换的特性：
```shell
先替换，再重组 command line!
```

有了这个两个基础后，再让我们来看Wildcard是什么回事吧。


### Part-I Wildcard （通配符）
---------------------------

首先，
```
`Wildcard` 也是属于 `command line` 的处理工序，作用于 `arguments` 里的 `path` 之上。
```

没错，它不用在`command_name`，也不用在`options`上。
而且，若argument不是path的话，那也与wildcard无关。

换句更为精确的定义来讲，
```   
 `wildcard`是一种命令行的路径扩展(path expansion)功能。
```
提到这个扩展，那就不要忘了 command line的“重组”特性了！

是的，这与`变量替换`(variable subtitution)及
`命令替换`(command substitution)的重组特性是一样的。

也就是在`wildcard`进行扩展后，
命令行会先完成重组，才会交给shell来处理。


了解了`wildcard`的扩展与重组特性后，
接下来，让我们了解一些常见的wildcard吧。 


|wildcard   | 功能               |
|-----------|--------------------|
| \*        | 匹配0个或多个字符|
| ?         | 匹配任意单一字符|
| [list]    | 匹配list中任意单一字符|
| [!list]   | 匹配不在list中任意单一字符|
| {string1,string2,...}| 匹配string1或者stsring2或者(...)中其一字符串|


Note:
  list 中可以指定单个字符，如abcd, 
 也可以指定ASCII字符的起止范围，如 a-d。
  即[abcd] 与 [a-d] 是等价的，称为一个自定义的字符类。


例如：
```shell
a*b     # a 与 b 之间可以有任意个字符（0个或多个），如aabcb, axyzb, a012b,ab等。
a?b     # a 与 b 之间只能有一个字符，但该字符可以任意字符，如 aab, abb, acb, azb等。
a[xyz]b # a 与 b 之间只能有一个字符，但这个字符只能是x或者y或者z，如：axb, ayb, azb这三个。
a[!0-9]b# a 与 b 之间只能有一个字符，但这个字符不能是阿拉伯数字，如aab，ayb，a-b等。
a{abc,xyz,123}b # a 与 b之间只能是abc或者xyz或者123这三个字串之一，扩展后是aabcb，axyzb，a123b。 
```


1. `[! ]` 中的`!` 只有放在第一位时，才有取反的功效。
  eg:
    `[!a]*` 表示当前目录下不以a开头的路径名称；
    `/tmp/[a\!]*`表示/tmp目录下所有以a 或者 ! 开头的路径名称；
    
    思考：为何!前面要加\呢？提示是十三问之4.

2. `[ - ]`中`-`左右两边均有字符时，才表示一个范围，否则,仅作`-`(减号)字符来处理。
举例：
    `/tmp/*[-z]/[a-zA-Z]*` 表示/tmp 目录下所有以z或者-结尾的子目录中，
    以英文字母(不分大小写)开头的目录名称。

3. 以\*或?开头的wildcard不能匹配隐藏文件(即以.开头的文件名)。
 eg: 
    `*.txt`并不能匹配`.txt`但能匹配1.txt这样的路径名。
    但1*txt及1?txt均可匹配1.txt这样的路径名。


基本上，要掌握wildcard并不难，
只要多加练习，再勤于思考，就能灵活运用了。

再次提醒：
```
别忘了wildcard的"扩展" + "重组" 这个重要特性，而且只作用在 argument的path上。
```
比方说，
假如当前目录下有：
a.txt b.txt c.txt 1.txt 2.txt 3.txt 这几个文件。

当我们在命令行中执行`ls -l [0-9].txt`的命令行时，
因为wildcard处于argument的位置上，

于是根据匹配的路径，扩展为: 1.txt 2.txt 3.txt，
在重组出`ls -l 1.txt 2.txt 3.txt` 这样的命令行。

因此，你在命令行上敲 `ls -l [0-9].txt` 
与 `ls -l 1.txt 2.txt 3.txt` 输出的结果是一样，
原因就是在于此。

## shell是十三问的总结语
------------------------

好了，该是到了结束的时候了。
婆婆妈妈地跟大家啰嗦了一堆shell的基础概念。

目的不是要告诉大家“答案”，而是要带给大家“启发”...

在日后的关于shell的讨论中，我或许经常用"连接"的方式
指引十三问中的内容。

以便我们在进行技术探讨时，彼此能有一些讨论的基础，
而不至于各说各话、徒费时力。

但更希望十三问能带给你更多的思考与乐趣，
至为重要的是通过实践来加深理解。

是的，我很重视**实践**与**独立思考**这两项学习要素。

若你能够掌握其中的真谛，那请容我说声：
**恭喜十三问你没白看了** ^_^


p.s.
至于补充问题部分，我暂时不写了。
而是希望：

1. 大家补充题目。
2. 一起来写心得。

Good luck and happy studing！

--------------------------------

##shell十三问原作者**`网中人`**签名中的bash的fork bomb
--------------

最后，Markdown整理者补上本书的原作者**网中人**的个性签名：

> ** 君子博学而日叁省乎己，则知明而行无过矣。**

> 一个能让系统shell崩溃的shell 片段：

```shell
:() { :|:& }; :      # <--- 这个别乱跑！好奇会死人的！
echo '十人|日一|十十o' | sed 's/.../&\n/g'   # <--- 跟你讲就不听，再跑这个就好了...
```


原来是一个bash的fork炸弹：ref：http://en.wikipedia.org/wiki/Fork_bomb

整理后的代码：
```shell
:() {
	
	:|:&
}
:
```
> 代码分析：

> (即除最后一行外)

> 定义了一个 shell 函数，函数名是`:`，

> 而这个函数体执行一个后台命令`:|:` 

> 即冒号命令(或函数，下文会解释)的输出
通过管道再传给冒号命令做输入

>最后一行执行“:”命令

在各种shell中运行结果分析：
> 这个代码只有在 **bash** 中执行才会出现不断创建进程而耗尽系统资源的严重后果;

> 在 ksh (Korn shell), sh (Bourne shell)中并不会出现，

> 在 ksh88 和传统 unix Bourne shell 中冒号不能做函数名，

> 即便是在 unix-center freebsd 系统中的 sh 和 pdksh（ksh93 手边没有，没试）中冒号可以做函数名，但还是不会出现那个效果。


>原因是 sh、ksh 中内置命令的优先级高于函数，所以执行“:”，
总是执行内置命令“:”而不是刚才定义的那个恐怖函数。

> 但是在 **bash** 中就不一样，bash 中函数的优先级高于内置命令，
> 所以执行“:”结果会导致不断的递归，而其中有管道操作，
> 这就需要创建两个子进程来实现，这样就会不断的创建进程而导致资源耗尽。


众所周知，bash是一款极其强大的shell，提供了强大的交互与编程功能。

这样的一款shell中自然不会缺少“函数”这个元素来帮助程序进行模块化的高效开发与管理。
于是产生了由于其特殊的特性，bash拥有了fork炸弹。

Jaromil在2002年设计了最为精简的一个fork炸弹的实现。

> 所谓fork炸弹是一种恶意程序，它的内部是一个不断在fork进程的无限循环.

> fork炸弹并不需要有特别的权限即可对系统造成破坏。

> fork炸弹实质是一个简单的递归程序。

> 由于程序是递归的，如果没有任何限制，

> 这会导致这个简单的程序迅速耗尽系统里面的所有资源.




 










 








