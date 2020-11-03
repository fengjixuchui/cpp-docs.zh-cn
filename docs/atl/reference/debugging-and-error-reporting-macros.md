---
title: 调试和错误报告宏
ms.date: 05/06/2019
f1_keywords:
- atldef/ATL::_ATL_DEBUG_INTERFACES
- atldef/ATL::_ATL_DEBUG_QI
- atldef/ATL::ATLASSERT
- afx/ATL::ATLENSURE
- atltrace/ATL::ATLTRACENOTIMPL
- atltrace/ATL::ATLTRACE
helpviewer_keywords:
- macros, error reporting
ms.assetid: 4da9b87f-ec5c-4a32-ab93-637780909b9d
ms.openlocfilehash: 6b969cfb841a9a95d695eacc0a25f9dd378379ac
ms.sourcegitcommit: ced5ff1431ffbd25b20d106901955532723bd188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2020
ms.locfileid: "92135536"
---
# <a name="debugging-and-error-reporting-macros"></a>调试和错误报告宏

这些宏提供有用的调试和跟踪功能。

|名称|描述|
|-|-|
|[_ATL_DEBUG_INTERFACES](#_atl_debug_interfaces)|向 "输出" 窗口写入在调用时检测到的任何接口泄漏 `_Module.Term` 。|
|[_ATL_DEBUG_QI](#_atl_debug_qi)|将所有对的调用写入到 `QueryInterface` 输出窗口。|
|[ATLASSERT](#atlassert)|执行与 C 运行时库中的 [_ASSERTE](../../c-runtime-library/reference/assert-asserte-assert-expr-macros.md) 宏相同的功能。|
|[ATLENSURE](#atlensure)|执行参数验证。 `AtlThrow`如果需要，调用|
|[ATLTRACENOTIMPL](#atltracenotimpl)|向转储设备发送一条消息，指出指定的函数未实现。|
|[ATLTRACE](#atltrace)|根据指示的标志和级别向输出设备（如调试器窗口）报告警告。 为了向后兼容而提供。|
|[ATLTRACE2](#atltrace2)|根据指示的标志和级别向输出设备（如调试器窗口）报告警告。|

## <a name="_atl_debug_interfaces"></a><a name="_atl_debug_interfaces"></a> _ATL_DEBUG_INTERFACES

在包含任何 ATL 标头文件之前定义此宏，以跟踪对 " `AddRef` `Release` 输出" 窗口的组件接口的所有和调用。

```
#define _ATL_DEBUG_INTERFACES
```

### <a name="remarks"></a>注解

跟踪输出将如下所示：

`ATL: QIThunk - 2008         AddRef  :   Object = 0x00d81ba0   Refcount = 1   CBug - IBug`

每个跟踪的第一部分将始终为 `ATL: QIThunk` 。 接下来是一个值，该值标识正在使用的特定 *接口 thunk* 。 接口 thunk 是一个对象，用于维护引用计数，并提供此处使用的跟踪功能。 每次调用时都会创建一个新的接口 thunk `QueryInterface` `IUnknown` ，在这种情况下，对接口 (的请求除外，每次都将返回相同的 thunk，以符合 COM 的标识规则) 。

接下来，你将看到 `AddRef` 或 `Release` 指示所调用的方法。 之后，你将看到一个值，该值标识其接口引用计数已更改的对象。 跟踪的值是 **`this`** 对象的指针。

所跟踪的引用计数是在或调用后该 thunk 上的引用计数 `AddRef` `Release` 。 请注意，此引用计数可能与对象的引用计数不匹配。 每个 thunk 都维护其自己的引用计数，以帮助你完全遵守 COM 的引用计数规则。

跟踪的最后一条信息是对象的名称，接口受 `AddRef` 或 `Release` 调用影响。

将按如下所示记录在服务器关闭并调用时检测到的任何接口泄漏 `_Module.Term` ：

`ATL: QIThunk - 2005         LEAK    :   Object = 0x00d81ca0   Refcount = 1   MaxRefCount = 1   CBug - IBug`

此处提供的信息直接映射到前面的跟踪语句中提供的信息，因此您可以在接口 thunk 的整个生存期内检查引用计数。 此外，还会显示该接口 thunk 上的最大引用计数。

> [!NOTE]
> _ATL_DEBUG_INTERFACES 可以在零售版本中使用。

## <a name="_atl_debug_qi"></a><a name="_atl_debug_qi"></a> _ATL_DEBUG_QI

将所有对的调用写入到 `QueryInterface` 输出窗口。

```
#define _ATL_DEBUG_QI
```

### <a name="remarks"></a>注解

如果对的调用 `QueryInterface` 失败，则将显示输出窗口：

*接口名称* - `failed`

## <a name="atlassert"></a><a name="atlassert"></a> ATLASSERT

ATLASSERT 宏执行的功能与 C 运行时库中的 [_ASSERTE](../../c-runtime-library/reference/assert-asserte-assert-expr-macros.md) 宏的功能相同。

```
ATLASSERT(booleanExpression);
```

### <a name="parameters"></a>参数

*booleanExpression*<br/>
计算结果为零或不为零的表达式（包括指针）。

### <a name="remarks"></a>注解

在调试版本中，ATLASSERT 计算 *booleanExpression* 并在结果为 false 时生成调试报告。

### <a name="requirements"></a>要求

**标头：** atldef

## <a name="atlensure"></a><a name="atlensure"></a> ATLENSURE

此宏用于验证传递给函数的参数。

```
ATLENSURE(booleanExpression);
ATLENSURE_THROW(booleanExpression, hr);
```

### <a name="parameters"></a>参数

*booleanExpression*<br/>
指定要测试的布尔表达式。

*小时*<br/>
指定要返回的错误代码。

### <a name="remarks"></a>注解

这些宏提供一种机制，用于检测和通知用户不正确的参数用法。

如果条件未能调用，宏将调用 ATLASSERT `AtlThrow` 。

在 ATLENSURE 情况下， `AtlThrow` 通过 E_FAIL 调用。

在 ATLENSURE_THROW 情况下， `AtlThrow` 将调用并指定 HRESULT。

ATLENSURE 和 ATLASSERT 之间的区别在于 ATLENSURE 在发布版本以及调试版本中引发异常。

### <a name="example"></a>示例

[!code-cpp[NVC_ATL_Utilities#108](../../atl/codesnippet/cpp/debugging-and-error-reporting-macros_1.cpp)]

### <a name="requirements"></a>要求

**标头：** afx

## <a name="atltracenotimpl"></a><a name="atltracenotimpl"></a> ATLTRACENOTIMPL

在 ATL 的调试版本中，将字符串 "未实现的 *funcname* " 发送到转储设备并返回 E_NOTIMPL。

```
ATLTRACENOTIMPL(funcname);
```

### <a name="parameters"></a>参数

*funcname*<br/>
中一个字符串，包含未实现的函数的名称。

### <a name="remarks"></a>注解

在发布版本中，只返回 E_NOTIMPL。

### <a name="example"></a>示例

[!code-cpp[NVC_ATL_Utilities#127](../../atl/codesnippet/cpp/debugging-and-error-reporting-macros_2.cpp)]

### <a name="requirements"></a>要求

**标头：** atltrace

## <a name="atltrace"></a><a name="atltrace"></a> ATLTRACE

根据指示的标志和级别向输出设备（如调试器窗口）报告警告。 为了向后兼容而提供。

```
ATLTRACE(exp);

ATLTRACE(
    DWORD category,
    UINT  level,
    LPCSTR lpszFormat, ...);
```

### <a name="parameters"></a>参数

*exp*<br/>
中要发送到 "输出" 窗口或任何捕获这些消息的应用程序的字符串和变量。

*category*<br/>
中要报告的事件或方法的类型。 有关类别列表，请参阅 "备注"。

*level*<br/>
中要报告的跟踪级别。 有关详细信息，请参阅 "备注"。

*lpszFormat*<br/>
中要发送到转储设备的格式化字符串。

### <a name="remarks"></a>注解

有关 ATLTRACE 的说明，请参阅 [ATLTRACE2](#atltrace2) 。 ATLTRACE 和 ATLTRACE2 具有相同的行为，包括 ATLTRACE，以实现向后兼容性。

## <a name="atltrace2"></a><a name="atltrace2"></a> ATLTRACE2

根据指示的标志和级别向输出设备（如调试器窗口）报告警告。

```
ATLTRACE2(exp);

ATLTRACE2(
    DWORD category,
    UINT level,
    LPCSTR lpszFormat,  ...);
```

### <a name="parameters"></a>参数

*exp*<br/>
中要发送到 "输出" 窗口或任何捕获这些消息的应用程序的字符串。

*category*<br/>
中要报告的事件或方法的类型。 有关类别列表，请参阅 "备注"。

*level*<br/>
中要报告的跟踪级别。 有关详细信息，请参阅 "备注"。

*lpszFormat*<br/>
中 `printf`要用于创建要发送到转储设备的字符串的样式格式字符串。

### <a name="remarks"></a>注解

ATLTRACE2 的缩写形式将字符串写入调试器的输出窗口。 第二种形式的 ATLTRACE2 还会将输出写入调试器的输出窗口，但受 ATL/MFC 跟踪工具的设置的限制 (参见 [ATLTraceTool 示例](../../overview/visual-cpp-samples.md)) 。 例如，如果将 *级别* 设置为4，将 ATL/MFC 跟踪工具设置为级别0，则不会看到消息。 *级别* 可以是0、1、2、3或4。 默认值为0，只报告最严重的问题。

*Category*参数列出要设置的跟踪标志。 这些标志对应于要报告的方法的类型。 下表列出了可用于 *category* 参数的有效跟踪标志。

### <a name="atl-trace-flags"></a>ATL 跟踪标志

|ATL 类别|描述|
|------------------|-----------------|
|`atlTraceGeneral`|所有 ATL 应用程序的报表。 默认值。|
|`atlTraceCOM`|有关 COM 方法的报告。|
|`atlTraceQI`|对 QueryInterface 调用的报告。|
|`atlTraceRegistrar`|报告对象的注册。|
|`atlTraceRefcount`|报告更改引用计数。|
|`atlTraceWindowing`|Windows 方法上的报表;例如，报告无效的消息映射 ID。|
|`atlTraceControls`|有关控件的报表;例如，报告控件或其窗口被销毁的时间。|
|`atlTraceHosting`|报告承载消息;例如，当激活容器中的客户端时，报告。|
|`atlTraceDBClient`|OLE DB 使用者模板的报告;例如，当对对的调用失败时，输出可能包含 HRESULT。|
|`atlTraceDBProvider`|OLE DB 提供程序模板上的报表;例如，报表创建列失败。|
|`atlTraceSnapin`|MMC 管理单元应用程序的报告。|
|`atlTraceNotImpl`|报告指定的函数未实现。|
|`atlTraceAllocation`|报告 atldbgmem 中的内存调试工具打印的消息。|

### <a name="mfc-trace-flags"></a>MFC 跟踪标志

|MFC 类别|描述|
|------------------|-----------------|
|`traceAppMsg`|一般用途是 MFC 消息。 始终推荐。|
|`traceDumpContext`|来自 [CDumpContext](../../mfc/reference/cdumpcontext-class.md)的消息。|
|`traceWinMsg`|来自 MFC 消息处理代码的消息。|
|`traceMemory`|来自 MFC 内存管理代码的消息。|
|`traceCmdRouting`|来自 MFC Windows 命令传送代码的消息。|
|`traceHtml`|来自 MFC 的 DHTML 对话框支持的消息。|
|`traceSocket`|来自 MFC 套接字支持的消息。|
|`traceOle`|来自 MFC OLE 支持的消息。|
|`traceDatabase`|来自 MFC 的数据库支持的消息。|
|`traceInternet`|来自 MFC Internet 支持的消息。|

若要声明自定义跟踪类别，请按如下所示声明类的全局实例 `CTraceCategory` ：

[!code-cpp[NVC_ATL_Utilities#109](../../atl/codesnippet/cpp/debugging-and-error-reporting-macros_3.cpp)]

在此示例中，类别名称 MY_CATEGORY 是您为 *category* 参数指定的名称。 第一个参数是将在 ATL/MFC 跟踪工具中显示的类别名称。 第二个参数为默认跟踪级别。 此参数是可选的，默认跟踪级别为0。

使用用户定义的类别：

[!code-cpp[NVC_ATL_Utilities#110](../../atl/codesnippet/cpp/debugging-and-error-reporting-macros_4.cpp)]

若要指定要筛选跟踪消息，请将这些宏的定义插入到 Stdafx.h 中的 `#include <atlbase.h>` 语句前面。

或者，您可以在 " **属性页** " 对话框中的预处理器指令中设置筛选器。 单击 " **预处理器** " 选项卡，然后在 " **预处理器定义** " 编辑框中插入全局。

Atlbase.h 包含 ATLTRACE2 宏的默认定义，如果在处理 atlbase.h 之前没有定义这些符号，则将使用这些定义。

在发布版本中，ATLTRACE2 编译为 `(void) 0` 。

在格式化后，ATLTRACE2 会将要发送到转储设备的字符串的内容限制为不超过1023个字符。

ATLTRACE 和 ATLTRACE2 具有相同的行为，包括 ATLTRACE，以实现向后兼容性。

### <a name="example"></a>示例

[!code-cpp[NVC_ATL_Utilities#111](../../atl/codesnippet/cpp/debugging-and-error-reporting-macros_5.cpp)]

## <a name="see-also"></a>另请参阅

[宏](../../atl/reference/atl-macros.md)<br/>
[调试和错误报告全局函数](../../atl/reference/debugging-and-error-reporting-global-functions.md)
