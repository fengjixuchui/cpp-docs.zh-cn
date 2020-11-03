---
title: .vcxproj 文件和通配符
description: C + + 本机 MSBuild 项目 .vcxproj 文件如何处理通配符。
ms.date: 09/30/2020
helpviewer_keywords:
- .vcxproj file structure
ms.assetid: 14d0c552-29db-480e-80c1-7ea89d6d8e9c
ms.openlocfilehash: 23d36a63f328e14cca59d1d57838173b4dcb0954
ms.sourcegitcommit: f7fbdc39d73e1fb3793c396fccf7a1602af7248b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/02/2020
ms.locfileid: "91663093"
---
# <a name="vcxproj-files-and-wildcards"></a>`.vcxproj` 文件和通配符

Visual Studio IDE 不支持文件的项目项中的某些构造 *`.vcxproj`* 。 这些不支持的构造包括通配符、分号分隔的列表或扩展到多个文件的 MSBuild 宏。 `.vcxproj`C + + 生成的项目系统比 MSBuild 具有更强的限制性。 每个项目项都需要有自己的 MSBuild 项。 有关文件格式的详细信息 `.vcxproj` ，请参阅[ `.vcxproj` 和 `.props` 文件结构](vcxproj-file-structure.md)。

IDE 不支持这些构造示例：

```xml
<ItemGroup>
  <None Include="*.txt">
  <ClCompile Include="a.cpp;b.cpp"/>
  <ClCompile Include="@(SomeItems)" />
</ItemGroup>
```

如果 `.vcxproj` 在 IDE 中加载包含这些构造的项目文件，则项目可能会在第一次运行。 但是，一旦 Visual Studio 修改了项目，然后将其保存在磁盘上，就可能会出现问题。 你可能会遇到随机崩溃和未定义的行为。

在 Visual Studio 2019 版本16.7 中，当 Visual Studio 加载 `.vcxproj` 项目文件时，它会自动检测项目项中不受支持的项。 在解决方案加载过程中，将在 "输出" 窗口中看到警告。

Visual Studio 2019 版本16.7 还添加了只读项目支持。 只读支持允许 IDE 使用手动编写的项目，这些项目不具有 IDE 可编辑项目的额外限制。

如果 `.vcxproj` 文件使用了一个或多个不受支持的构造，则可以使用以下选项之一在 IDE 中将其加载而不出现警告：

- 显式列出所有项
- 将项目标记为只读
- 将通配符项移动到目标正文

## <a name="list-all-items-explicitly"></a>显式列出所有项

目前，没有办法使通配符扩展项在非只读项目的 "解决方案资源管理器" 窗口中可见。 解决方案资源管理器要求项目显式列出所有项。

若要使 `.vcxproj` 项目在 Visual Studio 2019 版本16.7 或更高版本中自动扩展通配符，请将 `ReplaceWildcardsInProjectItems` 属性设置为 `true` 。 建议你 *`Directory.Build.props`* 在根目录中创建一个文件，并使用以下内容：

```xml
<Project>
  <PropertyGroup>
    <ReplaceWildcardsInProjectItems>true</ReplaceWildcardsInProjectItems>
  </PropertyGroup>
</Project>
```

## <a name="mark-your-project-as-read-only"></a>将项目标记为只读

在 Visual Studio 2019 版本16.7 及更高版本中，你可以将项目标记为 *只读*。 若要将项目标记为只读，请将以下属性添加到 *`.vcxproj`* 文件，或添加到它导入的任何文件：

```xml
<PropertyGroup>
    <ReadOnlyProject>true</ReadOnlyProject>
</PropertyGroup>
```

此 `<ReadOnlyProject>` 设置可防止 Visual Studio 编辑和保存项目，因此可以使用其中的任何 MSBuild 构造，包括通配符。

如果 Visual Studio 在文件中的项目项 *`.vcxproj`* 或任何导入中检测到通配符，则必须知道项目缓存不可用。 如果有大量使用通配符的项目，IDE 中的解决方案加载时间会更长。

## <a name="move-wildcard-items-to-a-target-body"></a>将通配符项移动到目标正文

你可能想要使用通配符收集资源、添加生成的源等。 如果不需要它们列在 "解决方案资源管理器" 窗口中，可以改为使用此过程：

1. 更改项组的名称以添加通配符。 例如，而不是：

   ```xml
   <Image Include="*.bmp" />
   <ClCompile Include="*.cpp" />
   ```

   将其更改为：

   ```xml
   <_WildCardImage Include="*.bmp" />
   <_WildCardClCompile Include="*.cpp" />
   ```

1. 将此内容添加到  *`.vcxproj`* 文件。或者，将其添加到 *`Directory.Build.targets`* 根目录中的文件，以影响该根目录下的所有项目：

   ```xml
   <Target Name="AddWildCardItems"
       AfterTargets="BuildGenerateSources">
     <ItemGroup>
       <Image Include="@(_WildCardImage)" />
       <ClCompile Include="@(_WildCardClCompile)" />
     </ItemGroup>
   </Target>
   ```

   此更改使生成在文件中定义时看到项  *`.vcxproj`* 。 但现在，它们在 "解决方案资源管理器" 窗口中不可见，并且它们不会导致 IDE 中出现问题。

1. 若要  `_WildCardClCompile`   在编辑器中打开这些文件时显示正确的项 IntelliSense，请添加以下内容：

   ```xml
   <PropertyGroup>
     <ComputeCompileInputsTargets>
       AddWildCardItems
       $(ComputeCompileInputsTargets)
     </ComputeCompileInputsTargets>
   </PropertyGroup>
   ```

实际上，可以将通配符用于目标正文中的任何项。 你还可以在  `ItemGroup`   未通过定义为项目项的中使用通配符 [`ProjectSchemaDefinition`](https://devblogs.microsoft.com/cppblog/vc-MSBuild-extensibility-example/) 。

> [!NOTE]
> 如果将通配符包括从文件移 *`.vcxproj`* 到导入的文件，则这些文件将不会显示在 "解决方案资源管理器" 窗口中。 此更改还允许您的项目无需修改即可在 IDE 中加载。 但是，我们不建议采用这种方法，因为它会禁用项目缓存。

## <a name="see-also"></a>请参阅

[在 Visual Studio 中设置 C++ 编译器并生成属性](../working-with-project-properties.md)<br/>
[属性页 XML 文件](property-page-xml-files.md)
