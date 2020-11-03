---
title: '&lt;random&gt; 函数'
ms.date: 09/04/2019
f1_keywords:
- random/std::generate_canonical
ms.assetid: 2ac9ec59-619b-4b85-a425-f729277c1bc8
helpviewer_keywords:
- std::generate_canonical
ms.openlocfilehash: 3d94f607fc6b7bdf22d7f573f590b451dbaa718d
ms.sourcegitcommit: 19016630f9d35f365e9ba249e0f3617515d7ca33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92274592"
---
# <a name="ltrandomgt-functions"></a>&lt;random&gt; 函数

## <a name="generate_canonical"></a><a name="generate_canonical"></a> generate_canonical

从随机序列返回浮点值。

```cpp
template <class RealType, size_t Bits, class Generator>
RealType generate_canonical(Generator& Gen);
```

### <a name="parameters"></a>参数

*RealType*\
浮点整型。 有关可能的类型，请参阅 [\<random>](../standard-library/random.md) 。

*带宽*\
要使用的随机性位数。

*生成器*\
随机数生成器类。

*常规*\
对类型 *生成器*的随机数生成器的实例的引用。

### <a name="remarks"></a>备注

模板函数重复调用 `operator()` 了*Gen* ，并将返回的值打包为 RealType 类型的浮点值， `x` 直到它在中收集了指定数目的尾数位*RealType* `x` 。 指定的数字是 *位数* 较小的 (，其中必须为非零) ，以及 *RealType*中的尾数位的完整数目。 第一个调用提供最低序位。 该函数返回 `x`。
