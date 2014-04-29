---
layout: post
title: "指针和引用"
description: ""
category: 
tags: ["c/c++"]
---
`返回值为引用`如果函数返回的是引用，调用者的处理方式跟其声明有关

{% highlight c++ %}
string& foo(string& x){ 
    x = "hello " + x;
    return x;
}

int main(){
    string x{"rolex"};
    string& y = foo(x);
    y += " test";
    cout << "x : " << x << endl;
    cout << "y : " << y << endl;
    return 0;
}
{% endhighlight %}

这种情况下y是x的引用，对y的修改会影响x，因此输出为  
>x : hello rolex test  
>y : hello rolex test

{% highlight c++ %}
string& foo(string& x){ 
    x = "hello " + x;
    return x;
}

int main(){
    string x{"rolex"};
    string y{foo(x)};
    y += " test";
    cout << "x : " << x << endl;
    cout << "y : " << y << endl;
    return 0;
}
{% endhighlight %}

这种情况下y是x的引用来进行初始化，对y的修改不会影响x，因此输出为  
>x : hello rolex  
>y : hello rolex test
