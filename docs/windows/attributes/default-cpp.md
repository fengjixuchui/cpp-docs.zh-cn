---
title: '默认 (c + + COM 特性) '
ms.date: 10/02/2018
f1_keywords:
- vc-attr.default
helpviewer_keywords:
- default attribute
- attributes [C#], default attribute
- defaults, default attribute
ms.assetid: 0cdca716-1ba8-46d7-9399-167e55492870
ms.openlocfilehash: b53420d721b43f9a51b19c4cc8e4a83fc8b94ed4
ms.sourcegitcommit: ec6dd97ef3d10b44e0fedaa8e53f41696f49ac7b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2020
ms.locfileid: "88842437"
---
# <a name="default-c"></a>default (C++)

指示组件类中定义的自定义接口或调度接口表示默认的可编程性接口。

## <a name="syntax"></a>语法

```cpp
[ default(interface1, interface2) ]
```

### <a name="parameters"></a>参数

*interface1*<br/>
默认接口，将可用于根据使用特性定义的类创建对象的脚本环境 **`default`** 。

如果未指定默认接口，则第一个出现的非源接口用作默认接口。

*interface2*<br/>
 (可选) 默认源接口。 你还必须使用 [source](source-cpp.md) 属性指定此接口。

如果未指定默认源接口，则第一个源接口用作默认接口。

## <a name="remarks"></a>注解

**`default`** C + + 特性具有与[默认](/windows/win32/Midl/default)MIDL 特性相同的功能。 该 **`default`** 属性还与 [case](case-cpp.md) 属性结合使用。

## <a name="example"></a>示例

下面的代码演示如何 **`default`** 在组件类的定义中使用来将指定 `ICustomDispatch` 为默认可编程性接口：

```cpp
// cpp_attr_ref_default.cpp
// compile with: /LD
#include "windows.h"
[module(name="MyLibrary")];

[object, uuid("9E66A290-4365-11D2-A997-00C04FA37DDB")]
__interface ICustom {
   HRESULT Custom([in] long l, [out, retval] long *pLong);
};

[dual, uuid("9E66A291-4365-11D2-A997-00C04FA37DDB")]
__interface IDual {
   HRESULT Dual([in] long l, [out, retval] long *pLong);
};

[object, uuid("9E66A293-4365-11D2-A997-00C04FA37DDB")]
__interface ICustomDispatch : public IDispatch {
   HRESULT Dispatch([in] long l, [out, retval] long *pLong);
};

[   coclass, default(ICustomDispatch), source(IDual), uuid("9E66A294-4365-11D2-A997-00C04FA37DDB")
]
class CClass : public ICustom, public IDual, public ICustomDispatch {
   HRESULT Custom(long l, long *pLong) { return(S_OK); }
   HRESULT Dual(long l, long *pLong) { return(S_OK); }
   HRESULT Dispatch(long l, long *pLong) { return(S_OK); }
};

int main() {
#if 0 // Can't instantiate without implementations of IUnknown/IDispatch
   CClass *pClass = new CClass;

   long llong;

   pClass->custom(1, &llong);
   pClass->dual(1, &llong);
   pClass->dispinterface(1, &llong);
   pClass->dispatch(1, &llong);

   delete pClass;
#endif
   return(0);
}
```

[Source](source-cpp.md)属性还提供了有关如何使用的示例 **`default`** 。

## <a name="requirements"></a>要求

| 特性上下文 | “值” |
|-|-|
|**适用于**|**`class`**、 **`struct`** 、数据成员|
|**且**|否|
|**必需属性**|**coclass**当应用于 **`class`** 或) 时，组件类 (**`struct`**|
|**无效的特性**|无|

有关详细信息，请参见 [特性上下文](cpp-attributes-com-net.md#contexts)。

## <a name="see-also"></a>另请参阅

[IDL 特性](idl-attributes.md)<br/>
[类特性](class-attributes.md)<br/>
[coclass](coclass.md)
