# VPS服务器初始化配置
[TOC]
## 添加和root权限一样的用户

1.添加用户并创建用户目录，直接按照adduser命令提示就可以了

    adduser admin

    passwd  admin （修改密码）

    然后输入密码   （密码简单了通不过）

    系统提示输入确认密码后再输入一次。OK添加成功。

 2.修改 /etc/sudoers 文件，找到下面一行，在root下面添加一行，如下所示：

    vim /etc/sudoers 
    ## Allow root to run any commands anywhere
    root    ALL=(ALL)     ALL
    admin   ALL=(ALL)     ALL

 这个文件只读是一种保护机制,如果你使用vi编辑器的话,只要保存时使用:wq!就可以保存了。 或者使用visudo命令来进入sudoers文件的编辑，就可以正常保存


    1）单行复制
    在命令模式下，将光标移动到将要复制的行处，按“yy”进行复制；
    2）多行复制
    在命令模式下，将光标移动到将要复制的首行处，按“nyy”复制n行；其中n为1、2、3……
    2、粘贴
    在命令模式下，将光标移动到将要粘贴的行处，按“p”进行粘贴

3.或者直接将新加的用户添加进sudo用户组，命令如下：

    usermod -aG sudo admin

## 在禁用密码登录,禁用root远程登录

要先配置好免密登录，不然退出后你就上不去了

rsync --archive --chown=sammy:sammy ~/.ssh /home/sammy

编辑远程服务器上的sshd_config文件：

vi /etc/ssh/sshd_config


PasswordAuthentication yes改为no    //禁用密码登录

PermitRootLogin yes 改为no          //禁止root用户远程登录

编辑保存完成后，重启ssh服务使得新配置生效，然后就无法使用口令来登录ssh了

systemctl restart sshd.service 


## UFW 防火墙配置


ufw app list   //可以按应用列出防火墙规则


  