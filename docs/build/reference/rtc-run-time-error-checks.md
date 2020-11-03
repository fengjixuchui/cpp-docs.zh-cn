---
title: /RTC（运行时错误检查）
description: 用于运行时错误检查的 Microsoft C/c + + 编译器/RTC 选项。
ms.date: 07/31/2020
f1_keywords:
- /rtc
- VC.Project.VCCLCompilerTool.SmallerTypeCheck
- VC.Project.VCCLCompilerTool.UninitializedVariableCheck
- VC.Project.VCCLCompilerTool.StackFrameCheck
- VC.Project.VCCLCompilerTool.BasicRuntimeChecks
helpviewer_keywords:
- /RTCs compiler option [C++]
- -RTC1 compiler option [C++]
- run-time errors, error checks
- -RTCu compiler option [C++]
- /RTC1 compiler option [C++]
- /RTCc compiler option [C++]
- /RTCu compiler option [C++]
- __MSVC_RUNTIME_CHECKS macro
- -RTCs compiler option [C++]
- RTCs compiler option
- RTC1 compiler option
- run-time errors, run-time checks
- run-time checks, /RTC option
- RTCu compiler option
- RTCc compiler option
- -RTCc compiler option [C++]
ms.assetid: 9702c558-412c-4004-acd5-80761f589368
ms.openlocfilehash: 888a81d0d5c21b0b85420a43d534c5b2742aa082
ms.sourcegitcommit: 30792632548d1c71894f9fecbe2f554294b86020
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91765229"
---
# <a name="rtc-run-time-error-checks"></a>`/RTC` (运行时错误检查) 

用于启用和禁用运行时错误检查功能，与 [runtime_checks](../../preprocessor/runtime-checks.md) 杂注一起使用。

## <a name="syntax"></a>语法

> **`/RTC1`**\
> **`/RTCc`**\
> **`/RTCs`**\
> **`/RTCu`**

## <a name="arguments"></a>参数

**`/RTC1`**<br/>
等效于 **`/RTCsu`** 。

**`/RTCc`**<br/>
报告何时向较小的数据类型分配值并导致数据丢失。 例如，它报告 **`short`** 的类型值 `0x0101` 是否分配给类型的变量 **`char`** 。

此选项可以报告要截断的情况。 例如，你希望将返回的的第一个8位 **`int`** 返回为 **`char`** 。 由于在 **`/RTCc`** 赋值导致任何信息丢失的情况下导致运行时错误，首先会屏蔽需要避免运行时错误的信息。 例如： 。

```C
#include <crtdbg.h>

char get8bits(unsigned value, int position) {
   _ASSERT(position < 32);
   return (char)(value >> position);
   // Try the following line instead:
   // return (char)((value >> position) & 0xff);
}

int main() {
   get8bits(12341235,3);
}
```

由于 **`/RTCc`** 拒绝符合标准的代码，因此 c + + 标准库不支持。 使用的代码 **`/RTCc`** 和 c + + 标准库可能会导致编译器错误 [C1189](../../error-messages/compiler-errors-1/fatal-error-c1189.md)。 您可以定义 `_ALLOW_RTCc_IN_STL` 以使警告静音并使用 **`/RTCc`** 选项。

**`/RTCs`**<br/>
启用堆栈帧运行时错误检查，如下所示：

- 将局部变量初始化为一个非零值。 此选项有助于识别在调试模式下运行时未显示的 bug。 与发布版本相比，在调试版本中，堆栈变量的值仍为零。 这是因为在发布版本中，堆栈变量的编译器优化。 一旦某个程序使用了其堆栈的区域，编译器将永远不会重置为0。 这意味着，以后使用同一堆栈区域的任何未初始化的堆栈变量都可以从以前使用此堆栈内存的过程返回剩余的值。

- 检测局部变量（如数组）的溢出和不足。 **`/RTCs`** 当访问在结构中由编译器填充导致的内存时，不会检测溢出。 通过使用 [`align`](../../cpp/align-cpp.md) 、 [ `/Zp` (结构成员对齐) ](zp-struct-member-alignment.md)或 [`pack`](../../preprocessor/pack.md) ，或者如果按要求编译器添加填充的方式对结构元素进行排序，可以发生填充。

- 堆栈指针验证，检测堆栈指针损坏。 堆栈指针损坏可能是由于调用约定不匹配造成的。 例如，使用函数指针，可以调用导出为的 DLL 中的函数，但会将指向 [`__stdcall`](../../cpp/stdcall.md) 该函数的指针声明为 [`__cdecl`](../../cpp/cdecl.md) 。

**`/RTCu`**<br/>
报告在没有初始化的情况下使用变量的时间。 例如，生成警告 C4701 的指令也可能会在下生成运行时错误 **`/RTCu`** 。 任何生成 [编译器警告 (等级1和等级 4) C4700](../../error-messages/compiler-warnings/compiler-warning-level-1-and-level-4-c4700.md) 的指令将在下生成运行时错误 **`/RTCu`** 。

不过，请考虑以下代码片段：

```cpp
int a, *b, c;
if ( 1 )
b = &a;
c = a;  // No run-time error with /RTCu
```

如果变量可能已初始化，则它不会在运行时通过进行报告 **`/RTCu`** 。 例如，变量通过指针化名后，编译器不会跟踪该变量并报告未初始化的使用情况。 实际上，可以通过获取变量的地址来初始化变量。 **`&`** 在这种情况下，运算符的工作方式类似于赋值运算符。

## <a name="remarks"></a>注解

通过运行时错误检查，你可以在运行代码中查找问题;有关详细信息，请参阅 [如何：使用本机运行时检查](/visualstudio/debugger/how-to-use-native-run-time-checks)。

可以 **`/RTC`** 在命令行中指定多个选项。 选项参数可以合并;例如，与 **`/RTCcu`** 相同 **`/RTCc /RTCu`** 。

如果你使用任意编译器选项在命令行编译程序，则 **`/RTC`** 代码中的任何杂注 [`optimize`](../../preprocessor/optimize.md) 指令将以静默方式失败。 这是因为运行时错误检查在 (优化) 生成的版本中无效。

用于 **`/RTC`** 开发生成;请勿用于 **`/RTC`** 发布版本。 **`/RTC`** 不能与编译器优化一起使用 ([ `/O` 选项 (优化代码) ](o-options-optimize-code.md)) 。 使用生成的程序映像 **`/RTC`** 稍微大一些，比使用生成的映像慢 **`/Od`** (比生成) 慢 5% **`/Od`** 。

`__MSVC_RUNTIME_CHECKS`当你使用任何选项或时，将定义预处理器指令 **`/RTC`** [`/GZ`](gz-enable-stack-frame-run-time-error-checking.md) 。

### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项

1. 打开项目的“属性页”  对话框。 有关详细信息，请参阅[在 Visual Studio 中设置 C++ 编译器和生成属性](../working-with-project-properties.md)。

1. 选择 "**配置属性**" "  >  **c/c + +**  >  **代码生成**" 属性页。

1. 修改以下一个或两个属性： **基本运行时检查** 或 **更小的类型检查**。

### <a name="to-set-this-compiler-option-programmatically"></a>以编程方式设置此编译器选项

- 请参阅 <xref:Microsoft.VisualStudio.VCProjectEngine.VCCLCompilerTool.BasicRuntimeChecks%2A> 和 <xref:Microsoft.VisualStudio.VCProjectEngine.VCCLCompilerTool.SmallerTypeCheck%2A> 属性。

## <a name="see-also"></a>请参阅

[MSVC 编译器选项](compiler-options.md)<br/>
[MSVC 编译器命令行语法](compiler-command-line-syntax.md)<br/>
[如何：使用本机运行时检查](/visualstudio/debugger/how-to-use-native-run-time-checks)
