---
title: 实现属性页
ms.date: 11/04/2016
helpviewer_keywords:
- IPropertyPage2 class
- IPropertyPage class
- property pages, implementing
ms.assetid: 62f29440-33a7-40eb-a1ef-3634c95f640c
ms.openlocfilehash: c9b1c0c159a15ec7d9a5c99208baa2ed6af2d6d1
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92923898"
---
# <a name="implementing-property-pages"></a>实现属性页

::: moniker range="msvc-160"

ATL 属性页向导不适用于 Visual Studio 2019 及更高版本。

::: moniker-end

::: moniker range="<=msvc-150"

属性页是实现 `IPropertyPage` 或 `IPropertyPage2` 接口的 COM 对象。 ATL 支持通过 [ATL 属性页向导](../atl/reference/atl-property-page-wizard.md)中的[“添加类”对话框](../ide/adding-a-class-visual-cpp.md#add-class-dialog-box)实现属性页。

若要使用 ATL 创建属性页，请执行以下操作：

- 创建或打开 ATL 动态链接库 (DLL) 服务器项目。

- 打开  。

- 请确保属性页已单元线程化（因为它有用户界面）。

- 设置要与属性页关联的标题、说明（文档字符串）和帮助文件。

- 将控件添加到生成的对话框资源中，以用作属性页的用户界面。

- 响应属性页中用户界面的更改，以执行验证、更新属性页网站或更新与属性页关联的对象。 特别是，当用户更改属性页时，调用 [IPropertyPageImpl::SetDirty](../atl/reference/ipropertypageimpl-class.md#setdirty)。

- （可选）遵循以下指导原则来重写 `IPropertyPageImpl` 方法。

   |IPropertyPageImpl 方法|需要执行以下操作时重写...|说明|
   |------------------------------|----------------------------------|-----------|
   |[SetObjects](../atl/reference/ipropertypageimpl-class.md#setobjects)|对要传递到属性页及其支持接口的对象数量执行基本的完整性检查。|在调用基类实现前执行你自己的代码。 如果要设置的对象不符合预期，应尽快让调用失效。|
   |[激活](../atl/reference/ipropertypageimpl-class.md#activate)|初始化属性页的用户界面（例如，使用对象中的当前属性值来设置对话框控件、动态创建控件或执行其他初始化）。|在执行代码前调用基类实现，这样基类就有机会在你尝试更新前创建对话框窗口和所有控件。|
   |[应用](../atl/reference/ipropertypageimpl-class.md#apply)|验证属性设置，并更新对象。|无需调用基类实现，因为它除了跟踪调用之外什么都不做。|
   |[停用](../atl/reference/ipropertypageimpl-class.md#deactivate)|清理与窗口相关的项。|基类实现销毁表示属性页的对话框。 如果需要在销毁对话框前先进行清理，应在调用基类前添加代码。|

有关属性页实现的示例，请参见 [示例：实现属性页](../atl/example-implementing-a-property-page.md)。

> [!NOTE]
> 若要在属性页中托管 ActiveX 控件，需要更改向导生成的类的派生内容。 在基类列表中将 **CDialogImpl \<CYourClass>** 替换为 **CAxDialogImpl \<CYourClass>** 。

::: moniker-end

## <a name="see-also"></a>请参阅

[属性页](../atl/atl-com-property-pages.md)<br/>
[ATLPages 示例](../overview/visual-cpp-samples.md)
