# Linux 服务器开发（c/c++）

## 1.Linux编程开发基础

### 1.1静态库的制作和使用

```shell
gcc -c add.c sub.c div.c mult.c
ar rcs libcal.a add.o sub.o mult.o div.o
#生成.a的lib文件
cp ../calc/libcal.a lib/
#将库文件导入lib文件夹下

#首先要生成.o文件
gcc main.c -o app

gcc main.c -o app -I ./include

gcc main.c -o app -I ./include -l cal -L ./lib
#-I 指定include 包含文件的搜索目录
#-L 指定搜索库的路径
#-l 指定库的名称
```

![image-20210316110232199](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210316110232199.png)

### 1.2动态库的制作

![image-20210316112939597](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210316112939597.png)

```
静态库： GCC 进行链接时，会把静态库中代码打包到可执行程序中
动态库： GCC 进行链接时，动态库的代码不会被打包到可执行程序中
程序启动之后，动态库会被动态加载到内存中，通过 ldd (list dynamicdependencies)命令检查动态库依赖关系
如何定位共享库文件呢？
当系统加载可执行代码时候，能够知道其所依赖的库的名字，但是还需要知道绝对路径。此时就需要系统的动态载入器来获取该绝对路径。对于 elf 格式的可执行程序，是由 ld-linux.so 来完成的，它先后搜索 elf 文件的 DT_RPATH 段 ————> 环境变量LD_LIBRARY_PATH ————> /etc/ld.so.cache 文件列表 ————> / lib/，/usr/lib目录找到库文件后将其载入内存。
```

**env 查看环境变量**

```shell
#动态库创建和使用
#直接将编译出来的动态库进行使用，会导致如下错误
./main: error while loading shared libraries: libcalc.so: cannot open shared object file: No such file or directory

ldd main
#ldd可以列出一个程序所需要得动态链接库（so）
	linux-vdso.so.1 (0x00007fffa6bd1000)
	libcalc.so => not found
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fb461e2c000)
	/lib64/ld-linux-x86-64.so.2 (0x00007fb46241f000)
	
#使用动态库需要将动态库导入到环境变量中
1.利用export在终端导入
export LD_LIBRARY_PATH =$LD_LIBRARY_PATH:~/Linux/Lession3/library/lib
2.将环境变量导入bashrc脚本中
export LD_LIBRARY_PATH =$LD_LIBRARY_PATH:~/Linux/Lession3/library/lib
3.将环境变量添加到/etc/profile
export LD_LIBRARY_PATH =$LD_LIBRARY_PATH:~/Linux/Lession3/library/lib
4.将环境变量添加到/etc/ld.so.conf
/home/wen/Linux/Lession3/library/lib

ldd main
	linux-vdso.so.1 (0x00007ffce8f77000)
	libcalc.so => /home/wen/Linux/Lession3/library/lib/libcalc.so (0x00007f7e7eec7000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f7e7ead6000)
	/lib64/ld-linux-x86-64.so.2 (0x00007f7e7f2cb000)
```

