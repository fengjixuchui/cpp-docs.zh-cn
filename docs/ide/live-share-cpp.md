---
title: 在 Visual Studio 中使用适用于 C++ 的 Live Share 进行协作
description: 在 Visual Studio 中使用适用于 C++ 的 Live Share 进行实时协作和共享代码。
ms.date: 05/24/2019
ms.openlocfilehash: 60830ad6c6b98f644e1c3ddb2e78fbf7397ae919
ms.sourcegitcommit: c1fd917a8c06c6504f66f66315ff352d0c046700
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2020
ms.locfileid: "90684516"
---
# <a name="collaborate-using-live-share-for-c"></a>使用用于 C++ 的 Live Share 协作

在 Visual Studio 2019 和 Visual Studio Code 中，可以使用 Live Share 实时协作处理 C++ 项目  。 借助 Live Share，无需安装你的项目或其任何依赖项，其他人即可编辑和调试你的代码  。 你可以在编辑时看到彼此的编辑内容，并且每项编辑内容都标记有编辑人员的姓名。

![C&#43;&#43; Live Share 编辑](../ide/media/live-share-edit-cpp.png "C++ 中的 Live Share 编辑")

## <a name="live-share-host-and-guests"></a>Live Share 主机和来宾

Live Share 会话中具有一个主机以及一个或多个来宾。 主机和来宾都可以使用 Visual Studio 或 Visual Studio Code。 Windows 上的 Visual Studio 2019 主机可与 Linux 上的 Visual Studio Code 来宾共享。

主机为来宾提供了提高工作效率所需的一切内容。 来宾无需拥有源代码、编译器、外部依赖项，甚至是相同的安装组件。

主机和来宾可以使用以下 IntelliSense 功能：

- 成员列表
- 参数帮助
- 快速信息
- 调试/断点
- 查找所有引用
- 转到定义
- 符号搜索 (Ctrl+T)
- 引用突出显示
- 诊断/错误/波浪线

![C&#43;&#43; Live Share 调试](../ide/media/live-share-debug-cpp.png "C++ 中的 Live Share 调试")

## <a name="start-and-end-a-live-share-session"></a>开始和结束 Live Share 会话

要在 Visual Studio 中启动 Live Share 会话，请单击右上角的“共享”按钮，或转到“文件” > “启动协作会话” 。 此操作将生成可与协作者共享的链接。

![Live Share 按钮的小屏幕截图。](../ide/media/live-share-button-cpp.png "Live Share 按钮")

要结束会话，请从“共享”下拉列表中选择“结束协作会话”  。

![已突出显示“结束协作会话”选项的“共享”下拉列表的屏幕截图。](../ide/media/live-share-end-session-cpp.png "Live Share 按钮")

## <a name="for-more-information"></a>更多信息

有关 Visual Studio 中 Live Share 的详细信息，请参阅  。 有关 Visual Studio Code 中 Live Share 的详细信息，请参阅 [Live Share](https://marketplace.visualstudio.com/items?itemName=ms-vsliveshare.vsliveshare)。

## <a name="see-also"></a>另请参阅

[编辑和重构代码 (C++)](writing-and-refactoring-code-cpp.md)</br>
[在 Visual Studio 中导航 C++ 代码库](navigate-code-cpp.md)</br>
[阅读并理解 C++ 代码](read-and-understand-code-cpp.md)</br>
