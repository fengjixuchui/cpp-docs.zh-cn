---
title: 创建提供程序
ms.date: 05/09/2019
helpviewer_keywords:
- OLE DB providers, creating
ms.assetid: 2506ba8f-010d-4231-aac1-387432f7b6b9
ms.openlocfilehash: 5a24245ae19fc6fa2a66d4bf102765b712b4cf5c
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92921498"
---
# <a name="creating-the-provider"></a>创建提供程序

::: moniker range="msvc-160"

ATL OLE DB 提供程序向导不适用于 Visual Studio 2019 及更高版本。

::: moniker-end

::: moniker range="<=msvc-150"

## <a name="to-create-an-ole-db-provider-with-the-atl-ole-db-provider-wizard"></a>使用 ATL OLE DB 提供程序向导创建 OLE DB 提供程序的具体步骤

1. 右键单击项目。

1. 在快捷菜单上，单击 " **添加** "，然后单击 " **添加类** "。

1. 在“添加类”  对话框中，依次转到“已安装”  下，选择“ATL OLEDB 提供程序”  图标，再单击“打开”  。

1. 在 ATL OLE DB 提供程序向导  的“短名称”  框中，输入提供程序的短名称。 下面的主题使用短名称“Custom”  ，但你可以使用其他名称。 其他名称框会根据你输入的名称进行填充。

1. 如果需要，编辑其他名称框。 除了对象和文件名外，还可以编辑以下内容：

   - **Coclass** ： COM 用于创建提供程序的名称。

   - **ProgID** ：编程标识符，它是可用于代替 GUID 的文本字符串。

   - **版本** ：与 ProgID 和 Coclass 一起使用，生成依赖于版本的编程 ID。

1. 单击“完成”。

::: moniker-end

## <a name="see-also"></a>请参阅

[创建 OLE DB 提供程序](../../data/oledb/creating-an-ole-db-provider.md)
