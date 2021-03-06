---
title: 数据对象和数据源：操作
ms.date: 11/04/2016
helpviewer_keywords:
- data objects [MFC], manipulating
- data sources [MFC], data operations
- data sources [MFC], inserting data
- Clipboard [MFC], determining available formats
- OLE [MFC], data objects
- Clipboard [MFC], passing format information
- data sources [MFC], determining available formats
- delayed rendering [MFC]
- OLE [MFC], data sources
ms.assetid: f7f27e77-bb5d-4131-b819-d71bf929ebaf
ms.openlocfilehash: f1a83511edbf240d9a05d6d489f6cda9453ccea9
ms.sourcegitcommit: c21b05042debc97d14875e019ee9d698691ffc0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84620403"
---
# <a name="data-objects-and-data-sources-manipulation"></a>数据对象和数据源：操作

创建数据对象或数据源之后，您可以对数据执行一些常见操作，例如插入和删除数据、枚举数据的格式等。 本文介绍完成最常见操作所需的技术。 主题包括：

- [将数据插入数据源](#_core_inserting_data_into_a_data_source)

- [确定数据对象中可用的格式](#_core_determining_the_formats_available_in_a_data_object)

- [从数据对象检索数据](#_core_retrieving_data_from_a_data_object)

## <a name="inserting-data-into-a-data-source"></a><a name="_core_inserting_data_into_a_data_source"></a>将数据插入数据源

如何将数据插入数据源取决于数据是立即提供，还是按需提供，以及在哪个介质上提供。 可能的情况如下所示。

### <a name="supplying-data-immediately-immediate-rendering"></a>立即提供数据（即时呈现）

- `COleDataSource::CacheGlobalData`为提供数据的每个剪贴板格式重复调用。 传递要使用的剪贴板格式，它是包含数据的内存的句柄，还可以是描述数据的**FORMATETC**结构（可选）。

     \- 或 -

- 如果要直接使用**STGMEDIUM**结构，请在 `COleDataSource::CacheData` `COleDataSource::CacheGlobalData` 上面的选项中调用而不是。

### <a name="supplying-data-on-demand-delayed-rendering"></a>按需提供数据（延迟渲染）

这是一个高级主题。

- `COleDataSource::DelayRenderData`为提供数据的每个剪贴板格式重复调用。 传递要使用的剪贴板格式，以及描述数据的**FORMATETC**结构（可选）。 请求数据时，框架将调用 `COleDataSource::OnRenderData` ，必须重写。

     \- 或 -

- 如果使用 `CFile` 对象来提供数据，请调用 `COleDataSource::DelayRenderFileData` 而不是 `COleDataSource::DelayRenderData` 在上一选项中调用。 请求数据时，框架将调用 `COleDataSource::OnRenderFileData` ，必须重写。

## <a name="determining-the-formats-available-in-a-data-object"></a><a name="_core_determining_the_formats_available_in_a_data_object"></a>确定数据对象中可用的格式

在应用程序允许用户将数据粘贴到其中之前，需要知道剪贴板上是否有可处理的格式。 为此，应用程序应执行以下操作：

1. 创建 `COleDataObject` 对象和**FORMATETC**结构。

1. 调用数据对象的 `AttachClipboard` 成员函数，将数据对象与剪贴板上的数据相关联。

1. 执行下列操作之一：

   - `IsDataAvailable`如果只有一种或两种格式需要，则调用数据对象的成员函数。 这可以节省时间，因为剪贴板上的数据比应用程序所支持的格式要大得多。

     \- 或 -

   - 调用数据对象的 `BeginEnumFormats` 成员函数，开始枚举剪贴板上可用的格式。 然后调用 `GetNextFormat` ，直到剪贴板返回应用程序支持的格式，或者没有其他格式。

如果使用**ON_UPDATE_COMMAND_UI**，现在可以在 "编辑" 菜单中启用 "粘贴" 和 "粘贴"。 为此，请调用 `CMenu::EnableMenuItem` 或 `CCmdUI::Enable` 。 有关容器应用程序应如何处理菜单项和时间的详细信息，请参阅[菜单和资源：容器添加](menus-and-resources-container-additions.md)内容。

## <a name="retrieving-data-from-a-data-object"></a><a name="_core_retrieving_data_from_a_data_object"></a>从数据对象检索数据

一旦您决定了数据格式后，就会继续从数据对象中检索数据。 若要执行此操作，用户需要决定数据的放置位置，应用程序将调用相应的函数。 数据将在以下其中一个介质中提供：

|中型|要调用的函数|
|------------|----------------------|
|全局内存（ `HGLOBAL` ）|`COleDataObject::GetGlobalData`|
|文件（ `CFile` ）|`COleDataObject::GetFileData`|
|**STGMEDIUM**结构（ `IStorage` ）|`COleDataObject::GetData`|

通常，介质将连同其剪贴板格式一起指定。 例如， **CF_EMBEDDEDSTRUCT**对象始终位于 `IStorage` 需要**STGMEDIUM**结构的介质中。 因此，您可以使用， `GetData` 因为它是可以接受**STGMEDIUM**结构的两个函数中的一个。

对于剪贴板格式为 `IStream` 或 medium 的情况 `HGLOBAL` ，框架可以提供 `CFile` 引用数据的指针。 然后，应用程序可以使用文件读取来获取数据，其方式与从文件导入数据的方式几乎相同。 实质上，这是 `OnRenderData` 数据源中的和例程的客户端接口 `OnRenderFileData` 。

用户现在可以像对任何其他数据一样，在文档中插入数据。

### <a name="what-do-you-want-to-know-more-about"></a>要了解有关的详细信息

- [拖放](drag-and-drop-ole.md)

- [剪贴板](clipboard.md)

## <a name="see-also"></a>另请参阅

[数据对象和数据源（OLE）](data-objects-and-data-sources-ole.md)<br/>
[COleDataObject 类](reference/coledataobject-class.md)<br/>
[COleDataSource 类](reference/coledatasource-class.md)
