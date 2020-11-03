---
title: 添加类
ms.date: 05/14/2019
f1_keywords:
- vc.addclass
helpviewer_keywords:
- ATL projects, adding classes
- classes [C++], creating
- classes [C++], adding
- Add Class dialog box
ms.assetid: c34b5f70-4e72-4faa-ba21-e2b05361c4d9
ms.openlocfilehash: b1c64505a63b8720ed7aee855f2bbbbdb9134e28
ms.sourcegitcommit: a1676bf6caae05ecd698f26ed80c08828722b237
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2020
ms.locfileid: "91505981"
---
# <a name="add-a-class"></a>添加类

若要在 Visual Studio C++ 项目中添加类，请在“解决方案资源管理器”中，右键单击该项目，然后依次选择“添加”和“类”************。 这将打开[“添加类”对话框](#add-class-dialog-box)。

添加类时，指定的名称必须与 MFC 或 ATL 中已有的类的名称不同。 如果指定了 MFC 或 ATL 库中已存在的名称，IDE 将显示一条错误消息。

如果项目命名约定要求使用现有名称，则只需更改名称中一个或多个字母的大小写，因为 C++ 区分大小写。 例如，无法将类命名为 `CDocument`，但可将其命名为 `cdocument`。

## <a name="in-this-section"></a>在此部分中

- [要添加哪种类型的类？](#what-kind-of-class-do-you-want-to-add)
- [“添加类”对话框](#add-class-dialog-box)

## <a name="what-kind-of-class-do-you-want-to-add"></a>要添加哪种类型的类？

在“添加类”对话框中，展开左窗格中的“Visual C++”节点时，将显示几个组，列出已安装的模板********。 这些组包括 CLR、ATL、MFC 和 C++****************。 选择组时，中间的窗格将显示该组中可用模板的列表。 每个模板都包含类所需的文件和源代码。

要生成新类，请在中间的窗格选择一个模板，在“名称”框中键入类的名称，然后选择“添加”********。 这会打开“添加类向导”，可在此指定该类的选项****。

- 有关如何创建 MFC 类的详细信息，请参阅 [MFC 类](../mfc/reference/adding-an-mfc-class.md)。

- 有关如何创建 ATL 类的详细信息，请参阅 [ATL 简单对象](../atl/reference/adding-an-atl-simple-object.md)。

> [!NOTE]
> “将 ATL 支持添加到 MFC”模板不会创建类，而是将项目配置为使用 ATL****。 有关详细信息，请参阅 [MFC 项目中的 ATL 支持](../mfc/reference/adding-atl-support-to-your-mfc-project.md)。

要创建不使用 MFC、ATL 和 CLR 的 C++ 类，请使用已安装模板的“C++”组中的“C++ 类”模板********。 有关详细信息，请参阅[添加一般 C++ 类](../ide/adding-a-generic-cpp-class.md)。

有两种基于表单的 C++ 类可用。 第一种是 [CFormView 类](../mfc/reference/cformview-class.md)，可创建 MFC 类。 第二种创建 CLR Windows 窗体类。

## <a name="add-class-dialog-box"></a>“添加类”对话框

“添加类” **** 对话框包含的模板允许你：

- 打开相应的代码向导（如果有可用的向导）。 有关详细信息，请参阅[使用代码向导添加功能](../ide/adding-functionality-with-code-wizards-cpp.md)。

   \- 或 -

- 通过向项目中添加相应的文件和源代码，自动创建新类。

可以从“项目” **** 菜单、“解决方案资源管理器” **** 或 [类视图](/visualstudio/ide/viewing-the-structure-of-code)访问“添加类” **** 对话框。

> [!NOTE]
> 如果尝试将不适合的类添加到当前项目，则会收到错误信息。 选择“确定”返回到“添加类”对话框********。

### <a name="add-class-templates"></a>添加类模板

有四种类别的“添加类” **** 模板：.NET、ATL、MFC 和泛型。 当在“模板” **** 窗格中选择模板时，描述所选内容的文本将出现在“类别” **** 和“模板” **** 窗格之下。

#### <a name="net"></a>.NET

|模板|向导|
|--------------|------------|
|ASP.NET Web 服务|不可用|
|组件类 (.NET)|不可用|
|安装程序类 (.NET)|不可用|
|用户控件 (.NET)|不可用|
|Windows 窗体 (.NET)|不可用|

#### <a name="atl"></a>ATL

|模板|向导|
|--------------|------------|
|向 MFC 添加 ATL 支持|不可用|
|ATL 控件|[ATL 控件向导](../atl/reference/atl-control-wizard.md)|
|ATL 对话框|[ATL 对话框向导](../atl/reference/atl-dialog-wizard.md)|
|ATL 简单对象|[ATL 简单对象向导](../atl/reference/atl-simple-object-wizard.md)|
|WMI 事件提供程序|WMI 事件提供程序向导|
|WMI 实例提供程序|WMI 实例提供程序向导|

#### <a name="mfc"></a>MFC

|模板|向导|
|--------------|------------|
|MFC 类|[MFC 添加类向导](../mfc/reference/mfc-add-class-wizard.md)|

#### <a name="generic-classes"></a>一般类

|模板|向导|
|--------------|------------|
|泛型 C++ 类|[一般 C++ 类向导](./adding-a-generic-cpp-class.md#generic-c-class-wizard)|
