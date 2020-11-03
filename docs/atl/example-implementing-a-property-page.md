---
title: 实现属性页 (ATL)
ms.date: 05/09/2019
helpviewer_keywords:
- property pages, implementing
ms.assetid: c30b67fe-ce08-4249-ae29-f3060fa8d61e
ms.openlocfilehash: 82c2bd3765802f87025eaf2dfbda8f7467bbe9a0
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92921238"
---
# <a name="example-implementing-a-property-page"></a>示例：实现属性页

::: moniker range="msvc-160"

ATL 属性页向导不适用于 Visual Studio 2019 及更高版本。

::: moniker-end

::: moniker range="<=msvc-150"

此示例展示了如何生成属性页来显示（并允许更改）[文档类](../mfc/document-classes.md)接口的属性。

此示例基于 [ATLPages 示例](../overview/visual-cpp-samples.md)。

若要完成此示例，请执行以下操作：

- 使用 ATL 属性页向导中的“添加类”对话框来[添加 ATL 属性页类](#vcconusing_the_atl_object_wizard)。

- 通过为受关注的 `Document` 接口的属性添加新控件，[编辑对话框资源](#vcconediting_the_dialog_resource)。

- [添加消息处理程序](#vcconadding_message_handlers)，以确保属性页网站随时掌握用户所做的更改。

- 在[“保养工作”](#vcconhousekeeping)部分中，添加一些 `#import` 语句和 typedef。

- [重写 IPropertyPageImpl::SetObjects](#vcconoverriding_ipropertypageimpl_setobjects)，以验证对象是否要传递到属性页。

- [重写 IPropertyPageImpl::Activate](#vcconoverriding_ipropertypageimpl_activate)，以初始化属性页的接口。

- [重写 IPropertyPageImpl::Apply](#vcconoverride_ipropertypageimpl_apply)，以使用最新属性值来更新对象。

- 通过创建简单的帮助程序对象，[显示属性页](#vccontesting_the_property_page)。

- [创建用于测试属性页的宏](#vcconcreating_a_macro)。

## <a name="adding-the-atl-property-page-class"></a><a name="vcconusing_the_atl_object_wizard"></a> 添加 ATL 属性页类

首先，为名为“`ATLPages7`”的 DLL 服务器新建 ATL 项目。 现在使用 [ATL 属性页向导](../atl/reference/atl-property-page-wizard.md)来生成属性页。 为属性页命名“DocProperties”  作为“短名称”  ，然后切换到“字符串”  页，以设置属性页专用项，如下表所示。

|项|值|
|----------|-----------|
|标题|TextDocument|
|文档字符串|VCUE TextDocument 属性|
|帮助文件|*\<blank>*|

调用 `IPropertyPage::GetPageInfo` 时，在向导的此页上设置的值会返回给属性页容器。 此后对字符串执行的操作取决于容器，但它们通常用于向用户标识属性页。 “标题”通常显示在属性页上方的选项卡中，“文档字符串”可能显示在状态栏或工具提示中（尽管标准属性框架根本不使用此字符串）。

> [!NOTE]
> 此处设置的字符串由向导存储为项目中的字符串资源。 如果需要在生成属性页代码后更改此信息，可以使用资源编辑器轻松编辑这些字符串。

单击“确定”  ，让向导生成属性页。

## <a name="editing-the-dialog-resource"></a><a name="vcconediting_the_dialog_resource"></a> 编辑对话框资源

至此，已生成属性页。需要将一些控件添加到表示属性页的对话框资源中。 添加编辑框、静态文本控件和复选框，并设置它们的 ID，如下所示：

![编辑对话框资源](../atl/media/ppgresourcelabeled.gif "编辑对话框资源")

这些控件用于显示文档的文件名及其只读状态。

> [!NOTE]
> 对话框资源既没有框架或命令按钮，也没有你可能期望的选项卡式外观。 这些功能由属性页框架提供。例如，通过调用 [OleCreatePropertyFrame](/windows/win32/api/olectl/nf-olectl-olecreatepropertyframe) 创建的属性页框架。

## <a name="adding-message-handlers"></a><a name="vcconadding_message_handlers"></a> 添加消息处理程序

在控件就位后，可以添加消息处理程序，用于在任一控件的值更改时更新属性页的脏状态：

[!code-cpp[NVC_ATL_Windowing#73](../atl/codesnippet/cpp/example-implementing-a-property-page_1.h)]

此代码通过调用 [IPropertyPageImpl::SetDirty](../atl/reference/ipropertypageimpl-class.md#setdirty) 来通知属性页网站属性页已更改，从而响应编辑控件或复选框的更改。 属性页网站的响应方式通常为，在属性页框架上启用或禁用“应用”  按钮。

> [!NOTE]
> 在你自己的属性页中，你可能需要精确跟踪用户更改了哪些属性，以免更新尚未更改的属性。 此示例实现了相应代码，具体方式为跟踪原始属性值，并将它们与应用更改后 UI 中的当前值进行比较。

## <a name="housekeeping"></a><a name="vcconhousekeeping"></a> 保养工作

现在，将一些 `#import` 语句添加到 DocProperties.h 中，让编译器知晓 `Document` 接口：

[!code-cpp[NVC_ATL_Windowing#74](../atl/codesnippet/cpp/example-implementing-a-property-page_2.h)]

还需要引用基类，并 `IPropertyPageImpl` 将以下 **`typedef`** 内容添加到 `CDocProperties` 类：

[!code-cpp[NVC_ATL_Windowing#75](../atl/codesnippet/cpp/example-implementing-a-property-page_3.h)]

## <a name="overriding-ipropertypageimplsetobjects"></a><a name="vcconoverriding_ipropertypageimpl_setobjects"></a> 重写 IPropertyPageImpl::SetObjects

需要重写的第一个 `IPropertyPageImpl` 方法是 [SetObjects](../atl/reference/ipropertypageimpl-class.md#setobjects)。 随后将添加代码，以检查是否只传递了一个对象，且它是否支持所需的 `Document` 接口：

[!code-cpp[NVC_ATL_Windowing#76](../atl/codesnippet/cpp/example-implementing-a-property-page_4.h)]

> [!NOTE]
> 最好对此页仅支持一个对象，因为这样用户可以设置对象的文件名（也就是说，任一位置上只能有一个文件）。

## <a name="overriding-ipropertypageimplactivate"></a><a name="vcconoverriding_ipropertypageimpl_activate"></a> 重写 IPropertyPageImpl::Activate

下一步是在首次创建属性页时用基础对象的属性值初始化属性页。

在此示例中，应将以下成员添加到类中，因为当属性页的用户应用更改时，你还将使用初始属性值进行比较：

[!code-cpp[NVC_ATL_Windowing#77](../atl/codesnippet/cpp/example-implementing-a-property-page_5.h)]

[Activate](../atl/reference/ipropertypageimpl-class.md#activate) 方法的基类实现负责创建对话框及其控件，所以你可以重写此方法，并在调用基类后添加你自己的初始化：

[!code-cpp[NVC_ATL_Windowing#78](../atl/codesnippet/cpp/example-implementing-a-property-page_6.h)]

此代码使用 `Document` 接口的 COM 方法来获取你感兴趣的属性。 然后，它使用 [CDialogImpl](../atl/reference/cdialogimpl-class.md) 提供的 Win32 API 包装器及其基类，向用户显示属性值。

## <a name="overriding-ipropertypageimplapply"></a><a name="vcconoverride_ipropertypageimpl_apply"></a> 重写 IPropertyPageImpl::Apply

如果用户要将更改应用于对象，属性页网站会调用 [Apply](../atl/reference/ipropertypageimpl-class.md#apply) 方法。 此时要做的操作与 `Activate` 中的代码相反：`Activate` 是从对象获取值，并将值推送到属性页上的控件；而 `Apply` 则是从属性页上的控件获取值，并将值推送到对象。

[!code-cpp[NVC_ATL_Windowing#79](../atl/codesnippet/cpp/example-implementing-a-property-page_7.h)]

> [!NOTE]
> 此实现开头针对 [m_bDirty](../atl/reference/ipropertypageimpl-class.md#m_bdirty) 的检查是初始检查，以免在多次调用 `Apply` 时不必要地更新对象。 此外，还有针对每个属性值的检查，以确保只有更改才会导致对 `Document` 执行方法调用。

> [!NOTE]
> `Document` 将 `FullName` 公开为只读属性。 若要根据属性页更改来更新文档的文件名，必须使用 `Save` 方法来保存名称不同的文件。 因此，属性页中的代码不必将自己限制为只能获取或设置属性。

## <a name="displaying-the-property-page"></a><a name="vccontesting_the_property_page"></a> 显示属性页

若要显示此页，需要创建简单的帮助程序对象。 帮助程序对象提供可简化 `OleCreatePropertyFrame` API 的方法，用于显示连接到单一对象的一个属性页。 此帮助程序将被设计为可以在 Visual Basic 中使用。

使用[“添加类”对话框](../ide/adding-a-class-visual-cpp.md#add-class-dialog-box)和 [ATL 简单对象向导](../atl/reference/atl-simple-object-wizard.md)来生成新类，并使用“`Helper`”作为它的短名称。 创建类后，立即添加如下表所示的方法。

|项|“值”|
|----------|-----------|
|方法名|`ShowPage`|
|参数|`[in] BSTR bstrCaption, [in] BSTR bstrID, [in] IUnknown* pUnk`|

bstrCaption  参数是要显示为对话框标题的描述文字。 bstrID  参数是表示要显示的属性页的 CLSID 或编程 ID 的字符串。 pUnk  参数是属性由属性页配置的对象的 `IUnknown` 指针。

按如下所示实现方法：

[!code-cpp[NVC_ATL_Windowing#80](../atl/codesnippet/cpp/example-implementing-a-property-page_8.cpp)]

## <a name="creating-a-macro"></a><a name="vcconcreating_a_macro"></a> 创建宏

生成项目后，便能使用简单的宏来测试属性页和帮助程序对象，此宏可以在 Visual Studio 开发环境中创建和运行。 此宏创建帮助程序对象，然后使用 DocProperties  属性页的编程 ID 和 Visual Studio 编辑器中当前活动文档的 `IUnknown` 指针来调用它的 `ShowPage` 方法。 此宏所需的代码如下所示：

```vb
Imports EnvDTE
Imports System.Diagnostics

Public Module AtlPages

Public Sub Test()
    Dim Helper
    Helper = CreateObject("ATLPages7.Helper.1")

    On Error Resume Next
    Helper.ShowPage( ActiveDocument.Name, "ATLPages7Lib.DocumentProperties.1", DTE.ActiveDocument )
End Sub

End Module
```

在你运行此宏后，属性页显示，其中包括当前活动文本文档的文件名和只读状态。 文档的只读状态仅反映能否在开发环境中向文档写入内容；并不影响磁盘上文件的只读属性。

::: moniker-end

## <a name="see-also"></a>请参阅

[属性页](../atl/atl-com-property-pages.md)<br/>
[ATLPages 示例](../overview/visual-cpp-samples.md)
