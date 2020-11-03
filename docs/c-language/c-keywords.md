---
title: C 关键字
description: 标准 C 和 Microsoft C 编译器扩展中的关键字。
ms.date: 10/15/2020
helpviewer_keywords:
- keywords [C]
- redefining keywords
- Microsoft-specific keywords
ms.assetid: 2d932335-97bf-45cd-b367-4ae00db0ff42
ms.openlocfilehash: 24981c8d70cb56b4578fd905a30ccc57eaa83d45
ms.sourcegitcommit: f19f02f217b80804ab321d463c76ce6f681abcc6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2020
ms.locfileid: "92176227"
---
# <a name="c-keywords"></a>C 关键字

“关键字”是对 C 编译器具有特殊含义的单词。 在翻译的第 7 和第 8 阶段中，标识符不能具有与 C 关键字相同的拼写和大小写。 有关详细信息，请参阅《预处理器参考》中的[翻译阶段](../preprocessor/phases-of-translation.md)。 有关标识符的详细信息，请参阅[标识符](../c-language/c-identifiers.md)。

## <a name="standard-c-keywords"></a>标准 C 关键字

C 语言使用下列关键字：

:::row:::
    :::column:::
        **`auto`**\
        **`break`**\
        **`case`**\
        **`char`**\
        **`const`**\
        **`continue`**\
        **`default`**\
        **`do`**\
        **`double`**\
        **`else`**\
        **`enum`**
    :::column-end:::
    :::column:::
        **`extern`**\
        **`float`**\
        **`for`**\
        **`goto`**\
        **`if`**\
        **`inline`** <sup>1, a</sup>\
        **`int`**\
        **`long`**\
        **`register`**\
        **`restrict`** <sup>1, a</sup>\
        **`return`**
    :::column-end:::
    :::column:::
        **`short`**\
        **`signed`**\
        **`sizeof`**\
        **`static`**\
        **`struct`**\
        **`switch`**\
        **`typedef`**\
        **`union`**\
        **`unsigned`**\
        **`void`**\
        **`volatile`**
    :::column-end:::
    :::column:::
        **`while`**\
        **`_Alignas`** <sup>2, a</sup>\
        **`_Alignof`** <sup>2, a</sup>\
        **`_Atomic`** <sup>2, b</sup>\
        **`_Bool`** <sup>1, a</sup>\
        **`_Complex`** <sup>1, b</sup>\
        **`_Generic`** <sup>2, a</sup>\
        **`_Imaginary`** <sup>1, b</sup>\
        **`_Noreturn`** <sup>2, a</sup>\
        **`_Static_assert`** <sup>2, a</sup>\
        **`_Thread_local`** <sup>2, b</sup>
    :::column-end:::
:::row-end:::

<sup>1</sup>  ISO C99 中引入的关键字。

<sup>2</sup>   ISO C11 中引入的关键字。

<sup>a</sup>  从 Visual Studio 2019 版本 16.8 开始，如果指定了 `/std:c11` 或 `/std:c17` 编译器选项，将在编译为 C 的代码中支持这些关键字。

<sup>b</sup>  从 Visual Studio 2019 版本 16.8 开始，如果指定了 `/std:c11` 或 `/std:c17` 编译器选项，这些关键字将由编译器在编译为 C 的代码中识别，但不受支持。

不能重新定义关键字。 但是，你可以在编译前通过使用 C [预处理器指令](../preprocessor/preprocessor-directives.md)指定文本来替换关键字。

## <a name="microsoft-specific-c-keywords"></a>Microsoft 专用 C 关键字

ANSI 和 ISO C 标准允许为编译器实现保留带有两个前导下划线的标识符。 Microsoft 的惯例是在 Microsoft 专用关键字名称前加上双下划线。 这些单词不能用作标识符名称。 有关标识符命名规则的说明，包括双下划线的使用，请参阅[标识符](../c-language/c-identifiers.md)。

下列关键字和特殊标识符由 Microsoft C 编译器识别：

:::row:::
    :::column:::
        **`__asm`** <sup>5</sup>\
        **`dllimport`** <sup>4</sup>\
        **`__int8`** <sup>5</sup>\
        **`naked`** <sup>4</sup>\
        **`__based`** <sup>3、5</sup>
    :::column-end:::
    :::column:::
        **`__except`** <sup>5</sup>\
        **`__int16`** <sup>5</sup>\
        **`__stdcall`** <sup>5</sup>\
        **`__cdecl`** <sup>5</sup>\
        **`__fastcall`**
    :::column-end:::
    :::column:::
        **`__int32`** <sup>5</sup>\
        **`thread`** <sup>4</sup>\
        **`__declspec`** <sup>5</sup>\
        **`__finally`** <sup>5</sup>\
        **`__int64`** <sup>5</sup>
    :::column-end:::
    :::column:::
        **`__try`** <sup>5</sup>\
        **`dllexport`** <sup>4</sup>\
        **`__inline`** <sup>5</sup>\
        **`__leave`** <sup>5</sup>\
        **`static_assert`** <sup>6</sup>
    :::column-end:::
:::row-end:::

<sup>3</sup> `__based` 关键字对 32 位和 64 位目标编译的用途有限。

<sup>4</sup> 当与 `__declspec` 一起使用时，这些关键字是特殊的标识符；它们在其他情况下的使用不受限制。

<sup>5</sup> 为了与以前的版本兼容，当启用 Microsoft 扩展时，这些关键字既可以使用两个前导下划线，也可以使用一个前导下划线。

<sup>6</sup> 如果不包括 <assert.h>，则 Microsoft Visual C 编译器会将 `static_assert` 映射到 C11 `_Static_assert` 关键字 。

默认情况下将启用 Microsoft 扩展。 为了帮助创建可移植的代码，可以在编译过程中指定 [/Za \(禁用语言扩展)](../build/reference/za-ze-disable-language-extensions.md) 选项来禁用 Microsoft 扩展。 如果使用此选项，将禁用某些 Microsoft 专用关键字。

启用 Microsoft 扩展时，您可在程序中使用上面列出的关键字。 为了符合标准，这些关键字大多使用前导双下划线。 `dllexport`、`dllimport`、`naked` 和 `thread` 这 4 个关键字除外，它们只与 `__declspec` 一起使用，不需要前导双下划线。 为了向后兼容，支持其余的关键字的单下划线版本。

## <a name="see-also"></a>请参阅

[C 的元素](../c-language/elements-of-c.md)
