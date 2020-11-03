---
title: 编译器警告 (等级 4) C4800
ms.date: 03/14/2019
f1_keywords:
- C4800
helpviewer_keywords:
- C4800
ms.assetid: 4f409799-a250-45ed-bb5f-657691b0d9f7
ms.openlocfilehash: bcc98b9bb24f77e39a31332b8fbe2f7dcc5a7638
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924610"
---
# <a name="compiler-warning-level-4-c4800"></a>编译器警告 (等级 4) C4800

::: moniker range=">= msvc-160"
Visual Studio 2019 及更高版本：
> 从 " *type* " 到布尔值的隐式转换。 可能的信息丢失
::: moniker-end

C4800 是 Visual Studio 2015 及更早版本中的3级警告：
> " *type* "：将值强制为布尔值 "true" 或 "false" (性能警告) 

当值隐式转换为类型时，将生成此警告 **`bool`** 。 通常情况下，此消息是通过以下方式引起的：将变量赋给变量， **`int`** **`bool`** 其中 **`int`** 变量只包含值 **`true`** 和 **`false`** ，并且可以重新声明为类型 **`bool`** 。 如果无法重写表达式以使用类型 **`bool`** ，则可以将 "" 添加 `!=0` 到表达式中，后者将提供表达式类型 **`bool`** 。 将表达式强制转换为类型 **`bool`** 不会禁用该警告，这是设计使然。

::: moniker range=">= msvc-150"
此警告不会在 Visual Studio 2017 中发出。
::: moniker-end

::: moniker range=">= msvc-160"
从 Visual Studio 2019 开始，此警告默认为关闭状态。 使用 __/w__*n*__4800__ 启用 C4800 作为级别 *n* 警告，或使用 [/Wall](../../build/reference/compiler-option-warning-level.md) 启用默认情况下处于关闭状态的所有警告。 有关详细信息，请参阅 [默认情况下处于关闭状态的编译器警告](../../preprocessor/compiler-warnings-that-are-off-by-default.md)。
::: moniker-end

## <a name="example"></a>示例

下面的示例生成 C4800，并演示如何修复此问题：

```cpp
// C4800.cpp
// compile with: /W4 /w44800
int main() {
   int i = 0;

   // To fix, instead try:
   // bool i = 0;

   bool j = i;   // C4800
   j++;
}
```
