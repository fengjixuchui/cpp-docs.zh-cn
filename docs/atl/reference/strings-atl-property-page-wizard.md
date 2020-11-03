---
title: 字符串，ATL 属性页向导
ms.date: 05/09/2019
f1_keywords:
- vc.codewiz.class.atl.ppg.strings
helpviewer_keywords:
- ATL Property Page Wizard, strings
ms.assetid: 00547db6-911f-49eb-92e1-2ba67079d4df
ms.openlocfilehash: 61378e0aa2cee94420849195a94203be078418ff
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92921108"
---
# <a name="strings-atl-property-page-wizard"></a>字符串，ATL 属性页向导

::: moniker range="msvc-160"

ATL 属性页向导不适用于 Visual Studio 2019 及更高版本。

::: moniker-end

::: moniker range="<=msvc-150"

提供与属性页关联的文本。

- **标题**

   设置属性页的选项卡中显示的文本。

- **文档字符串**

   设置描述属性页的文本字符串。 此字符串可以显示在属性表对话框中。 属性框架可以使用状态栏或工具提示中的说明。 标准属性框架暂不使用此字符串。

- **帮助文件**

   设置用于描述如何使用属性页的帮助文件的名称。 此名称不得包含路径。 当用户按“帮助”  时，框架打开目录中的帮助文件，此目录是在 CLSID 下属性页注册表项中的 HelpDir 密钥值中指定。

::: moniker-end

## <a name="see-also"></a>请参阅

[ATL 属性页向导](../../atl/reference/atl-property-page-wizard.md)<br/>
[选项，ATL 属性页向导](../../atl/reference/options-atl-property-page-wizard.md)
