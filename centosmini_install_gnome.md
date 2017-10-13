```bash
# 测试版本 centos 6.9
# centos 6和7有些区别, 比如启动级别部分, 但大致的流程是这样的

#配置gnome桌面
# 查看安装了哪些软件包组以及可以安装的包组
yum grouplist | less 
yum groupinstall -y "Desktop"   "Desktop Platform" "Graphical Administration Tools"
yum groupinstall -y chinese-support

# 安装完成后重启
reboot

# 执行命令进入gnome桌面
startx

# 修改启动级别为5
vim /etc/inittab

# 配置中文
# 查看当前系统的语言
echo $LANG

# 查看是否有中文语言包
locale

vim /etc/sysconfig/i18n
# 增加
LANG="zh_CN.UTF-8"
vim /etc/profile
# 在export后面增加LANG
export LANG

# 配置输入法
# 在system-> input method里面配置，需要添加中文输入法
# 添加完成后重启，然后界面就是中文了

# 添加右键打开终端
yum -y install nautilus-open-terminal
# 安装成功后重启生效

reboot
# 或
shutdown -r now

```