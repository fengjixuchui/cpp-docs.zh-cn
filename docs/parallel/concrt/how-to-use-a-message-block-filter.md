---
title: 如何：使用消息块筛选器
ms.date: 11/04/2016
helpviewer_keywords:
- message-block filters, using [Concurrency Runtime]
- using message-block filters [Concurrency Runtime]
ms.assetid: db6b99fb-288d-4477-96dc-b9751772ebb2
ms.openlocfilehash: ac58ef2240d2ea6ba34b334106c08595e70b02e8
ms.sourcegitcommit: 43cee7a0d41a062661229043c2f7cbc6ace17fa3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92008634"
---
# <a name="how-to-use-a-message-block-filter"></a>如何：使用消息块筛选器

本文档演示如何使用筛选器函数来启用异步消息块，以根据该消息的负载来接受或拒绝消息。

创建诸如 [concurrency：： unbounded_buffer](reference/unbounded-buffer-class.md)、 [concurrency：： call](../../parallel/concrt/reference/call-class.md)或 [concurrency：：变压器](../../parallel/concrt/reference/transformer-class.md)这样的消息块对象时，可以提供 *筛选器函数* 来确定消息块是接受还是拒绝消息。 筛选器函数是一种有效的方法，可保证消息块仅接收某些值。

筛选器函数很重要，因为它们使您能够连接消息块来形成 *数据流网络*。 在数据流网络中，消息块通过只处理符合特定条件的消息来控制数据流。 将此与控制流模型进行比较，在此模型中，通过使用条件语句、循环等控制结构来控制数据流。

本文档提供了有关如何使用消息筛选器的基本示例。 有关使用消息筛选器和数据流模型来连接消息块的其他示例，请参阅 [演练：创建数据流代理](../../parallel/concrt/walkthrough-creating-a-dataflow-agent.md) 和 [演练：创建 Image-Processing 网络](../../parallel/concrt/walkthrough-creating-an-image-processing-network.md)。

## <a name="example-count_primes-function"></a>示例： count_primes 函数

请考虑以下函数，该函数 `count_primes` 阐释不筛选传入消息的消息块的基本用法。 消息块将质数追加到 [std：： vector](../../standard-library/vector-class.md) 对象。 `count_primes`函数向消息块发送多个数字，接收来自消息块的输出值，并将质数输出到控制台。

[!code-cpp[concrt-primes-filter#1](../../parallel/concrt/codesnippet/cpp/how-to-use-a-message-block-filter_1.cpp)]

`transformer`对象处理所有输入值; 但是，它只需要质数值。 尽管可以编写应用程序以便消息发送方仅发送质数，但消息接收方的要求不能始终是已知的。

## <a name="example-count_primes_filter-function"></a>示例： count_primes_filter 函数

下面的函数 `count_primes_filter` 执行与函数相同的任务 `count_primes` 。 但是， `transformer` 此版本中的对象使用筛选器函数来仅接受作为质数的值。 执行操作的函数仅接收质数;因此，它不必调用 `is_prime` 函数。

由于 `transformer` 对象只接收质数，因此 `transformer` 对象本身可以容纳质数。 换句话说， `transformer` 在此示例中，不要求对象将质数添加到 `vector` 对象。

[!code-cpp[concrt-primes-filter#2](../../parallel/concrt/codesnippet/cpp/how-to-use-a-message-block-filter_2.cpp)]

`transformer`现在，对象只处理那些为质数的值。 在上面的示例中， `transformer` 对象处理所有消息。 因此，前面的示例必须接收到其发送的消息数。 此示例使用 [concurrency：： send](reference/concurrency-namespace-functions.md#send) 函数的结果来确定要从该对象接收的消息数 `transformer` 。 `send` **`true`** 当消息缓冲区接受消息和 **`false`** 消息缓冲区拒绝消息时，函数将返回。 因此，消息缓冲区接受消息的次数与质数的计数相匹配。

## <a name="example-finished-message-block-filter-code-sample"></a>示例：已完成消息块筛选器代码示例

以下代码显示完整示例。 该示例调用 `count_primes` 函数和 `count_primes_filter` 函数。

[!code-cpp[concrt-primes-filter#3](../../parallel/concrt/codesnippet/cpp/how-to-use-a-message-block-filter_3.cpp)]

## <a name="compiling-the-code"></a>编译代码

复制代码示例并将其粘贴到 Visual Studio 项目中，或粘贴到一个名为的文件中， `primes-filter.cpp` 然后在 Visual Studio 命令提示符窗口中运行以下命令。

> **cl.exe/EHsc primes-filter**

## <a name="robust-programming"></a>可靠编程

筛选器函数可以是 lambda 函数、函数指针或函数对象。 每个筛选器函数都采用以下形式之一：

```Output
bool (T)
bool (T const &)
```

若要消除不必要的数据复制，请在具有通过值传输的聚合类型时使用第二种形式。

## <a name="see-also"></a>请参阅

[异步代理库](../../parallel/concrt/asynchronous-agents-library.md)<br/>
[演练：创建数据流代理](../../parallel/concrt/walkthrough-creating-a-dataflow-agent.md)<br/>
[演练：创建 Image-Processing 网络](../../parallel/concrt/walkthrough-creating-an-image-processing-network.md)<br/>
[变压器类](../../parallel/concrt/reference/transformer-class.md)
