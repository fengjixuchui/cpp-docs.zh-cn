---
title: Pragma 指令和 __pragma 关键字
description: '介绍了 Microsoft Visual C 和 c + + (MSVC 中提供的杂注指令) '
ms.date: 10/30/2020
f1_keywords:
- '#pragma'
helpviewer_keywords:
- '#pragma directives, C/C++'
- __pragma keyword
- pragma directives, C/C++
- pragmas, C/C++
- preprocessor
- pragmas
- preprocessor, pragmas
- pragma directives (#pragma)
ms.assetid: 9867b438-ac64-4e10-973f-c3955209873f
ms.openlocfilehash: bf4bbdcf74808edd8ef54149f8258f47bd94c600
ms.sourcegitcommit: 4abc6c4c9694f91685cfd77940987e29a51e3143
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93238403"
---
# <a name="pragma-directives-and-the-__pragma-keyword"></a>Pragma 指令和 __pragma 关键字

杂注指令指定计算机或操作系统特定的编译器功能。 **__Pragma** 关键字，它是特定于 Microsoft 编译器的，可让你在宏定义中编码杂注指令。

## <a name="syntax"></a>语法

> **#`pragma`***标记-字符串*\
> **`__pragma(`***标记-字符串* **`)`** 两个前导下划线-Microsoft 特定扩展 **`_Pragma(`** *字符串文本* **`)`** //C99

## <a name="remarks"></a>备注

C 和 C++ 的每个实现均支持某些对其主机或操作系统唯一的功能。 例如，某些程序必须对内存中的数据位置进行精确控制，或控制某些函数接收参数的方式。 **#Pragma** 指令为每个编译器提供了一种提供计算机和操作系统特定功能的方法，同时保持与 c 和 c + + 语言的总体兼容性。

杂注是计算机或操作系统特定于定义的，并且对于每个编译器通常是不同的。 杂注可用于条件指令，提供新的预处理器功能，或向编译器提供实现定义的信息。

*标记字符串* 是表示特定编译器指令和参数（如果有）的一系列字符。 数字符号 ( **#** ) 必须是包含杂注的行上的第一个非空白字符。 空白字符可以分隔数字符号和词 "pragma"。 **#Pragma** 后，编写转换器可分析为预处理令牌的任何文本。 **#Pragma** 的参数受宏展开的限制。

*字符串文本* 是的输入 `_Pragma` 。 删除外部引号和前导/尾随空格。 `\"` 将替换为 `"` ，并 `\\` 将替换为 `\` 。

编译器在找到它无法识别的杂注时发出警告并继续编译。

Microsoft C 和 C++ 编译器识别以下杂注：

:::row:::
   :::column span="":::
      [`alloc_text`](../preprocessor/alloc-text.md)\
      [`auto_inline`](../preprocessor/auto-inline.md)\
      [`bss_seg`](../preprocessor/bss-seg.md)\
      [`check_stack`](../preprocessor/check-stack.md)\
      [`code_seg`](../preprocessor/code-seg.md)\
      [`comment`](../preprocessor/comment-c-cpp.md)\
      [`component`](../preprocessor/component.md)\
      [`conform`](../preprocessor/conform.md)<sup>1</sup>\
      [`const_seg`](../preprocessor/const-seg.md)\
      [`data_seg`](../preprocessor/data-seg.md)\
      [`deprecated`](../preprocessor/deprecated-c-cpp.md)
   :::column-end:::
   :::column span="":::
      [`detect_mismatch`](../preprocessor/detect-mismatch.md)\
      [`fenv_access`](../preprocessor/fenv-access.md)\
      [`float_control`](../preprocessor/float-control.md)\
      [`fp_contract`](../preprocessor/fp-contract.md)\
      [`function`](../preprocessor/function-c-cpp.md)\
      [`hdrstop`](../preprocessor/hdrstop.md)\
      [`include_alias`](../preprocessor/include-alias.md)\
      [`init_seg`](../preprocessor/init-seg.md)<sup>1</sup>\
      [`inline_depth`](../preprocessor/inline-depth.md)\
      [`inline_recursion`](../preprocessor/inline-recursion.md)
   :::column-end:::
   :::column span="":::
      [`intrinsic`](../preprocessor/intrinsic.md)\
      [`loop`](../preprocessor/loop.md)<sup>1</sup>\
      [`make_public`](../preprocessor/make-public.md)\
      [`managed`](../preprocessor/managed-unmanaged.md)\
      [`message`](../preprocessor/message.md)\
      [`omp`](../preprocessor/omp.md)\
      [`once`](../preprocessor/once.md)\
      [`optimize`](../preprocessor/optimize.md)\
      [`pack`](../preprocessor/pack.md)\
      [`pointers_to_members`](../preprocessor/pointers-to-members.md)<sup>1</sup>
   :::column-end:::
   :::column span="":::
      [`pop_macro`](../preprocessor/pop-macro.md)\
      [`push_macro`](../preprocessor/push-macro.md)\
      [`region`，endregion](../preprocessor/region-endregion.md)\
      [`runtime_checks`](../preprocessor/runtime-checks.md)\
      [`section`](../preprocessor/section.md)\
      [`setlocale`](../preprocessor/setlocale.md)\
      [`strict_gs_check`](../preprocessor/strict-gs-check.md)\
      [`unmanaged`](../preprocessor/managed-unmanaged.md)\
      [`vtordisp`](../preprocessor/vtordisp.md)<sup>1</sup>\
      [`warning`](../preprocessor/warning.md)
   :::column-end:::
:::row-end:::

<sup>1</sup> 仅由 c + + 编译器支持。

## <a name="pragmas-and-compiler-options"></a>杂注和编译器选项

某些杂注提供与编译器选项相同的功能。 在源代码中遇到杂注时，将重写编译器选项所指定的行为。 例如，如果指定 [了/zp8](../build/reference/zp-struct-member-alignment.md)，则可以用 [pack](../preprocessor/pack.md)的代码的特定部分重写此编译器设置：

```cmd
cl /Zp8 some_file.cpp
```

```cpp
// some_file.cpp - packing is 8
// ...
#pragma pack(push, 1) - packing is now 1
// ...
#pragma pack(pop) - packing is 8 again
// ...
```

## <a name="the-__pragma-keyword"></a>__Pragma ( # A1 关键字

编译器还支持 Microsoft 特定的 **`__pragma`** 关键字，该关键字具有与指令相同的功能 **`#pragma`** 。 差别在于， **`__pragma`** 关键字可在宏定义中以内联方式使用。 **`#pragma`** 指令在宏定义中无法使用，因为编译器会将指令中的数字符号字符（ ( "# ) "）解释为 [字符串化运算符 ( # )](../preprocessor/stringizing-operator-hash.md)。

下面的代码示例演示如何 **`__pragma`** 在宏中使用关键字。 此代码从 "编译器 COM 支持示例" 的 ACDUAL 示例中的 *mfcdual.h* 标头中摘录内容：

```cpp
#define CATCH_ALL_DUAL \
CATCH(COleException, e) \
{ \
_hr = e->m_sc; \
} \
AND_CATCH_ALL(e) \
{ \
__pragma(warning(push)) \
__pragma(warning(disable:6246)) /*disable _ctlState prefast warning*/ \
AFX_MANAGE_STATE(pThis->m_pModuleState); \
__pragma(warning(pop)) \
_hr = DualHandleException(_riidSource, e); \
} \
END_CATCH_ALL \
return _hr; \
```

## <a name="the-_pragma-preprocessing-operator-c99-c11"></a>`_Pragma`预处理运算符 (C99，c + + 11) 

`_Pragma` 类似于 Microsoft 特定的 [`__pragma`](#the-__pragma-keyword) 关键字，只不过它是标准的一部分。 它是在 C99 中为 C 引入的。 对于 c + +，它是在 c + + 11 中引入的。

 它允许将杂注放入宏定义中。 它有一个前导下划线 `_` ，而不是由 Microsoft 特定的关键字拥有的两个前导下划线 `__` ，第一个字母大写。

字符串应为后面放置语句的字符串 *`#pragma`* 。 例如：

```c
#pragma message("--the #pragma way")
_Pragma ("message( \"the _Pragma way\")") 
```

引号和反斜杠应进行转义，如上所示。 忽略无法识别的杂注字符串。

下面的代码示例演示 **`_Pragma`** 当条件表达式恰好为常量时，如果不希望收到警告，则在类似于断言的宏中如何使用关键字。 

宏定义为多语句宏使用 do/while (0) 方法，以便可以像使用一个语句一样使用它。 有关详细信息，请参阅 Stack Overflow 上的 [C 多行宏](https://stackoverflow.com/questions/1067226/c-multi-line-macro-do-while0-vs-scope-block) 。 _Pragma 语句仅适用于其后的代码行。

```C
// Compile with /W4

#include <stdio.h>
#include <stdlib.h>

#define MY_ASSERT(BOOL_EXPRESSION) \
    do { \
        _Pragma("warning(suppress: 4127)") /* C4127 conditional expression is constant */  \
        if (!(BOOL_EXPRESSION)) {   \
            printf("MY_ASSERT FAILED: \"" #BOOL_EXPRESSION "\" on %s(%d)", __FILE__, __LINE__); \
            exit(-1); \
        } \
    } while (0)

int main()
{
    MY_ASSERT(0 && "Note that there is no warning: C4127 conditional expression is constant");

    return 0;
}
```

## <a name="see-also"></a>另请参阅

[C/c + + 预处理器参考](../preprocessor/c-cpp-preprocessor-reference.md)\
[C 杂注](../c-language/c-pragmas.md)\
[关键字](../cpp/keywords-cpp.md)
