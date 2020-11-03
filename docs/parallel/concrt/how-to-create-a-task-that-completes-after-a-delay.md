---
title: 如何：创建在延迟一段时间后完成的任务
description: 使用 PPL ConcRT 库创建在延迟一段时间后完成的任务。
ms.date: 10/19/2020
helpviewer_keywords:
- task_completion_event class, example
- create a task that completes after a delay, example [C++]
ms.assetid: 3fc0a194-3fdb-4eba-8b8a-b890981a985d
ms.openlocfilehash: 694b6190a7ec60043a5674e920dc54e6e7bf0eb6
ms.sourcegitcommit: 19016630f9d35f365e9ba249e0f3617515d7ca33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92274561"
---
# <a name="how-to-create-a-task-that-completes-after-a-delay"></a>如何：创建在延迟一段时间后完成的任务

此示例演示如何使用 [`concurrency::task`](../../parallel/concrt/reference/task-class.md) 、 [`concurrency::cancellation_token_source`](../../parallel/concrt/reference/cancellation-token-source-class.md) 、、、 [`concurrency::cancellation_token`](../../parallel/concrt/reference/cancellation-token-class.md) [`concurrency::task_completion_event`](../../parallel/concrt/reference/task-completion-event-class.md) [`concurrency::timer`](../../parallel/concrt/reference/timer-class.md) 和 [`concurrency::call`](../../parallel/concrt/reference/call-class.md) 类来创建一个在延迟后完成的任务。 您可以使用此方法来生成偶尔轮询数据的循环。 还可以引入超时、将用户输入的延迟处理预先确定的时间等等。

## <a name="example-complete_after-and-cancel_after_timeout-functions"></a>示例： complete_after 和 cancel_after_timeout 函数

下面的示例显示 `complete_after` 和 `cancel_after_timeout` 函数。 `complete_after` 函数将创建在指定的延迟后完成的 `task` 对象。 它使用 `timer` 对象和 `call` 对象在指定延迟后设置 `task_completion_event` 对象。 通过使用 `task_completion_event` 类，您可以定义在一个线程或另一个任务发出有可用值的信号后完成的任务。 设置事件后，侦听器任务完成，它们的延续按计划运行。

> [!TIP]
> 有关和类的详细 `timer` 信息 `call` ，这些类是异步代理库的一部分，请参阅 [异步消息块](../../parallel/concrt/asynchronous-message-blocks.md)。

`cancel_after_timeout`函数在函数上建立 `complete_after` ，以便在任务未在给定超时之前完成时取消任务。 `cancel_after_timeout` 函数将创建两个任务。 第一个任务指示成功，并在提供的任务完成后完成。 第二个任务指示失败并在指定的超时后完成。 `cancel_after_timeout` 函数将创建一个在成功或失败任务完成后运行的延续任务。 如果失败任务先完成，则延续将取消标记源，以便取消整个任务。

[!code-cpp[concrt-task-delay#1](../../parallel/concrt/codesnippet/cpp/how-to-create-a-task-that-completes-after-a-delay_1.cpp)]

## <a name="example-compute-count-of-prime-numbers"></a>示例：素数的计算计数

下面的示例多次计算范围 [0, 100000] 内质数的计数。 如果未在给定的时间限制内完成，则操作将失败。 `count_primes` 函数演示如何使用 `cancel_after_timeout` 函数。 它计算给定范围内的 primes 数，如果任务在提供的时间内未完成，则会失败。 `wmain` 函数多次调用 `count_primes` 函数。 每次将时间限制减半。 在当前时间限制内操作未完成后，程序完成。

[!code-cpp[concrt-task-delay#2](../../parallel/concrt/codesnippet/cpp/how-to-create-a-task-that-completes-after-a-delay_2.cpp)]

当你使用此方法在延迟后取消任务时，所有未完成的任务都不会在整个任务取消后启动。 但是，任何长时间运行的任务都要快速响应取消操作非常重要。 有关任务取消的详细信息，请参阅 [PPL 中的取消](cancellation-in-the-ppl.md)操作。

## <a name="complete-code-example"></a>完整代码示例

以下是该示例的完整代码：

[!code-cpp[concrt-task-delay#3](../../parallel/concrt/codesnippet/cpp/how-to-create-a-task-that-completes-after-a-delay_3.cpp)]

## <a name="compiling-the-code"></a>编译代码

若要编译代码，请复制代码，然后将其粘贴到 Visual Studio 项目中，或粘贴到一个名为的文件中， `task-delay.cpp` 然后在 Visual Studio 命令提示符窗口中运行以下命令。

**cl.exe/EHsc task-delay**

## <a name="see-also"></a>另请参阅

[任务并行度](../../parallel/concrt/task-parallelism-concurrency-runtime.md)<br/>
[并发运行时的任务类 () ](../../parallel/concrt/reference/task-class.md)<br/>
[cancellation_token_source 类](../../parallel/concrt/reference/cancellation-token-source-class.md)<br/>
[cancellation_token 类](../../parallel/concrt/reference/cancellation-token-class.md)<br/>
[task_completion_event 类](../../parallel/concrt/reference/task-completion-event-class.md)<br/>
[timer 类](../../parallel/concrt/reference/timer-class.md)<br/>
[call 类](../../parallel/concrt/reference/call-class.md)<br/>
[异步消息块](../../parallel/concrt/asynchronous-message-blocks.md)<br/>
[PPL 中的取消操作](cancellation-in-the-ppl.md)
