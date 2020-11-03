---
title: 操作员地址： &amp;
description: C + + 语言中的地址运算符。
ms.date: 10/02/2020
f1_keywords:
- '&'
helpviewer_keywords:
- address-of operator (&)
- '& operator'
- '& operator [C++], address-of operator'
ms.assetid: 2828221a-15f6-4acc-87fe-25e34feebb88
ms.openlocfilehash: 8ef7ad065281e4de58ddbdebea25950f8eb9dd06
ms.sourcegitcommit: 30792632548d1c71894f9fecbe2f554294b86020
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91765284"
---
# <a name="address-of-operator-amp"></a>操作员地址： &amp;

## <a name="syntax"></a>语法

> **`&`** *`cast-expression`*

## <a name="remarks"></a>注解

运算符的一元地址 (**`&`**) 采用其操作数的地址。 Address 运算符的操作数可以是函数指示符或指定不是位域的对象的左值。

Address 运算符只能应用于在文件范围级别声明的基本、结构、类或联合类型的变量，或者应用于下标数组引用。 在这些表达式中，可在地址表达式中添加或减去不包含 address 运算符的常量表达式。

当应用于函数或左值时，该表达式的结果将是派生自操作数类型（右值）的指针类型。 例如，如果操作数的类型为，则 **`char`** 表达式的结果为指向的类型指针 **`char`** 。 应用于或对象的 address 运算符的 **`const`** **`volatile`** 计算结果为 `const type *` 或 `volatile type *` ，其中 `type` 是原始对象的类型。

只有在清除函数的哪个版本被引用时，才能执行重载函数的地址。 有关如何获取特定重载函数的地址的信息，请参阅 [函数重载](function-overloading.md) 。

当将地址运算符应用于限定名称时，结果取决于 *限定名* 是否指定了静态成员。 如果是这样，则结果为指向成员声明中指定的类型的指针。 对于非静态成员，结果是一个指针，指向由*限定类名称*指示的类的成员*名称*。 有关 *限定类名*的详细信息，请参阅 [主表达式](../cpp/primary-expressions.md)。

## <a name="example-address-of-static-member"></a>示例：静态成员的地址

下面的代码段演示如何根据类成员是否是静态的，地址运算符结果如何变化：

```cpp
// expre_Address_Of_Operator.cpp
// C2440 expected
class PTM {
public:
    int iValue;
    static float fValue;
};

int main() {
   int   PTM::*piValue = &PTM::iValue;  // OK: non-static
   float PTM::*pfValue = &PTM::fValue;  // C2440 error: static
   float *spfValue     = &PTM::fValue;  // OK
}
```

在此示例中，由于 `&PTM::fValue` 是静态成员，因此表达式 `float *` 产生类型 `float PTM::*` 而不是类型 `fValue`。

## <a name="example-address-of-a-reference-type"></a>示例：引用类型的地址

通过将 address-of 运算符应用于引用类型，可获得与将该运算符应用于引用绑定到的对象所获得的结果相同的结果。 例如： 。

```cpp
// expre_Address_Of_Operator2.cpp
// compile with: /EHsc
#include <iostream>
using namespace std;
int main() {
   double d;        // Define an object of type double.
   double& rd = d;  // Define a reference to the object.

   // Obtain and compare their addresses
   if( &d == &rd )
      cout << "&d equals &rd" << endl;
}
```

```Output
&d equals &rd
```

## <a name="example-function-address-as-parameter"></a>示例：作为参数的函数地址

以下示例使用 address-of 运算符将指针自变量传递给函数：

```cpp
// expre_Address_Of_Operator3.cpp
// compile with: /EHsc
// Demonstrate address-of operator &

#include <iostream>
using namespace std;

// Function argument is pointer to type int
int square( int *n ) {
   return (*n) * (*n);
}

int main() {
   int mynum = 5;
   cout << square( &mynum ) << endl;   // pass address of int
}
```

```Output
25
```

## <a name="see-also"></a>请参阅

[使用一元运算符的表达式](../cpp/expressions-with-unary-operators.md)<br/>
[C + + 内置运算符、优先级和结合性](../cpp/cpp-built-in-operators-precedence-and-associativity.md)<br/>
[Lvalue 引用声明符：&](../cpp/lvalue-reference-declarator-amp.md)<br/>
[间接寻址和 Address-of 运算符](../c-language/indirection-and-address-of-operators.md)
