---
title: _InterlockedAdd 内部函数
ms.date: 09/02/2019
f1_keywords:
- _InterlockedAdd64_acq_cpp
- _InterlockedAdd64_acq
- _InterlockedAdd_acq
- _InterlockedAdd_nf
- _InterlockedAdd64_rel
- _InterlockedAdd64
- _InterlockedAdd_cpp
- _InterlockedAdd_rel_cpp
- _InterlockedAdd_rel
- _InterlockedAdd64_rel_cpp
- _InterlockedAdd64_cpp
- _InterlockedAdd_acq_cpp
- _InterlockedAdd64_nf
- _InterlockedAdd
helpviewer_keywords:
- _InterlockedAdd_nf intrinsic
- _InterlockedAdd_rel intrinsic
- _InterlockedAdd intrinsic
- _InterlockedAdd64 intrinsic
- _InterlockedAdd64_acq intrinsic
- _InterlockedAdd64_nf intrinsic
- _InterlockedAdd_acq intrinsic
- _InterlockedAdd64_rel intrinsic
ms.assetid: 3d319603-ea9c-4fdd-ae61-e52430ccc3b1
ms.openlocfilehash: c611a22e696b9dda0c6910cd4aac84399cc7d20a
ms.sourcegitcommit: ced5ff1431ffbd25b20d106901955532723bd188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2020
ms.locfileid: "92135549"
---
# <a name="_interlockedadd-intrinsic-functions"></a>_InterlockedAdd 内部函数

**Microsoft 专用**

这些函数执行原子加法，这可确保当多个线程有权访问共享变量时操作成功完成。

## <a name="syntax"></a>语法

```C
long _InterlockedAdd(
   long volatile * Addend,
   long Value
);
long _InterlockedAdd_acq(
   long volatile * Addend,
   long Value
);
long _InterlockedAdd_nf(
   long volatile * Addend,
   long Value
);
long _InterlockedAdd_rel(
   long volatile * Addend,
   long Value
);
__int64 _InterlockedAdd64(
   __int64 volatile * Addend,
   __int64 Value
);
__int64 _InterlockedAdd64_acq(
   __int64 volatile * Addend,
   __int64 Value
);
__int64 _InterlockedAdd64_nf (
   __int64 volatile * Addend,
   __int64 Value
);
__int64 _InterlockedAdd64_rel(
   __int64 volatile * Addend,
   __int64 Value
);
```

### <a name="parameters"></a>参数

*加数*\
[in，out]指向要添加到的整数的指针;替换为添加的结果。

*“值”* \
中要添加的值。

## <a name="return-value"></a>返回值

两个函数都将返回加法结果。

## <a name="requirements"></a>要求

|Intrinsic|体系结构|
|---------------|------------------|
|`_InterlockedAdd`|ARM，ARM64|
|`_InterlockedAdd_acq`|ARM，ARM64|
|`_InterlockedAdd_nf`|ARM，ARM64|
|`_InterlockedAdd_rel`|ARM，ARM64|
|`_InterlockedAdd64`|ARM，ARM64|
|`_InterlockedAdd64_acq`|ARM，ARM64|
|`_InterlockedAdd64_nf`|ARM，ARM64|
|`_InterlockedAdd64_rel`|ARM，ARM64|

**头文件** \<intrin.h>

## <a name="remarks"></a>注解

带 `_acq` 或 `_rel` 后缀的这些版本的函数可在获取或发布语义后执行互锁加法。 *获取语义* 意味着在以后的内存读取和写入之前，操作的结果将对所有线程和处理器可见。 进入临界区时，获取十分有用。 *版本语义* 表示在操作的结果使其自身可见之前，所有内存读取和写入均强制对所有线程和处理器可见。 离开临界区时，发布十分有用。 `_nf` ( "无防护" ) 后缀的内部函数不能充当内存屏障。

这些例程只能用作内部函数。

## <a name="example-_interlockedadd"></a>示例：`_InterlockedAdd`

```cpp
// interlockedadd.cpp
// Compile with: /Oi /EHsc
// processor: ARM
#include <stdio.h>
#include <intrin.h>

#pragma intrinsic(_InterlockedAdd)

int main()
{
        long data1 = 0xFF00FF00;
        long data2 = 0x00FF0000;
        long retval;
        retval = _InterlockedAdd(&data1, data2);
        printf("0x%x 0x%x 0x%x", data1, data2, retval);
}
```

## <a name="output-_interlockedadd"></a>输出：`_InterlockedAdd`

```Output
0xffffff00 0xff0000 0xffffff00
```

## <a name="example-_interlockedadd64"></a>示例：`_InterlockedAdd64`

```cpp
// interlockedadd64.cpp
// compile with: /Oi /EHsc
// processor: ARM
#include <iostream>
#include <intrin.h>
using namespace std;

#pragma intrinsic(_InterlockedAdd64)

int main()
{
        __int64 data1 = 0x0000FF0000000000;
        __int64 data2 = 0x00FF0000FFFFFFFF;
        __int64 retval;
        cout << hex << data1 << " + " << data2 << " = " ;
        retval = _InterlockedAdd64(&data1, data2);
        cout << data1 << endl;
        cout << "Return value: " << retval << endl;
}
```

## <a name="output-_interlockedadd64"></a>输出：`_InterlockedAdd64`

```Output
ff0000000000 + ff0000ffffffff = ffff00ffffffff
Return value: ffff00ffffffff
```

**结束 Microsoft 专用**

## <a name="see-also"></a>请参阅

[编译器内部函数](../intrinsics/compiler-intrinsics.md)\
[与 x86 编译器冲突](../build/x64-software-conventions.md#conflicts-with-the-x86-compiler)
