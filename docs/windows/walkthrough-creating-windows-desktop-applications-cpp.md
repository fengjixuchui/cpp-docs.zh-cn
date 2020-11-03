---
title: '演练：创建传统的 Windows 桌面应用程序 (c + +) '
description: 如何使用 Visual Studio、c + + 和 Win32 API 创建最小的传统 Windows 桌面应用程序
ms.custom: get-started-article
ms.date: 05/28/2020
helpviewer_keywords:
- Windows applications [C++], Win32
- Windows Desktop applications [C++]
- Windows API [C++]
ms.openlocfilehash: 36991cf98867e7da218f7414d1ea02aab55301a3
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924238"
---
# <a name="walkthrough-create-a-traditional-windows-desktop-application-c"></a>演练：创建传统的 Windows 桌面应用程序 (c + +) 

本演练演示如何在 Visual Studio 中创建传统的 Windows 桌面应用程序。 要创建的示例应用程序使用 Windows API 显示 "Hello，Windows desktop！" 应用程序。 可以将本演练中开发的代码作为模式来创建其他 Windows 桌面应用程序。

Windows API (也称为 Win32 API、Windows 桌面 API 和 Windows Classic API) 是基于 C 语言的框架，用于创建 Windows 应用程序。 它已存在，因为它已被占用了20% 的时间，用于创建数十年的 Windows 应用程序。 更高级、更易于编程的框架是在 Windows API 之上构建的。 例如，MFC，ATL，.NET framework。 甚至使用 c + +/WinRT 编写的 UWP 和应用商店应用的最新式 Windows 运行时代码也使用下面的 Windows API。 有关 Windows API 的详细信息，请参阅 [WINDOWS Api Index](/windows/win32/apiindex/windows-api-list)。 有多种方法可以创建 Windows 应用程序，但上面的过程是第一个。

> [!IMPORTANT]
> 为了简洁起见，文本中省略了一些代码语句。 本文档末尾的 " [生成代码"](#build-the-code) 部分显示了完整的代码。

## <a name="prerequisites"></a>先决条件

- 运行 Microsoft Windows 7 或更高版本的计算机。 建议使用 Windows 10 以实现最佳开发体验。

- Visual Studio 的副本。 有关如何下载和安装 Visual Studio 的信息，请参阅[安装 Visual Studio](/visualstudio/install/install-visual-studio)。 运行安装程序时，请务必选中“使用 C++ 的桌面开发”  工作负载。 如果在安装 Visual Studio 时未安装此工作负载，请不要担心。 可以再次运行安装程序并立即安装。

   使用 C++ 的桌面开发![](../build/media/desktop-development-with-cpp.png "使用 C++ 的桌面开发")

- 了解使用 Visual Studio IDE 的基础知识。 如果你之前使用过 Windows 桌面应用，可能具备一定的相关知识。 有关简介，请参阅 [Visual Studio IDE 功能导览](/visualstudio/ide/visual-studio-ide)。

- 了解足够的 C++ 语言基础知识以供继续操作。 别担心，我们不会执行过于复杂的操作。

## <a name="create-a-windows-desktop-project"></a>创建 Windows 桌面项目

按照以下步骤创建您的第一个 Windows 桌面项目。 当你执行此操作时，你将为正在运行的 Windows 桌面应用程序输入代码。 若要查看 Visual Studio 首选项的文档，请使用“版本”选择器控件。 它位于此页面上目录表的顶部。

::: moniker range="msvc-160"

### <a name="to-create-a-windows-desktop-project-in-visual-studio-2019"></a>在 Visual Studio 2019 中创建 Windows 桌面项目

1. 在主菜单中，依次选择“文件”>“新建”>“项目”，以打开“新建项目”对话框。

1. 在对话框顶部，将 " **语言** " 设置为 " **c + +** "，将 " **平台** " 设置为 " **Windows** "，将 " **项目类型** " 设置为 " **桌面**

1. 从筛选的项目类型列表中，选择 " **Windows 桌面向导** "，然后选择 " **下一步** "。 在下一页中，输入项目的名称，例如 " *DesktopApp* "。

1. 选择“创建”  按钮创建项目。

1. 此时将显示 " **Windows 桌面项目** " 对话框。 在 " **应用程序类型** " 下，选择 " **桌面应用程序 ( .exe)** 。 在“附加选项”  下，选择“空项目”  。 选择“确定”，创建项目  。

1. 在 **解决方案资源管理器** 中，右键单击 **DesktopApp** 项目，选择 " **添加** "，然后选择 " **新建项** "。

   ![显示用户在 Visual Studio 2019 中将新项添加到 DesktopApp 项目的简短视频。](../build/media/desktop-app-project-add-new-item-153.gif "向 DesktopApp 项目添加新项")

1. 在“添加新项”  对话框中选择“C++ 文件(.cpp)”  。 在 " **名称** " 框中，键入文件的名称，例如 " *HelloWindowsDesktop* "。 选择“添加”。

   ![Visual Studio 2019 中的 "添加新项" 对话框的屏幕截图，其中已安装 > Visual C + + plus，并且突出显示了 C + + 文件选项。](../build/media/desktop-app-add-cpp-file-153.png "将 .cpp 文件添加到 DesktopApp 项目")

此时会创建项目，并在编辑器中打开源文件。 若要继续，请跳到 [创建代码](#create-the-code)。

::: moniker-end

::: moniker range="msvc-150"

### <a name="to-create-a-windows-desktop-project-in-visual-studio-2017"></a>在 Visual Studio 2017 中创建 Windows 桌面项目

1. 在“文件”菜单上选择“新建”，再选择“项目”。 

1. 在 " **新建项目** " 对话框的左窗格中，展开 " **已安装**  >  " **Visual C++** ，然后选择 " **Windows 桌面** "。 在中间窗格中，选择 " **Windows 桌面向导** "。

   在 " **名称** " 框中，键入项目的名称，例如 " *DesktopApp* "。 选择 **“确定”** 。

   ![Visual Studio 2017 中的 "新建项目" 对话框的屏幕截图，其中安装的 > Visual C + + > 选中 "windows 桌面向导" 选项，突出显示了 Windows 桌面向导选项，并在 "名称" 文本框中键入 DesktopApp。](../build/media/desktop-app-new-project-name-153.png "为 DesktopApp 项目命名")

1. 在 " **Windows 桌面项目** " 对话框中的 " **应用程序类型** " 下，选择 " **Windows 应用程序 ( .exe)** 。 在“附加选项”  下，选择“空项目”  。 请确保未选择 " **预编译头** "。 选择“确定”，创建项目  。

1. 在 **解决方案资源管理器** 中，右键单击 **DesktopApp** 项目，选择 " **添加** "，然后选择 " **新建项** "。

   ![显示用户在 Visual Studio 2017 中将新项添加到 DesktopApp 项目的简短视频。](../build/media/desktop-app-project-add-new-item-153.gif "向 DesktopApp 项目添加新项")

1. 在“添加新项”  对话框中选择“C++ 文件(.cpp)”  。 在 " **名称** " 框中，键入文件的名称，例如 " *HelloWindowsDesktop* "。 选择“添加”。

   ![Visual Studio 2017 中的 "添加新项" 对话框的屏幕截图，其中已安装 > Visual C + + plus，并且突出显示了 C + + 文件选项。](../build/media/desktop-app-add-cpp-file-153.png "将 .cpp 文件添加到 DesktopApp 项目")

此时会创建项目，并在编辑器中打开源文件。 若要继续，请跳到 [创建代码](#create-the-code)。

::: moniker-end

::: moniker range="msvc-140"

### <a name="to-create-a-windows-desktop-project-in-visual-studio-2015"></a>在 Visual Studio 2015 中创建 Windows 桌面项目

1. 在“文件”菜单上选择“新建”，再选择“项目”。 

1. 在 " **新建项目** " 对话框的左窗格中，展开 " **已安装**  >  的 **模板** "  >  **Visual C++** ，然后选择 " **Win32** "。 在中间窗格中，选择“Win32 项目”  。

   在 " **名称** " 框中，键入项目的名称，例如 " *DesktopApp* "。 选择 **“确定”** 。

   ![Visual Studio 2015 中的 "新建项目" 对话框的屏幕截图，其中包含已安装的 > 模板 > Visual C + + 所选 > Win32，"Win32 项目" 选项突出显示，并在 "名称" 文本框中键入 DesktopApp。](../build/media/desktop-app-new-project-name-150.png "为 DesktopApp 项目命名")

1. 在 " **Win32 应用程序向导** " 的 " **概述** " 页上，选择 " **下一步** "。

   ![在 Win32 应用程序向导中创建 DesktopApp 概述](../build/media/desktop-app-win32-wizard-overview-150.png "在 Win32 应用程序向导中创建 DesktopApp 概述")

1. 在 " **应用程序设置** " 页的 " **应用程序类型** " 下，选择 " **Windows 应用程序** "。 在 **其他选项** 下，取消选中 " **预编译头** "，然后选择 " **空项目** "。 选择“完成”以创建项目  。

1. 在 **解决方案资源管理器** 中，右键单击 DesktopApp 项目，选择 " **添加** "，然后选择 " **新建项** "。

   ![显示用户在 Visual Studio 2015 中将新项添加到 DesktopApp 项目的简短视频。](../build/media/desktop-app-project-add-new-item-150.gif "向 DesktopApp 项目添加新项")

1. 在“添加新项”  对话框中选择“C++ 文件(.cpp)”  。 在 " **名称** " 框中，键入文件的名称，例如 " *HelloWindowsDesktop* "。 选择“添加”。

   ![Visual Studio 2015 中的 "添加新项" 对话框的屏幕截图，其中已安装 > Visual C + + plus，并且突出显示了 C + + 文件选项。](../build/media/desktop-app-add-cpp-file-150.png "将 .cpp 文件添加到 DesktopApp 项目")

此时会创建项目，并在编辑器中打开源文件。

::: moniker-end

## <a name="create-the-code"></a>创建代码

接下来，你将学习如何在 Visual Studio 中为 Windows 桌面应用程序创建代码。

### <a name="to-start-a-windows-desktop-application"></a>启动 Windows 桌面应用程序

1. 正如每个 C 应用程序和 c + + 应用程序必须将 `main` 函数作为其起始点一样，每个 Windows 桌面应用程序都必须具有一个 `WinMain` 函数。 `WinMain` 具有以下语法。

   ```cpp
   int CALLBACK WinMain(
      _In_ HINSTANCE hInstance,
      _In_opt_ HINSTANCE hPrevInstance,
      _In_ LPSTR     lpCmdLine,
      _In_ int       nCmdShow
   );
   ```

   有关此函数的参数和返回值的信息，请参阅 [WinMain 入口点](/windows/win32/api/winbase/nf-winbase-winmain)。

   > [!NOTE]
   > 所有这些额外的词，例如 `CALLBACK` 、或 `HINSTANCE` `_In_` 。 传统的 Windows API 广泛使用 typedef 和预处理器宏来抽象掉某些类型的详细信息和特定于平台的代码，例如调用约定、 **`__declspec`** 声明和编译器杂注。 在 Visual Studio 中，可以使用 IntelliSense [快速信息](/visualstudio/ide/using-intellisense#quick-info) 功能来查看这些 typedef 和宏定义的内容。 将鼠标悬停在感兴趣的字词上，或选择它， **然后按 ctrl** + **K** ， **ctrl** + **I** 获取包含定义的小的弹出窗口。 有关详细信息，请参阅[使用 IntelliSense](/visualstudio/ide/using-intellisense)。 参数和返回类型通常使用 *SAL 批注* 来帮助您捕获编程错误。 有关详细信息，请参阅 [使用 SAL 注释减少 C/c + + 代码缺陷](../code-quality/using-sal-annotations-to-reduce-c-cpp-code-defects.md)。

1. Windows 桌面程序需要 &lt;>。 &lt;tchar> 定义 `TCHAR` 宏， **`wchar_t`** 如果在项目中定义了 UNICODE 符号，则该宏将最终解析为，否则它将解析为 **`char`** 。  如果你始终启用了 UNICODE 生成，则无需 TCHAR，只需直接使用即可 **`wchar_t`** 。

   ```cpp
   #include <windows.h>
   #include <tchar.h>
   ```

1. 除了 `WinMain` 函数，每个 Windows 桌面应用程序还必须具有一个窗口过程函数。 此函数通常名为 `WndProc` ，但你可以将其命名为你喜欢的任何名称。 `WndProc` 具有以下语法。

   ```cpp
   LRESULT CALLBACK WndProc(
      _In_ HWND   hWnd,
      _In_ UINT   message,
      _In_ WPARAM wParam,
      _In_ LPARAM lParam
   );
   ```

   在此函数中，你将编写代码来处理应用程序在发生 *事件* 时从 Windows 接收的 *消息* 。 例如，如果用户在您的应用程序中选择了 "确定" 按钮，Windows 将向您发送一条消息，您可以在函数内编写 `WndProc` 执行任何适当工作的代码。 这称为 *处理* 事件。 仅处理与应用程序相关的事件。

   有关详细信息，请参阅 [窗口过程](/windows/win32/winmsg/window-procedures)。

### <a name="to-add-functionality-to-the-winmain-function"></a>向 WinMain 函数添加功能

1. 在 `WinMain` 函数中，您将填充类型为 [WNDCLASSEX](/windows/win32/api/winuser/ns-winuser-wndclassexw)的结构。 结构包含有关窗口的信息：应用程序图标、窗口的背景色、在标题栏中显示的名称，等等。 重要的是，它包含指向您的窗口过程的函数指针。 下面的示例演示一个典型 `WNDCLASSEX` 结构。

   ```cpp
   WNDCLASSEX wcex;

   wcex.cbSize         = sizeof(WNDCLASSEX);
   wcex.style          = CS_HREDRAW | CS_VREDRAW;
   wcex.lpfnWndProc    = WndProc;
   wcex.cbClsExtra     = 0;
   wcex.cbWndExtra     = 0;
   wcex.hInstance      = hInstance;
   wcex.hIcon          = LoadIcon(hInstance, IDI_APPLICATION);
   wcex.hCursor        = LoadCursor(NULL, IDC_ARROW);
   wcex.hbrBackground  = (HBRUSH)(COLOR_WINDOW+1);
   wcex.lpszMenuName   = NULL;
   wcex.lpszClassName  = szWindowClass;
   wcex.hIconSm        = LoadIcon(wcex.hInstance, IDI_APPLICATION);
   ```

   有关上述结构的字段的信息，请参阅 [WNDCLASSEX](/windows/win32/api/winuser/ns-winuser-wndclassexw)。

1. 向 Windows 注册， `WNDCLASSEX` 使其了解你的窗口以及如何向其发送消息。 使用 [RegisterClassEx](/windows/win32/api/winuser/nf-winuser-registerclassexw) 函数，并将窗口类结构作为参数传递。 使用 `_T` 宏是因为我们使用的是 `TCHAR` 类型。

   ```cpp
   if (!RegisterClassEx(&wcex))
   {
      MessageBox(NULL,
         _T("Call to RegisterClassEx failed!"),
         _T("Windows Desktop Guided Tour"),
         NULL);

      return 1;
   }
   ```

1. 现在可以创建窗口了。 使用 [CreateWindow](/windows/win32/api/winuser/nf-winuser-createwindoww) 函数。

   ```cpp
   static TCHAR szWindowClass[] = _T("DesktopApp");
   static TCHAR szTitle[] = _T("Windows Desktop Guided Tour Application");

   // The parameters to CreateWindow explained:
   // szWindowClass: the name of the application
   // szTitle: the text that appears in the title bar
   // WS_OVERLAPPEDWINDOW: the type of window to create
   // CW_USEDEFAULT, CW_USEDEFAULT: initial position (x, y)
   // 500, 100: initial size (width, length)
   // NULL: the parent of this window
   // NULL: this application does not have a menu bar
   // hInstance: the first parameter from WinMain
   // NULL: not used in this application
   HWND hWnd = CreateWindow(
      szWindowClass,
      szTitle,
      WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, CW_USEDEFAULT,
      500, 100,
      NULL,
      NULL,
      hInstance,
      NULL
   );
   if (!hWnd)
   {
      MessageBox(NULL,
         _T("Call to CreateWindow failed!"),
         _T("Windows Desktop Guided Tour"),
         NULL);

      return 1;
   }
   ```

   此函数返回一个 `HWND` ，它是窗口的句柄。 句柄有些类似于 Windows 用来跟踪打开的窗口的指针。 有关详细信息，请参阅 [Windows 数据类型](/windows/win32/WinProg/windows-data-types)。

1. 此时，已创建了窗口，但我们仍需要告知 Windows 使其可见。 这就是此代码的作用：

   ```cpp
   // The parameters to ShowWindow explained:
   // hWnd: the value returned from CreateWindow
   // nCmdShow: the fourth parameter from WinMain
   ShowWindow(hWnd,
      nCmdShow);
   UpdateWindow(hWnd);
   ```

   显示的窗口不包含太多内容，因为您尚未实现 `WndProc` 函数。 换句话说，应用程序尚未处理 Windows 现在正在向其发送的消息。

1. 为了处理这些消息，我们首先添加一个消息循环来侦听 Windows 发送的消息。 当应用程序收到消息时，此循环会将它调度到要 `WndProc` 处理的函数。 该消息循环类似于以下代码。

   ```cpp
   MSG msg;
   while (GetMessage(&msg, NULL, 0, 0))
   {
      TranslateMessage(&msg);
      DispatchMessage(&msg);
   }

   return (int) msg.wParam;
   ```

   有关消息循环中的结构和函数的详细信息，请参阅 [MSG](/windows/win32/api/winuser/ns-winuser-msg)、 [GetMessage](/windows/win32/api/winuser/nf-winuser-getmessage)、 [TranslateMessage](/windows/win32/api/winuser/nf-winuser-translatemessage)和 [DispatchMessage](/windows/win32/api/winuser/nf-winuser-dispatchmessage)。

   此时， `WinMain` 函数类似于以下代码。

   ```cpp
   int WINAPI WinMain(HINSTANCE hInstance,
                      HINSTANCE hPrevInstance,
                      LPSTR lpCmdLine,
                      int nCmdShow)
   {
      WNDCLASSEX wcex;

      wcex.cbSize = sizeof(WNDCLASSEX);
      wcex.style          = CS_HREDRAW | CS_VREDRAW;
      wcex.lpfnWndProc    = WndProc;
      wcex.cbClsExtra     = 0;
      wcex.cbWndExtra     = 0;
      wcex.hInstance      = hInstance;
      wcex.hIcon          = LoadIcon(hInstance, IDI_APPLICATION);
      wcex.hCursor        = LoadCursor(NULL, IDC_ARROW);
      wcex.hbrBackground  = (HBRUSH)(COLOR_WINDOW+1);
      wcex.lpszMenuName   = NULL;
      wcex.lpszClassName  = szWindowClass;
      wcex.hIconSm        = LoadIcon(wcex.hInstance, IDI_APPLICATION);

      if (!RegisterClassEx(&wcex))
      {
         MessageBox(NULL,
            _T("Call to RegisterClassEx failed!"),
            _T("Windows Desktop Guided Tour"),
            NULL);

         return 1;
      }

      // Store instance handle in our global variable
      hInst = hInstance;

      // The parameters to CreateWindow explained:
      // szWindowClass: the name of the application
      // szTitle: the text that appears in the title bar
      // WS_OVERLAPPEDWINDOW: the type of window to create
      // CW_USEDEFAULT, CW_USEDEFAULT: initial position (x, y)
      // 500, 100: initial size (width, length)
      // NULL: the parent of this window
      // NULL: this application dows not have a menu bar
      // hInstance: the first parameter from WinMain
      // NULL: not used in this application
      HWND hWnd = CreateWindow(
         szWindowClass,
         szTitle,
         WS_OVERLAPPEDWINDOW,
         CW_USEDEFAULT, CW_USEDEFAULT,
         500, 100,
         NULL,
         NULL,
         hInstance,
         NULL
      );

      if (!hWnd)
      {
         MessageBox(NULL,
            _T("Call to CreateWindow failed!"),
            _T("Windows Desktop Guided Tour"),
            NULL);

         return 1;
      }

      // The parameters to ShowWindow explained:
      // hWnd: the value returned from CreateWindow
      // nCmdShow: the fourth parameter from WinMain
      ShowWindow(hWnd,
         nCmdShow);
      UpdateWindow(hWnd);

      // Main message loop:
      MSG msg;
      while (GetMessage(&msg, NULL, 0, 0))
      {
         TranslateMessage(&msg);
         DispatchMessage(&msg);
      }

      return (int) msg.wParam;
   }
   ```

### <a name="to-add-functionality-to-the-wndproc-function"></a>向 WndProc 函数添加功能

1. 若要启用 `WndProc` 函数以处理应用程序收到的消息，请实现 switch 语句。

   要处理的一项重要消息是 [WM_PAINT](/windows/win32/gdi/wm-paint) 消息。 `WM_PAINT`当必须更新其显示窗口的一部分时，应用程序将接收该消息。 如果用户将窗口移到窗口的前面，则会发生此事件，然后再将其移开。 您的应用程序不知道这些事件发生的时间。 只有 Windows 知道，因此它将使用消息通知您的应用程序 `WM_PAINT` 。 第一次显示窗口时，必须对其进行更新。

   要处理 `WM_PAINT` 消息，首先应调用 [BeginPaint](/windows/win32/api/winuser/nf-winuser-beginpaint)，然后处理所有的逻辑以在窗口中布局文本、按钮和其他控件，然后调用 [EndPaint](/windows/win32/api/winuser/nf-winuser-endpaint)。 对于应用程序，开始调用和结束调用之间的逻辑显示字符串 "Hello，Windows desktop！" “Hello，World!”。 在下面的代码中， [TextOut](/windows/win32/api/wingdi/nf-wingdi-textoutw) 函数用于显示字符串。

   ```cpp
   PAINTSTRUCT ps;
   HDC hdc;
   TCHAR greeting[] = _T("Hello, Windows desktop!");

   switch (message)
   {
   case WM_PAINT:
      hdc = BeginPaint(hWnd, &ps);

      // Here your application is laid out.
      // For this introduction, we just print out "Hello, Windows desktop!"
      // in the top left corner.
      TextOut(hdc,
         5, 5,
         greeting, _tcslen(greeting));
      // End application-specific layout section.

      EndPaint(hWnd, &ps);
      break;
   }
   ```

   `HDC` 在代码中，是用于在窗口的工作区中进行绘制的设备上下文的句柄。 使用 `BeginPaint` 和 `EndPaint` 函数来准备并完成工作区中的绘图。 `BeginPaint` 返回用于在客户端区域中绘制的显示设备上下文的句柄; `EndPaint` 结束绘制请求并释放设备上下文。

1. 应用程序通常会处理许多其他消息。 例如，在首次创建窗口时 [WM_CREATE](/windows/win32/winmsg/wm-create) ，当窗口关闭时 [WM_DESTROY](/windows/win32/winmsg/wm-destroy) 。 以下代码显示基本但完整的 `WndProc` 函数。

   ```cpp
   LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
   {
      PAINTSTRUCT ps;
      HDC hdc;
      TCHAR greeting[] = _T("Hello, Windows desktop!");

      switch (message)
      {
      case WM_PAINT:
         hdc = BeginPaint(hWnd, &ps);

         // Here your application is laid out.
         // For this introduction, we just print out "Hello, Windows desktop!"
         // in the top left corner.
         TextOut(hdc,
            5, 5,
            greeting, _tcslen(greeting));
         // End application specific layout section.

         EndPaint(hWnd, &ps);
         break;
      case WM_DESTROY:
         PostQuitMessage(0);
         break;
      default:
         return DefWindowProc(hWnd, message, wParam, lParam);
         break;
      }

      return 0;
   }
   ```

## <a name="build-the-code"></a>生成代码

正如我们所承诺的，以下是适用于工作应用程序的完整代码。

### <a name="to-build-this-example"></a>生成此示例

1. 在编辑器中删除在 *HelloWindowsDesktop* 中输入的任何代码。 复制此示例代码，然后将其粘贴到 *HelloWindowsDesktop* 中：

   ```cpp
   // HelloWindowsDesktop.cpp
   // compile with: /D_UNICODE /DUNICODE /DWIN32 /D_WINDOWS /c

   #include <windows.h>
   #include <stdlib.h>
   #include <string.h>
   #include <tchar.h>

   // Global variables

   // The main window class name.
   static TCHAR szWindowClass[] = _T("DesktopApp");

   // The string that appears in the application's title bar.
   static TCHAR szTitle[] = _T("Windows Desktop Guided Tour Application");

   HINSTANCE hInst;

   // Forward declarations of functions included in this code module:
   LRESULT CALLBACK WndProc(HWND, UINT, WPARAM, LPARAM);

   int CALLBACK WinMain(
      _In_ HINSTANCE hInstance,
      _In_opt_ HINSTANCE hPrevInstance,
      _In_ LPSTR     lpCmdLine,
      _In_ int       nCmdShow
   )
   {
      WNDCLASSEX wcex;

      wcex.cbSize = sizeof(WNDCLASSEX);
      wcex.style          = CS_HREDRAW | CS_VREDRAW;
      wcex.lpfnWndProc    = WndProc;
      wcex.cbClsExtra     = 0;
      wcex.cbWndExtra     = 0;
      wcex.hInstance      = hInstance;
      wcex.hIcon          = LoadIcon(hInstance, IDI_APPLICATION);
      wcex.hCursor        = LoadCursor(NULL, IDC_ARROW);
      wcex.hbrBackground  = (HBRUSH)(COLOR_WINDOW+1);
      wcex.lpszMenuName   = NULL;
      wcex.lpszClassName  = szWindowClass;
      wcex.hIconSm        = LoadIcon(wcex.hInstance, IDI_APPLICATION);

      if (!RegisterClassEx(&wcex))
      {
         MessageBox(NULL,
            _T("Call to RegisterClassEx failed!"),
            _T("Windows Desktop Guided Tour"),
            NULL);

         return 1;
      }

      // Store instance handle in our global variable
      hInst = hInstance;

      // The parameters to CreateWindow explained:
      // szWindowClass: the name of the application
      // szTitle: the text that appears in the title bar
      // WS_OVERLAPPEDWINDOW: the type of window to create
      // CW_USEDEFAULT, CW_USEDEFAULT: initial position (x, y)
      // 500, 100: initial size (width, length)
      // NULL: the parent of this window
      // NULL: this application does not have a menu bar
      // hInstance: the first parameter from WinMain
      // NULL: not used in this application
      HWND hWnd = CreateWindow(
         szWindowClass,
         szTitle,
         WS_OVERLAPPEDWINDOW,
         CW_USEDEFAULT, CW_USEDEFAULT,
         500, 100,
         NULL,
         NULL,
         hInstance,
         NULL
      );

      if (!hWnd)
      {
         MessageBox(NULL,
            _T("Call to CreateWindow failed!"),
            _T("Windows Desktop Guided Tour"),
            NULL);

         return 1;
      }

      // The parameters to ShowWindow explained:
      // hWnd: the value returned from CreateWindow
      // nCmdShow: the fourth parameter from WinMain
      ShowWindow(hWnd,
         nCmdShow);
      UpdateWindow(hWnd);

      // Main message loop:
      MSG msg;
      while (GetMessage(&msg, NULL, 0, 0))
      {
         TranslateMessage(&msg);
         DispatchMessage(&msg);
      }

      return (int) msg.wParam;
   }

   //  FUNCTION: WndProc(HWND, UINT, WPARAM, LPARAM)
   //
   //  PURPOSE:  Processes messages for the main window.
   //
   //  WM_PAINT    - Paint the main window
   //  WM_DESTROY  - post a quit message and return
   LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
   {
      PAINTSTRUCT ps;
      HDC hdc;
      TCHAR greeting[] = _T("Hello, Windows desktop!");

      switch (message)
      {
      case WM_PAINT:
         hdc = BeginPaint(hWnd, &ps);

         // Here your application is laid out.
         // For this introduction, we just print out "Hello, Windows desktop!"
         // in the top left corner.
         TextOut(hdc,
            5, 5,
            greeting, _tcslen(greeting));
         // End application-specific layout section.

         EndPaint(hWnd, &ps);
         break;
      case WM_DESTROY:
         PostQuitMessage(0);
         break;
      default:
         return DefWindowProc(hWnd, message, wParam, lParam);
         break;
      }

      return 0;
   }
   ```

1. 在 **“生成”** 菜单上，选择 **“生成解决方案”** 。 编译结果应显示在 Visual Studio 的 " **输出** " 窗口中。

   ![生成 DesktopApp 项目](../build/media/desktop-app-project-build-150.gif "生成 DesktopApp 项目")

1. 若要运行应用程序，请按 **F5** 。 包含文本 "Hello，Windows desktop！" 的窗口 文字的窗口。

   ![运行 DesktopApp 项目](../build/media/desktop-app-project-run-157.PNG "运行 DesktopApp 项目")

恭喜！ 您已经完成了本演练，并构建了传统的 Windows 桌面应用程序。

## <a name="see-also"></a>请参阅

[Windows 桌面应用程序](./desktop-applications-visual-cpp.md)
