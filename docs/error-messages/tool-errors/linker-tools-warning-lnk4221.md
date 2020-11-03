---
title: 链接器工具警告 LNK4221
ms.date: 08/19/2019
f1_keywords:
- LNK4221
helpviewer_keywords:
- LNK4221
ms.assetid: 8e2eb2de-9532-4b85-908a-8c9ff5c4cccb
ms.openlocfilehash: f18224150232384adbf8ee7cc31af7bb7678eae5
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92919197"
---
# <a name="linker-tools-warning-lnk4221"></a>链接器工具警告 LNK4221

此对象文件未定义任何之前未定义的公共符号，因此任何使用此库的链接操作都不会使用它

请考虑以下两个代码片段。

```cpp
// a.cpp
#include <atlbase.h>
```

```cpp
// b.cpp
#include <atlbase.h>
int function()
{
   return 0;
}
```

若要编译文件和创建两个对象文件，请在命令提示符下运行 **cl/c a .cpp b.** 。 如果通过运行 **链接/lib/out：** LNK4221 链接对象文件，则将收到 "" 警告。 如果通过运行 **链接/lib/out： test.txt a .obj** 链接对象，则不会收到警告。

在第二种情况下不会发出任何警告，因为链接器在后进先出 (后进先出) 方法中进行操作。 在第一个方案中，先处理 .obj，然后再处理 .obj，而 .obj 没有要添加的新符号。 通过指示链接器首先处理 .obj，你可以避免出现此警告。

::: moniker range=">=msvc-160"

此错误的一个常见原因是两个源文件指定选项/Yc (使用 **预编译标头** 字段中指定的同一标头文件 [) 创建预编译头文件](../../build/reference/yc-create-precompiled-header-file.md)。 此问题的一个常见原因是处理 *pch* ，因为默认情况下， *pch* 包含 *pch* ，不添加任何新符号。 如果另一个源文件包含带有 **/yc** 的 *pch* ，并在 pch 之前处理相关的 .obj 文件，则链接器将引发 LNK4221。

::: moniker-end

::: moniker range="<=msvc-150"

此错误的一个常见原因是两个源文件指定选项/Yc (使用 **预编译标头** 字段中指定的同一标头文件 [) 创建预编译头文件](../../build/reference/yc-create-precompiled-header-file.md)。 此问题的一个常见原因是 *stdafx.h* ，因为默认情况下， *stdafx.h* 包含 *stdafx.h* ，不添加任何新符号。 如果另一个源文件包含带有 **/yc** 的 *stdafx.h* 并且在 stdafx.h 之前处理关联的 .obj 文件，则链接器将引发 LNK4221。

::: moniker-end

解决此问题的一种方法是，确保对于每个预编译标头，只有一个源文件与 **/yc** 一起包含。 所有其他源文件必须使用预编译头。 有关如何更改此设置的详细信息，请参阅 [/yu (使用预编译头文件) ](../../build/reference/yu-use-precompiled-header-file.md)。
