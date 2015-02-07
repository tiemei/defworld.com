---
title: java语言基础细节
date: '2013-05-18'
description: java语言基础细节
categories: "java"
tags: java

---
2011年的笔记，当时是在看《细说 Java》，算是一个入门书。  
虽然基础，但是平时工作中并不一定注意到这些细节，现在这些零碎的东西都整理起来。  
只挑选目前看还有些价值的  

## java基本数据类型
基本数据类型由低到高排序：

    byte(1) short(2) --->
                         int(4) long(8) float(4) double(8)
            char (2) --->
    boolean(1)

由低到高隐式转换，高到底必须显示申明，因为数据会失真。  
char是无符号类型，所以char short   
boolean不能和其他类型进行相互转换  
必要时使用BigInteger & BigDecimal  

## 值传递与引用传递
两者其实都是值传递

## 私有属性的优势
* 对外暴露get/set，可实现额外的校验或其他逻辑控制
* 隐藏内部细节，对外屏蔽内部变化

## 类加载及对象初始化时动作
1. 在类加载时（第一次使用类是），为类中的静态成员变量分配内存空间，并赋予默认值
2. 执行静态成员变量的初始化操作。前面已经指出，静态成员的初始化有两种：在声明时直接初始化与静态初始化块。两种方式会按照在类中出现的顺序（声明的顺序）来执行。A，B步只会在类加载时执行一次。
3. 如果创建了类的对象，便在堆中为类的实例分配内存空间，实例变量被初始化为默认值
4. 执行实例化变量的初始化操作。实例变量的前两种初始化方式：声明时直接初始化与初始化块，这两种初始化方法会按照在类中出现的顺序（声明的顺序）来执行。
5. 执行类的构造器。C~E在每次创建类的对象时都会执行。

## 重载/重写/隐藏
### 重载
方法的重载就是方法具有相同的名称不同的参数列表，在编译期间确定，编译器可以根据参数的类型与个数加以区分，而方法的返回类型与方法抛出异常等因素的不同不作为重载的标准，因为其不能提供足够的信息。如果参数已引用传递，那么硬系那个调用的因素时引用的类型，而与引用所指向的对象类型无关。  
编译器选择重载方法时顺序时：  
A选择形参类型与制定的实参列表完全一致的方法  
B如果A中的方法不存在，选择形参列表的参数类型能够兼容对应的实参列表的参数类型。  
  如果满足这一条件的方法不止一个，那么如果A方法参数类表在类型上可以兼容对应的B方法参数列表的类型，那么将排除B方法，按照这种方法排除，如果排除以后仍然剩下多于一个的方法，那么编译器会给出错误信息，指出引用类型不明确。  
C如果B中的方法不存在，自动封箱与拆箱操作将会执行，然后按照B继续寻找。  
D如果C中的方法不存在，则选择具有可变形参的方法，如果方法存在，调用该方法，否则给出错误信息，指出方法不存在。  
### 重写
方法重写的条件：  

* 子类与父类的方法必须都是实例方法。如果父类是实例方法而子类是静态方法，或者相反，则编译器会报错。如果子类与父类都是静态方法，那么子类隐藏父类的方法，而不是重写父类的方法。
* 子类与父类的方法需要具有相同的方法名称、参数列表，并且子类的返回类型与父类相同或者是父类的子类。如果方法名称相同而参数列表不同（返回类型可相同也可不相同），那么只是方法的重载。如果方法名称与参数列表相同而返回类型不同（并且子类方法的返回类型不是父类的子类型），那么编译器将会报错。
* 子类方法的访问权限不能小于父类的访问权限。
* 子类方法不能比父类方法抛出更多的已检测异常（也称编译时异常）。
* 父类的方法在子类中必须可见。

### 隐藏
静态方法隐藏的条件:  

* 静态（required)
* 相同的方法名称、参数列表、子类的返回类型和父类相同或者是父类的子类型
* 子类方法的访问权限不能小于父类方法
* 子类方法不恩能够比父类方法抛出更多的已检测异常

成员变量隐藏：  
与静态方法类似，子类不能重写类中的成员变量，只能隐藏父类的成员变量（不论静态变量还是实例变量）。只要求子类与父类的成员变量的名字相同即可，与变量类型无关。  
注意成员变量的继承，如果子类继承了父类的成员变量，则子类与父类共享同一个成员变量。如果是实例变量，在对象内部共享，如果是静态成员变量，在父类与子类之间共享。  

## 变量和方法的搜索顺序
遵循由近及远的原则
### 变量的搜索顺序

* 从访问某变量所在的语句块中（最小语句块）搜索变量的声明
* 访问该语句块的外围语句块，知道整个方法所在的语句块，方法的参数也作为搜索的对象。
* 搜索类中声明的成员变量，如果不存在其声明，搜索从父类型中集成的成员变量。
* 如果该类是一个内部类，那么对外围类返回步骤3进行搜索，知道顶层类为止。
* 搜索明确静态导入的成员（即单一静态导入的成员）
* 搜索使用星号静态导入的成员

### 方法的搜索顺序

* 当前类的类型
* 内部类的类型
* 明确导入的类型
* 与当前类处在同一个包中的其他类型
* 使用带星号导入的类型

## 泛型
使用Object需要在运行时强制转型，而泛型编译期间就能确定类型，运行截断参数类型的信息将被擦除。  
List\<Super\>只能接收List\<Super\>类型的参数，不能接收List\<Sub\>。用List\<?\>/List\<? extends Super\>/List\<? super Number\>  

## 引用类型

* 强引用存在永远不会回收
* soft引用存在只有在内存满是才有可能被回收（看GC算法）
* weak reference 对象与soft引用对象不同点是：后者在GC回收时要通过算法检查是否会后，对前者总是回收，但是复杂关系的weak对象群要好几次GC的运行才会被回收。只要勇于map
* 虚引用一般适用于执行完了finalize函数，并且为不可到达对象，但是还没有被GC回收的对象。辅助finalize进行后期回收工作。

