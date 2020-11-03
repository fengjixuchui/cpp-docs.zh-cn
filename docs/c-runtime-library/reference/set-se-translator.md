---
title: _set_se_translator
ms.date: 02/21/2018
api_name:
- _set_se_translator
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
api_type:
- DLLExport
topic_type:
- apiref
f1_keywords:
- _set_se_translator
- set_se_translator
helpviewer_keywords:
- set_se_translator function
- exception handling, changing
- _set_se_translator function
ms.assetid: 280842bc-d72a-468b-a565-2d3db893ae0f
ms.openlocfilehash: 9de0c62b9e9a0bca0753d31ef64396e00c379253
ms.sourcegitcommit: 43cee7a0d41a062661229043c2f7cbc6ace17fa3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92008624"
---
# <a name="_set_se_translator"></a>_set_se_translator

设置每个线程的回调函数，以便将 Win32 异常 (C 结构化异常) 转换为 c + + 类型的异常。

## <a name="syntax"></a>语法

```cpp
_se_translator_function _set_se_translator(
    _se_translator_function seTransFunction
);
```

### <a name="parameters"></a>parameters

*seTransFunction*<br/>
指向您编写的 C 结构化异常转换器函数的指针。

## <a name="return-value"></a>返回值

返回指向 **_set_se_translator**注册的上一个转换器函数的指针，以便稍后可以还原以前的函数。 如果以前未设置任何函数，则返回值可用于还原默认行为;此值可以为 **`nullptr`** 。

## <a name="remarks"></a>备注

**_Set_se_translator**函数提供了一种方法，用于处理 Win32 异常 (c 结构化异常) 作为 c + + 类型的异常。 若要允许 c + + 处理程序处理每个 C 异常 **`catch`** ，请先定义一个可以使用或从中派生的 c 异常包装器类，以将特定的类类型属性指定为 C 异常。 若要使用此类，请安装自定义 C 异常转换器函数，该函数在每次引发 C 异常时由内部异常处理机制调用。 在转换器函数内，您可以引发可由匹配的 c + + 处理程序捕获的任意类型异常 **`catch`** 。

使用 **_set_se_translator**时，必须使用[/eha](../../build/reference/eh-exception-handling-model.md) 。

若要指定自定义转换函数，请使用转换函数的名称作为其参数，调用 **_set_se_translator** 。 对于具有块的堆栈上的每个函数调用，将调用您编写的转换器函数一次 **`try`** 。 没有默认的转换器函数。

转换器函数只能是引发 C++ 类型的异常。 如果它执行了除引发异常之外的任何操作（例如，对日志文件进行写入操作），您的程序可能不会按预期方式运行，因为调用转换器函数的次数是与平台相关的。

在多线程环境中，单独为每个线程维护转换器函数。 每个新线程都需要安装它自己的转换器函数。 因此，每个线程都负责处理它自己的转换。 **_set_se_translator** 特定于一个线程;另一个 DLL 可安装不同的转换函数。

你编写的 *seTransFunction* 函数必须是本机编译的函数， (未使用/clr) 编译。 它必须采用一个无符号整数和一个指向 Win32 **_EXCEPTION_POINTERS** 结构的指针作为参数。 参数分别是对 Win32 API **GetExceptionCode** 和 **GetExceptionInformation** 函数的调用的返回值。

```cpp
typedef void (__cdecl *_se_translator_function)(unsigned int, struct _EXCEPTION_POINTERS* );
```

对于 **_set_se_translator**，动态链接到 CRT 时会有影响;进程中的另一个 DLL 可能调用 **_set_se_translator** ，并将您的处理程序替换为它自己的处理程序。

当使用 **_set_se_translator** 来自托管代码 (使用/clr) 或混合本机代码和托管代码编译的代码时，请注意转换器仅影响本机代码中生成的异常。 托管代码中生成的任何托管异常（例如，在引发 `System::Exception` 时）都不会通过转换器函数进行传送。 使用 Win32 函数 **RaiseException** 或由系统异常（如被零除异常）导致的托管代码中引发的异常将通过转换器进行路由。

## <a name="requirements"></a>要求

|例程所返回的值|必需的标头|
|-------------|---------------------|
|**_set_se_translator**|\<eh.h>|

有关兼容性的详细信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

## <a name="example-catch-__try-exception-error"></a>示例： Catch __try 异常错误

此示例包装对设置结构化异常转换器的调用，并在 RAII 类中还原旧的 `Scoped_SE_Translator` 。 此类使你可以将特定于作用域的转换器引入为单个声明。 当控件离开作用域时，类析构函数将还原原始转换器。

```cpp
// crt_settrans.cpp
// compile with: cl /W4 /EHa crt_settrans.cpp
#include <stdio.h>
#include <windows.h>
#include <eh.h>
#include <exception>

class SE_Exception : public std::exception
{
private:
    const unsigned int nSE;
public:
    SE_Exception() noexcept : SE_Exception{ 0 } {}
    SE_Exception( unsigned int n ) noexcept : nSE{ n } {}
    unsigned int getSeNumber() const noexcept { return nSE; }
};

class Scoped_SE_Translator
{
private:
    const _se_translator_function old_SE_translator;
public:
    Scoped_SE_Translator( _se_translator_function new_SE_translator ) noexcept
        : old_SE_translator{ _set_se_translator( new_SE_translator ) } {}
    ~Scoped_SE_Translator() noexcept { _set_se_translator( old_SE_translator ); }
};

void SEFunc()
{
    __try
    {
        printf( "In __try, about to force exception\n" );
        int x = 5;
        int y = 0;
        int *p = &y;
        *p = x / *p;
    }
    __finally
    {
        printf( "In __finally\n" );
    }
}

void trans_func( unsigned int u, EXCEPTION_POINTERS* )
{
    throw SE_Exception( u );
}

int main()
{
    Scoped_SE_Translator scoped_se_translator{ trans_func };
    try
    {
        SEFunc();
    }
    catch( const SE_Exception& e )
    {
        printf( "Caught a __try exception, error %8.8x.\n", e.getSeNumber() );
    }
}
```

```Output
In __try, about to force exception
In __finally
Caught a __try exception, error c0000094.
```

## <a name="example-catch-se_exception-error"></a>示例： Catch SE_Exception 错误

尽管 **_set_se_translator** 提供的功能在托管代码中不可用，但只要使用指示本机代码，就可以在本机代码中使用此映射，即使本机代码位于 **/clr** 开关下的编译中 `#pragma unmanaged` 。 如果要映射的托管代码中引发了结构化异常，则必须标记生成和处理异常的代码 `#pragma unmanaged` 。 以下代码演示了可能的用途。 有关详细信息，请参阅 [Pragma 指令和 __Pragma 关键字](../../preprocessor/pragma-directives-and-the-pragma-keyword.md)。

```cpp
// crt_set_se_translator_clr.cpp
// compile with: cl /W4 /clr crt_set_se_translator_clr.cpp
#include <windows.h>
#include <eh.h>
#include <stdio.h>
#include <exception>

int thrower_func( int i ) {
   int y = 0;
   int *p = &y;
   *p = i / *p;
   return 0;
}

class SE_Exception : public std::exception
{
private:
    const unsigned int nSE;
public:
    SE_Exception() noexcept : SE_Exception{ 0 } {}
    SE_Exception( unsigned int n ) noexcept : nSE{ n } {}
    unsigned int getSeNumber() const noexcept { return nSE; }
};

class Scoped_SE_Translator
{
private:
    const _se_translator_function old_SE_translator;
public:
    Scoped_SE_Translator( _se_translator_function new_SE_translator ) noexcept
        : old_SE_translator{ _set_se_translator( new_SE_translator ) } {}
    ~Scoped_SE_Translator() noexcept { _set_se_translator( old_SE_translator ); }
};

#pragma unmanaged
void my_trans_func( unsigned int u, PEXCEPTION_POINTERS )
{
    throw SE_Exception( u );
}

void DoTest()
{
    try
    {
        thrower_func( 10 );
    }
    catch( const SE_Exception& e )
    {
        printf( "Caught SE_Exception, error %8.8x\n", e.getSeNumber() );
    }
    catch(...)
    {
        printf( "Caught unexpected SEH exception.\n" );
    }
}
#pragma managed

int main() {
    Scoped_SE_Translator scoped_se_translator{ my_trans_func };

    DoTest();
}
```

```Output
Caught SE_Exception, error c0000094
```

## <a name="see-also"></a>请参阅

[异常处理例程](../../c-runtime-library/exception-handling-routines.md)<br/>
[set_terminate](set-terminate-crt.md)<br/>
[set_unexpected](set-unexpected-crt.md)<br/>
[终止](terminate-crt.md)<br/>
[之外](unexpected-crt.md)<br/>
