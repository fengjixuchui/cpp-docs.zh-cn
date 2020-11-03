---
title: ATL OLE DB 提供程序向导
ms.date: 05/09/2019
helpviewer_keywords:
- ATL projects, adding ATL OLE DB providers
ms.assetid: cf91ba78-01d1-4d12-b673-e95d96bfbebe
ms.openlocfilehash: 43b8ed4507b004f1e78bc1b9dda64c44ff56e1d7
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92921134"
---
# <a name="atl-ole-db-provider-wizard"></a>ATL OLE DB 提供程序向导

::: moniker range="msvc-160"

此向导不适用于 Visual Studio 2019 及更高版本。

::: moniker-end

::: moniker range="<=msvc-150"

## <a name="remarks"></a>注解

自 Visual Studio 2008 起，此向导生成的注册脚本在 HKEY_CURRENT_USER  （而不是 HKEY_LOCAL_MACHINE  ）下注册它的 COM 组件。 若要修改此行为，请设置 ATL 向导的“为所有用户注册组件”  选项。

下面列出并介绍了 ATL OLE DB 提供程序向导的选项：

- **短名称**

   键入要创建的提供程序的短名称。 向导中的其他编辑框会根据你在此处键入的内容自动填充。 如果需要，可以编辑其他名称框。

- **Coclass**

   组件类的名称。 “编程 ID”名称会更改来与此名称匹配。

- **特性化**

   此选项指定向导是使用特性还是使用模板声明来创建提供程序类。 如果你选中此选项，向导使用的是特性，而不是模板声明（这是在创建了特性化项目时的默认选项）。 如果你取消选中此选项，向导使用的是模板声明，而不是特性（这是在创建了非特性化项目时的默认选项）。

   如果你在创建了非特性化项目时选中此选项，向导警告你项目会转换为特性化项目，并询问你是否继续。

- **ProgID**

   编程 ID（或编程标识符）是应用程序可用来代替 GUID 的文本字符串。 “编程 ID”名称采用 Projectname.Coclassname  形式。

- **版本**

   提供程序的版本号。 默认值为 1。

- **DataSource 类**

   数据源类的名称，采用 CShortname  Source形式。

- **数据源 .h 文件**

   数据源类的头文件。 可以编辑此文件名，也可以选择现有头文件。

- **会话类**

   会话类的名称，采用 CShortname  Session 形式。

- **会话 .h 文件**

   会话类的头文件。 可以编辑此文件名，也可以选择现有头文件。

- **命令类**

   命令类的名称，采用 CShortname  Command 形式。

- **命令 .h 文件**

   命令类的头文件。 此名称无法编辑，它依赖行集头文件的名称。

- **行集类**

   行集类的名称，采用 CShortname  Rowset 形式。

- **行集 .h 文件**

   行集类的头文件。 可以编辑此文件名，也可以选择现有头文件。

- **行集 .cpp 文件**

   提供程序的实现文件。 可以编辑此文件名，也可以选择现有实现文件。

::: moniker-end

## <a name="see-also"></a>请参阅

[ATL OLE DB 提供程序](../../atl/reference/adding-an-atl-ole-db-provider.md)
