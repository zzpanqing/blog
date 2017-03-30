# 理解特殊成员函数的生成 #

在C++98，特殊成员函数有四个

* default constructor
* destructor
* copy constructor
* copy assignment operator

用到的时候才生成。

它们是 隐式public和inline的。

某个类继承了 destructor 是virutal 的基类，那么这个类的 destructor 是虚函数。
