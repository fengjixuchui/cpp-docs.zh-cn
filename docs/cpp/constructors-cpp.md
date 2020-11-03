---
title: 构造函数 (C++)
ms.date: 12/27/2019
helpviewer_keywords:
- constructors [C++]
- objects [C++], creating
- instance constructors
ms.assetid: 3e9f7211-313a-4a92-9584-337452e061a9
ms.openlocfilehash: b8373eda52967db881d7d58477fcd2a9181ac3ee
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924732"
---
# <a name="constructors-c"></a>构造函数 (C++)

若要自定义类成员的初始化方式，或在创建类的对象时调用函数，请定义 *构造函数* 。 构造函数具有与类相同的名称，没有返回值。 你可以根据需要定义任意多个重载构造函数，以各种方式自定义初始化。 通常，构造函数具有公共可访问性，以便类定义或继承层次结构外的代码可以创建类的对象。 但也可以将构造函数声明为 **`protected`** 或 **`private`** 。

构造函数可以选择采用成员 init list。 这是一种更有效的方法来初始化类成员，而不是在构造函数主体中赋值。 下面的示例演示 `Box` 具有三个重载构造函数的类。 最后两个使用成员 init 列表：

```cpp
class Box {
public:
    // Default constructor
    Box() {}

    // Initialize a Box with equal dimensions (i.e. a cube)
    explicit Box(int i) : m_width(i), m_length(i), m_height(i) // member init list
    {}

    // Initialize a Box with custom dimensions
    Box(int width, int length, int height)
        : m_width(width), m_length(length), m_height(height)
    {}

    int Volume() { return m_width * m_length * m_height; }

private:
    // Will have value of 0 when default constructor is called.
    // If we didn't zero-init here, default constructor would
    // leave them uninitialized with garbage values.
    int m_width{ 0 };
    int m_length{ 0 };
    int m_height{ 0 };
};
```

在声明类的实例时，编译器会根据重载决策的规则选择要调用的构造函数：

```cpp
int main()
{
    Box b; // Calls Box()

    // Using uniform initialization (preferred):
    Box b2 {5}; // Calls Box(int)
    Box b3 {5, 8, 12}; // Calls Box(int, int, int)

    // Using function-style notation:
    Box b4(2, 4, 6); // Calls Box(int, int, int)
}
```

- 构造函数可以声明为 **`inline`** 、 [显式](#explicit_constructors) **`friend`** 或 [constexpr](#constexpr_constructors)。
- 构造函数可以初始化已声明为或的对象 **`const`** **`volatile`** **`const volatile`** 。 **`const`** 构造函数完成后，对象变为。
- 若要在实现文件中定义构造函数，请为它提供一个与任何其他成员函数相同的限定名称： `Box::Box(){...}` 。

## <a name="member-initializer-lists"></a><a name="member_init_list"></a> 成员初始值设定项列表

构造函数可以选择具有成员初始值设定项列表，该列表在执行构造函数正文之前初始化类成员。  (注意，成员初始值设定项列表与 [std：： initializer_list \<T>](../standard-library/initializer-list-class.md)类型的 *初始值设定项列表* 不同。 ) 

首选使用成员初始值设定项列表，而不是在构造函数的主体中赋值，因为它直接初始化成员。 在下面的示例中，成员初始值设定项列表由冒号后面的所有 **标识符 (参数)** 表达式组成：

```cpp
    Box(int width, int length, int height)
        : m_width(width), m_length(length), m_height(height)
    {}
```

标识符必须引用类成员;它是用参数的值初始化的。 参数可以是构造函数参数之一、函数调用或[std：： initializer_list \<T> ](../standard-library/initializer-list-class.md)。

**`const`** 引用类型的成员和成员必须在成员初始值设定项列表中进行初始化。

应在初始值设定项列表中对参数化基类构造函数进行调用，以确保在执行派生的构造函数之前完全初始化基类。

## <a name="default-constructors"></a><a name="default_constructors"></a> 默认构造函数

*默认构造函数* 通常没有参数，但它们可以包含具有默认值的参数。

```cpp
class Box {
public:
    Box() { /*perform any required default initialization steps*/}

    // All params have default values
    Box (int w = 1, int l = 1, int h = 1): m_width(w), m_height(h), m_length(l){}
...
}
```

默认构造函数是一种 [特殊的成员函数](special-member-functions.md)。 如果未在类中声明任何构造函数，则编译器将提供隐式的 **`inline`** 默认构造函数。

```cpp
#include <iostream>
using namespace std;

class Box {
public:
    int Volume() {return m_width * m_height * m_length;}
private:
    int m_width { 0 };
    int m_height { 0 };
    int m_length { 0 };
};

int main() {
    Box box1; // Invoke compiler-generated constructor
    cout << "box1.Volume: " << box1.Volume() << endl; // Outputs 0
}
```

如果依赖于隐式的默认构造函数，请务必在类定义中初始化成员，如前面的示例中所示。 如果没有这些初始值设定项，成员将未初始化，卷 ( # A1 调用将产生一个垃圾值。 通常，使用这种方法初始化成员是一种很好的做法，即使不依赖于隐式默认构造函数也是如此。

可以通过将其定义为 [已删除](#explicitly_defaulted_and_deleted_constructors)，阻止编译器生成隐式默认构造函数：

```cpp
    // Default constructor
    Box() = delete;
```

如果任何类成员不是默认值可构造，则编译器生成的默认构造函数将定义为已删除。 例如，类类型的所有成员及其类类型成员必须具有可访问的默认构造函数和析构函数。 引用类型的所有数据成员以及 **`const`** 成员必须具有默认成员初始值设定项。

调用编译器生成的默认构造函数并尝试使用括号时，将发出警告：

```cpp
class myclass{};
int main(){
myclass mc();     // warning C4930: prototyped function not called (was a variable definition intended?)
}
```

这是“最棘手的解析”问题的示例。 这种示例表达式既可以解释为函数的声明，也可以解释为对默认构造函数的调用，而且 C++ 分析器更偏向于声明，因此表达式会被视为函数声明。 有关详细信息，请参阅 [大多数棘手分析](https://en.wikipedia.org/wiki/Most_vexing_parse)。

如果声明了任何非默认构造函数，编译器不会提供默认构造函数：

```cpp
class Box {
public:
    Box(int width, int length, int height)
        : m_width(width), m_length(length), m_height(height){}
private:
    int m_width;
    int m_length;
    int m_height;

};

int main(){

    Box box1(1, 2, 3);
    Box box2{ 2, 3, 4 };
    Box box3; // C2512: no appropriate default constructor available
}
```

如果类没有默认构造函数，将无法通过单独使用方括号语法来构造该类的对象数组。 例如，在前面提到的代码块中，框的数组无法进行如下声明：

```cpp
Box boxes[3]; // C2512: no appropriate default constructor available
```

但是，可以使用一组初始值设定项列表来初始化 Box 对象的数组：

```cpp
Box boxes[3]{ { 1, 2, 3 }, { 4, 5, 6 }, { 7, 8, 9 } };
```

有关详细信息，请参阅 [初始值设定项](initializers.md)。

## <a name="copy-constructors"></a><a name="copy_and_move_constructors"></a> 复制构造函数

*复制构造函数* 通过从同一类型的对象复制成员值来初始化对象。 如果类成员是所有简单类型（如标量值），则编译器生成的复制构造函数便已足够，无需自行定义。 如果你的类需要更复杂的初始化，则需要实现自定义复制构造函数。 例如，如果类成员是一个指针，则需要定义一个复制构造函数以分配新内存并从另一个指向对象复制值。 编译器生成的复制构造函数只复制指针，使新指针仍指向另一个内存位置。

复制构造函数可能具有以下其中一个签名：

```cpp
    Box(Box& other); // Avoid if possible--allows modification of other.
    Box(const Box& other);
    Box(volatile Box& other);
    Box(volatile const Box& other);

    // Additional parameters OK if they have default values
    Box(Box& other, int i = 42, string label = "Box");
```

定义复制构造函数时，还应将复制赋值运算符定义 (=) 。 有关详细信息，请参阅 [分配](assignment.md) 和 [复制构造函数和复制赋值运算符](copy-constructors-and-copy-assignment-operators-cpp.md)。

可以通过将复制构造函数定义为已删除，阻止复制对象：

```cpp
    Box (const Box& other) = delete;
```

尝试复制对象会生成错误 *C2280：正在尝试引用已删除的函数* 。

## <a name="move-constructors"></a><a name="move_constructors"></a> 移动构造函数

*移动构造函数* 是一种特殊的成员函数，它将现有对象的数据的所有权转移到新的变量，而不复制原始数据。 它采用右值引用作为其第一个参数，任何其他参数都必须具有默认值。 移动构造函数可在传递大型对象时显著提高程序的效率。

```cpp
Box(Box&& other);
```

在某些情况下，编译器将在以下情况下选择移动构造函数：对象正由与要销毁的同一类型的另一个对象进行初始化，不再需要其资源。 下面的示例演示了通过重载决策选择移动构造函数时的一种情况。 在调用的构造函数中 `get_Box()` ，返回值是 *Xvalue* (过期值) 。 它未分配给任何变量，因此即将超出范围。 若要为此示例提供动机，我们将提供一个表示其内容的大型字符串矢量。 移动构造函数将其从 "过期" 值 "box" 中删除，以便向量现在属于新对象，而不是复制向量及其字符串。 对的调用 `std::move` 是所需的全部，因为 `vector` 和 `string` 类实现其自己的移动构造函数。

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

class Box {
public:
    Box() { std::cout << "default" << std::endl; }
    Box(int width, int height, int length)
       : m_width(width), m_height(height), m_length(length)
    {
        std::cout << "int,int,int" << std::endl;
    }
    Box(Box& other)
       : m_width(other.m_width), m_height(other.m_height), m_length(other.m_length)
    {
        std::cout << "copy" << std::endl;
    }
    Box(Box&& other) : m_width(other.m_width), m_height(other.m_height), m_length(other.m_length)
    {
        m_contents = std::move(other.m_contents);
        std::cout << "move" << std::endl;
    }
    int Volume() { return m_width * m_height * m_length; }
    void Add_Item(string item) { m_contents.push_back(item); }
    void Print_Contents()
    {
        for (const auto& item : m_contents)
        {
            cout << item << " ";
        }
    }
private:
    int m_width{ 0 };
    int m_height{ 0 };
    int m_length{ 0 };
    vector<string> m_contents;
};

Box get_Box()
{
    Box b(5, 10, 18); // "int,int,int"
    b.Add_Item("Toupee");
    b.Add_Item("Megaphone");
    b.Add_Item("Suit");

    return b;
}

int main()
{
    Box b; // "default"
    Box b1(b); // "copy"
    Box b2(get_Box()); // "move"
    cout << "b2 contents: ";
    b2.Print_Contents(); // Prove that we have all the values

    char ch;
    cin >> ch; // keep window open
    return 0;
}
```

如果类未定义移动构造函数，则编译器将生成隐式构造函数（如果没有用户声明的复制构造函数、复制赋值运算符、移动赋值运算符或析构函数）。 如果未定义显式或隐式移动构造函数，则使用移动构造函数的操作将改为使用复制构造函数。 如果类声明了移动构造函数或移动赋值运算符，则隐式声明的复制构造函数将定义为已删除。

如果任何属于类类型的成员缺少析构函数，或者编译器无法确定要用于移动操作的构造函数，则将隐式声明的移动构造函数定义为已删除。

有关如何编写非普通移动构造函数的详细信息，请参阅 [移动构造函数和移动赋值运算符 (c + +) ](../cpp/move-constructors-and-move-assignment-operators-cpp.md)。

## <a name="explicitly-defaulted-and-deleted-constructors"></a><a name="explicitly_defaulted_and_deleted_constructors"></a> 显式默认的和删除的构造函数

可以显式 *默认* 复制构造函数、默认构造函数、移动构造函数、复制赋值运算符、移动赋值运算符和析构函数。 您可以显式 *删除* 所有特殊成员函数。

```cpp
class Box
{
public:
    Box2() = delete;
    Box2(const Box2& other) = default;
    Box2& operator=(const Box2& other) = default;
    Box2(Box2&& other) = default;
    Box2& operator=(Box2&& other) = default;
    //...
};
```

有关详细信息，请参阅 [显式默认和已删除的函数](../cpp/explicitly-defaulted-and-deleted-functions.md)。

## <a name="constexpr-constructors"></a><a name="constexpr_constructors"></a> constexpr 构造函数

构造函数可以声明为 [constexpr](constexpr-cpp.md) （如果

- 它既可以声明为默认值，也可以满足所有的 [constexpr 函数](constexpr-cpp.md#constexpr_functions) 条件;
- 类没有虚拟基类;
- 每个参数都是 [文本类型](trivial-standard-layout-and-pod-types.md#literal_types);
- 主体不是函数 try 块;
- 所有非静态数据成员和基类子对象均已初始化;
- 如果类 () 具有 variant 成员的联合，或 (b) 具有匿名联合，则只初始化其中一个联合成员;
- 类类型的所有非静态数据成员和所有基类子对象都具有 constexpr 构造函数

## <a name="initializer-list-constructors"></a><a name="init_list_constructors"></a> 初始值设定项列表构造函数

如果构造函数采用[std：： initializer_list \<T\> ](../standard-library/initializer-list-class.md)作为其参数，并且任何其他参数都具有默认参数，则当通过直接初始化对类进行实例化时，将在重载决策中选择该构造函数。 您可以使用 initializer_list 来初始化任何可接受它的成员。 例如，假设之前显示的 Box 类 () 具有 `std::vector<string>` 成员 `m_contents` 。 可以提供如下所示的构造函数：

```cpp
    Box(initializer_list<string> list, int w = 0, int h = 0, int l = 0)
        : m_contents(list), m_width(w), m_height(h), m_length(l)
{}
```

然后创建 Box 对象，如下所示：

```cpp
    Box b{ "apples", "oranges", "pears" }; // or ...
    Box b2(initializer_list<string> { "bread", "cheese", "wine" }, 2, 4, 6);
```

## <a name="explicit-constructors"></a><a name="explicit_constructors"></a> 显式构造函数

如果类具有带一个参数的构造函数，或是如果除了一个参数之外的所有参数都具有默认值，则参数类型可以隐式转换为类类型。 例如，如果 `Box` 类具有一个类似于下面这样的构造函数：

```cpp
Box(int size): m_width(size), m_length(size), m_height(size){}
```

可以初始化 Box，如下所示：

```cpp
Box b = 42;
```

或将一个 int 传递给采用 Box 的函数：

```cpp
class ShippingOrder
{
public:
    ShippingOrder(Box b, double postage) : m_box(b), m_postage(postage){}

private:
    Box m_box;
    double m_postage;
}
//elsewhere...
    ShippingOrder so(42, 10.8);
```

这类转换可能在某些情况下很有用，但更常见的是，它们可能会导致代码中发生细微但严重的错误。 作为一般规则，应 **`explicit`** 在构造函数 (和用户定义的运算符) 上使用关键字，以防止这种隐式类型转换：

```cpp
explicit Box(int size): m_width(size), m_length(size), m_height(size){}
```

构造函数是显式函数时，此行会导致编译器错误：`ShippingOrder so(42, 10.8);`。  有关详细信息，请参阅 [用户定义的类型转换](../cpp/user-defined-type-conversions-cpp.md)。

## <a name="order-of-construction"></a><a name="order_of_construction"></a> 构造顺序

构造函数按此顺序执行工作：

1. 按声明顺序调用基类和成员构造函数。

1. 如果类派生自虚拟基类，则会将对象的虚拟基指针初始化。

1. 如果类具有或继承了虚函数，则会将对象的虚函数指针初始化。 虚函数指针指向类中的虚函数表，确保虚函数正确地调用绑定代码。

1. 它执行自己函数体中的所有代码。

下面的示例显示，在派生类的构造函数中，基类和成员构造函数的调用顺序。 首先，调用基构造函数，然后按照基类成员在类声明中出现的顺序对这些成员进行初始化，然后，调用派生构造函数。

```cpp
#include <iostream>

using namespace std;

class Contained1 {
public:
    Contained1() { cout << "Contained1 ctor\n"; }
};

class Contained2 {
public:
    Contained2() { cout << "Contained2 ctor\n"; }
};

class Contained3 {
public:
    Contained3() { cout << "Contained3 ctor\n"; }
};

class BaseContainer {
public:
    BaseContainer() { cout << "BaseContainer ctor\n"; }
private:
    Contained1 c1;
    Contained2 c2;
};

class DerivedContainer : public BaseContainer {
public:
    DerivedContainer() : BaseContainer() { cout << "DerivedContainer ctor\n"; }
private:
    Contained3 c3;
};

int main() {
    DerivedContainer dc;
}
```

输出如下：

```Output
Contained1 ctor
Contained2 ctor
BaseContainer ctor
Contained3 ctor
DerivedContainer ctor
```

派生类构造函数始终调用基类构造函数，因此，在完成任何额外任务之前，它可以依赖于完全构造的基类。 按派生顺序调用基类构造函数，例如，如果派生自，派生自，则 `ClassA` `ClassB` `ClassC` `ClassC` 首先调用构造函数，然后调用构造函数 `ClassB` ，然后调用 `ClassA` 构造函数。

如果基类没有默认构造函数，则必须在派生类构造函数中提供基类构造函数参数：

```cpp
class Box {
public:
    Box(int width, int length, int height){
       m_width = width;
       m_length = length;
       m_height = height;
    }

private:
    int m_width;
    int m_length;
    int m_height;
};

class StorageBox : public Box {
public:
    StorageBox(int width, int length, int height, const string label&) : Box(width, length, height){
        m_label = label;
    }
private:
    string m_label;
};

int main(){

    const string aLabel = "aLabel";
    StorageBox sb(1, 2, 3, aLabel);
}
```

如果构造函数引发异常，析构的顺序与构造的顺序相反：

1. 构造函数主体中的代码将展开。

1. 基类和成员对象将被销毁，顺序与声明顺序相反。

1. 如果是非委托构造函数，所有完全构造的基类对象和成员均将被销毁。 但是，对象本身不是完全构造的，因此析构函数不会运行。

## <a name="derived-constructors-and-extended-aggregate-initialization"></a><a name="extended_aggregate"></a> 派生的构造函数和扩展的聚合初始化

如果基类的构造函数是非公共的，但派生类可访问，则在 Visual Studio 2017 和更高版本中的 **/std： c + + 17** 模式下，不能使用空大括号初始化派生类型的对象。

以下示例演示 C++14 一致行为：

```cpp
struct Derived;

struct Base {
    friend struct Derived;
private:
    Base() {}
};

struct Derived : Base {};

Derived d1; // OK. No aggregate init involved.
Derived d2 {}; // OK in C++14: Calls Derived::Derived()
               // which can call Base ctor.
```

在 C++17，`Derived` 现被视作聚合类型。 这意味着 `Base` 通过私有默认构造函数进行的初始化将作为扩展的聚合初始化规则的一部分而直接发生。 以前，`Base` 私有构造函数通过 `Derived` 构造函数调用，它之所以能够成功是因为友元声明。

下面的示例演示 Visual Studio 2017 和更高版本的 **/std： c + + 17** 模式下的 c + + 17 行为：

```cpp
struct Derived;

struct Base {
    friend struct Derived;
private:
    Base() {}
};

struct Derived : Base {
    Derived() {} // add user-defined constructor
                 // to call with {} initialization
};

Derived d1; // OK. No aggregate init involved.

Derived d2 {}; // error C2248: 'Base::Base': cannot access
               // private member declared in class 'Base'
```

### <a name="constructors-for-classes-that-have-multiple-inheritance"></a>具有多重继承的类的构造函数

如果类从多个基类派生，那么将按照派生类声明中列出的顺序调用基类构造函数：

```cpp
#include <iostream>
using namespace std;

class BaseClass1 {
public:
    BaseClass1() { cout << "BaseClass1 ctor\n"; }
};
class BaseClass2 {
public:
    BaseClass2() { cout << "BaseClass2 ctor\n"; }
};
class BaseClass3 {
public:
    BaseClass3() { cout << "BaseClass3 ctor\n"; }
};
class DerivedClass : public BaseClass1,
                     public BaseClass2,
                     public BaseClass3
                     {
public:
    DerivedClass() { cout << "DerivedClass ctor\n"; }
};

int main() {
    DerivedClass dc;
}
```

你应看到以下输出：

```Output
BaseClass1 ctor
BaseClass2 ctor
BaseClass3 ctor
DerivedClass ctor
```

## <a name="delegating-constructors"></a><a name="delegating_constructors"></a> 委托构造函数

*委托构造函数* 调用同一类中的不同构造函数来执行某些初始化工作。 如果有多个构造函数需要执行类似的工作，则这非常有用。 可以在一个构造函数中编写主逻辑，并从其他构造函数调用它。 在下面的简单示例中，Box (int) 将其工作委托给 Box (int，int，int) ：

```cpp
class Box {
public:
    // Default constructor
    Box() {}

    // Initialize a Box with equal dimensions (i.e. a cube)
    Box(int i) :  Box(i, i, i);  // delegating constructor
    {}

    // Initialize a Box with custom dimensions
    Box(int width, int length, int height)
        : m_width(width), m_length(length), m_height(height)
    {}
    //... rest of class as before
};
```

所有构造函数完成后，完全初始化的构造函数将立即创建对象。 有关详细信息，请参阅 [委托构造函数](../cpp/delegating-constructors.md)。

## <a name="inheriting-constructors-c11"></a><a name="inheriting_constructors"></a> (c + + 11) 继承构造函数

派生类可以通过使用声明从直接基类继承构造函数 **`using`** ，如以下示例中所示：

```cpp
#include <iostream>
using namespace std;

class Base
{
public:
    Base() { cout << "Base()" << endl; }
    Base(const Base& other) { cout << "Base(Base&)" << endl; }
    explicit Base(int i) : num(i) { cout << "Base(int)" << endl; }
    explicit Base(char c) : letter(c) { cout << "Base(char)" << endl; }

private:
    int num;
    char letter;
};

class Derived : Base
{
public:
    // Inherit all constructors from Base
    using Base::Base;

private:
    // Can't initialize newMember from Base constructors.
    int newMember{ 0 };
};

int main()
{
    cout << "Derived d1(5) calls: ";
    Derived d1(5);
    cout << "Derived d1('c') calls: ";
    Derived d2('c');
    cout << "Derived d3 = d2 calls: " ;
    Derived d3 = d2;
    cout << "Derived d4 calls: ";
    Derived d4;
}

/* Output:
Derived d1(5) calls: Base(int)
Derived d1('c') calls: Base(char)
Derived d3 = d2 calls: Base(Base&)
Derived d4 calls: Base()*/
```

::: moniker range=">=msvc-150"

**Visual Studio 2017 及更高版本** ： **`using`** **/std： c + + 17** 模式中的语句使基类中的所有构造函数的作用域除外，但对派生类中的构造函数具有完全相同的签名。 一般而言，当派生类未声明新数据成员或构造函数时，最好使用继承构造函数。

::: moniker-end

如果类型指定基类，则类模板可以从类型参数继承所有构造函数：

```cpp
template< typename T >
class Derived : T {
    using T::T;   // declare the constructors from T
    // ...
};
```

如果基类的构造函数具有相同签名，则派生类无法从多个基类继承。

## <a name="constructors-and-composite-classes"></a><a name="constructors_in_composite_classes"></a> 构造函数和复合类

包含类类型成员的类称为 *复合类* 。 创建复合类的类类型成员时，调用类自己的构造函数之前，先调用构造函数。 当包含的类没有默认构造函数是，必须使用复合类构造函数中的初始化列表。 在之前的 `StorageBox` 示例中，如果将 `m_label` 成员变量的类型更改为新的 `Label` 类，则必须调用基类构造函数，并且将 `m_label` 变量（位于 `StorageBox` 构造函数中）初始化：

```cpp
class Label {
public:
    Label(const string& name, const string& address) { m_name = name; m_address = address; }
    string m_name;
    string m_address;
};

class StorageBox : public Box {
public:
    StorageBox(int width, int length, int height, Label label)
        : Box(width, length, height), m_label(label){}
private:
    Label m_label;
};

int main(){
// passing a named Label
    Label label1{ "some_name", "some_address" };
    StorageBox sb1(1, 2, 3, label1);

    // passing a temporary label
    StorageBox sb2(3, 4, 5, Label{ "another name", "another address" });

    // passing a temporary label as an initializer list
    StorageBox sb3(1, 2, 3, {"myname", "myaddress"});
}
```

## <a name="in-this-section"></a>在本节中

- [复制构造函数和复制赋值运算符](copy-constructors-and-copy-assignment-operators-cpp.md)
- [移动构造函数和移动赋值运算符](move-constructors-and-move-assignment-operators-cpp.md)
- [委托构造函数](delegating-constructors.md)

## <a name="see-also"></a>请参阅

[类和结构](classes-and-structs-cpp.md)
