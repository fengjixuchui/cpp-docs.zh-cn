---
title: 如何：使用 Windows 窗体执行 DDX-DDV 数据绑定
ms.custom: get-started-article
ms.date: 11/04/2016
helpviewer_keywords:
- MFC [C++], hosting a Windows Forms Control
- Windows Forms [C++], MFC support
ms.assetid: b2957370-cf1f-4779-94ac-228cd393686c
ms.openlocfilehash: a0759eba1c55e72f2c0a99964b0b2d254df82a25
ms.sourcegitcommit: 43cee7a0d41a062661229043c2f7cbc6ace17fa3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92008315"
---
# <a name="how-to-do-ddxddv-data-binding-with-windows-forms"></a>如何：使用 Windows 窗体执行 DDX/DDV 数据绑定

[DDX_ManagedControl](../mfc/reference/standard-dialog-data-exchange-routines.md#ddx_managedcontrol) 调用 [CWinFormsControl：： CreateManagedControl](../mfc/reference/cwinformscontrol-class.md#createmanagedcontrol) 来创建一个与资源控件 ID 相匹配的控件。 如果用于 `DDX_ManagedControl` `CWinFormsControl` 控件 (在向导生成的代码) 中，则不应 `CreateManagedControl` 为同一控件显式调用。

`DDX_ManagedControl`在[CWnd：:D odataexchange](../mfc/reference/cwnd-class.md#dodataexchange)中调用，以从资源 id 创建控件。 对于数据交换，无需将 DDX/DDV 函数用于 Windows 窗体控件。 相反，你可以在对话框 (或查看) 类的方法中放置代码来访问托管控件的属性 `DoDataExchange` ，如下面的示例中所示。

下面的示例演示如何将本机 c + + 字符串绑定到 .NET 用户控件。

## <a name="example-ddxddv-data-binding"></a>示例： DDX/DDV 数据绑定

下面是一个 `m_str` 使用 .net 用户控件的用户定义属性的 MFC 字符串的 DDX/DDV 数据绑定的示例 `NameText` 。

当 [CDialog：： OnInitDialog](../mfc/reference/cdialog-class.md#oninitdialog) 第一次调用时，将创建该控件 `CMyDlg::DoDataExchange` ，因此，引用的任何代码都 `m_UserControl` 必须在 `DDX_ManagedControl` 调用之后。

你可以在 " [如何：创建用户控件并在一个对话框中承载](../dotnet/how-to-create-the-user-control-and-host-in-a-dialog-box.md)" 对话框中创建的 MFC01 应用程序中实现此代码。

将以下代码放入 CMFC01Dlg 的声明中：

```
class CMFC01Dlg : public CDialog
{
   CWinFormsControl<WindowsFormsControlLibrary1::UserControl1> m_MyControl;
   CString m_str;
};
```

## <a name="example-implement-dodataexchange"></a>示例：实现 DoDataExchange ( # A1

在 CMFC01Dlg 的实现中添加以下代码：

```cpp
void CMFC01Dlg::DoDataExchange(CDataExchange* pDX)
{
   CDialog::DoDataExchange(pDX);
   DDX_ManagedControl(pDX, IDC_CTRL1, m_MyControl);

   if (pDX->m_bSaveAndValidate) {
      m_str = m_MyControl->textBox1->Text;
   } else
   {
      m_MyControl->textBox1->Text = gcnew System::String(m_str);
   }
}
```

## <a name="example-add-handler-method"></a>示例：添加处理程序方法

现在，我们将为单击 "确定" 按钮添加处理程序方法。 单击 " **资源视图** " 选项卡。在资源视图中，双击 `IDD_MFC01_DIALOG` 。 对话框资源将出现在 "资源编辑器" 中。 然后双击 "确定" 按钮。

按如下所示定义处理程序。

```cpp
void CMFC01Dlg::OnBnClickedOk()
{
   AfxMessageBox(CString(m_MyControl.GetControl()->textBox1->Text));
   OnOK();
}
```

## <a name="example-set-the-textbox-text"></a>示例：设置文本框文本

并将以下行添加到 BOOL CMFC01Dlg：： OnInitDialog 的实现 ( # A1。

```
m_MyControl.GetControl()->textBox1->Text = "hello";
```

现在可以构建并运行应用程序。 请注意，当应用程序关闭时，文本框中的任何文本都将显示在弹出消息框中。

## <a name="see-also"></a>请参阅

[CWinFormsControl 类](../mfc/reference/cwinformscontrol-class.md)<br/>
[DDX_ManagedControl](../mfc/reference/standard-dialog-data-exchange-routines.md#ddx_managedcontrol)<br/>
[CWnd：:D oDataExchange](../mfc/reference/cwnd-class.md#dodataexchange)
