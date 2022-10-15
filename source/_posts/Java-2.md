---
title: Java - 第一个程序：Hello, World!
date: 2016-10-30 11:14:36
tags:
- Java
categories:
- Code
---

# 写在前面
`Hello, World!`可谓是程序界最著名的程序了，几乎每本书的第一个程序都是输出一行“Hello, World!”。理由很简单，能让学习者体会到成就感，而且一般编程语言的标准输出都不是很难。那么今天我们就要写第一个Java程序：`Hello, World!`。
<!--more-->
# 目录
  > - [准备工作](#准备工作)
  >   - [Vim](#Vim)
  >   - [Sublime Text](#Sublime-Text)
  >   - [Edit Plus](#Edit-Plus)
  > - [Hello, World!的历史](#Hello-World-的历史)
  > - [开始编写程序](#开始编写程序)
  > - [编译并运行程序](#编译并运行程序)
  > - [成功](#成功)

# 准备工作
> * 安装并且配置好环境变量的JDK
> * 任意一款无格式纯文本编辑软件

JDK的安装和配置，可以参考我在2016年10月30日写的博客《Java - 搭建开发环境》。
无格式的纯文本编辑软件，我推荐这三款：`VIM`，`Sublime Text`，`EditPlus`。

## Vim
Vim([点击查看](http://baike.baidu.com/item/VIM/60410))，是从vi发展出来的一个文本编辑器。代码补全、编译及错误跳转等方便编程的功能特别丰富，在程序员中被广泛使用，和Emacs并列成为类Unix系统用户最喜欢的文本编辑器。特点是根本用不到鼠标，用上去有种高大上的样子，而且可以提高开发效率，但相应的，学习成本增加，你需要明白这款编辑器的快捷键的使用方法，甚至需要学习正则表达式。如图是VIM软件。
![VIM编辑器][1]
## Sublime Text
Sublime Text([点击查看](http://baike.baidu.com/view/10701920.htm))，是一个代码编辑器（Sublime Text 2是收费软件，但可以无限期试用），也是HTML和散文先进的文本编辑器。Sublime Text是由程序员Jon Skinner于2008年1月份所开发出来，它最初被设计为一个具有丰富扩展功能的Vim。Sublime Text具有漂亮的用户界面和强大的功能，例如代码缩略图，Python的插件，代码段等。还可自定义键绑定，菜单和工具栏。Sublime Text 的主要功能包括：拼写检查，书签，完整的 Python API，Goto功能，即时项目切换，多选择，多窗口等等。Sublime Text是一个跨平台的编辑器，同时支持Windows、Linux、Mac OS X等操作系统。如图是Sublime Text 2：
![Sublime Text 2][2]
## Edit Plus
Edit Plus([点击查看](http://baike.baidu.com/view/206636.htm))，是一款由韩国 Sangil Kim （ES-Computing）出品的小巧但是功能强大的可处理文本、HTML和程序语言的Windows编辑器，你甚至可以通过设置用户工具将其作为C,Java,Php等等语言的一个简单的IDE。功能强大，可取代记事本，拥有无限制的撤消与重做、英文拼字检查、自动换行、列数标记、搜寻取代、同时编辑多文件、全屏幕浏览功能。而它还有一个好用的功能，就是它有监视剪贴板的功能，同步于剪贴板可自动粘贴进EditPlus的窗口中省去粘贴的步骤。另外它也是一个非常好用的HTML编辑器，它除了支持颜色标记、HTML标记，同时支持C、C++、Perl、Java，另外，它还内建完整的HTML & CSS1 指令功能，对于习惯用记事本编辑网页的朋友，它可帮你节省一半以上的网页制作时间，若你有安装IE3.0 以上版本，它还会结合IE浏览器于 EditPlus窗口中，让你可以直接预览编辑好的网页（若没安装IE，也可指定浏览器路径）。因此，它是一个相当棒又多用途多状态的编辑软件。如图是Edit Plus。
![Edit Plus][3]

# Hello, World!的历史
“Hello, world"程序是指在计算机屏幕上输出“Hello,world”这行字符串的计算机程序，“hello, world”的中文意思是“世界，你好”。这个例程在Brian Kernighan 和Dennis M. Ritchie合著的《The C Programme Language》使用而广泛流行。因为它的简洁，实用，并包含了一个该版本的C程序首次出现在1974年Brian Kernighan所撰写的《Programming in C: A Tutorial》
```C
printf("hello, world\n");
```
实际上将“Hello”和“World”一起使用的程序最早出现于1972年，出现在贝尔实验室成员Brian Kernighan撰写的内部技术文件《Introduction to the Language B》之中：
```B
main(){
    extern a,b,c;
    putchar(a);putchar(b);putchar(c);putchar('!*n');
}
a'hell';
b'o,w';
c'orld';
```
最初的"hello, world"打印内容有个标准，即全小写，有逗号，逗号后空一格，且无感叹号。不过沿用至今，完全遵循传统标准形式的反而很少出现。

# 开始编写程序
首先，打开你的编辑器，在其中输入如下代码：
```Java
public class helloworld{
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```
写完后保存为`helloworld.java`。
下面我们来分析这个程序。
第一行，`public class helloworld`代表一个公共的类，名字为helloworld，现在可以不用理解什么是“公共的类”，只需要记住即可。需要注意的是，Java源文件的名字**必须**与其中的一个公共类的名字相同（**区分大小写**）。然后是两个成对的花括号，代表类开始和结束，可以把这两个花括号理解为箱子，里面是程序主体。
第二行，`public static void main(String[] args)`是主函数的方法签名，整个程序由此开始执行，必须写成这样，具体意思此处无法解释，同样只需要记住即可。
第三行，`System.out.println("Hello, World!");`在屏幕上输出一行"Hello, World!"并换行，注意结尾的分号。Java语言的语法要求每一个语句后面以分号结束。

# 编译并运行程序
保存后，打开命令提示符（Windows系统使用`Windows键`+`R键`，输入cmd，回车），进入所在目录，使用`javac helloworld.java`编译程序，如果**没有任何提示**就是成功了，编译完成后使用`java helloworld`运行程序，注意没有`.java`。下面给出两种系统编译运行Java程序的命令。
> Windows系统
 ```bash
 cd Desktop\java
 javac helloworld.java
 java helloworld
 ```

---
> Linux系统
 ```bash
 javac helloworld.java
 java helloworld
 ```

# 成功
至此，你已经编写并且运行了自己的第一个Java程序，迈出了编程的第一步。如果在编写过程中遇到什么问题、错误，多使用搜索引擎，如果还是不明白可以在下面留言问我，或者给我发邮件。


[1]: https://foreveropp.github.io/pic/vim.png
[2]: https://foreveropp.github.io/pic/sublimetext.png
[3]: https://foreveropp.github.io/pic/editplus.png
