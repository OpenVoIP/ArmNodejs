# Node.js

由于 ARM gcc 版本太老导致不能直接安装官方提供的动态库编译版本, 需要通过交叉编译静态库版本的 Node.

## 编译步骤

- 下载 Node.js

``` shell
  wget https://nodejs.org/dist/v10.14.2/node-v10.14.2.tar.gz
```

- 下载交叉编译工具
 [gcc-linaro-7.3.1-2018.05-x86_64_arm-linux-gnueabihf.tar.xz](https://releases.linaro.org/components/toolchain/binaries/7.3-2018.05/arm-linux-gnueabihf/gcc-linaro-7.3.1-2018.05-x86_64_arm-linux-gnueabihf.tar.xz)
xz 先解压为 tar, 再用 tar 解开包

``` shell
  wget https://releases.linaro.org/components/toolchain/binaries/7.3-2018.05/arm-linux-gnueabihf/gcc-linaro-7.3.1-2018.05-x86_64_arm-linux-gnueabihf.tar.xz
  xz -d gcc-linaro-7.3.1-2018.05-x86_64_arm-linux-gnueabihf.tar.xz
  tar -xvf gcc-linaro-7.3.1-2018.05-x86_64_arm-linux-gnueabihf.tar -C /usr
```

- 配置 profile

``` shell
  export AR_host="ar"
  export CC_host="gcc"
  export CXX_host="g++"
  export LINK_host="g++"

  export PATH=$PATH:/usr/gcc-linaro-7.3.1-2018.05-x86_64_arm-linux-gnueabihf/bin

  export ARCH=arm
  export PATH=/usr/gcc-linaro-7.3.1-2018.05-x86_64_arm-linux-gnueabihf/bin/:$PATH
  export CROSS_COMPILE=arm-linux-gnueabihf-
  export CC=arm-linux-gnueabihf-gcc
  export CXX=arm-linux-gnueabihf-g++
  export LD=arm-linux-gnueabihf-ld
  export AR=arm-linux-gnueabihf-ar
  export AS=arm-linux-gnueabihf-as
  export RANLIB=arm-linux-gnueabihf-ranlib
```

- 配置 Makefile

``` shell
  sudo apt install python-minimal // python 2
  sudo apt-get install g++-multilib // 32
  ./configure --dest-cpu=arm --dest-os=linux --cross-compiling --with-arm-float-abi=hard --without-snapshot --fully-static --without-int
  ./configure --dest-cpu=arm --dest-os=linux --cross-compiling --with-arm-float-abi=hard --without-snapshot
```

## 可能出现的错误
  
- https://github.com/nodejs/node/issues/18620
- Missing or stale config.gypi, please run ./configure 时间设置正确，否则可能提示
- [icupkg 错误](https://askubuntu.com/questions/1036688/exec-format-error-on-node-v8-11-1-out-release-icupkg-while-cross-compiling-nodej)

## 其他

- [iotjs](https://github.com/Samsung/iotjs) 三星的 iotjs 可以比 Nodejs 少 10 倍的内存运行 js 程序， 但是不是完全的兼容 npm 中的库。
- [ARM 源码编译](https://blog.csdn.net/wanyi3605/article/details/78131241)
- [https://blog.csdn.net/sanallen/article/details/80393420](https://blog.csdn.net/sanallen/article/details/80393420)
- [BUILDING](https://github.com/nodejs/node/blob/master/BUILDING.md)
- [Error “gnu/stubs-32.h: No such file](https://stackoverflow.com/questions/7412548/error-gnu-stubs-32-h-no-such-file-or-directory-while-compiling-nachos-source)
