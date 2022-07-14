---
title: 学习大数据的前期准备工作（二）
date: 2019-03-06 19:47:13
categories: 大数据
tags: 
 - 大数据
 - Linux
---

## Linux 常用命令


> 命令通用格式 : **命令 选项（参数） 操作的文件或者目录**

### 普通命令

> **ls**    
> 格式：**ls 目录**  
> 参数: **a** : 列举包含隐藏文件  
>       **l** : 使用长格式查看  **ls -l** 等同于  **ll**

> **cd**  
> 改变目录  

> **pwd**  
> 打印当前所在目录  

> **clear**   
> 清屏 等同于 **ctrl** + **l**  

> **history**  
> 查看命令的历史记录  


### 文件操作命令

#### 创建

> 创建文件  
> touch a.txt

![](touch.png)

> 创建一个空目录  
> mkdir dir1  

![](mkdir1.png)

> 递归创建一个非空目录  
> mkdir -p dir2/dir3 

![](mkdir2.png)

#### 删除

> 删除文件  
> rm a.txt  
> -f 参数 命令是免确认

![](rm1.png)

> 删除目录（只能删空目录）  
> rm -rf mkdir2

![](rm2.png)  


#### 编辑

> vi / vim  

#### 复制

> 复制文件  
> cp a.txt dir1/  

![](cp.png)

> 复制目录  
> cp -r dir1 dir2/

![](cp2.png) 

#### 移动

> 将 a.txt 文件移动到 dir2 目录  
> mv a.txt dir2

![](mv1.png)

#### 查看

> more  
> 分页查看 显示一屏数据，按“enter”往下翻页

> less  
> 分页查看 显示一屏数据，按方向上下键翻页 按“q”退出预览

> head  
> 查看文件头部内容，默认读取文件前10行
> 常用参数：**-n**: 查看行数

> tail  
> 查看文件尾部内容，默认读取文件后10行  
> 常用参数:  
> -n:查看行数  
> -F:动态查看，监听一个文件的变化

### 用户管理

#### 管理员

> 用户 id 是 0

#### 普通用户

> 用户 id 从 500 开始,可以自己创建普通用户

#### 用户信息文件  
> more /etc/passwd

![](viewuser.png)

> 标红意思  
> 用户名:密码:uid:gid:所属组的名字:家目录:所用shell

#### 用户组信息文件

> more /etc/group

#### 切换用户

##### 切换到管理员

> su -

##### 切换到普通用户

> su -用户名

#### 退出

> exit

#### 创建用户

> 格式：useradd username  
> 例: useradd LW

#### 查看用户

> 格式：id username  
> 例: id LW

![](viewuserinfo.png)

#### 设置密码

> 格式：passwd username  
> 例: passwd LW

#### 删除用户

> 格式：userdel username  
> 参数：-r  既删除用户又删除该用户的家  
> 例: userdel LW



### 权限管理

#### 用户分类
> 所有者: **u**  
> 同组用户: **g**  
> 其他人: **o**

#### 权限分类
> 读: **r**  
> 写: **w**  
> 执行: **x**  
> 没有权限: **-**

#### 文件权限详情

![](per.png)

> 第一位：文件类型，**d**:目录，**-**：文件  
> 剩下9位，3位一组  
> rwx：所有者拥有的 **读** **写** **执行** 权限  
> rwx：同组用户拥有的  **读** **写** **执行** 权限  
> r-x：其他人拥有的  **读** **执行** 权限  
> huadian huadian：所有者 所属组  
> 4096：文件大小  
> Mar  6 09:14: 日期
> b: 文件名

#### 修改权限

> 语法： Chmod  [添加或者删除权限]  文件或者目录  
##### 通过字符修改

> 给其他人添加 写的 权限

![](chmod1.png)

> 给其他人删除 写的 权限

![](chmod2.png)

> 给任何人都添加上 可执行 权限  

![](chmod3.png)

> 给所属者和同组用户删除权限

![](chmod4.png)

##### 通过数组修改

> **说明**  
> 该位有权限用 1 表示,没有用 0 表示  
> **rwx**  111  => 7(十进制)  
> **r-x**  101  => 5(十进制)  
> **rw-**  110  => 6(十进制)

> 例子：让 **任何用户** 拥有 **全部权限**  
> 例子：chmod 777 a.txt

![](chmod5.png)

> 例子：让 **任何用户** 拥有 **读写** 没有 **执行** 的权限  
> 例子：chmod 666 a.txt

![](chmod6.png)

#### 修改文件 所有者 和 所属组

> 语法格式：**chown username:groupname 文件或者目录**  
> 参数： -R 递归修改所有的文件  
> 例：chown -R root:root b

![](chown.png)

### 常见符号

#### .

> 表示当前目录

#### .

> 表示上一级目录

#### ~

> 用户的家目录  
> 例如： cd ~

![](gohome.png)

#### |

> 表示管道符  
> 例如：Ps –ef | grep java   过滤留下java的进程

#### >

> 覆盖重写某个文件

#### >>

> 追加到某个文件

![](override.png)

### 文本编辑 vi / vim

#### 语法格式

>　vi 文件名
>  如果编辑一个不存在的文件，默认会创建一个文件

#### 种模式

##### 命令行模式

> gg：跳转到第一行  
> G:跳转到最后一行  
> 5gg:跳转到第5行  
> yy:复制当前行  
> p:在当前位置粘贴  
> dd:删除光标所在行  
> 5dd:从当行开始，删除连续5行  
> 99999999999999dd: 清除文档  
> u:撤销上一步  
> i:进入插入模式  
> o:在光标所在行 下一行 进入插入模式  
> O:在光标所在乎 前一行进入插入模式   


##### 插入模式

> 在命令模式下，输入“i”,进入插入模式

##### 最后行模式

> 进入方式:  
> 按下“ESC”进入命令模式  
>	按下shift + : 进入最后行模式

> 命令: 
> wq：保存退出  
>	q: 不保存退出  
>	!: 强制  
>	q!: 强制退出  
>	set nu(number): 显示行号  
>	/string 检索字符串  按“n”找下一个

### 常用工具命令 

#### visudo (较重要)

> 在企业中，通常不会使用root，为了安全，一般使用普通用户。  
> 普通用户有时候，也需要操作一些管理员需要的权限

> 例：让 huadian 用户拥有 root 所有权限   操作如下  
> 切换到 root 用户  
> 执行 visudo 命令  
> 添加如下配置

![](visudo.png)

> 普通用户如果使用额外赋予的权限  
> sudo 额外权限名

![](sudo.png)

#### man 

> 帮助命令
> 例: man ls

#### wc

> 文本统计工具  
> 例： wc a.txt   
> 结果：行 单词书 字符数 文件名

![](wc.png)

#### du

> 文件大小统计  
> du -a.txt/

![](du.png)

### 系统管理命令

#### 关机

> shutdown  -h now  
>	halt  
>	init 0

#### 开机选项配置

> Vi /etc/inittab  
> id:3:initdefault:  启动linux，不启动图形化界面  
> init 5:恢复图形化界面

![](option.png)

#### 重启

> init 6 
> reboot

### 服务管理命令

> 语法： service s_name start|stop|status|restart|  
> 查看防火墙: sudo service iptables status  
> 查看网络：sudo service network status  
> Mysql数据：mysql/mysqld(低版本)

### chkconfig 查看或设置开机是否启动

> 查看开机是否启动  
> chkconfig iptables –list  
> 结果：2~5都是on :开机启动

![](chkconfig.png)

> 设置开机是否启动  
> chkconfig iptables off|on

![](chkconfig2.png)

### 进程管理

#### 查看进程 ps

>　ps –ef |grep java  
> 含义：查看所有 java 进程

#### 杀死进程 kill

> kill -9 进程id  
> 含义：彻底杀死进程

### Redhat的slinux安全机制

> vi /etc/selinux/config  
> 重启成效

![](redhatsecurity.png)

### 压缩管理

#### zip
#### gzip
> 后缀: .gz  
> 压缩: gzip file_path  
> 解压: gunzip file.gz   
> 特点: 压缩后源文件没有了,  不能对目录进行压缩

#### bzip2
> 适合较大文件压缩  
>	后缀.bz2  
>	压缩：bzip2 file_path  
>	解压：bunzip2 file.bz2  
> 特点：压缩后源文件没有了,不能对目录进行压缩

#### tar
打包命令，将多个文件或者目录打包成一个文件  
> 格式：  
> 打包：tar [选项参数]  file.tar  source   
> 解包：tar  [选项参数]  file.tar –C  source  
> 参数：  
> -C: 指定解压的目录  
> -x: 解压  
> -v: 是否显示进度  
> -z: 是否使用gzip压缩  
> -j: 是否使用bzip2压缩  

> 应用（使用最频繁）：使用tar + gzip  
> 压缩：tar –zcvf  jdk-8u91-linux-x64.tar.gz  source  
>	解压：tar –zxvf jdk-8u91-linux-x64.tar.gz  -C  target  
	
> 使用 tar + bzip2  
> 压缩：tar –jcvf  jdk-8u91-linux-x64.tar.gz  source  
>	解压：tar –jxvf jdk-8u91-linux-x64.tar.gz  -C  target

以上差不多是 Linux 工作中常用到的命令。






















 













