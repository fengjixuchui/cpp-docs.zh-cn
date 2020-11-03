---
title: C 枚举声明
description: C 编程语言中的枚举声明。
ms.date: 10/02/2020
helpviewer_keywords:
- declarations, enumerations
- define directive (#define), alternative to
- enumerators, declaring
- '#define directive, alternative to'
- named constants, enumeration declarations
- declaring enumerations
ms.assetid: bd18f673-4dda-4bc1-92fd-d1ce10074910
ms.openlocfilehash: b7df41475a630b9f6e1d735f5454f6d9601cdd16
ms.sourcegitcommit: 30792632548d1c71894f9fecbe2f554294b86020
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91765209"
---
# <a name="c-enumeration-declarations"></a>C 枚举声明

枚举由一组命名整数常量构成。 枚举类型声明提供（可选）枚举标记的名称。 并且，它定义了一组已命名的整数标识符（称为“枚举集”、“枚举器常量”、“枚举器”或“成员”）   。 枚举类型的变量存储该类型所定义的枚举集的值之一。

类型为 `enum` 的变量可用于索引表达式，并且可用作所有算术和关系运算符的操作数。 枚举提供了 `#define` 预处理器指令的替代方法，带来的好处是可为您生成值并遵循一般范围规则。

在 ANSI C 中，定义枚举器常量值的表达式始终具有 `int` 类型。 这意味着，与枚举变量关联的存储是单个 `int` 值所需的存储。 可以在 C 语言允许整数表达式的任意位置使用枚举常量或枚举类型的值。

## <a name="syntax"></a>语法

*`enum-specifier`*:\
&emsp; **`enum`** *`identifier`* <sub>opt</sub> **`{`** *`enumerator-list`* **`}`** \
&emsp;**`enum`** *`identifier`*

*`enumerator-list`*:\
&emsp;*`enumerator`*\
&emsp;*`enumerator-list`* **`,`** *`enumerator`*

*`enumerator`*:\
&emsp;*`enumeration-constant`*\
&emsp;*`enumeration-constant`* **`=`** *`constant-expression`*

*`enumeration-constant`*:\
&emsp;*`identifier`*

可选的 `identifier` 命名由 `enumerator-list` 定义的枚举类型 。 此标识符通常称为列表指定的枚举的“标记”。 类型说明符声明 `identifier` 是由 `enumerator-list` 非终止符指定的枚举的标记，如下所示：

```C
enum identifier
{
    // enumerator-list
}
```

`enumerator-list` 定义枚举集的成员。

如果标记的声明可见，则后续使用标记但忽略 `enumerator-list` 的声明将指定之前声明的枚举的类型。 标记必须引用定义的枚举类型，并且该枚举类型必须在当前范围内。 由于在其他位置定义枚举类型，因此 `enumerator-list` 不会出现在此声明中。 派生自枚举的类型的声明和枚举类型的 `typedef` 声明可以在定义枚举类型之前使用枚举标记。

`enumerator-list` 中的每个 `enumeration-constant` 命名一个枚举集的值 。 默认情况下，第一个 `enumeration-constant` 与值 0 相关联。 列表中的下一个 `enumeration-constant` 与 (`constant-expression` + 1) 的值相关联，除非显式将其与另一个值相关联 。 `enumeration-constant` 的名称与其值等效。

可使用 `enumeration-constant` = `constant-expression` 替代值的默认序列 。 也就是说，如果`enumeration-constant` = `constant-expression` 出现在 `enumerator-list` 中，则 `enumeration-constant` 与 `constant-expression` 给定的值相关联    。 `constant-expression` 必须是 `int` 类型，并且可以为负。

下面的规则适用于枚举集的成员：

- 枚举集可以包含重复的常量值。 例如，可以将值 0 与两个不同的标识符（例如同一集合中名为 `null` 和 `zero` 的成员）关联。

- 枚举列表中的标识符必须与同一范围中具有相同可见性的其他标识符不同。 这包括普通变量名和其他枚举列表中的标识符。

- 枚举标记遵循一般范围规则。 它们必须不同于具有相同可见性的其他枚举、结构和联合标记。

## <a name="examples"></a>示例

这些示例阐释枚举声明：

```C
enum DAY            /* Defines an enumeration type    */
{
    saturday,       /* Names day and declares a       */
    sunday = 0,     /* variable named workday with    */
    monday,         /* that type                      */
    tuesday,
    wednesday,      /* wednesday is associated with 3 */
    thursday,
    friday
} workday;
```

默认情况下，值 0 与 `saturday` 关联。 标识符 `sunday` 将显式设置为 0。 默认情况下，将为剩余标识符提供从 1 到 5 的值。

在此示例中，将集 `DAY` 中的值赋给变量 `today`。

```C
enum DAY today = wednesday;
```

可使用枚举常量的名称进行赋值。 由于之前声明了 `DAY` 枚举类型，因此仅枚举标记 `DAY` 是必需的。

若要显式将整数值赋给枚举数据类型的变量，请使用类型转换：

```C
workday = ( enum DAY ) ( day_value - 1 );
```

建议在 C 中进行此转换，但这不是必需的。

```C
enum BOOLEAN  /* Declares an enumeration data type called BOOLEAN */
{
    false,     /* false = 0, true = 1 */
    true
};

enum BOOLEAN end_flag, match_flag; /* Two variables of type BOOLEAN */
```

还可将此声明指定为

```C
enum BOOLEAN { false, true } end_flag, match_flag;\
```

或指定为

```C
enum BOOLEAN { false, true } end_flag;
enum BOOLEAN match_flag;
```

使用上述变量的示例可能类似于以下内容：

```C
if ( match_flag == false )
    {
     .
     .   /* statement */
     .
    }
    end_flag = true;
```

还可以声明未命名的枚举器数据类型。 忽略数据类型的名称，但可以声明变量。 变量 `response` 是已定义的类型的变量：

```C
enum { yes, no } response;
```

## <a name="see-also"></a>请参阅

[枚举](../cpp/enumerations-cpp.md)
