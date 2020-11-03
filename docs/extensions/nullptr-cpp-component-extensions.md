---
title: nullptr（C++/CLI 和 C++/CX）
ms.date: 10/12/2018
ms.topic: reference
helpviewer_keywords:
- __nullptr keyword (C++)
- nullptr keyword [C++]
ms.assetid: 594cfbf7-06cb-4366-9ede-c0b703e1d095
ms.openlocfilehash: 7e9cf88fdc0444f736f1cfac0d06dfc675a162cc
ms.sourcegitcommit: 43cee7a0d41a062661229043c2f7cbc6ace17fa3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92008356"
---
# <a name="nullptr--ccli-and-ccx"></a>nullptr（C++/CLI 和 C++/CX）

**`nullptr`** 关键字表示*null 指针值*。 空指针值可用于指明对象句柄、内部指针或本机指针类型不指向对象。

使用 **`nullptr`** 托管代码或本机代码。 编译器为托管空指针值和本机空指针值发出相应但不同的指令。 若要了解如何使用此关键字的 ISO 标准 C++ 版本，请参阅 [nullptr](../cpp/nullptr.md)。

**__Nullptr**关键字是 Microsoft 特定的关键字，其含义与相同 **`nullptr`** ，但仅适用于本机代码。 如果你使用 **`nullptr`** with 本机 C/c + + 代码，然后使用 [/clr](../build/reference/clr-common-language-runtime-compilation.md) 编译器选项进行编译，则编译器无法确定是否 **`nullptr`** 指示本机或托管 null 指针值。 为了使你的意图对编译器清楚，请使用 **`nullptr`** 指定托管值或 **__nullptr** 来指定本机值。

**`nullptr`** 关键字与 c # 中**的**Visual Basic 和**null**都等效。

## <a name="usage"></a>使用情况

可以在 **`nullptr`** 可以使用句柄、本机指针或函数参数的任何位置使用关键字。

**`nullptr`** 关键字不是类型，并且不支持与一起使用：

- [sizeof](../cpp/sizeof-operator.md)

- [typeid](../cpp/typeid-operator.md)

- `throw nullptr`（尽管 `throw (Object^)nullptr;` 可以正常发挥作用）

**`nullptr`** 关键字可用于以下指针类型的初始化：

- 本机指针

- Windows 运行时句柄

- 托管句柄

- 托管内部指针

**`nullptr`** 关键字可用于在使用引用之前测试指针或句柄引用是否为 null。

应正确解释使用空指针值进行错误检查的语言之间的函数调用。

不能将句柄初始化为零;只能 **`nullptr`** 使用。 将常数 0 赋给对象句柄会生成装箱的 `Int32`，并强制转换为 `Object^`。

## <a name="example-nullptr-keyword"></a>示例： `nullptr` 关键字

下面的代码示例演示可以在 **`nullptr`** 可以使用句柄、本机指针或函数参数的任何位置使用关键字。 该示例说明 **`nullptr`** 关键字可用于在使用之前检查引用。

```cpp
// mcpp_nullptr.cpp
// compile with: /clr
value class V {};
ref class G {};
void f(System::Object ^) {}

int main() {
// Native pointer.
   int *pN = nullptr;
// Managed handle.
   G ^pG = nullptr;
   V ^pV1 = nullptr;
// Managed interior pointer.
   interior_ptr<V> pV2 = nullptr;
// Reference checking before using a pointer.
   if (pN == nullptr) {}
   if (pG == nullptr) {}
   if (pV1 == nullptr) {}
   if (pV2 == nullptr) {}
// nullptr can be used as a function argument.
   f(nullptr);   // calls f(System::Object ^)
}
```

## <a name="example-use-nullptr-and-zero-interchangeably"></a>示例：使用 `nullptr` 和零交换

下面的代码示例演示了 **`nullptr`** 如何在本机指针上交替使用和零。

```cpp
// mcpp_nullptr_1.cpp
// compile with: /clr
class MyClass {
public:
   int i;
};

int main() {
   MyClass * pMyClass = nullptr;
   if ( pMyClass == nullptr)
      System::Console::WriteLine("pMyClass == nullptr");

   if ( pMyClass == 0)
      System::Console::WriteLine("pMyClass == 0");

   pMyClass = 0;
   if ( pMyClass == nullptr)
      System::Console::WriteLine("pMyClass == nullptr");

   if ( pMyClass == 0)
      System::Console::WriteLine("pMyClass == 0");
}
```

```Output
pMyClass == nullptr

pMyClass == 0

pMyClass == nullptr

pMyClass == 0
```

## <a name="example-interpret-nullptr-as-a-handle"></a>示例：解释 `nullptr` 为句柄

下面的代码示例演示 **`nullptr`** 如何将解释为任何类型的句柄或指向任何类型的本机指针。 如果函数重载了多个指向不同类型的句柄，便会生成多义性错误。 **`nullptr`** 必须显式强制转换为类型。

```cpp
// mcpp_nullptr_2.cpp
// compile with: /clr /LD
void f(int *){}
void f(int ^){}

void f_null() {
   f(nullptr);   // C2668
   // try one of the following lines instead
   f((int *) nullptr);
   f((int ^) nullptr);
}
```

## <a name="example-cast-nullptr"></a>示例： Cast `nullptr`

下面的代码示例演示允许强制转换 **`nullptr`** ，并将指针或句柄返回到包含值的强制转换类型 **`nullptr`** 。

```cpp
// mcpp_nullptr_3.cpp
// compile with: /clr /LD
using namespace System;
template <typename T>
void f(T) {}   // C2036 cannot deduce template type because nullptr can be any type

int main() {
   f((Object ^) nullptr);   // T = Object^, call f(Object ^)

   // Delete the following line to resolve.
   f(nullptr);

   f(0);   // T = int, call f(int)
}
```

## <a name="example-pass-nullptr-as-a-function-parameter"></a>示例： `nullptr` 作为函数参数传递

下面的代码示例演示 **`nullptr`** 可用作函数参数的。

```cpp
// mcpp_nullptr_4.cpp
// compile with: /clr
using namespace System;
void f(Object ^ x) {
   Console::WriteLine("test");
}

int main() {
   f(nullptr);
}
```

```Output
test
```

## <a name="example-default-initialization"></a>示例：默认初始化

下面的代码示例演示在声明句柄且未显式初始化时，它们默认初始化为 **`nullptr`** 。

```cpp
// mcpp_nullptr_5.cpp
// compile with: /clr
using namespace System;
ref class MyClass {
public:
   void Test() {
      MyClass ^pMyClass;   // gc type
      if (pMyClass == nullptr)
         Console::WriteLine("NULL");
   }
};

int main() {
   MyClass ^ x = gcnew MyClass();
   x -> Test();
}
```

```Output
NULL
```

## <a name="example-assign-nullptr-to-a-native-pointer"></a>示例：分配 `nullptr` 给本机指针

下面的代码示例演示了在 **`nullptr`** 使用进行编译时，可以分配给本机指针 `/clr` 。

```cpp
// mcpp_nullptr_6.cpp
// compile with: /clr
int main() {
   int * i = 0;
   int * j = nullptr;
}
```

## <a name="requirements"></a>要求

编译器选项： (不是必需的;受所有代码生成选项支持，包括 `/ZW` 和 `/clr`) 

## <a name="see-also"></a>请参阅

[适用于 .NET 和 UWP 的组件扩展](component-extensions-for-runtime-platforms.md)<br/>
[nullptr](../cpp/nullptr.md)
