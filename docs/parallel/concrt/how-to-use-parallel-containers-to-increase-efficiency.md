---
title: 如何：使用并行容器提高效率
ms.date: 11/04/2016
helpviewer_keywords:
- increasing efficiency with parallel containers [Concurrency Runtime]
- concurrent_queue class, examples
- concurrent_vector class, examples
ms.assetid: bd00046d-e9b6-4ae1-b661-3995f671b867
ms.openlocfilehash: 361e0e32efb45468ba108ee975879f990ac98395
ms.sourcegitcommit: 43cee7a0d41a062661229043c2f7cbc6ace17fa3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92008325"
---
# <a name="how-to-use-parallel-containers-to-increase-efficiency"></a>如何：使用并行容器提高效率

本主题说明如何使用并行容器以并行方式高效存储和访问数据。

示例代码并行计算质数和 Carmichael 数字集。 然后，对于每个 Carmichael 号，代码将计算该数字的质数。

## <a name="example-determine-if-an-input-value-is-a-prime-number"></a>示例：确定输入值是否为质数

下面的示例演示了 `is_prime` 一个函数，该函数确定输入值是否为质数，并使用 `is_carmichael` 函数确定输入值是否为 Carmichael。

[!code-cpp[concrt-carmichael-primes#1](../../parallel/concrt/codesnippet/cpp/how-to-use-parallel-containers-to-increase-efficiency_1.cpp)]

## <a name="example-compute-prime-and-carmichael-numbers"></a>示例：计算质数和 Carmichael 数字

下面的示例使用 `is_prime` 和 `is_carmichael` 函数来计算质数和 Carmichael 数的集。 该示例使用 [concurrency：:p arallel_invoke](reference/concurrency-namespace-functions.md#parallel_invoke) 和 [concurrency：:p arallel_for](reference/concurrency-namespace-functions.md#parallel_for) 算法并行计算每个集。 有关并行算法的详细信息，请参阅 [并行算法](../../parallel/concrt/parallel-algorithms.md)。

此示例使用 [concurrency：： concurrent_queue](../../parallel/concrt/reference/concurrent-queue-class.md) 对象保存 Carmichael 数字集，因为稍后会将该对象用作工作队列。 它使用 [concurrency：： concurrent_vector](../../parallel/concrt/reference/concurrent-vector-class.md) 对象来保存质数集，因为它稍后将循环访问此集以查找主要因素。

[!code-cpp[concrt-carmichael-primes#2](../../parallel/concrt/codesnippet/cpp/how-to-use-parallel-containers-to-increase-efficiency_2.cpp)]

## <a name="example-find-all-prime-factors-of-a-given-value"></a>示例：查找给定值的所有质数系数

下面的示例演示了 `prime_factors_of` 一个函数，该函数使用试除法来查找给定值的所有质数因子。

此函数使用 [concurrency：:p arallel_for_each](reference/concurrency-namespace-functions.md#parallel_for_each) 算法来循环访问质数的集合。 `concurrent_vector`通过对象，并行循环可以同时向结果中添加质数。

[!code-cpp[concrt-carmichael-primes#3](../../parallel/concrt/codesnippet/cpp/how-to-use-parallel-containers-to-increase-efficiency_3.cpp)]

## <a name="example-processes-each-element-in-the-queue-of-carmichael-numbers"></a>示例：处理 Carmichael 数字队列中的每个元素

此示例通过调用函数来计算 Carmichael 数字队列中的每个元素 `prime_factors_of` ，以计算其主要因素。 它使用任务组并行执行此工作。 有关任务组的详细信息，请参阅 [任务并行](../../parallel/concrt/task-parallelism-concurrency-runtime.md)。

此示例将打印每个 Carmichael 数的质数因子（如果该数字具有四个以上的质数系数）。

[!code-cpp[concrt-carmichael-primes#4](../../parallel/concrt/codesnippet/cpp/how-to-use-parallel-containers-to-increase-efficiency_4.cpp)]

## <a name="example-finished-parallel-container-code-sample"></a>示例：已完成并行容器代码示例

以下代码显示了完整的示例，该示例使用并行容器来计算 Carmichael 数字的质数系数。

[!code-cpp[concrt-carmichael-primes#5](../../parallel/concrt/codesnippet/cpp/how-to-use-parallel-containers-to-increase-efficiency_5.cpp)]

此示例将生成以下示例输出。

```Output
Prime factors of 9890881 are: 7 11 13 41 241.
Prime factors of 825265 are: 5 7 17 19 73.
Prime factors of 1050985 are: 5 13 19 23 37.
```

## <a name="compiling-the-code"></a>编译代码

复制代码示例并将其粘贴到 Visual Studio 项目中，或粘贴到一个名为的文件中， `carmichael-primes.cpp` 然后在 Visual Studio 命令提示符窗口中运行以下命令。

> **cl.exe/EHsc carmichael-primes**

## <a name="see-also"></a>请参阅

[并行容器和对象](../../parallel/concrt/parallel-containers-and-objects.md)<br/>
[任务并行度](../../parallel/concrt/task-parallelism-concurrency-runtime.md)<br/>
[concurrent_vector 类](../../parallel/concrt/reference/concurrent-vector-class.md)<br/>
[concurrent_queue 类](../../parallel/concrt/reference/concurrent-queue-class.md)<br/>
[parallel_invoke 函数](reference/concurrency-namespace-functions.md#parallel_invoke)<br/>
[parallel_for 函数](reference/concurrency-namespace-functions.md#parallel_for)<br/>
[task_group 类](reference/task-group-class.md)
