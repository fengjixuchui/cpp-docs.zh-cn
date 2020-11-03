---
title: 编译定目标到 CLR 的 C++/CLI 程序
description: 使用 Microsoft c + + 创建可连接本机 c + + 代码和 .NET 程序的程序和库。
ms.date: 04/23/2019
helpviewer_keywords:
- command-line applications [C++], managed code
- compiling programs [C++]
- Visual C++, managed code
- managed code [C++]
ms.assetid: 339f89df-a5d2-4040-831a-ddbe25b5dce4
ms.openlocfilehash: d6d06104cc007ae5ff20f579225c885690a704e4
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924224"
---
# <a name="walkthrough-compile-a-ccli-program-that-targets-the-clr-in-visual-studio"></a>演练：在 Visual Studio 中编译面向 CLR 的 c + +/CLI 程序

通过使用 c + +/CLI，你可以创建使用 .NET 类和本机 c + + 类型的 c + + 程序。 C + +/CLI 专用于控制台应用程序和包装本机 c + + 代码的 Dll 中，并使其可从 .NET 程序进行访问。 若要基于 .NET 创建 Windows 用户界面，请使用 c # 或 Visual Basic。

对于此过程，你可以键入你自己的 c + + 程序或使用其中一个示例程序。 我们在此过程中使用的示例程序创建了名为“textfile.txt”的文本文件，并将其保存到项目目录。

## <a name="prerequisites"></a>先决条件

- 你需要了解 C++ 语言的基础知识。
- 在 Visual Studio 2017 和更高版本中，c + +/CLI 支持是一个可选组件。 若要安装它，请从 Windows "开始" 菜单打开 **Visual Studio 安装程序** 。 确保选中 " **带 c + + 的桌面开发** " 磁贴，并在 **可选** 组件部分中检查 **c + +/cli 支持** 。

## <a name="create-a-new-project"></a>创建新项目

根据使用的 Visual Studio 版本，以下步骤会有所不同。 若要查看 Visual Studio 首选项的文档，请使用“版本”选择器控件。 它位于此页面上目录表的顶部。

::: moniker range="msvc-160"

### <a name="to-create-a-ccli-project-in-visual-studio-2019"></a>在 Visual Studio 2019 中创建 c + +/CLI 项目

1. 在 **解决方案资源管理器** 中，右键单击顶部以打开 "新建 **项目** " 对话框。

1. 在对话框的顶部，在 "搜索" 框中键入 " **clr** "，然后从 "结果" 列表中选择 " **clr 空项目** "。

1. 选择“创建”  按钮创建项目。

::: moniker-end

::: moniker range="msvc-150"

### <a name="to-create-a-ccli-project-in-visual-studio-2017"></a>在 Visual Studio 2017 中创建 c + +/CLI 项目

1. 创建新项目。 在 **“文件”** 菜单上，指向 **“新建”** ，再单击 **“项目”** 。

1. 在 Visual C++ 项目类型中，依次单击“CLR”、“CLR 空项目”  。

1. 键入项目名称。 默认情况下，包含该项目的解决方案具备和新项目一样的名称，不过可以输入一个不同的名称。 可以按需要为项目输入不同的位置。

1. 单击“确定”以创建新项目  。

::: moniker-end

::: moniker range="msvc-140"

### <a name="to-create-a-ccli-project-in-visual-studio-2015"></a>在 Visual Studio 2015 中创建 c + +/CLI 项目

1. 创建新项目。 在 **“文件”** 菜单上，指向 **“新建”** ，再单击 **“项目”** 。

1. 在 Visual C++ 项目类型中，依次单击“CLR”、“CLR 空项目”  。

1. 键入项目名称。 默认情况下，包含该项目的解决方案具备和新项目一样的名称，不过可以输入一个不同的名称。 可以按需要为项目输入不同的位置。

1. 单击“确定”以创建新项目  。

::: moniker-end

## <a name="add-a-source-file"></a>添加源文件

1. 如果“解决方案资源管理器”  不可见，请单击“视图”  菜单上的“解决方案资源管理器”  。

1. 向项目添加新的源文件：

   - 在“解决方案资源管理器”  中右键单击“源文件”  文件夹，指向“添加”  并单击“新建项”  。

   - 单击“C++ 文件 (.cpp)”并键入文件名，然后单击“添加”  。

   .cpp  文件会出现在“解决方案资源管理器”  中的“源文件”  文件夹中，并且当你按需要在该文件中键入代码时，会出现一个选项卡式窗口。

1. 单击 Visual Studio 中新创建的选项卡并键入有效的 Visual C++ 程序或从示例程序中复制粘贴一个。

   例如，可以使用  。

   如果使用示例程序，请注意，在 **`gcnew`** **`new`** 创建 .net 对象时使用关键字，而不是 **`gcnew`** 返回 () 的句柄， `^` 而不是)  (的指针 `*` ：

   `StreamWriter^ sw = gcnew StreamWriter(fileName);`

   有关 c + +/CLI 语法的详细信息，请参阅 [运行时平台的组件扩展](../extensions/component-extensions-for-runtime-platforms.md)。

1. 在 **“生成”** 菜单上，单击 **“生成解决方案”** 。

   “输出”窗口显示编译进度的相关信息，例如生成日志的位置以及指示生成状态的消息  。

   如果在未进行生成操作的情况下做出更改且运行程序，则可能出现一个指示该程序已过期的对话框。 如果希望 Visual Studio 始终使用当前版本的文件而不是在每次生成应用程序时都出现提示，请选择该对话框中的复选框后再单击“确定”  。

1. 在 " **调试** " 菜单上，单击 " **启动（不调试** ）"。

1. 如果使用了示例程序，在运行程序时会显示一个命令窗口，该窗口指示已创建文本文件。

   textfile.txt 文本文件现在位于你的项目目录中  。 可以使用记事本打开此文件。

   > [!NOTE]
   > 选择自动设置 `/clr` 编译器选项的空 CLR 项目模板。 为了验证这一点，请在“解决方案资源管理器”中右键单击该项目并单击“属性”，然后选中“配置属性”的“常规”节点中的“公共语言运行时支持”选项  。

## <a name="see-also"></a>请参阅

[C++ 语言参考](../cpp/cpp-language-reference.md)<br/>
[项目和生成系统](../build/projects-and-build-systems-cpp.md)<br/>
