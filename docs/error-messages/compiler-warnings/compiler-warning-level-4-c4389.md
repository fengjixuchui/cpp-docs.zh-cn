---
title: 编译器警告（等级 4）C4389
description: Microsoft C/c + + 编译器警告 C4389，其原因和解决方法。
ms.date: 10/16/2020
f1_keywords:
- c4389
helpviewer_keywords:
- C4389
ms.openlocfilehash: b06b013151ed12b4f66bb23a7e862992d05e6b30
ms.sourcegitcommit: f19f02f217b80804ab321d463c76ce6f681abcc6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2020
ms.locfileid: "92176263"
---
# <a name="compiler-warning-level-4-c4389"></a>编译器警告（等级 4）C4389

> "*相等-运算符*"：有符号/无符号不匹配

**`==`** 或 **`!=`** 运算涉及 **`signed`** 和 **`unsigned`** 变量。 这可能会导致数据丢失。

## <a name="remarks"></a>备注

解决此警告的一种方法是在比较和类型时转换这两种类型 **`signed`** 之一 **`unsigned`** 。

## <a name="example"></a>示例

下面的示例生成 C4389：

```cpp
// C4389.cpp
// compile with: cl /EHsc /W4 C4389.cpp

int main()
{
   int a = 9;
   unsigned int b = 10;
   int result = 0;

   if (a == b)   // C4389
      result = 1;
   else
      result = 2;

   if (unsigned(a) == b) // OK
      result = 3;
   else
      result = 4;

   return result;
}
```

## <a name="see-also"></a>另请参阅

[编译器警告 C4018](compiler-warning-level-3-c4018.md)\
[编译器警告 (等级 4) C4388](c4388.md)
