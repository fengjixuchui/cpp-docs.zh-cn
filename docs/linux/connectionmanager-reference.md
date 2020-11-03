---
title: ConnectionManager 引用
description: 如何通过命令行工具管理远程 SSH 连接。
ms.date: 10/7/2020
f1_keywords:
- ConnectionManager
helpviewer_keywords:
- ConnectionManager program
ms.openlocfilehash: 2f38fec21e7526fa214db811b00fc545504f0610
ms.sourcegitcommit: 611e903f222ec794ef14195796b332851ab98904
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/08/2020
ms.locfileid: "91847133"
---
# <a name="connectionmanager-reference"></a>ConnectionManager 引用

::: moniker range="<=vs-2017"

可以在 Visual Studio 2019 版本 16.5 及更高版本中使用 ConnectionManager.exe。

::: moniker-end

::: moniker range="vs-2019"

ConnectionManager.exe 是用于在 Visual Studio 之外管理远程开发连接的命令行实用工具。 它对于预配新开发计算机之类的任务非常有用。 你也可以使用它来设置 Visual Studio 进行持续集成。可以在“开发人员命令提示”窗口中使用它。 若要详细了解开发人员命令提示，请参阅[通过命令行使用 Microsoft C++ 工具集](../build/building-on-the-command-line.md)。

可以在 Visual Studio 2019 版本 16.5 及更高版本中使用 ConnectionManager.exe。 它属于 Visual Studio 安装程序中的“使用 C++ 的 Linux 开发”  工作负载。 它也会在你选择安装程序中的“连接管理器”  组件时自动安装。 它的安装路径为 %VCIDEInstallDir%\\Linux\\bin\\ConnectionManagerExe\\ConnectionManager.exe  。

在 Visual Studio 中，也可以使用 ConnectionManager.exe 的功能。 若要在 IDE 中管理远程开发连接，请在菜单栏上依次选择“工具” > “选项”，以打开“选项”对话框。 在“选项”对话框中，依次选择“跨平台” > “连接管理器”。

## <a name="syntax"></a>语法

> **`ConnectionManager.exe`** *command* \[*arguments* ] \[*options* ]

### <a name="commands-and-arguments"></a>命令和参数

- **`add`** *user\@host* \[ **`--port`** *port* ] \[ **`--password`** *password* ] \[ **`--privatekey`** *privatekey_file* ]

  验证身份，并添加新连接。 默认情况下，它使用端口 22 和密码身份验证。 （系统会提示你输入密码。）同时使用 -`-password` 和 `--privatekey` 指定私钥的密码 。

- **`remove`** \[*connection_id* \| *user\@host* \[ **`--port`** *port* ]]

  删除连接。 如果你没有指定任何参数，系统会提示你指定要删除哪个连接。
  
- **`modify`** \[*default* \| *connection_id* \| *user\@host* \[ **`--port`** *port* ]] \[ **`--property`** *key=value* ]

  定义或修改连接的属性。
  如果 value 为空，则删除属性 key 。
  如果身份验证失败，则不进行任何更改。
  如果未指定连接（上述的默认情况），则使用用户的默认远程连接。

- **`remove-all`**

  删除已存储的所有连接。
  
- **`clean`**

  删除不再存在的连接的标头缓存。 

- **`list`** \[**`--properties`** ]

  显示已存储的所有连接的信息、ID 和属性。 

- **`help`**

  显示帮助屏幕。

- **`version`**

  显示版本信息。

### <a name="options"></a>选项

- **`-q`** , **`--quiet`**

  防止输出到 `stdout` 或 `stderr`。

- **`--no-prompt`**

  视情况发生故障，而不是提示。

- **`--no-verify`**

  添加或修改连接，而无需进行身份验证。

- **`--file`** *filename*

  从提供的 filename  中读取连接信息。

- **`--no-telemetry`**

  禁用将用法数据发送回 Microsoft。 除非传递的是 `--no-telemetry` 标志，否则用法数据会被收集并发送回 Microsoft。  

- **`-n`** , **`--dry-run`**

  试运行命令。
 
- **`--p`**

  与 `--password` 相同。

- **`-i`**

  与 `--privatekey` 相同。

## <a name="examples"></a>示例

下面的命令在 localhost 上为名为“user”的用户添加连接。 连接使用 %USERPROFILE%\.ssh\id_rsa  中的密钥文件进行身份验证。

```cmd
ConnectionManager.exe add user@127.0.0.1 --privatekey "%USERPROFILE%\.ssh\id_rsa"
```

下面的命令从连接列表中删除 ID 为 1975957870 的连接。

```cmd
ConnectionManager.exe remove 1975957870
```

此命令将覆盖连接 ID 为 21212121 的连接的 shell。 支持的 shell 为：`sh, csh, bash, tcsh, ksh, zsh, dash`。 如果不支持在 Linux 系统上找到的 shell，则回退到对所有命令显式使用 `sh`。

```cmd
ConnectionManager.exe modify 21212121 --property shell=csh
```

## <a name="see-also"></a>另请参阅

[在 Visual Studio 中连接到你的目标 Linux 系统](connect-to-your-remote-linux-computer.md)

::: moniker-end