---
title: 静态库 (C++/CX)
ms.date: 02/03/2017
ms.assetid: 7faf53c8-fa21-42cc-8246-d32533ef9dfa
ms.openlocfilehash: 756f8d2c1af2c6be414ad39b4a96fa6cc7ccfb02
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924719"
---
# <a name="static-libraries-ccx"></a>静态库 (C++/CX)

通用 Windows 平台中使用的静态库 (UWP) 应用可以包含 ISO 标准 c + + 代码（包括 STL 类型），还可以调用不从 Windows 运行时应用平台中排除的 Win32 Api。 静态库使用 Windows 运行时组件，可能会创建具有特定限制的 Windows 运行时组件。

## <a name="creating-static-libraries"></a>创建静态库

根据你安装的 Visual Studio 版本，创建新项目的说明有所不同。 若要查看 Visual Studio 首选项的文档，请使用“版本”选择器控件。 它位于此页面上目录表的顶部。

::: moniker range="msvc-160"

### <a name="to-create-a-uwp-static-library-in-visual-studio-2019"></a>在 Visual Studio 2019 中创建 UWP 静态库

1. 在菜单栏上，选择“文件”>“新建”>“项目”，打开“创建新项目”对话框     。

1. 在对话框顶部，将 "  **语言** " 设置为 " **c + +** "，将 " **平台** " 设置为 " **Windows** "，将 " **项目类型** " 设置为 **UWP** 。

1. 从筛选后的项目类型列表中，选择 " **静态库 (通用 Windows-c + +/cx)** ，然后选择" **下一步** "。 在下一页中，为项目指定一个名称，并指定项目位置（如果需要）。

1. 选择“创建”  按钮创建项目。

::: moniker-end

::: moniker range="<=msvc-150"

### <a name="to-create-a-uwp-static-library-in-visual-studio-2017-or-visual-studio-2015"></a>在 Visual Studio 2017 或 Visual Studio 2015 中创建 UWP 静态库

1. 在菜单栏上，依次选择“文件” > “新建” > “项目”。 在 " **Visual C++**  >  **Windows 通用** " 下，选择 " **静态库" (通用 Windows)** "。

1. 在“解决方案资源管理器” 中，打开项目的快捷菜单，然后选择“属性” 。 在 " **属性** " 对话框的 " **配置属性** " "  >  **c/c + +** " 页上，将 " **使用 Windows 运行时扩展** " 设置为 **"是" (/ZW)** 。

::: moniker-end

编译新的静态库时，如果调用了对 UWP 应用排除的 Win32 API，编译器将引发错误 C3861 "找不到标识符"。 若要查找 Windows 运行时支持的替代方法，请参阅 [UWP 应用中的 Windows api 的替代](/uwp/win32-and-com/alternatives-to-windows-apis-uwp)方法。

如果向 UWP 应用解决方案添加 c + + 静态库项目，则可能需要更新库项目的属性设置，以便 "UWP 支持" 属性设置为 **"是"** 。 如果没有此设置，代码将生成和链接，但当你尝试验证 Microsoft Store 的应用时，会发生错误。 编译静态库时的编译器设置应与使用该库的项目的编译器设置相同。

如果使用创建公共 `ref` 类、公共接口类或公共值类的静态库，则链接器会引发此警告：

> **警告 LNK4264：** 将使用/ZW 编译的对象文件归档到静态库中;请注意，在创作 Windows 运行时类型时，建议不要与包含 Windows 运行时元数据的静态库链接。

仅当静态库不生成在库自身之外使用的 Windows 运行时组件时，你才可以放心地忽略此警告。 如果该库不使用其定义的组件，则链接器可通过优化去除实现，即使公共元数据包含类型信息，情况也不例外。 这意味着，静态库中的公共组件将进行编译，但不会在运行时激活。 出于此原因，必须在动态链接 (库中实现任何旨在供其他组件或应用使用的 Windows 运行时组件) 。

## <a name="see-also"></a>请参阅

[线程处理和封送处理](../cppcx/threading-and-marshaling-c-cx.md)
