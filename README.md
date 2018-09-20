
# ijkplayer
已编译好的ijkplayer支持ffmpeg所支持的所有格式.
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

4. 配置SDK,NDK的环境变量   
`vim .bash_profile`   
`i`      
`export ANDROID_SDK=<你的SDK路径>`   
`export ANDROID_NDK=<你的NDK路径>`   
`esc`   
`:wq`   
`source .bash_profile`   
到这儿准备工作差不多就结束了,下面就是大家伙了

## 二. 准备源码   
`git clone https://github.com/Bilibili/ijkplayer.git ijkplayer-android`   
`cd ijkplayer-android`   
`git checkout -B latest k0.8.8`   
`./init-android.sh`   
等上面执行完,下面👇的代码看需要,作用是编译更多格式的.so   
`cd config`   
`rm module.sh`   
`ln -s module-default.sh module.sh`   
`cd ..`  
## 三. 开始编译
下面开始正式编译ffmpeg工作了,这是一个长长的过程,你喝喝茶摸摸鱼就等结束就好   
`cd android/contrib`   
`./compile-ffmpeg.sh clean`   
`./compile-ffmpeg.sh all`   
***
如果在执行上一步👆时报个错"fatal error: linux/perf_event.h: No such file or directory",不要慌不要慌,执行下面:   
`vim ~/ijkplayer-android/config/module.s`   
`i`
然后在末尾加上   
`export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-linux-perf"`   
然后   
`esc`   
`:wq`
做完了最后再来一遍   
`cd android/contrib`   
`./compile-ffmpeg.sh clean`   
`./compile-ffmpeg.sh all`   
***
到这儿ffmpeg的.so差不多就好了,然后就是编译ijkplayer   
`cd ..`   
`./compile-ijk.sh all`   
再去喝杯茶差不多就结束了,然后整个编译过程就结束了,你可以愉快地去找到你想要的
