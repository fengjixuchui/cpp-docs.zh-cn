---
title: Unicode：宽字符集
description: Microsoft C 运行时中的 Unicode 宽字符集简介。
ms.topic: conceptual
ms.date: 11/04/2016
helpviewer_keywords:
- Unicode [C++], wide character set
- wide characters [C++], Unicode
ms.assetid: b6a05a21-59a5-4d30-8c85-2dbe185f7a74
ms.openlocfilehash: 7cd170ae43223f1e8e61d9fc576e49baa2164b23
ms.sourcegitcommit: 30792632548d1c71894f9fecbe2f554294b86020
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91765336"
---
# <a name="unicode-the-wide-character-set"></a>Unicode：宽字符集

宽字符是双字节多语言字符代码。 在现代全球计算业内使用的任意字符（包括技术符号和特殊的发布字符），都可以根据 Unicode 规范表示为宽字符形式。 由包括 Microsoft 在内的大财团开发和维护的 Unicode 标准现在被广泛接受。

宽字符的类型为 **`wchar_t`** 。 宽字符字符串表示为 **`wchar_t[]`** 数组。 使用指针指向数组 `wchar_t*` 。

可以通过在字母前面加上前缀来将任何 ASCII 字符表示为宽字符 `L` 。 例如， `L'\0'` 是终止宽 (16 位) null 字符。

可以通过在字母前面加上前缀来将任何 ASCII 字符串文本表示为宽字符字符串 `L` 。 例如 `L"Hello"`。

通常，宽字符在内存中占用的空间比多字节字符多。 但宽字符处理速度更快。 多字节编码中一次只能表示一个区域设置。 世界上的所有字符集同时按 Unicode 表示形式表示。

## <a name="see-also"></a>请参阅

[国际化](../c-runtime-library/internationalization.md)\
[按类别分的通用 C 运行时例程](../c-runtime-library/run-time-routines-by-category.md)
