# C++11 unique_ptr #

* 我有一个 unique_ptr member, 我的 getter 不能 return unique_ptr member, 要返回 member.get()

* unique_ptr member 不用初始化，default constructor 将 unique_ptr 初始化成 vide 的, 但本身不为 null. 
