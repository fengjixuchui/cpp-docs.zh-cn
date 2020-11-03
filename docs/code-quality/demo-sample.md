---
title: 用于代码分析的示例 C++ 项目
description: 如何创建一个示例解决方案，以便在 Visual Studio 中的 Microsoft c + + 代码分析演练中使用。
ms.date: 04/14/2020
ms.topic: sample
helpviewer_keywords:
- demo sample [Visual Studio ALM]
- code analysis, samples
ms.assetid: 09e1b9f7-5916-4ed6-a001-5c2d7e710682
ms.openlocfilehash: dd4fe67c05200ccc2865bc7c48b1f5047d77565e
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924629"
---
# <a name="sample-c-project-for-code-analysis"></a>用于代码分析的示例 C++ 项目

以下过程说明如何为演练创建示例 [：分析 C/c + + 代码中的缺陷](../code-quality/walkthrough-analyzing-c-cpp-code-for-defects.md)。 这些过程将创建：

- 名为 *CppDemo* 的 Visual Studio 解决方案。

- 名为 *CodeDefects* 的静态库项目。

- 名为 *批注* 的静态库项目。

这些过程还提供了标头代码以及静态库的 .cpp 文件  。

## <a name="create-the-cppdemo-solution-and-the-codedefects-project"></a>创建 CppDemo 解决方案和 CodeDefects 项目

::: moniker range=">=msvc-160"

1. 打开 Visual Studio 并选择 " **创建新项目** "

1. 在 " **创建新项目** " 对话框中，将语言筛选器更改为 **c + +** 。

1. 选择 " **Windows 桌面向导** " 并选择 " **下一步** " 按钮。

1. 在 " **配置新项目** " 页上的 " **项目名称** " 文本框中，输入 *CodeDefects* 。

1. 在 " **解决方案名称** " 文本框中，输入 *CppDemo* 。

1. 选择“创建”。

1. 在 " **Windows 桌面项目** " 对话框中，将 **应用程序类型** 更改为 **静态库 ()** 。

1. 在“附加选项”  下，选择“空项目”  。

1. 选择 **"确定"** 以创建解决方案和项目。

::: moniker-end

::: moniker range="msvc-150"

1. 打开 Visual Studio。 在菜单栏上，依次选择“文件” > “新建” > “项目”。

1. 在 " **新建项目** " 对话框中，选择 " **Visual C++** > **Windows 桌面** "。

1. 选择 " **Windows 桌面向导** "。

1. 在 " **名称** " 文本框中，输入 *CodeDefects* 。

1. 在 " **解决方案名称** " 文本框中，输入 *CppDemo* 。

1. 选择 **“确定”** 。

1. 在 " **Windows 桌面项目** " 对话框中，将 **应用程序类型** 更改为 **静态库 ()** 。

1. 在“附加选项”  下，选择“空项目”  。

1. 选择 **"确定"** 以创建解决方案和项目。

::: moniker-end

::: moniker range="msvc-140"

1. 打开 Visual Studio。 在菜单栏上，依次选择“文件” > “新建” > “项目”。

1. 在 " **新建项目** " 对话框中，选择 " **模板** > **Visual C++** > **Win32** "。

1. 选择 " **Win32 控制台应用程序** "。

1. 在 " **名称** " 文本框中，输入 *CodeDefects* 。

1. 在 " **解决方案名称** " 文本框中，输入 *CppDemo* 。

1. 选择 **“确定”** 。

1. 在 " **Win32 应用程序向导** " 对话框中，选择 " **下一步** " 按钮。

1. 将 **应用程序类型** 更改为 **静态库** 。

1. 在 " **其他选项** " 下，取消 **预编译头** 。

1. 选择 " **完成** " 以创建解决方案和项目。

::: moniker-end

## <a name="add-the-header-and-source-file-to-the-codedefects-project"></a>将标头和源文件添加到 CodeDefects 项目

1. 在解决方案资源管理器中，展开 **CodeDefects** 。

1. 右键单击以打开 **标头文件** 的上下文菜单。 选择 " **添加**  >  **新项** "。

1. 在 " **添加新项** " 对话框中，选择 " **Visual C++**  >  **代码** "，然后选择 " **标头文件 ( .h)** "。

1. 在 " **名称** " 编辑框中，输入 " *Bug .h* "，然后选择 " **添加** " 按钮。

1. 在 " *Bug .h* " 的编辑窗口中，选择并删除内容。

1. 复制以下代码并将其粘贴到编辑器中的“Bug.h”文件中  。

    ```cpp
    #pragma once

    #include <windows.h>

    // Function prototypes
    bool CheckDomain(wchar_t const *);
    HRESULT ReadUserAccount();

    // These constants define the common sizes of the
    // user account information throughout the program
    const int USER_ACCOUNT_LEN = 256;
    const int ACCOUNT_DOMAIN_LEN = 128;
    ```

1. 在解决方案资源管理器中，右键单击以打开 " **源文件** " 的上下文菜单。 选择 " **添加**  >  **新项** "。

1. 在“添加新项”  对话框中选择“C++ 文件(.cpp)”  。

1. 在 " **名称** " 编辑框中，输入 " *Bug .cpp* "，然后选择 " **添加** " 按钮。

1. 复制以下代码并将其粘贴到编辑器中的“Bug.cpp”文件中  。

    ```cpp
    #include "Bug.h"

    // the user account
    wchar_t g_userAccount[USER_ACCOUNT_LEN] = { L"domain\\user" };
    int len = 0;

    bool CheckDomain(wchar_t const* domain)
    {
        return (wcsnlen_s(domain, USER_ACCOUNT_LEN) > 0);
    }

    HRESULT ReadUserAccount()
    {
        return S_OK;
    }

    bool ProcessDomain()
    {
        wchar_t* domain = new wchar_t[ACCOUNT_DOMAIN_LEN];
        // ReadUserAccount gets a 'domain\user' input from
        //the user into the global 'g_userAccount'
        if (ReadUserAccount())
        {
            // Copies part of the string prior to the '\'
            // character onto the 'domain' buffer
            for (len = 0; (len < ACCOUNT_DOMAIN_LEN) && (g_userAccount[len] != L'\0'); len++)
            {
                if (g_userAccount[len] == L'\\')
                {
                    // Stops copying on the domain and user separator ('\')
                    break;
                }
                domain[len] = g_userAccount[len];
            }
            if ((len = ACCOUNT_DOMAIN_LEN) || (g_userAccount[len] != L'\\'))
            {
                // '\' was not found. Invalid domain\user string.
                delete[] domain;
                return false;
            }
            else
            {
                domain[len] = L'\0';
            }
            // Process domain string
            bool result = CheckDomain(domain);

            delete[] domain;
            return result;
        }
        return false;
    }

    int path_dependent(int n)
    {
        int i;
        int j;
        if (n == 0)
            i = 1;
        else
            j = 1;
        return i + j;
    }
    ```

1. 在菜单栏上，选择 " **文件** " "  >  **全部保存** "。

## <a name="add-the-annotations-project-and-configure-it-as-a-static-library"></a>添加 Annotations 项目并将其配置为静态库

::: moniker range=">=msvc-160"

1. 在解决方案资源管理器中，右键单击 **CppDemo** 以打开上下文菜单。 选择 " **添加**  >  **新项目** "。

1. 在 " **添加新项目** " 对话框中，选择 " **Windows 桌面向导** "，然后选择 " **下一步** " 按钮。

1. 在 " **配置新项目** " 页上的 " **项目名称** " 文本框中，输入 *注释* ，然后选择 " **创建** "。

1. 在 " **Windows 桌面项目** " 对话框中，将 **应用程序类型** 更改为 **静态库 ()** 。

1. 在“附加选项”  下，选择“空项目”  。

1. 选择“确定”，创建项目  。

::: moniker-end

::: moniker range="msvc-150"

1. 在解决方案资源管理器中，右键单击 **CppDemo** 以打开上下文菜单。 选择 " **添加**  >  **新项目** "。

1. 在 " **添加新项目** " 对话框中，选择 " **Visual C++** > **Windows 桌面** "。

1. 选择 " **Windows 桌面向导** "。

1. 在 " **名称** " 文本框中，输入 *注释* ，然后选择 **"确定"** 。

1. 在 " **Windows 桌面项目** " 对话框中，将 **应用程序类型** 更改为 **静态库 ()** 。

1. 在“附加选项”  下，选择“空项目”  。

1. 选择“确定”，创建项目  。

::: moniker-end

::: moniker range="msvc-140"

1. 在解决方案资源管理器中，右键单击 **CppDemo** 以打开上下文菜单。 选择 " **添加**  >  **新项目** "。

1. 在 " **添加新项目** " 对话框中，选择 " **Visual C++** > **Win32** "。

1. 选择 " **Win32 控制台应用程序** "。

1. 在 " **名称** " 文本框中，输入 *注释* 。

1. 选择 **“确定”** 。

1. 在 " **Win32 应用程序向导** " 对话框中，选择 " **下一步** " 按钮。

1. 将 **应用程序类型** 更改为 **静态库** 。

1. 在 " **其他选项** " 下，取消 **预编译头** 。

1. 选择“完成”以创建项目  。

::: moniker-end

## <a name="add-the-header-file-and-source-file-to-the-annotations-project"></a>将头文件和源文件添加到 Annotations 项目

1. 在解决方案资源管理器中，展开 " **批注** "。

1. 右键单击以打开 " **批注** " 下 **标题文件** 的上下文菜单。 选择 " **添加**  >  **新项** "。

1. 在 " **添加新项** " 对话框中，选择 " **Visual C++**  >  **代码** "，然后选择 " **标头文件 ( .h)** "。

1. 在 " **名称** " 编辑框中，输入 " *注释 .h* "，然后选择 " **添加** " 按钮。

1. 在 " *批注* " 的 "编辑" 窗口中，选择并删除内容。

1. 复制以下代码并将其粘贴到编辑器中的“annotations.h”文件中  。

    ```cpp
    #pragma once
    #include <sal.h>

    struct LinkedList
    {
        struct LinkedList* next;
        int data;
    };

    typedef struct LinkedList LinkedList;

    _Ret_maybenull_ LinkedList* AllocateNode();
    ```

1. 在解决方案资源管理器中，右键单击以打开 " **批注** " 下的 **源文件** 的上下文菜单。 选择 " **添加**  >  **新项** "。

1. 在“添加新项”  对话框中选择“C++ 文件(.cpp)”  。

1. 在 " **名称** " 编辑框中，输入 " *注释 .cpp* "，然后选择 " **添加** " 按钮。

1. 复制以下代码并将其粘贴到编辑器中的“annotations.cpp”文件中  。

    ```cpp
    #include "annotations.h"
    #include <malloc.h>

    _Ret_maybenull_ LinkedList* AllocateNode()
    {
        LinkedList* result = static_cast<LinkedList*>(malloc(sizeof(LinkedList)));
        return result;
    }

    LinkedList* AddTail(LinkedList* node, int value)
    {
        // finds the last node
        while (node->next != nullptr)
        {
            node = node->next;
        }

        // appends the new node
        LinkedList* newNode = AllocateNode();
        newNode->data = value;
        newNode->next = 0;
        node->next = newNode;

        return newNode;
    }
    ```

1. 在菜单栏上，选择 " **文件** " "  >  **全部保存** "。

解决方案现已完成，应顺利生成，没有错误。

::: moniker range="msvc-150"

> [!NOTE]
> 在 Visual Studio 2017 中，IntelliSense 引擎可能会出现虚假警告 `E1097 unknown attribute "no_init_all"` 。 可以放心地忽略此警告。

::: moniker-end
