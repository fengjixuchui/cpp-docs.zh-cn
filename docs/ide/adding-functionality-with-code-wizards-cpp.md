---
title: 用代码向导添加功能 (C++)
ms.date: 05/14/2019
helpviewer_keywords:
- code wizards [C++]
ms.assetid: 6afb7ef9-7056-423d-b244-91bb4236d1d7
ms.openlocfilehash: 651eac8c630636455dd1ffa04fdd02b71fa3cf62
ms.sourcegitcommit: a1676bf6caae05ecd698f26ed80c08828722b237
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2020
ms.locfileid: "91509700"
---
# <a name="adding-functionality-with-code-wizards-c"></a>用代码向导添加功能 (C++)

创建项目后，你会需要更改或添加该项目的功能。 这类任务包括创建新类、添加新成员函数和变量以及添加自动化方法和属性。 可使用代码向导执行所有这些操作。

> [!NOTE]
> Visual Studio 2019 中删除了以下很少使用的代码向导。 对 ATL 和 MFC 的常规支持不会受到删除这些向导的影响。 这些技术的示例代码存档在 Microsoft Docs 和 VCSamples GitHub 存储库中。

- ATL COM+ 1.0 组件向导
- ATL Active Server Pages 组件向导
- ATL OLE DB 提供程序向导
- ATL 属性页向导
- ATL OLE DB 使用者向导
- MFC ODBC 使用者
- ActiveX 控件中的 MFC 类
- Type Lib 中的 MFC 类。

> [!NOTE]
> 可以添加消息处理程序并将消息映射到它们，然后使用[MFC 经典向导](../mfc/reference/mfc-class-wizard.md)替代 MFC 虚拟函数。

## <a name="accessing-c-code-wizards"></a>访问 C++ 代码向导

可从三个位置访问 C++ 代码向导：

- 使用“项目”菜单上的“添加新项”命令打开 `Add New Item` 对话框，此对话框有助于将新文件添加到你的项目********。 “添加类”命令显示[添加类](./adding-a-class-visual-cpp.md#add-class-dialog-box)对话框，对于可以添加到项目的每个类类型，此对话框可为依次打开向导****。 对于 MFC 类，使用 [MFC 类向导](../mfc/reference/mfc-class-wizard.md)。 “添加资源”命令显示[添加资源](../windows/how-to-create-a-resource-script-file.md)对话框，可以从该对话框创建或选择要添加到项目的资源****。

   如果在类视图中突出显示项目中的一个类或接口，则“项目”菜单还会显示以下命令****：

  - **实现接口**（仅从控件类实现）

  - **添加函数**

  - **添加变量**

  - **添加连接点**（仅适用于 ATL 类）

  - **添加方法**（仅从接口添加）

  - **添加属性**（仅从接口添加）

  - **添加事件**（仅从控件类添加）

- 在“解决方案资源管理器”中，右键单击任一文件夹并单击快捷菜单中的“添加”，可将新文件或现有文件、更多文件夹、项、类、资源和 Web 引用添加到项目********。

- 从[类视图窗口](/visualstudio/ide/viewing-the-structure-of-code)，右键单击适当的节点并单击快捷菜单中的“添加”，可将函数、变量、类、属性、方法、事件、接口、连接点或其他代码添加到项目****。

   > [!NOTE]
   > Visual Studio 不提供将接口添加到项目的向导。 可将接口添加到 ATL 项目或通过使用 [ATL 简单对象向导](../atl/reference/atl-simple-object-wizard.md)添加一个简单项目，从而[向 MFC 项目添加 ATL 支持](../mfc/reference/adding-atl-support-to-your-mfc-project.md)。 或者打开项目的 .idl 文件并通过键入以下内容创建接口：

    ```IDL
    interface IMyInterface {
    };
    ```

   有关详细信息，请参阅[实现接口](../ide/implementing-an-interface-visual-cpp.md)和[向 ATL 项目添加对象和控件](../atl/reference/adding-objects-and-controls-to-an-atl-project.md)。

   |访问代码向导的途径|说明|
   |-----------------------------|-----------------|
   |添加新项|“添加新项”代码向导将源文件添加到项目。 如有必要，创建其他目录来包含项目生成引擎所需的文件。 可从添加项图标访问的代码向导包括：<br /><br />- 添加 C++ 源文件（.cpp、.h、.idl、.rc、.srf、.def 和 .rgs）。<br />- 添加 Web 开发文件（.html、.asp、.css 和 .xml）。<br />- 添加实用工具和资源文件（.bmp、.cur、.ico、.rct、.sql 和 .txt）。<br /><br />这些代码向导通常会将文件添加到你的开发树但不会询问任何信息。 你可以重命名属性窗口中的文件。|
   |“解决方案资源管理器”|解决方案资源管理器中可用的代码向导取决于右键单击某项时光标焦点的位置。 如果右键单击某项时没有出现“添加”选项，请在开发树中将光标向上移动一级并重试****。 代码向导会始终将其他代码放在开发树中适当的位置，且不受光标位置的影响。 可从解决方案资源管理器访问的代码向导包括：<br /><br />- 添加类（打开包含新代码向导的“添加类”对话框）。<br />- 添加资源（新建、导入或自定义）。<br />- 添加 Web 引用。|
   |类视图|类视图中可用的代码向导取决于右键单击某个项时光标焦点的位置。 如果右键单击某项时没有出现“添加”选项，请在类树中将光标向上移动一级并重试****。 代码向导会始终将其他代码放在开发树中适当的位置，且不受光标位置的影响。 可从类视图访问的代码向导包括：<br /><br />- [添加成员函数](../ide/adding-a-member-function-visual-cpp.md)。<br />- [添加成员变量](../ide/adding-a-member-variable-visual-cpp.md)。<br />- [添加类](../ide/adding-a-class-visual-cpp.md)。<br />- [实现接口](./implementing-an-interface-visual-cpp.md#implement-interface-wizard)（仅从控件类实现）<br />- [添加连接点](./implementing-a-connection-point-visual-cpp.md#implement-connection-point-wizard)（仅适用于 ATL 类）<br />- [添加方法](./adding-a-method-visual-cpp.md#add-method-wizard)（仅从接口添加）<br />- [添加属性](./adding-a-property-visual-cpp.md#names-add-property-wizard)（仅从接口添加）<br />- [添加事件](./adding-an-event-visual-cpp.md#add-event-wizard)（仅从控件类添加）<br /><br />选择“添加类”打开“添加类”对话框，此对话框能让你访问所有新的“添加类”代码向导****。|

## <a name="see-also"></a>请参阅

[重写虚函数](../ide/overriding-a-virtual-function-visual-cpp.md)<br>
[在 Visual Studio 中导航 C++ 代码库](../ide/navigate-code-cpp.md)<br>
[Visual Studio 中的 C++ 项目类型](../build/reference/visual-cpp-project-types.md)<br>
[为 Visual Studio C++ 项目创建的文件类型](../build/reference/file-types-created-for-visual-cpp-projects.md)
