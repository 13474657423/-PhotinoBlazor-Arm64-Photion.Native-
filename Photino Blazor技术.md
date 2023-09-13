# Photino Blazor技术

## 1.问题

1.在运行Photino例子程序时，出现以下问题：

在ubuntu系统上运行正常，但是发布为arm64程序并在嵌入式arm64系统开发板是总是提示

![image-20230912165646362](C:\Users\ActionPower\AppData\Roaming\Typora\typora-user-images\image-20230912165646362.png)

根据提示是Photino.Native.so文件的问题，然后使用

```pascal
file Photino.Native.so
```

提示

```pascal
root@ok3568:~/blazor/linux-arm64# file Photino.Native.so 
Photino.Native.so: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, BuildID[sha1]=9252f20a763f988ffb41c32a11f25b2747a74239, not stripped  
```

从返回的信息中可以看出此库位X64系统的库，于是就到官网下载了源代码，然后拷贝到arm64开发板使用makefile进行编译。具体步骤:

1.将下载的源代码文件拷贝到arm64开发板中

2.打开终端定位到photino.Native-2.4.1目录下，因为文件已经编译好了makefile文件直接使用make命令执行。具体命令如下

```pascal
root@ok3568:~/photino.Native-2.4.1# make linux-arm64
```

3.编译时出错，提示错误:

```
./Photino.Native/Photino.Linux.cpp: In constructor ‘Photino::Photino(PhotinoInitParams*)’:
./Photino.Native/Photino.Linux.cpp:161:74: warning: comparison with string literal results in unspecified behavior [-Waddress]
  161 |  if (initParams->WindowIconFile != NULL && initParams->WindowIconFile != "")
```

从结果看是代码问题，就使用vi命令进入到代码将 && initParams->WindowIconFile != ""删除。然后再使用 make linux-arm64命令编辑就成功了。

4.使用file Photino.Native.so查看文件

```pascal
root@ok3568:~/blazor/linux-arm64# file Photino.Native.so 
Photino.Native.so: ELF 64-bit LSB shared object, ARM arrch64, version 1 (SYSV), dynamically linked, BuildID[sha1]=9252f20a763f988ffb41c32a11f25b2747a74239, not stripped  
```

说明此时生成的库为arm64系统的库，将此库拷贝到程序文件中，然后运行程序。

![image-20230912175311379](C:\Users\ActionPower\AppData\Roaming\Typora\typora-user-images\image-20230912175311379.png)

```pascal
sudo apt-get update && sudo apt-get install libgtk-3-dev libwebkit2gtk-4.0-dev
```

