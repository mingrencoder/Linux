# VIM文本编辑器

### 一、VIM编辑器概述
    VI -> VIM
    提升点：
        1、支持多级撤销
        2、跨平台运行
        3、语法高亮
        4、支持图形界面

### 二、VIM编辑器的操作模式
    命令模式
    输入模式
    底行模式（尾行、末行）

### 三、VIM编辑器的命令模式
    vim abc
    vim + abc       打开文件后，光标定位到最后一行
    vim +3 abc      打开文件后，光标定位到第三行
    vim +/xxx abc   打开文件后，光标定位到xxx第一次出现的那行，用n来切换每个包括xxx的行
    vim aa bb cc    一次性打开(创建)多个文件，用底行模式 :n  :N (:prev)切换

### 四、底行模式和命令模式常用指令

##### 1、底行模式
    :w      保存修改
    :q      退出
    :q!     强制执行
    :ls     列出所有打开的文件
    :n      切换打开文件
    :15     定位行号
    /xxx    从光标位置开始，向后搜索xxx
    ?xxx    从光标位置开始，向前搜索xxx

##### 2、命令模式
    h j k l             光标左下右上
    ctrl + f/b/d/u      向下/上翻页  向下/上翻半页
    dd      删除光标所在行
    o       在光标所在行下发插入一行，并进入输入模式
    yy      复制光标所在行
    p/P     在光标所在行下方/上方粘贴


# 磁盘管理

# 用户管理

### 一、用户和用户组概念
##### 1、etc/group
    etc/group 储存当前系统所有用户组信息
    Group :      x     :  123  :  abc,def,xyz
    组名称 : 组密码占位符 : 组编号 : 组中用户名列表

##### 2、etc/gshadow
    etc/gshadow 存储当前系统中用户组的密码信息
    Group :   *   :         :  abc,def,xyz
    组名称 : 组密码 : 组管理者 : 组中用户名列表
    *代表没有组面，组管理者为空代表组内用户都可以管理该组

##### 3、etc/passwd
    etc/passwd 存储当前系统中所有用户的信息
    user :   x    :  123  :   456  :    xxx   :/home/user : /bin/bash
    用户名:密码占位符:用户编号:用户组编号:用户注释信息: 用户主目录  : shell类型

##### 3、etc/shadow
    etc/shadow 存储当前系统中所有用户的密码信息

### 二、用户和用户组基本命令

##### 1、用户组
    groupadd name                   添加组
    groupmod -n newname oldname     修改组名称
    groupmod -g 666 name            修改组编号
    groupadd -g 888 aaa             添加指定组编号的组
    groupdel name                   删除组(删除组之前必须删除组内用户)
    gpasswd grpname                 修改组密码

##### 2、用户
    groupadd list
    useradd -g list a           添加用户a到用户组list(主要用户组)
    useradd -g list b
    useradd -d /home/xxx c      修改用户的用户目录地址
    usermod -c remark a         为a添加注释
    usermod -l new old          使用new用户代替old用户
    userdel name                删除用户
    userdel -r name             删除用户并删除其用户目录文件夹下文件

    touch etc/nologin   建立文件，可以禁止除了root用户以外的用户登录

### 三、用户和用户组进阶命令
    passwd -l name      锁定账户
    passwd -u name      解锁账户
    passwd -d name      无密码登录

    用户可以属于多个组 -- 一个主要组和多个附属组
    gpasswd -a username grpname1, grpname2       添加附属组
    gpasswd -d username grpname1                 移除附属组
    若用户直接创建文件则默认属于主要组
    newgrp boss x       用户自己登录后使用该命令切换，x为组密码

    useradd -g grp -G grp1,grp2

### 四、用户和用户组其他命令
    su      切换用户
    whoami
    id username
    groups username     显示用户所在所有组
    chfn username       设置用户详细资料
    finger username     显示用户详细资料





