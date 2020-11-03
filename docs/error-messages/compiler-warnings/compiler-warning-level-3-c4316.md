---
title: 编译器警告 (等级 3) C4316
description: C + + 编译器警告 C4316 说明
ms.date: 11/04/2016
f1_keywords:
- C4316
helpviewer_keywords:
- C4316
ms.assetid: 10371f01-aeb8-40ac-a290-59e63efa5ad4
ms.openlocfilehash: 3cb512aa9b851f3b3b26f7a50854a4d887087e81
ms.sourcegitcommit: 8caaf5e00aeb727741a273aecafa15de293426cf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/07/2020
ms.locfileid: "91806548"
---
# <a name="compiler-warning-level-3-c4316"></a>编译器警告 (等级 3) C4316

堆上分配的对象可能不会与此类型对齐。

使用分配的过度对齐的对象 `operator new` 可能没有指定的对齐方式。 为过度对齐的类型重写 [运算符 new](../../c-runtime-library/new-operator-crt.md) 和 [operator delete](../../c-runtime-library/delete-operator-crt.md) ，以便它们使用对齐的分配例程（例如 [_aligned_malloc](../../c-runtime-library/reference/aligned-malloc.md) 和 [_aligned_free](../../c-runtime-library/reference/aligned-free.md)）。 下面的示例生成 C4316：

```cpp
// C4316.cpp
// Test: cl /W3 /c C4316.cpp

__declspec(align(32)) struct S {}; // C4324

int main() {
    new S; // C4316
}
```
