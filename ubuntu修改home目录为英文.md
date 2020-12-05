方法一（比较快捷的）：

 

打开终端，在终端中输入命令:  

 

        export LANG=en_US

 

       xdg-user-dirs-gtk-update

 

跳出对话框询问是否将目录转化为英文路径,同意并关闭.
在终端中输入命令:

 

        export LANG=zh_CN
关闭终端,并重起.下次进入系统,系统会提示是否把转化好的目录改回中文.选择不再提示,并取消修改.主目录的中文转英文就完成了~

方法二：


1、先转换成英文

sudo gedit /etc/default/locale

将内容改为：

LANG=”en_US.UTF-8″
LANGUAGE=”en_US:en”

再运行

sudo locale-gen

然后重启，此时界面上会出现一个对话框提示是否将文件夹改成英文的，此时选择“Update...”

2、修改成中文

sudo gedit /etc/default/locale

将内容改为：

LANG="zh_CN.UTF-8"
LANGUAGE="zh_CN:zh"

再运行

sudo locale-gen

然后重启，此时界面上会出现一个对话框提示是否将文件夹改成中文的，此时选择“保持现状...”
其实就是来回切换之后不修改所得到的


方法三：


STEP2: 修改配置文件  ～/.config/user-dirs.dirs ，将对应的路径改为英文名（要和STEP1中修改的英文名对应）

vim ~/.config/user-dirs.dirs

配置文件修改后的内容如下：

XDG_DESKTOP_DIR="$HOME/Desktop"
XDG_DOWNLOAD_DIR="$HOME/Download"
XDG_TEMPLATES_DIR="$HOME/Template"
XDG_PUBLICSHARE_DIR="$HOME/Public"
XDG_DOCUMENTS_DIR="$HOME/Document"
XDG_MUSIC_DIR="$HOME/Music"
XDG_PICTURES_DIR="$HOME/Picture"
XDG_VIDEOS_DIR="$HOME/Video"

————————————————
版权声明：本文为CSDN博主「atgeretg」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/atgeretg/article/details/79192883