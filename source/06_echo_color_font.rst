.. _06_echo_color_font:

========================
echo输出带颜色的字体
========================

概述
--------------

在linux命令行终端或Bash Shell脚本中适当使用颜色，能够让第一时间感觉到您的命令或脚本执行过程中差异。本文主要介绍使用echo输出带颜色的字体。

- echo不带参数输出字符串

当echo命令不带参数时，可以直接输出字符串，如下所示::

    [meizhaohui@hopewait ~]# echo "abc"
    abc
    [meizhaohui@hopewait ~]# echo def
    def

- echo带参数输出字符串

当echo命令带-e参数时，可以使用反斜杠 \\ 输出特殊的字符。echo输出带颜色的字体格式如下::

    echo -e "\033[字背景颜色;字体颜色m字符串\033[0m"
    
其中: "\\033" 引导非常规字符序列。”m”意味着设置属性然后结束非常规字符序列。

例如::

    echo -e "\033[41;32m红色背景绿色文字\033[0m"
    
其中: 41的位置代表底色, 32的位置是代表字的颜色。

下面是代码输出绿色文字::

    echo -e "\033[32m绿色文字\033[0m"


使用复杂的颜色输出
-------------------

例1::

    echo -e "\033[31m红字\033[32m绿字\033[0m" # 输出红字和绿字
    

.. image:: ./_static/images/red_green_font.png

例2::

    echo -e "\033[31m红字\033[43;32m绿字带黄色背景\033[0m" # 输出红字和带黄色背景的绿字
    
.. image:: ./_static/images/red_green_font_yellow_background.png

例3::

    echo -e "\033[4;47;31m带下划线的白色背景的红字\033[0m\033[1;41;32m高亮的红色背景的绿字\033[0m"
    
.. image:: ./_static/images/underline_highlight1.png

例4::

    echo -e "\033[4m\033[47m\033[31m带下划线的白色背景的红字\033[0m \033[1m\033[41m\033[32m高亮的红色背景的绿字\033[0m"
    
.. image:: ./_static/images/underline_highlight2.png

例5::

    echo -e "\033[4m\033[47m\033[31m带下划线的白色背景的红字\033[0m \033[1;4m\033[41m\033[32m高亮的红色背景的带下划线的绿字\033[0m"
    
.. image:: ./_static/images/underline_highlight3.png

通过以上示例可知：控制符可以进行组合在一起，如例3中的 \\033[4;47;31m将三个属性组合在一起(属性数字中间使用分号;隔开)；也可以像例4中 \\033[4m\\033[47m\\033[31m每个属性单独写。

1. 字背景颜色范围:40–47

- 40:黑
- 41:红
- 42:绿
- 43:黄色
- 44:蓝色
- 45:紫色
- 46:天蓝
- 47:白色

2. 字颜色:30–37

- 30:黑
- 31:红
- 32:绿
- 33:黄
- 34:蓝色
- 35:紫色
- 36:天蓝
- 37:白色

3. ANSI控制码的说明：

- \\33[0m 关闭所有属性
- \\33[1m 设置高亮度
- \\33[4m 下划线
- \\33[5m 闪烁
- \\33[7m 反显

如果经常使用颜色控制的话，可以将颜色控制符进行定义好。
可以在~/.bashrc中设置个人偏好，使用vi ~/.bashrc打开.bashrc文件：并将下面的变量写入到文件中：

将下面的变量写入到~/.bashrc文件中::

    [meizhaohui@hopewait ~]# vi ~/.bashrc
    bg_black=”\033[40m”
    bg_red=”\033[41m”
    bg_green=”\033[42m”
    bg_yellow=”\033[43m”
    bg_blue=”\033[44m”
    bg_purple=”\033[45m”
    bg_cyan=”\033[46m”
    bg_white=”\033[47m”
    
    fg_black=”\033[30m”
    fg_red=”\033[31m”
    fg_green=”\033[32m”
    fg_yellow=”\033[33m”
    fg_blue=”\033[34m”
    fg_purple=”\033[35m”
    fg_cyan=”\033[36m”
    fg_white=”\033[37m”
    
    set_clear=”\033[0m”
    set_bold=”\033[1m”
    set_underline=”\033[4m”
    set_flash=”\033[5m”
    

输入完成后，先按Esc键，再按:键，并输入wq保存退出。

使用以下命令使刚才的修改生效::

    [meizhaohui@hopewait ~]# source ~/.bashrc
    
    
此时按如下命令输入相应的字体::

    [meizhaohui@hopewait ~]# echo -e "${bg_red}${fg_green}${set_bold}红色背景粗体的绿色字${set_clear}"
    红色背景粗体的绿色字
    红色背景粗体的绿色字  
    [meizhaohui@hopewait ~]# echo -e "${bg_red}${fg_green}红色背景的绿色字${set_clear}"
    红色背景的绿色字
    红色背景的绿色字

如果要在脚本中使用使用~/.bashrc中定义的bg_red、fg_green等变量，可以在shell脚本中使用source ~/.bashrc 或者点操作符加载~/.bashrc文件到脚本中。

打印颜色脚本::

    [meizhaohui@hopewait ~]# cat print_color.sh
    #!/bin/bash
    #Source personal definitions
    source ~/.bashrc
    # 或使用以下命令：
    # . ~/.bashrc
    echo -e "${bg_red}${fg_green}${set_bold}红色背景粗体的绿色字${set_clear}"

运行脚本::

    [meizhaohui@hopewait ~]# sh print_color.sh
    
.. image:: ./_static/images/red_back_bold_green.PNG

参考文献：

1、`控制输出颜色的shell脚本 <http://www.jb51.net/article/90436.htm>`_

