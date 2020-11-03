---
title: 多线程和区域设置
ms.date: 11/04/2016
helpviewer_keywords:
- locales [C++], multithreading
- multithreading [C++], locales
- threading [C++], locales
- per-thread locale
ms.assetid: d6fb159a-eaca-4130-a51a-f95d62f71485
ms.openlocfilehash: 82b410c592e5b68737514dda5a864c7bd15f6dc3
ms.sourcegitcommit: 43cee7a0d41a062661229043c2f7cbc6ace17fa3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92008588"
---
# <a name="multithreading-and-locales"></a>多线程和区域设置

C 运行时库和 c + + 标准库均提供对更改程序区域设置的支持。 本主题讨论在多线程应用程序中使用这两个库的区域设置功能时所发生的问题。

## <a name="remarks"></a>备注

使用 C 运行时库，可以使用和函数创建多线程应用程序 `_beginthread` `_beginthreadex` 。 本主题仅介绍使用这些函数创建的多线程应用程序。 有关详细信息，请参阅 [_beginthread _beginthreadex](../c-runtime-library/reference/beginthread-beginthreadex.md)。

若要使用 C 运行时库更改区域设置，请使用 [setlocale](../preprocessor/setlocale.md) 函数。 在以前版本的 Visual C++ 中，此函数将始终修改整个应用程序中的区域设置。 现在支持在每个线程的基础上设置区域设置。 这是使用 [_configthreadlocale](../c-runtime-library/reference/configthreadlocale.md) 函数实现的。 若要指定 [setlocale](../preprocessor/setlocale.md) 只应更改当前线程的区域设置，请 `_configthreadlocale(_ENABLE_PER_THREAD_LOCALE)` 在该线程中调用。 相反，调用 `_configthreadlocale(_DISABLE_PER_THREAD_LOCALE)` 将导致该线程使用全局区域设置，并且对该线程中的 [setlocale](../preprocessor/setlocale.md) 的任何调用都将更改所有未显式启用每线程区域设置的线程的区域设置。

若要使用 c + + 运行时库更改区域设置，请使用 [区域设置类](../standard-library/locale-class.md)。 通过调用 [locale：： global](../standard-library/locale-class.md#global) 方法，你可以更改每个未显式启用每线程区域设置的每个线程的区域设置。 若要更改单个线程或应用程序部分的区域设置，只需 `locale` 在该线程或部分代码中创建对象的实例。

> [!NOTE]
> 调用 [locale：： global](../standard-library/locale-class.md#global) 会更改 c + + 标准库和 c 运行时库的区域设置。 但是，调用 [setlocale](../preprocessor/setlocale.md) 只会更改 C 运行库的区域设置;c + + 标准库不受影响。

下面的示例演示如何使用 [setlocale](../preprocessor/setlocale.md) 函数、 [locale 类](../standard-library/locale-class.md)和 [_configthreadlocale](../c-runtime-library/reference/configthreadlocale.md) 函数在多个不同的方案中更改应用程序的区域设置。

## <a name="example-change-locale-with-per-thread-locale-enabled"></a>示例：已启用每线程区域设置更改区域设置

在此示例中，主线程生成两个子线程。 线程 A 的第一个线程通过调用来启用每个线程的区域设置 `_configthreadlocale(_ENABLE_PER_THREAD_LOCALE)` 。 第二个线程（即线程 B）以及主线程不启用每线程区域设置。 然后，线程 A 继续使用 C 运行时库的 [setlocale](../preprocessor/setlocale.md) 函数更改区域设置。

由于线程 A 启用了每线程区域设置，因此只有线程 A 中的 C 运行库函数开始使用 "法语" 区域设置。 线程 B 和主线程中的 C 运行库函数继续使用 "C" 区域设置。 此外，由于 [setlocale](../preprocessor/setlocale.md) 不会影响 c + + 标准库区域设置，因此所有 c + + 标准库对象将继续使用 "C" 区域设置。

```cpp
// multithread_locale_1.cpp
// compile with: /EHsc /MD
#include <clocale>
#include <cstdio>
#include <locale>
#include <process.h>
#include <windows.h>

#define NUM_THREADS 2
using namespace std;

unsigned __stdcall RunThreadA(void *params);
unsigned __stdcall RunThreadB(void *params);

BOOL localeSet = FALSE;
HANDLE printMutex = CreateMutex(NULL, FALSE, NULL);

int main()
{
    HANDLE threads[NUM_THREADS];

    unsigned aID;
    threads[0] = (HANDLE)_beginthreadex(
        NULL, 0, RunThreadA, NULL, 0, &aID);

    unsigned bID;
    threads[1] = (HANDLE)_beginthreadex(
        NULL, 0, RunThreadB, NULL, 0, &bID);

    WaitForMultipleObjects(2, threads, TRUE, INFINITE);

    printf_s("[Thread main] Per-thread locale is NOT enabled.\n");
    printf_s("[Thread main] CRT locale is set to \"%s\"\n",
        setlocale(LC_ALL, NULL));
    printf_s("[Thread main] locale::global is set to \"%s\"\n",
        locale().name().c_str());

    CloseHandle(threads[0]);
    CloseHandle(threads[1]);
    CloseHandle(printMutex);

    return 0;
}

unsigned __stdcall RunThreadA(void *params)
{
    _configthreadlocale(_ENABLE_PER_THREAD_LOCALE);
    setlocale(LC_ALL, "french");
    localeSet = TRUE;

    WaitForSingleObject(printMutex, INFINITE);
    printf_s("[Thread A] Per-thread locale is enabled.\n");
    printf_s("[Thread A] CRT locale is set to \"%s\"\n",
        setlocale(LC_ALL, NULL));
    printf_s("[Thread A] locale::global is set to \"%s\"\n\n",
        locale().name().c_str());
    ReleaseMutex(printMutex);

    return 1;
}

unsigned __stdcall RunThreadB(void *params)
{
    while (!localeSet)
        Sleep(100);

    WaitForSingleObject(printMutex, INFINITE);
    printf_s("[Thread B] Per-thread locale is NOT enabled.\n");
    printf_s("[Thread B] CRT locale is set to \"%s\"\n",
        setlocale(LC_ALL, NULL));
    printf_s("[Thread B] locale::global is set to \"%s\"\n\n",
        locale().name().c_str());
    ReleaseMutex(printMutex);

    return 1;
}
```

```Output
[Thread A] Per-thread locale is enabled.
[Thread A] CRT locale is set to "French_France.1252"
[Thread A] locale::global is set to "C"

[Thread B] Per-thread locale is NOT enabled.
[Thread B] CRT locale is set to "C"
[Thread B] locale::global is set to "C"

[Thread main] Per-thread locale is NOT enabled.
[Thread main] CRT locale is set to "C"
[Thread main] locale::global is set to "C"
```

## <a name="example-change-global-locale-with-per-thread-locale-enabled"></a>示例：在每个线程的区域设置启用时更改全局区域设置

在此示例中，主线程生成两个子线程。 线程 A 的第一个线程通过调用来启用每个线程的区域设置 `_configthreadlocale(_ENABLE_PER_THREAD_LOCALE)` 。 第二个线程（即线程 B）以及主线程不启用每线程区域设置。 然后，线程 A 继续使用 c + + 标准库的 [locale：： global](../standard-library/locale-class.md#global) 方法更改区域设置。

由于线程 A 启用了每线程区域设置，因此只有线程 A 中的 C 运行库函数开始使用 "法语" 区域设置。 线程 B 和主线程中的 C 运行库函数继续使用 "C" 区域设置。 但是，由于 [locale：： global](../standard-library/locale-class.md#global) 方法更改区域设置 "global"，所有线程中的所有 c + + 标准库对象均使用 "法语" 区域设置启动。

```cpp
// multithread_locale_2.cpp
// compile with: /EHsc /MD
#include <clocale>
#include <cstdio>
#include <locale>
#include <process.h>
#include <windows.h>

#define NUM_THREADS 2
using namespace std;

unsigned __stdcall RunThreadA(void *params);
unsigned __stdcall RunThreadB(void *params);

BOOL localeSet = FALSE;
HANDLE printMutex = CreateMutex(NULL, FALSE, NULL);

int main()
{
    HANDLE threads[NUM_THREADS];

    unsigned aID;
    threads[0] = (HANDLE)_beginthreadex(
        NULL, 0, RunThreadA, NULL, 0, &aID);

    unsigned bID;
    threads[1] = (HANDLE)_beginthreadex(
        NULL, 0, RunThreadB, NULL, 0, &bID);

    WaitForMultipleObjects(2, threads, TRUE, INFINITE);

    printf_s("[Thread main] Per-thread locale is NOT enabled.\n");
    printf_s("[Thread main] CRT locale is set to \"%s\"\n",
        setlocale(LC_ALL, NULL));
    printf_s("[Thread main] locale::global is set to \"%s\"\n",
        locale().name().c_str());

    CloseHandle(threads[0]);
    CloseHandle(threads[1]);
    CloseHandle(printMutex);

    return 0;
}

unsigned __stdcall RunThreadA(void *params)
{
    _configthreadlocale(_ENABLE_PER_THREAD_LOCALE);
    locale::global(locale("french"));
    localeSet = TRUE;

    WaitForSingleObject(printMutex, INFINITE);
    printf_s("[Thread A] Per-thread locale is enabled.\n");
    printf_s("[Thread A] CRT locale is set to \"%s\"\n",
        setlocale(LC_ALL, NULL));
    printf_s("[Thread A] locale::global is set to \"%s\"\n\n",
        locale().name().c_str());
    ReleaseMutex(printMutex);

    return 1;
}

unsigned __stdcall RunThreadB(void *params)
{
    while (!localeSet)
        Sleep(100);

    WaitForSingleObject(printMutex, INFINITE);
    printf_s("[Thread B] Per-thread locale is NOT enabled.\n");
    printf_s("[Thread B] CRT locale is set to \"%s\"\n",
        setlocale(LC_ALL, NULL));
    printf_s("[Thread B] locale::global is set to \"%s\"\n\n",
        locale().name().c_str());
    ReleaseMutex(printMutex);

    return 1;
}
```

```Output
[Thread A] Per-thread locale is enabled.
[Thread A] CRT locale is set to "French_France.1252"
[Thread A] locale::global is set to "French_France.1252"

[Thread B] Per-thread locale is NOT enabled.
[Thread B] CRT locale is set to "C"
[Thread B] locale::global is set to "French_France.1252"

[Thread main] Per-thread locale is NOT enabled.
[Thread main] CRT locale is set to "C"
[Thread main] locale::global is set to "French_France.1252"
```

## <a name="example-change-locale-without-per-thread-locale-enabled"></a>示例：在没有启用每线程区域设置的情况下更改区域设置

在此示例中，主线程生成两个子线程。 线程 A 的第一个线程通过调用来启用每个线程的区域设置 `_configthreadlocale(_ENABLE_PER_THREAD_LOCALE)` 。 第二个线程（即线程 B）以及主线程不启用每线程区域设置。 然后，线程 B 继续使用 C 运行时库的 [setlocale](../preprocessor/setlocale.md) 函数更改区域设置。

由于线程 B 没有启用每个线程的区域设置，线程 B 和中的 C 运行库函数使用 "法语" 区域设置开始。 线程 A 中的 C 运行库函数继续使用 "C" 区域设置，因为线程 A 启用了每线程区域设置。 此外，由于 [setlocale](../preprocessor/setlocale.md) 不会影响 c + + 标准库区域设置，因此所有 c + + 标准库对象将继续使用 "C" 区域设置。

```cpp
// multithread_locale_3.cpp
// compile with: /EHsc /MD
#include <clocale>
#include <cstdio>
#include <locale>
#include <process.h>
#include <windows.h>

#define NUM_THREADS 2
using namespace std;

unsigned __stdcall RunThreadA(void *params);
unsigned __stdcall RunThreadB(void *params);

BOOL localeSet = FALSE;
BOOL configThreadLocaleCalled = FALSE;
HANDLE printMutex = CreateMutex(NULL, FALSE, NULL);

int main()
{
    HANDLE threads[NUM_THREADS];

    unsigned aID;
    threads[0] = (HANDLE)_beginthreadex(
        NULL, 0, RunThreadA, NULL, 0, &aID);

    unsigned bID;
    threads[1] = (HANDLE)_beginthreadex(
        NULL, 0, RunThreadB, NULL, 0, &bID);

    WaitForMultipleObjects(2, threads, TRUE, INFINITE);

    printf_s("[Thread main] Per-thread locale is NOT enabled.\n");
    printf_s("[Thread main] CRT locale is set to \"%s\"\n",
        setlocale(LC_ALL, NULL));
    printf_s("[Thread main] locale::global is set to \"%s\"\n",
        locale().name().c_str());

    CloseHandle(threads[0]);
    CloseHandle(threads[1]);
    CloseHandle(printMutex);

    return 0;
}

unsigned __stdcall RunThreadA(void *params)
{
    _configthreadlocale(_ENABLE_PER_THREAD_LOCALE);
    configThreadLocaleCalled = TRUE;
    while (!localeSet)
        Sleep(100);

    WaitForSingleObject(printMutex, INFINITE);
    printf_s("[Thread A] Per-thread locale is enabled.\n");
    printf_s("[Thread A] CRT locale is set to \"%s\"\n",
        setlocale(LC_ALL, NULL));
    printf_s("[Thread A] locale::global is set to \"%s\"\n\n",
        locale().name().c_str());
    ReleaseMutex(printMutex);

    return 1;
}

unsigned __stdcall RunThreadB(void *params)
{
    while (!configThreadLocaleCalled)
        Sleep(100);
    setlocale(LC_ALL, "french");
    localeSet = TRUE;

    WaitForSingleObject(printMutex, INFINITE);
    printf_s("[Thread B] Per-thread locale is NOT enabled.\n");
    printf_s("[Thread B] CRT locale is set to \"%s\"\n",
        setlocale(LC_ALL, NULL));
    printf_s("[Thread B] locale::global is set to \"%s\"\n\n",
        locale().name().c_str());
    ReleaseMutex(printMutex);

    return 1;
}
```

```Output
[Thread B] Per-thread locale is NOT enabled.
[Thread B] CRT locale is set to "French_France.1252"
[Thread B] locale::global is set to "C"

[Thread A] Per-thread locale is enabled.
[Thread A] CRT locale is set to "C"
[Thread A] locale::global is set to "C"

[Thread main] Per-thread locale is NOT enabled.
[Thread main] CRT locale is set to "French_France.1252"
[Thread main] locale::global is set to "C"
```

## <a name="example-change-global-locale-without-per-thread-locale-enabled"></a>示例：在没有启用每线程区域设置的情况下更改全局区域设置

在此示例中，主线程生成两个子线程。 线程 A 的第一个线程通过调用来启用每个线程的区域设置 `_configthreadlocale(_ENABLE_PER_THREAD_LOCALE)` 。 第二个线程（即线程 B）以及主线程不启用每线程区域设置。 然后，线程 B 继续使用 c + + 标准库的 [locale：： global](../standard-library/locale-class.md#global) 方法更改区域设置。

由于线程 B 没有启用每个线程的区域设置，线程 B 和中的 C 运行库函数使用 "法语" 区域设置开始。 线程 A 中的 C 运行库函数继续使用 "C" 区域设置，因为线程 A 启用了每线程区域设置。 但是，由于 [locale：： global](../standard-library/locale-class.md#global) 方法更改区域设置 "global"，所有线程中的所有 c + + 标准库对象均使用 "法语" 区域设置启动。

```cpp
// multithread_locale_4.cpp
// compile with: /EHsc /MD
#include <clocale>
#include <cstdio>
#include <locale>
#include <process.h>
#include <windows.h>

#define NUM_THREADS 2
using namespace std;

unsigned __stdcall RunThreadA(void *params);
unsigned __stdcall RunThreadB(void *params);

BOOL localeSet = FALSE;
BOOL configThreadLocaleCalled = FALSE;
HANDLE printMutex = CreateMutex(NULL, FALSE, NULL);

int main()
{
    HANDLE threads[NUM_THREADS];

    unsigned aID;
    threads[0] = (HANDLE)_beginthreadex(
        NULL, 0, RunThreadA, NULL, 0, &aID);

    unsigned bID;
    threads[1] = (HANDLE)_beginthreadex(
        NULL, 0, RunThreadB, NULL, 0, &bID);

    WaitForMultipleObjects(2, threads, TRUE, INFINITE);

    printf_s("[Thread main] Per-thread locale is NOT enabled.\n");
    printf_s("[Thread main] CRT locale is set to \"%s\"\n",
        setlocale(LC_ALL, NULL));
    printf_s("[Thread main] locale::global is set to \"%s\"\n",
        locale().name().c_str());

    CloseHandle(threads[0]);
    CloseHandle(threads[1]);
    CloseHandle(printMutex);

    return 0;
}

unsigned __stdcall RunThreadA(void *params)
{
    _configthreadlocale(_ENABLE_PER_THREAD_LOCALE);
    configThreadLocaleCalled = TRUE;
    while (!localeSet)
        Sleep(100);

    WaitForSingleObject(printMutex, INFINITE);
    printf_s("[Thread A] Per-thread locale is enabled.\n");
    printf_s("[Thread A] CRT locale is set to \"%s\"\n",
        setlocale(LC_ALL, NULL));
    printf_s("[Thread A] locale::global is set to \"%s\"\n\n",
        locale().name().c_str());
    ReleaseMutex(printMutex);

    return 1;
}

unsigned __stdcall RunThreadB(void *params)
{
    while (!configThreadLocaleCalled)
        Sleep(100);
    locale::global(locale("french"));
    localeSet = TRUE;

    WaitForSingleObject(printMutex, INFINITE);
    printf_s("[Thread B] Per-thread locale is NOT enabled.\n");
    printf_s("[Thread B] CRT locale is set to \"%s\"\n",
        setlocale(LC_ALL, NULL));
    printf_s("[Thread B] locale::global is set to \"%s\"\n\n",
        locale().name().c_str());
    ReleaseMutex(printMutex);

    return 1;
}
```

```Output
[Thread B] Per-thread locale is NOT enabled.
[Thread B] CRT locale is set to "French_France.1252"
[Thread B] locale::global is set to "French_France.1252"

[Thread A] Per-thread locale is enabled.
[Thread A] CRT locale is set to "C"
[Thread A] locale::global is set to "French_France.1252"

[Thread main] Per-thread locale is NOT enabled.
[Thread main] CRT locale is set to "French_France.1252"
[Thread main] locale::global is set to "French_France.1252"
```

## <a name="see-also"></a>请参阅

[针对旧代码的多线程支持 (Visual C++)](multithreading-support-for-older-code-visual-cpp.md)<br/>
[_beginthread、_beginthreadex](../c-runtime-library/reference/beginthread-beginthreadex.md)<br/>
[_configthreadlocale](../c-runtime-library/reference/configthreadlocale.md)<br/>
[setlocale](../preprocessor/setlocale.md)<br/>
[国际化](../c-runtime-library/internationalization.md)<br/>
[区域设置](../c-runtime-library/locale.md)<br/>
[\<clocale>](../standard-library/clocale.md)<br/>
[\<locale>](../standard-library/locale.md)<br/>
[locale 类](../standard-library/locale-class.md)
