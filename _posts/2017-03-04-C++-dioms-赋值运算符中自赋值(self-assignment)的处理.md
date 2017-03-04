# 赋值运算符(assignment operator)中自赋值(self-assignment)的处理

T::operator= 需要处理左操作数 (LHS) 和右操作数 (RHS)指向同一个对象的情况。


    T& operator= (const T& that)
    {
        if (this == &that)
            return *this;

        // handle assignment here

        return *this;
    }

## 注意事项

* identity 和 equality 的意义不同：identity 是指左操作数 LHS和RHS 右操作是同一对象。equality 是指左操作数 LHS 和 右操作数RHS有同样的值。 T::operator= 应该保证 LHS 和 RHS 指向的不是同一个对象。


* 在某些情况下，可以使用 swap. 比如 T 所有的成员（mem1, mem2, ... memeN）都提供了 swap() 函数，那么 operator= 重载就可以这样写：    
    
    '''
    T& operator= (T that)
    {
       // that is constructed by the copy constructor

       mem1.swap (that.mem1);
       mem2.swap (that.mem2);

       ...

       memN.swap (that.memN);
 
       // now what were originally this->mem1, this->mem2, etc. get
       // destroyed when that gets destroyed, and that.mem1, etc. are
       // retained in *this 
       return *this;
    }
    '''
