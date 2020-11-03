---
title: COM+ 1.0，ATL COM+ 1.0 组件向导
ms.date: 05/09/2019
f1_keywords:
- vc.codewiz.class.atl.mts.options
ms.assetid: 2fbe259c-6be1-4d0e-9cfe-721c75c97cb1
ms.openlocfilehash: 986e579de4d04aea4db8ab74e1e4d4c9e3263014
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92923693"
---
# <a name="com-10-atl-com-10-component-wizard"></a>COM+ 1.0，ATL COM+ 1.0 组件向导

::: moniker range="msvc-160"

此向导不适用于 Visual Studio 2019 及更高版本。

::: moniker-end

::: moniker range="<=msvc-150"

使用 ATL COM+ 1.0 组件向导的此页来指定接口类型和要支持的其他接口。

有关 ATL 项目和 ATL COM 类的详细信息，请参阅 [ATL COM 桌面组件](../../atl/atl-com-desktop-components.md)。

- **Interface**

   指示对象支持的接口类型。 默认情况下，对象支持双重接口。

   |选项|说明|
   |------------|-----------------|
   |**双**|指定对象支持双重接口（其 vtable 具有自定义接口函数和晚期绑定 `IDispatch` 方法）。 使 COM 客户端和自动化控制器都能访问对象。|
   |**自定义**|指定对象支持自定义接口（其 vtable 具有自定义接口函数）。 自定义接口的速度比双重接口更快，尤其对于跨进程边界而言。<br /><br /> - **自动化兼容** 将自动化支持添加到自定义接口。 对于特性化项目，请在组件类中设置 oleautomation 特性  。|

- **支持队列**

   指示客户端可以使用消息队列异步调用此组件。 将组件特性化宏 custom(TLBATTR_QUEUEABLE, 0) 添加到 .h 文件（特性化项目）或 .idl 文件（非特性化项目）。

- **支持**

   指示对错误处理和对象控制的其他支持。

   |选项|说明|
   |------------|-----------------|
   |**ISupportErrorInfo**|创建对 [ISupportErrorInfo](../../atl/reference/isupporterrorinfoimpl-class.md) 接口的支持，使对象可以将错误信息返回到客户端。|
   |**IObjectControl**|为对象提供对三个 [IObjectControl](/windows/win32/api/comsvcs/nn-comsvcs-iobjectcontrol) 方法的访问： [Activate](/windows/win32/api/comsvcs/nf-comsvcs-iobjectcontrol-activate)、 [CanBePooled](/windows/win32/api/comsvcs/nf-comsvcs-iobjectcontrol-canbepooled)和 [Deactivate](/windows/win32/api/comsvcs/nf-comsvcs-iobjectcontrol-deactivate)。|
   |**IObjectConstruct**|创建对 [IObjectConstruct](/windows/win32/api/comsvcs/nn-comsvcs-iobjectconstruct) 接口的支持以管理来自其他方法或对象的传入参数。|

- **事务**

   指示对象支持事务。 .idl 文件中包含文件 mtxattr.h（非特性化项目）。

   |选项|说明|
   |------------|-----------------|
   |**支持**|通过将组件特性宏 custom(TLBATTR_TRANS_SUPPORTED,0) 添加到 .h 文件（特性化项目）或 .idl 文件（非特性化项目）来指定对象永远不是事务流的根。|
   |**必需**|通过将组件特性宏 custom(TLBATTR_TRANS_REQUIRED,0) 添加到 .h 文件（特性化项目）或 .idl 文件（非特性化项目）来指定对象可能是或可能不是事务流的根。|
   |**不支持**|指定对象不包含事务。 将组件特性宏 custom(TLBATTR_TRANS_NOTSUPP,0) 添加到 .h 文件（特性化项目）或 .idl 文件（非特性化项目）。|
   |**要求新建**|通过将组件特性宏 custom(TLBATTR_TRANS_REQNEW,0) 添加到 .h 文件（特性化项目）或 .idl 文件（非特性化项目）来指定对象始终是事务流的根。|

::: moniker-end

## <a name="see-also"></a>请参阅

[ATL COM + 1.0 组件向导](../../atl/reference/atl-com-plus-1-0-component-wizard.md)<br/>
[ATL COM+ 1.0 组件](../../atl/reference/adding-an-atl-com-plus-1-0-component.md)
