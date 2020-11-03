---
title: 函数系列概述
description: 按系列的 Microsoft C 运行时函数的概述。
ms.date: 10/05/2020
ms.assetid: b05a2755-9d11-4ea9-ab97-d00a4e872e16
ms.openlocfilehash: de5192cd03c3821afc646241d75a3e8c6c8c12e3
ms.sourcegitcommit: 8caaf5e00aeb727741a273aecafa15de293426cf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/07/2020
ms.locfileid: "91806508"
---
# <a name="function-family-overview"></a>函数系列概述

本部分按函数系列列出 C 运行库例程。

## <a name="crt-library-routine-families"></a>CRT 库例程系列

[_exec，_wexec](exec-wexec-functions.md)\
用于加载和执行新进程的函数。

[文件名搜索函数](filename-search-functions.md)\
用于搜索指定文件名并关闭搜索的函数。

[和的格式规范语法 `printf``wprintf`](format-specification-syntax-printf-and-wprintf-functions.md)\
描述和的格式字符串和参数 `printf` `wprintf` 。

[格式规范字段字符： `scanf` 和 `wscanf`](format-specification-fields-scanf-and-wscanf-functions.md)\
介绍为整个函数系列分析输入流的格式规范字段 `scanf` 。

[`is`， `isw` 函数](is-isw-routines.md)\
用于测试字符是否为大写字母、ASCII、数字、标点符号等的函数。

[`_ismbb` 函数](ismbb-routines.md)\
用于测试整数值是否表示字母字符、空白字符、打印字符等的函数。

[`_ismbc` 函数](ismbc-routines.md)\
用于测试多字节字符是否表示字母字符、空白字符、打印字符等的函数。

[运算符 `delete` (CRT) ](delete-operator-crt.md)\
从 Visual Studio 2013 开始，通用 C 运行时 (UCRT) 不再支持 c + + 特定的 operator delete 函数。 它现在是 c + + 标准库的一部分。

[运算符 `new` (CRT) ](new-operator-crt.md)\
从 Visual Studio 2013 开始，通用 C 运行时 (UCRT) 不再支持 c + + 特定的 operator new 函数。 它现在是 c + + 标准库的一部分。

[`printf` 位置参数函数](printf-p-positional-parameters.md)\
位置参数按要替换为格式字符串中的字段的参数的数目指定。

[`scanf` 类型字段字符](scanf-type-field-characters.md)\
类型字符确定关联的自变量是解释为任何 `scanf` 函数系列（包括安全版本，如）的字符、字符串或数字 `scanf_s` 。

[`scanf` 宽度规范](scanf-width-specification.md)\
Width 字段是一个正整数，用于控制为该字段读取的最大字符数。 适用于任何 `scanf` 函数系列（包括安全版本），如 `scanf_s` 。

[_spawn，_wspawn 函数](spawn-wspawn-functions.md)\
用于创建和执行新进程的函数。

[`strcoll` 函数](strcoll-functions.md)\
`strcoll`和 `wcscoll` 函数根据 `LC_COLLATE` 区域设置代码页的类别设置来比较两个字符串。

[字符串到数值函数](string-to-numeric-value-functions.md)\
`strtod`函数系列将以 null 结尾的字符串转换为数字值。

[`vprintf` 函数](vprintf-functions.md)\
`vprintf`函数采用指向自变量列表的指针，设置其格式，并将结果写入指定的目标。 函数在执行参数验证时有所不同，无论它们是使用宽字符串还是单字节字符串、输出目标，还支持指定在格式字符串中使用参数的顺序。

## <a name="see-also"></a>请参阅

[C 运行时库引用](c-run-time-library-reference.md)