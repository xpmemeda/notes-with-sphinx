variant
=======

``std::variant`` 定义在头文件 ``<variant>`` 中。

基本用法
--------

1. 初始化：该结构可以代表多种类型，初始化列表中有且仅能有其声明的某一项

 - ``std::variant`` ：必须声明模板类型。

2. 提取内容

 - ``std::get`` ：需要类型或者索引，当此处索取的类型和变量实际拥有的类型不一致时报错。

.. warning::

    ``std::get`` 是一个常量表达式，无法在运行时确定要获取哪个值。

.. code-block:: cpp

    #include <variant>
    #include <string>
    
    int main()
    {
        std::variant<int, float> v, w;
        v = 12; // v contains int
        int i = std::get<int>(v);
        w = std::get<int>(v);
        w = std::get<0>(v); // same effect as the previous line
        w = v; // same effect as the previous line
    
        //  std::get<double>(v); // error: no double in [int, float]
        //  std::get<3>(v);      // error: valid index values are 0 and 1
    
        try {
        std::get<float>(w); // w contains int, not float: will throw
        }
        catch (const std::bad_variant_access&) {}
    }

.. warning::

    ``std::variant`` 在初始化时可以完成隐式的类型转换，但是可能会出现两者皆可的情况：

    ``std::variant<std::string, void const*> y("abc")``

动态类型
-------

在Python代码中，函数的返回类型可以在运行时再决定，而C++不支持这样，C++的所有类型都已经在编译时确定。

``std::variant`` 可以解决这一问题，但其并没有改变C++这门语言的特性，因为 ``std::variant`` 本身的类型就是确定的。

.. code-block:: cpp

    #include <iostream>
    #include <variant>

    std::variant<bool, int, std::string>
    foo(const std::string & s) {
        std::variant<bool, int, std::string> v;
        if (s == "bool") {
            v = false;
        } else if (s == "int") {
            v = 0;
        } else {
            v = "other";
        }
        
        return v;
    }


std::visit
----------

**以下是猜测。。。**

``std::invoke`` 没办法处理 ``std::variant`` 这种输入，原因是不知道这个 ``std::variant`` 当前持有的类型。

而 ``std::visit`` 可以让 ``std::variant`` 作为参数传入时，坍缩成其当前持有的类型。

.. code-block:: cpp

    #include <iostream>
    #include <functional>
    #include <variant>
    #include <string>
    #include <vector>

    using var_t = std::variant<int, long, double, std::string>;

    int main() {
        std::vector<var_t> vec = {10, 15l, 1.5, "hello"};
        for(auto& v: vec)
            // arg 的类型依次是 int, long, double, std::string
            std::visit([](auto&& arg){std::cout << arg << std::endl;}, v);

            // 编译时报错，arg 每次都是 variant 类型，而它没有定义 << 操作符
            // std::visit([](auto&& arg){std::cout << arg << std::endl;}, v); 
    }
