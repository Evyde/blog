---
title: LaTeX学习笔记
date: 2020-01-17 21:52:52
tags:
- LaTeX
- Study
categories:
- Study
---
由于需要参加美赛，所以我来学习LaTeX了，这个LaTeX我早有耳闻，但是只知道它表示数学公式比较不错，接触之后发现它简直就是科研工作者的救星，因为只需要学习简单的几个指令就可以进行标准化的排版，下面这篇博客是对我学习过程及一些LaTeX语法的记录。

<!--more-->
- 准备工作
  首先需要下载一款可以支持LaTeX的编辑器，由于时间不是很充足，我没有找到原生支持LaTeX的编辑器。我一开始看到LaTeX，以为它应该像MarkDown那样被广泛支持，但是实际上它的支持情况并不理想，甚至2020年的今天，要想显示中文还是比较困难的一件事。

  LaTeX是需要编译的，编译结果为PDF。我在网上看到的大部分方法都需要下载LaTeX官方的编译器，大概为4个G，我不知道为什么这么大，但是我看到下载完这4个G仍然需要进行复杂的设置才能获得接近MarkDown的书写体验，我就放弃了本地客户端转向在线客户端。

  当然，在线客户端的问题很多，最直接的就是无法安装自己想要的包。不过，我们现在就是简单学习一下，并不需要多么复杂的包和宏集，所以，我找到了一款在线的LaTeX编辑器，免费且支持实时预览，请移步[ShareLaTeX, 在线LaTeX编辑器](https://cn.sharelatex.com)。

- 基本语法
  由于我仅仅是看了两天，所以这篇文章会持续更新，现在先记录一下我所知道的。
  1. 文档标识
    LaTeX文件使用`.tex`扩展名，且需要在文件头尾写入以下格式化文本：
    ```LaTeX
    \documentclass[xxx]{xxx}
    % 这里是导言区
    \begin{document}
    % 这里是正文
    \end{document}
    ```
    暂时不清楚第一行`\documentclass`中的[U]和{article}有什么具体含义和可以修改的范围，按下不表。
    如你所见，这个LaTeX文档分为导言区和正文，这似乎与HTML类似，导言区即为\<head\>标签包裹的内容区域，正文即\<body\>。然后一个`%`为注释。
          注意：一个LaTeX标签（或者命令，姑且这样称呼）是以空格或者换行结尾的，有些标签是默认向后一个字符结尾
  2. 导言区常用标签
    - `\title{Title}`
      这表示该文档的标题，通常显示在文档的正上方中央。
    - `\author{Evyde}`
      这表示文档的作者，会显示在标题下方。
    - `\date{\today}`
      标签嵌套？(笑)，代表当前日期，显示在作者下方。

    这是我目前知道的导言区常用的标签。

  3. 正文常用标签
    - `\maketitle`
    很重要，没有它导言区的那些标题作者什么的就无法显示。
    - `\tableofcontents`
    这个是我觉得LaTeX优于MarkDown的一个地方（也可能是我孤陋寡闻），就是使用这个标签可以自动根据你的文章生成一个目录放在文章前。
    - `\begin{}`
      这个其实不能算标签，这个我会另讲。现在说一下与这个类似且常用的
      - `\begin{abstract}`
        这是创建一个摘要，效果就是一个居中的Abstract下面有你写的文字，当然，所有的`\begin{argu}`标签必须以`\end{argu}`结束。

    - `\label{eqx}`
      x可用数值替换，作用是标注一个等式，效果如下
$$
E = mc^2
$$
    在公式@eq:test中...
$$
A = \begin{bmatrix}
        a_{11}    & a_{12}    & ...    & a_{1n}\\
        a_{21}    & a_{22}    & ...    & a_{2n}\\
        a_{31}    & a_{22}    & ...    & a_{3n}\\
        \vdots    & \vdots    & \ddots & \vdots\\
        a_{n1}    & a_{n2}    & ... & a_{nn}\\
    \end{bmatrix} , b = \begin{bmatrix}
        b_{1}  \\
        b_{2}  \\
        b_{3}  \\
        \vdots \\
        b_{n}  \\
    \end{bmatrix}
$$

- 关于Hexo中启用LaTeX支持

~~
  博客嘛，LaTeX支持是很有必要的，不然怎么写公式？查阅了网上很多的资料发现，一个最简单且完美的方式是更换Hexo的MarkDown渲染器，更换成Pandoc即可。当然，首先需要安装Pandoc，再安装渲染器。但是，我一开始按照网上简单的两行命令操作并没有用。
  ```shell
  npm install hexo-renderer-pandoc --save
  npm uninstall hexo-renderer-marked --save
  ```
  找了许多资料也没有解决问题，后来尝试了这么几个命令之后就成功了：
  ```shell
  npm install hexo-math --save
  npm install hexo-renderer-mathjax
  npm install hexo-katex
  ```
  同时，建议在Hexo文件夹中的`_config.yml`文件中加入：
  ```yml
  pandoc:
    mathEngine: katex
  ```
  当然，也可以使用默认的mathjax。
~~
  请使用[hexo-renderer-markdown-it-plus](https://github.com/CHENXCHEN/hexo-renderer-markdown-it-plus)。
