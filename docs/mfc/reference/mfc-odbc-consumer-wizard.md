---
title: MFC ODBC 使用者向导
ms.date: 08/29/2019
helpviewer_keywords:
- wizards [MFC]
ms.assetid: f64a890b-a252-4887-88a1-782a7cd4ff3d
ms.openlocfilehash: c915408eede80c1564dacc88ab7b9354bd72fc11
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92918833"
---
# <a name="mfc-odbc-consumer-wizard"></a>MFC ODBC 使用者向导

::: moniker range="msvc-160"

此向导不适用于 Visual Studio 2019 及更高版本。

::: moniker-end

::: moniker range="<=msvc-150"

此向导设置 ODBC 记录集类和访问指定数据源所需的数据绑定。

## <a name="uielement-list"></a>UIElement 列表

- **数据源**

  " **数据源** " 按钮允许您使用指定的 ODBC 驱动程序设置指定的数据源。 有关 (DSN) 的数据源文件的详细信息，请参阅 ODBC SDK 中的 [文件数据源](/sql/odbc/reference/file-data-sources) 。

  " **选择数据源** " 对话框包含两个选项卡：

  - **文件数据源** 选项卡：

     " **查找范围** " 框指定要在其中选择要用作数据源的文件的目录。 默认值为 \Program Files\Common Files\ODBC\Data 源。 现有的文件数据源 ( .dsn 文件) 出现在主列表框中。 您可以使用 [ODBC 数据源管理器](/sql/odbc/admin/odbc-data-source-administrator)上的 " **文件 DSN** " 选项卡提前设置数据源，或者使用此对话框创建新的数据源。

     若要从此对话框中创建新的文件数据源，请单击 `New` 以指定 DSN 名称; 此时将显示 " **创建新数据源** " 对话框。 在 " **创建新数据源** " 对话框中，选择相应的驱动程序，然后单击 `Next` " **浏览** "，然后选择要用作数据源的文件的名称 (你必须选择 "所有文件" 才能查看非 DSN 文件（如 .xls 文件) ; 单击 `Next` ，然后单击 " **完成** "。  (如果您选择了一个非 DSN 文件，您将收到特定于驱动程序的对话框，如 "ODBC Microsoft Excel 安装程序"，它会将该文件转换为 DSN。 ) 

     > [!NOTE]
     > 还可以事先使用 ODBC 数据源管理器创建新的文件数据源。 从 " **开始** " 菜单中，选择 " **设置** "、 **"控制面板** "、" **管理工具** "、" **数据源 (Odbc)** ，然后选择" **odbc 数据源管理器** "。

     " **DSN 名称** " 框用于指定文件数据源的名称。 必须确保 DSN 名称以相应的文件扩展名（如 .xls for Excel 文件）或 .mdb （用于 Access 文件）结尾。

     有关 Dsn 的详细信息，请参阅 ODBC SDK 中的 [文件数据源](/sql/odbc/reference/file-data-sources) 。

  - " **计算机数据源** " 选项卡：

     此选项卡列出系统数据源和用户数据源。 用户数据源特定于此计算机上的用户。 此计算机上的所有用户或系统数据源可以使用系统数据源。 请参阅 ODBC SDK 中的[计算机数据源](/sql/odbc/reference/machine-data-sources)

     有关 ODBC 数据源的详细信息，请参阅 ODBC SDK 中的 [数据源](/sql/odbc/reference/data-sources) 。

  单击“确定”  以完成操作。 此时，“选择数据库对象”  对话框显示。 从此对话框中，选择使用者将使用的表或视图。 请注意，在单击项时，可以通过按住 ctrl 键来选择多个视图和表。 单击“确定”  以完成操作。

- **类**

   使用者类的名称，默认情况下，基于所选的文件或计算机数据源的名称。

- **.h 文件**

   使用者类头文件的名称，默认情况下，基于所选的文件或计算机数据源的名称。

- **.cpp 文件**

   使用者类实现文件的名称，默认情况下，基于所选的文件或计算机数据源的名称。

- **类型**

   指定记录集是否为动态集 (默认) 或快照。

  - **动态** 集：指定记录集为动态集。 动态集是查询的结果，它提供查询数据库的数据的索引视图。 动态集仅缓存原始数据的整数索引，从而为快照提供性能提升。 索引将直接指向作为查询结果找到的每个记录，并指示是否删除记录。 您还可以访问所查询记录中的已更新信息。 这是默认值。

  - **Snapshot** ：指定记录集为快照。 快照是查询的结果，在一个时间点就是数据库的一个视图。 将缓存作为查询结果发现的所有记录，因此看不到对原始记录所做的任何更改。

- **绑定所有列**

   指定是否绑定所选表中的所有列。 如果选择此框 (默认) ，则绑定所有列;如果不选择此框，则不会绑定任何列，必须在 recordset 类中手动绑定列。

::: moniker-end

## <a name="see-also"></a>请参阅

[MFC ODBC 使用](../../mfc/reference/adding-an-mfc-odbc-consumer.md)<br/>
[用代码向导添加功能](../../ide/adding-functionality-with-code-wizards-cpp.md)
