---
title: 提供程序向导生成的文件
ms.date: 05/09/2019
helpviewer_keywords:
- OLE DB providers, wizard-generated files
ms.assetid: 6e1ac94b-eb90-4abf-82b3-06944b947ebc
ms.openlocfilehash: 3bc680e5999c077dda384823ec4f67d2456af284
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924329"
---
# <a name="provider-wizard-generated-files"></a>提供程序向导生成的文件

::: moniker range="msvc-160"

ATL OLE DB 提供程序向导不适用于 Visual Studio 2019 及更高版本。

::: moniker-end

::: moniker range="<=msvc-150"

ATL OLE DB 提供程序向导  生成以下文件。 下面的主题使用短名称“Custom”  ，但确切文件名具体视你在创建提供程序时所做的选择而定。

|文件名|说明|
|---------------|-----------------|
|Custom  RS.cpp|包含命令帮助程序 `Execute` 方法和提供程序列映射。|
|Custom  DS.h|实现数据源对象。 此头文件包含数据源属性的属性映射。|
|Custom  RS.h|实现命令和行集对象。 此头文件包含行集和命令属性的属性映射。|
|Custom  Sess.h|实现会话对象。 此头文件包含会话属性的属性映射。|
|Custom  .rgs|包含 OLE DB 提供程序向导  生成的已注册对象。|

::: moniker-end

## <a name="see-also"></a>请参阅

[创建 OLE DB 提供程序](../../data/oledb/creating-an-ole-db-provider.md)<br/>
