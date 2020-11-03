---
title: invalid_compute_domain 类
ms.date: 11/04/2016
f1_keywords:
- invalid_compute_domain
- AMPRT/invalid_compute_domain
- AMPRT/Concurrency::invalid_compute_domain::invalid_compute_domain
helpviewer_keywords:
- invalid_compute_domain class
ms.assetid: ac7a7166-8bdb-4db1-8caf-ea129ab5117e
ms.openlocfilehash: 09418991e805e494c1d79ef31980bbec66a2e172
ms.sourcegitcommit: ced5ff1431ffbd25b20d106901955532723bd188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2020
ms.locfileid: "92135562"
---
# <a name="invalid_compute_domain-class"></a>invalid_compute_domain 类

当运行时无法通过使用在 [parallel_for_each](concurrency-namespace-functions-amp.md#parallel_for_each) 调用站点指定的计算域启动内核时引发的异常。

## <a name="syntax"></a>语法

```cpp
class invalid_compute_domain : public runtime_exception;
```

## <a name="members"></a>成员

### <a name="public-constructors"></a>公共构造函数

|“属性”|描述|
|----------|-----------------|
|[invalid_compute_domain 构造函数](#ctor)|初始化 `invalid_compute_domain` 类的新实例。|

## <a name="inheritance-hierarchy"></a>继承层次结构

`exception`

`runtime_exception`

`invalid_compute_domain`

## <a name="requirements"></a>要求

**标头：** amprt

**命名空间：** 并发

## <a name="invalid_compute_domain"></a><a name="ctor"></a> invalid_compute_domain

初始化此类的新实例。

### <a name="syntax"></a>语法

```cpp
explicit invalid_compute_domain(
    const char * _Message ) throw();

invalid_compute_domain() throw();
```

### <a name="parameters"></a>参数

*_Message*<br/>
对错误的说明。

### <a name="return-value"></a>返回值

类的实例 `invalid_compute_domain`

## <a name="see-also"></a>另请参阅

[并发命名空间 (C++ AMP) ](concurrency-namespace-cpp-amp.md)
