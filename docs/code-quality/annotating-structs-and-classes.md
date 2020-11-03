---
title: 批注结构和类
ms.date: 06/28/2019
ms.topic: conceptual
f1_keywords:
- _Field_size_bytes_part_
- _Field_size_bytes_full_opt_
- _Field_size_bytes_
- _Field_size_opt_
- _Field_size_part_
- _Field_size_bytes_part_opt_
- _Field_range_
- _Field_size_part_opt_
- _Field_size_
- _Field_size_bytes_opt_
- _Struct_size_bytes_
- _Field_size_bytes_full_
- _Field_size_full_
- _Field_size_full_opt_
- _Field_z_
ms.assetid: b8278a4a-c86e-4845-aa2a-70da21a1dd52
ms.openlocfilehash: fe177e6afea088b59b16bfbd0bff6fa00b526222
ms.sourcegitcommit: 30792632548d1c71894f9fecbe2f554294b86020
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91765118"
---
# <a name="annotating-structs-and-classes"></a>批注结构和类

您可以通过使用类似于固定条件的批注来批注结构和类成员，在任何将封闭结构作为参数或结果值的函数调用或函数入口/出口上，它们都假设为 true。

## <a name="struct-and-class-annotations"></a>结构和类批注

- `_Field_range_(low, high)`

     该字段处于 (包含) 从到的范围 `low` `high` 。  等效于 `_Satisfies_(_Curr_ >= low && _Curr_ <= high)` 使用合适的前置或后置条件应用于带批注的对象。

- `_Field_size_(size)`, `_Field_size_opt_(size)`, `_Field_size_bytes_(size)`, `_Field_size_bytes_opt_(size)`

     在元素中具有可写大小的字段 (或字节) ，由指定 `size` 。

- `_Field_size_part_(size, count)`, `_Field_size_part_opt_(size, count)`,         `_Field_size_bytes_part_(size, count)`, `_Field_size_bytes_part_opt_(size, count)`

     一个字段，该字段在元素中具有可写的大小 (或指定的字节) `size` ，以及 `count` 这些元素的 (字节) 可读的。

- `_Field_size_full_(size)`, `_Field_size_full_opt_(size)`, `_Field_size_bytes_full_(size)`, `_Field_size_bytes_full_opt_(size)`

     在元素中具有可读和可写大小的字段 (或) ，由指定 `size` 。

- `_Field_z_`

     具有以 null 结尾的字符串的字段。

- `_Struct_size_bytes_(size)`

     适用于 struct 或类声明。  指示该类型的有效对象可能大于声明的类型，以及所指定的字节数 `size` 。  例如： 。

    ```cpp

    typedef _Struct_size_bytes_(nSize)
    struct MyStruct {
        size_t nSize;
        ...
    };

    ```

     然后，将类型为的参数的缓冲区大小（以字节为单位）为 `pM` `MyStruct *` ：

    ```cpp
    min(pM->nSize, sizeof(MyStruct))
    ```

## <a name="example"></a>示例

```cpp
#include <sal.h>

// This _Struct_size_bytes_ is equivalent to what below _Field_size_ means.
_Struct_size_bytes_(__builtin_offsetof(MyBuffer, buffer) + bufferSize * sizeof(int))
struct MyBuffer
{
    static int MaxBufferSize;

    _Field_z_
    const char* name;

    int firstField;

    // ... other fields

    _Field_range_(1, MaxBufferSize)
    int bufferSize;

    _Field_size_(bufferSize)        // Preferred way - easier to read and maintain.
    int buffer[]; // Using C99 Flexible array member
};
```

此示例的说明：

- `_Field_z_` 等效于 `_Null_terminated_`。  `_Field_z_` 对于 "名称" 字段，指定名称字段为以 null 结尾的字符串。
- `_Field_range_` 对于 `bufferSize` 指定，的值 `bufferSize` 应在1和 `MaxBufferSize` (都包含) 。
- 和批注的最终结果 `_Struct_size_bytes_` `_Field_size_` 是等效的。 对于具有相似布局的结构或类， `_Field_size_` 更易于读取和维护，因为它具有比等效批注更少的引用和计算 `_Struct_size_bytes_` 。 `_Field_size_` 不需要转换为字节大小。 如果字节大小是唯一的选项，例如，对于 void 指针字段， `_Field_size_bytes_` 可以使用。 如果 `_Struct_size_bytes_` 和均 `_Field_size_` 存在，则它们将可用于工具。 如果两个批注不一致，就会执行该操作。

## <a name="see-also"></a>请参阅

- [使用 SAL 注释减少 C/C++ 代码缺陷](../code-quality/using-sal-annotations-to-reduce-c-cpp-code-defects.md)
- [了解 SAL](../code-quality/understanding-sal.md)
- [对函数参数和返回值进行批注](../code-quality/annotating-function-parameters-and-return-values.md)
- [对函数行为进行批注](../code-quality/annotating-function-behavior.md)
- [对锁定行为进行批注](../code-quality/annotating-locking-behavior.md)
- [指定何时以及在何处应用批注](../code-quality/specifying-when-and-where-an-annotation-applies.md)
- [内部函数](../code-quality/intrinsic-functions.md)
- [最佳做法和示例](../code-quality/best-practices-and-examples-sal.md)
