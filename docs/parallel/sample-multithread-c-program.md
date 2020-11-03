---
title: 多线程 C 程序示例
ms.date: 08/09/2019
ms.assetid: 4706f6cd-ff9c-4dbf-99a2-1c999b568f17
ms.openlocfilehash: 355024b995dc638b89a335983bd2e0f2cb0d96ce
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924763"
---
# <a name="sample-multithread-c-program"></a>多线程 C 程序示例

反弹是一个示例多线程程序，每次键入字母或时都会创建一个新线程 `a` `A` 。 每个线程都在屏幕上弹跳不同颜色的字母。 最多可以创建32个线程。 此程序的正常终止发生在 `q` 键入或时 `Q` 。

## <a name="compile-and-link-a-multithread-program"></a>编译和链接多线程程序

默认情况下，程序被编译为多线程。

### <a name="to-compile-and-link-the-multithread-program-bouncec-from-within-the-development-environment"></a>从开发环境中编译和链接多线程程序弹跳

::: moniker range=">=msvc-160"

1. 在“文件”菜单上，选择“新建”>“项目”。

1. 在 " **创建新项目** " 对话框中，选择包含 **c + +** 、 **Windows** 和 **控制台** 标记的 **控制台应用程序** 模板。 选择“下一步”  以继续。

1. 在 " **配置新项目** " 对话框中，输入项目的名称，例如 "弹跳"。 选择 " **创建** " 以继续。

1. 在 " **解决方案资源管理器** " 窗口中，打开项目下的 " **源文件** " 文件夹，并将源文件的名称更改为扩展名 .c。

1. 在编辑窗口中，删除现有源代码，并将其替换为示例代码。

1. 在 **“生成”** 菜单上，选择 **“生成解决方案”** 。

1. 按 **F5** 在调试器中启动程序。

::: moniker-end

::: moniker range="<=msvc-150"

1. 在“文件”菜单上，选择“新建”>“项目”。

1. 在 " **新建项目** " 对话框的左窗格中，选择 " **Visual C++** "，然后在中间窗格中选择 " **空项目** "。

1. 在 " **名称** " 编辑框中，输入项目的名称，例如 "弹跳"。 选择 **"确定"** 以创建空项目。

1. 在 " **解决方案资源管理器** " 窗口中，打开项目下的 " **源文件** " 文件夹，并将包含 C 源代码的文件添加到项目。

1. 在 " **生成** " 菜单上，通过选择 " **生成解决方案** " 命令来生成项目。

1. 按 **F5** 在调试器中启动程序。

::: moniker-end

### <a name="to-compile-and-link-the-multithread-program-bouncec-from-the-command-line"></a>从命令行编译和链接多线程程序弹跳

1. 编译和链接程序：

    ```cmd
    cl bounce.c
    ```

## <a name="example"></a>示例

若要在命令行上生成，请将此示例复制并保存到扩展名为 .c 的源文件中。 在 IDE 中，将模板创建的任何源代码替换为以下示例：

```C
// sample_multithread_c_program.c
// compile with: /c
//
//  Bounce - Creates a new thread each time the letter 'a' is typed.
//  Each thread bounces a character of a different color around
//  the screen. All threads are terminated when the letter 'Q' is
//  entered.
//

#include <windows.h>
#include <stdlib.h>
#include <string.h>
#include <stdio.h>
#include <conio.h>
#include <process.h>

#define MAX_THREADS  32

// The function getrandom returns a random number between
// min and max, which must be in integer range.
#define getrandom( min, max ) (SHORT)((rand() % (int)(((max) + 1) - \
                               (min))) + (min))

int main(void);                    // Thread 1: main
void KbdFunc(void);                // Keyboard input, thread dispatch
void BounceProc(void* MyID);       // Threads 2 to n: display
void ClearScreen(void);            // Screen clear
void ShutDown(void);               // Program shutdown
void WriteTitle(int ThreadNum);    // Display title bar information

HANDLE  hConsoleOut;                 // Handle to the console
HANDLE  hRunMutex;                   // "Keep Running" mutex
HANDLE  hScreenMutex;                // "Screen update" mutex
int     ThreadNr;                    // Number of threads started
CONSOLE_SCREEN_BUFFER_INFO csbiInfo; // Console information
COORD   consoleSize;
BOOL    bTrails;

int main() // Thread One
{
    // Get display screen information & clear the screen.
    hConsoleOut = GetStdHandle(STD_OUTPUT_HANDLE);
    GetConsoleScreenBufferInfo(hConsoleOut, &csbiInfo);
    consoleSize.X = csbiInfo.srWindow.Right;
    consoleSize.Y = csbiInfo.srWindow.Bottom;
    ClearScreen();
    WriteTitle(0);

    // Create the mutexes and reset thread count.
    hScreenMutex = CreateMutexW(NULL, FALSE, NULL);  // Cleared
    hRunMutex = CreateMutexW(NULL, TRUE, NULL);      // Set
    ThreadNr = 0;
    bTrails = FALSE;

    // Start waiting for keyboard input to dispatch threads or exit.
    KbdFunc();

    // All threads done. Clean up handles.
    if (hScreenMutex) CloseHandle(hScreenMutex);
    if (hRunMutex) CloseHandle(hRunMutex);
    if (hConsoleOut) CloseHandle(hConsoleOut);
}

void ShutDown(void) // Shut down threads
{
    while (ThreadNr > 0)
    {
        // Tell thread to die and record its death.
        ReleaseMutex(hRunMutex);
        ThreadNr--;
    }

    // Clean up display when done
    WaitForSingleObject(hScreenMutex, INFINITE);
    ClearScreen();
}

void KbdFunc(void) // Dispatch and count threads.
{
    int         KeyInfo;

    do
    {
        KeyInfo = _getch();
        if (tolower(KeyInfo) == 'a' &&
            ThreadNr < MAX_THREADS)
        {
            ThreadNr++;
            _beginthread(BounceProc, 0, &ThreadNr);
            WriteTitle(ThreadNr);
        }
        if (tolower(KeyInfo) == 't')
        {
            bTrails = !bTrails;
        }
    } while (tolower(KeyInfo) != 'q');

    ShutDown();
}

void BounceProc(void* pMyID)
{
    wchar_t MyCell, OldCell;
    WORD    MyAttrib, OldAttrib = 0;
    wchar_t BlankCell = 0x20;
    COORD   Coords, Delta;
    COORD   Old = { 0,0 };
    DWORD   Dummy;
    char* MyID = (char*)pMyID;

    // Generate update increments and initial
    // display coordinates.
    srand((unsigned int)* MyID * 3);

    Coords.X = getrandom(0, consoleSize.X - 1);
    Coords.Y = getrandom(0, consoleSize.Y - 1);
    Delta.X = getrandom(-3, 3);
    Delta.Y = getrandom(-3, 3);

    // Set up character & generate color
    // attribute from thread number.
    if (*MyID > 16)
        MyCell = 0x60 + *MyID - 16; // lower case
    else
        MyCell = 0x40 + *MyID;      // upper case
    MyAttrib = *MyID & 0x0f;   // force black background

    do
    {
        // Wait for display to be available, then lock it.
        WaitForSingleObject(hScreenMutex, INFINITE);

        if (!bTrails)
        {
            // If we still occupy the old screen position, blank it out.
            ReadConsoleOutputCharacterW(hConsoleOut, &OldCell, 1,
                Old, &Dummy);
            ReadConsoleOutputAttribute(hConsoleOut, &OldAttrib, 1,
                Old, &Dummy);
            if ((OldCell == MyCell) && (OldAttrib == MyAttrib))
                WriteConsoleOutputCharacterW(hConsoleOut, &BlankCell, 1,
                    Old, &Dummy);
        }

        // Draw new character, then clear screen lock
        WriteConsoleOutputCharacterW(hConsoleOut, &MyCell, 1,
            Coords, &Dummy);
        WriteConsoleOutputAttribute(hConsoleOut, &MyAttrib, 1,
            Coords, &Dummy);
        ReleaseMutex(hScreenMutex);

        // Increment the coordinates for next placement of the block.
        Old.X = Coords.X;
        Old.Y = Coords.Y;
        Coords.X += Delta.X;
        Coords.Y += Delta.Y;

        // If we are about to go off the screen, reverse direction
        if (Coords.X < 0 || Coords.X >= consoleSize.X)
        {
            Delta.X = -Delta.X;
            Beep(400, 50);
        }
        if (Coords.Y < 0 || Coords.Y > consoleSize.Y)
        {
            Delta.Y = -Delta.Y;
            Beep(600, 50);
        }
    }
    // Repeat while RunMutex is still taken.
    while (WaitForSingleObject(hRunMutex, 75L) == WAIT_TIMEOUT);
}

void WriteTitle(int ThreadNum)
{
    enum
    {
        sizeOfNThreadMsg = 120
    };
    wchar_t    NThreadMsg[sizeOfNThreadMsg] = { L"" };

    swprintf_s(NThreadMsg, sizeOfNThreadMsg,
        L"Threads running: %02d.  Press 'A' "
        L"to start a thread, 'T' to toggle "
        L"trails, 'Q' to quit.", ThreadNum);
    SetConsoleTitleW(NThreadMsg);
}

void ClearScreen(void)
{
    DWORD    dummy = 0;
    COORD    Home = { 0, 0 };
    FillConsoleOutputCharacterW(hConsoleOut, L' ',
        consoleSize.X * consoleSize.Y,
        Home, &dummy);
}
```

## <a name="see-also"></a>请参阅

[使用 C 和 Win32 进行多线程编程](multithreading-with-c-and-win32.md)
