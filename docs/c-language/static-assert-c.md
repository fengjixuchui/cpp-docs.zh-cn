---
title: _Static_assert 关键字和 static_assert 宏 (C11)
description: 描述 C11 _Static_assert 关键字和 C11 static_assert 宏。
ms.date: 10/13/2020
f1_keywords:
- static_assert_c
- _Static_assert
helpviewer_keywords:
- assertions [C], _Static_assert, static_assert
ms.openlocfilehash: dbe49b1dcbb8dd4e0d9df678f4456bc605e01c3f
ms.sourcegitcommit: 651348f8cd92ab0d52f09e9225a7eb41562559db
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92060974"
---
# <a name="_static_assert-keyword-and-static_assert-macro-c11"></a>_Static_assert 关键字和 static_assert 宏 (C11)

在编译时测试断言。 如果指定的常数表达式为 `false`，则编译器显示指定的消息，并且编译失败，错误为 C2338；否则，不会产生任何影响。 C11 中的新增功能。

`_Static_assert` 是 C11 中引入的关键字。
`static_assert` 是 C11 中引入的宏，它映射到 `_Static_assert` 关键字 。

## <a name="syntax"></a>语法

```C
_Static_assert(constant-expression, string-literal);
static_assert(constant-expression, string-literal);
```

### <a name="parameters"></a>parameters

constant-expression\
可在编译时计算的整型常数表达式。 如果表达式为零 (false)，则显示 string-literal 参数，并且编译因出错而失败。 如果表达式不为零 (true)，则不会产生任何影响。

string-literal\
如果 constant-expression 计算结果为零 (false)，则显示此消息。 此消息必须使用编译器的[基本字符集](../c-language/ascii-character-set.md)来生成。 字符不能为[多字节字符或宽字符](../c-language/multibyte-and-wide-characters.md)。

## <a name="remarks"></a>备注

`_Static_assert` 关键字和 `static_assert` 宏均在编译时测试软件断言 。 它们可用于全局或函数范围。

相反，[assert 宏、_assert 和 _wassert 函数](../c-runtime-library/reference/assert-macro-assert-wassert.md)在运行时测试软件断言，并产生运行时成本。

Microsoft 特定行为

在 C 中，不包含 `<assert.h>` 时，Microsoft Visual C/C++ 编译器会将 `static_assert` 视为映射到 `_Static_assert` 的关键字 。 首选使用 `static_assert`，因为相关代码在 C 和 C++ 中均适用。

## <a name="example-of-a-compile-time-assert"></a>编译时断言示例

在下面的示例中，`static_assert` 和 `_Static_assert` 用于验证枚举中有多少个元素以及整数的宽度是否为 32 位 。

```C
// requires /std:c11 or higher
#include <assert.h>

enum Items
{
    A,
    B,
    C,
    LENGTH
};

int main()
{
    // _Static_assert is a C11 keyword
    _Static_assert(LENGTH == 3, "Expected Items enum to have three elements");

    // Preferred: static_assert maps to _Static_Assert and is compatible with C++
    static_assert(sizeof(int) == 4, "Expecting 32 bit integers"); 

    return 0;
}
```

## <a name="requirements"></a>要求

|宏|必需的标头|
|-------------|---------------------|
|**`static_assert`**|\<assert.h>|

## <a name="see-also"></a>另请参阅

[_STATIC_ASSERT 宏](../c-runtime-library/reference/static-assert-macro.md)\
[assert 宏、_assert 和 _wassert 函数](../c-runtime-library/reference/assert-macro-assert-wassert.md)
[/std（指定语言标准版本）](../build/reference/std-specify-language-standard-version.md)