```bash
# 测试系统 centos6.9 
# 安装gcc7.1.0
# gcc的编译需要较长时间, 编译后的文件夹大小接近4个g, 本地虚拟机编译用了十几个小时(笔记本,配置较差)

# 安装依赖库
# ftp://gcc.gnu.org/pub/gcc/infrastructure/

# GMP4.2+
wget -c ftp://gcc.gnu.org/pub/gcc/infrastructure/gmp-6.1.0.tar.bz2
tar jxf gmp-6.1.0.tar.bz2
cd gmp-6.1.0
./configure --prefix=/usr/local/gmp-6.1.0
make
make install


# MPFR2.4.0+
wget -c ftp://gcc.gnu.org/pub/gcc/infrastructure/mpfr-3.1.4.tar.bz2
tar jxf mpfr-3.1.4.tar.bz2
cd mpfr-3.1.4
./configure --prefix=/usr/local/mpfr-3.1.4 \
--with-gmp=/usr/local/gmp-6.1.0
make 
make install


# MPC0.8.0+
wget -c ftp://gcc.gnu.org/pub/gcc/infrastructure/mpc-1.0.3.tar.gz
tar zxf mpc-1.0.3.tar.gz
cd mpc-1.0.3
./configure --prefix=/usr/local/mpc-1.0.3 \
--with-gmp=/usr/local/gmp-6.1.0 \ --with-mpfr=/usr/local/mpfr-3.1.4
make
make install


# 设置环境变量，避免库找不到的问题
vim /etc/bashrc
# 增加下面的内容
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/mpc-1.0.3/lib:/usr/local/gmp-6.1.0/lib:/usr/local/mpfr-3.1.4/lib
# 如果加在profile文件中会导致安装完成后编译其他软件的时候找不到动态加载库
# 生效
source /etc/bashrc


# 先安装GMP，其次MPFR，最后才是MPC
# http://ftp.tsukuba.wide.ad.jp/software/gcc/releases
wget -c ftp://gcc.gnu.org/pub/gcc/releases/gcc-7.1.0/gcc-7.1.0.tar.gz
tar zxf gcc-7.1.0.tar.gz
cd gcc-7.1.0
./configure \
--prefix=/usr/local/gcc-7.1.0 \ -enable-threads=posix \
-disable-checking \
-disable-multilib \
-enable-languages=c,c++ \
--with-gmp=/usr/local/gmp-6.1.0 \ --with-mpfr=/usr/local/mpfr-3.1.4 \ --with-mpc=/usr/local/mpc-1.0.3
make 
make install


# yum安装的4.4.7版本改名
mv /usr/bin/gcc /usr/bin/gcc44
mv /usr/bin/g++ /usr/bin/g++44
# 不少低版本编译的软件使用gcc7.1编译会报错, 可以使用export配置, 如下
export CC=gcc44
export CXX=g++44


# 加入环境变量
vim /etc/profile
# 加入下面语句
export $PATh=$PATH:/usr/local/gcc-7.1.0/bin
# 生效
source /etc/profile

# 给gcc增加软连接(redis编译的时候有用到)
cd /usr/local/gcc-7.1.0/bin
ln -s /usr/local/gcc-7.1.0/bin/gcc ./cc


# 给gcc添加man帮助
vim /etc/man.config
# 找到MANPATH, 增加下面这行
MANPATH /usr/local/gcc-7.1.0/share/man
```