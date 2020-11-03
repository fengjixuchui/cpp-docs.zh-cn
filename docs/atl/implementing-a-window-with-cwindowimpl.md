---
title: 使用 CWindowImpl 实现窗口
ms.date: 11/04/2016
helpviewer_keywords:
- ATL, windows
- windows [C++], subclassing
- windows [C++], superclassing
- windows [C++], ATL
- subclassing ATL window classes
- superclassing, ATL
ms.assetid: 3fc40550-f1d6-4702-8b7c-4cf682b6a855
ms.openlocfilehash: 7ce1a2ec08e49e047aee5248bda0094d9e392614
ms.sourcegitcommit: ced5ff1431ffbd25b20d106901955532723bd188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2020
ms.locfileid: "92135510"
---
# <a name="implementing-a-window-with-cwindowimpl"></a>使用 CWindowImpl 实现窗口

若要实现窗口，请从派生一个类 `CWindowImpl` 。 在派生类中，声明消息映射和消息处理程序函数。 你现在可以使用三种不同的方法来使用你的类：

- [基于新的 Windows 类创建窗口](#_atl_creating_a_window_based_on_a_new_windows_class)

- [将现有 Windows 类作为超类](#_atl_superclassing_an_existing_windows_class)

- [为现有窗口划分子类](#_atl_subclassing_an_existing_window)

## <a name="creating-a-window-based-on-a-new-windows-class"></a><a name="_atl_creating_a_window_based_on_a_new_windows_class"></a> 基于新的 Windows 类创建窗口

`CWindowImpl` 包含用于声明 Windows 类信息的 [DECLARE_WND_CLASS](reference/window-class-macros.md#declare_wnd_class) 宏。 此宏实现 `GetWndClassInfo` 函数，该函数使用 [CWndClassInfo](../atl/reference/cwndclassinfo-class.md) 定义新的 Windows 类的信息。 `CWindowImpl::Create`调用时，将注册此 Windows 类并创建新窗口。

> [!NOTE]
> `CWindowImpl` 将 NULL 传递给 `DECLARE_WND_CLASS` 宏，这意味着 ATL 将生成一个 Windows 类名称。 若要指定自己的名称，请将字符串传递到派生类中的 DECLARE_WND_CLASS `CWindowImpl` 。

## <a name="example-implement-a-window"></a>示例：实现窗口

下面是一个实现基于新 Windows 类的窗口的类的示例：

[!code-cpp[NVC_ATL_Windowing#64](../atl/codesnippet/cpp/implementing-a-window-with-cwindowimpl_1.h)]

若要创建窗口，请创建的实例， `CMyWindow` 然后调用 `Create` 方法。

> [!NOTE]
> 若要重写默认的 Windows 类信息，请 `GetWndClassInfo` 通过将 `CWndClassInfo` 成员设置为相应的值来实现派生类中的方法。

## <a name="superclassing-an-existing-windows-class"></a><a name="_atl_superclassing_an_existing_windows_class"></a> Superclassing 现有 Windows 类

使用 [DECLARE_WND_SUPERCLASS](reference/window-class-macros.md#declare_wnd_superclass) 宏，可以创建将现有 Windows 类作为超类的窗口。 在派生类中指定此宏 `CWindowImpl` 。 与任何其他 ATL 窗口一样，消息是通过消息映射来处理的。

使用 DECLARE_WND_SUPERCLASS 时，会注册一个新的 Windows 类。 这个新类将与您指定的现有类相同，但会将该窗口过程替换为 `CWindowImpl::WindowProc` (或替换) 的函数。

## <a name="example-superclass-the-edit-class"></a>示例：编辑类的超类

下面是一个类，该类是标准编辑类的超类：

[!code-cpp[NVC_ATL_Windowing#65](../atl/codesnippet/cpp/implementing-a-window-with-cwindowimpl_2.h)]

若要创建超类编辑窗口，请创建的实例， `CMyEdit` 然后调用 `Create` 方法。

## <a name="subclassing-an-existing-window"></a><a name="_atl_subclassing_an_existing_window"></a> 为现有窗口的子类

若要为现有窗口划分子类，请从派生一个类， `CWindowImpl` 并声明一个消息映射，如两个前面的示例中所示。 但请注意，您不指定任何 Windows 类信息，因为您将为现有窗口划分子类。

`Create`调用 `SubclassWindow` 并向其传递指向你要为其划分子类的现有窗口的句柄，而不是调用。 在窗口成为子类后，它将使用 `CWindowImpl::WindowProc` (或重写此方法的函数) 将消息定向到消息映射。 若要从对象分离子类窗口，请调用 `UnsubclassWindow` 。 然后，将还原窗口的原始窗口过程。

## <a name="see-also"></a>另请参阅

[实现窗口](../atl/implementing-a-window.md)
