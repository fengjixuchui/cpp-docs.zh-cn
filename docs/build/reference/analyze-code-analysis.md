---
title: /analyze（代码分析）
description: Microsoft c + + 编译器/analyze 选项语法和用法。
ms.date: 07/27/2020
f1_keywords:
- VC.Project.VCCLCompilerTool.EnablePREfast
- /analyze
- VC.Project.VCCLCompilerTool.PREfastAdditionalOptions
- VC.Project.VCCLCompilerTool.PREfastAdditionalPlugins
helpviewer_keywords:
- /analyze compiler option [C++]
- -analyze compiler option [C++]
- analyze compiler option [C++]
ms.assetid: 81da536a-e030-4bd4-be18-383927597d08
ms.openlocfilehash: dcf44f1d282a9dd39205aecb4e75b59a6e8481f9
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92919093"
---
# <a name="analyze-code-analysis"></a>`/analyze` (代码分析) 

启用代码分析和控制选项。

## <a name="syntax"></a>语法

::: moniker range=">=msvc-150"

> **`/analyze`**\
> **`/analyze-`**\
> **`/analyze:autolog`**\
> **`/analyze:autolog-`**\
> **`/analyze:autolog:ext`***扩展*\
> **`/analyze:log`***filename*\
> **`/analyze:max_paths`***编号*\
> **`/analyze:only`**\
> **`/analyze:plugin`***插件-dll*\
> **`/analyze:quiet`**\
> **`/analyze:ruleset`***规则集*\
> **`/analyze:stacksize`***编号*\
> **`/analyze:WX-`**

::: moniker-end
::: moniker range="msvc-140"

> **`/analyze`**\
> **`/analyze-`**\
> **`/analyze:autolog`**\
> **`/analyze:autolog-`**\
> **`/analyze:autolog:ext`***扩展*\
> **`/analyze:log`***filename*\
> **`/analyze:max_paths`***编号*\
> **`/analyze:only`**\
> **`/analyze:plugin`***插件-dll*\
> **`/analyze:quiet`**\
> **`/analyze:stacksize`***编号*\
> **`/analyze:WX-`**

::: moniker-end

## <a name="arguments"></a>参数

**`/analyze`**\
在默认模式下打开分析。 分析输出会转到控制台或 Visual Studio **输出** 窗口，如其他错误消息。 使用 **`/analyze-`** 显式关闭分析。

**`/analyze:autolog`**\
详细分析器结果将以 XML 形式写入到文件，该文件具有与源文件相同的基名称和扩展名 *`.pftlog`* 。 **`/analyze:autolog-`** 禁用此日志文件。

**`/analyze:autolog:ext`***扩展*\
详细分析器结果将以 XML 形式写入到文件，该文件具有与源文件相同的基名称 *和扩展插件。*

**`/analyze:log`***filename*\
详细分析器结果将以 XML 形式写入 *filename* 指定的文件。

**`/analyze:max_paths`***编号*\
与此选项一起使用的 *number* 参数指定要分析的代码路径的最大数目。 如果未指定此参数，则默认情况下，该数字为256。 较大的值会导致更彻底的检查，但分析可能需要更长的时间。

**`/analyze:only`**\
通常，编译器在运行分析器后会生成代码并执行更多语法检查。 **`/analyze:only`** 选项关闭此代码生成过程。 它可以更快地进行分析，但不会发出编译器在代码生成过程中可能会发现的编译器错误和警告。 如果程序未出现代码生成错误，则分析结果可能不可靠。 建议仅在代码已通过代码生成语法检查且没有错误的情况下才使用此选项。

**`/analyze:plugin`***插件-dll*\
在运行代码分析过程中启用指定的 PREfast 插件。

::: moniker range="<=msvc-150"

LocalEspC.dll 是在 C261XX 警告范围内实现与并发相关的代码分析检查的插件。 例如， [C26100](../../code-quality/c26100.md)， [C26101](../../code-quality/c26101.md)，...，  [C26167](../../code-quality/c26167.md)。

若要运行 LocalEspC.dll，请使用此编译器选项： **`/analyze:plugin LocalEspC.dll`**

::: moniker-end
::: moniker range=">=msvc-160"

ConcurrencyCheck.dll 在 C261XX 警告范围内实现与并发相关的代码分析检查。 例如， [C26100](../../code-quality/c26100.md)， [C26101](../../code-quality/c26101.md)，...，  [C26167](../../code-quality/c26167.md)。

若要运行 ConcurrencyCheck.dll，请先从开发人员命令提示符处运行以下命令：

```cmd
set Esp.Extensions=ConcurrencyCheck.dll
```

然后使用此编译器选项： **`/analyze:plugin EspXEngine.dll`** 。

若要运行 CppCoreCheck.dll，请先从开发人员命令提示符处运行以下命令：

```cmd
set Esp.Extensions=CppCoreCheck.dll
```

然后使用此编译器选项： **`/analyze:plugin EspXEngine.dll`** 。

::: moniker-end

**`/analyze:quiet`**\
关闭到控制台或 Visual Studio **输出** 窗口的分析器输出。

::: moniker range=">=msvc-150"

**`/analyze:ruleset`***file_path. 规则集*\
允许您指定要分析的规则集，包括您自己可以创建的自定义规则集。 设置此开关后，规则引擎将更有效，因为它在运行前排除指定规则集的非成员。 否则，引擎将检查所有规则。

提供 Visual Studio 附带的规则集 *`%VSINSTALLDIR%\Team Tools\Static Analysis Tools\Rule Sets`* 。

下面的示例自定义规则集通知规则引擎检查 C6001 和 C26494。 只要该文件具有扩展名，就可以将此文件放置 *`.ruleset`* 在任何位置，并且可以在参数中提供完整路径。

```xml
<?xml version="1.0" encoding="utf-8"?>
<RuleSet Name="New Rule Set" Description="New rules to apply." ToolsVersion="15.0">
  <Rules AnalyzerId="Microsoft.Analyzers.NativeCodeAnalysis" RuleNamespace="Microsoft.Rules.Native">
    <Rule Id="C6001" Action="Warning" />
    <Rule Id="C26494" Action="Warning" />
  </Rules>
</RuleSet>
```

::: moniker-end

**`/analyze:stacksize`***编号*\
与此选项一起使用的 *number* 参数指定为其生成警告 [C6262](../../code-quality/c6262.md) 的堆栈帧的大小（以字节为单位）。 *Number* 之前的空格是可选的。 如果未指定此参数，则默认情况下，堆栈帧大小为16KB。

**`/analyze:WX-`**\
使用进行编译时，不会将代码分析警告视为错误 **`/WX`** 。 有关详细信息，请参阅[ `/WX` (Warning level) ](compiler-option-warning-level.md)。

## <a name="remarks"></a>注解

有关详细信息，请参阅 [c/c + + 代码分析概述](../../code-quality/code-analysis-for-c-cpp-overview.md) 和 [c/c + + 警告的代码分析](../../code-quality/code-analysis-for-c-cpp-warnings.md)。

### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项

1. 打开项目的“属性页”  对话框。 有关详细信息，请参阅[在 Visual Studio 中设置 C++ 编译器和生成属性](../working-with-project-properties.md)。

1. 选择 **配置属性**  >  **代码分析**  >  **常规** 属性页。

1. 修改一个或多个 **代码分析** 属性。

### <a name="to-set-this-compiler-option-programmatically"></a>以编程方式设置此编译器选项

1. 请参阅 <xref:Microsoft.VisualStudio.VCProjectEngine.VCCLCompilerTool.EnablePREfast%2A>。

## <a name="see-also"></a>请参阅

[MSVC 编译器选项](compiler-options.md)\
[MSVC 编译器 Command-Line 语法](compiler-command-line-syntax.md)
