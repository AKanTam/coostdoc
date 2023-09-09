---
weight: 32
title: "编译"
---


## 编译器要求

各平台需要安装的编译器如下：

- Linux: [gcc 4.8+](https://gcc.gnu.org/projects/cxx-status.html#cxx11)
- Mac: [clang 3.3+](https://clang.llvm.org/cxx_status.html)
- Windows: [vs2015+](https://visualstudio.microsoft.com/)




## xmake

co 推荐使用 [xmake](https://github.com/xmake-io/xmake) 作为构建工具。



### 安装 xmake

windows, mac 与 debian/ubuntu 可以直接去 xmake 的 [release](https://github.com/xmake-io/xmake/releases) 页面下载安装包，其他系统请参考 xmake 的 [Installation](https://xmake.io/#/guide/installation) 说明。



### 快速构建

在 co 根目录执行下述命令构建：

```sh
xmake -a    # 构建所有项目 (libco, gen, test, unitest)
```

若需要使用 HTTP 或 SSL 特性，则可以用下面的命令构建：

```sh
xmake f --with_libcurl=true --with_openssl=true
xmake -a
```

启用 HTTP 或 SSL 特性时，xmake 会自动从网络安装 libcurl 与 openssl，可能需要花点时间。

命令行中的 `-a` 表示构建 co 中的所有项目，如果不加 `-a`，默认只会构建 libco。另外，可以用 `-v` 或 `-vD` 让 xmake 打印更详细的编译信息：

```sh
xmake -v -a
```



### 编译选项

xmake 提供了 `xmake f` 命令，用于配置编译选项。需要注意的是，**多个配置选项必须在一条 xmake f 命令中完成**，若多次执行 `xmake f` 命令，后面的会覆盖前面的配置。


#### 编译 debug 版本的 libco

```sh
xmake f -m debug
xmake -v
```


#### 编译动态库

```bash
xmake f -k shared
xmake -v
```



#### 编译 32 位的 libco

- Windows

```bash
xmake f -a x86
xmake -v
```

- Linux

```bash
xmake f -a i386
xmake -v
```

`xmake f` 命令中的 `-a` 表示 arch，不同平台支持的 arch 可能不一样，可以执行 `xmake f --help` 命令查看详情。


#### Windows 平台指定 vs_runtime

co 在 Windows 平台默认使用 **MT** 运行库，用户可以用下面的命令配置 vs_runtime：

```cpp
xmake f --vs_runtime=MD
xmake -v
```


#### Android 与 IOS 支持

co 在 Android 与 IOS 平台也能编译，详情见 [Github Actions](https://github.com/idealvin/coost/actions)。目前暂未在 Android 与 IOS 上测试。

- android

```bash
xmake f -p android --ndk=/path/to/android-ndk-r21
xmake -v
```

- ios

```bash
xmake f -p iphoneos
xmake -v
```



### 构建及运行 unitest 代码

[co/unitest](https://github.com/idealvin/coost/tree/master/unitest) 是单元测试代码，可以执行下述命令构建及运行：

```bash
xmake -b unitest        # build unitest
xmake r unitest -a      # 执行所有单元测试
xmake r unitest -os     # 执行单元测试 os
xmake r unitest -json   # 执行单元测试 json
```



### 构建及运行 test 代码

[co/test](https://github.com/idealvin/coost/tree/master/test) 是一些测试代码，在 co/test 目录或其子目录下增加 `xx.cc` 源文件，然后在 co 根目录下执行 `xmake -b xx` 即可构建。

```bash
xmake -b flag                # 编译 test/flag.cc
xmake -b log                 # 编译 test/log.cc
xmake -b json                # 编译 test/json.cc
xmake -b rpc                 # 编译 test/rpc.cc

xmake r flag -xz             # 测试 flag 库
xmake r log                  # 测试 log 库
xmake r log -cout            # 终端也打印日志
xmake r log -perf            # log 库性能测试
xmake r json                 # 测试 json
xmake r rpc                  # 启动 rpc server
xmake r rpc -c               # 启动 rpc client
```



### 构建及使用 gen

```bash
xmake -b gen
cp gen /usr/local/bin/
gen hello_world.proto
```



### 安装 libco

构建完 libco 后，可以用 `xmake install` 命令安装 libco 到指定的目录：

```bash
xmake install -o pkg          # 打包安装到 pkg 目录
xmake i -o pkg                # 同上
xmake i -o /usr/local         # 安装到 /usr/local 目录
```



### 从 xmake repo 安装 libco

```cpp
xrepo install -f "openssl=true,libcurl=true" coost
```




## cmake

[izhengfan](https://github.com/izhengfan) 帮忙提供了 cmake 编译脚本：

- 默认只编译 libco。
- 编译生成的库文件在 build/lib 目录下，可执行文件在 build/bin 目录下。
- 可以用 **BUILD_ALL** 指定编译所有项目。
- 可以用 **CMAKE_INSTALL_PREFIX** 指定安装目录。
- cmake 只提供简单的编译选项，若需要更复杂的配置，请使用 xmake。



### 默认构建 libco

```bash
mkdir build && cd build
cmake ..
make -j8
```



### 构建所有项目

```bash
mkdir build && cd build
cmake .. -DBUILD_ALL=ON -DCMAKE_INSTALL_PREFIX=pkg
make -j8
```



### 启用 HTTP 与 SSL 特性 

使用 HTTP 或 SSL 特性，需要安装 libcurl, zlib, 以及 openssl 1.1.0 或以上版本。

```sh
mkdir build && cd build
cmake .. -DBUILD_ALL=ON -DWITH_LIBCURL=ON -DWITH_OPENSSL=ON
make -j8
```
