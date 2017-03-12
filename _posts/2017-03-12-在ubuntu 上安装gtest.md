# 在ubuntu 上安装gtest

* 安装源代码

        $ sudo apt-get install libgtest-dev        
       
* 编译源代码

        $ cd /usr/src/gtest
 该目录下，有一个源码文件夹,一个cmake文件夹和一个cmake的配置文件(CMakeLists.txt)
        
        $ sudo cmake .
 cmake 生成 makefile, 然后就可以 make 了
 
        $ sudo make
 编译生成的临时问价可以被删掉。
 
* 将编译生成好的库拷贝到系统目录下
 
        $ sudo cp libgtest*.a /usr/local/lib
         
 google gtest的库全部安装好了
