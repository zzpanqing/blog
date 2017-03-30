# 理解特殊成员函数的生成 #

在C++98，特殊成员函数有四个

* default constructor
* destructor
* copy constructor
* copy assignment operator

用到的时候才生成。

它们是 隐式public和inline的。

某个类继承了 destructor 是virutal 的基类，那么这个类的 destructor 是虚函数。

C++11 中，又添加两个新的，编译器可以自动生成的函数 move constructor 和 move assignment operator

    class Widget {
      public:
          ...
          Widget(Widget&& rhs);   // move constructor
                             `
          Widget& operator=(Widget&& rhs);   // move assign operator
          ..
      };
      
move constructor 对 rhs 的 non-static member 逐一请求移动。
move assignment operator 也将 rhs 的 non-static member 逐一请求移动。

请求移动是指，能移动就移动 （使用 std::move），不能移动就拷贝（不支持移动操作的类型，例如，大部分C++98的类）。通过重载函数决策来决定std::move表示为拷贝还是移动.

如果你生成了 move constructor 和 move assign operator，compiler 就不会替你生成。 

move constructor 和 move assignment operator 相互 non independent, 如果你生成了其中一个， compiler 就 move constructor 和 move assignement constructor 都不生成。原因： compiler 生成的默认的 move constructor 和 move assignment operator 试图作 memberwise move.如果你生成了move constructor 或者move assignment operator 就说明你不需要 compiler 提供的memberwise move 这样的行为。    
