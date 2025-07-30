---
title: The Four Pillars of OOP in C++
date: 2025-07-17 22:40:23
tags: [C++,OOP]
category: 
- CPP
- OOP
category_bar: true
archive: true
---


## 抽象性 (Abstract)

提取事物的本质特征，找出共性，忽略非本质特征！

- ### 数据抽象

抽象出对象的属性和状态的描述。（变量）

- ### 行为抽象

抽象出对象行为的描述。（成员函数）

## 封装性 (Encapsulation)

1. 设计者将对象的全部属性和行为封装在对象内部，对象的属性值（变量）只能由这个对象的行为（成员函数）来读取和修改！
   
2. 使用者无需关心内部结构，只需关心能做什么、如何使用！
   
### 成员函数的定义：

- 关键词```private```、```protected```、```public```在类中使用的先后次序无关紧要，且**可以多次使用**。
  
- 因为类是一种数据类型，系统**不会为其分配内存空间**，所以在定义类中的数据成员时，不能对其进行初始化，也不能指定其存储类型。对于类内**非static数据成员的初始化**通常**使用构造函数**进行。

1. 在**类内**定义成员函数

```cpp
class cgoods{//商品类
     
     private:
        string ID; //商品编号
        string name; //商品名称
        double Purchasingprice; //进货价格
        double Sellingprice; //售出价格
        int SellCount; //售出数量
        static double Profit; //总利润
     
     protected://无
     
     public:
        cgoods(string id, string name, double purchasingprice){//构造函数
        ID=id;
        Name=name;
        }

        cgoods(string id, string name, double purchasingprice){//构造函数
        ID=id;
        Name=name;
        Purchasingprice=purchasingprice;
        }

        ~cgoods(){//析构函数
        } 

        void SetPurchasingprice(double purchasingprice){//设置进货价格
        Purchasingprice=purchasingprice;
        }

        void SetSellingprice(double sellingprice){//设置出货价格
        Sellingprice=sellingprice;
        }

        void display(){
        cout<<"编号"<<ID<<endl;
        cout<<"名称"<<Name<<endl;
        cout<<"进货价格"<<Purchasingprice<<endl;
        cout<<"出货价格"<<Sellingprice<<endl;
        }
}
```
   
2. 在**类外**定义成员函数

成员函数**在类体内进行声明**，而将成员函数的**定义放在类外**，相比于类内定义成员函数，成员函数名前要多**加上“类名::”**,“::”是作用于运算符，以说明这个函数是属于那类的成员函数，否则编译器就会认为该函数是一个普通函数。

```cpp
返回类型 类名::成员函数名(参数说明){
     函数体
}
```
```cpp
class cgoods{//商品类
     
     private:
        string ID; //商品编号
        string name; //商品名称
        double Purchasingprice; //进货价格
        double Sellingprice; //售出价格
        int SellCount; //售出数量
        static double Profit; //总利润
     
     protected://无
     
     public:
        
        cgoods(string id, string name, double purchasingprice);//构造函数
        
        cgoods(string id, string name, double purchasingprice);//构造函数
        
        ~cgoods();//析构函数

        void SetPurchasingprice(double purchasingprice);//设置进货价格

        void SetSellingprice(double sellingprice);//设置出货价格

        void display();//显示商品信息
}

        cgoods::cgoods(string id, string name){//构造函数
        ID=id;
        Name=name;
        }

        cgoods::cgoods(string id, string name, double purchasingprice){//构造函数
        ID=id;
        Name=name;
        Purchasingprice=purchasingprice;
        }

        void cgoods::SetPurchasingprice(double purchasingprice){//设置进货价格
        Purchasingprice=purchasingprice;
        }

        void cgoods::SetSellingprice(double sellingprice){//设置出货价格
        Sellingprice=sellingprice;
        }

        void cgoods::display(){//显示商品信息
        cout<<"编号"<<ID<<endl;
        cout<<"名称"<<Name<<endl;
        cout<<"进货价格"<<Purchasingprice<<endl;
        cout<<"出货价格"<<Sellingprice<<endl;
        }
```

## 继承性 (Inherit)

派生类既能有自己新定义的属性和行为，又能够继承父类的所有属性和行为，无需重复定义，这种允许和鼓励类的重用的继承设计对于提高软件开发效率有着重要意义！

## 多态性 (Polymorphism)

多态性使得可以通过相同的调用方式来调用这些具有不同功能的同名函数，这样同一个属性在父类和派生类中具有不同的语义！