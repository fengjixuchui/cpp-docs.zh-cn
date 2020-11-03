---
title: 演练：分析 C/c + + 代码的缺陷
description: 演示如何在 Visual Studio 中将代码分析与 Microsoft c + + 一起使用。
ms.date: 04/14/2020
ms.topic: conceptual
helpviewer_keywords:
- C/C++, code analysis
- code analysis, walkthroughs
- code, analyzing C/C++
- code analysis tool, walkthroughs
ms.openlocfilehash: bb81ca376651c17c760ee776510303efaa13fd9a
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924778"
---
# <a name="walkthrough-analyzing-cc-code-for-defects"></a>演练：分析 C/c + + 代码的缺陷

本演练演示如何分析 C/c + + 代码是否存在潜在的代码缺陷。 它使用适用于 C/c + + 代码的代码分析工具。

在本演练中，你将：

- 在本机代码上运行代码分析。
- 分析代码缺陷警告。
- 将警告视为错误。
- 批注源代码以改进代码缺陷分析。

## <a name="prerequisites"></a>先决条件

- [CppDemo 示例](../code-quality/demo-sample.md)的副本。
- 基本了解 C/c + +。

## <a name="run-code-analysis-on-native-code"></a>在本机代码上运行代码分析

### <a name="to-run-code-defect-analysis-on-native-code"></a>在本机代码上运行代码缺陷分析

::: moniker range=">=msvc-160"

1. 在 Visual Studio 中打开 CppDemo 解决方案。

     CppDemo 解决方案现在 **解决方案资源管理器** 中填充。

1. 在 " **生成** " 菜单上，选择 " **重新生成解决方案** "。

     解决方案生成时不会出现任何错误或警告。

1. 在 **解决方案资源管理器** 中，选择 CodeDefects 项目。

1. 在 **“项目”** 菜单上，选择 **“属性”** 。

     随即显示 " **CodeDefects 属性页** " 对话框。

1. 选择 " **代码分析** " 属性页。

1. 将 " **对生成启用代码分析** " 属性更改为 **"是"** 。 选择“确定”以保存更改  。

1. 重新生成 CodeDefects 项目。

     " **错误列表** " 窗口中将显示代码分析警告。

::: moniker-end

::: moniker range="<=msvc-150"

1. 在 Visual Studio 中打开 CppDemo 解决方案。

     CppDemo 解决方案现在 **解决方案资源管理器** 中填充。

1. 在 " **生成** " 菜单上，选择 " **重新生成解决方案** "。

     解决方案生成时不会出现任何错误或警告。

     > [!NOTE]
     > 在 Visual Studio 2017 中，IntelliSense 引擎可能会出现虚假警告 `E1097 unknown attribute "no_init_all"` 。 可以放心地忽略此警告。

1. 在 **解决方案资源管理器** 中，选择 CodeDefects 项目。

1. 在 **“项目”** 菜单上，选择 **“属性”** 。

     随即显示 " **CodeDefects 属性页** " 对话框。

1. 选择 " **代码分析** " 属性页。

1. 选中 " **生成时启用代码分析"** 复选框。 选择“确定”以保存更改  。

1. 重新生成 CodeDefects 项目。

     " **错误列表** " 窗口中将显示代码分析警告。

::: moniker-end

### <a name="to-analyze-code-defect-warnings"></a>分析代码缺陷警告

1. 在 " **视图** " 菜单上，选择 " **错误列表** "。

     此菜单项可能不可见。 这取决于你在 Visual Studio 中选择的开发人员配置文件。 您可能必须指向 " **视图** " 菜单上的 " **其他窗口** "，然后选择 " **错误列表** "。

1. 在 " **错误列表** " 窗口中，双击以下警告：

     C6230：语义不同类型之间的隐式强制转换：在 Boolean 上下文中使用 HRESULT。

     "代码编辑器" 显示导致函数内出现警告的行 `bool ProcessDomain()` 。 此警告意味着 `HRESULT` 在需要布尔值结果的 "if" 语句中使用。 这通常是一个错误，因为 `S_OK` 从函数返回 HRESULT 时，它表示成功，但在转换为其计算结果为的布尔值时 **`false`** 。

1. 使用宏更正此警告 `SUCCEEDED` ，此宏在 **`true`** `HRESULT` 返回值指示成功时转换为。 你的代码应与以下代码类似：

   ```cpp
   if (SUCCEEDED(ReadUserAccount()))
   ```

1. 在 **错误列表** 中，双击以下警告：

     C6282：运算符不正确：在 Boolean 上下文中分配常量。 请考虑改用 "= ="。

1. 通过测试相等性来更正此警告。 你的代码应类似于以下代码：

   ```cpp
   if ((len == ACCOUNT_DOMAIN_LEN) || (g_userAccount[len] != L'\\'))
   ```

1. 在 **错误列表** 中，通过 `i` 将和初始化为0来更正剩余的 C6001 警告 `j` 。

1. 重新生成 CodeDefects 项目。

     项目生成时不会出现任何警告或错误。

## <a name="correct-source-code-annotation-warnings"></a>更正源代码批注警告

### <a name="to-enable-the-source-code-annotation-warnings-in-annotationc"></a>在 annotation 中启用源代码批注警告

::: moniker range=">=msvc-160"

1. 在解决方案资源管理器中，选择 "批注" 项目。

1. 在 **“项目”** 菜单上，选择 **“属性”** 。

     将显示 " **批注属性页** " 对话框。

1. 选择 " **代码分析** " 属性页。

1. 将 " **对生成启用代码分析** " 属性更改为 **"是"** 。 选择“确定”以保存更改  。

::: moniker-end

::: moniker range="<=msvc-150"

1. 在解决方案资源管理器中，选择 "批注" 项目。

1. 在 **“项目”** 菜单上，选择 **“属性”** 。

     将显示 " **批注属性页** " 对话框。

1. 选择 " **代码分析** " 属性页。

1. 选中 " **生成时启用代码分析"** 复选框。 选择“确定”以保存更改  。

::: moniker-end

### <a name="to-correct-the-source-code-annotation-warnings-in-annotationc"></a>更正 annotation 中的源代码批注警告

1. 重新生成批注项目。

1. 在 " **生成** " 菜单上，选择 **"对批注运行代码分析** "。

1. 在 **错误列表** 中，双击以下警告：

     C6011：取消对 NULL 指针 "newNode" 的引用。

     此警告指示调用方检查返回值失败。 在这种情况下，对的调用 `AllocateNode` 可能会返回 NULL 值。 请参阅的函数声明的批注 .h 头文件 `AllocateNode` 。

1. 光标位于出现警告的注释 .cpp 文件中的位置。

1. 若要更正此警告，请使用 "if" 语句测试返回值。 你的代码应与以下代码类似：

   ```cpp
   LinkedList* newNode = AllocateNode();
   if (nullptr != newNode)
   {
       newNode->data = value;
       newNode->next = 0;
       node->next = newNode;
   }
   ```

1. 重新生成批注项目。

     项目生成时不会出现任何警告或错误。

## <a name="use-source-code-annotation-to-discover-more-issues"></a>使用源代码批注发现更多问题

### <a name="to-use-source-code-annotation"></a>使用源代码批注

1. 注释函数的形参和返回值 `AddTail` ，以指示指针值可以为 null：

   ```cpp
   _Ret_maybenull_ LinkedList* AddTail(_Maybenull_ LinkedList* node, int value)
   ```

1. 在“生成”菜单上，选择“对解决方案运行代码分析”。 

1. 在 **错误列表** 中，双击以下警告：

     C6011：取消引用 NULL 指针 "node"。

     此警告意味着传递到函数的节点可能为 null。

1. 若要更正此警告，请在函数开头使用 "if" 语句来测试传入的值。 你的代码应与以下代码类似：

   ```cpp
   if (nullptr == node)
   {
        return nullptr;
   }
   ```

1. 在“生成”菜单上，选择“对解决方案运行代码分析”。 

     项目现在生成时没有任何警告或错误。

## <a name="see-also"></a>请参阅

[演练：对托管代码进行代码缺陷分析](/visualstudio/code-quality/walkthrough-analyzing-managed-code-for-code-defects)\
[C/C++ 代码分析](../code-quality/code-analysis-for-c-cpp-overview.md)
