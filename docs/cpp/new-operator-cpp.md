---
title: new 运算符 (C++)
ms.date: 11/04/2016
helpviewer_keywords:
- new keyword [C++]
ms.assetid: 69fee812-1c28-4882-8fda-d1ad17860004
ms.openlocfilehash: 0c3ed59262ec5ad061572d23c1166b2153cb16ca
ms.sourcegitcommit: 43cee7a0d41a062661229043c2f7cbc6ace17fa3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92008578"
---
# <a name="new-operator-c"></a>new 运算符 (C++)

从免费存储为 *类型* 为的对象或对象数组分配内存，并将适当类型的非零指针返回到该对象。

> [!NOTE]
> Microsoft c + + 组件扩展提供对 **`new`** 关键字的支持，以添加 vtable 槽条目。 有关详细信息，请参阅 [vtable 中的 new (新槽) ](../extensions/new-new-slot-in-vtable-cpp-component-extensions.md)

## <a name="syntax"></a>语法

```
[::] new [placement] new-type-name [new-initializer]
[::] new [placement] ( type-name ) [new-initializer]
```

## <a name="remarks"></a>备注

如果不成功，则 **`new`** 返回零或引发异常; 有关详细信息，请参阅 [new 和 delete 运算符](../cpp/new-and-delete-operators.md) 。 可以通过以下方式更改此默认行为：编写自定义异常处理例程，并使用您的函数名称作为其参数调用 [_set_new_handler](../c-runtime-library/reference/set-new-handler.md) 运行时库函数。

有关如何在托管堆上创建对象的信息，请参阅 [gcnew](../extensions/ref-new-gcnew-cpp-component-extensions.md)。

当 **`new`** 用于为 c + + 类对象分配内存时，将在分配内存后调用对象的构造函数。

使用 [delete](../cpp/delete-operator-cpp.md) 运算符释放用运算符分配的内存 **`new`** 。

以下示例先分配然后释放一个二维字符数组，数组的大小为 `dim` x 10。 在分配多维数组时，除第一个维度之外的所有维度必须是计算结果为正值的常量表达式；最左侧的数组维度可以是计算结果为正值的任何表达式。 当使用运算符分配数组时 **`new`** ，第一个维度可为零，运算符将 **`new`** 返回唯一的指针。

```cpp
char (*pchar)[10] = new char[dim][10];
delete [] pchar;
```

*类型名*不能包含 **`const`** 、 **`volatile`** 、类声明或枚举声明。 因此，以下表达式是非法的：

```cpp
volatile char *vch = new volatile char[20];
```

**`new`** 运算符不分配引用类型，因为它们不是对象。

**`new`** 运算符不能用于分配函数，但它可用于分配指向函数的指针。 下面的示例为返回整数的函数分配然后释放一个包含 7 个指针的数组。

```cpp
int (**p) () = new (int (*[7]) ());
delete *p;
```

如果使用 **`new`** 不带任何额外参数的运算符，并使用 [/Gx](../build/reference/gx-enable-exception-handling.md)、 [/eha](../build/reference/eh-exception-handling-model.md)或 [/ehs](../build/reference/eh-exception-handling-model.md) 选项进行编译，则编译器将在 **`delete`** 构造函数引发异常时生成代码来调用运算符。

以下列表描述了的语法元素 **`new`** ：

*虚拟*<br/>
提供了一种方法，以便在重载时传递其他参数 **`new`** 。

*类型名称*<br/>
指定要分配的类型；它可以是内置类型，也可以是用户定义的类型。 如果类型规范非常复杂，则可用括号将其括起来以强制实施绑定顺序。

*initializer*<br/>
为初始化对象提供值。 不能为数组指定初始值设定项。 **`new`** 仅当类具有默认构造函数时，运算符才会创建对象的数组。

## <a name="example-allocate-and-free-a-character-array"></a>示例：分配并释放字符数组

下面的代码示例分配类 `CName` 的一个字符数组和一个对象，然后释放它们。

```cpp
// expre_new_Operator.cpp
// compile with: /EHsc
#include <string.h>

class CName {
public:
   enum {
      sizeOfBuffer = 256
   };

   char m_szFirst[sizeOfBuffer];
   char m_szLast[sizeOfBuffer];

public:
   void SetName(char* pszFirst, char* pszLast) {
     strcpy_s(m_szFirst, sizeOfBuffer, pszFirst);
     strcpy_s(m_szLast, sizeOfBuffer, pszLast);
   }

};

int main() {
   // Allocate memory for the array
   char* pCharArray = new char[CName::sizeOfBuffer];
   strcpy_s(pCharArray, CName::sizeOfBuffer, "Array of characters");

   // Deallocate memory for the array
   delete [] pCharArray;
   pCharArray = NULL;

   // Allocate memory for the object
   CName* pName = new CName;
   pName->SetName("Firstname", "Lastname");

   // Deallocate memory for the object
   delete pName;
   pName = NULL;
}
```

## <a name="example-new-operator"></a>示例： `new` 运算符

如果使用运算符的放置新形式，则 **`new`** 除了分配的大小外，具有参数的窗体外， **`delete`** 如果构造函数引发异常，则编译器不支持运算符的放置形式。 例如：

```cpp
// expre_new_Operator2.cpp
// C2660 expected
class A {
public:
   A(int) { throw "Fail!"; }
};
void F(void) {
   try {
      // heap memory pointed to by pa1 will be deallocated
      // by calling ::operator delete(void*).
      A* pa1 = new A(10);
   } catch (...) {
   }
   try {
      // This will call ::operator new(size_t, char*, int).
      // When A::A(int) does a throw, we should call
      // ::operator delete(void*, char*, int) to deallocate
      // the memory pointed to by pa2.  Since
      // ::operator delete(void*, char*, int) has not been implemented,
      // memory will be leaked when the deallocation cannot occur.

      A* pa2 = new(__FILE__, __LINE__) A(20);
   } catch (...) {
   }
}

int main() {
   A a;
}
```

## <a name="initializing-object-allocated-with-new"></a>初始化使用 new 运算符分配的对象

运算符的语法中包含一个可选的 " *初始值设定项* " 字段 **`new`** 。 这样就可以使用用户定义的构造函数来初始化新对象。 有关初始化如何完成的详细信息，请参阅 [初始值设定项](../cpp/initializers.md)。 下面的示例演示如何将初始化表达式与运算符一起使用 **`new`** ：

```cpp
// expre_Initializing_Objects_Allocated_with_new.cpp
class Acct
{
public:
    // Define default constructor and a constructor that accepts
    //  an initial balance.
    Acct() { balance = 0.0; }
    Acct( double init_balance ) { balance = init_balance; }
private:
    double balance;
};

int main()
{
    Acct *CheckingAcct = new Acct;
    Acct *SavingsAcct = new Acct ( 34.98 );
    double *HowMuch = new double ( 43.0 );
    // ...
}
```

在此示例中， `CheckingAcct` 使用运算符分配对象 **`new`** ，但未指定任何默认初始化。 因此，调用了类的默认构造函数 `Acct()`。 然后，以相同的方式分配了对象 `SavingsAcct`，只不过将它显式初始化为 34.98。 由于34.98 的类型为 **`double`** ，因此将调用采用该类型的参数的构造函数来处理初始化。 最后，将非类类型 `HowMuch` 初始化为 43.0。

如果对象属于类类型，并且该类具有如下所示的构造函数 (如前面的示例所示) ，则 **`new`** 仅当满足以下条件之一时，运算符才能初始化对象：

- 初始值设定项中提供的自变量与构造函数的自变量一致。

- 该类有一个默认构造函数（可在没有参数的情况下调用的构造函数）。

使用运算符分配数组时，不能进行显式的每个元素初始化 **`new`** ; 仅调用默认构造函数（如果存在）。 有关详细信息，请参阅 [默认参数](../cpp/default-arguments.md) 。

如果内存分配失败 (**运算符 new** 返回值 0) ，则不执行初始化。 这可防止尝试初始化不存在的数据。

与函数调用一样，未定义初始化表达式的计算顺序。 此外，您不应指望这些表达式能在执行内存分配前完全计算。 如果内存分配失败且 **`new`** 运算符返回零，则初始值设定项中的某些表达式可能无法完全计算。

## <a name="lifetime-of-objects-allocated-with-new"></a>使用 new 运算符分配的对象的生存期

用运算符分配的对象在 **`new`** 其定义的作用域被退出时不会被销毁。 由于 **`new`** 运算符返回指向它所分配的对象的指针，因此程序必须定义一个具有适当范围的指针来访问这些对象。 例如：

```cpp
// expre_Lifetime_of_Objects_Allocated_with_new.cpp
// C2541 expected
int main()
{
    // Use new operator to allocate an array of 20 characters.
    char *AnArray = new char[20];

    for( int i = 0; i < 20; ++i )
    {
        // On the first iteration of the loop, allocate
        //  another array of 20 characters.
        if( i == 0 )
        {
            char *AnotherArray = new char[20];
        }
    }

    delete [] AnotherArray; // Error: pointer out of scope.
    delete [] AnArray;      // OK: pointer still in scope.
}
```

在上面的示例中，指针 `AnotherArray` 一旦超出范围，将无法再删除对象。

## <a name="how-new-works"></a>new 的工作方式

*分配表达式*（包含运算符的表达式 **`new`** ）执行三个操作：

- 定位并保留要分配的对象的存储。 此阶段完成后，将分配正确的存储量，但它还不是对象。

- 初始化对象。 初始化完成后，将为成为对象的已分配存储显示足够的信息。

- 返回一个指针，该指针指向从 *新类型名称* 或 *类型名称*派生的指针类型的对象 () 。 程序使用此指针来访问最近分配的对象。

**`new`** 运算符调用函数**new**。 对于任何类型的数组和不属于 **`class`** 、或类型的对象， **`struct`** **`union`** 将调用全局函数 **：： operator new**来分配存储。 类类型的对象可以基于每个类定义自己的 **运算符 new** 静态成员函数。

当编译器遇到 **`new`** 用于分配类型**类型的**对象的运算符时，将发出对 `type` **：： operator new ( sizeof (** 的调用 `type` **) ) ** ，或者，如果未定义用户定义的**运算符 new** ，则 **：： operator new ( sizeof (** `type` ** **) ) 。 因此， **`new`** 操作员可以为对象分配正确的内存量。

> [!NOTE]
> **Operator new**的参数的类型为 `size_t` 。 此类型在、、、、、、、和中定义 \<direct.h> \<malloc.h> \<memory.h> \<search.h> \<stddef.h> \<stdio.h> \<stdlib.h> \<string.h> \<time.h> 。

语法中的选项允许 *放置* 规范 (参阅 [new Operator](../cpp/new-operator-cpp.md)) 的语法。 *位置*参数仅可用于**运算符 new**的用户定义实现;它允许将额外信息传递给**new 运算符**。 *placement* `T *TObject = new ( 0x0040 ) T;` `T *TObject = T::operator new( sizeof( T ), 0x0040 );` 如果类 T 具有成员运算符 new，则具有位置字段（如）的表达式转换为，否则转换为 `T *TObject = ::operator new( sizeof( T ), 0x0040 );` 。

*放置*字段的原始目的是允许在用户指定的地址分配硬件相关的对象。

> [!NOTE]
> 虽然上面的示例仅在 " *位置* " 字段中显示一个参数，但并不限制可通过这种方式将多少个额外参数传递给 **运算符** 。

即使为类类型定义了 **new 运算符** ，也可以使用此示例的形式来使用全局运算符：

```cpp
T *TObject =::new TObject;
```

范围解析运算符 (`::`) 强制使用全局 **`new`** 运算符。

## <a name="see-also"></a>请参阅

[使用一元运算符的表达式](../cpp/expressions-with-unary-operators.md)<br/>
[关键字](../cpp/keywords-cpp.md)<br/>
[new 和 delete 运算符](../cpp/new-and-delete-operators.md)
