---
title: ATL 属性页向导
ms.date: 05/09/2019
f1_keywords:
- vc.codewiz.class.atl.ppg.overview
helpviewer_keywords:
- ATL projects, adding property pages
- ATL Property Page Wizard
ms.assetid: 6113e325-facd-4f68-b491-144d75209922
ms.openlocfilehash: 85de65dc1b18ee5c2e62388e57b0199ed64cee3f
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92921121"
---
# <a name="atl-property-page-wizard"></a>ATL 属性页向导

::: moniker range="msvc-160"

此向导不适用于 Visual Studio 2019 及更高版本。

::: moniker-end

::: moniker range="<=msvc-150"

此向导[将属性页添加到 ATL 项目](../../atl/reference/adding-an-atl-property-page.md)，或添加到支持 ATL 的 MFC 项目。 ATL 属性页提供用户界面，用于设置一个或多个 COM 对象的属性，或用于调用一个或多个 COM 对象的方法。

## <a name="remarks"></a>注解

自 Visual Studio 2008 起，此向导生成的注册脚本在 HKEY_CURRENT_USER  （而不是 HKEY_LOCAL_MACHINE  ）下注册它的 COM 组件。 若要修改此行为，请设置 ATL 向导的“为所有用户注册组件”  选项。

## <a name="names"></a>名称

命名要添加到项目中的对象、接口和类。 除了“短名称”  外，其他所有框都可以独立编辑。 如果你更改“短名称”  的文本，更改会反映在此页中其他所有框的名称中。 如果你更改 COM 部分中的“组件类”  名称，更改会反映在“类型”  和“编程 ID”  框中。 此命名行为旨在方便你在开发属性页时可轻松识别所有名称。

> [!NOTE]
> “组件类”  仅对非特性化项目可编辑。 如果项目已特性化，便无法编辑“组件类”  。

### <a name="c"></a>C++

提供为了实现对象而创建的 C++ 类的信息。

|术语|定义|
|-|-|
|**短名称**|设置对象的缩写名称。 你提供的名称决定了“类”和“组件类”  名称、“.cpp 文件”  和“.h 文件”  名称、“类型”  名称和“编程 ID”  ，除非你单独更改这些字段。|
|**.h 文件**|为新项目的类的头文件设置名称。 默认情况下，此名称以你在“短名称”  中提供的名称为依据。 单击省略号按钮以将该文件名保存至所选择的位置，或追加到某个现有文件的类声明中。 如果你选择现有文件，除非你单击向导中的“完成”  ，否则向导不会将文件保存到选定位置。<br /><br /> 向导不会覆盖文件。 如果选择现有文件的名称，当你单击“完成”时，向导会询问你是否要将该类声明追加至文件的内容中  。 单击“是”，则追加该文件；单击“否”，则返回至向导并指定另一个文件名  。|
|**类**|设置用于实现对象的类的名称。 此名称以你在“短名称”  中提供的名称为依据，跟在典型的类名前缀“C”后面。|
|**.cpp 文件**|为新项目的类的实现文件设置名称。 默认情况下，此名称以你在“短名称”  中提供的名称为依据。 单击省略号按钮以将文件名保存到所选位置。 向导在你单击“完成”之前不会将该文件保存到所选位置  。<br /><br /> 向导不会覆盖文件。 如果选择现有文件的名称，当你单击“完成”时，向导会询问你是否要将该类实现追加至文件的内容中  。 单击“是”，则追加该文件；单击“否”，则返回至向导并指定另一个文件名  。|
|**特性化**|指明对象是否使用特性。 若要将对象添加到特性化 ATL 项目，在选中此选项后便无法更改；也就是说，只能将特性化对象添加到创建的支持特性的项目中。<br /><br /> 只能将特性化对象添加到使用特性的 ATL 项目中。 如果你对不支持特性的 ATL 项目选中此选项，向导会提示你指定是否要向项目添加特性支持。<br /><br /> 默认情况下，在设置此选项后添加的任何对象都被指定为特性化对象（复选框处于选中状态）。 可以取消选中此框来添加不使用特性的对象。<br /><br /> 有关详细信息，请参阅 [应用程序设置，ATL 项目向导](../../atl/reference/application-settings-atl-project-wizard.md) 和 [属性的基本机制](../../windows/attributes/cpp-attributes-com-net.md#basic-mechanics-of-attributes) 。|

### <a name="com"></a>COM

提供对象的 COM 功能的相关信息。

- **Coclass**

   设置包含对象支持的接口列表的组件类的名称。

   > [!NOTE]
   > 如果使用特性创建项目，或在此向导页上指明属性页使用特性，便无法更改这一选项，因为 ATL 不包括 `coclass` 特性。

- **类型**

   设置注册表中显示的对象说明

- **ProgID**

   设置容器可用来代替对象 CLSID 的名称。

::: moniker-end

## <a name="see-also"></a>请参阅

[选项，ATL 属性页向导](../../atl/reference/options-atl-property-page-wizard.md)<br/>
[ATL 属性页向导的“字符串”](../../atl/reference/strings-atl-property-page-wizard.md)<br/>
[示例：实现属性页](../../atl/example-implementing-a-property-page.md)
