---
title: gmtime、_gmtime32、_gmtime64
description: 、和的 API 参考， `gmtime` `_gmtime32` `_gmtime64` 可将值转换 `time_t` 为 `tm` 结构。
ms.date: 10/27/2020
api_name:
- _gmtime32
- gmtime
- _gmtime64
- _o__gmtime32
- _o__gmtime64
api_location:
- msvcrt.dll
- msvcr80.dll
- msvcr90.dll
- msvcr100.dll
- msvcr100_clr0400.dll
- msvcr110.dll
- msvcr110_clr0400.dll
- msvcr120.dll
- msvcr120_clr0400.dll
- ucrtbase.dll
- api-ms-win-crt-time-l1-1-0.dll
- api-ms-win-crt-private-l1-1-0.dll
api_type:
- DLLExport
topic_type:
- apiref
f1_keywords:
- gmtime
- _gmtime32
- _gmtime64
helpviewer_keywords:
- gmtime32 function
- _gmtime64 function
- gmtime function
- time functions
- _gmtime32 function
- gmtime64 function
- time structure conversion
ms.assetid: 315501f3-477e-475d-a414-ef100ee0db27
ms.openlocfilehash: bb8bee6b752f64d85dfb0f8c9e5ba7acf204a76f
ms.sourcegitcommit: 9c801a43ee0d4d84956b03fd387716c818705e0d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907540"
---
# <a name="gmtime-_gmtime32-_gmtime64"></a>`gmtime`, `_gmtime32`, `_gmtime64`

将 `time_t` 时间值转换为 `tm` 结构。 提供这些函数的更多安全版本;请参阅[ `gmtime_s` 、 `_gmtime32_s` 、 `_gmtime64_s` ](gmtime-s-gmtime32-s-gmtime64-s.md)。

## <a name="syntax"></a>语法

```C
struct tm *gmtime( const time_t *sourceTime );
struct tm *_gmtime32( const __time32_t *sourceTime );
struct tm *_gmtime64( const __time64_t *sourceTime );
```

### <a name="parameters"></a>参数

*`sourceTime`*\
指向存储时间的指针。 时间表示为自 1970 年 1 月 1 日午夜 (00:00:00)，协调世界时 (UTC) 以来所经过的秒数。

## <a name="return-value"></a>返回值

指向类型的结构的指针 [`tm`](../../c-runtime-library/standard-types.md) 。 返回的结构的字段 *`sourceTime`* 以 UTC 而不是以本地时间保存参数的计算值。 每个结构字段的类型均为 `int`，如下所示：

|字段|说明|
|-|-|
|`tm_sec`|分钟 (0-59) 之后的秒数。|
|`tm_min`| (0-59) 后的分钟数。|
|`tm_hour`|自午夜 (0-23) 。|
|`tm_mday`|月 (1-31) 的第几天。|
|`tm_mon`|月 (0-11;1月 = 0) 。|
|`tm_year`|年（当前年份减去 1900）。|
|`tm_wday`|星期几 (0-6;星期日 = 0) 。|
|`tm_yday`|0-365 年的某一日 (;1月1日 = 0) 。|
|`tm_isdst`|对于 **gmtime** ，始终为0。|

、、和的32位和64位版本都 **`gmtime`** [`mktime`](mktime-mktime32-mktime64.md) [`mkgmtime`](mkgmtime-mkgmtime32-mkgmtime64.md) [`localtime`](localtime-localtime32-localtime64.md) `tm` 为转换使用每个线程一个公共结构。 对这些函数的每次调用都会破坏以前调用的结果。 如果 *`sourceTime`* 表示1970年1月1日午夜之前的日期，则 **`gmtime`** 返回 `NULL` 。 无错误返回。

使用结构的 **_gmtime64** `__time64_t` 允许日期向上表示为23:59:59 年12月31日3000，UTC。 **`_gmtime32`** 仅表示日期为23:59:59 年1月18日2038（UTC）。 1970 年 1 月 1 日午夜是这两个函数的日期范围下限。

**`gmtime`** 是计算结果为的内联函数 **`_gmtime64`** ， `time_t` 除非定义，否则它等效于 `__time64_t` `_USE_32BIT_TIME_T` 。 如果必须强制编译器将解释 `time_t` 为旧的32位 `time_t` ，则可以定义 `_USE_32BIT_TIME_T` ，但这样做会导致 **`gmtime`** 按顺序排列到， **`_gmtime32`** 并将 `time_t` 定义为 `__time32_t` 。 不建议这样做，因为在64位平台上不允许这样做。 在任何情况下，应用程序可能会在2038年1月18日后失败。

这些函数验证其参数。 如果 *`sourceTime`* 是 null 指针，或者如果 *`sourceTime`* 该值为负，则这些函数将调用无效参数处理程序，如 [参数验证](../../c-runtime-library/parameter-validation.md)中所述。 如果允许执行继续，则这些函数返回 `NULL`，并且将 `errno` 设置为 `EINVAL`。

## <a name="remarks"></a>注解

**`_gmtime32`** 函数会对值进行分解 *`sourceTime`* ，并将其存储在中定义的静态分配的类型结构中 `tm` `TIME.H` 。 的值 *`sourceTime`* 通常从对函数的调用中获取 [`time`](time-time32-time64.md) 。

> [!NOTE]
> 在大多数情况下，目标环境尝试确定夏令时是否生效。 C 运行时库假设使用美国规则实现夏令时 (DST) 的计算。

默认情况下，此函数的全局状态的作用域限定为应用程序。 若要更改此项，请参阅 [CRT 中的全局状态](../global-state.md)。

## <a name="requirements"></a>要求

|例程所返回的值|必需的 C 标头|必需的 C++ 标头|
|-------------|---------------------|-|
|**`gmtime`** , **`_gmtime32`** , **`_gmtime64`**|`<time.h>`| `<ctime>` 或 `<time.h>`|

有关其他兼容性信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

## <a name="example"></a>示例

```C
// crt_gmtime.c
// compile with: /W3
// This program uses _gmtime64 to convert a long-
// integer representation of coordinated universal time
// to a structure named newtime, then uses asctime to
// convert this structure to an output string.

#include <time.h>
#include <stdio.h>

int main(void)
{
   struct tm *newtime;
   __int64 ltime;
   char buff[80];

   _time64( &ltime );

   // Obtain coordinated universal time:
   newtime = _gmtime64( &ltime ); // C4996
   // Note: _gmtime64 is deprecated; consider using _gmtime64_s
   asctime_s( buff, sizeof(buff), newtime );
   printf( "Coordinated universal time is %s\n", buff );
}
```

```Output
Coordinated universal time is Tue Feb 12 23:11:31 2002
```

## <a name="see-also"></a>另请参阅

[时间管理](../../c-runtime-library/time-management.md)\
[`asctime`, `_wasctime`](asctime-wasctime.md)\
[`ctime`, `_ctime32`, `_ctime64`, `_wctime`, `_wctime32`, `_wctime64`](ctime-ctime32-ctime64-wctime-wctime32-wctime64.md)\
[`_ftime`, `_ftime32`, `_ftime64`](ftime-ftime32-ftime64.md)\
[`gmtime_s`, `_gmtime32_s`, `_gmtime64_s`](gmtime-s-gmtime32-s-gmtime64-s.md)\
[`localtime`, `_localtime32`, `_localtime64`](localtime-localtime32-localtime64.md)\
[`_mkgmtime`, `_mkgmtime32`, `_mkgmtime64`](mkgmtime-mkgmtime32-mkgmtime64.md)\
[`mktime`, `_mktime32`, `_mktime64`](mktime-mktime32-mktime64.md)\
[`time`, `_time32`, `_time64`](time-time32-time64.md)
