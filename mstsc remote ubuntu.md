# Windows 10 利用远程登陆桌面(mstsc)连接到 Ubuntu 18.04.2 LTS

sudo apt install xrdp
sudo systemctl enable xrdp

sudo vim /etc/xrdp/startwm.sh
将最下面的test和exec两行注释掉,添加一行
gnome-session

检查iptables&ufw是否放行3389端口,检查端口的连通性

保存重启ubuntu,重启完毕后,不要登陆

# 问题
# 登入后提示"Authentication is Required to create a color managed device"

    在路径/etc/polkit-1/localauthority.conf.d/处新增一个文件,文件名为 02-allow-colord.conf
    文件内容如下

        polkit.addRule(function(action, subject) {
        if ((action.id == "org.freedesktop.color-manager.create-device" ||
        action.id == "org.freedesktop.color-manager.create-profile" ||
        action.id == "org.freedesktop.color-manager.delete-device" ||
        action.id == "org.freedesktop.color-manager.delete-profile" ||
        action.id == "org.freedesktop.color-manager.modify-device" ||
        action.id == "org.freedesktop.color-manager.modify-profile") &&
        subject.isInGroup("{users}")) {
        return polkit.Result.YES;
        }
        });
 
    保存重启ubuntu,重启完毕后,不要登陆情况下尝试用mstsc xorg进行远程
