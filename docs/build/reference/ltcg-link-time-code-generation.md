---
title: /LTCG（链接时间代码生成）
description: MSVC 链接器选项/LTCG 为全程序优化启用链接时间代码生成。
ms.date: 07/08/2020
f1_keywords:
- VC.Project.VCLinkerTool.LinkTimeCodeGeneration
- VC.Project.VCConfiguration.WholeProgramOptimization
- VC.Project.VCCLWCECompilerTool.WholeProgramOptimization
- /ltcg
- VC.Project.VCCLCompilerTool.WholeProgramOptimization
helpviewer_keywords:
- link-time code generation in C++ linker
- /LTCG linker option
- -LTCG linker option
- LTCG linker option
ms.assetid: 788c6f52-fdb8-40c2-90af-4026ea2cf2e2
ms.openlocfilehash: 6c0009e5236f33119ed411dc81ce6a4385f21a2a
ms.sourcegitcommit: f7fbdc39d73e1fb3793c396fccf7a1602af7248b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/02/2020
ms.locfileid: "91662263"
---
# <a name="ltcg-link-time-code-generation"></a>`/LTCG` (的链接时间代码生成) 

使用 **`/LTCG`** 执行全程序优化，或创建按配置优化 (PGO) 检测、执行定型并创建按配置优化的生成。

## <a name="syntax"></a>语法

> **`/LTCG`**[**`:`**{**`INCREMENTAL`**|**`NOSTATUS`**|**`STATUS`**|**`OFF`**}]

这些选项自 Visual Studio 2015 开始已弃用：

> **`/LTCG:`**{**`PGINSTRUMENT`**|**`PGOPTIMIZE`**|**`PGUPDATE`**}

### <a name="arguments"></a>参数

**`INCREMENTAL`**<br/>
 (可选) 指定链接器仅对受编辑影响的文件（而不是整个项目）应用整个程序优化或链接时间代码生成 (LTCG) 。 默认情况下，如果指定了，则不设置此标志 **`/LTCG`** ，并且通过使用全程序优化链接整个项目。

**`NOSTATUS`** &#124; **`STATUS`**<br/>
（可选）指定链接器是否显示用来指示链接的完成百分比的进度指示器。 默认情况下，不显示此状态信息。

**`OFF`**<br/>
（可选）禁用链接时代码生成。 此行为与 **`/LTCG`** 命令行中未指定时的行为相同。

**`PGINSTRUMENT`**<br/>
（可选）这些选项自 Visual Studio 2015 开始已弃用。 相反，请使用 **`/LTCG`** 和[ `/GENPROFILE` `/FASTGENPROFILE` 或](genprofile-fastgenprofile-generate-profiling-instrumented-build.md)为按配置文件优化生成检测的生成。 从检测过的运行中收集的数据用于创建优化的映像。 有关详细信息，请参阅[按配置优化](../profile-guided-optimizations.md)。 此选项的缩写形式是 **`/LTCG:PGI`** 。

**`PGOPTIMIZE`**<br/>
（可选）这些选项自 Visual Studio 2015 开始已弃用。 请改用 **`/LTCG`** 和  [`/USEPROFILE`](useprofile.md) 来生成优化的映像。 有关详细信息，请参阅[按配置优化](../profile-guided-optimizations.md)。 此选项的缩写形式是 **`/LTCG:PGO`** 。

**`PGUPDATE`**<br/>
（可选）这些选项自 Visual Studio 2015 开始已弃用。 请改用 **`/LTCG`** 和  **`/USEPROFILE`** 来重新生成优化的映像。 有关详细信息，请参阅[按配置优化](../profile-guided-optimizations.md)。 此选项的缩写形式是 **`/LTCG:PGU`** 。

## <a name="remarks"></a>备注

**`/LTCG`** 选项通知链接器调用编译器并执行全程序优化。 还可以执行按配置优化。 有关详细信息，请参阅[按配置优化](../profile-guided-optimizations.md)。

但在以下情况下，不能将链接器选项添加到和 **`/LTCG`** **`/USEPROFILE`** 选项的上 pgo 初始化组合中未指定的的 PGO 组合中 **`/LTCG`** **`/GENPROFILE`** ：

- [`/BASE`](base-base-address.md)

- [`/FIXED`](fixed-fixed-base-address.md)

- **`/LTCG`**

- [`/MAP`](map-generate-mapfile.md)

- [`/MAPINFO`](mapinfo-include-information-in-mapfile.md)

- [`/NOLOGO`](nologo-suppress-startup-banner-linker.md)

- [`/OUT`](out-output-file-name.md)

- [`/PGD`](pgd-specify-database-for-profile-guided-optimizations.md)

- [`/PDB`](pdb-use-program-database.md)

- [`/PDBSTRIPPED`](pdbstripped-strip-private-symbols.md)

- [`/STUB`](stub-ms-dos-stub-file-name.md)

- [`/VERBOSE`](verbose-print-progress-messages.md)

**`/LTCG`** **`/GENPROFILE`** 使用和生成时，无需指定与用于初始化 PGO 的和选项一起指定的任何链接器选项 **`/LTCG`** **`/USEPROFILE`** ; 它们都是隐含的。

本文的其余部分将讨论由完成的链接时间代码生成 **`/LTCG`** 。

**`/LTCG`** 隐含 [`/GL`](gl-whole-program-optimization.md) 。

如果链接器通过使用编译的模块 **`/GL`** 或 MSIL 模块 (查看[ `.netmodule` 作为链接器输入](netmodule-files-as-linker-input.md)) 的文件，则链接器将调用链接时代码生成。 如果在将 **`/LTCG`** **`/GL`** 或 MSIL 模块传递给链接器时未显式指定，链接器最终将检测到这种情况，并通过使用重新启动链接 **`/LTCG`** 。 在将 **`/LTCG`** **`/GL`** 和 MSIL 模块传递给链接器以获得尽可能最快的生成性能时，请显式指定。

为了获得更快的性能，请使用 **`/LTCG:INCREMENTAL`** 。 此选项通知链接器仅重新优化受源文件更改影响的文件，而不是整个项目。 此选项可显著减少所需的链接时间。 此选项与 [增量链接](incremental-link-incrementally.md)不是相同的选项。

**`/LTCG`** 不适用于 [`/INCREMENTAL`](incremental-link-incrementally.md) 。

当 **`/LTCG`** 用于链接使用、、或编译的模块时，将 [`/Og`](og-global-optimizations.md) [`/O1`](o1-o2-minimize-size-maximize-speed.md) [`/O2`](o1-o2-minimize-size-maximize-speed.md) [`/Ox`](ox-full-optimization.md) 执行以下优化：

- 跨模块内联

- 过程间寄存器分配（仅限 64 位操作系统）

- 自定义调用约定（仅限 x86）

- 小型 TLS 置换（仅限 x86）

- 堆栈双倍字长对齐（仅限 x86）

- 改进了内存消歧（改善全局变量和输入参数的干扰信息）

> [!NOTE]
> 链接器确定编译每个函数时采用哪种优化并在链接时应用相同的优化。

使用 **`/LTCG`** 并 **`/O2`** 导致双对齐优化。

如果 **`/LTCG`** **`/O1`** 指定了和，则不执行双对齐。 如果对应用程序中的大部分函数进行了编译以实现速度，则有几个为大小编译的函数 (例如，通过使用 [`optimize`](../../preprocessor/optimize.md) 杂注) ，编译器会将为大小优化的函数加倍，前提是它们调用需要双重对齐的函数。

如果编译器可标识函数的所有调用站点，编译器将忽略函数上的显式调用约定修饰符并尝试优化函数的调用约定：

- 在寄存器中传递参数

- 重新排列参数以进行对齐

- 删除未使用的参数

如果通过函数指针调用函数，或者从使用编译的模块外部调用函数 **`/GL`** ，则编译器不会尝试优化函数的调用约定。

> [!NOTE]
> 如果你使用 **`/LTCG`** 和重新定义 `mainCRTStartup` ，你的应用程序可能具有与在初始化全局对象之前执行的用户代码相关的无法预测的行为。 有三种方法可以解决此问题：不要重新定义 `mainCRTStartup` ，不使用编译包含的文件， `mainCRTStartup` 也可以 **`/LTCG`** 静态初始化全局变量和对象。

### <a name="ltcg-and-msil-modules"></a>`/LTCG` 和 MSIL 模块

当指定时，使用和编译的模块 [`/GL`](gl-whole-program-optimization.md) [`/clr`](clr-common-language-runtime-compilation.md) 可用作链接器的输入 **`/LTCG`** 。

- **`/LTCG`** 可接受本机对象文件，并 (使用) 编译混合本机/托管对象文件 **`/clr`** 。 在 **`/clr:pure`** **`/clr:safe`** visual studio 2015 和更高版本中2017不支持和编译器选项。

- **`/LTCG:PGI`** 不接受使用和编译的本机模块 **`/GL`****`/clr`**

### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项

1. 打开项目“属性页” **** 对话框。 请参阅[在 Visual Studio 中设置 C++ 编译器和生成属性](../working-with-project-properties.md)。

1. 选择 "**配置属性**" "  >  **常规**" 属性页。

1. 修改 **** “全程序优化”属性。

您还可以 **`/LTCG`** 通过在**Build**  >  菜单栏上选择 "按优化生成**配置文件**"，或在项目的快捷菜单上选择 "按配置优化" 选项之一来应用于特定生成。

### <a name="to-set-this-compiler-option-programmatically"></a>以编程方式设置此编译器选项

- 请参阅 <xref:Microsoft.VisualStudio.VCProjectEngine.VCLinkerTool.LinkTimeCodeGeneration%2A>。

## <a name="see-also"></a>请参阅

[MSVC 链接器参考](linking.md)\
[MSVC 链接器选项](linker-options.md)
