---
title: /Qspectre
ms.date: 09/06/2019
f1_keywords:
- VC.Project.VCCLCompilerTool.SpectreMitigation
helpviewer_keywords:
- /Qspectre
ms.openlocfilehash: a4872d18fed4523e235aee70839f1e482d78f345
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92919106"
---
# <a name="qspectre"></a>/Qspectre

指定编译器生成指令以缓解某些 Spectre 变体 1 安全漏洞。

## <a name="syntax"></a>语法

> **/Qspectre**

## <a name="remarks"></a>注解

Visual Studio 2017 版本 15.5.5 及更高版本和 Visual Studio 2015 Update 3 中已提供“/Qspectre”选项，请参阅  。 它可使编译器插入说明以缓解某些 [Spectre 安全漏洞](https://spectreattack.com/spectre.pdf)。 这些漏洞称为 " *推理执行方通道攻击* "。 它们影响许多操作系统和新式处理器，包括来自 Intel、AMD 和 ARM 的处理器。

默认情况下，/Qspectre 选项处于禁用状态  。

在其初始版本中，/Qspectre 选项仅处理优化的代码  。 在 Visual Studio 2017 版本 15.7 及更高版本中，所有优化级别均支持 /Qspectre 选项  。

Microsoft Visual C++ 库也可在具有 Spectre 缓解功能的版本中使用。 可以在 Visual Studio 安装程序中下载适用于 Visual Studio 2017 及更高版本的 Spectre 缓解库。 它们位于 " **编译器"、"生成工具" 和 "运行时** " 下的 " **单个组件** " 选项卡中，并且名称中包含 "lib for Spectre"。 启用了缓解功能的 DLL 和静态运行时库可用于 Visual C++ 运行时的子集： VC + + 启动代码、vcruntime140、msvcp140、concrt140 和 vcamp140。 仅应用程序本地部署支持 Dll。 尚未修改 Visual C++ 2017 和更高版本的运行时库的内容。

你还可以安装 Spectre 的 MFC 和 ATL 库。 它们位于 **sdk、库和框架** 下的 " **单个组件** " 选项卡中。

> [!NOTE]
> 通用 Windows (UWP) 应用程序或组件没有任何版本的 Spectre 库。 不能对此类库进行应用本地部署。

### <a name="applicability"></a>适用范围

如果你的代码对跨越信任边界的数据进行操作，则建议你使用 **/Qspectre** 选项重新生成并重新部署代码，以尽快减少此问题。 此类代码的一个示例是加载可能影响执行的不受信任输入的代码。 例如，执行远程过程调用的代码，解析不受信任的输入或文件，或使用其他本地进程间通信 (IPC) 接口。 标准沙盒技术可能还不足以彻底解决问题。 在确定代码不跨越信任边界之前，请仔细调查您的沙盒。

### <a name="availability"></a>可用性

**/Qspectre** 选项在 Visual Studio 2017 版本15.5.5 中提供，在 Microsoft c + + 编译器的所有更新中， (MSVC) 在2018年1月23日或之后进行。 使用 Visual Studio 安装程序更新编译器并安装 Spectre 缓解库作为单独组件。 /Qspectre 选项也可通过修补程序在 Visual Studio 2015 Update 3 中使用  。 有关详细信息，请参阅 [KB 4338871](https://support.microsoft.com/help/4338871)。

所有版本的 Visual Studio 2017 版本15.5 和 Visual Studio 的所有预览均2017版本15.6。 包括未记录的选项 **/d2guardspecload** 。 它等效于 **/Qspectre** 的初始行为。 可以使用 /d2guardspecload 将相同的缓解功能应用于这些版本的编译器中的代码  。 建议更新生成以在支持该选项的编译器中使用 **/Qspectre** 。 **/Qspectre** 选项可能还支持更高版本的编译器中的新缓解。

### <a name="effect"></a>效果

/Qspectre 选项输出代码以缓解 Specter 变体 1、绕过边界检查  。 它通过插入充当推理性代码执行屏障的指令进行运作。 用于减轻处理器推理的特定指令取决于处理器及其微体系结构，并且可能在编译器的未来版本中发生变化。

当你启用 **/Qspectre** 选项时，编译器会尝试标识一些实例，其中的推理执行可能会绕过边界检查。 这就是插入关卡说明的位置。 务必要知道编译器可以执行的分析限制，以便识别变型1的实例。 因此，不能保证在 **/Qspectre** 下检测变体1的所有可能的实例。

### <a name="performance-impact"></a>性能影响

在多个可调整大小的代码库中， **/Qspectre** 的性能影响可能会忽略不计。 但是，不保证 **/Qspectre** 下的代码的性能不受影响。 应对代码进行基准测试，以确定该选项对性能的影响。 如果你知道在性能关键块或循环中不需要缓解，则可以通过使用 [__declspec (spectre (nomitigation) # B3 ](../../cpp/spectre.md) 指令来有选择地禁用缓解措施。 此指令在仅支持 **/d2guardspecload** 选项的编译器中不可用。

### <a name="required-libraries"></a>所需的库

**/Qspectre** 编译器选项生成隐式链接为提供 Spectre 缓解而构建的运行时库版本的代码。 以下库是可选组件，必须使用 Visual Studio 安装程序进行安装：

- 面向 Spectre \[(x86 和 x64) | (ARM) | (ARM64)] 的 MSVC 版本 version_numbers 库 
- 带有 Spectre 缓解功能的 Visual C++ ATL for \[(x86/x64) | ARM | ARM64]
- 带有 Spectre 缓解功能的 Visual C++ MFC for \[x86/x64 | ARM | ARM64]

如果使用 **/Qspectre** 生成代码，但未安装这些库，则生成系统 **会报告警告 MSB8038： Spectre 缓解功能已启用，但找不到 Spectre 缓解库** 。 如果 MFC 或 ATL 代码无法生成，并且链接器报告错误 **LNK1104：无法打开文件 "oldnames.lib"** ，则可能会导致缺少库。

### <a name="additional-information"></a>其他信息

有关详细信息，请参阅官方 [Microsoft 安全公告 ADV180002、缓解推理执行端通道漏洞的指南](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002)。 也可从 Intel [推理性执行旁道缓解措施](https://software.intel.com/sites/default/files/managed/c5/63/336996-Speculative-Execution-Side-Channel-Mitigations.pdf) 和 ARM [缓存推理旁道](https://developer.arm.com/-/media/Files/pdf/Cache_Speculation_Side-channels.pdf)获取指南。 有关 Spectre 和 Meltdown 缓解的特定于 Windows 的概述，请参阅 [了解 Windows 系统上 Spectre 和 Meltdown 缓解的性能影响](https://www.microsoft.com/security/blog/2018/01/09/understanding-the-performance-impact-of-spectre-and-meltdown-mitigations-on-windows-systems/)。 有关 MSVC 缓解措施解决的 Spectre 漏洞的概述，请参阅 c + + 团队博客上的 [MSVC 中的 Spectre 缓解措施](https://devblogs.microsoft.com/cppblog/spectre-mitigations-in-msvc./) 。

### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项

::: moniker range="msvc-160"

1. 打开项目的“属性页”  对话框。 有关详细信息，请参阅[在 Visual Studio 中设置 C++ 编译器和生成属性](../working-with-project-properties.md)。

1. 选择 " **配置属性** " " > **c/c + +** > **代码生成** " 属性页。

1. 为 **Spectre 缓解** 属性选择一个新值。 选择“确定”应用更改  。

::: moniker-end

::: moniker range="<=msvc-150"

1. 打开项目的“属性页”  对话框。 有关详细信息，请参阅[在 Visual Studio 中设置 C++ 编译器和生成属性](../working-with-project-properties.md)。

1. 选择“配置属性”“C/C++”“命令行”属性页  >  >  。

1. 在“其他选项”框中输入“/Qspectre”编译器选项  。 选择“确定”应用更改  。

::: moniker-end

### <a name="to-set-this-compiler-option-programmatically"></a>以编程方式设置此编译器选项

- 请参阅 <xref:Microsoft.VisualStudio.VCProjectEngine.VCCLCompilerTool.AdditionalOptions%2A>。

## <a name="see-also"></a>请参阅

[/Q 选项 (低级别操作) ](q-options-low-level-operations.md)<br/>
[MSVC 编译器选项](compiler-options.md)<br/>
[MSVC 编译器 Command-Line 语法](compiler-command-line-syntax.md)
