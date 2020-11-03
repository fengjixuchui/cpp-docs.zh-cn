---
title: 编译器警告（等级 1）C4727
ms.date: 08/19/2019
f1_keywords:
- C4727
helpviewer_keywords:
- C4727
ms.assetid: 991b0087-3a50-40f5-9cdb-cdc367cd472c
ms.openlocfilehash: e1eeb7e466e325772d6a1522e77983fd3de04293
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92923959"
---
# <a name="compiler-warning-level-1-c4727"></a>编译器警告（等级 1）C4727

"具有相同时间戳的 PCH pch_file obj_file_1 和 obj_file_2。  使用第一个 PCH。

> [!NOTE]
> 在 Visual Studio 2017 及更早版本中，预编译标头默认称为 *stdafx.h* ，在 Visual studio 2019 和更高版本中，默认情况下，它称为 " *.pch* "。

用 **/yc** 编译多个 compiland 时，以及编译器能够用同一 .pch 时间戳标记所有 .obj 文件时，会发生 C4727。

若要解决此问题，请使用 **/yc/c** 编译一个源文件 (创建 pch) ，另外使用 **/yu/c** 进行编译 (使用 pch) ，然后将它们链接在一起。

如果执行了以下操作，则会生成 C4727：

::: moniker range="<=msvc-150"

**cl/clr/GL a .cpp b./Ycstdafx。h**

您可以改为执行以下操作：

**cl/clr/GL a .cpp/Ycstdafx.h/c**

**cl/clr/GL b. .cpp/Yustdafx.h/link a .obj**

::: moniker-end

::: moniker range=">=msvc-160"

**cl/clr/GL a .cpp b./Ycpch。h**

您可以改为执行以下操作：

**cl/clr/GL a .cpp/Ycpch.h/c**

**cl/clr/GL b. .cpp/Yupch.h/link a .obj**

::: moniker-end

有关详细信息，请参阅

- [/Yc (创建预编译头文件) ](../../build/reference/yc-create-precompiled-header-file.md)

- [/Yu (使用预编译头文件) ](../../build/reference/yu-use-precompiled-header-file.md)
