---
title: 标准类型
ms.date: 11/04/2016
f1_keywords:
- __time64_t
- _diskfree_t
- _CRT_DUMP_CLIENT
- _fsize_t
- __timeb64
- File
- __utimeb64
- jmp_buf
- __finddata64_t
- _wfinddata_t
- _finddata_t
- utimbuf64
- wint_t
- wctrans_t
- wctype_t
- _HFILE
- _secerr_handler_func
- clock_t
- fpos_t
- _dev_t
- time64_t
- wfinddata64_t
- stat64
- ldiv_t
- _EXCEPTION_POINTERS
- terminate_function
- size_t
- timeb64
- tm
- _HEAPINFO
- unexpected_function
- _CrtMemState
- _se_translator_function
- sig_atomic_t
- _CRT_REPORT_HOOK
- _complex
- _w_finddatai64_t
- _timeb
- _onexit_t
- _RTC_ErrorNumber
- lconv
- _PNH
- _off_t
- ptrdiff_t
- time_t
- _FPIEEE_RECORD
- _exception
- __w_finddata64_t
- __stat64
- _utimbuf
- __utimbuf64
- div_t
- _CRT_ALLOC_HOOK
- int8_t
- uint8_t
- int16_t
- uint16_t
- int32_t
- uint32_t
- int64_t
- int_least8_t
- uint_least8_t
- int_least16_t
- uint_least16_t
- int_least32_t
- uint_least32_t
- int_least64_t
- uint_least64_t
- int_fast8_t
- uint_fast8_t
- int_fast16_t
- uint_fast16_t
- int_fast32_t
- uint_fast32_t
- int_fast64_t
- uint_fast64_t
- intmax_t
- uintmax_t
helpviewer_keywords:
- __timeb64 type
- tm type
- clock_t type
- intptr_t type
- diskfree_t type
- wctype_t type
- CRT_DUMP_CLIENT type
- sig_atomic_t type
- _PNH type
- time_t type
- wfinddata_t type
- timeb64
- CRT, standard types
- wint_t type
- _RTC_ErrorNumber type
- _diskfree_t type
- _dev_t type
- _wfinddata_t type
- HFILE type
- __utimbuf64 type
- div_t type
- _onexit_t type
- _secerr_handler_func type
- FPIEEE_RECORD type
- HEAPINFO type
- PNH type
- _CRT_ALLOC_HOOK type
- _se_translater_function type
- va_list type
- wctrans_t type
- secerr_handler_func type
- _locale_t type
- timeb type
- lconv type
- utimbuf type
- dev_t type
- unexpected_function typedef
- _complex type
- _off_t type
- off_t type
- RTC_ErrorNumber type
- _FPIEEE_RECORD type
- exception type
- _CRT_REPORT_HOOK type
- _HEAPINFO type
- ldiv_t type
- terminate_function type
- uintptr_t type
- _CRT_DUMP_CLIENT type
- _utimbuf type
- wfinddatai64_t type
- fpos_t type
- wchar_t type
- CRT_ALLOC_HOOK type
- _HFILE type
- __time64_t type
- _timeb type
- CrtMemState type
- __finddata64_t type
- _exception type
- stat type
- onexit_t type
- FILE constant
- _stat type
- finddata_t type
- __wfinddata64_t type
- ptrdiff_t type
- complex types
- _wfinddatai64_t type
- _EXCEPTION_POINTERS type
- jmp_buf type
- se_translater_function type
- size_t type
- EXCEPTION_POINTERS type
- __stat64 type
- _fsize_t type
- CRT_REPORT_HOOK type
- _finddata_t type
ms.assetid: 23312dd2-4a6a-4d70-9b48-2a5d0d8c9f28
ms.openlocfilehash: 376c29371fb9213ed46435b41bea1666058c7b4e
ms.sourcegitcommit: 30792632548d1c71894f9fecbe2f554294b86020
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91765317"
---
# <a name="standard-types"></a>标准类型

Microsoft 运行库定义下列标准类型和 typedef。

### <a name="fixed-width-integral-types-stdinth"></a>定宽整型类型 (stdint.h)

|名称|等效内置类型|
|----------|-------------------------------|
|int8\_t、uint8\_t|signed char、unsigned char|
|int16\_t、uint16\_t|short、unsigned short|
|int32\_t、uint32\_t|int、unsigned int|
|int64\_t、uint64\_t|long long、unsigned long long|
|int_least8_t、uint_least8_t|signed char、unsigned char|
|int_least16_t、uint_least16_t|short、unsigned short|
|int_least32_t、uint_least32_t|int、unsigned int|
|int_least64_t、uint_least64_t|long long、unsigned long long|
|int_fast8_t、uint_fast8_t|signed char、unsigned char|
|int_fast16_t、uint_fast16_t|int、unsigned int|
|int_fast32_t、uint_fast32_t|int、unsigned int|
|int_fast64_t、uint_fast64_t|long long、unsigned long long|
|intmax_t、uintmax_t|long long、unsigned long long|

|类型|说明|声明位置|
|----------|-----------------|-----------------|
|`clock_t` (long)|存储时间值；由 [clock](../c-runtime-library/reference/clock.md) 使用。|TIME.H|
|`_complex` 结构|存储复数的实数部分和虚数部分；由 [_cabs](../c-runtime-library/reference/cabs.md) 使用。|MATH.H|
|`_CRT_ALLOC_HOOK`|用户定义的挂钩函数的类型定义。 在 [_CrtSetAllocHook](../c-runtime-library/reference/crtsetallochook.md) 中使用。|CRTDBG.H|
|`_CRT_DUMP_CLIENT`,<br /><br /> `_CRT_DUMP_CLIENT_M`|将在 [_CrtMemDumpAllObjectsSince](../c-runtime-library/reference/crtmemdumpallobjectssince.md) 中调用的回调函数的类型定义。|CRTDBG.H|
|`_CrtMemState` 结构|提供有关 C 运行时调试堆的当前状态的信息。|CRTDBG.H|
|`_CRT_REPORT_HOOK`,<br /><br /> `_CRT_REPORT_HOOKW`,<br /><br /> `_CRT_REPORT_HOOKW_M`|将在 [_CrtDbgReport](../c-runtime-library/reference/crtdbgreport-crtdbgreportw.md) 中调用的回调函数的类型定义。<br /><br /> 此函数的参数包括：报告类型、输出消息和回调函数的返回值。|CRTDBG.H|
|`dev_t`、`_dev_t` short 或 unsigned integer|表示设备句柄。|SYS\TYPES.H|
|`_diskfree_t` 结构|包含有关磁盘驱动器的信息。 由 [_getdiskfree](../c-runtime-library/reference/getdiskfree.md)**** 使用。|DOS.H 和 DIRECT.H|
|`div_t`、`ldiv_t` 和 `lldiv_t` 结构|存储由 [div](reference/div.md)、[ldiv](./reference/div.md) 和 [lldiv](./reference/div.md) 分别返回的值。|STDLIB.H|
|`errno_t` integer|用于处理 `errno` 的错误代码的函数返回类型或参数。|STDDEF.H、<br /><br /> CRTDEFS.H|
|`_exception` 结构|存储 [_matherr](../c-runtime-library/reference/matherr.md) 的错误信息。|MATH.H|
|`_EXCEPTION_POINTERS`|包含一个异常记录。 有关详细信息，请参阅 [EXCEPTION_POINTERS](/windows/win32/api/winnt/ns-winnt-exception_pointers)。|FPIEEE.H|
|`FILE` 结构|存储有关流的当前状态的信息；在所有流 I/O 操作中使用。|STDIO.H|
|`_finddata_t`、`_wfinddata_t`、`_finddata32_t`、`_wfinddata32_t`、`_finddatai64_t`、`_wfinddatai64_t`、`__finddata64_t`、`__wfinddata64_t`、`__finddata32i64_t`、`__wfinddata32i64_t`、`__finddata64i32_t`、`__wfinddata64i32_t` 结构|存储由 [_findfirst、_wfindfirst 和相关函数](../c-runtime-library/reference/findfirst-functions.md)及 [_findnext、_wfindnext 和相关函数](../c-runtime-library/reference/findnext-functions.md)返回的文件特性信息。 有关结构成员的信息，请参阅[文件名称搜索函数](../c-runtime-library/filename-search-functions.md)。|IO.H、WCHAR.H|
|`_FPIEEE_RECORD` 结构|包含有关 IEEE 浮点异常的信息；由 [_fpieee_flt](../c-runtime-library/reference/fpieee-flt.md) 传递给用户定义的陷阱处理程序。|FPIEEE.H|
|`fpos_t` (长整数、 **`__int64`** 或结构，取决于目标平台) |由 [fsetpos](../c-runtime-library/reference/fsetpos.md) 和 [fgetpos](../c-runtime-library/reference/fgetpos.md) 用于记录信息以在文件中唯一指定每个位置。|STDIO.H|
|`_fsize_t` (unsigned long integer)|用于表示文件的大小。|IO.H、<br /><br /> WCHAR.H|
|`_HEAPINFO` 结构|包含有关 [_heapwalk](../c-runtime-library/reference/heapwalk.md) 的下一个堆条目的信息。|MALLOC.H|
|`_HFILE` (void \*)|操作系统文件句柄。|CRTDBG.H|
|`imaxdiv_t`|[imaxdiv](../c-runtime-library/reference/imaxdiv.md) 函数返回的值的类型，包含商和余数。|inttypes.h|
|`ino_t`、`_ino_t` (unsigned short)|用于返回状态信息。|WCHAR.H|
|`intmax_t`|一个带符号整数类型，能够表示任何带符号整数类型的任何值。|stdint.h|
|`intptr_t` (长整数或 **`__int64`** ，具体取决于目标平台) |在 Win32 和 Win64 平台上存储指针（或 HANDLE）。|STDDEF.H 和其他包含文件|
|`jmp_buf` 数组|由 [setjmp](../c-runtime-library/reference/setjmp.md) 和 [longjmp](../c-runtime-library/reference/longjmp.md) 用于保存和还原程序环境。|SETJMP.H|
|`lconv` 结构|包含数字值在不同国家/地区的格式设置规则。 由 [localeconv](../c-runtime-library/reference/localeconv.md) 使用。|LOCALE.H|
|`_LDOUBLE`,<br /><br /> `_LONGDOUBLE`,<br /><br /> `_LDBL12`（long double 或 unsigned char 数组）|用于表示一个长双精度值。|STDLIB.H|
|`_locale_t` 结构|存储当前区域设置值；在所有区域设置特定 C 运行库中使用。|CRTDEFS.H|
|`mbstate_t`|跟踪多字节字符转换的状态。|WCHAR.H|
|`off_t`、`_off_t` long integer|表示文件偏移量值。|WCHAR.H、SYS\TYPES.H|
|`_onexit_t`,<br /><br /> `_onexit_m_t` 指针|由 [_onexit、_onexit_m](../c-runtime-library/reference/onexit-onexit-m.md) 返回。|STDLIB.H|
|指向函数的 `_PNH` 指针|[_set_new_handler](../c-runtime-library/reference/set-new-handler.md) 的参数的类型。|NEW.H|
|`ptrdiff_t` (长整数或 **`__int64`** ，具体取决于目标平台) |两个指针相减的结果。|CRTDEFS.H|
|`_purecall_handler`,<br /><br /> `_purecall_handler_m`|调用纯虚函数时调用的回调函数的类型定义。 由 [_get_purecall_handler、_set_purecall_handler](../c-runtime-library/reference/get-purecall-handler-set-purecall-handler.md) 使用。 `_purecall_handler` 函数应具有 void 返回类型。|STDLIB.H|
|`_RTC_error_fn` 类型定义|将处理运行时错误检查的函数的类型定义。 在 [_RTC_SetErrorFunc](../c-runtime-library/reference/rtc-seterrorfunc.md) 中使用。|RTCAPI.H|
|`_RTC_error_fnW` 类型定义|将处理运行时错误检查的函数的类型定义。 在 [_RTC_SetErrorFuncW](../c-runtime-library/reference/rtc-seterrorfuncw.md) 中使用。|RTCAPI.H|
|`_RTC_ErrorNumber` 枚举|定义 [_RTC_GetErrDesc](../c-runtime-library/reference/rtc-geterrdesc.md) 和 [_RTC_SetErrorType](../c-runtime-library/reference/rtc-seterrortype.md) 的错误条件。|RTCAPI.H|
|`_se_translator_function`|转换异常的回调函数的类型定义。 第一个参数是异常代码，第二个参数是异常记录。 由 [_set_se_translator](../c-runtime-library/reference/set-se-translator.md) 使用。|EH.H|
|`sig_atomic_t` integer|可修改为原子实体（即使在出现异步中断时也是如此）的对象的类型；与 [signal](../c-runtime-library/reference/signal.md) 一起使用。|SIGNAL.H|
|`size_t`（unsigned __int64 或 unsigned integer，取决于目标平台）|运算符的结果 **`sizeof`** 。|CRTDEFS.H 和其他包含文件|
|`_stat` 结构|包含 [_stat](../c-runtime-library/reference/stat-functions.md) 和 [_fstat](../c-runtime-library/reference/fstat-fstat32-fstat64-fstati64-fstat32i64-fstat64i32.md) 返回的文件状态信息。|SYS\STAT.H|
|`__stat64` 结构|包含 [_fstat64](../c-runtime-library/reference/fstat-fstat32-fstat64-fstati64-fstat32i64-fstat64i32.md)、[_stat64](../c-runtime-library/reference/stat-functions.md) 和 [_wstat64](../c-runtime-library/reference/stat-functions.md) 返回的文件状态信息。|SYS\STAT.H|
|`_stati64` 结构|包含 [_fstati64](../c-runtime-library/reference/fstat-fstat32-fstat64-fstati64-fstat32i64-fstat64i32.md)、[_stati64](../c-runtime-library/reference/stat-functions.md) 和 [_wstati64](../c-runtime-library/reference/stat-functions.md) 返回的文件状态信息。|SYS\STAT.H|
|`terminate_function` 类型定义|调用 [terminate](../c-runtime-library/reference/terminate-crt.md) 时调用的回调函数的类型定义。 由 [set_terminate](../c-runtime-library/reference/set-terminate-crt.md) 使用。|EH.H|
|`time_t`（__int64 或 long integer）|表示 [mktime](../c-runtime-library/reference/mktime-mktime32-mktime64.md)、[time](../c-runtime-library/reference/time-time32-time64.md)、[ctime、_ctime32、_ctime64、_wctime、_wctime32、_wctime64](../c-runtime-library/reference/ctime-ctime32-ctime64-wctime-wctime32-wctime64.md)、[ctime_s、_ctime32_s、_ctime64_s、_wctime_s、_wctime32_s、_wctime64_s](../c-runtime-library/reference/ctime-s-ctime32-s-ctime64-s-wctime-s-wctime32-s-wctime64-s.md)、[ctime、_ctime32、_ctime64、_wctime、_wctime32、_wctime64](../c-runtime-library/reference/ctime-ctime32-ctime64-wctime-wctime32-wctime64.md) 和 [gmtime、_gmtime32、_gmtime64](../c-runtime-library/reference/gmtime-gmtime32-gmtime64.md) 中的时间值。 自 1970 年 1 月 1 日 0:00 (UTC) 时起的秒数。 如果定义 _USE_32BIT_TIME_T，则 `time_t` 为 long integer。 如果未定义，则它为 64 位整数。|TIME.H、<br /><br /> SYS\STAT.H、<br /><br /> SYS\TIMEB.H|
|`__time32_t` (long integer)|表示 [mktime、_mktime32、_mktime64](../c-runtime-library/reference/mktime-mktime32-mktime64.md)、[ctime、_ctime32、_ctime64、_wctime、_wctime32、_wctime64](../c-runtime-library/reference/ctime-ctime32-ctime64-wctime-wctime32-wctime64.md)、[ctime_s、_ctime32_s、_ctime64_s、_wctime_s、_wctime32_s、_wctime64_s](../c-runtime-library/reference/ctime-s-ctime32-s-ctime64-s-wctime-s-wctime32-s-wctime64-s.md)、[gmtime、_gmtime32、_gmtime64](../c-runtime-library/reference/gmtime-gmtime32-gmtime64.md) 和 [localtime、_localtime32、_localtime64](../c-runtime-library/reference/localtime-localtime32-localtime64.md) 中的时间值。|CRTDEFS.H、SYS\STAT.H、<br /><br /> SYS\TIMEB.H|
|`__time64_t` (**`__int64`**)|表示 [mktime、_mktime32、_mktime64](../c-runtime-library/reference/mktime-mktime32-mktime64.md)、[_ctime64、_wctime64](../c-runtime-library/reference/ctime-ctime32-ctime64-wctime-wctime32-wctime64.md)、[ctime_s、 _ctime32_s、_ctime64_s、_wctime_s、_wctime32_s、_wctime64_s](../c-runtime-library/reference/ctime-s-ctime32-s-ctime64-s-wctime-s-wctime32-s-wctime64-s.md)、[_gmtime64](../c-runtime-library/reference/gmtime-gmtime32-gmtime64.md)、[_localtime64](../c-runtime-library/reference/localtime-localtime32-localtime64.md) 和 [_time64](../c-runtime-library/reference/time-time32-time64.md) 中的时间值。|TIME.H、<br /><br /> SYS\STAT.H、<br /><br /> SYS\TIMEB.H|
|`_timeb` 结构|由 [_ftime](../c-runtime-library/reference/ftime-ftime32-ftime64.md) 和 [_ftime_s、_ftime32_s、_ftime64_s](../c-runtime-library/reference/ftime-s-ftime32-s-ftime64-s.md) 用于存储当前系统时间。|SYS\TIMEB.H|
|`__timeb32` 结构|由 [_ftime、_ftime32、_ftime64](../c-runtime-library/reference/ftime-ftime32-ftime64.md) 和 [_ftime_s、_ftime32_s、_ftime64_s](../c-runtime-library/reference/ftime-s-ftime32-s-ftime64-s.md) 用于存储当前系统时间。|SYS\TIMEB.H|
|`__timeb64` 结构|由 [_ftime64](../c-runtime-library/reference/ftime-ftime32-ftime64.md) 和 [_ftime_s、_ftime32_s、_ftime64_s](../c-runtime-library/reference/ftime-s-ftime32-s-ftime64-s.md) 用于存储当前系统时间。|SYS\TIMEB.H|
|`tm` 结构|由 [asctime、_wasctime](../c-runtime-library/reference/asctime-wasctime.md)、[asctime_s、_wasctime_s](../c-runtime-library/reference/asctime-s-wasctime-s.md)、[gmtime、_gmtime32、_gmtime64](../c-runtime-library/reference/gmtime-gmtime32-gmtime64.md)、[gmtime_s、_gmtime32_s、_gmtime64_s](../c-runtime-library/reference/gmtime-s-gmtime32-s-gmtime64-s.md)、[localtime、_localtime32、_localtime64](../c-runtime-library/reference/localtime-localtime32-localtime64.md)、[localtime_s、_localtime32_s、_localtime64_s](../c-runtime-library/reference/localtime-s-localtime32-s-localtime64-s.md)、[mktime、_mktime32、_mktime64](../c-runtime-library/reference/mktime-mktime32-mktime64.md) 和 [strftime、wcsftime、_strftime_l, _wcsftime_l](../c-runtime-library/reference/strftime-wcsftime-strftime-l-wcsftime-l.md) 用于存储和检索时间信息。|TIME.H|
|`uintmax_t`|一个带符号整数类型，能够表示任何带符号整数类型的任何值。|stdint.h|
|`uintptr_t` (长整数或 **`__int64`** ，具体取决于目标平台) |`intptr_t` 的 unsigned integer 或 unsigned __int64 版本|STDDEF.H 和其他包含文件|
|`unexpected_function`|调用 [unexpected](../c-runtime-library/reference/unexpected-crt.md) 时调用的回调函数的类型定义。 由 [set_unexpected](../c-runtime-library/reference/set-unexpected-crt.md) 使用。|EH.H|
|`_utimbuf` 结构|存储 [_utime、_wutime](../c-runtime-library/reference/utime-utime32-utime64-wutime-wutime32-wutime64.md) 和 [_futime、_futime32、_futime64](../c-runtime-library/reference/futime-futime32-futime64.md) 用于更改文件修改日期的文件访问时间和修改时间。|SYS\UTIME.H|
|`_utimbuf32` 结构|存储 [_utime、_utime32、_utime64、_wutime、_wutime32、_wutime64](../c-runtime-library/reference/utime-utime32-utime64-wutime-wutime32-wutime64.md) 和 [_futime、_futime32、_futime64](../c-runtime-library/reference/futime-futime32-futime64.md) 用于更改文件修改日期的文件访问时间和修改时间。|SYS\UTIME.H|
|`__utimbuf64` 结构|由 [_utime64、_wutime64](../c-runtime-library/reference/utime-utime32-utime64-wutime-wutime32-wutime64.md) 和 [_futime64](../c-runtime-library/reference/futime-futime32-futime64.md) 用于存储当前时间。|SYS\UTIME.H|
|`va_list` 结构|用于保留 [va_arg](../c-runtime-library/reference/va-arg-va-copy-va-end-va-start.md) 和 [va_end](../c-runtime-library/reference/va-arg-va-copy-va-end-va-start.md) 宏所需的信息。 被调用的函数将可作为参数传递的 `va_list` 类型的变量声明为另一个函数。|STDARG.H、<br /><br /> CRTDEFS.H|
|**`wchar_t`** 宽字符|用于为国际市场编写可移植程序。|STDDEF.H、STDLIB.H、<br /><br /> CRTDEFS.H、<br /><br /> SYS\STAT.H|
|`wctrans_t` integer|表示区域设置特定字符映射。|WCTYPE.H|
|`wctype_t` integer|可表示任何语言字符集的所有字符。|WCHAR.H、<br /><br /> CRTDEFS.H|
|`wint_t` integer|可保留任何宽字符或宽文件尾值的数据对象的类型。|WCHAR.H、<br /><br /> CRTDEFS.H|

## <a name="see-also"></a>请参阅

[C 运行时库参考](../c-runtime-library/c-run-time-library-reference.md)
