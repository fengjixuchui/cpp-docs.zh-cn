---
title: /std（指定语言标准版本）
description: MSVC 编译器选项/std 指定编译器支持的 C 或 c + + 语言标准。
ms.date: 10/29/2020
f1_keywords:
- /std
- -std
- /std:c++14
- /std:c++17
- /std:c11
- /std:c17
- VC.Project.VCCLCompilerTool.CppLanguageStandard
ms.assetid: 0acb74ba-1aa8-4c05-b96c-682988dc19bd
ms.openlocfilehash: 208789071ff028107d3c7311c3b5c6cf3eea7c1d
ms.sourcegitcommit: 4abc6c4c9694f91685cfd77940987e29a51e3143
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93238468"
---
# <a name="std-specify-language-standard-version"></a>`/std` (指定语言标准版本) 

启用 C 或 c + + 语言标准的指定版本中支持的 C 和 c + + 语言功能。

## <a name="syntax"></a>语法

> **`/std:c++14`**\
> **`/std:c++17`**\
> **`/std:c++latest`**\
> **`/std:c11`**\
> **`/std:c17`**

## <a name="remarks"></a>备注

此 **`/std`** 选项在 Visual Studio 2017 和更高版本中可用。 它用于控制在代码编译期间启用的特定于版本的 ISO C 或 c + + 编程语言标准功能。 此选项允许你禁用对某些新语言和库功能的支持：可能会破坏符合特定语言标准版本的现有代码的支持。

### <a name="c-standards-support"></a>C + + 标准支持

默认情况下， **`/std:c++14`** 指定，这将禁用在更高版本的 c + + 语言标准中找到的语言和标准库功能。 使用  **`/std:c++17`** 启用 c + + 17 标准特定功能和行为。 若要显式启用当前实现的编译器和标准库功能建议用于下一草案标准，请使用 **`/std:c++latest`** 。 所有 c + + 20 功能都需要 **`/std:c++latest`** ; 实现完成后， **`/std:c++20`** 将启用一个新选项。

默认 **`/std:c++14`** 选项启用由 MSVC 编译器实现的 c + + 14 功能集。 此选项将禁用编译器和标准库支持，以对在较新版本的语言标准中更改或新增的功能禁用该功能。 它不会禁用先前版本的 MSVC 编译器已经实现的 c + + 17 功能。 若要避免对已对 Visual Studio 2015 Update 2 之前或之前的功能具有依赖关系的用户进行重大更改，则在指定选项时，这些功能仍保持启用状态 **`/std:c++14`** ：

- [`auto`With 大括号内的规则](https://wg21.link/n3922)

- [`typename` 模板模板中的参数](https://wg21.link/n4051)

- [删除三字符组](https://wg21.link/n4086)

- [命名空间和枚举器的属性](https://wg21.link/n4266)

- [u8 字符文本](https://wg21.link/n4267)

当你指定可用时，将启用哪一列表 c + + 14 和 c + + 17 功能 **`/std:c++14`** 。 有关详细信息，请参阅 [Microsoft c + + 语言一致性表](../../overview/visual-cpp-language-conformance.md)中的说明。

**`/std:c++17`** 选项启用 MSVC 编译器实现的一组完整的 c + + 17 功能。 此选项对 c + + 17 之后新增或更改的功能禁用编译器和标准库支持。 这包括 c + + 标准的工作草稿和缺陷更新版本中的 C + + 17 更改。

**`/std:c++latest`** 选项可启用当前在编译器和库中实现的 C + + 17 语言和库功能。 这些功能可能包括 c + + 20 个工作草案的更改、c + + 17 中未包含的缺陷更新以及草案标准的实验性提议。 有关支持的语言和库功能列表，请参阅 [Visual C++ 的新增功能](../../overview/what-s-new-for-visual-cpp-in-visual-studio.md)。 **`/std:c++latest`** 选项不会启用交换机保护的功能 **`/experimental`** ，但可能需要启用它们。

> [!IMPORTANT]
> 启用的编译器和库功能 **`/std:c++latest`** 表示可能出现在将来的 c + + 标准中的功能，以及已批准的 c + + 20 功能。 未经批准的功能可能会在不经通知的情况下进行中断性变更或删除，并按原样提供。

**`/std`** 可以通过使用 [ \_ MSVC \_ LANG](../../preprocessor/predefined-macros.md)预处理器宏来检测 c + + 编译过程中有效的选项。 有关详细信息，请参阅 [预处理器宏](../../preprocessor/predefined-macros.md)。

**`/std:c++14`** **`/std:c++latest`** 从 Visual Studio 2015 Update 3 开始提供和选项。 **`/std:c++17`** 从 Visual Studio 2017 版本15.3 开始，可以使用选项。 如上所述，某些 c + + 17 标准行为是通过选项启用的 **`/std:c++14`** ，但所有其他 c + + 17 功能都是通过启用的 **`/std:c++17`** 。 在实现完成之前，c + + 20 功能处于启用状态 **`/std:c++latest`** 。

> [!NOTE]
> 根据 MSVC 编译器版本或更新级别，在指定选项时，c + + 17 功能可能未完全实现或完全一致 **`/std:c++17`** 。 有关 Visual C++ 中的 c + + 语言一致性概述，请参阅 [Microsoft c + + 语言一致性表](../../overview/visual-cpp-language-conformance.md)。

### <a name="c-standards-support"></a>C 标准支持

默认情况下，当代码编译为 C 时，MSVC 编译器不符合特定 C 标准。 它实现了具有多个 Microsoft 扩展的 ANSI C89，其中一些扩展是 ISO C99 的一部分。 某些 Microsoft 扩展可以通过使用 [`/Za`](za-ze-disable-language-extensions.md) 编译器选项来禁用，但其他扩展仍有效。 无法指定严格的 C89 一致性。

从 Visual Studio 2019 版本16.8 开始，你可以 **`/std:c11`** **`/std:c17`** 为编译为 C 的代码指定或。这些选项指定与 ISO C11 和 ISO C17 相对应的一致性模式。 由于需要新的预处理器来支持这些标准，因此 **`/std:c11`** 和 **`/std:c17`** 编译器选项 [`/Zc:preprocessor`](zc-preprocessor.md) 会自动设置选项。 如果要使用传统 (旧) 预处理器用于 C11 或 C17，则必须 **`/Zc:preprocessor-`** 显式设置编译器选项。 设置 **`/Zc:preprocessor-`** 选项可能会导致意外的行为，因此不建议使用此选项。

> [!NOTE]
> 发布时，最新的 Windows SDK 和 UCRT 库尚不支持 C11 和 C17 代码。 Windows SDK 和 UCRT 的预发行版本是必需的。 有关详细信息和安装说明，请参阅 [在 Visual Studio 中安装 C11 和 C17 支持](../../overview/install-c17-support.md)。

指定或时 **`/std:c11`** **`/std:c17`** ，MSVC 支持 C11 和 C17 的所有必需功能。 编译器选项支持以下功能：

- [`_Pragma`](../../preprocessor/pragma-directives-and-the-pragma-keyword.md#the-_pragma-preprocessing-operator-c99-c11)

- **`restrict`**

- **`_Noreturn`** 与 \<stdnoreturn.h>

- **`_Alignas`****`_Alignof`** 和\<stdalign.h>

- **`_Generic`** 与 \<tgmath.h>

- **`_Static_assert`**

当源文件具有 *`.c`* 文件扩展名时，或指定编译器选项时，IDE 使用 IntelliSense 和代码突出显示的 C 设置 [`/TC`](tc-tp-tc-tp-specify-source-file-type.md) 。 目前，IntelliSense 突出显示仅适用于关键字，而不能用于标准标头引入的宏。

由于 C17 在很大程度上是 ISO C11 的 bug 修复版本，因此 MSVC 对 C11 的支持已包括所有相关缺陷报告。 目前，除宏外，C11 和 C17 版本之间没有差异 `__STDC_VERSION__` 。 对于 `201112L` C11 和 C17，它将扩展为 `201710L` 。

编译器不支持 ISO C11 的任何可选功能。 C11 的这些可选功能中有几个是 C99 的必需功能，MSVC 尚未实现此功能是出于结构方面的原因。 可以使用功能测试宏（如） `__STDC_NO_VLA__` 来检测各个功能的编译器支持级别。 有关 C 特定的预定义宏的详细信息，请参阅 [预定义的宏](../../preprocessor/predefined-macros.md)。

- Visual Studio 2019 版本16.8 版本中没有一致的多线程、原子或复数支持。

- `aligned_alloc` 由于 Windows 堆实现，缺少支持。 替代方法是使用 [`_aligned_malloc`](../../c-runtime-library/reference/aligned-malloc.md) 。

- 当前未实现 DR 400 支持 `realloc` ，因为此更改将破坏 ABI。

- 不计划 (VLA) 支持的可变长度数组。 可变长度数组的效率通常低于可比较的固定大小的数组。 在安全安全地实现时，与等效的堆内存分配相比，它们也是低效的。 VLAs 提供了可与 `gets()` （不推荐使用）和计划删除的受攻击媒介。

### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项

1. 打开项目的“属性页”  对话框。 有关详细信息，请参阅[在 Visual Studio 中设置 C++ 编译器和生成属性](../working-with-project-properties.md)。

1. 依次选择“配置属性”、“C/C++”和“语言”  。

1. 在 c **+ + 语言标准** (或 c， **c 语言标准** ) 中，从下拉控件中选择要支持的语言标准，然后选择 **"确定" 或 "** **应用** " 保存更改。

## <a name="see-also"></a>另请参阅

[MSVC 编译器选项](compiler-options.md)<br/>
[MSVC 编译器 Command-Line 语法](compiler-command-line-syntax.md)
