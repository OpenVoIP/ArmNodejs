# Node.js [Arm 源码编译](https://blog.csdn.net/wanyi3605/article/details/78131241)

由于 Arm clib 版本太老，不能直接安装官方使用动态库的编译版本，通过交叉编译静态库版本的 Node.

## 编译步骤

- 下载 [Node.js v8.12](https://nodejs.org/dist/v8.12.0/node-v8.12.0.tar.gz)

- 下载交叉编译工具  [gcc-linaro-7.3.1-2018.05-x86_64_arm-linux-gnueabihf.tar.xz](https://releases.linaro.org/components/toolchain/binaries/7.3-2018.05/arm-linux-gnueabihf/gcc-linaro-7.3.1-2018.05-x86_64_arm-linux-gnueabihf.tar.xz)
xz 先解压为 tar, 再用 tar 解开包

``` shell
  xz -d gcc-linaro-7.3.1-2018.05-x86_64_arm-linux-gnueabihf.tar.xz
  tar -xvf gcc-linaro-7.3.1-2018.05-x86_64_arm-linux-gnueabihf.tar -C /usr
```

- 配置

  ``` shell
    ./configure --prefix=/root/armnodejs --dest-cpu=arm --dest-os=linux --cross-compiling --fully-static --with-arm-float-abi=hard --without-snapshot --without-intl
  ```

  时间设置正确，否则可能提示 Missing or stale config.gypi, please run ./configure
  [icupkg 错误](https://askubuntu.com/questions/1036688/exec-format-error-on-node-v8-11-1-out-release-icupkg-while-cross-compiling-nodej)