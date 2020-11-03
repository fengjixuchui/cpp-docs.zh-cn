---
title: 特性参数类型（C++/CLI 和 C++/CX）
ms.date: 10/12/2018
ms.topic: reference
helpviewer_keywords:
- custom attributes, parameter types
ms.assetid: d9f127a3-7f08-456f-acc6-256805632712
ms.openlocfilehash: cf33014c455bef145f7b7ec7ee353f27d1157f24
ms.sourcegitcommit: 43cee7a0d41a062661229043c2f7cbc6ace17fa3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92008605"
---
# <a name="attribute-parameter-types--ccli-and-ccx"></a>特性参数类型（C++/CLI 和 C++/CX）

编译器在编译时必须知道传递给特性的值。  特性参数可为下列类型：

- **`bool`**

- **`char`**, **`unsigned char`**

- **`short`**, **`unsigned short`**

- **`int`**, **`unsigned int`**

- **`long`**, **`unsigned long`**

- **`__int64`**， **未签名 __int64**

- **`float`**, **`double`**

- **`wchar_t`**

- **`char*`** 或 `wchar_t*` 或 `System::String*`

- `System::Type ^`

- `System::Object ^`

- **`enum`**

## <a name="example-attribute-parameter-types"></a>示例： Attribute 参数类型

### <a name="code"></a>代码

```cpp
// attribute_parameter_types.cpp
// compile with: /clr /c
using namespace System;
ref struct AStruct {};

[AttributeUsage(AttributeTargets::ReturnValue)]
ref struct Attr : public Attribute {
   Attr(AStruct ^ i){}
   Attr(bool i){}
   Attr(){}
};

ref struct MyStruct {
   static AStruct ^ x = gcnew AStruct;
   [returnvalue:Attr(x)] int Test() { return 0; }   // C3104
   [returnvalue:Attr] int Test2() { return 0; }   // OK
   [returnvalue:Attr(true)] int Test3() { return 0; }   // OK
};
```

## <a name="example-unnamed-arguments-precede-named-arguments"></a>示例：命名参数之前的未命名参数

### <a name="description"></a>说明

当指定特性时，所有未命名（位置）的自变量必须优先于命名自变量。

### <a name="code"></a>代码

```cpp
// extending_metadata_c.cpp
// compile with: /clr /c
using namespace System;
[AttributeUsage(AttributeTargets::Class)]
ref class MyAttr : public Attribute {
public:
   MyAttr() {}
   MyAttr(int i) {}
   property int Priority;
   property int Version;
};

[MyAttr]
ref class ClassA {};   // No arguments

[MyAttr(Priority = 1)]
ref class ClassB {};   // Named argument

[MyAttr(123)]
ref class ClassC {};   // Positional argument

[MyAttr(123, Version = 1)]
ref class ClassD {};   // Positional and named
```

## <a name="example-one-dimensional-array-attribute-parameter"></a>示例：一维数组特性参数

### <a name="description"></a>说明

特性参数可以为上述类型的一维数组。

### <a name="code"></a>代码

```cpp
// extending_metadata_d.cpp
// compile with: /clr /c
using namespace System;

[AttributeUsage(AttributeTargets::Class)]
public ref struct ABC : public Attribute {
   ABC(array<int>^){}
   array<double> ^ param;
};

[ABC( gcnew array<int> {1,2,3}, param = gcnew array<double>{2.71, 3.14})]
ref struct AStruct{};
```

## <a name="see-also"></a>请参阅

[用户定义的属性](user-defined-attributes-cpp-component-extensions.md)
