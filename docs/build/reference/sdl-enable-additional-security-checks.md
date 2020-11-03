---
title: /sdl（启用附加安全检查）
description: Microsoft C/c + + 编译器/sdl 选项启用 (SDL) 检查和警告的推荐安全开发生命周期。
ms.date: 10/02/2020
f1_keywords:
- VC.Project.VCCLCompilerTool.SDLCheck
ms.assetid: 3dcf86a0-3169-4240-9f29-e04a9f535826
ms.openlocfilehash: 8d679bb3fc48e52bcc42a85d507618c38fd3dcdc
ms.sourcegitcommit: 30792632548d1c71894f9fecbe2f554294b86020
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91765191"
---
# <a name="sdl-enable-additional-security-checks"></a>/sdl（启用附加安全检查）

启用 (SDL) 检查的推荐安全开发生命周期。 这些检查将与安全相关的警告更改为错误，并设置其他安全代码生成功能。

## <a name="syntax"></a>语法

> **`/sdl`**[**`-`**]

## <a name="remarks"></a>注解

**/sdl** 启用由提供的基线安全检查的超集 [`/GS`](gs-buffer-security-check.md) ，并替代 **`/GS-`** 。 默认情况下， **`/sdl`** 处于关闭状态。 **`/sdl-`** 禁用其他安全检查。

### <a name="compile-time-checks"></a>编译时检查

**`/sdl`** 启用这些警告作为错误：

| /sdl 启用的警告 | 等效的命令行开关 | 说明 |
|--|--|--|
| [C4146](../../error-messages/compiler-warnings/compiler-warning-level-2-c4146.md) | /we4146 | 将一元减号运算符应用于无符号类型，并生成了无符号的结果。 |
| [C4308](../../error-messages/compiler-warnings/compiler-warning-level-2-c4308.md) | /we4308 | 将负整型常数转换为无符号类型，并生成了可能无意义的结果。 |
| [C4532](../../error-messages/compiler-warnings/compiler-warning-level-1-c4532.md) | /we4532 | `continue` `break` `goto` 在块中使用、或关键字在 `__finally` / `finally` 异常终止期间具有未定义的行为。 |
| [C4533](../../error-messages/compiler-warnings/compiler-warning-level-1-c4533.md) | /we4533 | 初始化变量的代码不会执行。 |
| [C4700](../../error-messages/compiler-warnings/compiler-warning-level-1-and-level-4-c4700.md) | /we4700 | 使用未初始化的局部变量。 |
| [C4703](../../error-messages/compiler-warnings/compiler-warning-level-4-c4703.md) | /we4703 | 使用可能未初始化的局部指针变量。 |
| [C4789](../../error-messages/compiler-warnings/compiler-warning-level-1-c4789.md) | /we4789 | 使用特定 C 运行时 (CRT) 函数时缓冲区溢出。 |
| [C4995](../../error-messages/compiler-warnings/compiler-warning-level-3-c4995.md) | /we4995 | 使用标记有杂注的函数 [`deprecated`](../../preprocessor/deprecated-c-cpp.md) 。 |
| [C4996](../../error-messages/compiler-warnings/compiler-warning-level-3-c4996.md) | /we4996 | 使用标记为的函数 [`deprecated`](../../cpp/deprecated-cpp.md) 。 |

### <a name="runtime-checks"></a>运行时检查

**`/sdl`** 启用后，编译器将生成在运行时执行这些检查的代码：

- 启用 **`/GS`** 运行时缓冲区溢出检测的严格模式，等效于与的编译 `#pragma strict_gs_check(push, on)` 。

- 限制指针清理。 在不涉及取消引用的表达式和没有用户定义的析构函数的类型中，在调用后，指针引用将设置为无效的地址 **`delete`** 。 此净化有助于防止重复使用过时的指针引用。

- 初始化类成员指针。 在构造函数运行) 之前，自动将指针类型的类成员初始化为 **`nullptr`** (对象实例化。 它有助于防止使用未初始化的指针，构造函数不会显式初始化。 将调用编译器生成的成员指针初始化，条件是：

  - 不使用自定义 (用户定义的对象来分配对象) `operator new`

  - 对象不作为数组的一部分分配 (例如 `new A[x]`) 

  - 不管理或导入类

  - 类具有用户定义的默认构造函数。

  若要由编译器生成的类初始化函数进行初始化，成员必须是指针，而不是属性或常量。

有关详细信息，请参阅 [警告、/sdl 和改进未初始化的变量检测](https://www.microsoft.com/security/blog/2012/06/06/warnings-sdl-and-improving-uninitialized-variable-detection/)。

### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项

1. 打开项目的“属性页”  对话框。 有关详细信息，请参阅[在 Visual Studio 中设置 C++ 编译器和生成属性](../working-with-project-properties.md)。

1. 选择“配置属性” > “C/C++” > “常规”属性页    。

1. 通过使用 "属性" 下拉控件，设置 **SDL 检查** 属性。 选择 **"确定" 或 "** **应用** " 保存更改。

## <a name="see-also"></a>请参阅

[MSVC 编译器选项](compiler-options.md)<br/>
[MSVC 编译器命令行语法](compiler-command-line-syntax.md)
