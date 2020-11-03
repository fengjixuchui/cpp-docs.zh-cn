---
title: Scheduler 类
ms.date: 11/04/2016
f1_keywords:
- Scheduler
- CONCRT/concurrency::Scheduler
- CONCRT/concurrency::Scheduler::Scheduler
- CONCRT/concurrency::Scheduler::Attach
- CONCRT/concurrency::Scheduler::Create
- CONCRT/concurrency::Scheduler::CreateScheduleGroup
- CONCRT/concurrency::Scheduler::GetNumberOfVirtualProcessors
- CONCRT/concurrency::Scheduler::GetPolicy
- CONCRT/concurrency::Scheduler::Id
- CONCRT/concurrency::Scheduler::IsAvailableLocation
- CONCRT/concurrency::Scheduler::Reference
- CONCRT/concurrency::Scheduler::RegisterShutdownEvent
- CONCRT/concurrency::Scheduler::Release
- CONCRT/concurrency::Scheduler::ResetDefaultSchedulerPolicy
- CONCRT/concurrency::Scheduler::ScheduleTask
- CONCRT/concurrency::Scheduler::SetDefaultSchedulerPolicy
helpviewer_keywords:
- Scheduler class
ms.assetid: 34cf7961-048d-4852-8a5c-a32f823e3506
ms.openlocfilehash: 77ad876b8352ab1ae86fde622b05712ec5f2cea9
ms.sourcegitcommit: 19016630f9d35f365e9ba249e0f3617515d7ca33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92274720"
---
# <a name="scheduler-class"></a>Scheduler 类

表示并发运行时计划程序的抽象。

## <a name="syntax"></a>语法

```cpp
class Scheduler;
```

## <a name="members"></a>成员

### <a name="protected-constructors"></a>受保护的构造函数

|名称|说明|
|----------|-----------------|
|[计划程序](#ctor)|类的对象 `Scheduler` 只能使用工厂方法创建，或隐式创建。|
|[~ 计划程序析构函数](#dtor)|`Scheduler`当对类的所有外部引用停止存在时，该类的对象将被隐式销毁。|

### <a name="public-methods"></a>公共方法

|“属性”|说明|
|----------|-----------------|
|[附加](#attach)|将计划程序附加到调用上下文。 此方法返回后，调用上下文由计划程序管理，计划程序将成为当前计划程序。|
|[创建](#create)|创建新的计划程序，该计划程序的行为由 `_Policy` 参数描述，将初始引用置于计划程序上，并返回指向它的指针。|
|[CreateScheduleGroup](#createschedulegroup)|已重载。 在计划程序中创建新的计划组。 采用参数的版本 `_Placement` 导致新创建的计划组内的任务偏离到在该参数指定的位置执行。|
|[GetNumberOfVirtualProcessors](#getnumberofvirtualprocessors)|返回计划程序的当前虚拟处理器数。|
|[GetPolicy](#getpolicy)|返回创建计划程序所使用的策略的副本。|
|[Id](#id)|返回计划程序的唯一标识符。|
|[IsAvailableLocation](#isavailablelocation)|确定给定位置在计划程序上是否可用。|
|[引用](#reference)|递增计划程序引用计数。|
|[RegisterShutdownEvent](#registershutdownevent)|导致 `_Event` 当计划程序关闭并销毁自身时，在参数中传递的 Windows 事件句柄发出信号。 在事件终止时，已计划计划程序的所有工作都已完成。 可以通过此方法注册多个关闭事件。|
|[版本](#release)|递减计划程序的引用计数。|
|[ResetDefaultSchedulerPolicy](#resetdefaultschedulerpolicy)|将默认计划程序策略重置为运行时默认值。 下一次创建默认计划程序时，它将使用运行时默认策略设置。|
|[ScheduleTask](#scheduletask)|已重载。 计划计划程序中的轻量任务。 轻量任务将放置在运行时确定的计划组中。 采用参数 `_Placement` 的版本导致任务偏向在指定的位置执行。|
|[SetDefaultSchedulerPolicy](#setdefaultschedulerpolicy)|允许使用用户定义的策略来创建默认计划程序。 仅当进程中不存在任何默认计划程序时，才能调用此方法。 设置默认策略后，它将保持有效，直到下一次对 `SetDefaultSchedulerPolicy` 或 [ResetDefaultSchedulerPolicy](#resetdefaultschedulerpolicy) 方法的有效调用。|

## <a name="remarks"></a>备注

并发运行时计划程序使用映射到操作系统执行上下文（如线程）的执行上下文来执行应用程序在其上排队的工作。 在任何时候，计划程序的并发级别都等于资源管理器向其授予的虚拟处理器的数目。 虚拟处理器是处理资源的抽象，并映射到基础系统上的硬件线程。 在给定的时间，只有单个计划程序上下文可在虚拟处理器上执行。

并发运行时将为每个进程创建一个默认计划程序以执行并行工作。 此外，还可以创建自己的计划程序实例，并使用此类对其进行操作。

## <a name="inheritance-hierarchy"></a>继承层次结构

`Scheduler`

## <a name="requirements"></a>要求

**标头：** concrt

**命名空间：** 并发

## <a name="attach"></a><a name="attach"></a> 附件

将计划程序附加到调用上下文。 此方法返回后，调用上下文由计划程序管理，计划程序将成为当前计划程序。

```cpp
virtual void Attach() = 0;
```

### <a name="remarks"></a>备注

附加计划程序会将引用隐式置于计划程序中。

在将来的某个时间点，必须调用 [CurrentScheduler：:D etach](currentscheduler-class.md#detach) 方法，以允许计划程序关闭。

如果从已附加到不同计划程序的上下文中调用此方法，则会将现有计划程序视为前一个计划程序，新创建的计划程序将成为当前计划程序。 稍后调用方法时 `CurrentScheduler::Detach` ，会将上一个计划程序还原为当前计划程序。

如果此计划程序是调用上下文的当前计划程序，则此方法将引发 [improper_scheduler_attach](improper-scheduler-attach-class.md) 异常。

## <a name="create"></a><a name="create"></a>创建

创建新的计划程序，该计划程序的行为由 `_Policy` 参数描述，将初始引用置于计划程序上，并返回指向它的指针。

```cpp
static Scheduler* __cdecl Create(const SchedulerPolicy& _Policy);
```

### <a name="parameters"></a>参数

*_Policy*<br/>
描述新创建的计划程序行为的计划程序策略。

### <a name="return-value"></a>返回值

指向新创建的计划程序的指针。 此 `Scheduler` 对象上放置了初始引用计数。

### <a name="remarks"></a>备注

使用方法创建计划程序后 `Create` ，必须在 `Release` 未来的某个时间点调用方法，以便删除初始引用计数并允许计划程序关闭。

使用此方法创建的计划程序未附加到调用上下文。 它可以使用 [Attach](#attach) 方法附加到上下文。

此方法可能会引发各种异常，包括 [scheduler_resource_allocation_error](scheduler-resource-allocation-error-class.md) 和 [invalid_scheduler_policy_value](invalid-scheduler-policy-value-class.md)。

## <a name="createschedulegroup"></a><a name="createschedulegroup"></a> CreateScheduleGroup

在计划程序中创建新的计划组。 采用参数的版本 `_Placement` 导致新创建的计划组内的任务偏离到在该参数指定的位置执行。

```cpp
virtual ScheduleGroup* CreateScheduleGroup() = 0;

virtual ScheduleGroup* CreateScheduleGroup(location& _Placement) = 0;
```

### <a name="parameters"></a>参数

*_Placement*<br/>
对计划组中的任务在其下执行的偏差位置的引用。

### <a name="return-value"></a>返回值

指向新创建的计划组的指针。 此 `ScheduleGroup` 对象上放置了初始引用计数。

### <a name="remarks"></a>备注

向计划组计划工作完成后，必须对计划组调用 [Release](schedulegroup-class.md#release) 方法。 当队列中的所有工作完成后，计划程序将销毁计划组。

请注意，如果显式创建了此计划程序，则在发布对计划程序的引用之前，必须释放所有引用以计划该组内的组。

## <a name="getnumberofvirtualprocessors"></a><a name="getnumberofvirtualprocessors"></a> GetNumberOfVirtualProcessors

返回计划程序的当前虚拟处理器数。

```cpp
virtual unsigned int GetNumberOfVirtualProcessors() const = 0;
```

### <a name="return-value"></a>返回值

计划程序的当前虚拟处理器数。

## <a name="getpolicy"></a><a name="getpolicy"></a> GetPolicy

返回创建计划程序所使用的策略的副本。

```cpp
virtual SchedulerPolicy GetPolicy() const = 0;
```

### <a name="return-value"></a>返回值

创建计划程序所使用的策略的副本。

## <a name="id"></a><a name="id"></a> 识别

返回计划程序的唯一标识符。

```cpp
virtual unsigned int Id() const = 0;
```

### <a name="return-value"></a>返回值

计划程序的唯一标识符。

## <a name="isavailablelocation"></a><a name="isavailablelocation"></a> IsAvailableLocation

确定给定位置在计划程序上是否可用。

```cpp
virtual bool IsAvailableLocation(const location& _Placement) const = 0;
```

### <a name="parameters"></a>参数

*_Placement*<br/>
用于查询计划程序的位置的引用。

### <a name="return-value"></a>返回值

`_Placement`计划程序上是否提供由参数指定的位置。

### <a name="remarks"></a>备注

注意，返回值是给定位置是否可用的瞬时采样。 在存在多个计划程序时，动态资源管理可以从计划程序中的任意点添加或移除资源。 如果发生这种情况，那么给定位置的可用性会发生更改。

## <a name="reference"></a><a name="reference"></a> 对

递增计划程序引用计数。

```cpp
virtual unsigned int Reference() = 0 ;
```

### <a name="return-value"></a>返回值

新增加的引用计数。

### <a name="remarks"></a>备注

这通常用于管理组合的计划程序的生存期。 当计划程序的引用计数降为零时，计划程序将在其所有工作完成后关闭并析构自身。

如果在调用方法之前，引用计数为零，则该方法将引发 [improper_scheduler_reference](improper-scheduler-reference-class.md) 异常 `Reference` ，并从不由计划程序拥有的上下文进行调用。

## <a name="registershutdownevent"></a><a name="registershutdownevent"></a> RegisterShutdownEvent

导致 `_Event` 当计划程序关闭并销毁自身时，在参数中传递的 Windows 事件句柄发出信号。 在事件终止时，已计划计划程序的所有工作都已完成。 可以通过此方法注册多个关闭事件。

```cpp
virtual void RegisterShutdownEvent(HANDLE _Event) = 0;
```

### <a name="parameters"></a>参数

*_Event*<br/>
Windows 事件对象的句柄，当计划程序关闭并销毁自身时，运行时将向其发出信号。

## <a name="release"></a><a name="release"></a> 拆卸

递减计划程序的引用计数。

```cpp
virtual unsigned int Release() = 0;
```

### <a name="return-value"></a>返回值

新递减的引用计数。

### <a name="remarks"></a>备注

这通常用于管理组合的计划程序的生存期。 当计划程序的引用计数降为零时，计划程序将在其所有工作完成后关闭并析构自身。

## <a name="resetdefaultschedulerpolicy"></a><a name="resetdefaultschedulerpolicy"></a> ResetDefaultSchedulerPolicy

将默认计划程序策略重置为运行时默认值。 下一次创建默认计划程序时，它将使用运行时默认策略设置。

```cpp
static void __cdecl ResetDefaultSchedulerPolicy();
```

### <a name="remarks"></a>备注

当进程中存在默认计划程序时，可以调用此方法。 它不会影响现有默认计划程序的策略。 但是，如果默认计划程序为关闭状态，并且稍后要创建新的默认计划程序，则新计划程序将使用运行时默认策略设置。

## <a name="scheduler"></a><a name="ctor"></a> 日程

类的对象 `Scheduler` 只能使用工厂方法创建，或隐式创建。

```cpp
Scheduler();
```

### <a name="remarks"></a>备注

使用需要将计划程序附加到调用上下文的多个运行时函数时，将隐式创建进程的默认计划程序。 类中的方法 `CurrentScheduler` 和 PPL 和代理层的功能通常会执行隐式连接。

还可以通过方法或方法显式创建计划程序 `CurrentScheduler::Create` `Scheduler::Create` 。

## <a name="scheduler"></a><a name="dtor"></a> ~ 计划程序

`Scheduler`当对类的所有外部引用停止存在时，该类的对象将被隐式销毁。

```cpp
virtual ~Scheduler();
```

## <a name="scheduletask"></a><a name="scheduletask"></a> ScheduleTask

计划计划程序中的轻量任务。 轻量任务将放置在运行时确定的计划组中。 采用参数 `_Placement` 的版本导致任务偏向在指定的位置执行。

```cpp
virtual void ScheduleTask(
    TaskProc _Proc,
    _Inout_opt_ void* _Data) = 0;

virtual void ScheduleTask(
    TaskProc _Proc,
    _Inout_opt_ void* _Data,
    location& _Placement) = 0;
```

### <a name="parameters"></a>参数

*_Proc*<br/>
一个指针，指向用于执行轻量任务的主体的函数。

*_Data*<br/>
指向将作为参数传递给任务正文的数据的 void 指针。

*_Placement*<br/>
对偏向执行轻量任务的位置的引用。

## <a name="setdefaultschedulerpolicy"></a><a name="setdefaultschedulerpolicy"></a> SetDefaultSchedulerPolicy

允许使用用户定义的策略来创建默认计划程序。 仅当进程中不存在任何默认计划程序时，才能调用此方法。 设置默认策略后，它将保持有效，直到下一次对 `SetDefaultSchedulerPolicy` 或 [ResetDefaultSchedulerPolicy](#resetdefaultschedulerpolicy) 方法的有效调用。

```cpp
static void __cdecl SetDefaultSchedulerPolicy(const SchedulerPolicy& _Policy);
```

### <a name="parameters"></a>参数

*_Policy*<br/>
要设置为默认计划程序策略的策略。

### <a name="remarks"></a>备注

如果在 `SetDefaultSchedulerPolicy` 进程内已存在默认计划程序时调用该方法，则运行时将引发 [default_scheduler_exists](default-scheduler-exists-class.md) 异常。

## <a name="see-also"></a>另请参阅

[并发命名空间](concurrency-namespace.md)<br/>
[计划程序类](scheduler-class.md)<br/>
[PolicyElementKey](concurrency-namespace-enums.md)<br/>
[任务计划程序](../../../parallel/concrt/task-scheduler-concurrency-runtime.md)
