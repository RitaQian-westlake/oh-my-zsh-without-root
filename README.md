# oh-my-zsh-without_root
🤝💡🚀🔬📚💻🌐💬🔍🎯🔄🧪🕵️

## 第一步：
非root环境下安装zsh：https://blog.csdn.net/TinyJian/article/details/84034812


## 安装好并解压ncurses和zsh后

- tar -zxvf ncurses.tar.gz

- tar -xvf zsh.tar

### 下述已经存在 zsh.sh 中，直接bash zsh.sh即可
```sh
cd ncurses  # assuming /home/username/soft/ncurses
mkdir build
export CXXFLAGS=" -fPIC"
export CFLAGS=" -fPIC"
./configure --prefix=/home/username/soft/ncurses/build --enable-shared
make -j $(nproc)
make install
cd ..
cd zsh
mkdir build
INSTALLATION_PATH=/home/username/soft/zsh/build
export PATH=$INSTALLATION_PATH/bin/:$PATH
export LD_LIBRARY_PATH=$INSTALLATION_PATH/lib:$LD_LIBRARY_PATH
export CFLAGS=-I$INSTALLATION_PATH/include
export CPPFLAGS="-I$INSTALLATION_PATH/include"
export LDFLAGS="-L$INSTALLATION_PATH/lib"

# 如果gcc版本为14以上，需要从https://github.com/openwrt/packages/tree/master/utils/zsh/patches 下载全部patch，置于当前路径下，并按照序号逐个patch
patch -p1 < xxx.patch

./configure --prefix=$INSTALLATION_PATH --enable-shared
make -j $(nproc)
make install
cd ..

# 修改启动脚本，自动切换到zsh
echo 'export PATH="/home/username/soft/zsh/build/bin:$HOME/.local/bin:$PATH"' >> ~/.profile
echo '[ -f /home/username/soft/zsh/build/bin/zsh ] && exec /home/username/soft/zsh/build/bin/zsh -l' >> ~/.profile
```

### 以下部分需要单独运行
```sh
exit #退出
#....connect...

touch ~/.zshrc
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
zsh
```
【这个地方如果没有网的话，直接把ohmyzsh的项目下载下来，上传服务器，改名为.oh-my-zsh】


在文件夹/home/chenzhuo/.oh-my-zsh/tools中把install.sh文件修改：
- 搜索字符串 already, 将`If this user's login shell is already "zsh", do not attempt to switch.`下的 if 语句删除

然后执行安装脚本 install.sh 文件
`sh install.sh --skip-chsh`

~~当遇到：Do you want to change your default shell to zsh? [Y/n] 选择 **n**~~


## 替换 ~/.zshrc
### 其中自定义主题：
```sh
ZSH_THEME="agnoster"
export AGNOSTER_CONTEXT_BG=23  # customize color by changing id
export AGNOSTER_GIT_CLEAN_BG=178
export AGNOSTER_DIR_BG=075
```

----
然后
```sh
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
### PS： 如果无法联网可以直接上传(github项目解压上传之后直接放下面这个目录)
到这个文件夹：/home/username/.oh-my-zsh/custom/plugins

```sh
git clone git://github.com/wting/autojump.git
./install.py
```
然后在`.zshrc`中添加`[[ -s /home/username/.autojump/etc/profile.d/autojump.sh ]] && source /home/username/.autojump/etc/profile.d/autojump.sh`

autojump 注意：如果下载不了需要去.zshrc里面删除包含autojump的两行

# 离线环境迁移
https://zhuanlan.zhihu.com/p/540615230

没有虚拟环境的，可以把当前base环境先拷贝一份副本然后再打包

# 无网络环境安装第三方库
https://blog.csdn.net/Cameron_Rin/article/details/120790606
https://blog.csdn.net/luoluonuoyasuolong/article/details/123648032
https://blog.csdn.net/vincent_duan/article/details/120128108
```
先
pip download -r depency.txt -d "/home/chenzhuo/pkage" -i https://pypi.tuna.tsinghua.edu.cn/simple/
然后
scp -r xxx:/home/chenzhuo/pkage /home/chenzhuo/
或者
scp -r xxx:/home/chenzhuo/pkage /data/chenzhuo/
然后：
pip install --no-index --find-links=/home/chenzhuo/pkage tensorboard
或者
pip install --no-index --find-links=/home/chenzhuo/pkage -r depency.txt
```
其中：
- 可选：```--no-deps```指不安装依赖
- ```depency.txt```指需要下载的包
- ```tensorboard``` 是一个例子，也可以是存在于```dependency.txt```中

# 非root场景安装htop
```
1、上官网http://hisham.hm/htop/releases/下载最新的包
2、解压缩：tar -zxvf htop-2.2.0.tar.gz;
3、进入目标文件夹： cd htop-2.2.0
4、./configure --prefix=/home/chenzhuo/htop
5、make && make install
```
此时，在/home/username/htop/bin目录下，会生成可执行文件htop, 以下命令加在最后

```
cd /home/username
vim .bashrc
export  PATH=/home/chenzhuo/htop/bin:$PATH
source .bashrc
```
参考：
- https://blog.csdn.net/Xminyang/article/details/109344669

<a href="https://info.flagcounter.com/JO7r"><img src="https://s11.flagcounter.com/map/JO7r/size_l/txt_000000/border_CCCCCC/pageviews_0/viewers_0/flags_0/" alt="Flag Counter" border="0"></a>
