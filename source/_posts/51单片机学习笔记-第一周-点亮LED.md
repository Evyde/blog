---
title: 51单片机学习笔记-第一周-点亮LED
date: 2020-09-14 23:23:23
tags:
- 51
- Study
- Code
categories:
- Code
---
以前我也进行过51的学习，但是不太系统，这次跟着物理学院的一个嵌入式开发小组学习一下，应该会比较有意思。
<!--more-->  

# 环境搭建  
这个没什么好说的，跟着教程一步一步走即可，先装驱动，再装Keil，需要注意一下，Keil最新版本是5，到官网下载C51的最新版本即可。  
Keil在高分屏下的设置，需要右键`属性`->`兼容性`->`更改高DPI设置`->`高DPi缩放替代`即可。  
关于编辑器主题，这里附赠一个类似VS Code的暗色主题：  
  
``` Config [配置文件]  
    # properties for all file types
    indent.automatic=1
    virtual.space=1
    view.whitespace=0
    view.endofline=0
    code.page=936
    caretline.visible=1
    highlight.matchingbraces=1
    print.syntax.coloring=1
    use.tab.color=1
    create.backup.files=0
    auto.load.ext.modfiles=0
    save.prj.before.dbg=0
    save.files.before.dbg=0
    function.scanner.project=0
    function.scanner.files=0
    function.scanner.modules=1

    # properties for c/cpp files
    syntax.colouring.cpp=1
    use.tab.cpp=1
    tabsize.cpp=2
    line.margin.visible.cpp=1
    fold.cpp=1
    monospaced.font.cpp=1

    # properties for asm files
    syntax.colouring.asm=1
    use.tab.asm=0
    tabsize.asm=4
    line.margin.visible.asm=1
    monospaced.font.asm=1

    # properties for other files
    use.tabs=0
    tabsize=4
    line.margin.visible.txt=0
    monospaced.font.txt=1

    # setting for code completion and syntax check
    cc.autolist=1
    cc.highlightsyntax=1
    cc.showparameters=1
    cc.triggerlist=1
    cc.triggernumchars=3
    cc.enter.as.fillup=0
    cc.usealpha4inactcode=1
    cc.alphavalue=50

    # autosave for editor files
    autosave=0
    autosave.interval=5

    # vertical edge at right margin
    edge.mode=0
    edge.column=180


    # Specification for text selection and caret line
    selection.fore=#000000
    selection.back=#005EB3
    caret.fore=#FFFFFF
    caret.back=#000000

    # Color for vertical edge
    edge.colour=#66FAFA

    # C/C++ Editor files
    template.cpp="#define","#define |";"#if","#if |\r\n\r\n#endif";\\
        "#include","#include ";"Header","// Header:\r\n// File Name: |\r\n// Author:\r\n// Date:\r\n";\\
        "continue","continue;";"do","do\r\n{\r\n\t// TODO: enter the block content here\r\n\t\r\n\t|\r\n} while ();\r\n";\\
        "enum","enum |\r\n{\r\n\t\r\n};\r\n";"for","for(|;;)\r\n{\r\n}";\\
        "fpointer_type","typedef int (* |F)();\r\n";"function","void function(|)\r\n{\r\n\r\n}\r\n";\\
        "if","if (|)";"ifelse","if (|)\r\n{\r\n}\r\nelse\r\n{\r\n}";\\
        "struct","struct | \r\n{\r\n\r\n};\r\n";"switch","switch (|)\r\n{\r\n\tcase:\r\n\t\tbreak;\r\n\tcase:\r\n\t\tbreak;\r\n\tdefault:\r\n\t\tbreak;\r\n}";\\
        "void","void | ();\r\n";"while","while (|)\r\n{\r\n}";\\

    font.monospace.cpp=Fixedsys
    style.cpp.32=font:Fixedsys,size:16,fore:#9CDCFE,back:#1E1E1E
    style.cpp.4=font:Fixedsys,size:16,fore:#4EC9B0,back:#1E1E1E
    style.cpp.10=font:Fixedsys,size:16,fore:#DCDCDC,back:#1E1E1E
    style.cpp.1=font:Fixedsys,size:16,fore:#57A64A,back:#1E1E1E
    style.cpp.2=font:Fixedsys,size:16,fore:#007F00,back:#1E1E1E
    style.cpp.5=font:Fixedsys,size:16,fore:#007ACC,back:#1E1E1E
    style.cpp.6=font:Fixedsys,size:16,fore:#FF80FF,back:#1E1E1E
    style.cpp.11=font:Fixedsys,size:16,fore:#DCDCDC,back:#1E1E1E
    style.cpp.9=font:Fixedsys,size:16,fore:#4EC9B0,back:#1E1E1E
    style.cpp.7=font:Fixedsys,size:16,fore:#FF80FF,back:#1E1E1E
    style.cpp.34=font:Fixedsys,size:16,fore:#500000,back:#007ACC
    style.cpp.35=font:Fixedsys,size:16,fore:#FF0000,back:#1E1E1E
    style.cpp.16=font:Fixedsys,size:16,fore:#9CDCFE,back:#1E1E1E
    style.cpp.12=font:Fixedsys,size:16,fore:#FF80FF,back:#1E1E1E
    style.cpp.86=font:Fixedsys,size:16,fore:#696969,back:#FFFFFF


    # Asm Editor files
    font.monospace.asm=Courier New
    style.asm.32=font:Courier New,size:16,fore:#9CDCFE,back:#1E1E1E
    style.asm.1=font:Courier New,size:16,fore:#4EC9B0,back:#1E1E1E
    style.asm.2=font:Courier New,size:16,fore:#DCDCDC,back:#1E1E1E
    style.asm.3=font:Courier New,size:16,fore:#57A64A,back:#1E1E1E
    style.asm.4=font:Courier New,size:16,fore:#007F00,back:#1E1E1E
    style.asm.5=font:Courier New,size:16,fore:#007ACC,back:#1E1E1E
    style.asm.6=font:Courier New,size:16,fore:#FF80FF,back:#1E1E1E
    style.asm.7=font:Courier New,size:16,fore:#FF80FF,back:#1E1E1E
    style.asm.9=font:Courier New,size:16,fore:#FF80FF,back:#1E1E1E
    style.asm.10=font:Courier New,size:16,fore:#500000,back:#007ACC
    style.asm.11=font:Courier New,size:16,fore:#FF0000,back:#1E1E1E
    style.asm.12=font:Courier New,size:16,fore:#4EC9B0,back:#1E1E1E
    style.asm.8=font:Courier New,size:16,fore:#FF80FF,back:#1E1E1E


    # Editor Text files
    font.monospace.txt=Courier New
    style.txt.32=font:Verdana,size:16,fore:#9CDCFE,back:#1E1E1E   
```  
  
    
关于Keil，还有一些需要注意的，新建项目之后，需要右键点击左侧`Project`下面的`Source Group`来新建文件，否则无法自动加入编译目标。另外，使用`ALT`+`F7`来呼出构建菜单，在`Output`里面把`Create HEX File`打钩，就可以在`Objects`文件夹下生成可以烧录的文件。
Keil安装完成后，需要在STC官方提供的`STC-ISP`软件中选择`Keil仿真设置`中的`添加型号和头文件到Keil中`，选择Keil的安装目录（内必须有`C51`文件夹）即可。  
然后就是写代码了，这里是第一天所以先写一个简单的进度条LED程序测试环境是否搭建成功。  
  
# 写代码  
代码如下：   
   
``` C [进度条]  
  #include <reg52.h>

  void delay(void) {
      unsigned int x;
      x = 50000;
      while(x--);
  }

  void main(void) {
      unsigned int endFlag, upFlag;
      upFlag = 0;
      endFlag = 0;
      P1 = 0x7f;
      while(1) {
        if(endFlag) {
          P1 = 0x7f;
          endFlag = 0;
        }
        if(!P1&&upFlag==5) endFlag = 1;
        if(upFlag==5) {
          P1 = P1 / 2;
          upFlag = 0;
        }
        P1 = P1 * 2 + 1;
        delay();
        P1 = P1 / 2;
        delay();
        upFlag++;
      }

  }  
 ```  
  
    
# 程序解析  
我这个开发板，置0是开（估计是LED正极接VCC，然后负极接P1），置1是关，所以我只需要让每一位为0即可，闪烁也是如此。  
需要注意的就是这个`delay`，由于现在无法操作时钟模块，只能使用循环计时的方式进行延时。还有`P1*=2`及`P1/=2`相当于左移和右移。  
P1传一个十六进制数我觉得从汇编理解就是相当于`mov ax,0x7F`，也就是把P1看成一个寄存器，把这个数据输入进去。至于P1^0则是选择P1寄存器的第一个数据位。
  
# 编译链接   
和VC++ 6.0一样，点那个左上角两个箭头的按钮就可以直接生成二进制文件（准确说是十六进制），默认在`./Objects`目录下，可以看到一个`.hex`文件。
  
# 烧录  
打开STC-ISP软件，打开程序文件，可以看到
``` BIN [进度条]
02 00 62 E4 FB FA F5 82 F5 83 75 90 7F E5 82 45 
83 60 08 75 90 7F E4 F5 83 F5 82 E5 90 70 0B EB 
64 05 4A 70 05 F5 83 75 82 01 EB 64 05 4A 70 09 
E5 90 C3 13 F5 90 E4 FA FB E5 90 25 E0 04 F5 90 
12 00 53 E5 90 C3 13 F5 90 12 00 53 0B BB 00 01 
0A 80 BA 7F 50 7E C3 EF 1F AC 06 70 01 1E 4C 70 
F6 22 78 7F E4 F6 D8 FD 75 81 07 02 00 03 
```  
这就是我们的程序了。然后插入单片机，选择`检测MCU选项`，程序会自动检测单片机型号（比那时候方便多了），然后`下载/编程`即可。  
Linux下有一个[STC Flash的Python脚本](https://github.com/laborer/stcflash)可以用。效果不错。
