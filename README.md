
# ijkplayer
已编译好的ijkplayer支持ffmeg所支持的所有格式.
如需自己编译参照如下指南

# MAC编译ijkplayer指南(Android版本)
## 一.预备编译环境
1. 准备SDK:   
如没有安装Android studio ,自行Google(https://developer.android.com/studio/#downloads).   
如已经装有Android Studio ,可在用户目录下的library找到已安装的SDK.
  路径如下: /Users/{****:你自己的用户名,不知道的话我也没法了}/Library/Android

2. 准备NDK,只能使用(10,11,12,13,14)这几个版本,不要尝试最新,哔哩哔哩不干你也没法(https://developer.android.com/ndk/downloads/older_releases).

3. 开始搭建环境(**install homebrew, git, yasm**)
找到你的MAC上的终端,照着下面开始敲代码   
`ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)`   
`brew install git`   
`brew install yasm`   


## 二、准备源码
https://github.com/Bilibili/ijkplayer#build-android
1. 配置环境变量
$ export ANDROID_SDK=$HOME/android-sdk
// $ export ANDROID_NDK=$ANDROID_SDK/ndk-bundle //此项目最高支持NDK14的版本，因此用下面的位置。
$ export ANDROID_NDK=$ANDROID_SDK/android-ndk-r14b
2. 下载ijkplayer源代码
$ cd ~
$ git clone https://github.com/Bilibili/ijkplayer.git ijkplayer-android
$ cd ijkplayer-android
$ git checkout -B latest k0.8.8
3. 下载相关的android ffmpeg源代码
$ ./init-android.sh
4. 备份源码
$ cd ~
$ tar cvzf ijkplayer-android.tar.gz ijkplayer-android
## 三、编译
1. 用bash代替dash
$ sudo dpkg-reconfigure dash
选择NO
如果新打开的shell，记得按“一1”配置ANDROID_SDK和ANDROID_NDK环境变量。
2. 选择解码包
(1)默认是较少的codec/format生成较小尺寸的包。
(2)在(1)的基础上包含hevc功能
(3)最多的codec/format
如果选择(1)请直接到下一步。否则继续操作：
$ cd ~/ijkplayer-android/config
$ rm module.sh
$ ln -s module-lite-hevc.sh module.sh  <<<< (2)
$ ln -s module-default.sh module.sh    <<<< (3)
注意，选择(2)需要为ffmpeg额外安装latm，选择(3)可能需要手动安装更多的外部库。
3. 编译ffmepg
$ cd ~/ijkplayer-android/android/contrib
$ ./compile-ffmpeg.sh clean
$ ./compile-ffmpeg.sh all
成功进行下一步。
如果报错：fatal error: linux/perf_event.h: No such file or directory
$ vim ~/ijkplayer-android/config/module.sh
在结尾加入这一行：
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-linux-perf"
保存后执行
$ ./compile-ffmpeg.sh clean
$ ./compile-ffmpeg.sh all
4. 编译ijkplayer
$ cd ~/ijkplayer-android/android
$ ./compile-ijk.sh all
5. 备份成果
$ cd ~
$ tar cvzf ijkplayer-android-build.tar.gz ijkplayer-android
