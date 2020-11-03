---
title: 属性页 XML 规则文件
description: Visual Studio IDE c + + 属性页 XML 规则文件和内容的说明。
ms.date: 10/14/2020
helpviewer_keywords:
- property page XML files
ms.openlocfilehash: f8aa893fa2b062da2f1d0784e5a9b1a6f2b30c95
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92921394"
---
# <a name="property-page-xml-rule-files"></a>属性页 XML 规则文件

IDE 中的项目属性页是通过默认规则文件夹中的 XML 文件配置的。 XML 文件描述规则的名称、类别和各个属性、数据类型、默认值以及显示方式。 在 IDE 中设置属性时，新值存储在项目文件中。

::: moniker range="msvc-140"

默认规则文件夹的路径取决于所用的区域设置和 Visual Studio 的版本。 在 Visual Studio 2015 或更早版本的开发人员命令提示符中，规则文件夹为 `%ProgramFiles(x86)%\MSBuild\Microsoft.Cpp\v4.0\<version>\<locale>`。 在 Visual Studio 2015 中，`<version>` 值为 `v140`。 `<locale>` 为 LCID，例如，用于英语的 `1033`。 对于安装的每个 Visual Studio 版本和每种语言，将使用不同的路径。 例如，Visual Studio 2015 Community 英文版的默认规则文件夹路径可能是 `C:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\v140\1033\`。

::: moniker-end

::: moniker range="msvc-150"

默认规则文件夹的路径取决于所用的区域设置和 Visual Studio 的版本。 在 Visual Studio 2017 的开发人员命令提示符中，规则文件夹为 `%VSINSTALLDIR%Common7\IDE\VC\VCTargets\<locale>\`。 `<locale>` 为 LCID，例如，用于英语的 `1033`。 在 Visual Studio 2015 或更早版本的开发人员命令提示符中，规则文件夹为 `%ProgramFiles(x86)%\MSBuild\Microsoft.Cpp\v4.0\<version>\<locale>\`，其中 `<version>` 值是 Visual Studio 2015 中的 `v140` 。 对于安装的每个 Visual Studio 版本和每种语言，将使用不同的路径。 例如，Visual Studio 2017 Community 英文版的默认规则文件夹路径可能是 `C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\VC\VCTargets\1033\`。

::: moniker-end

::: moniker range=">=msvc-160"

默认规则文件夹的路径取决于所用的区域设置和 Visual Studio 的版本。 在 Visual Studio 2019 或更高版本的开发人员命令提示符中，规则文件夹为 `%VSINSTALLDIR%MSBuild\Microsoft\VC\<version>\<locale>\`，其中 `<version>` 值是 Visual Studio 2019 中的 `v160` 。 `<locale>` 为 LCID，例如，用于英语的 `1033`。 在 Visual Studio 2017 中，规则文件夹为 `%VSINSTALLDIR%Common7\IDE\VC\VCTargets\<locale>\`。 在 Visual Studio 2015 或更早版本的开发人员命令提示符中，规则文件夹为 `%ProgramFiles(x86)%\MSBuild\Microsoft.Cpp\v4.0\<version>\<locale>\`。 对于安装的每个 Visual Studio 版本和每种语言，将使用不同的路径。 例如，Visual Studio 2019 Community 英文版的默认规则文件夹路径可能是 `C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Microsoft\VC\v160\1033\`。

::: moniker-end

只需了解这些文件的内部工作原理，并在几个方案中了解 Visual Studio IDE：

- 要创建自定义属性页，或
- 你需要在不使用 Visual Studio IDE 的情况下自定义你的项目属性。

## <a name="contents-of-rule-files"></a>规则文件的内容

首先，让我们打开项目的属性页。 右键单击 " **解决方案资源管理器** 中的项目节点，然后选择" **属性** "：

![显示 Visual Studio c + + 项目属性对话框](../media/cpp-property-page-2017.png)

" **配置属性** " 下的每个节点称为 " *规则* "。 规则有时表示单一工具，如编译器。 通常，术语是指具有属性、执行并可能生成某些输出的内容。 每个规则都从默认规则文件夹中的 XML 文件填充。 例如，此处显示的 C/c + + 规则由 *"cl.xml"* 填充。

每个规则都具有一组属性，这些属性按 *类别* 进行组织。 规则下的每个子节点表示一个类别。 例如， **c/c + +** 下的 **优化** 节点包含编译器工具的所有与优化相关的属性。 属性及其值在右窗格中以网格格式呈现。

您可以 *`cl.xml`* 在记事本或任何 XML 编辑器中打开。 你将看到名为的根节点 `Rule` 。 它定义了在 UI 中显示的相同的属性列表以及其他元数据。

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--Copyright, Microsoft Corporation, All rights reserved.-->
<Rule Name="CL" PageTemplate="tool" DisplayName="C/C++" SwitchPrefix="/" Order="10" xmlns="http://schemas.microsoft.com/build/2009/properties" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" xmlns:sys="clr-namespace:System;assembly=mscorlib">
  <Rule.Categories>
    <Category Name="General" DisplayName="General" />
    <Category Name="Optimization" DisplayName="Optimization" />
    <Category Name="Preprocessor" DisplayName="Preprocessor" />
    <Category Name="Code Generation" DisplayName="Code Generation" />
    <Category Name="Language" DisplayName="Language" />
    <Category Name="Precompiled Headers" DisplayName="Precompiled Headers" />
    <Category Name="Output Files" DisplayName="Output Files" />
    <Category Name="Browse Information" DisplayName="Browse Information" />
    <Category Name="Advanced" DisplayName="Advanced" />
    <Category Name="All Options" DisplayName="All Options" Subtype="Search" />
    <Category Name="Command Line" DisplayName="Command Line" Subtype="CommandLine" />
  </Rule.Categories>
  <!-- . . . -->
</Rule>
```

"属性页" UI 中 " **配置属性** " 下的每个节点都有一个 XML 文件。 可以在 UI 中添加或删除规则：这是通过在项目中的相应 XML 文件中包含或删除位置来完成的。 例如， *`Microsoft.CppBuild.targets`* (找到的级别高于1033文件夹) 包括 *`cl.xml`* ：

```xml
<PropertyPageSchema Condition="'$(ConfigurationType)' != 'Utility'" Include="$(VCTargetsPath)$(LangID)\cl.xml"/>
```

如果您要去除 *`cl.xml`* 所有数据，则可以使用以下基本框架：

```xml
<?xml version="1.0" encoding="utf-8"?>
<Rule>
  <Rule.DataSource />
  <Rule.Categories>
    <Category />
    <!-- . . . -->
  </Rule.Categories>
  <BoolProperty />
  <EnumProperty />
  <IntProperty />
  <StringProperty />
  <StringListProperty />
</Rule>
```

下一节将介绍你可以附加的每个主要元素和某些元数据。

### <a name="rule-attributes"></a>规则属性

**`<Rule>`** 元素是 XML 文件中的根节点。 它可以有多个属性：

```xml
<Rule Name="CL" PageTemplate="tool" SwitchPrefix="/" Order="10"
          xmlns="http://schemas.microsoft.com/build/2009/properties"
          xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
          xmlns:sys="clr-namespace:System;assembly=mscorlib">
  <Rule.DisplayName>
    <sys:String>C/C++</sys:String>
  </Rule.DisplayName>
```

- **`Name`** ： Name 特性是的 ID `Rule` 。 它在项目的所有属性页 XML 文件中都必须是唯一的。

- **`PageTemplate`** ： UI 使用此属性的值从 UI 模板集合中进行选择。 “工具”模板以标准网格形式呈现属性。 此特性的其他内置值为“调试程序”和“通用”。 请分别查看“调试”节点和“常规”节点，以查看指定这些值所产生的 UI 格式。 "调试器" 页模板的 UI 使用下拉框在不同调试器的属性之间进行切换。 "通用" 模板在一页中显示不同的属性类别，而不是在节点下具有多个类别子节点 `Rule` 。 此属性只是对 UI 的建议。 该 XML 文件设计为与 UI 无关。 不同的 UI 可能出于不同目的使用此特性。

- **`SwitchPrefix`** ：用于开关的命令行中使用的前缀。 如果值为 `"/"` ，则将产生类似于、、等的开关 **`/ZI`** **`/nologo`** **`/W3`** 。

- **`Order`** ： `Rule` 与系统中的所有其他规则相比，在相对位置对预期的 UI 客户端提出建议。

- **`xmlns`** ：标准 XML 元素。 可以看到三个列出的命名空间。 这些属性分别对应于 XML 反序列化类、XML 架构和系统命名空间的命名空间。

- **`DisplayName`** ：显示在节点的属性页 UI 上的名称 `Rule` 。 此值已本地化。 作为的 `DisplayName` 子元素 `Rule` （而不是）作为 (如或) 属性的子元素创建， `Name` `SwitchPrefix` 因为内部本地化工具要求。 从 XML 角度来看，两者都是等效的。 因此，可以仅将其用作用于减少混乱或保持原样的特性。

- **`DataSource`** ：此重要属性告知项目系统读取和写入属性值的位置，并 (稍后将) 的分组。 对于 *`cl.xml`* ，这些值为：

    ```xml
    <DataSource Persistence="ProjectFile" ItemType="ClCompile" Label="" HasConfigurationCondition="true" />
    ```

  - `Persistence="ProjectFile"` 告诉项目系统，应将的所有属性都 `Rule` 写入项目文件或属性表文件 (具体取决于) 使用哪种节点来生成属性页。 其他可能的值为 `"UserFile"` ，它将向文件写入值 *`.user`* 。

  - `ItemType="ClCompile"` 表示属性将存储为 ItemDefinition 元数据或此项类型的项元数据（后者仅在属性页从解决方案资源管理器中的文件节点生成的情况下适用）。 如果未设置此字段，则该属性将作为 PropertyGroup 中的公共属性写入。

  - `Label=""` 指示在属性写入为 `ItemDefinition` 元数据时，父级 ItemDefinitionGroup 的标签将为空（每个 MSBuild 元素都可以有一个标签）。 Visual Studio 2017 和更高版本使用已标记的组来导航 vcxproj 项目文件。 包含大多数属性的组将 `Rule` 空字符串作为标签。

  - `HasConfigurationCondition="true"` 指示项目系统为值附加配置条件，以便其仅对当前项目配置生效（该条件可以附加到父组或值本身）。 例如，打开项目节点的属性页，并将“配置属性”>“C/C++ 常规”下的“将警报视为错误”属性值设为“是”  。 以下值将写入项目文件中。 请注意附加到父级 ItemDefinitionGroup 的配置条件。

    ```xml
    <ItemDefinitionGroup Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">
      <ClCompile>
        <TreatWarningAsError>true</TreatWarningAsError>
      </ClCompile>
    </ItemDefinitionGroup>
    ```

     如果在特定文件的属性页中设置此值（如 *`stdafx.cpp`* ），则应在项目文件中的项下写入属性值， `stdafx.cpp` 如下所示。 请注意配置条件如何直接附加到元数据本身：

    ```xml
    <ItemGroup>
      <ClCompile Include="stdafx.cpp">
        <TreatWarningAsError Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">true</TreatWarningAsError>
      </ClCompile>
    </ItemGroup>
    ```

  此处未列出的另一个属性 `DataSource` 为 `PersistedName` 。 通过此特性，可以使用不同的名称表示项目文件中的属性。 默认情况下，此特性设置为属性的 `Name` 。

  单个属性可以重写 `DataSource` 其父属性的 `Rule` 。 在这种情况下，该属性的值的位置将不同于中的其他属性 `Rule` 。

- 还有其他一些属性 `Rule` ，其中包括 `Description` 和 `SupportsFileBatching` ，此处未显示。 `Rule`可以通过浏览这些类型的文档来获取适用于任何其他元素上的或的完整属性集。 或者，可以检查 `Microsoft.Build.Framework.dll` 程序集中 `Microsoft.Build.Framework.XamlTypes` 命名空间中的类型的公共属性。

- **`DisplayName`** 、 **`PageTemplate`** 和 **`Order`** 是 ui 相关的属性，这些属性在此其他与 ui 无关的数据模型中提供。 用于显示属性页的任何 UI 几乎肯定会使用这些属性。 `DisplayName` 和 `Description` 是在 XML 文件中几乎所有元素上都存在的两个属性。 而且，这两个属性是本地化的唯一属性。

### <a name="category-elements"></a>Category 元素

`Rule`可以有多个 `Category` 元素。 在 XML 文件中列出类别的顺序是用户界面以相同顺序显示类别的建议。 例如，UI 中的 " **c/c + +** " 节点下的类别顺序与中的顺序相同 *`cl.xml`* 。 示例类别如下所示：

```xml
<Category Name="Optimization">
  <Category.DisplayName>
    <sys:String>Optimization</sys:String>
  </Category.DisplayName>
</Category>
```

此代码段显示 `Name` `DisplayName` 之前描述的和属性。 同样， `Category` 示例中不会显示其他属性。 您可以通过阅读文档或使用检查程序集来了解这些问题 *`ildasm.exe`* 。

### <a name="property-elements"></a>属性元素

大多数规则文件包含 `Property` 元素。 它们包含中的所有属性的列表 `Rule` 。 每个属性都可以是基本框架中显示的五种可能类型之一： `BoolProperty` 、 `EnumProperty` 、 `IntProperty` 、 `StringProperty` 和 `StringListProperty` 。 文件中可能只有几种类型。 属性具有多个属性，使其能够详细说明。 `StringProperty`此处介绍了。 其余内容类似。

```xml
<StringProperty Subtype="file" Name="ObjectFileName" Category="Output Files" Switch="Fo">
  <StringProperty.DisplayName>
    <sys:String>Object File Name</sys:String>
  </StringProperty.DisplayName>
  <StringProperty.Description>
    <sys:String>Specifies a name to override the default object file name; can be file or directory name.(/Fo[name])</sys:String>
  </StringProperty.Description>
</StringProperty>
```

此代码片段中的大部分特性上文前都已介绍过。 新的是 `Subtype` 、 `Category` 和 `Switch` 。

- **`Subtype`** 仅适用于 `StringProperty` 和元素的属性 `StringListProperty` 。 它提供上下文信息。 例如，值 `file` 指示属性表示文件路径。 Visual Studio 使用此类上下文信息来增强编辑体验。 例如，它可能提供一个 Windows 资源管理器窗口，该窗口允许用户将文件直观地选为属性的编辑器。

- **`Category`** ：此属性所属的类别。 尝试在 UI 的“输出文件”类别下查找此属性  。

- **`Switch`** ：当规则表示工具（例如编译器工具）时，大多数 `Rule` 属性将作为切换传递到生成时的工具可执行文件。 此属性的值指示要使用的开关文本。 该 `<StringProperty>` 示例指定其开关应为 **`Fo`** 。 与 `SwitchPrefix` 父级上的特性结合时 `Rule` ，此属性作为传递到可执行文件  **`/Fo"Debug\"`** 。 它在 C/c + + 的命令行中显示在属性页 UI 中。

   其他属性特性包括：

- **`Visible`** ：如果不希望属性显示在属性页中，但要在生成时使用，请将此特性设置为 `false` 。

- **`ReadOnly`** ：如果要在属性页中提供此属性值的只读视图，请将此特性设置为 `true` 。

- **`IncludeInCommandLine`** ：在生成时，工具可能不需要它的某些属性。 将此特性设置为 `false` ，以防止传递特定属性。
