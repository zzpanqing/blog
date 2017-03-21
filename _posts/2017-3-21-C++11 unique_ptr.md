# C++11 unique_ptr #

* 我有一个 unique_ptr member, 我的 getter 不能 return unique_ptr member, 要返回 member.get()

* unique_ptr member 不用初始化，default constructor 将 unique_ptr 初始化成 vide 的, 但本身不为 null. 

## 作为用于工厂函数 factory 的返回类型 ##

[modern effective c++ 中的例子](http://blog.csdn.net/big_yellow_duck/article/details/52317819)

假设我们有 Base class: Investment, 及其子类 Stock(股票), Bond(债券), RealEstate（房地产）
    
    class Investment { ... };

    class Stock : public Investment { ... };

    class Bond : public Investment { ... };

    class RealEstate : public Investment { ... };

工厂函数通常从堆上分配一个对象，然后返回一个指向它的指针，当不再需要它的时候，调用者要负责delete这个对象。那真是完美匹配std::unique_ptr，因为调用者需要为工厂返回的资源负责（即独占资源的所有权），然后std::unique_ptr在它销毁的时候可以自动 delete 它指向的对象。
    
    {                  
      ...
      auto pInvestment =                       // pInvestment的类型是
            makeInvestment(*arguments*);       // std::unique_ptr<Investment>
      ...
    }    // 销毁 *pInvestment

deleter 可以是函数对象，也可以是 lambda

    auto delInvmt = [](Investment *pInvestment)   // 自定义的删除器
                {                                 // 一个lambda表达式
                    makeLogEntry(pInvestment); 
                    delete pInvestment;
                };

    template <typename... Ts>
    std::unique_ptr<Investment, decltype(delInvmt)>   // 修改返回类型
    makeInvestment(Ts&&... params)
    {`
        std::unique_ptr<Investment, decltypedelInvmt)>
           pInv(nullptr, delInvmt);                   // 将要返回的指针
        if ( /* a Stock object should be created */ )
        {
            pInv.reset(new Stock(std::forward<Ts>(params)...));
        }
        else if (/* a Bond object should be created */) 
        {
            pInv.reset(new Bond(std::forward<Ts>(params)...));
        }
        else if ( /* a RealEstate obejct should be created */ )
        {
            pInv.reset(new RealEstate(std::forward<Ts>(params)...));
        }

        return pInv;
    }
    
对程序的几点说明:

* 调用者使用 auto 存储 makeInvestment 的返回值，完全不用关心资源的释放。

      auto pInvestment = makeInvestment(*arguments*);
* delInvmt是从makeInvestment返回对象的自定义删除器.所有的自定义删除函数都是接受一个指向需要销毁的对象的原生指针作为参数. 在这个例子中，删除器的行为是调用makelogEntry，然后应用delete.
* 我们使用自定义的删除器的时候，它的类型要作为std::unique_ptr的第二个模板参数。这里 deleter 是 delInvmt, 所以模板参数是 decltype(delInvmt).
   
      std::unique_ptr<Investment, decltype(delInvmt)>
* 解释 

      std::unique_ptr<Investment, decltypedelInvmt)>  pInv(nullptr, delInvmt);      

  makeInvestment的基本策略是先创建一个空的std::unique_ptr，然后指向一个类型合适的对象，然后返回它。为了关联删除器delInvmt和pInvmt，我们把删除器作为第二个构造参数传递给std::unique_ptr。
  
* 用 reset 将 new 出来的对象付给 unique_ptr

      pInv.reset(new Stock(std::forward<Ts>(params)...));
  试图将原生指针（例如new出来的指针）赋值给std::unique_ptr是不会通过编译的，因为这导致一个从原生指针到智能指针的隐式转换，这样的隐式转换是有问题的，所以C++11的智能指针禁止这样的转换。
  
* 
  
