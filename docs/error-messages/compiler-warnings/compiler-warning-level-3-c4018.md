---
title: 编译器警告 (等级 3) C4018
description: Microsoft C/c + + 编译器警告 C4018，其原因和解决方法。
ms.date: 10/16/2020
f1_keywords:
- C4018
helpviewer_keywords:
- C4018
ms.openlocfilehash: b9d01f6b733c2ca18880aa6f4b6fca9771f8123f
ms.sourcegitcommit: f19f02f217b80804ab321d463c76ce6f681abcc6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2020
ms.locfileid: "92176170"
---
# <a name="compiler-warning-level-3-c4018"></a>编译器警告 (等级 3) C4018

> "*token*"：有符号/无符号不匹配

使用 *标记* 运算符比较 **`signed`** 和 **`unsigned`** 数字要求编译器将 **`signed`** 值转换为 **`unsigned`** 。

## <a name="remarks"></a>备注

解决此警告的一种方法是在比较和类型时转换这两种类型 **`signed`** 之一 **`unsigned`** 。

## <a name="example"></a>示例

此示例生成 C4018，并演示如何修复此问题：

```cpp
// C4018.cpp
// compile with: cl /EHsc /W4 C4018.cpp
int main() {
    unsigned int uc = 0;
    int c = 0;
    unsigned int c2 = c; // implicit conversion

    if (uc < c)           // C4018
        uc = 0;

    if (uc < unsigned(c)) // OK
        uc = 0;

    if (uc < c2)          // Also OK
       uc = 0;
}
```

## <a name="see-also"></a>另请参阅

[编译器警告 (等级 4) C4388](c4388.md)\
[编译器警告 (等级 4) C4389](compiler-warning-level-4-c4389.md)
