---
title: 将并发运行时与其他并发模型进行比较
ms.date: 11/04/2016
helpviewer_keywords:
- Concurrency Runtime, compared to other models
ms.assetid: d8b9a1f4-f15f-43c3-a5b4-c0991edf9c86
ms.openlocfilehash: 5bc6691f6d0b166bb3084091ee6af70474937568
ms.sourcegitcommit: 19016630f9d35f365e9ba249e0f3617515d7ca33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92274698"
---
# <a name="comparing-the-concurrency-runtime-to-other-concurrency-models"></a>将并发运行时与其他并发模型进行比较

本文档介绍了并发运行时与其他技术在功能和编程模型上的区别。 通过了解并发运行时与其他编程模型各自优点的比较，你可以选择最能满足应用程序要求的技术。

如果当前正在使用其他编程模型（例如 Windows 线程池或 OpenMP），可以在适当情况下迁移到并发运行时中。 例如，主题 [Migrating from OpenMP to the Concurrency Runtime](../../parallel/concrt/migrating-from-openmp-to-the-concurrency-runtime.md) 描述了何时适合从 OpenMP 迁移到并发运行时。 但是，如果你对应用程序性能和当前调试支持感到满意，则无需进行迁移。

可以使用并发运行时的功能和工作效率的优势来补充使用另一种并发模型的现有应用程序。 多个任务计划程序争夺同一计算资源时，并发运行时无法保证负载平衡。 但是，工作负载不重叠时，这种影响非常小。

## <a name="sections"></a><a name="top"></a> 个

- [比较抢先式计划与协作式计划](#models)

- [将并发运行时与 Windows API 进行比较](#winapi)

- [将并发运行时与 OpenMP 进行比较](#openmp)

## <a name="comparing-preemptive-scheduling-to-cooperative-scheduling"></a><a name="models"></a>比较抢先式计划与协作式计划

抢先式模型和协作式计划模型是启用多任务以共享计算资源的两种常用方式，例如，处理器或硬件线程。

### <a name="preemptive-and-cooperative-scheduling"></a>抢先式和协作式计划

*抢先式计划* 是一种基于优先级的轮循机制，它在给定时间内为每个任务提供单独访问计算资源的权限，并在之后切换到其他任务。 抢先式计划在多任务操作系统（例如 Windows）中很常见。 *协作式计划* 是一种机制，它为每个任务提供对计算资源的独占访问权限，直到任务完成或任务生成对资源的访问权限为止。 并发运行时将操作系统的抢先式计划程序与协作式计划配合使用，以达到处理资源的最大使用率。

### <a name="differences-between-preemptive-and-cooperative-schedulers"></a>抢先式和协作式计划程序之间的差异

抢先式计划程序寻求为多个线程授予同等的计算资源访问权限，确保每个线程都可以取得进行。 在有许多计算资源的计算机上，确保公平的访问权限不再是问题；然而，确保资源的高效利用却成为了主要问题。

抢先式内核模式计划程序要求应用程序代码根据操作系统制定计划决策。 相反，用户模式协作式计划程序允许应用程序代码制定自己的计划决策。 由于协作式计划可使应用程序作出多个计划决策，因此它减少了与内核模式同步相关的大部分开销。 协作式计划程序通常在没有其他工作要计划时，交由操作系统内核来制定计划决策。 协作式计划程序还在有一个传递给内核的阻止操作，但该操作不传递给用户模式计划程序时，交由操作系统计划程序制定计划决策。

### <a name="cooperative-scheduling-and-efficiency"></a>协作式计划和效率

对于抢先式计划程序，所有具有相同优先级的工作都是同等的。 抢先式计划程序通常按进程的创建顺序计划进程。 此外，抢先式计划程序根据线程优先级，以轮循方式为每个线程提供一个时间片。 虽然此机制提供了公平性（每个线程都有所进展），但也要考虑一些效率成本。 例如，许多计算密集型算法不需要公平。 相反，重要的是在最短的时间内完成相关任务。 协作式计划使应用程序更高效地安排工作。 例如，考虑一个具有多个线程的应用程序。 计划不共享资源的线程并发运行可以降低同步开销，从而提高效率。 另一种计划任务的高效方式是在同一处理器上运行任务的管道（每个任务作用于前一个任务的输出），以便每个管道阶段的输入都加载到内存缓存中。

### <a name="using-preemptive-and-cooperative-scheduling-together"></a>共同使用抢先式和协作式计划

协作式计划无法解决所有计划问题。 例如，没有公平地为其他任务让行的任务会消耗所有可用计算资源并阻止其他任务取得进展。 并发运行时使用协作式计划的高效优势来补充抢先式计划的公平性保证。 默认情况下，并发运行时提供一个协作计划程序，该计划程序使用工作窃取算法在计算资源之间高效地分配工作。 但是，并发运行时计划程序还依赖于操作系统的抢先式计划程序，以便在应用程序之间公平地分配资源。 还可以在应用程序中创建自定义计划程序和计划程序策略来对线程执行进行精细控制。

[[顶部](#top)]

## <a name="comparing-the-concurrency-runtime-to-the-windows-api"></a><a name="winapi"></a> 比较并发运行时与 Windows API

Microsoft Windows 应用程序编程接口（通常称为 Windows API，以前称为 Win32）提供了在应用程序中启用并发的编程模型。 并发运行时基于 Windows API 生成，以提供无法从基础操作系统中获得的其他编程模型。

并发运行时基于 Windows API 线程模型生成以执行并行工作。 同时还使用 Windows API 内存管理和线程本地存储区机制。 在 Windows 7 和 Windows Server 2008 R2 上，它将 Windows API 用于支持用户可计划的线程和具有 64 个以上硬件线程的计算机。 并发运行时通过提供协同式任务计划程序和工作窃取算法来最大化计算资源的使用，并启用多个同时计划程序实例，来扩展 Windows API 模型。

### <a name="programming-languages"></a>编程语言

Windows API 使用 C 编程语言来公开编程模型。 并发运行时提供一个采用 C++ 语言中最新功能的 C++ 编程接口。 例如，lambda 函数提供简洁、类型安全的机制，用于定义并行工作函数。 有关并发运行时使用的最新 C++ 功能的详细信息，请参阅[概述](../../parallel/concrt/asynchronous-message-blocks.md)。

### <a name="threads-and-thread-pools"></a>线程和线程池

Windows API 中的中心并发机制是线程。 通常使用 [CreateThread](/windows/win32/api/processthreadsapi/nf-processthreadsapi-createthread) 函数来创建线程。 尽管线程的创建和使用相对简单，但操作系统需要分配大量的时间和其他资源来对管理它们。 此外，虽然每个线程都保证能够收到与其他任何同一优先级的线程相同的执行时间，但相关开销要求你创建足够大的任务。 对于更小或更精细的任务，与并发相关的开销会超过并行运行任务的收益。

线程池是一种降低线程管理成本的方式。 Windows API 提供的自定义线程池和线程池实现都使小型工作项可以高效并行运行。 Windows 线程池维护先入先出 (FIFO) 队列中的工作项。 每个工作项都按其添加到池的顺序依次启动。

并发运行时实现了工作窃取算法以扩展 FIFO 计划机制。 该算法将尚未开始的任务移到用完工作项的线程。 尽管工作窃取算法可以平衡工作负载，但它也会导致工作项被重新排序。 此重新排序过程会导致工作项按与提交顺序不同的顺序启动。 这对于递归算法很有用，数据有更好的机会在较新的任务间共享，而不是在较旧的任务之间共享。 让新项首先运行意味着缓存未命中情况较少且页错误可能较少。

从操作系统的角度来看，工作窃取是不公平的。 但是，当应用程序实现要并行运行的算法或任务时，子任务间的公平性不总是那么重要。 重要的是总体任务的完成速度。 对于其他算法，FIFO 是恰当的计划策略。

### <a name="behavior-on-various-operating-systems"></a>不同操作系统上的行为

在 Windows XP 和 Windows Vista 上，除了在 Windows Vista 上堆的性能有所改进外，使用并发运行时的应用程序的行为相似。

在 Windows 7 和 Windows Server 2008 R2 中，操作系统进一步支持并发和可伸缩性。 例如，这些操作系统支持具有 64 个以上硬件线程的计算机。 使用 Windows API 的现有应用程序必须进行修改才能利用这些新功能。 然而，使用并发运行时的应用程序无需修改就能自动使用这些功能。

[base.user-mode_scheduling](/windows/win32/procthread/user-mode-scheduling)

[[顶部](#top)]

## <a name="comparing-the-concurrency-runtime-to-openmp"></a><a name="openmp"></a> 比较并发运行时与 OpenMP

并发运行时支持各种编程模型。 这些模型可能会与其他库的模型重叠或对其进行补充。 本部分将并发运行时与 [OpenMP](../../parallel/concrt/comparing-the-concurrency-runtime-to-other-concurrency-models.md#openmp) 进行比较。

OpenMP 编程模型由开放标准定义，具有与 Fortran 和 C/C++ 编程语言定义完善的绑定。 OpenMP 2.0 版和 2.5 版都很适合迭代的并行算法；也就是说，它们在数据数组上执行并行迭代。 当预设了并行度，并匹配了系统上的可用资源时，OpenMP 的效率最高。 OpenMP 模型特别适合将大量运算问题分配到单一计算机的处理资源中的高性能计算。 在此方案中，硬件环境已知，开发人员可以合理期望执行该算法时拥有对计算资源的独占访问权限。

但是，OpenMP 可能不适合其他约束较少的计算环境。 例如，通过使用 OpenMP 来实现递归问题（如快速排序算法或搜索数据树）会更加困难。 并发运行时通过提供 [并行模式库](../../parallel/concrt/parallel-patterns-library-ppl.md) (PPL) 和 [异步代理库](../../parallel/concrt/asynchronous-agents-library.md)对 OpenMP 的功能进行了补充。 不同于 OpenMP，并发运行时提供适应可用资源和随工作负载更改调整并行度的动态计划程序。

并发运行时中的许多功能均可扩展。 还可以结合现有功能来构造新功能。 由于 OpenMP 依赖于编译器指令，不能轻松地对其进行扩展。

有关将并发运行时与 OpenMP 进行比较，以及如何迁移现有 OpenMP 代码以使用并发运行时的详细信息，请参阅 [Migrating from OpenMP to the Concurrency Runtime](../../parallel/concrt/migrating-from-openmp-to-the-concurrency-runtime.md)。

[[顶部](#top)]

## <a name="see-also"></a>另请参阅

[并发运行时](../../parallel/concrt/concurrency-runtime.md)<br/>
概述<br/>
[并行模式库 (PPL)](../../parallel/concrt/parallel-patterns-library-ppl.md)<br/>
[异步代理库](../../parallel/concrt/asynchronous-agents-library.md)<br/>
[OpenMP](../../parallel/concrt/comparing-the-concurrency-runtime-to-other-concurrency-models.md#openmp)
