---
title: .vcxproj 和 .props 文件结构
description: C + + 本机 MSBuild 项目 .vcxproj 和属性文件存储项目信息的方式。
ms.date: 09/30/2020
helpviewer_keywords:
- .vcxproj file structure
ms.assetid: 14d0c552-29db-480e-80c1-7ea89d6d8e9c
ms.openlocfilehash: 562ef0c1b371d7212f31da1917d19c012e4cbb24
ms.sourcegitcommit: f7fbdc39d73e1fb3793c396fccf7a1602af7248b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/02/2020
ms.locfileid: "91662277"
---
# <a name="vcxproj-and-props-file-structure"></a>`.vcxproj` 和 `.props` 文件结构

[MSBuild](../msbuild-visual-cpp.md)是 Visual Studio 中的默认项目系统;当你**File**  >  在中选择 "文件" "**新建项目**" 时 Visual C++ 你要创建一个 MSBuild 项目，其设置存储在具有扩展名的 XML 项目文件中 *`.vcxproj`* 。 项目文件也可能会导入 *`.props`* *`.targets`* 可存储设置的文件和文件。

建议仅 *`.vcxproj`* 在 IDE 中创建和修改项目，并尽可能避免手动编辑。 在大多数情况下，你永远不需要手动编辑项目文件。 应尽可能使用 Visual Studio 属性页来修改项目设置。 有关详细信息，请参阅[在 Visual Studio 中设置 C++ 编译器和生成属性](../working-with-project-properties.md)。

如果需要在 IDE 中无法进行自定义，我们建议添加自定义属性或目标。 插入自定义项的便利位置是 *`Directory.Build.props`* 和 *`Directory.Build.targets`* 文件，它们会自动导入到所有基于 MSBuild 的项目中。

在某些情况下，您可能仍需要手动修改 *`.vcxproj`* 项目文件或属性表。 建议你不要手动编辑它，除非你充分了解 MSBuild，并按照本文中的指南进行操作。 为了使 IDE 自动加载和更新 *`.vcxproj`* 文件，这些文件具有几个不适用于其他 MSBuild 项目文件的限制。 它们不是为手动编辑而设计的。 错误可能导致 IDE 崩溃或以意想不到的方式运行。

对于手动编辑方案，本文包含有关和相关文件的结构的基本信息 *`.vcxproj`* 。

**重要提示：**

如果选择手动编辑 *`.vcxproj`* 文件，请注意以下事实：

- 文件结构必须遵循本文中所述的规定形式。

- Visual Studio c + + 项目系统当前不支持直接在项目项中提供通配符或列表。 例如，不支持以下形式：

   ```xml
   <ItemGroup>
     <None Include="*.txt"/>
     <ClCompile Include="a.cpp;b.cpp"/>
   </ItemGroup>
   ```

   有关项目中的通配符支持和可能的解决方法的详细信息，请参阅[ `.vcxproj` 文件和通配符](vcxproj-files-and-wildcards.md)。

- Visual Studio c + + 项目系统当前不支持项目项路径中的宏。 例如，不支持此窗体：

   ```xml
   <ItemGroup>
     <ClCompile Include="$(IntDir)\generated.cpp"/>
   </ItemGroup>
   ```

   "不支持" 是指不保证宏适用于 IDE 中的所有操作。 不会更改其在不同配置中的值的宏应该可以正常工作，但如果将项移动到不同的筛选器或项目，则可能不会保留这些宏。 更改不同配置的值的宏会导致问题。 这是因为 IDE 不会希望项目项路径对于不同的项目配置不相同。

- 若要在 " **项目属性** " 对话框中编辑项目属性时正确添加、移除或修改项目属性，该文件必须为每个项目配置包含单独的组。 条件的格式必须为：

   ```xml
   Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'"
   ```

- 每个属性都必须在具有正确标签的组中指定，如属性规则文件中所指定。 有关详细信息，请参阅[属性页 XML 规则文件](property-page-xml-files.md)。

## <a name="vcxproj-file-elements"></a>`.vcxproj` 文件元素

您可以 *`.vcxproj`* 使用任何文本编辑器或 XML 编辑器检查文件的内容。 若要在 Visual Studio 中进行查看，请在解决方案资源管理器中右键单击项目，选择“卸载项目”，再选择“编辑 Foo.vcxproj”********。

要注意的第一点是顶级元素按特定顺序显示。 例如：

- 大多数的属性组和项定义组都在导入 Microsoft.Cpp.Default.props 后出现。

- 所有目标都在文件末尾导入。

- 存在多个属性组，每个都带有唯一标签并按特定顺序显示。

项目文件中元素的顺序至关重要，因为 MSBuild 基于顺序计算模型。  如果项目文件（包括所有导入的 *`.props`* 和 *`.targets`* 文件）包含一个属性的多个定义，则最后一个定义将覆盖前面的定义。 在下面的示例中，将在编译过程中设置值 "xyz"，因为 MSBUild 引擎最后在其计算过程中遇到它。

```xml
  <MyProperty>abc</MyProperty>
  <MyProperty>xyz</MyProperty>
```

以下代码片段演示了一个最小的 *`.vcxproj`* 文件。 *`.vcxproj`* Visual Studio 生成的任何文件都将包含这些顶级 MSBuild 元素。 而且，它们会按此顺序显示，尽管它们可能包含每个此类顶级元素的多个副本。 任何 `Label` 属性都是任意标记，仅供 Visual Studio 用作 signposts 进行编辑; 它们没有其他功能。

```xml
<Project DefaultTargets="Build" ToolsVersion="4.0" xmlns='http://schemas.microsoft.com/developer/msbuild/2003'>
  <ItemGroup Label="ProjectConfigurations" />
  <PropertyGroup Label="Globals" />
  <Import Project="$(VCTargetsPath)\Microsoft.Cpp.default.props" />
  <PropertyGroup Label="Configuration" />
  <Import Project="$(VCTargetsPath)\Microsoft.Cpp.props" />
  <ImportGroup Label="ExtensionSettings" />
  <ImportGroup Label="PropertySheets" />
  <PropertyGroup Label="UserMacros" />
  <PropertyGroup />
  <ItemDefinitionGroup />
  <ItemGroup />
  <Import Project="$(VCTargetsPath)\Microsoft.Cpp.targets" />
  <ImportGroup Label="ExtensionTargets" />
</Project>
```

以下各节描述了其中每个元素的用途，并说明了它们的排序方式：

### <a name="project-element"></a>Project 元素

```xml
<Project DefaultTargets="Build" ToolsVersion="4.0" xmlns='http://schemas.microsoft.com/developer/msbuild/2003' >
```

`Project` 为根节点。 它指定要使用的 MSBuild 版本，还指定在此文件传递到 MSBuild.exe 时要执行的默认目标。

### <a name="projectconfigurations-itemgroup-element"></a>ProjectConfigurations ItemGroup 元素

```xml
<ItemGroup Label="ProjectConfigurations" />
```

`ProjectConfigurations` 包含项目配置描述。 例如“Debug|Win32”、“Release|Win32”和“Debug|ARM”等等。 很多项目设置都特定于给定的配置。 例如，你可能想要设置发布版本的优化属性，但不是调试版本。

`ProjectConfigurations`生成时未使用项组。 Visual Studio IDE 需要它来加载项目。 此项组可以移动到 *`.props`* 文件并导入到文件中 *`.vcxproj`* 。 但在这种情况下，如果需要添加或删除配置，则必须手动编辑该 *`.props`* 文件; 不能使用 IDE。

### <a name="projectconfiguration-elements"></a>ProjectConfiguration 元素

下面的代码片段演示一个项目配置。 在此示例中，"Debug | x64" 是配置名称。 项目配置名称必须采用格式 `$(Configuration)|$(Platform)` 。 `ProjectConfiguration`节点可以有两个属性： `Configuration` 和 `Platform` 。 当配置处于活动状态时，这些属性会自动设置为此处指定的值。

```xml
<ProjectConfiguration Include="Debug|x64">
  <Configuration>Debug</Configuration>
  <Platform>x64</Platform>
</ProjectConfiguration>
```

IDE 需要为 `Configuration` `Platform` 所有项中使用的和值的任意组合查找项目配置 `ProjectConfiguration` 。 通常，这意味着，项目可能有无意义的项目配置来满足此要求。 例如，如果项目具有这些配置：

- Debug|Win32

- Retail|Win32

- Special 32-bit Optimization|Win32

那么即使“Special 32-bit Optimization”对于 x64 是没有意义的，项目还是需要这些配置：

- 调试|x64

- Retail|x64

- Special 32-bit Optimization|x64

可以在“解决方案配置管理器”中禁用任何配置的生成和部署命令****。

### <a name="globals-propertygroup-element"></a>Globals PropertyGroup 元素

```xml
<PropertyGroup Label="Globals" />
```

`Globals` 包含项目级别设置，例如 `ProjectGuid` 、 `RootNamespace` 、和 `ApplicationType` `ApplicationTypeRevision` 。 最后两项通常定义目标操作系统。 项目只能面向单个 OS，因为当前、引用和项目项不能有条件。 通常不会在项目文件中的其他位置重写这些属性。 此组不依赖于配置，并且 `Globals` 项目文件中通常只存在一个组。

### <a name="microsoftcppdefaultprops-import-element"></a>Microsoft.Cpp.default.props Import 元素

```xml
<Import Project="$(VCTargetsPath)\Microsoft.Cpp.default.props" />
```

**属性**属性表随 Visual Studio 一起提供，无法进行修改。 它包含项目的默认设置。 根据 ApplicationType，可能默认值会有所不同。

### <a name="configuration-propertygroup-elements"></a>Configuration PropertyGroup 元素

```xml
<PropertyGroup Label="Configuration" />
```

`Configuration` 属性组具备附加的配置条件（例如 `Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'"`），并在多个副本中出现（每个配置各一个）。 此属性组承载着为特定配置设置的属性。 配置属性包括 PlatformToolset，并控制 Microsoft.Cpp.props 中的系统属性表所包含的内容****。 例如，如果定义 `<CharacterSet>Unicode</CharacterSet>` 属性，则会包含系统属性表 microsoft.Cpp.unicodesupport.props****。 如果检查 **属性**，将看到以下行： `<Import Condition="'$(CharacterSet)' == 'Unicode'" Project="$(VCTargetsPath)\microsoft.Cpp.unicodesupport.props" />` 。

### <a name="microsoftcppprops-import-element"></a>Microsoft.Cpp.props Import 元素

```xml
<Import Project="$(VCTargetsPath)\Microsoft.Cpp.props" />
```

**属性**属性表 (直接或通过导入) 定义许多工具特定属性的默认值。 示例包括编译器的优化和警告级别属性、MIDL 工具的 TypeLibraryName 属性等。 它还会根据属性组中紧靠其后定义的配置属性导入各种系统属性表。

### <a name="extensionsettings-importgroup-element"></a>ExtensionSettings ImportGroup 元素

```xml
<ImportGroup Label="ExtensionSettings" />
```

`ExtensionSettings` 组包含属于生成自定义项的属性表的导入。 生成自定义项由最多三个文件定义： *`.targets`* 文件、 *`.props`* 文件和 *`.xml`* 文件。 此导入组包含文件的导入 *`.props`* 。

### <a name="propertysheets-importgroup-elements"></a>PropertySheets ImportGroup 元素

```xml
<ImportGroup Label="PropertySheets" />
```

`PropertySheets` 组包含用户属性表的导入。 这些导入是通过 Visual Studio 中的属性管理器视图添加的属性表。 这些导入的列出顺序很重要，且会反映在属性管理器中。 项目文件通常包含此类导入组的多个实例，每个项目配置各一个。

### <a name="usermacros-propertygroup-element"></a>UserMacros PropertyGroup 元素

```xml
<PropertyGroup Label="UserMacros" />
```

`UserMacros` 包含创建为变量的属性，这些属性用于自定义生成过程。 例径定如，可定义将自定义输出路义为 $(CustomOutputPath) 的用户宏，并用它来定义其他变量。 此属性组包含这类属性。 在 Visual Studio 中，此组不会在项目文件中填充，因为 Visual C++ 不支持用于配置的用户宏。 属性表支持用户宏。

### <a name="per-configuration-propertygroup-elements"></a>按配置 PropertyGroup 元素

```xml
<PropertyGroup />
```

此属性组有多个实例，所有项目配置都各有一个。 每个属性组必须具备一个附加的配置条件。 如果漏掉了任何配置，“项目属性”对话框将不会正常工作****。 与前面列出的属性组不同，此属性组没有标签。 此组包含项目配置级别的设置。 这些设置适用于所有属于该特定项组的文件。 在此处初始化生成自定义项定义元数据。

此 PropertyGroup 必须出现在之后 `<Import Project="$(VCTargetsPath)\Microsoft.Cpp.props" />` ，并且必须没有任何其他 PropertyGroup (否则，项目属性编辑不会正确) 。

### <a name="per-configuration-itemdefinitiongroup-elements"></a>按配置 ItemDefinitionGroup 元素

```xml
<ItemDefinitionGroup />
```

包含项定义。 这些定义必须遵循相同的条件规则和每个配置的标签 `PropertyGroup` 元素。

### <a name="itemgroup-elements"></a>ItemGroup 元素

```xml
<ItemGroup />
```

`ItemGroup` 元素包含项目中) 项 (项。 项目项不支持条件 (也就是说，按规则定义将项类型视为项目项) 。

对于每个配置，元数据应具有配置条件，即使它们都是相同的。 例如：

```xml
<ItemGroup>
  <ClCompile Include="stdafx.cpp">
    <TreatWarningAsError Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">true</TreatWarningAsError>
    <TreatWarningAsError Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">true</TreatWarningAsError>
  </ClCompile>
</ItemGroup>
```

Visual Studio c + + 项目系统当前不支持项目项中的通配符。

```xml
<ItemGroup>
  <ClCompile Include="*.cpp"> <!--Error-->
</ItemGroup>
```

Visual Studio c + + 项目系统当前不支持项目项中的宏。

```xml
<ItemGroup>
  <ClCompile Include="$(IntDir)\generated.cpp"> <!--not guaranteed to work in all scenarios-->
</ItemGroup>
```

ItemGroup 中指定了引用，且这些引用具有以下限制：

- 引用不支持条件。

- 引用元数据不支持条件。

### <a name="microsoftcpptargets-import-element"></a>Microsoft.Cpp.targets Import 元素

```xml
<Import Project="$(VCTargetsPath)\Microsoft.Cpp.targets" />
```

直接定义 (或通过导入) c + + 目标（如生成、清理等）。

### <a name="extensiontargets-importgroup-element"></a>ExtensionTargets ImportGroup 元素

```xml
<ImportGroup Label="ExtensionTargets" />
```

此组包含生成自定义项目标文件的导入。

## <a name="impact-of-incorrect-ordering"></a>排序错误的影响

Visual Studio IDE 依赖于前面所述排序的项目文件。 例如，当你在属性页中定义属性值时，IDE 通常会将属性定义放置在带有空标签的属性组中。 此顺序可确保在系统属性表中引入的默认值被用户定义的值覆盖。 同样，目标文件会在结束时导入，因为它们使用之前定义的属性，并且它们通常不会定义属性本身。 同样，用户属性表会在)  (包含的系统属性表后导入 *`Microsoft.Cpp.props`* 。 此顺序确保用户可以覆盖系统属性表中引入的任何默认值。

如果 *`.vcxproj`* 文件不遵循此布局，则生成结果可能不是你所期望的结果。 例如，如果在用户定义的属性表后错误地导入了系统属性表，则系统属性表将覆盖用户设置。

即使是 IDE 设计时体验，也取决于元素的正确排序的某些方面。 例如，如果您 *`.vcxproj`* 的文件没有 `PropertySheets` 导入组，则 IDE 可能无法确定在何处放置用户在 **属性管理器**中创建的新属性表。 这可能导致用户表被系统表覆盖。 尽管 IDE 使用的试探法可以容忍文件布局中的细微 *`.vcxproj`* 不一致，但我们强烈建议您不要偏离本文前面所述的结构。

## <a name="how-the-ide-uses-element-labels"></a>IDE 如何使用元素标签

在 IDE 中，在 "常规" 属性页中设置 " **UseOfAtl** " 属性时，该属性将写入项目文件中的配置属性组。 同一属性页中的 **TargetName** 属性将写入到每个配置不变的属性组。 Visual Studio 会在属性页的 xml 文件中查找关于属性写入位置的信息。 对于 " **常规** " 属性页，假设你有 Visual Studio 2019 Enterprise Edition 的英文版本，则该文件为 `%ProgramFiles(x86)%\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\VC\VCTargets\1033\general.xml` 。 属性页 XML 规则文件定义关于 Rule 及其所有属性的静态信息。 这类信息之一就是 Rule 属性在目标文件（也就是将要写入值的文件）中的首选位置。 首选位置由项目文件元素上的 Label 特性指定。

## <a name="property-sheet-layout"></a>属性表布局

以下 XML 代码片段是属性表 (.props) 文件的最简布局。 它类似于 *`.vcxproj`* 文件， *`.props`* 可以从前面的讨论中推断出元素的功能。

```xml
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <ImportGroup Label="PropertySheets" />
  <PropertyGroup Label="UserMacros" />
  <PropertyGroup />
  <ItemDefinitionGroup />
  <ItemGroup />
</Project>
```

若要创建自己的属性表，请复制该 *`.props`* 文件夹中的某个文件， *`VCTargets`* 并根据需要进行修改。 对于 Visual Studio 2019 企业版，默认 *`VCTargets`* 路径为 `%ProgramFiles%\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\VC\VCTargets` 。

## <a name="see-also"></a>请参阅

[在 Visual Studio 中设置 c + + 编译器和生成属性](../working-with-project-properties.md)\
[属性页 XML 文件](property-page-xml-files.md)\
[`.vcxproj` 文件和通配符](vcxproj-files-and-wildcards.md)
