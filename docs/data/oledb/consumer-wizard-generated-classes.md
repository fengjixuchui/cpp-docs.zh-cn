---
title: 使用者向导生成的类
ms.date: 05/09/2019
helpviewer_keywords:
- user record classes in OLE DB consumer
ms.assetid: dba0538f-2afe-4354-8cbb-f202ea8ade5a
ms.openlocfilehash: 5c8a975b2f4a8ae3285f035e7b708c5026f5f480
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924593"
---
# <a name="consumer-wizard-generated-classes"></a>使用者向导生成的类

::: moniker range="msvc-160"

ATL OLE DB 使用者向导不适用于 Visual Studio 2019 及更高版本。 但仍可以手动添加此功能。

::: moniker-end

::: moniker range="<=msvc-150"

使用 ATL OLE DB 使用者向导  生成使用者时，可以选择是使用 OLE DB 模板，还是使用 OLE DB 特性。 在这两种情况下，向导将分别生成命令类和用户记录类。 命令类包含用于打开在向导中指定的数据源和行集的代码。 用户记录类包含选定数据库表的列映射。 但是，这两种情况下生成的代码各不相同：

- 如果选择模板化使用者，则向导将生成命令类和用户记录类。 命令类以你在向导的“类”  框中输入的名称命名（例如，`CProducts`），用户记录类采用“ClassName  Accessor”格式的名称（例如，`CProductsAccessor`）。 这两个类都位于使用者的头文件中。

- 如果选择属性化使用者，则用户记录类将具有“_ *类名* Accessor”格式的名称，并将插入。 也就是说，只能在文本编辑器中查看命令类，并只能以插入代码的形式查看用户记录类。 有关查看插入代码的信息，请参阅 [调试插入代码](/visualstudio/debugger/how-to-debug-injected-code)。

下面的示例使用对 `Northwind` 数据库的 `Products` 表创建的命令类，展示了用于命令类和用户记录类的向导生成的使用者代码。

## <a name="templated-user-record-classes"></a>模板化用户记录类

如果使用 OLE DB 模板（而不是 OLE DB 属性）创建 OLE DB 使用者，则向导将生成本部分所述的代码。

### <a name="column-data-members"></a>列数据成员

用户记录类的第一部分包括数据成员声明以及每个数据绑定列的状态和长度数据成员。 有关这些成员的信息，请参阅 [向导生成的取值函数中的字段状态数据成员](../../data/oledb/field-status-data-members-in-wizard-generated-accessors.md)。

> [!NOTE]
> 如果修改用户记录类或编写自己的使用者，则数据变量必须在状态和长度变量之前出现。

> [!NOTE]
> ATL OLE DB 使用者向导使用 `DB_NUMERIC` 类型来绑定数值数据类型。 它之前使用的是 `DBTYPE_VARNUMERIC`（格式由 `DB_VARNUMERIC` 类型所描述；请参阅 Oledb.h）。 如果不使用向导来创建使用者，建议使用 `DB_NUMERIC`。

```cpp
// Products.H : Declaration of the CProducts class

class CProductsAccessor
{
public:
   // Column data members:
   LONG m_ProductID;
   TCHAR m_ProductName[41];
   LONG m_SupplierID;
   LONG m_CategoryID;
   TCHAR m_QuantityPerUnit[21];
   CURRENCY m_UnitPrice;
   SHORT m_UnitsInStock;
   SHORT m_UnitsOnOrder;
   SHORT m_ReorderLevel;
   VARIANT_BOOL m_Discontinued;

   // Column status data members:
   DBSTATUS m_dwProductIDStatus;
   DBSTATUS m_dwProductNameStatus;
   DBSTATUS m_dwSupplierIDStatus;
   DBSTATUS m_dwCategoryIDStatus;
   DBSTATUS m_dwQuantityPerUnitStatus;
   DBSTATUS m_dwUnitPriceStatus;
   DBSTATUS m_dwUnitsInStockStatus;
   DBSTATUS m_dwUnitsOnOrderStatus;
   DBSTATUS m_dwReorderLevelStatus;
   DBSTATUS m_dwDiscontinuedStatus;

   // Column length data members:
   DBLENGTH m_dwProductIDLength;
   DBLENGTH m_dwProductNameLength;
   DBLENGTH m_dwSupplierIDLength;
   DBLENGTH m_dwCategoryIDLength;
   DBLENGTH m_dwQuantityPerUnitLength;
   DBLENGTH m_dwUnitPriceLength;
   DBLENGTH m_dwUnitsInStockLength;
   DBLENGTH m_dwUnitsOnOrderLength;
   DBLENGTH m_dwReorderLevelLength;
   DBLENGTH m_dwDiscontinuedLength;
```

### <a name="rowset-properties"></a>行集属性

接下来，向导将设置行集属性。 如果在 ATL OLE DB 使用者向导中选择了“更改”  、“插入”  或“删除”  ，则此时将设置相应的属性（将始终设置 DBPROP_IRowsetChange，然后将分别设置 DBPROPVAL_UP_CHANGE、DBPROPVAL_UP_INSERT 和/或 DBPROPVAL_UP_DELETE 中的一个或多个）。

```cpp
void GetRowsetProperties(CDBPropSet* pPropSet)
{
   pPropSet->AddProperty(DBPROP_CANFETCHBACKWARDS, true, DBPROPOPTIONS_OPTIONAL);
   pPropSet->AddProperty(DBPROP_CANSCROLLBACKWARDS, true, DBPROPOPTIONS_OPTIONAL);
   pPropSet->AddProperty(DBPROP_IRowsetChange, true, DBPROPOPTIONS_OPTIONAL);
   pPropSet->AddProperty(DBPROP_UPDATABILITY, DBPROPVAL_UP_CHANGE | DBPROPVAL_UP_INSERT | DBPROPVAL_UP_DELETE);
}
```

### <a name="command-or-table-class"></a>命令类或表类

如果指定命令类，则向导将声明命令类；对于模板化代码，命令将如下所示：

```cpp
DEFINE_COMMAND_EX(CProductsAccessor, L" \
SELECT \
   ProductID, \
   ProductName, \
   SupplierID, \
   CategoryID, \
   QuantityPerUnit, \
   UnitPrice, \
   UnitsInStock, \
   UnitsOnOrder, \
   ReorderLevel, \
   Discontinued \
   FROM dbo.Products")
```

### <a name="column-map"></a>列映射

然后，向导将生成列绑定或列映射。 若要修复某些提供程序的问题，以下代码可能采用与提供程序所报告顺序不同的顺序来绑定列。

```cpp
   BEGIN_COLUMN_MAP(CProductsAccessor)
      COLUMN_ENTRY_LENGTH_STATUS(1, m_ProductID, m_dwProductIDLength, m_dwProductIDStatus)
      COLUMN_ENTRY_LENGTH_STATUS(2, m_ProductName, m_dwProductNameLength, m_dwProductNameStatus)
      COLUMN_ENTRY_LENGTH_STATUS(3, m_SupplierID, m_dwSupplierIDLength, m_dwSupplierIDStatus)
      COLUMN_ENTRY_LENGTH_STATUS(4, m_CategoryID, m_dwCategoryIDLength, m_dwCategoryIDStatus)
      COLUMN_ENTRY_LENGTH_STATUS(5, m_QuantityPerUnit, m_dwQuantityPerUnitLength, m_dwQuantityPerUnitStatus)
      _COLUMN_ENTRY_CODE(6, DBTYPE_CY, _SIZE_TYPE(m_UnitPrice), 0, 0, offsetbuf(m_UnitPrice), offsetbuf(m_dwUnitPriceLength), offsetbuf(m_dwUnitPriceStatus))
      COLUMN_ENTRY_LENGTH_STATUS(7, m_UnitsInStock, m_dwUnitsInStockLength, m_dwUnitsInStockStatus)
      COLUMN_ENTRY_LENGTH_STATUS(8, m_UnitsOnOrder, m_dwUnitsOnOrderLength, m_dwUnitsOnOrderStatus)
      COLUMN_ENTRY_LENGTH_STATUS(9, m_ReorderLevel, m_dwReorderLevelLength, m_dwReorderLevelStatus)
      _COLUMN_ENTRY_CODE(10, DBTYPE_BOOL, _SIZE_TYPE(m_Discontinued), 0, 0, offsetbuf(m_Discontinued), offsetbuf(m_dwDiscontinuedLength), offsetbuf(m_dwDiscontinuedStatus))
   END_COLUMN_MAP()
};
```

### <a name="class-declaration"></a>类声明

最后，向导将生成如下命令类声明：

```cpp
class CProducts : public CCommand<CAccessor<CProductsAccessor>>
```

## <a name="attribute-injected-user-record-classes"></a>属性插入的用户记录类

如果使用数据库属性（ [db_command](../../windows/attributes/db-command.md) 或 [db_table](../../windows/attributes/db-table.md)）创建 OLE DB 使用者，属性将插入一个名称为“_ *类名* Accessor”格式的用户记录类。 例如，如果将命令类命名为 `COrders`，用户记录类将为 `_COrdersAccessor`。 虽然用户记录类显示在“类视图”  中，但双击它会改为转到头文件中的命令类或表类。 在这些情况下，只能通过查看属性插入的代码来查看用户记录类的实际声明。

如果在属性化使用者中添加或重写方法，可能会出现问题。 例如，可以将 `_COrdersAccessor` 构造函数添加到 `COrders` 声明中，但请注意，实际上这一操作将构造函数添加到了插入的 `COrdersAccessor` 类中。 此类构造函数可以初始化列/参数，但你无法通过这种方式创建复制构造函数，因为它无法直接实例化 `COrdersAccessor` 对象。 如果需要直接位于 `COrders` 类上的构造函数（或其他方法），建议定义派生自 `COrders` 的新类，并将所需的方法添加到其中。

在下面的示例中，向导为 `COrders` 类生成声明，但用户记录类 `COrdersAccessor` 不显示，因为特性会将它插入。

```cpp
#define _ATL_ATTRIBUTES
#include <atlbase.h>
#include <atldbcli.h>
[
   db_source(L"your connection string"),
   db_command(L"Select ShipName from Orders;")
]
class COrders
{
public:

   // COrders()            // incorrect constructor name
   _COrdersAccessor()      // correct constructor name
   {
   }
      [db_column(1) ] TCHAR m_ShipName[41];
   };
```

插入的命令类声明如下所示：

```cpp
class CProducts : public CCommand<CAccessor<_CProductsAccessor>>
```

大部分插入的代码均与模板化版本相同或相似。 主要区别在于插入的方法，这在 [使用者向导生成的方法](../../data/oledb/consumer-wizard-generated-methods.md)中介绍。

有关查看插入代码的信息，请参阅 [调试插入代码](/visualstudio/debugger/how-to-debug-injected-code)。

::: moniker-end

## <a name="see-also"></a>请参阅

[使用向导创建 OLE DB 使用者](../../data/oledb/creating-an-ole-db-consumer-using-a-wizard.md)
