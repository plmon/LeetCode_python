# LeetCode_python

# 编译安装AFL 2.52b
1. 上[AFL2.52b官网](http://lcamtuf.coredump.cx/afl/)下载源码压缩包afl-2.52b.tgz，放置在linux目录/usr/local/share/下。
2. 解压缩安装包，生成afl-2.52b文件夹<br>
```sudo tar -zxvf afl-2.52b -C afl-2.52b ```
3. 进入afl-2.52b文件夹，执行make命令<br>
```sudo make```
4. 执行make install，/usr/local/share目录下出现afl文件夹则代表安装成功<br>
```sudo make install```

# 使用AFL测试binutils
1. 下载并解压缩binutils安装包
2. 进入binutils解压缩文件夹，设置编译器环境为afl-gcc并执行configure命令，
有两种方式，一种是利用export方式改变编译环境为afl-gcc，一种在执行configure命令时添加```CC=afl-gcc```
```
//第一种方式：
1. export CC=afl-gcc //设置环境为afl-gcc
2. export CXX=afl-g++ //设置g++环境
3. env | grep CC   //查看当前编译器环境
4. env | grep CXX
5. sudo ./configure 

//第二种方式：
1.  sudo CC=afl-gcc ./configure
```
3. make，这一步从输出的信息中可以看到afl插桩，此时afl-gcc编译并安装binutils完成，此时binutils文件夹下会生成objdump、nm-new（nm）、readelf三个文件夹
4. 切换为root身份，创建in、out文件夹，创建空文本放入in文件夹中，执行afl：<br>
```afl-fuzz -i in -o out binutils-2.26/binutils/objdump -d @@```<br>
其中 in为输入文件夹，存放初始文件，out为输出文件夹，存放fuzz结果，@@在实际执行时会替换为替换成测试样本目录下的测试样本。
5. 退出afl：ctrl + c
