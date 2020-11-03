---
title: 使用者向导生成的方法
ms.date: 05/09/2019
helpviewer_keywords:
- OLE DB consumers, wizard-generated classes and methods
ms.assetid: d80ee51c-8bb3-4dca-8760-5808e0fb47b4
ms.openlocfilehash: 15d41aabee174a409ad11e4422292bcea5f10bbf
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92919119"
---
# <a name="consumer-wizard-generated-methods"></a>使用者向导生成的方法

::: moniker range="msvc-160"

ATL OLE DB 使用者向导不适用于 Visual Studio 2019 及更高版本。 但仍可以手动添加此功能。

::: moniker-end

::: moniker range="<=msvc-150"

ATL OLE DB 使用者向导  和 MFC 应用程序向导  生成一些你应该知道的函数。 由于一些方法在特性化项目中以不同方式实现，因此有几点注意事项；下面分别介绍了每种情况。 有关查看插入代码的信息，请参阅 [调试插入代码](/visualstudio/debugger/how-to-debug-injected-code)。

- `OpenAll` 打开数据源、行集，并在书签可用时启用书签。

- `CloseAll` 关闭所有打开的行集，并释放所有命令执行。

- `OpenRowset` 由 `OpenAll` 调用，以打开使用者的一个或多个行集。

- `GetRowsetProperties` 检索指向行集的属性集的指针，此指针可用于设置属性。

- `OpenDataSource` 使用“数据链接属性”  对话框中指定的初始化字符串打开数据源。

- `CloseDataSource` 以适当方式关闭数据源。

## <a name="openall-and-closeall"></a>OpenAll 和 CloseAll

```cpp
HRESULT OpenAll();

void CloseAll();
```

以下示例演示在重复执行同一命令时如何调用 `OpenAll` 和 `CloseAll`。 比较 [ccommand::Close](./ccommand-class.md#close) 中的代码示例，它展示了调用 `Close` 和 `ReleaseCommand`（而不是 `CloseAll`）的变体。

```cpp
int main(int argc, char* argv[])
{
   HRESULT hr;

   hr = CoInitialize(NULL);

   CCustOrdersDetail rs;      // Your CCommand-derived class
   rs.m_OrderID = 10248;      // Open order 10248
   hr = rs.OpenAll();         // (Open also executes the command)
   hr = rs.MoveFirst();         // Move to the first row and print it
   printf( "Name: %s, Unit Price: %d, Quantity: %d, Discount %d, Extended Price %d\n", rs.m_ProductName, rs.m_UnitPrice.int64, rs.m_Quantity, rs.m_Discount, rs.m_ExtendedPrice.int64 );

   // Close the first command execution
   rs.Close();

   rs.m_OrderID = 10249;      // Open order 10249 (a new order)
   hr = rs.Open();            // (Open also executes the command)
   hr = rs.MoveFirst();         // Move to the first row and print it
   printf( "Name: %s, Unit Price: %d, Quantity: %d, Discount %d, Extended Price %d\n", rs.m_ProductName, rs.m_UnitPrice.int64, rs.m_Quantity, rs.m_Discount, rs.m_ExtendedPrice.int64 );

   // Close the second command execution;
   // Instead of rs.CloseAll() you could call
   // rs.Close() and rs.ReleaseCommand():
   rs.CloseAll();

   CoUninitialize();
   return 0;
}
```

### <a name="remarks"></a>注解

如果你定义 `HasBookmark` 方法，`OpenAll` 代码设置 `DBPROP_IRowsetLocate` 属性；请确保仅在提供程序支持此属性时才这样做。

## <a name="openrowset"></a>OpenRowset

```cpp
// OLE DB Template version:
HRESULT OpenRowset(DBPROPSET* pPropSet = NULL)
// Attribute-injected version:
HRESULT OpenRowset(const CSession& session, LPCWSTR szCommand = NULL);
```

`OpenAll` 调用此方法来打开使用者的一个或多个行集。 通常情况下，除非要处理多个数据源/会话/行集，否则不需要调用 `OpenRowset`。 `OpenRowset` 是在命令或表类头文件中声明：

```cpp
// OLE DB Template version:
HRESULT OpenRowset(DBPROPSET *pPropSet = NULL)
{
   HRESULT hr = Open(m_session, NULL, pPropSet);
   #ifdef _DEBUG
   if(FAILED(hr))
      AtlTraceErrorRecords(hr);
   #endif
   return hr;
}
```

属性以不同方式实现此方法。 此版本需要使用会话对象和命令字符串（默认为 db_command 中指定的命令字符串，但也可以传递其他命令字符串）。 如果你定义 `HasBookmark` 方法，`OpenRowset` 代码设置 `DBPROP_IRowsetLocate` 属性；请确保仅在提供程序支持此属性时才这样做。

```cpp
// Attribute-injected version:
HRESULT OpenRowset(const CSession& session, LPCWSTR szCommand=NULL)
{

   DBPROPSET *pPropSet = NULL;
   CDBPropSet propset(DBPROPSET_ROWSET);
   __if_exists(HasBookmark)

   {
      propset.AddProperty(DBPROP_IRowsetLocate, true);
      pPropSet= &propset;
      }
...
}
```

## <a name="getrowsetproperties"></a>GetRowsetProperties

```cpp
void GetRowsetProperties(CDBPropSet* pPropSet);
```

此方法检索指向行集的属性集的指针；可以使用此指针来设置属性（如 `DBPROP_IRowsetChange`）。 `GetRowsetProperties` 用于用户记录类，如下所示。 可以将下面的代码修改为设置其他行集属性：

```cpp
void GetRowsetProperties(CDBPropSet* pPropSet)
{
   pPropSet->AddProperty(DBPROP_CANFETCHBACKWARDS, true, DBPROPOPTIONS_OPTIONAL);
   pPropSet->AddProperty(DBPROP_CANSCROLLBACKWARDS, true, DBPROPOPTIONS_OPTIONAL);
   pPropSet->AddProperty(DBPROP_IRowsetChange, true, DBPROPOPTIONS_OPTIONAL);
   pPropSet->AddProperty(DBPROP_UPDATABILITY, DBPROPVAL_UP_CHANGE | DBPROPVAL_UP_INSERT | DBPROPVAL_UP_DELETE);
}
```

### <a name="remarks"></a>注解

不得定义全局 `GetRowsetProperties` 方法，因为它可能会与向导定义的方法冲突。 这是模板化和特性化项目随附的向导生成方法；属性不插入此代码。

## <a name="opendatasource-and-closedatasource"></a>OpenDataSource 和 CloseDataSource

```cpp
HRESULT OpenDataSource();

void CloseDataSource();
```

### <a name="remarks"></a>注解

向导定义了 `OpenDataSource` 和 `CloseDataSource` 方法；`OpenDataSource` 调用 [CDataSource::OpenFromInitializationString](./cdatasource-class.md#openfrominitializationstring)。

::: moniker-end

## <a name="see-also"></a>请参阅

[使用向导创建 OLE DB 使用者](../../data/oledb/creating-an-ole-db-consumer-using-a-wizard.md)
