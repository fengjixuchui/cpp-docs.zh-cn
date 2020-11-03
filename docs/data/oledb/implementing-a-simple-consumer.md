---
title: 实现简单使用者
ms.date: 08/19/2019
helpviewer_keywords:
- OLE DB consumers, implementing
ms.assetid: 13828167-23a4-4e94-8b6c-878262fda464
ms.openlocfilehash: 2d59989f8afd180b39153eed1ad0a20435aad9d4
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92923905"
---
# <a name="implementing-a-simple-consumer"></a>实现简单使用者

::: moniker range="msvc-160"

ATL OLE DB 使用者向导不适用于 Visual Studio 2019 及更高版本。 但仍可以手动添加此功能。 有关详细信息，请参阅[不使用向导创建使用者](creating-a-consumer-without-using-a-wizard.md)。

::: moniker-end

::: moniker range="<=msvc-150"

下面各主题介绍了如何通过编辑 MFC 应用程序向导  和 ATL OLE DB 使用者向导  创建的文件来创建简单使用者。 此示例包含以下部分：

- [通过使用者检索数据](#retrieve)展示了如何在使用者中实现从数据库表中逐行读取所有数据的代码。

- [向使用者添加书签支持](#bookmark)展示了如何向使用者添加书签支持。

> [!NOTE]
> 可以使用此部分中描述的使用者应用程序来测试 `MyProv` 和 `Provider` 示例提供程序。

> [!NOTE]
> 若要生成使用者应用程序来测试 `MyProv`（[增强简单只读提供程序](../../data/oledb/enhancing-the-simple-read-only-provider.md)中所述的相同提供程序），必须添加书签支持，如[向使用者添加书签支持](#bookmark)中所述。

## <a name="retrieving-data-with-the-consumer"></a><a name="retrieve" ></a> 通过使用者检索数据

### <a name="to-modify-the-console-application-to-use-the-ole-db-consumer"></a>将控制台应用程序修改为使用 OLE DB 使用者的具体步骤

1. 在 `MyCons.cpp` 中，通过插入如下粗体文本来更改主代码：

    ```cpp
    // MyCons.cpp : Defines the entry point for the console application.
    //
    #include "pch.h" // "stdafx.h" in Visual Studio 2017 and earlier
    #include "Products.h"
    ...
    int main(int argc, char* argv[])
    {
       HRESULT hr = CoInitialize(NULL);   // Instantiate rowset
       CProducts rs;
       hr = rs.OpenAll();
       ATLASSERT(SUCCEEDED(hr ) );
       hr = rs.MoveFirst();   // Iterate through the rowset
       while(SUCCEEDED(hr) && hr != DB_S_ENDOFROWSET )   {      // Print out the column information for each row
         printf("Product ID: %d, Name: %s, Unit Price: %d, Quantity per Unit: %d, Units in Stock %d, Reorder Level %d\n",
           rs.m_ProductID, rs.m_ProductName, rs.m_UnitPrice, rs.m_QuantityPerUnit, rs.m_UnitsInStock, rs.m_ReorderLevel );
         hr = rs.MoveNext();   }
       rs.Close();
       rs.ReleaseCommand();
       CoUninitialize();

       return 0;
    }
    ```

## <a name="adding-bookmark-support-to-the-consumer"></a><a name="bookmark" ></a> 向使用者添加书签支持

书签是唯一标识表中行的列。 它通常是键列，但并不总是这样；它是提供程序专用列。 此部分介绍了如何添加书签支持。 为此，需要在用户记录类中执行以下步骤：

- 实例化书签。 这些是 [CBookmark](../../data/oledb/cbookmark-class.md) 类型的对象。

- 通过设置 `DBPROP_IRowsetLocate` 属性，从提供程序请求获取书签列。

- 使用 [BOOKMARK_ENTRY](./macros-and-global-functions-for-ole-db-consumer-templates.md#bookmark_entry) 宏将书签条目添加到列映射中。

执行上面的步骤可以获取书签支持，以及要处理的书签对象。 此代码示例展示了书签，如下所示：

- 打开文件以写入内容。

- 将行集数据逐行输出到文件行。

- 通过调用 [MoveToBookmark](./crowset-class.md#movetobookmark)，将行集光标移到书签。

- 输出已添加书签的行，同时将它追加到文件末尾。

> [!NOTE]
> 如果使用此使用者应用程序来测试 `Provider` 示例提供程序应用程序，请忽略此部分中介绍的书签支持。

### <a name="to-instantiate-the-bookmark"></a>实例化书签的具体步骤

1. 取值函数需要保留 [CBookmark](../../data/oledb/cbookmark-class.md) 类型的对象。 nSize  参数以字节为单位指定书签缓冲区的大小（对于 32 位平台，通常为 4 字节；对于 64 位平台，通常为 8 字节）。 将以下声明添加到用户记录类中的列数据成员：

    ```cpp
    //////////////////////////////////////////////////////////////////////
    // Products.h
    class CProductsAccessor
    {
    public:
       CBookmark<4> m_bookmark;   // Add bookmark declaration
       LONG m_ProductID;
       ...
    ```

### <a name="to-request-a-bookmark-column-from-the-provider"></a>从提供程序请求获取书签列的具体步骤

1. 在用户记录类的 `GetRowsetProperties` 方法中添加以下代码：

    ```cpp
    // Set the DBPROP_IRowsetLocate property.
    void GetRowsetProperties(CDBPropSet* pPropSet)
    {
       pPropSet->AddProperty(DBPROP_CANFETCHBACKWARDS, true, DBPROPOPTIONS_OPTIONAL);
       pPropSet->AddProperty(DBPROP_CANSCROLLBACKWARDS, true, DBPROPOPTIONS_OPTIONAL);
       // Add DBPROP_IRowsetLocate property to support bookmarks   pPropSet->AddProperty(DBPROP_IRowsetLocate, true);
    }
    ```

### <a name="to-add-a-bookmark-entry-to-the-column-map"></a>将书签条目添加到列映射中的具体步骤

1. 将以下条目添加到用户记录类的列映射中：

    ```cpp
    // Set a bookmark entry in the column map.
    BEGIN_COLUMN_MAP(CProductsAccessor)
       BOOKMARK_ENTRY(m_bookmark)   // Add bookmark entry
       COLUMN_ENTRY_LENGTH_STATUS(1, m_ProductID, m_dwProductIDLength, m_dwProductIDStatus)
       COLUMN_ENTRY_LENGTH_STATUS(2, m_ProductName, m_dwProductNameLength, m_dwProductNameStatus)
    ...
    END_COLUMN_MAP()
    ```

### <a name="to-use-a-bookmark-in-your-main-code"></a>在主代码中使用书签的具体步骤

1. 在之前创建的控制台应用程序的 `MyCons.cpp` 文件中，将主代码更改为如下所示。 若要使用书签，主代码必须实例化它自己的书签对象 (`myBookmark`)；此书签不同于取值函数中的书签 (`m_bookmark`)。

    ```cpp
    ///////////////////////////////////////////////////////////////////////
    // MyCons.cpp : Defines the entry point for the console application.
    //

    #include "stdafx.h"
    #include "Products.h"
    #include <iostream>
    #include <fstream>
    using namespace std;

    int _tmain(int argc, _TCHAR* argv[])
    {
       HRESULT hr = CoInitialize(NULL);

       // Instantiate rowset
       CProducts rs;

       hr = rs.OpenAll();
       hr = rs.MoveFirst();

       // Cast CURRENCY m_UnitPrice to a long value
       LONGLONG lPrice = rs.m_UnitPrice.int64;

       // Open file output.txt for writing in overwrite mode
       ofstream outfile( "C:\\output.txt", ios::out );

       if (!outfile)      // Test for invalid file
          return -1;

       // Instantiate a bookmark object myBookmark for the main code
       CBookmark<4> myBookmark;
       int nCounter = 0;

       // Iterate through the rowset and output column data to output.txt row by row
       // In the file, mark the beginning of this set of data:
       outfile << "initial row dump" << endl;
       while(SUCCEEDED(hr) && hr != DB_S_ENDOFROWSET )
       {
          nCounter++;
          if(nCounter == 5 )
             myBookmark = rs.m_bookmark;
          // Output the column information for each row:
          outfile << rs.m_ProductID << rs.m_ProductName << lPrice << rs.m_QuantityPerUnit << rs.m_UnitsInStock << rs.m_ReorderLevel << endl;
          hr = rs.MoveNext();
       }

       // Move cursor to bookmark
       hr = rs.MoveToBookmark(myBookmark);

       // Iterate through the rowset and output column data to output.txt row by row
       // In the file, mark the beginning of this set of data:
       outfile << "row dump starting from bookmarked row" << endl;
       while(SUCCEEDED(hr) && hr != DB_S_ENDOFROWSET )
       {
          // Output the column information for each row
          outfile << rs.m_ProductID << rs.m_ProductName << lPrice << rs.m_QuantityPerUnit << rs.m_UnitsInStock << rs.m_ReorderLevel << endl;
          hr = rs.MoveNext();
       }

       rs.CloseAll();
       CoUninitialize();

       return 0;
    }
    ```

若要详细了解书签，请参阅[使用书签](../../data/oledb/using-bookmarks.md)。 [更新行集](../../data/oledb/updating-rowsets.md)中还收录了书签示例。

::: moniker-end

## <a name="see-also"></a>请参阅

[使用向导创建 OLE DB 使用者](../../data/oledb/creating-an-ole-db-consumer-using-a-wizard.md)
