---
title: ATL 向导添加的 ATL 支持的详细信息
ms.date: 08/20/2019
f1_keywords:
- vc.codewiz.atl.support
helpviewer_keywords:
- MFC, ATL support
- ATL, MFC projects
ms.assetid: aa66bad0-008f-4886-94c1-2a0a0d04bce4
ms.openlocfilehash: aeac01ce58deb429f14058c06524dff53abde060
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924447"
---
# <a name="details-of-atl-support-added-by-the-atl-wizard"></a>ATL 向导添加的 ATL 支持的详细信息

::: moniker range=">=msvc-160"

[将 ATL 支持添加到现有 MFC 可执行文件或 DLL](../../mfc/reference/adding-atl-support-to-your-mfc-project.md)时，Visual Studio 会在默认情况下添加一个名为 " *框架* " 的头文件，其中包含 `#include` 用于在 `#define` 项目中启用 ATL 的预处理器指令。 不会添加任何其他文件或类，正如在以前版本的 Visual Studio 中所做的那样。

::: moniker-end

::: moniker range="<=msvc-150"

[向现有 mfc 可执行文件或 DLL 添加 ATL 支持](../../mfc/reference/adding-atl-support-to-your-mfc-project.md)时，Visual Studio 将对现有 mfc 项目进行以下修改 (在此示例中，该项目称为 `MFCEXE`) ：

- 两个新文件 (一个 .idl 文件和一个 .rgs 文件，用于注册添加的服务器) 。

- 在主应用程序标头和实现文件中 (Mfcexe 和 Mfcexe) ，将添加派生自) 的新类 (`CAtlMFCModule` 。 除了新类之外，还会向注册添加代码 `InitInstance` 。 还会将代码添加到 `ExitInstance` 用于撤消类对象的函数。 在头文件中，在实现文件中包含两个新的标头文件 (Initguid.h 和 Mfcexe_i) ，并为派生类声明并初始化新的 Guid `CAtlMFCModule` 。

- 若要正确注册服务器，请将新的 .rgs 文件条目添加到项目的资源文件中。

::: moniker-end

## <a name="notes-for-dll-projects"></a>DLL 项目说明

向 MFC DLL 项目添加 ATL 支持时，你将看到一些不同之处。 向和函数添加代码 `DLLRegisterServer` ， `DLLUnregisterServer` 用于注册和注销 DLL。 还将代码添加到 [DllCanUnloadNow](../../atl/reference/catldllmodulet-class.md#dllcanunloadnow) 和 [DllGetClassObject](../../atl/reference/catldllmodulet-class.md#dllgetclassobject)中。

## <a name="see-also"></a>请参阅

[MFC 项目中的 ATL 支持](../../mfc/reference/adding-atl-support-to-your-mfc-project.md)<br/>
[用代码向导添加功能](../../ide/adding-functionality-with-code-wizards-cpp.md)<br/>
[添加类](../../ide/adding-a-class-visual-cpp.md)<br/>
[添加成员函数](../../ide/adding-a-member-function-visual-cpp.md)<br/>
[添加成员变量](../../ide/adding-a-member-variable-visual-cpp.md)<br/>
[重写虚函数](../../ide/overriding-a-virtual-function-visual-cpp.md)<br/>
[MFC 消息处理程序](../../mfc/reference/adding-an-mfc-message-handler.md)<br/>
[导航类结构](../../ide/navigate-code-cpp.md)
