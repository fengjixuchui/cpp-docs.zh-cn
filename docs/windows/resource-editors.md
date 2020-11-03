---
title: '资源编辑器 (c + +) '
ms.date: 02/14/2019
f1_keywords:
- vs.editors.resource
- vc.resvw.resource.editors
- vs.resvw.resource.editors
- vs.resourceview
- vc.resvw.resource.previewing
- vs.resvw.resource.previewing
helpviewer_keywords:
- editors [C++], resource
- editors [C++]
- resource editors
- Windows [C++], application resource editing
- resources [C++], viewing
- layouts, previewing resource
- resource editors [C++], viewing resources
- .rc files [C++], viewing resources
- resources [C++], editing
- properties [C++], resources
- resources [C++], properties
ms.assetid: e20a29ec-d6fb-4ead-98f3-431a0e23aaaf
ms.openlocfilehash: f854f3cc6e61e3022e784befc738bf28c1adeb6c
ms.sourcegitcommit: 30792632548d1c71894f9fecbe2f554294b86020
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91765242"
---
# <a name="resource-editors-c"></a>资源编辑器 (c + +) 

资源编辑器是用于创建或修改 Visual Studio 项目中所含资源的专用环境。 Visual Studio 资源编辑器共享技术和接口，以帮助你快速、轻松地创建和修改应用程序资源。 通过资源编辑器，你可以在适当的编辑器中查看和编辑资源并预览资源。

创建或打开某个资源时，将自动打开相应的编辑器。

> [!NOTE]
> 由于托管的项目不使用资源脚本文件，因此必须从 **解决方案资源管理器**打开资源。 您可以使用 [图像编辑器](../windows/image-editor-for-icons.md) 和 [二进制编辑器](binary-editor.md) 处理托管项目中的资源文件。 你要编辑的任何托管资源都必须是链接的资源。 Visual Studio 资源编辑器不支持编辑嵌入的资源。

|使用...|编辑...|
|----------------|----------------|
|[快捷键编辑器](../windows/accelerator-editor.md)|Visual Studio c + + 项目中的快捷键对应表。|
|[二进制编辑器](binary-editor.md)|二进制数据信息和 Visual C++、Visual Basic 或 Visual C# 项目中的自定义资源。|
|[对话框编辑器](../windows/dialog-editor.md)|Visual Studio c + + 项目中的对话框。|
|[图像编辑器](../windows/image-editor-for-icons.md)|Visual C++、Visual Basic 或 Visual C# 项目中的位图、图标、光标以及其他图像文件。|
|[菜单编辑器](../windows/menu-editor.md)|Visual Studio c + + 项目中的菜单资源。|
|[Ribbon 编辑器](../mfc/ribbon-designer-mfc.md)|MFC 项目中的功能区资源。|
|[字符串编辑器](../windows/string-editor.md)|Visual Studio c + + 项目中的字符串表。|
|[工具栏编辑器](../windows/toolbar-editor.md)|Visual Studio c + + 项目中的工具栏资源。 **工具栏编辑器**是**图像编辑器**的一部分。|
|[版本信息编辑器](../windows/version-information-editor.md)|Visual Studio c + + 项目中的版本信息。|

> [!NOTE]
> 如果你的项目尚未包含 .rc 文件，请参阅 [如何：创建资源](../windows/how-to-create-a-resource-script-file.md)。

## <a name="view-and-edit-resources"></a>查看和编辑资源

每个资源类型都有一个特定于该资源类型的资源编辑器。 您可以重新排列、调整大小、添加控件和功能，或者使用关联的编辑器来修改资源的各个方面。 您还可以编辑文本格式和二进制格式的资源。 有关详细信息，请参阅 [如何：创建资源](../windows/how-to-create-a-resource-script-file.md)。

某些资源类型是可以通过各种方式导入和使用的单独文件。其中包括位图、图标、光标、工具栏和 html 文件。 此类资源具有文件名和 [资源标识符](../windows/symbols-resource-identifiers.md)。 Win32 项目中的其他类（如对话框、菜单和字符串表）仅作为资源脚本 ( 的一部分存在) 文件或资源模板 ( .rct) 文件中。

还可以在项目外编辑资源，而无需打开项目。 有关详细信息，请参阅 [如何：创建资源](./how-to-create-a-resource-script-file.md)。

> [!NOTE]
> 可以使用 " **属性** " 窗口修改资源的属性。

- 若要编辑资源的属性，请在 [资源视图](how-to-create-a-resource-script-file.md#create-resources)中，右键单击要编辑的资源，然后选择 " **属性**"。  然后，在 " [属性窗口](/visualstudio/ide/reference/properties-window)中，更改资源的属性。

- 若要撤消对资源的属性所做的更改，请确保资源在**资源视图**中具有焦点，然后从 "**编辑**" 菜单中选择 "**撤消**"。

### <a name="win32-resources"></a>Win32 资源

可以在 " [资源视图](how-to-create-a-resource-script-file.md#create-resources) " 窗格中访问 Win32 资源。

#### <a name="to-view-a-win32-resource-in-a-resource-editor"></a>在资源编辑器中查看 Win32 资源

1. "中转到" 菜单**查看**  >  **其他 Windows**  >  **资源视图**。

1. 如果 **资源视图** 窗口不是最顶层窗口，请选择 " **资源视图** " 选项卡将其置于顶部。

1. 在 **资源视图**中，展开包含要查看的资源的项目的文件夹。 例如，如果想要查看对话框资源，请展开 **对话框** 文件夹。

1. 双击资源，例如 **IDD_ABOUTBOX**。

   资源将在适当的编辑器中打开。 例如，对于对话框资源，资源在 **对话框编辑器**中打开。

#### <a name="to-delete-an-existing-win32-resource"></a>删除现有的 Win32 资源

1. 在 **资源视图**中，展开资源类型的节点。

1. 右键单击要删除的资源，然后选择 " **删除**"。

> [!TIP]
> 如果在项目外的文档窗口中打开 .rc 文件，还可以使用此方法。

### <a name="managed-project-resources"></a>托管项目资源

由于托管的项目不使用资源脚本文件，因此必须从 **解决方案资源管理器**打开资源。 使用 [图像编辑器](../windows/image-editor-for-icons.md) 和 [二进制编辑器](binary-editor.md) 处理托管项目中的资源文件。 要编辑的任何托管资源都必须是链接的资源，Visual Studio 资源编辑器不支持编辑嵌入的资源。

- 若要在资源编辑器中查看托管资源，请在 **解决方案资源管理器**中，双击资源，例如 *Bitmap1.bmp*，然后在相应的编辑器中打开资源。

- 若要删除现有的托管资源，请在 **解决方案资源管理器**中右键单击要删除的资源，然后选择 " **删除**"。

## <a name="preview-resources"></a>预览资源

预览资源，以便在不打开图形资源的情况下查看图形资源。 对可执行文件进行编译后，预览也很有用，因为资源标识符更改为数字。 由于这些数值标识符通常不提供足够的信息，因此预览资源可帮助你快速识别它们。

以下资源类型提供了可视布局预览：位图、对话框、图标、菜单、光标、工具栏

以下资源不提供视觉对象预览：加速器、清单、字符串表、版本信息

> [!NOTE]
> 若要预览资源，需要 Win32。

### <a name="to-preview-resources"></a>预览资源

1. 在 [资源视图](how-to-create-a-resource-script-file.md#create-resources) 或文档窗口中，选择您的资源，例如 **IDD_ABOUTBOX**。

1. 在 [属性窗口](/visualstudio/ide/reference/properties-window)中，选择 " **属性页** " 按钮。

   > [!TIP]
   > 使用快捷方式，请参阅菜单**视图**  >  **属性页**。

   资源的 **属性** 页将打开，并显示该资源的预览。 您可以使用 **向上** 键和 **向下** 键来导航 **资源视图** 或文档窗口中的树控件。 **属性**页将保持打开状态，并显示有焦点且可以预览的任何资源。

## <a name="requirements"></a>要求

无

## <a name="see-also"></a>请参阅

[使用资源文件](../windows/working-with-resource-files.md)<br/>
[资源文件](../windows/resource-files-visual-studio.md)<br/>
[ (符号的资源标识符) ](../windows/symbols-resource-identifiers.md)<br/>
