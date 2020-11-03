---
title: 演练：创建基于代理的应用程序
ms.date: 04/25/2019
helpviewer_keywords:
- asynchronous agents, creating
- agent class, example
ms.assetid: 730f42ce-6d58-4753-b948-fd9c9ef2ce6c
ms.openlocfilehash: 9d9fdd3ddface01f84f6426dd334600cf88b84e7
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924829"
---
# <a name="walkthrough-creating-an-agent-based-application"></a>演练：创建基于代理的应用程序

本主题介绍如何创建基于代理的基本应用程序。 在本演练中，可以创建一个代理，用于以异步方式从文本文件读取数据。 应用程序使用 Adler-32 校验和算法来计算该文件的内容的校验和。

## <a name="prerequisites"></a>先决条件

若要完成本演练，必须了解以下主题：

- [异步代理](../../parallel/concrt/asynchronous-agents.md)

- [异步消息块](../../parallel/concrt/asynchronous-message-blocks.md)

- [消息传递函数](../../parallel/concrt/message-passing-functions.md)

- [同步数据结构](../../parallel/concrt/synchronization-data-structures.md)

## <a name="sections"></a><a name="top"></a> 个

本演练演示如何执行以下任务：

- [创建控制台应用](#createapplication)

- [创建 file_reader 类](#createagentclass)

- [在应用程序中使用 file_reader 类](#useagentclass)

## <a name="creating-the-console-application"></a><a name="createapplication"></a> 创建控制台应用程序

本部分演示如何创建一个 c + + 控制台应用程序，该应用程序引用程序将使用的头文件。 初始步骤因您使用的 Visual Studio 的版本而异。 若要查看 Visual Studio 首选项的文档，请使用“版本”选择器控件。 它位于此页面上目录表的顶部。

::: moniker range="msvc-160"

### <a name="to-create-a-c-console-application-in-visual-studio-2019"></a>在 Visual Studio 2019 中创建 c + + 控制台应用程序

1. 在主菜单中，依次选择“文件”>“新建”>“项目”，以打开“新建项目”对话框。

1. 在对话框顶部，将“语言”  设置为“C++”  ，将“平台”  设置为“Windows”  ，并将“项目类型”  设置为“控制台”  。

1. 从筛选的项目类型列表中，选择“控制台应用”，然后选择“下一步”   。 在下一页中，输入 `BasicAgent` 作为项目的名称，并指定项目位置（如果需要）。

1. 选择“创建”  按钮创建项目。

1. 右键单击 " **解决方案资源管理器** 中的项目节点，然后选择" **属性** "。 在 " **配置属性** " "  >  **c/c + +**  >  **预编译头** "  >  **预编译标头** **Create**

::: moniker-end

::: moniker range="<=msvc-150"

### <a name="to-create-a-c-console-application-in-visual-studio-2017-and-earlier"></a>在 Visual Studio 2017 和更早版本中创建 c + + 控制台应用程序

1. 在 " **文件** " 菜单上，单击 " **新建** "，然后单击 " **项目** " 以显示 " **新建项目** " 对话框。

1. 在 " **新建项目** " 对话框中，选择 " **项目类型** " 窗格中的 " **Visual C++** " 节点，然后在 " **模板** " 窗格中选择 " **Win32 控制台应用程序** "。 键入项目的名称，例如，， `BasicAgent` 然后单击 **"确定"** 以显示 " **Win32 控制台应用程序向导** "。

1. 在 " **Win32 控制台应用程序向导** " 对话框中，单击 " **完成** "。

::: moniker-end

1. 在 *(Visual* Studio 2017 及更低) 版本的 *stdafx.h* 中，添加以下代码：

[!code-cpp[concrt-basic-agent#1](../../parallel/concrt/codesnippet/cpp/walkthrough-creating-an-agent-based-application_1.h)]

   标头文件代理包含 [concurrency：： agent](../../parallel/concrt/reference/agent-class.md) 类的功能。

1. 通过生成并运行该应用程序，验证该应用程序是否已成功创建。 若要生成应用程序，请在 " **生成** " 菜单上单击 " **生成解决方案** "。 如果应用程序成功生成，则通过单击 " **调试** " 菜单上的 " **启动调试** " 来运行该应用程序。

[[顶部](#top)]

## <a name="creating-the-file_reader-class"></a><a name="createagentclass"></a> 创建 file_reader 类

本部分演示如何创建 `file_reader` 类。 运行时计划每个代理在其自身的上下文中执行工作。 因此，你可以创建一个以同步方式执行工作但与其他组件异步交互的代理。 `file_reader`类从给定的输入文件中读取数据，并将数据从该文件发送到给定的目标组件。

#### <a name="to-create-the-file_reader-class"></a>创建 file_reader 类

1. 向项目中添加新的 c + + 头文件。 为此，请在 **解决方案资源管理器** 中右键单击 " **头文件** " 节点，单击 " **添加** "，然后单击 " **新建项** "。 在 " **模板** " 窗格中，选择 " **头文件 ( .h)** 。 在 " **添加新项** " 对话框中，在 `file_reader.h` " **名称** " 框中键入，然后单击 " **添加** "。

1. 在 file_reader 中，添加以下代码。

[!code-cpp[concrt-basic-agent#17](../../parallel/concrt/codesnippet/cpp/walkthrough-creating-an-agent-based-application_2.h)]

1. 在 file_reader 中，创建一个从派生的名为 `file_reader` 的类 `agent` 。

[!code-cpp[concrt-basic-agent#2](../../parallel/concrt/codesnippet/cpp/walkthrough-creating-an-agent-based-application_3.h)]

1. 将以下数据成员添加到 **`private`** 类的部分。

[!code-cpp[concrt-basic-agent#3](../../parallel/concrt/codesnippet/cpp/walkthrough-creating-an-agent-based-application_4.h)]

   `_file_name`成员是代理读取的文件名。 `_target`成员是代理将文件内容写入到的[concurrency：： ITarget](../../parallel/concrt/reference/itarget-class.md)对象。 该 `_error` 成员保存在代理生命周期内发生的任何错误。

1. 将下面的构造函数的代码添加 `file_reader` 到 **`public`** 类的部分 `file_reader` 。

[!code-cpp[concrt-basic-agent#4](../../parallel/concrt/codesnippet/cpp/walkthrough-creating-an-agent-based-application_5.h)]

   每个构造函数重载设置 `file_reader` 数据成员。 第二个和第三个构造函数重载使您的应用程序可以将特定的计划程序用于您的代理。 第一个重载将默认计划程序用于代理。

1. 将 `get_error` 方法添加到类的公共部分 `file_reader` 。

[!code-cpp[concrt-basic-agent#5](../../parallel/concrt/codesnippet/cpp/walkthrough-creating-an-agent-based-application_6.h)]

   `get_error`方法检索在代理生命周期内发生的任何错误。

1. 在类的部分中实现 [concurrency：： agent：： run](reference/agent-class.md#run) 方法 **`protected`** 。

[!code-cpp[concrt-basic-agent#6](../../parallel/concrt/codesnippet/cpp/walkthrough-creating-an-agent-based-application_7.h)]

`run`方法打开文件，并从中读取数据。 `run`方法使用异常处理来捕获文件处理期间发生的任何错误。

   此方法每次从文件读取数据时，它都会调用 [concurrency：： asend](reference/concurrency-namespace-functions.md#asend) 函数以将该数据发送到目标缓冲区。 它将空字符串发送到其目标缓冲区以指示处理结束。

下面的示例演示 file_reader 的完整内容。

[!code-cpp[concrt-basic-agent#7](../../parallel/concrt/codesnippet/cpp/walkthrough-creating-an-agent-based-application_8.h)]

[[顶部](#top)]

## <a name="using-the-file_reader-class-in-the-application"></a><a name="useagentclass"></a> 在应用程序中使用 file_reader 类

本部分演示如何使用 `file_reader` 类读取文本文件的内容。 它还演示了如何创建 [concurrency：： call](../../parallel/concrt/reference/call-class.md) 对象，该对象接收此文件数据并计算其 Adler-32 校验和。

#### <a name="to-use-the-file_reader-class-in-your-application"></a>在应用程序中使用 file_reader 类

1. 在 BasicAgent 中，添加以下 `#include` 语句。

[!code-cpp[concrt-basic-agent#8](../../parallel/concrt/codesnippet/cpp/walkthrough-creating-an-agent-based-application_9.cpp)]

1. 在 BasicAgent 中，添加以下 **`using`** 指令。

[!code-cpp[concrt-basic-agent#9](../../parallel/concrt/codesnippet/cpp/walkthrough-creating-an-agent-based-application_10.cpp)]

1. 在 `_tmain` 函数中，创建一个对处理结束发出信号的 [concurrency：： event](../../parallel/concrt/reference/event-class.md) 对象。

[!code-cpp[concrt-basic-agent#10](../../parallel/concrt/codesnippet/cpp/walkthrough-creating-an-agent-based-application_11.cpp)]

1. 创建一个 `call` 对象，该对象在接收数据时更新校验和。

[!code-cpp[concrt-basic-agent#11](../../parallel/concrt/codesnippet/cpp/walkthrough-creating-an-agent-based-application_12.cpp)]

   此 `call` 对象还在 `event` 收到空字符串时设置对象，以指示处理结束。

1. 创建一个 `file_reader` 对象，该对象从文件读取 test.txt 并将该文件的内容写入 `call` 对象中。

[!code-cpp[concrt-basic-agent#12](../../parallel/concrt/codesnippet/cpp/walkthrough-creating-an-agent-based-application_13.cpp)]

1. 启动代理，并等待其完成。

[!code-cpp[concrt-basic-agent#13](../../parallel/concrt/codesnippet/cpp/walkthrough-creating-an-agent-based-application_14.cpp)]

1. 等待 `call` 对象接收所有数据并完成。

[!code-cpp[concrt-basic-agent#14](../../parallel/concrt/codesnippet/cpp/walkthrough-creating-an-agent-based-application_15.cpp)]

1. 检查文件读取器中是否有错误。 如果未发生错误，则计算最终的 Adler-32 sum，并将和输出到控制台。

[!code-cpp[concrt-basic-agent#15](../../parallel/concrt/codesnippet/cpp/walkthrough-creating-an-agent-based-application_16.cpp)]

下面的示例显示了完整的 BasicAgent 文件。

[!code-cpp[concrt-basic-agent#16](../../parallel/concrt/codesnippet/cpp/walkthrough-creating-an-agent-based-application_17.cpp)]

[[顶部](#top)]

## <a name="sample-input"></a>示例输入

这是输入文件 text.txt 的示例内容：

```Output
The quick brown fox
jumps
over the lazy dog
```

## <a name="sample-output"></a>示例输出

与示例输入一起使用时，此程序生成以下输出：

```Output
Adler-32 sum is fefb0d75
```

## <a name="robust-programming"></a>可靠编程

为了防止对数据成员的并发访问，我们建议你将执行工作的方法添加到 **`protected`** 类的或 **`private`** 部分。 仅向类的部分添加发送或接收消息的方法 **`public`** 。

始终调用 [concurrency：： agent：:d 一种](reference/agent-class.md#done) 方法将代理移动到已完成状态。 在从方法返回之前，通常会调用此方法 `run` 。

## <a name="next-steps"></a>后续步骤

有关基于代理的应用程序的另一个示例，请参阅 [演练：使用联接防止死锁](../../parallel/concrt/walkthrough-using-join-to-prevent-deadlock.md)。

## <a name="see-also"></a>请参阅

[异步代理库](../../parallel/concrt/asynchronous-agents-library.md)<br/>
[异步消息块](../../parallel/concrt/asynchronous-message-blocks.md)<br/>
[消息传递函数](../../parallel/concrt/message-passing-functions.md)<br/>
[同步数据结构](../../parallel/concrt/synchronization-data-structures.md)<br/>
[演练：使用 join 避免死锁](../../parallel/concrt/walkthrough-using-join-to-prevent-deadlock.md)
