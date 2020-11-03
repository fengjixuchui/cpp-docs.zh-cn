---
title: 在 Visual Studio 中创建 C++ 生成文件项目
ms.date: 08/05/2019
f1_keywords:
- vc.appwiz.makefile.project
helpviewer_keywords:
- Makefile projects [C++]
ms.assetid: dd077af3-97a8-48fb-baaa-cf7e07ddef61
ms.openlocfilehash: 69e6a2ff0d7e82f3bc431a28f57127e8d9a3aa4d
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92921524"
---
# <a name="create-a-c-makefile-project"></a>创建 C++ 生成文件项目

生成文件  是一种说明如何编译和链接（或生成  ）一组 C++ 源代码文件的文本文件。 生成  程序会读取生成文件并调用编译器、链接器和其他程序来生成可执行文件。 Microsoft 的 *make* 程序实现称为 [NMAKE](nmake-reference.md)。

如果你有现有生成文件项目，则当你想要在 Visual Studio IDE 中编码和/或对其进行调试时，你有以下选择：

- 在 Visual Studio 中创建一个生成文件项目，该项目使用现有的生成文件来配置 Visual Studio 将用于 IntelliSense 的 .vcxproj 文件。  (你将不具有使用本机 MSBuild 项目获取的所有 IDE 功能。 ) 请参阅下面的 [创建生成文件项目](#create_a_makefile_project) 。
- 使用“从现有代码文件新建项目”  向导来从源代码创建本机 MSBuild 项目。 生成该文件之后将不再使用原始生成文件。 有关详细信息，请参阅[如何：通过现有代码创建 C++ 项目](../how-to-create-a-cpp-project-from-existing-code.md)。
- **Visual Studio 2017 及更高版本** ：使用 " **打开文件夹** " 功能可以按原样编辑和生成生成文件项目，而无需使用 MSBuild 系统。 有关详细信息，请参阅 [C++ 的“打开文件夹”项目](../open-folder-projects-cpp.md)。
- **Visual Studio 2019 及更高版本** ：创建适用于 LINUX 的 UNIX makefile 项目。

## <a name="a-namecreate_a_makefile_project-to-create-a-makefile-project-with-the-makefile-project-template"></a><a name="create_a_makefile_project"> 使用生成文件项目模板创建生成文件项目

在 Visual Studio 2017 和更高版本中，生成项目模板在安装 C++ 桌面开发工作负载后可用。

按照向导来指定用于生成文件的命令和环境。 然后可以在 Visual Studio 中使用该项目生成代码。

默认情况下，在解决方案资源管理器中，生成文件项目不显示文件。 生成文件项目指定生成设置，这些设置反映在项目的属性页中。

在项目中指定的输出文件对生成脚本生成的名称无效；它仅声明意图。 生成文件仍控制生成过程并指定生成目标。

::: moniker range="msvc-160"

### <a name="to-create-a-makefile-project-in-visual-studio-2019"></a>在 Visual Studio 2019 中创建生成文件项目

1. 在 Visual Studio 主菜单中，选择 " **文件**  >  " " **新建**  >  **项目** "，然后在 "搜索" 框中键入 "makefile"。 或者，在“新建项目”对话框中，展开“Visual C++” > “常规”(Visual Studio 2015) 或“其他”(Visual Studio 2017)，然后从两个选项中进行选择，具体取决于是面向 Windows 还是 Linux  。

1. **仅限 Windows** ：在 " **调试配置设置** " 页中，提供调试和零售版本的命令、输出、清理和重新生成信息。 如果想要为发布配置指定其他设置，请单击“下一步”  。

1. 单击“完成”关闭对话框并在“解决方案资源管理器”中打开新创建的项目  。

::: moniker-end

::: moniker range="<=msvc-150"

### <a name="to-create-a-makefile-project-in-visual-studio-2015-or-visual-studio-2017"></a>在 Visual Studio 2015 或 Visual Studio 2017 中创建生成文件项目

1. 从 Visual Studio 起始页上，在“新建项目”  搜索框中键入“生成文件”。 或者，在“新建项目”  对话框中，展开“Visual C++”  > “常规”  (Visual Studio 2015) 或“其他”  (Visual Studio 2017)，然后选择“模板”窗格中的“生成文件项目”  以打开项目向导。

1. 在 **应用程序设置** 页中，提供用于调试和传播生成的命令、输出、清除和重新生成信息。

1. 单击“完成”关闭向导并在解决方案资源管理器中打开新创建的项目  。

::: moniker-end

可以在项目的属性页中查看和编辑项目的属性。 请参阅[在 Visual Studio 中设置 C++ 编译器和生成属性](../working-with-project-properties.md)以了解关于显示属性页的信息。

## <a name="makefile-project-wizard"></a>生成文件项目向导

创建生成文件项目后，可在项目属性页的“Nmake”页中查看和编辑以下每个选项  。

- **生成命令行：** 指定用户从 "生成" 菜单中选择 "生成" 时要运行的命令行。 显示在项目属性页的“Nmake”页的“生成”命令行字段中。

- **输出：** 指定将包含命令行输出的文件的名称。 默认情况下，此选项根据项目名称而定。 显示在项目属性页的“Nmake”页的“输出”字段中。

- **清除命令：** 指定用户从 "生成" 菜单中选择 "清理" 时要运行的命令行。 显示在项目属性页的“Nmake”页的“清理”命令行字段中。

- **重新生成命令行：** 指定用户从 "生成" 菜单中选择 "重新生成" 时要运行的命令行。 显示在项目属性页的“Nmake”页的“重新生成所有命令行”字段中。

## <a name="how-to-enable-intellisense-for-makefile-projects"></a>如何：对生成文件项目启用 IntelliSense

错误设置某些项目设置或编译器操作时，IntelliSense 无法运行生成文件项目。 请按照这些步骤配置生成文件项目，以便 IntelliSense 按预期运行：

1. 打开“属性页”对话框  。 有关详细信息，请参阅[在 Visual Studio 中设置 C++ 编译器和生成属性](../working-with-project-properties.md)。

1. 展开“配置属性”  节点。

1. 选择“NMake”属性页，然后在“IntelliSense”下适当修改属性  。

   - 设置“预处理器定义”属性以定义生成文件项目中的任何预处理器符号  。 请参阅 [/D （预处理器定义）](d-preprocessor-definitions.md)，获取详细信息。

   - 设置“包含搜索路径”属性，指定编译器将搜索的目录列表，以解析传递到生成文件项目中的预处理器指令的文件引用  。 请参阅 [/I（附加包含目录）](i-additional-include-directories.md)，获取详细信息。

   - 对于从命令窗口使用 CL.EXE 生成的项目，设置“INCLUDE”环境变量，指定编译器将搜索的目录，以解析传递到生成文件项目中的预处理器指令的文件引用  。

   - 设置“强制包含”属性，指定生成生成文件时，要处理哪些头文件  。 请参阅 [/FI（命名强制包含文件）](fi-name-forced-include-file.md)，获取详细信息。

   - 设置“程序集搜索路径”属性指定编译器将搜索的目录列表，以解析对项目中 .NET 程序集的引用  。 请参阅 [/AI（指定元数据目录）](ai-specify-metadata-directories.md)，获取详细信息。

   - 设置“强制使用程序集”属性指定生成生成文件时，要处理哪些 .NET 程序集  。 请参阅 [/FU（命名强制 #using 文件）](fu-name-forced-hash-using-file.md)，获取详细信息。

   - 设置 " **附加选项** " 属性以指定在分析 c + + 文件时 IntelliSense 使用的其他编译器开关。

1. 单击“确定”关闭属性页  。

1. 使用“全部保存”命令，保存已修改的项目设置  。

下一次在 Visual Studio 开发环境中打开生成文件项目时，请在生成文件项目上依次运行“清理解决方案”命令和“生成解决方案”命令  。 IntelliSense 应该会在 IDE 中正常运行。

## <a name="see-also"></a>另请参阅

[使用 IntelliSense](/visualstudio/ide/using-intellisense)<br>
[NMAKE 参考](nmake-reference.md)<br>
[如何：从现有代码创建 C++ 项目](../how-to-create-a-cpp-project-from-existing-code.md)<br>
[生成文件中的特殊字符](special-characters-in-a-makefile.md)<br/>
[生成文件的内容](contents-of-a-makefile.md)<br/>
