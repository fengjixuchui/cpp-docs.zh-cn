---
title: " (c + +/CLI) 文本"
description: Literal 关键字是适用于编译时常量的 Microsoft c + +/CLI 上下文相关的关键字。
ms.date: 10/20/2020
ms.topic: reference
f1_keywords:
- literal
- literal_cpp
helpviewer_keywords:
- literal keyword [C++]
ms.openlocfilehash: 2d71a535252ba51f89407670b474a34b407eaffc
ms.sourcegitcommit: 59b7c18703d1ffd66827db0e2eeece490d3d8789
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2020
ms.locfileid: "92337208"
---
# <a name="literal-ccli"></a>`literal` (C + +/CLI) 

在编译中标记为的变量 () 数据成员 **`literal`** **`/clr`** 是编译时常量。 它是 c # 变量的本机等效项 [`const`](/dotnet/csharp/language-reference/keywords/const) 。

## <a name="all-platforms"></a>所有平台

### <a name="remarks"></a>备注

（此语言功能没有适用于所有运行时的备注。）

## <a name="windows-runtime"></a>Windows 运行时

### <a name="remarks"></a>备注

(此语言功能没有只适用于 Windows 运行时的备注。）

## <a name="common-language-runtime"></a>公共语言运行时

## <a name="remarks"></a>备注

标记为的数据成员 **`literal`** 必须在声明时进行初始化。 值必须为常量整型、枚举或字符串类型。 从初始化表达式的类型到数据成员的类型的转换 **`literal`** 不能要求用户定义的转换。

在运行时没有为该 **`literal`** 字段分配内存; 编译器仅将其值插入到类的元数据中。 **`literal`** 该值被视为编译时常量。 标准 c + + 中最接近的等效项是 **`constexpr`** ，但数据成员不能是 **`constexpr`** c + +/cli

标记为的变量 **`literal`** 与标记的变量不同 **`static const`** 。 **`static const`** 数据成员在元数据中无法用于其他编译器。 有关详细信息，请参阅 [`static`](../cpp/storage-classes-cpp.md) 和 [`const`](../cpp/const-cpp.md)。

**`literal`** 是上下文相关的关键字。 有关详细信息，请参阅 [上下文相关的关键字](context-sensitive-keywords-cpp-component-extensions.md)。

## <a name="examples"></a>示例

此示例显示 **`literal`** 变量表示 **`static`** 。

```cpp
// mcppv2_literal.cpp
// compile with: /clr
ref struct X {
   literal int i = 4;
};

int main() {
   int value = X::i;
}
```

下面的示例演示了 **`literal`** 在元数据中的效果：

```cpp
// mcppv2_literal2.cpp
// compile with: /clr /LD
public ref struct A {
   literal int lit = 0;
   static const int sc = 1;
};
```

注意 `sc` 和 `lit` 在元数据上的区别：`modopt` 指令应用于 `sc`，表示其他编译器会将其忽略。

```MSIL
.field public static int32 modopt([mscorlib]System.Runtime.CompilerServices.IsConst) sc = int32(0x00000001)
```

```MSIL
.field public static literal int32 lit = int32(0x00000000)
```

下面的示例（用 c # 编写）引用前面的示例中创建的元数据，并显示 **`literal`** 和 **`static const`** 变量的效果：

```csharp
// mcppv2_literal3.cs
// compile with: /reference:mcppv2_literal2.dll
// A C# program
class B {
   public static void Main() {
      // OK
      System.Console.WriteLine(A.lit);
      System.Console.WriteLine(A.sc);

      // C# does not enforce C++ const
      A.sc = 9;
      System.Console.WriteLine(A.sc);

      // C# enforces const for a literal
      A.lit = 9;   // CS0131

      // you can assign a C++ literal variable to a C# const variable
      const int i = A.lit;
      System.Console.WriteLine(i);

      // but you cannot assign a C++ static const variable
      // to a C# const variable
      const int j = A.sc;   // CS0133
      System.Console.WriteLine(j);
   }
}
```

## <a name="requirements"></a>要求

编译器选项：`/clr`

## <a name="see-also"></a>另请参阅

[适用于 .NET 和 UWP 的组件扩展](component-extensions-for-runtime-platforms.md)
