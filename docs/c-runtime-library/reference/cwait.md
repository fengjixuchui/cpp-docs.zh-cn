---
title: _cwait
description: Microsoft Visual C 运行时函数的 API 参考 `_cwait()` 。
ms.date: 10/23/2020
api_name:
- _cwait
- _o__cwait
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
- api-ms-win-crt-process-l1-1-0.dll
- api-ms-win-crt-private-l1-1-0.dll
api_type:
- DLLExport
topic_type:
- apiref
f1_keywords:
- _cwait
helpviewer_keywords:
- cwait function
- _cwait function
ms.assetid: d9b596b5-45f4-4e03-9896-3f383cb922b8
ms.openlocfilehash: 5b4c4db3c40645b947583b722d345c2e80dcaa8e
ms.sourcegitcommit: faecabcdd12ff53eb79dc0df193fc3567f2f037c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92639102"
---
# <a name="_cwait"></a>_cwait

请等待，直到另一个进程终止。

> [!IMPORTANT]
> 此 API 不能用于在 Windows 运行时中执行的应用程序。 有关详细信息，请参阅[通用 Windows 平台应用中不支持的 CRT 函数](../../cppcx/crt-functions-not-supported-in-universal-windows-platform-apps.md)。

## <a name="syntax"></a>语法

```C
intptr_t _cwait(
   int *termstat,
   intptr_t procHandle,
   int action
);
```

### <a name="parameters"></a>参数

*`termstat`*\
指向将存储指定进程的结果代码的缓冲区的指针或 **`NULL`** 。

*`procHandle`*\
要在其上等待的进程的句柄 (即，必须在 **_cwait** 之前终止的进程才能返回) 。

*`action`*\
**`NULL`** ： Windows 操作系统应用程序忽略;对于其他应用程序：要在上执行的操作代码 *`procHandle`* 。

## <a name="return-value"></a>返回值

成功完成指定的进程后，将返回指定进程的句柄，并将设置 *`termstat`* 为指定进程返回的结果代码。 否则，将返回-1，并 **`errno`** 按如下所示设置。

|值|说明|
|-----------|-----------------|
|**`ECHILD`**|指定进程不存在、 *`procHandle`* 无效或对 [`GetExitCodeProcess`](/windows/win32/api/processthreadsapi/nf-processthreadsapi-getexitcodeprocess) 或 API 的调用 [`WaitForSingleObject`](/windows/win32/api/synchapi/nf-synchapi-waitforsingleobject) 失败。|
|**`EINVAL`**|*`action`* 无效。|

有关这些和其他返回代码的详细信息，请参阅 [`errno, _doserrno, _sys_errlist, and _sys_nerr`](../../c-runtime-library/errno-doserrno-sys-errlist-and-sys-nerr.md) 。

## <a name="remarks"></a>注解

**`_cwait`** 函数等待由提供的指定进程的进程 ID 终止 *`procHandle`* 。 *`procHandle`* 传递给的值 **`_cwait`** 应为对创建指定进程的函数的调用所返回的值 [`_spawn`](../../c-runtime-library/spawn-wspawn-functions.md) 。 如果进程 ID 在调用前终止 **`_cwait`** ，则 **`_cwait`** 立即返回。 **`_cwait`** 任何进程都可以使用等待) 存在有效句柄 (任何其他已知进程 *`procHandle`* 。

*`termstat`* 指向将存储指定进程的返回代码的缓冲区。 的值 *`termstat`* 指示是否通过调用 Windows API 正常终止了指定进程 [`ExitProcess`](/windows/win32/api/processthreadsapi/nf-processthreadsapi-exitprocess) 。 **`ExitProcess`** 如果指定进程调用 **`exit`** 或 **`_exit`** 、从返回 **`main`** 或到达的末尾，则将在内部调用 **`main`** 。 有关传递回的值的详细信息 *`termstat`* ，请参阅 [GetExitCodeProcess](/windows/win32/api/processthreadsapi/nf-processthreadsapi-getexitcodeprocess)。 如果 **`_cwait`** 通过使用 **`NULL`** 的值调用，则 *`termstat`* 不会存储指定进程的返回代码。

*`action`* 由于在这些环境中没有实现父子关系，因此 Windows 操作系统会忽略参数。

除非对 *`procHandle`* 当前进程或线程)  (句柄为-1 或-2，否则将关闭句柄。 在这种情况下，请不要使用返回的句柄。

默认情况下，此函数的全局状态的作用域限定为应用程序。 若要更改此项，请参阅 [CRT 中的全局状态](../global-state.md)。

## <a name="requirements"></a>要求

|例程所返回的值|必需的标头|可选标头|
|-------------|---------------------|---------------------|
|**`_cwait`**|\<process.h>|\<errno.h>|

有关兼容性的详细信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

## <a name="example"></a>示例

```C
// crt_cwait.c
// compile with: /c
// This program launches several processes and waits
// for a specified process to finish.

#define _CRT_RAND_S

#include <windows.h>
#include <process.h>
#include <stdlib.h>
#include <stdio.h>
#include <time.h>

// Macro to get a random integer within a specified range
#define getrandom( min, max ) (( (rand_s (&number), number) % (int)((( max ) + 1 ) - ( min ))) + ( min ))

struct PROCESS
{
    intptr_t hProcess;
    char    name[40];
} process[4] = { { 0, "Ann" }, { 0, "Beth" }, { 0, "Carl" }, { 0, "Dave" } };

int main(int argc, char* argv[])
{
    int termstat, c;
    unsigned int number;

    srand((unsigned)time(NULL));    // Seed randomizer

    // If no arguments, this is the calling process
    if (argc == 1)
    {
        // Spawn processes in numeric order
        for (c = 0; c < 4; c++) {
            _flushall();
            process[c].hProcess = _spawnl(_P_NOWAIT, argv[0], argv[0],
                process[c].name, NULL);
        }

        // Wait for randomly specified process, and respond when done
        c = getrandom(0, 3);
        printf("Come here, %s.\n", process[c].name);
        _cwait(&termstat, process[c].hProcess, _WAIT_CHILD);
        printf("Thank you, %s.\n", process[c].name);

    }
    // If there are arguments, this must be a spawned process
    else
    {
        // Delay for a period that's determined by process number
        Sleep((argv[1][0] - 'A' + 1) * 1000L);
        printf("Hi, Dad. It's %s.\n", argv[1]);
    }
}
```

输出的顺序因运行而异。

```Output
Hi, Dad. It's Ann.
Come here, Ann.
Thank you, Ann.
Hi, Dad. It's Beth.
Hi, Dad. It's Carl.
Hi, Dad. It's Dave.
```

## <a name="see-also"></a>另请参阅

[进程和环境控制](../../c-runtime-library/process-and-environment-control.md)\
[_spawn，_wspawn 函数](../../c-runtime-library/spawn-wspawn-functions.md)
