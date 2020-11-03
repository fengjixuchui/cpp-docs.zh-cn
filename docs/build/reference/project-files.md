---
title: 示例项目文件
ms.date: 08/19/2019
helpviewer_keywords:
- .vcxproj files
- C++ projects, project file format
ms.assetid: 5261cf45-3136-40a6-899e-dc1339551401
ms.openlocfilehash: eef28961ab8c4d3a34a74999c7e0c69a4fc3fced
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924008"
---
# <a name="project-files"></a>项目文件

Visual Studio 中的 c + + 项目文件是基于 XML 的文件，其文件扩展名为 .vcxproj，包含生成 c + + 项目所需的信息。 请注意，项目文件将导入具有 ". 属性" 或 ".targets" 扩展名的各种项目文件。 这些文件包含其他生成信息，并且可能会引用其他 "属性" 或 ".targets" 文件。 文件路径中的宏（例如 `$(VCTargetsPath)`）取决于安装的 Visual Studio。 有关这些宏和 ". 属性" 和 ".targets" 文件的详细信息，请参阅 " [VC + + 目录" 属性页](vcpp-directories-property-page.md)、 [设置 c + + 编译器和在 Visual Studio 中生成属性](../working-with-project-properties.md) 和 [生成命令和属性的常见宏](common-macros-for-build-commands-and-properties.md)。

## <a name="example"></a>示例

::: moniker range=">=msvc-160"

下面的 .vcxproj 文件是通过在 " **新建项目** " 对话框中选择 " **Windows 桌面向导** " 生成的。 要处理项目文件，请在命令行处使用 msbuild.exe 工具或使用 IDE 中的“生成”命令  。  (无法处理此示例，因为未提供所需的源文件和头文件。 ) 有关项目文件中的 XML 元素的详细信息，请参阅 [项目文件架构参考](/visualstudio/msbuild/msbuild-project-file-schema-reference)。

::: moniker-end

::: moniker range="<=msvc-150"

以下示例 .vcxproj 文件是由在“新建项目”对话框中指定“Win32 控制台应用程序”而生成的  。 要处理项目文件，请在命令行处使用 msbuild.exe 工具或使用 IDE 中的“生成”命令  。  (无法处理此示例，因为未提供所需的源文件和头文件。 ) 有关项目文件中的 XML 元素的详细信息，请参阅 [项目文件架构参考](/visualstudio/msbuild/msbuild-project-file-schema-reference)。

::: moniker-end

>[!NOTE]
> 对于 Visual Studio 2017 及更早版本中的项目，请将更改 `pch.h` 为 `stdafx.h` 和 `pch.cpp` `stdafx.cpp` 。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <ItemGroup Label="ProjectConfigurations">
    <ProjectConfiguration Include="Debug|Win32">
      <Configuration>Debug</Configuration>
      <Platform>Win32</Platform>
    </ProjectConfiguration>
    <ProjectConfiguration Include="Release|Win32">
      <Configuration>Release</Configuration>
      <Platform>Win32</Platform>
    </ProjectConfiguration>
  </ItemGroup>
  <PropertyGroup Label="Globals">
    <ProjectGuid>{96F21549-A7BF-4695-A1B1-B43625B91A14}</ProjectGuid>
    <Keyword>Win32Proj</Keyword>
    <RootNamespace>SomeProjName</RootNamespace>
  </PropertyGroup>
  <Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />
  <PropertyGroup Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'" Label="Configuration">
    <ConfigurationType>Application</ConfigurationType>
    <CharacterSet>Unicode</CharacterSet>
  </PropertyGroup>
  <PropertyGroup Condition="'$(Configuration)|$(Platform)'=='Release|Win32'" Label="Configuration">
    <ConfigurationType>Application</ConfigurationType>
    <WholeProgramOptimization>true</WholeProgramOptimization>
    <CharacterSet>Unicode</CharacterSet>
  </PropertyGroup>
  <Import Project="$(VCTargetsPath)\Microsoft.Cpp.props" />
  <ImportGroup Label="ExtensionSettings">
  </ImportGroup>
  <ImportGroup Label="PropertySheets" Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">
    <Import Project="$(UserRootDir)\Microsoft.Cpp.$(Platform).user.props" Condition="exists('$(UserRootDir)\Microsoft.Cpp.$(Platform).user.props')" Label="LocalAppDataPlatform" />
  </ImportGroup>
  <ImportGroup Label="PropertySheets" Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">
    <Import Project="$(UserRootDir)\Microsoft.Cpp.$(Platform).user.props" Condition="exists('$(UserRootDir)\Microsoft.Cpp.$(Platform).user.props')" Label="LocalAppDataPlatform" />
  </ImportGroup>
  <PropertyGroup Label="UserMacros" />
  <PropertyGroup Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">
    <LinkIncremental>true</LinkIncremental>
  </PropertyGroup>
  <PropertyGroup Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">
    <LinkIncremental>false</LinkIncremental>
  </PropertyGroup>
  <ItemDefinitionGroup Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">
    <ClCompile>
      <PrecompiledHeader>Use</PrecompiledHeader>
      <WarningLevel>Level3</WarningLevel>
      <MinimalRebuild>true</MinimalRebuild>
      <DebugInformationFormat>EditAndContinue</DebugInformationFormat>
      <Optimization>Disabled</Optimization>
      <BasicRuntimeChecks>EnableFastChecks</BasicRuntimeChecks>
      <RuntimeLibrary>MultiThreadedDebugDLL</RuntimeLibrary>
      <PreprocessorDefinitions>WIN32;_DEBUG;_CONSOLE;%(PreprocessorDefinitions)</PreprocessorDefinitions>
    </ClCompile>
    <Link>
      <SubSystem>Console</SubSystem>
      <GenerateDebugInformation>true</GenerateDebugInformation>
    </Link>
  </ItemDefinitionGroup>
  <ItemDefinitionGroup Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">
    <ClCompile>
      <WarningLevel>Level3</WarningLevel>
      <PrecompiledHeader>Use</PrecompiledHeader>
      <DebugInformationFormat>ProgramDatabase</DebugInformationFormat>
      <Optimization>MaxSpeed</Optimization>
      <RuntimeLibrary>MultiThreadedDLL</RuntimeLibrary>
      <FunctionLevelLinking>true</FunctionLevelLinking>
      <IntrinsicFunctions>true</IntrinsicFunctions>
      <PreprocessorDefinitions>WIN32;NDEBUG;_CONSOLE;%(PreprocessorDefinitions)</PreprocessorDefinitions>
    </ClCompile>
    <Link>
      <SubSystem>Console</SubSystem>
      <GenerateDebugInformation>true</GenerateDebugInformation>
      <EnableCOMDATFolding>true</EnableCOMDATFolding>
      <OptimizeReferences>true</OptimizeReferences>
    </Link>
  </ItemDefinitionGroup>
  <ItemGroup>
    <None Include="ReadMe.txt" />
  </ItemGroup>
  <ItemGroup>
    <ClInclude Include="pch.h" />
    <ClInclude Include="targetver.h" />
  </ItemGroup>
  <ItemGroup>
    <ClCompile Include="SomeProjName.cpp" />
    <ClCompile Include="pch.cpp">
      <PrecompiledHeader Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">Create</PrecompiledHeader>
      <PrecompiledHeader Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">Create</PrecompiledHeader>
    </ClCompile>
  </ItemGroup>
  <Import Project="$(VCTargetsPath)\Microsoft.Cpp.targets" />
  <ImportGroup Label="ExtensionTargets">
  </ImportGroup>
</Project>
```

## <a name="see-also"></a>请参阅

[Visual Studio 项目 - C++](../creating-and-managing-visual-cpp-projects.md)<br>
[在 Visual Studio 中设置 C++ 编译器并生成属性](../working-with-project-properties.md)
