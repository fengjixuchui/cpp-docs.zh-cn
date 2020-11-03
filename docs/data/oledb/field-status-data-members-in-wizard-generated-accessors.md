---
title: 向导生成的访问器中的字段状态数据成员
ms.date: 05/09/2019
helpviewer_keywords:
- OLE DB consumer templates, field status
- field status in OLE DB templates
ms.assetid: 66e4e223-c60c-471e-860d-d23abcdfe371
ms.openlocfilehash: 476c91f55071f6d1c7f243257273a32798813cae
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924630"
---
# <a name="field-status-data-members-in-wizard-generated-accessors"></a>向导生成的访问器中的字段状态数据成员

::: moniker range="msvc-160"

ATL OLE DB 使用者向导不适用于 Visual Studio 2019 及更高版本。 但仍可以手动添加此功能。 有关详细信息，请参阅[不使用向导创建使用者](creating-a-consumer-without-using-a-wizard.md)。

::: moniker-end

::: moniker range="<=msvc-150"

当你使用 ATL OLE DB 使用者向导  创建使用者时，向导在用户记录类中为你在列映射中指定的每个字段生成数据成员。 每个数据成员都是 `DWORD` 类型，并包含与其各自字段相对应的状态值。

例如，对于数据成员 m_OwnerID  ，向导生成一个用于字段状态 (dwOwnerIDStatus  ) 的附加数据成员，以及另一个用于字段长度 (dwOwnerIDLength  ) 的数据成员。 它还生成包含 COLUMN_ENTRY_LENGTH_STATUS 条目的列映射。

下面的代码对此进行了演示：

```cpp
class CAuthorsAccessor
{
public:
   LONG m_AuID;
   TCHAR m_Author[53];
   LONG m_YearBorn;

   DBSTATUS m_dwAuIDStatus;
   DBSTATUS m_dwAuthorStatus;
   DBSTATUS m_dwYearBornStatus;

   DBLENGTH m_dwAuIDLength;
   DBLENGTH m_dwAuthorLength;
   DBLENGTH m_dwYearBornLength;

   DEFINE_COMMAND_EX(CAuthorsAccessor, L" \
   SELECT \
      AuID, \
      Author, \
      YearBorn \
      FROM dbo.Authors")

   BEGIN_COLUMN_MAP(CAuthorsAccessor)
      COLUMN_ENTRY_LENGTH_STATUS(1, m_AuID, dwAuIDLength, dwAuIDStatus)
      COLUMN_ENTRY_LENGTH_STATUS(2, m_Author, dwAuthorLength, dwAuthorStatus)
      COLUMN_ENTRY_LENGTH_STATUS(3, m_YearBorn, dwYearBornLength, dwYearBornStatus)
   END_COLUMN_MAP()
...
```

> [!NOTE]
> 如果修改用户记录类或编写自己的使用者，则数据变量必须在状态和长度变量之前出现。

可以将状态值用于调试目的。 如果 ATL OLE DB 使用者向导  生成的代码生成了编译错误（如 DB_S_ERRORSOCCURRED 或 DB_E_ERRORSOCCURRED），你应先查看字段状态数据成员的当前值。 值非零的数据成员对应于有问题的列。

还可以使用状态值来设置特定字段的 NULL 值。 这样做可有助于将字段值区分为 NULL（而不是零）。 至于 NULL 是有效值还是特殊值，以及应用程序应如何处理它，都由你自己决定。 OLE DB 将 DBSTATUS_S_ISNULL 定义为指定泛型 NULL 值的正确方法。 如果使用者读取数据且值为 NULL，那么状态字段设置为“DBSTATUS_S_ISNULL”。 若要设置 NULL 值，使用者先将状态值设置为“DBSTATUS_S_ISNULL”，再调用提供程序。

接下来，打开 Oledb.h，并搜索 DBSTATUSENUM。 然后，可以根据 DBSTATUSENUM 枚举值来匹配非零状态数值。 如果枚举名称不足以说明错在何处，请参阅  一节中的“状态”  主题。 此主题包含获取或设置数据时使用的状态值表。 若要了解长度值，请参阅同一节中的“长度”  主题。

## <a name="retrieving-the-length-or-status-of-a-column"></a>检索列长度或列状态

可以检索可变长度列的长度，也可以检索列状态（例如，检查是否有 DBSTATUS_S_ISNULL）：

- 若要获取长度，请使用 COLUMN_ENTRY_LENGTH 宏。

- 若要获取状态，请使用 COLUMN_ENTRY_STATUS 宏。

- 若要同时获取长度和状态，请使用 COLUMN_ENTRY_LENGTH_STATUS，如下所示：

    ```cpp
    class CProducts
    {
    public:
       char      szName[40];
       long      nNameLength;
       DBSTATUS   nNameStatus;

    BEGIN_COLUMN_MAP(CProducts)
    // Bind the column to CProducts.m_ProductName.
    // nOrdinal is zero-based, so the column number of m_ProductName is 1.
       COLUMN_ENTRY_LENGTH_STATUS(1, szName, nNameLength, nNameStatus)
    END_COLUMN_MAP()
    };
    ```

- 然后，对长度和/或状态进行取值，如下所示：

    ```cpp
    CTable<CAccessor<CProducts >> product;
    CSession session;

    product.Open(session, "Product");

    while (product.MoveNext() == S_OK)
    {
       // Check the product name isn't NULL before tracing it
       if (product.nNameStatus == DBSTATUS_S_OK)
          ATLTRACE("%s is %d characters\n", product.szName, product.nNameLength);
    }
    ```

当你使用 `CDynamicAccessor` 时，系统会自动为你绑定长度和状态。 若要检索长度值和状态值，请使用 `GetLength` 和 `GetStatus` 成员函数。

::: moniker-end

## <a name="see-also"></a>请参阅

[使用 OLE DB 使用者模板](../../data/oledb/working-with-ole-db-consumer-templates.md)
