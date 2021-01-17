.. _lambda表达式:

Lambda表达式
===========

“Lambda 表达式”(lambda expression)是指匿名函数，基本语法如下：

**[capture list] (parameter list) ->return type { function body }**

- [] // 沒有定义任何变量。使用未定义变量会导致错误。 
- [x, &y] // x 以传值的方式传入，y 以参考方式传入。 
- [&] // 任何被使用到的外部变量皆隐式地以參考方式加以引用。 
- [=] // 任何被使用到的外部变量皆隐式地以传值方式加以引用。 
- [&, x] // x 显式地以传值方式加以引用。其余变量以参考方式加以引用。 
- [=, &z] // z 显式地以参考方式加以引用。其余变量以传值方式加以引用。

.. code-block:: cpp

    class CTest 
    { 
    public:  
    CTest() : m_nData(20) { NULL; }  
    void TestLambda()  
    {   
        vector<int> vctTemp;   
        vctTemp.push_back(1);   
        vctTemp.push_back(2);    

    // 无函数对象参数，输出：1 2   
    {    
        for_each(vctTemp.begin(), vctTemp.end(), [](int v){ cout << v << endl; });   
    }   

    // 以值方式传递作用域内所有可见的局部变量（包括this），输出：11 12   
    {    
        int a = 10;    
        for_each(vctTemp.begin(), vctTemp.end(), [=](int v){ cout << v+a << endl; });   
    }    

    // 以引用方式传递作用域内所有可见的局部变量（包括this），输出：11 13 12   
    {    
        int a = 10;   
        for_each(vctTemp.begin(), vctTemp.end(), [&](int v)mutable{ cout << v+a << endl; a++; });    
        cout << a << endl;   
    }    

    // 以值方式传递局部变量a，输出：11 13 10   
    {    
        int a = 10;    
        for_each(vctTemp.begin(), vctTemp.end(), [a](int v)mutable{ cout << v+a << endl; a++; });    
        cout << a << endl;   
    }    

    // 以引用方式传递局部变量a，输出：11 13 12   
    {    
        int a = 10;    
        for_each(vctTemp.begin(), vctTemp.end(), [&a](int v){ cout << v+a << endl; a++; });    
        cout << a << endl;  
    }    

    // 传递this，输出：21 22 
    {  
        for_each(vctTemp.begin(), vctTemp.end(), [this](int v){ cout << v+m_nData << endl; });   
    }    

    // 除b按引用传递外，其他均按值传递，输出：11 12 17   
    {    
        int a = 10;    
        int b = 15;    
        for_each(vctTemp.begin(), vctTemp.end(), [=, &b](int v){ cout << v+a << endl; b++; });    
        cout << b << endl;   
    }     
    // 操作符重载函数参数按引用传递，输出：2 3   
    {    
        for_each(vctTemp.begin(), vctTemp.end(), [](int &v){ v++; });    
        for_each(vctTemp.begin(), vctTemp.end(), [](int v){ cout << v << endl; });   
    }    
    // 空的Lambda表达式   
    {    
        [](){}();    []{}();   
    }  
    }  
    private:  int m_nData; 
    };
