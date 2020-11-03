---
title: 成员函数模板
ms.date: 11/04/2016
helpviewer_keywords:
- function templates, member functions
ms.assetid: 83d51835-6a27-40ed-997c-7d90dc9182d8
ms.openlocfilehash: 8514c8ffe630f5bc44d8d287d6ccf08c7755e3a0
ms.sourcegitcommit: 43cee7a0d41a062661229043c2f7cbc6ace17fa3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92008556"
---
# <a name="member-function-templates"></a>成员函数模板

术语成员模板引用了成员函数模板和嵌套类模板。 成员函数模板是类或类模板的成员的模板函数。

成员函数可以是多个环境中的函数模板。 类模板的所有函数都是泛型的，但却不称为成员模板或成员函数模板。 如果这些成员函数采用其自己的模板自变量，则将它们视为成员函数模板。

## <a name="example-declare-member-function-templates"></a>示例：声明成员函数模板

非模板或模板类的成员函数模板将声明为带有其模板参数的函数模板。

```cpp
// member_function_templates.cpp
struct X
{
   template <class T> void mf(T* t) {}
};

int main()
{
   int i;
   X* x = new X();
   x->mf(&i);
}
```

## <a name="example-member-function-template-of-template-class"></a>示例：模板类的成员函数模板

以下示例显示模板类的成员函数模板。

```cpp
// member_function_templates2.cpp
template<typename T>
class X
{
public:
   template<typename U>
   void mf(const U &u)
   {
   }
};

int main()
{
}
```

## <a name="example-define-member-templates-outside-class"></a>示例：在类外部定义成员模板

```cpp
// defining_member_templates_outside_class.cpp
template<typename T>
class X
{
public:
   template<typename U>
   void mf(const U &u);
};

template<typename T> template <typename U>
void X<T>::mf(const U &u)
{
}

int main()
{
}
```

## <a name="example-templated-user-defined-conversion"></a>示例：模板化用户定义的转换

局部类不允许具有成员模板。

当使用与基类虚函数相同的名称进行声明时，成员模板函数不能是虚函数并且不能从基类重写虚函数。

下面的示例演示了一个模板化用户定义的转换：

```cpp
// templated_user_defined_conversions.cpp
template <class T>
struct S
{
   template <class U> operator S<U>()
   {
      return S<U>();
   }
};

int main()
{
   S<int> s1;
   S<long> s2 = s1;  // Convert s1 using UDC and copy constructs S<long>.
}
```

## <a name="see-also"></a>请参阅

[函数模板](../cpp/function-templates.md)
