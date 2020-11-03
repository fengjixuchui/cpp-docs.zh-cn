---
title: 主表达式
description: C + + 编程语言中的主表达式。
ms.date: 10/02/2020
helpviewer_keywords:
- primary expressions
- expressions [C++], name
- expressions [C++], literal
- expressions [C++], primary
- expressions [C++], qualified names
ms.assetid: 8ef9a814-6058-4b93-9b6e-e8eb8350b1ca
ms.openlocfilehash: 4c52992071453bc189a3078db9592b02dfb8ba9b
ms.sourcegitcommit: 30792632548d1c71894f9fecbe2f554294b86020
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91765320"
---
# <a name="primary-expressions"></a>主表达式

主表达式是更复杂的表达式的构造块。 它们可能是由范围解析运算符限定的文本、名称和名称 (`::`) 。 主表达式可以具有以下任一形式：

*`primary-expression`*\
&emsp;*`literal`*\
&emsp;**`this`**\
&emsp;*`name`*\
&emsp;**`::`** *`name`* **`(`** *`expression`* **`)`**

*`literal`* 是常量主表达式。 其类型取决于其规范的形式。 有关指定文本的完整信息，请参阅 [文本](../cpp/numeric-boolean-and-pointer-literals-cpp.md) 。

**`this`** 关键字是指向类对象的指针。 它在非静态成员函数中可用。 它指向为其调用函数的类的实例。 **`this`** 关键字不能在类成员函数体的外部使用。

指针的类型 **`this`** 为 `type * const` (其中 `type` 是不专门修改指针的函数中的类名称) **`this`** 。 下面的示例演示了成员函数声明和类型 **`this`** ：

```cpp
// expre_Primary_Expressions.cpp
// compile with: /LD
class Example
{
public:
    void Func();          //  * const this
    void Func() const;    //  const * const this
    void Func() volatile; //  volatile * const this
};
```

有关修改指针类型的详细信息 **`this`** ，请参阅[ `this` 指针](this-pointer.md)。

范围解析运算符 (**`::`**) 后跟名称是主表达式。  此类名称必须是全局范围内的名称，而不是成员名称。 表达式的类型由名称的声明决定。 它是一个左值 (也就是说，它可以出现在赋值表达式的左侧) 如果声明名称是左值。 范围解析运算符允许引用全局名称，即使该名称隐藏在当前范围中也如此。 有关如何使用范围解析运算符的示例，请参阅 [范围](../cpp/scope-visual-cpp.md) 。

括在括号中的表达式是主表达式。 其类型和值与 unparenthesized 表达式的类型和值相同。 如果 unparenthesized 表达式是左值，则它是左值。

主表达式的示例包括：

```cpp
100 // literal
'c' // literal
this // in a member function, a pointer to the class instance
::func // a global function
::operator + // a global operator function
::A::B // a global qualified name
( i + 1 ) // a parenthesized expression
```

这些示例都是所有 *名称*，在各种形式下都是这样的主表达式：

```cpp
MyClass // an identifier
MyClass::f // a qualified name
operator = // an operator function name
operator char* // a conversion operator function name
~MyClass // a destructor name
A::B   // a qualified name
A<int> // a template id
```

## <a name="see-also"></a>请参阅

[表达式的类型](../cpp/types-of-expressions.md)
