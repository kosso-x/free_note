
# gcc更新链接

## [gcc更新-简书][gcc_update]

1. * 查看gcc版本
     `gcc -v`
2. * 安装开发必备环境:
     `yum install glibc-static libstdc++-static`
3. * GCC源码地址为http://ftp.gnu.org/gnu/gcc, 里面有GCC的各个版本(下载好后放到指定目录)
4. * 解压文件
     `tar -zxvf gcc-8.3.0.tar.gz`
5. * 解压完成，进入文件目录
     `cd gcc-8.3.0`
6. * 利用源码包里自带的工具下载所需要的依赖项
     `./contrib/download_prerequisites`
7. * 创建编译输出目录
     `mkdir build`
8. * 进入build目录
     `cd build`
9. * 生成Makefile
     `../configure --enable-checking=release --enable-languages=c,c++ --disable-multilib`
10. * 编译
      `make`
11. * 编译完成之后，安装
      `make install`
12. * 检查是否安装成功(可能需要重启)
      `gcc -v`
      [解决类似 /usr/lib64/libstdc++.so.6: version `GLIBCXX_3.4.21' not found 的问题][GLIBCXX_3.4.21 not found]
13. * 运行以下命令检查动态库
      `strings /usr/lib64/libstdc++.so.6 | grep GLIBC`
14. * 发现没有GLIBCXX_3.4.21
15. * 找找有没有别的新的库
      `find / -name "libstdc++.so*"`
16. * 发现了有libstdc++.so.6.0.25版本的
17. * 进去看看有没有缺失的文件
      `strings /usr/local/lib64/libstdc++.so.6.0.25 | grep GLIBC`
18. * 有缺失的库
19. * 替换/usr/lib64/libstdc++.so.6，原来是libstdc++.so.6.0.20版本的软链替换为libstdc++.so.6.0.25的软链
20. * 删除原来的软链
      `rm -rf /usr/lib64/libstdc++.so.6`
21. * 创建新链接
      `ln -s /usr/lib64/libstdc++.so.6.0.25 /usr/lib64/libstdc++.so.6`

[gcc_update]: https://www.jianshu.com/p/cedbdf0b6bca
[GLIBCXX_3.4.21 not found]: https://www.jianshu.com/p/28a0c98027a8
