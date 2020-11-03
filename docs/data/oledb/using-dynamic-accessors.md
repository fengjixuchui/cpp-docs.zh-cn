---
title: 使用动态访问器
ms.date: 10/18/2018
helpviewer_keywords:
- accessors [C++], dynamic
- dynamic accessors
ms.assetid: e5d5bfa6-2b1d-49d0-8ced-914666422431
ms.openlocfilehash: 4f42d6f20da819cf325cad06a04878b46e52352a
ms.sourcegitcommit: 43cee7a0d41a062661229043c2f7cbc6ace17fa3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92008676"
---
# <a name="using-dynamic-accessors"></a>使用动态访问器

当您不知道数据库架构 (基础结构) 时，动态访问器可让您访问数据源。 OLE DB 模板库提供了多个类来帮助您。

[DynamicConsumer](https://github.com/Microsoft/VCSamples/tree/master/VC2008Samples/ATL/OLEDB/Consumer/DynamicConsumer)示例演示如何使用动态访问器类来获取列信息和动态创建访问器。

## <a name="using-cdynamicaccessor"></a>使用 CDynamicAccessor

当您不知道数据库的基础结构)  (数据库架构时， [CDynamicAccessor](../../data/oledb/cdynamicaccessor-class.md)允许您访问数据源。 `CDynamicAccessor` 方法获取列信息，如列名称、计数和数据类型。 使用此列信息可在运行时动态创建访问器。 列信息存储在由此类创建和管理的缓冲区中。 使用 [GetValue](./cdynamicaccessor-class.md#getvalue) 方法从缓冲区中获取数据。

## <a name="example-cdynamic-accessors"></a>示例： CDynamic 访问器

```cpp
// Using_Dynamic_Accessors.cpp
// compile with: /c /EHsc
#include <stdio.h>
#include <objbase.h>
#include <atldbcli.h>

int main(int argc, char* argv[] )
{
    HRESULT hr = CoInitialize(NULL );

    CDataSource ds;
    CSession ss;

    CTable<CDynamicAccessor> rs;

    // The following is an example initialization string:
    hr = ds.OpenFromInitializationString(L"Provider=SQLOLEDB.1;"
      L"Integrated Security=SSPI;Persist Security Info=False;"
      L"Initial Catalog=Loginname;Data Source=your_data_source;"
      L"Use Procedure for Prepare=1;Auto Translate=True;"
      L"Packet Size=4096;Workstation ID=LOGINNAME01;"
      L"Use Encryption for Data=False;"
      L"Tag with column collation when possible=False");

    hr = ss.Open(ds );
    hr = rs.Open(ss, "Shippers" );

    hr = rs.MoveFirst();
    while(SUCCEEDED(hr ) && hr != DB_S_ENDOFROWSET )
    {
        for(size_t i = 1; i <= rs.GetColumnCount(); i++ )
        {
            DBTYPE type;
            rs.GetColumnType(i, &type );
            printf_s( "Column %d [%S] is of type %d\n",
                      i, rs.GetColumnName(i ), type );

            switch(type )
            {
                case DBTYPE_WSTR:
                    printf_s( "value is %S\n",
                              (WCHAR*)rs.GetValue(i ) );
                break;
                case DBTYPE_STR:
                    printf_s( "value is %s\n",
                              (CHAR*)rs.GetValue(i ) );
                default:
                    printf_s( "value is %d\n",
                              *(long*)rs.GetValue(i ) );
            }
        }
        hr = rs.MoveNext();
    }

    rs.Close();
    ss.Close();
    ds.Close();
    CoUninitialize();

    return 0;
}
```

## <a name="using-cdynamicstringaccessor"></a>使用 CDynamicStringAccessor

[CDynamicStringAccessor](../../data/oledb/cdynamicstringaccessor-class.md) 类似于 [CDynamicAccessor](../../data/oledb/cdynamicaccessor-class.md)，但有一个重要的方法。 `CDynamicAccessor`请求提供程序报告的本机格式的数据时， `CDynamicStringAccessor` 请求提供程序将从数据存储中访问的所有数据作为字符串数据获取。 对于不需要在数据存储中计算值的简单任务（如显示或打印数据存储的内容），该过程特别有用。

使用 `CDynamicStringAccessor` 方法来获取列信息。 使用此列信息可在运行时动态创建访问器。 列信息存储在由此类创建和管理的缓冲区中。 使用 [CDynamicStringAccessor：： GetString](./cdynamicstringaccessor-class.md#getstring) 从缓冲区中获取数据，或使用 [CDynamicStringAccessor：： SetString](./cdynamicstringaccessor-class.md#setstring)将数据存储到缓冲区。

## <a name="example-cdynamicstringaccessor"></a>示例： CDynamicStringAccessor

```cpp
// Using_Dynamic_Accessors_b.cpp
// compile with: /c /EHsc
#include <stdio.h>
#include <objbase.h>
#include <atldbcli.h>

int main(int argc, char* argv[] )
{
    HRESULT hr = CoInitialize(NULL );
    if (hr != S_OK)
    {
        exit (-1);
    }

    CDataSource ds;
    CSession ss;

    CTable<CDynamicStringAccessor> rs;

    // The following is an example initialization string:
    hr = ds.OpenFromInitializationString(L"Provider=SQLOLEDB.1;"
      L"Integrated Security=SSPI;Persist Security Info=False;"
      L"Initial Catalog=Loginname;Data Source=your_data_source;"
      L"Use Procedure for Prepare=1;Auto Translate=True;"
      L"Packet Size=4096;Workstation ID=LOGINNAME01;"
      L"Use Encryption for Data=False;"
      L"Tag with column collation when possible=False");

    hr = ss.Open(ds );
    hr = rs.Open(ss, "Shippers" );

    hr = rs.MoveFirst();
    while(SUCCEEDED(hr ) && hr != DB_S_ENDOFROWSET )
    {
        for(size_t i = 1; i <= rs.GetColumnCount(); i++ )
        {
            printf_s( "column %d value is %s\n",
                      i, rs.GetString(i ) );
        }
        hr = rs.MoveNext();
    }

    rs.Close();
    ss.Close();
    ds.Close();
    CoUninitialize();

   return 0;
}
```

## <a name="using-cdynamicparameteraccessor"></a>使用 CDynamicParameterAccessor

[CDynamicParameterAccessor](../../data/oledb/cdynamicparameteraccessor-class.md) 类似于 [CDynamicAccessor](../../data/oledb/cdynamicaccessor-class.md)，只是 `CDynamicParameterAccessor` 通过调用 [ICommandWithParameters](/sql/relational-databases/native-client-ole-db-interfaces/icommandwithparameters) 接口来获取要设置的参数信息。 访问接口必须支持 `ICommandWithParameters` 以便使用者使用此类。

参数信息存储在由此类创建和管理的缓冲区中。 使用 [CDynamicParameterAccessor：： GetParam](./cdynamicparameteraccessor-class.md#getparam) 和 [CDynamicParameterAccessor：： GetParamType](./cdynamicparameteraccessor-class.md#getparamtype)从缓冲区中获取参数数据。

有关演示如何使用此类执行 SQL Server 存储过程并获取输出参数值的示例，请参阅 GitHub 上的[Microsoft VCSamples](https://github.com/Microsoft/VCSamples)存储库中的[DynamicConsumer](https://github.com/Microsoft/VCSamples/tree/master/VC2008Samples/ATL/OLEDB/Consumer/DynamicConsumer)示例代码。

## <a name="see-also"></a>请参阅

[使用访问器](../../data/oledb/using-accessors.md)<br/>
[CDynamicAccessor 类](../../data/oledb/cdynamicaccessor-class.md)<br/>
[CDynamicStringAccessor 类](../../data/oledb/cdynamicstringaccessor-class.md)<br/>
[CDynamicParameterAccessor 类](../../data/oledb/cdynamicparameteraccessor-class.md)<br/>
[DynamicConsumer 示例](https://github.com/Microsoft/VCSamples/tree/master/VC2008Samples/ATL/OLEDB/Consumer/DynamicConsumer)
