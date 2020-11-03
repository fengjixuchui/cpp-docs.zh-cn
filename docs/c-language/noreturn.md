---
title: _Noreturn 关键字和 noreturn 宏 (C11)
description: 描述 `_Noreturn` 关键字和 `noreturn` 宏。
ms.date: 10/19/2020
f1_keywords:
- _Noreturn_c
- noreturn
helpviewer_keywords:
- keywords [C]
ms.openlocfilehash: 68448d8b8c999c92fb240100c25048fa94a559b9
ms.sourcegitcommit: 19016630f9d35f365e9ba249e0f3617515d7ca33
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92274694"
---
# <a name="_noreturn-keyword-and-noreturn-macro-c11"></a>`_Noreturn` 关键字和 `noreturn` 宏 (C11)

`_Noreturn` 关键字在 C11 中引入。 它告知编译器，应用编译器的函数不返回调用方。 编译器知晓调用 `_Noreturn` 函数后的代码是不可访问的。 不返回的函数的一个示例为 [abort](../c-runtime-library/reference/abort.md)。 如果控制流有可能返回调用方，则函数不能具有 `_Noreturn` 属性。

该关键字通常通过 <stdnoreturn.h> 中提供的便捷宏 `noreturn` 来进行使用，该宏映射到 `_Noreturn` 关键字。

使用 `_Noreturn`（或等效的 `noreturn`）的主要好处是在代码中明确函数的意向，便于将来的读者了解，以及检测意外产生的无法访问的代码。

标记为 `noreturn` 的函数不应包括返回类型，因为它不会将值返回给调用方。 它应为 `void`。

## <a name="example-using-noreturn-macro-and-_noreturn-keyword"></a>使用 `noreturn` 宏和 `_Noreturn ` 关键字的示例

下面的示例演示了 `_Noreturn` 关键字和等效的 `noreturn` 宏。

如果使用可以忽略的宏 `noreturn`，则 IntelliSense 可能会生成虚假错误 `E0065`。 它不会阻止你运行该示例。

```C
// Compile with Warning Level4 (/W4) and /std:c11
#include <stdio.h>
#include <stdlib.h>
#include <stdnoreturn.h>

noreturn void fatal_error(void)
{
    exit(3);
}

_Noreturn void not_coming_back(void)
{
    puts("There's no coming back");
    fatal_error();
    return; // warning C4645 - function declared with noreturn has a return statement
}

void done(void)
{
    puts("We'll never get here");
}

int main(void)
{
    not_coming_back();
    done(); // warning c4702 - unreachable code

    return 0;
}
```

## <a name="requirements"></a>要求

|宏|必需的标头|
|-------------|---------------------|
|**`noreturn`**|\<stdnoreturn.h>|

## <a name="see-also"></a>另请参阅

[/std（指定语言标准版本）](../build/reference/std-specify-language-standard-version.md)\
[/W4（指定警告等级）](../build/reference/compiler-option-warning-level.md)\
[C4702 警告](../error-messages\compiler-warnings\compiler-warning-level-4-c4702.md)\
[__declspec(noreturn)](../cpp/noreturn.md)
