## Linux常用命令格式

---

### 一、命令基本格式

#### 命令提示符
    [root@loacalhost ~]#  
    其中：  
    root                当前登录用户
    loacalhost          主机名
    ~                   当前所在目录（家目录）
                        超级用户 /root          普通用户 /home/user1/
                        根目录 /
    #                   超级用户的提示符
                        普通用户的提示符是$

#### 命令格式
    命令 [选项]  [参数]
    注意：  个别命令使用不遵循此格式
        当有多个选项时，可以写在一起
        简化选项与完整选项
            -a 等于 --all

    ls [选项]  [文件或目录]
    选项：  -a  显示所有文件，包括隐藏文件
        -l  显示详细信息
        -d  查看目录属性
        -h  人性化显示文件大小
        -i  显示inode

    - 例：
    ls -l
    -rw-r--r--. 1 root root 1207 1月 14 18:18 abc.cfg
        文件类型（-：文件    d：目录    l：软链接文件）
        rw-      r--      r--
        u所有者  g所属组  o其他人
        r读 w写 x执行
        .  ACL权限
        1  引用计数
        前面的root是用户
        后面的root是用户组

    ls /    查看目录文件

### 二、文件处理命令

#### 目录处理命令
##### 1、建立目录：mkdir
    mkdir -p [目录]
    -p  递归创建目录，例如 mkdir -p abc/d

##### 2、切换目录：cd
    cd [目录]
    简化操作：
    cd ~    进入当前用户的家目录
    cd      进入当前用户的家目录
    cd -    进入上次目录
    cd ..   进入上一级目录
    cd .    进入当前目录
    相对路径：参照当前目录进行查找
    如 [root@coder ~]# cd ../usr/local/src/
    绝对路径：从根目录开始指定，一级一级递归查找
    如 [root@coder ~]# cd /etc/

##### 3、删除目录：rmdir
    rmdir [目录名]
    删除空目录

##### 4、删除文件或目录：rm
    rm -rf [文件或目录]
    选项：
        -r      删除目录
        -f      强制

##### 5、复制命令：cp
    cp [选项]  [原文件或目录]  [目标目录]
    选项：
        -r      复制目录
        -p      连带文件属性复制，例如时间参数
        -d      若源文件是链接文件，则复制链接属性
        -a      相当于 -pdr 上面三个都包括
    例如：若复制一个目录到另外一个目录，需要加 -r

##### 6、剪切或改名命令：mv
    mv [原文件或目录]  [目标目录名]
    原文件和目标文件在同一目录下，则为改名
    剪切例如： mv aaa/ /temp/bbb
    注意：这里如果是目录不需要加-r

#### 常用目录的作用
    - /           根目录
    - ./          当前目录
    - /bin        命令保存目录（普通用户就可以读取的命令）
                    在根目录和usr/下都有/bin和/sbin
                    其中/bin普通用户可以执行，/sbin是超级用户才能执行
    - /boot       启动目录，启动相关文件 
    - /dev        设备文件保存目录
    - /etc        配置文件保存目录
    - /home       普通用户的家目录
    - /lib        系统库保存目录
    - /mnt        系统挂载目录
    - /media      挂载目录
    - /temp       临时目录，临时数据
    - /sbin       命令保存目录（超级用户）
    - /proc       直接写入内存的，不能直接操作
    - /sys        同上
    - /usr        系统软件资源目录
                    /usr/bin
                    /usr/sbin
    - /var        系统相关文档内容

    - 白色：表示普通文件
    - 蓝色：表示目录
    - 绿色：表示可执行文件
    - 红色：表示压缩文件
    - 浅蓝色：链接文件
    - 红色闪烁：表示链接的文件有问题
    - 黄色：表示设备文件
    - 灰色：表示其它文件

#### 技巧:
    - ctrl + l    清屏
    - tap         自动补充文件名，命令，按两下可以显示所有路径或命令
    - ll和ls -al完全一样，ll会显示隐藏文件信息而ls -l不行！！
    - 可在家目录和temp目录下随意加入文件

#### 文件处理命令
##### 7、链接命令：ln
    ln -s [原文件]  [目标文件]
    原文：link
    功能：生成链接文件
    选项：
        -s  创建软链接

    软链接特征：
    1、类似于windows的快捷方式
    2、软链接拥有自己的i节点和block快，但数据块中只保存原文件的文件名和i节点号，并没有实际的文件数据
    3、lrwxrwxrwx l软链接的权限都是rwxrwxrwx
    4、修改任意文件，另一个都改变
    5、删除原文件，软链接不能使用

    硬连接特征：
    1、拥有相同的i结点和存储block快，可以看作是同一个文件
    2、可通过i节点识别
    3、不能跨分区
    4、不能针对目录使用

### 三、文件搜索命令

#### 1、文件搜索命令：locate
    locate [文件名]
    在后台数据库中按文件名搜索，搜索速度更快

    - /var/lib/mlocate
        #locate命令所搜索的后台数据库，每天更新一次
    - updatedb
        手动更新数据库
    - locate locate
        找到与locate相关的文件
    

#### 2、命令搜索命令：whereis和which
    whereis 命令名
    搜索命令所在路径及帮助文档所在位置
    选项：
        -b：     只查找可执行文件
        -m：     只查找帮助文件

    which 命令名
    可以查看别名和路径，但不是所有命令都有别名

    - whoami命令，whatis ls命令
    - PATH环境变量
    定义的是系统搜索命令的路径
    若以后自己写一个命令，为了方便不写命令所在的全路径，可以在PATH下定义
    [root@localhost ~]# echo $PATH
    /usr/lib/qt-3.3/bin:
    /usr/local/sbin:/usr/local/bin:/sbin/bin:/usr/sbin:/usr/bin:root/bin

##### 配置文件（针对上述两类命令）：/etc/updatadb.conf
    1、PRUNE_BIND_MOUNTS = "yes"
    开启搜索限制
    2、PRUNEFS = 
    搜索时，不搜索的文件系统
    3、PRUNENAMES = 
    搜索时，不搜索的文件类型
    4、PRUNEPATHS = 
    搜索时，不搜索的路径

#### 3、文件搜索命令：find
    find [搜索范围]  [搜索条件]
    find / -name install.log
    搜索文件，要避免大范围搜索，会非常耗费系统资源
##### 1.模糊匹配
    find是在系统当中搜索符合条件的文件名，如需要匹配，使用通配符匹配。
##### 2.不区分大小写
    find /root -iname aaa.log
##### 3.按照所有者搜索，搜索user所有的文件
    find /root -user root
##### 4.查找没有所有者的文件
    find /root -nouser
    /proc和/sys目录下的内核文件，拷贝的外来文件没有所有者，
    除了这两种情况，都是垃圾文件
##### 5.根据时间查找
    find /var/log/ -mtime +10
    -10     10天内修改的文件
    10      10天当天修改的文件
    +10     10天前修改的文件
    
    atime   文件访问时间
    ctime   改变文件属性
    mtime   修改文件内容
##### 6.根据文件大小查找
    find . -size 25k
    查找文件大小是25k的文件
    -25k    小于25KB的文件
    25k     等于25KB的文件
    +25k    大于25KB的文件
    注意，k是小写，M是大写
##### 7.根据节点查找
    find . -inum 262422
    查找节点是262422的文件    
##### 8.逻辑判断
    find /etc -size +20k -a -size -50k
    -a  and  与
    -o  or   或
##### 9.增加信息的处理命令
    find /etc -size +20k -exec ls -lh {} \;
    -exec 命令 {} \;
    其中这里命令是指：能够去处理查询到的信息的命令

    - Linux中的通配符
    "*"任意内容  "?"任意一个字符  "[]"任意一个中括号内的字符
    find /root -name "abc.log"*

#### 4、字符串搜索命令：grep
    grep [选项] 字符串 文件名
    选项：
        -i:  忽略大小写    
        -v:  排除指定字符串，即取反
    grep "abc" aaa.cfg

    - 可以使用正则表达式匹配

### 四、帮助命令

#### 1、帮助命令man
    意思：manual的缩写

##### 1.获取指定命令的帮助
    man 命令
    q键退出

##### 2.查看命令拥有哪个级别的帮助
    man -f 命令
    相当于 whatis 命令
    例如：(有时数字前没有-)
        man -5 passwd
        man -4 null
        man -8 ifconfig

##### 3.查看和命令相关（包括模糊匹配）的所有帮助
    man -k 命令
    相当于 apropos 命令
    例如：
        apropos passwd

##### man的级别
    1       查看命令的帮助
    2       查看可被内核调用的函数的帮助
    3       查看函数和函数库的帮助
    4       查看特殊文件的帮助（主要是/dev目录下的文件）
    5       查看配置文件的帮助
    6       查看游戏的帮助
    7       查看其他杂项的帮助
    8       查看系统管理员可用命令的帮助
    9       查看和内核相关文件的帮助

    man支持查看多个命令，例如：
    man ls 1 8

#### 2、其他帮助命令

##### 1.选项帮助
    命令 --help
    获取命令选项的帮助

    例如：
        ls --help

##### 2.shell内部命令
    help shell内部命令
    获取shell内部命令（系统自带的）的帮助

    例如：
        whereis cd
        确定是否是shell内部命令，根据的是路径是否有bin执行文件路径
        help cd
        获取内部命令帮助

##### 3.详细命令帮助
    info 命令
    将所有信息显示出来
    - 回车：   进入子帮助页面（带有*号标记）
    - u：      进入上层页面
    - n:       进入下一个帮助小节
    - p:       进入上一个帮助小节
    - q:       退出

### 五、压缩与解压命令

    常见压缩格式： .zip  .gz  .bz2  .tar.gz  .tar.bz2

#### 1、.zip压缩格式
    与windows是公用的

##### 1.压缩文件
    zip 压缩文件名 源文件

##### 2.压缩目录
    zip -r 压缩文件名 源文件

##### 3.解压缩
    unzip 压缩文件

#### 2、.gz压缩格式
    可以在windows下解压缩

##### 1.压缩为.gz格式文件，源文件会消失
    gzip 源文件

##### 2.压缩为.gz格式文件，源文件保留
    gzip -c 源文件 > 压缩文件
    这里 > 符号，是把输出的结果储存在目标文件中
    例如：
        gzip -c abc > abc.gz
        ls > abc

##### 3.压缩目录下所有的子文件，但不能压缩目录
    gzip -r 目录

##### 4.解压缩
    gzip -d 压缩文件
    或 gunzip 压缩文件
    对于目录： gzip -r 压缩文件

#### 3、.bz2压缩格式
    bzip2命令不能压缩目录

##### 1.压缩为.bz2格式文件，源文件会消失
    bzip2 源文件

##### 2.压缩为.bz2格式文件，源文件保留
    bzip2 -k 源文件

##### 3.解压缩
    bzip2 -d 压缩文件
    或 bunzip2 压缩文件
    注意： -k保留压缩文件

#### 4、.tar.gz和.tar.bz2压缩格式
    tar命令可以将目录进行打包，而.tar.gz或.bz2可以实现先打包为.tar格式，再压缩为.gz或.bz2格式，其中tar命令如下：
##### 1.打包命令tar
    tar -cvf 打包文件名 源文件
    选项：
        -c:     打包
        -v:     显示过程
        -f:     指定打包后的文件名
    例如：
        tar -cvf aaa.tar aaa

##### 2.解压缩tar
    tar -xvf 打包文件名
    选项：
        -x:     解打包

##### 3.压缩
    tar -zcvf 压缩包名.tar.gz 源文件
    tar -jcvf 压缩包名.tar.bz2 源文件

##### 4.解压缩
    tar -zxvf 压缩包名.tar.gz
    tar -jxvf 压缩包名.tar.bz2

    指定解压缩位置：
    tar -jxvf 压缩包名 -C /temp/

    压缩多个文件至temp文件夹
    tar -zcvf /temp/test.tar.gz a1 a2

##### 5.只查看，不解压
    tar -ztvf test.tar.gz

### 六、关机和重启命令

#### 1、shutdown命令（较安全）
    [root@localhost ~]# shutdown [选项] 时间
    选项：
        -c:     取消前一个关机命令
        -h:     关机
        -r：    重启
    时间：now

#### 2、其他关机命令(不安全)
    [root@localhost ~]# halt
    [root@localhost ~]# powerooff
    [root@localhost ~]# init 0

#### 3、其他重启命令
    [root@localhost ~]# reboot
    [root@localhost ~]# init 6

#### 系统运行级别：
    0   关机
    1   单用户（类似于安全模式）
    2   不完全多用户，不含NFS服务
    3   完全多用户（普通界面）
    4   未分配
    5   图形界面
    6   重启
    [root@localhost ~]# cat/etc/inittab 
    修改系统默认运行级别
    id:3:initdefault
    
    [root@localhost ~]# runlevel
    查询系统运行级别
    N 3

#### 4、退出登录命令
    [root@localhost ~]# logout

    - & 将命令放在后台执行

### 七、其他命令

#### 1、Linux中挂载命令
    用户登录查看和用户交互命令，可以理解为windows中分配盘符
    一般为开机自动挂载，而光盘、U盘等需要人为挂载

##### 1.查询与自动挂载
    [root@localhost ~]# mount
    查询系统中已经挂载的设备
    例：
    /dev/sda5/ on / type ext4 (rw)
    硬件设备的命令，第一块sd硬盘的第一个逻辑分区是根分区，文件系统是ext4，权限是读写

    [root@localhost ~]# mount -a
    根据配置文件/etc/fstab的内容，(开机)自动挂载
    尽量不要把自己的光盘U盘等也写入该配置文件，因为若忘记插入光盘，系统会崩溃

##### 2.挂载命令格式
    [root@localhost ~]# mount [-t 文件系统] [-o 特殊选项] 设备文件名 挂载点
    选项：
        -t 文件系统：加入文件系统类型来指定挂载的类型，可以ext3、ext4(centOS6以上)、iso9660(光盘标准)等文件系统
        -o 特殊选项：可以指定挂载的额外选项
    例：
        mount -o remount,noexec /home/ 重新挂载分区，可执行文件无权限

##### 3.挂载光盘
    [root@localhost ~]# mkdir /mnt/cdrom
    建立挂载点

    [root@localhost ~]# mount -t iso9660 /dev/cdrom /mnt/cdrom/
    相当于
    [root@localhost ~]# mount -t iso9660 /dev/sr0 /mnt/cdrom/
    挂载光盘

##### 4.卸载光盘
    [root@localhost ~]# umount 设备文件名或挂载点

##### 5.挂载U盘
    [root@localhost ~]# fdisk -l
    查看U盘设备文件名

    [root@localhost ~]# mount -t vfat /dev/sdb1 /mnt/usd/
    vfat:   U盘文件系统默认是fat32位系统
    sdb1:   这里是上面看到的名字

    注意：Linux默认是不支持NTFS文件系统的

#### 2、用户登录查看命令
##### 1.查看登录用户信息
    w 用户名
    更为详细
    who
    更为简单

##### 2.查看当前登录和过去登录的用户信息
    last
    查询的是/var/log/wtmp二进制文件

##### 3.查看所有用户的最后一次登录时间
    lastlog
