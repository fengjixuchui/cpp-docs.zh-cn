---
title: 使用 C++ 核心准则检查程序
description: 如何为 C++ Core Guidelines 设置和使用 Microsoft c + + 代码分析规则。
ms.date: 07/27/2020
ms.topic: conceptual
dev_langs:
- CPP
ms.openlocfilehash: a041ecd4053360ebfc0da198efe9a3decb73ffd2
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924791"
---
# <a name="use-the-c-core-guidelines-checkers"></a>使用 C++ Core Guidelines 检查器

C++ Core Guidelines 是有关 c + + 专家和设计人员创建的 c + + 编码的一组可移植指导原则、规则和最佳实践。 Visual Studio 当前支持将这些规则作为 c + + 代码分析工具的一部分。 默认情况下，在 Visual Studio 2017 和 Visual Studio 2019 中安装核心准则检查程序。 它们 [可用作 Visual Studio 2015 的 NuGet 包](#vs2015_corecheck)。

## <a name="the-c-core-guidelines-project"></a>C++ Core Guidelines 项目

C++ Core Guidelines 是由 Bjarne Stroustrup 和其他用户创建的，它是一种安全、有效地使用现代 c + + 的指南。 这些指南强调了静态类型安全和资源安全性。 它们确定了消除或最小化语言中最容易出错的部分的方法。 它们还建议如何使代码更简单、更可靠，并获得更好的性能。 这些准则由标准的 c + + 基础维护。 若要了解详细信息，请参阅文档， [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)，并访问 [GitHub](https://github.com/isocpp/CppCoreGuidelines)上的 C++ Core Guidelines 文档项目文件。

## <a name="enable-the-c-core-check-guidelines-in-code-analysis"></a>启用代码分析中的 C++ Core Check 准则

::: moniker range="<=msvc-150"

Microsoft 本机建议规则集中包含 C++ Core Check 规则的子集。 这是启用代码分析时默认运行的规则集。

### <a name="to-enable-code-analysis-on-your-project"></a>对项目启用代码分析

1. 打开项目的 "  **属性页** " 对话框。

1. 选择 " **配置属性** > **代码分析** " 属性页。

1. 选中 " **生成时启用代码分析"** 复选框。

![代码分析常规设置的属性页](media/cppcorecheck_codeanalysis_general.png)

若要启用其他核心检查规则，请打开下拉列表并选择要包括的规则集：

![其他 C++ Core Check 规则集的下拉列表](media/cppcorecheck_codeanalysis_extensions.png)

::: moniker-end
::: moniker range=">=msvc-160"

Microsoft 本机建议规则集中包含 C++ Core Check 规则的子集。 这是启用 Microsoft 代码分析时默认运行的规则集。

### <a name="to-enable-code-analysis-on-your-project"></a>对项目启用代码分析：

1. 打开项目的 "  **属性页** " 对话框。

1. 选择 " **配置属性** > **代码分析** " 属性页。

1. 设置 " **生成时启用代码分析"** 和 " **启用 Microsoft 代码分析** " 属性。

你还可以选择运行所有受支持的 C++ Core Check 规则，或选择你自己的子集运行：

### <a name="to-enable-additional-core-check-rules"></a>启用其他核心检查规则

1. 打开项目的 "  **属性页** " 对话框。

1. 选择 " **配置属性** > **代码分析** " > **Microsoft** 属性页。

1. 打开 " **活动规则** " 下拉列表，然后选择 " **选择多个规则集** "。

1. 在 " **添加或删除规则集** " 对话框中，选择要包括的规则集。

::: moniker-end

## <a name="examples"></a>示例

下面是 C++ Core Check 规则可以发现的一些问题的示例：

```cpp
// CoreCheckExample.cpp
// Add CppCoreCheck package and enable code analysis in build for warnings.

int main()
{
    int arr[10];           // warning C26494
    int* p = arr;          // warning C26485

    [[gsl::suppress(bounds.1)]] // This attribute suppresses Bounds rule #1
    {
        int* q = p + 1;    // warning C26481 (suppressed)
        p = q++;           // warning C26481 (suppressed)
    }

    return 0;
}
```

此示例演示了 C++ Core Check 规则可以发现的几个警告：

- C26494 是规则类型。5：始终初始化对象。

- C26485 是规则界限。3：没有数组到指针的衰减。

- C26481 是规则界限。1：不要使用指针算法。 请改用 `span`。

安装并启用 C++ Core Check 代码分析规则集，然后编译此代码。 代码分析输出前两个警告，并取消第三个警告。 下面是 Visual Studio 2015 中示例代码的生成输出：

```Output
1>------ Build started: Project: CoreCheckExample, Configuration: Debug Win32 ------
1>  CoreCheckExample.cpp
1>  CoreCheckExample.vcxproj -> C:\Users\username\documents\visual studio 2015\Projects\CoreCheckExample\Debug\CoreCheckExample.exe
1>  CoreCheckExample.vcxproj -> C:\Users\username\documents\visual studio 2015\Projects\CoreCheckExample\Debug\CoreCheckExample.pdb (Full PDB)
c:\users\username\documents\visual studio 2015\projects\corecheckexample\corecheckexample\corecheckexample.cpp(6): warning C26494: Variable 'arr' is uninitialized. Always initialize an object. (type.5: http://go.microsoft.com/fwlink/p/?LinkID=620421)
c:\users\username\documents\visual studio 2015\projects\corecheckexample\corecheckexample\corecheckexample.cpp(7): warning C26485: Expression 'arr': No array to pointer decay. (bounds.3: http://go.microsoft.com/fwlink/p/?LinkID=620415)
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

C++ Core Guidelines 可帮助你编写更好、更安全的代码。 但是，你可能会发现不应应用规则或配置文件的实例。 可以轻松地直接在代码中将其取消。 您可以使用 `[[gsl::suppress]]` 属性来防止 C++ Core Check 检测和报告以下代码块中的任何规则冲突。 您可以标记各个语句以禁止显示特定规则。 甚至可以通过编写 `[[gsl::suppress(bounds)]]` 而不包含特定的规则号来禁止整个边界配置文件。

## <a name="supported-rule-sets"></a>支持的规则集

将新规则添加到 C++ Core Guidelines 检查器时，为预先存在的代码生成的警告数可能会增加。 您可以使用预定义的规则集来筛选要启用的规则类型。 你将在 [Visual Studio C++ Core Check 引用](code-analysis-for-cpp-corecheck.md)中找到大多数规则的参考文章。

- **算术规则** ：用于检测算术 [溢出](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Res-overflow)、 [未签名的操作](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Res-unsigned)和 [位操作](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Res-nonnegative)的规则。 <sup>15.6</sup>

- **边界规则** ：强制实施 [C++ Core Guidelines 的边界配置文件](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#probounds-bounds-safety-profile)。 <sup>15.3</sup>

- **类规则** ：几个将重点放在正确使用特殊成员函数和虚拟规范的规则。 它们是为 [类和类层次结构](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#S-class)推荐的检查的子集。<sup>15.5</sup>

- **并发规则** ：捕获不良防护对象声明的单个规则。 有关详细信息，请参阅 [与并发相关的准则](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#S-concurrency)。<sup>15.5</sup>

- **Const 规则** ： [从 C++ Core Guidelines 强制执行与常量有关的检查](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#con-constants-and-immutability)。 <sup>15.3</sup>

- **声明规则** ： [接口准则](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#S-interfaces) 中的几个规则，这些规则重点介绍全局变量的声明方式。 <sup>15.5</sup>

- **枚举规则** ：这些规则 [从 C++ Core Guidelines 强制执行与枚举相关的检查](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#S-enum)。 <sup>16.3</sup>

- **实验性规则** 这些是一些有用的试验性 C++ Core Check 规则，但不能用于日常使用。 试用并 [提供反馈](https://developercommunity.visualstudio.com/content/idea/post.html?space=62)。<sup>16.0</sup>

- **函数规则** ：有助于采用说明符的两个检查 **`noexcept`** 。 它们是用于 [清除函数设计和实现](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#S-functions)的指导原则。<sup>15.5</sup>

- **GSL 规则** ：这些规则强制执行与 [C++ Core Guidelines 中的准则支持库](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#S-gsl)相关的检查。 <sup>15.7</sup>

- **生存期规则** ：这些规则强制执行 [C++ Core Guidelines 的生存期配置文件](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#prolifetime-lifetime-safety-profile)。 <sup>15.7</sup>

- **所有者指针规则** ：强制执行与 [ \<T> C++ Core Guidelines 中的所有者相关的资源管理检查](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#r-resource-management)。 <sup>15.3</sup>

- **原始指针规则** ：强制执行 [与 C++ Core Guidelines 中的原始指针相关的资源管理检查](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#r-resource-management)。 <sup>15.3</sup>

- **共享指针规则** ：这是强制执行 [资源管理](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#S-resource) 指导原则的一部分。 <sup>15.5</sup> 我们添加了几个特定于共享指针如何传递到函数或在本地使用的规则。

- **STL 规则** ：这些规则强制执行与 [c + + 标准库相关的检查， (STL) 的 C++ Core Guidelines](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#S-stdlib)。 <sup>15.7</sup>

- **样式规则** ：一个简单但重要的检查，它 ban 使用 [goto](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Res-goto)。 <sup>15.5</sup> 在 c + + 中，这是提高编码样式和使用表达式和语句的第一步。

- **类型规则** ：强制实施 [C++ Core Guidelines 的类型配置文件](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#prosafety-type-safety-profile)。 <sup>15.3</sup>

- **唯一的指针规则** ：强制执行与 [C++ Core Guidelines 中具有唯一指针语义的类型相关的资源管理检查](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#r-resource-management)。 <sup>15.3</sup>

- **C++ Core Check 规则** ：此规则集包含 [C++ Core Guidelines](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#c-core-guidelines)中当前实现的所有检查，实验性规则除外。

<sup>15.3</sup> 这些规则首先出现在 Visual Studio 2017 版本 15.3 \ 中
<sup>15.5</sup> 这些规则首先出现在 Visual Studio 2017 版本 15.5 \ 中
<sup>15.6</sup> 这些规则首先出现在 Visual Studio 2017 版本 15.6 \ 中
<sup>15.7</sup> 这些规则首先出现在 Visual Studio 2017 版本 15.7 \ 中
<sup>16.0</sup> 这些规则首先出现在 Visual Studio 2019 版本 16.0 \ 中
<sup>16.3</sup> Visual Studio 2019 版本16.3 中首次出现这些规则

可以选择将警告仅限制为一个或多个组。 **本机的最小** 和 **本机建议** 规则集包括 C++ Core Check 规则和其他 PREfast 检查。

::: moniker range="<=msvc-150"

若要查看可用的规则集，请打开 " **项目属性** " 对话框。 在 " **属性页** " 对话框中，选择 **配置属性**  >  **代码分析**  >  **常规** 属性页。 然后，打开 " **规则集** " 组合框中的下拉列表，查看可用的规则集。 若要生成自定义规则集组合，请选择 " **选择多个规则集** "。 " **添加或删除规则集** " 对话框列出了可以选择的规则。 有关在 Visual Studio 中使用规则集的详细信息，请参阅 [使用规则集指定要运行的 c + + 规则](using-rule-sets-to-specify-the-cpp-rules-to-run.md)。

::: moniker-end
::: moniker range=">=msvc-160"

若要查看可用的规则集，请打开 " **项目属性** " 对话框。 在 " **属性页** " 对话框中，选择 **配置属性**  >  **代码分析**  >  **Microsoft** 属性页。 然后，打开 " **活动规则** " 组合框中的下拉列表，查看可用的规则集。 若要生成自定义规则集组合，请选择 " **选择多个规则集** "。 " **添加或删除规则集** " 对话框列出了可以选择的规则。 有关在 Visual Studio 中使用规则集的详细信息，请参阅 [使用规则集指定要运行的 c + + 规则](using-rule-sets-to-specify-the-cpp-rules-to-run.md)。

::: moniker-end

## <a name="macros"></a>宏

C++ Core Guidelines 检查器附带了标头文件，该文件定义了宏，使您可以更轻松地在代码中禁止显示所有类别的警告：

```cpp
ALL_CPPCORECHECK_WARNINGS
CPPCORECHECK_TYPE_WARNINGS
CPPCORECHECK_RAW_POINTER_WARNINGS
CPPCORECHECK_CONST_WARNINGS
CPPCORECHECK_OWNER_POINTER_WARNINGS
CPPCORECHECK_UNIQUE_POINTER_WARNINGS
CPPCORECHECK_BOUNDS_WARNINGS
```

这些宏对应于规则集，并将其扩展到以空格分隔的警告数字列表。 使用适当的 pragma 构造，可以配置对项目或代码部分感兴趣的有效规则集。 在下面的示例中，代码分析仅对缺少的常量修饰符发出警告：

```cpp
#include <CppCoreCheck\Warnings.h>
#pragma warning(disable: ALL_CPPCORECHECK_WARNINGS)
#pragma warning(default: CPPCORECHECK_CONST_WARNINGS)
```

## <a name="attributes"></a>属性

Microsoft c + + 编译器限制了对属性的支持 `[[gsl::suppress]]` 。 它可用于取消对函数内的 expression 和 block 语句的警告。

```cpp
// Suppress only warnings from the 'r.11' rule in expression.
[[gsl::suppress(r.11)]] new int;

// Suppress all warnings from the 'r' rule group (resource management) in block.
[[gsl::suppress(r)]]
{
    new int;
}

// Suppress only one specific warning number.
// For declarations, you may need to use the surrounding block.
// Macros are not expanded inside of attributes.
// Use plain numbers instead of macros from the warnings.h.
[[gsl::suppress(26400)]]
{
    int *p = new int;
}
```

## <a name="suppress-analysis-by-using-command-line-options"></a>使用命令行选项禁止分析

您可以在文件的属性页中使用命令行选项来禁止显示项目或单个文件的警告，而不是 #pragmas。 例如，若要对文件禁用警告 C26400，请执行以下操作：

1. 在 **解决方案资源管理器** 中右键单击该文件，然后选择 " **属性** "。

1. 在 " **属性页** " 对话框中，选择 " **配置属性** " "  >  **c/c + +**  >  **命令行** " 属性页。

1. 在 " **其他选项** " 编辑框中，添加 *`/wd26400`* 。

您可以使用命令行选项通过指定为文件暂时禁用所有代码分析 **`/analyze-`** 。 你将看到警告 *D9025 使用 "/analyze-" 替代 "/analyze* "，这会提醒你稍后重新启用代码分析。

## <a name="enable-the-c-core-guidelines-checker-on-specific-project-files"></a><a name="corecheck_per_file"></a> 启用特定项目文件的 C++ Core Guidelines 检查器

有时，执行焦点代码分析并仍使用 Visual Studio IDE 是非常有用的。 对于大型项目，请尝试下面的示例方案。 它可以节省生成时间，并更容易筛选结果：

1. 在命令外壳中，设置 `esp.extension` 和 `esp.annotationbuildlevel` 环境变量。

1. 若要继承这些变量，请从命令行界面打开 Visual Studio。

1. 加载项目并打开其属性。

1. 启用代码分析，选取适当的规则集，但不要启用代码分析扩展。

1. 在 C++ Core Guidelines 检查器中转到要分析的文件，然后打开其属性。

1. 选择 " **配置属性** " "  >  **c/c + +**  >  **命令行** " "  >  **其他选项** " 并添加 *`/analyze:plugin EspXEngine.dll`*

1. 禁用预编译头 ( **配置属性**  >  **C/c + +**  >  **预编译标头** ) 。 这是必需的，因为扩展引擎可能会尝试从预编译标头 (PCH) 读取其内部信息。 如果 PCH 是用默认项目选项编译的，则它将不兼容。

1. 重新生成项目。 常见的 PREFast 检查应在所有文件上运行。 由于默认情况下未启用 C++ Core Guidelines 检查器，因此它只应在配置为使用它的文件上运行。

## <a name="how-to-use-the-c-core-guidelines-checker-outside-of-visual-studio"></a>如何在 Visual Studio 外使用 C++ Core Guidelines 检查器

你可以使用自动生成中的 C++ Core Guidelines 检查。

### <a name="msbuild"></a>MSBuild

本机代码分析检查器 (PREfast) 通过自定义目标文件集成到 MSBuild 环境中。 您可以使用 "项目属性" 来启用它，并添加 C++ Core Guidelines 检查器 (基于 PREfast) 的检查器：

```xml
  <PropertyGroup>
    <EnableCppCoreCheck>true</EnableCppCoreCheck>
    <CodeAnalysisRuleSet>CppCoreCheckRules.ruleset</CodeAnalysisRuleSet>
    <RunCodeAnalysis>true</RunCodeAnalysis>
  </PropertyGroup>
```

请确保在导入该文件之前添加这些属性 *`Microsoft.Cpp.targets`* 。 可以选取特定规则集或创建自定义规则集。 或者，使用包含其他 PREfast 检查的默认规则集。

只能对指定的文件运行 c + + Core 检查器。 使用与 [前面所述](#corecheck_per_file)相同的方法，但使用 MSBuild 文件。 可以使用项设置环境变量 `BuildMacro` ：

```xml
<ItemGroup>
    <BuildMacro Include="Esp_AnnotationBuildLevel">
      <EnvironmentVariable>true</EnvironmentVariable>
      <Value>Ignore</Value>
    </BuildMacro>
    <BuildMacro Include="Esp_Extensions">
      <EnvironmentVariable>true</EnvironmentVariable>
      <Value>CppCoreCheck.dll</Value>
    </BuildMacro>
</ItemGroup>
```

如果不想修改项目文件，则可以在命令行上传递属性：

```cmd
msbuild /p:EnableCppCoreCheck=true /p:RunCodeAnalysis=true /p:CodeAnalysisRuleSet=CppCoreCheckRules.ruleset ...
```

### <a name="non-msbuild-projects"></a>非 MSBuild 项目

如果你使用的生成系统不依赖于 MSBuild，你仍可以运行检查器。 若要使用它，需要熟悉代码分析引擎配置的一些内部机制。 在 Visual Studio 的未来版本中，我们不保证支持这些内部机制。

代码分析需要一些环境变量和编译器命令行选项。 建议使用 **本机工具命令提示** 环境，这样就不必搜索编译器的特定路径，包括目录，等等。

- **环境变量**
  - `set esp.extensions=cppcorecheck.dll` 这会告知引擎加载 C++ Core Guidelines 模块。
  - `set esp.annotationbuildlevel=ignore` 这将禁用处理 SAL 批注的逻辑。 批注不会影响 C++ Core Guidelines 检查器中的代码分析，但其处理时间可能会很长 () 。 此设置是可选的，但强烈建议使用此设置。
  - `set caexcludepath=%include%` 我们强烈建议你禁用在标准标头上触发的警告。 你可以在此处添加更多路径，例如项目中的公共标头的路径。

- **命令行选项**
  - **`/analyze`**  启用代码分析 (也考虑使用 **`/analyze:only`** 和 **`/analyze:quiet`** ) 。
  - **`/analyze:plugin EspXEngine.dll`** 此选项将代码分析扩展引擎加载到 PREfast 中。 此引擎反过来会加载 C++ Core Guidelines 检查器。

## <a name="use-the-guideline-support-library"></a>使用准则支持库

准则支持库 (GSL) 旨在帮助你遵循核心准则。 GSL 包括一些定义，使你可以用更安全的替代方法替换容易出错的构造。 例如，可以将一 `T*, length` 对参数替换为 `span<T>` 类型。 GSL 在上可用 [http://www.nuget.org/packages/Microsoft.Gsl](https://www.nuget.org/packages/Microsoft.Gsl) 。 库是开源的，因此可以查看源、进行注释或做出贡献。 可在中找到该项目 [https://github.com/Microsoft/GSL](https://github.com/Microsoft/GSL) 。

::: moniker range="msvc-140"

## <a name="use-the-c-core-check-guidelines-in-visual-studio-2015-projects"></a><a name="vs2015_corecheck"></a> 使用 Visual Studio 2015 项目中的 C++ Core Check 准则

如果使用 Visual Studio 2015，默认情况下不会安装 C++ Core Check 代码分析规则集。 还需要执行其他步骤，然后才能在 Visual Studio 2015 中启用 C++ Core Check 代码分析工具。 Microsoft 通过使用 NuGet 包为 Visual Studio 2015 项目提供支持。 此包的名称为 CppCoreCheck，可从获取 [http://www.nuget.org/packages/Microsoft.CppCoreCheck](https://www.nuget.org/packages/Microsoft.CppCoreCheck) 。 此程序包需要至少安装了 Visual Studio 2015 Update 1。

包还会安装另一个包作为依赖项，仅限标头的准则支持库 (GSL) 。 GitHub 上还提供了 GSL [https://github.com/Microsoft/GSL](https://github.com/Microsoft/GSL) 。

由于在 Visual Studio 2015 中加载代码分析规则的方式，你必须将 `Microsoft.CppCoreCheck` NuGet 包安装到要检查的每个 c + + 项目中。

### <a name="to-add-the-microsoftcppcorecheck-package-to-your-project-in-visual-studio-2015"></a>将 CppCoreCheck 包添加到 Visual Studio 2015 中的项目

1. 在 **解决方案资源管理器** 中，右键单击以打开要将包添加到的解决方案中项目的上下文菜单。 选择 " **管理 Nuget 包** "，打开 **NuGet 包管理器** 。

1. 在 " **NuGet 包管理器** " 窗口中，搜索 CppCoreCheck。

    !["Nuget 包管理器" 窗口显示 CppCoreCheck 包](../code-quality/media/cppcorecheck_nuget_window.png)

1. 选择 CppCoreCheck 包，然后选择 " **安装** " 按钮，将规则添加到项目。

   在 *`.targets`* 项目上启用代码分析时，NuGet 包会向项目添加一个附加的 MSBuild 文件。 *`.targets`* 文件将 C++ Core Check 规则作为附加扩展添加到 Visual Studio 代码分析工具。 安装包后，可以使用 "属性页" 对话框启用或禁用已发布和实验性规则。

::: moniker-end

## <a name="see-also"></a>请参阅

- [Visual Studio C++ Core Check 参考](code-analysis-for-cpp-corecheck.md)
