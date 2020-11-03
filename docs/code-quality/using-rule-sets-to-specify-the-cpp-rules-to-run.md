---
title: 使用规则集指定要运行的 C++ 规则
ms.date: 07/27/2020
ms.topic: conceptual
f1_keywords:
- vs.codeanalysis.rulesets.native
ms.openlocfilehash: 53e1122cd1aac74401ca956ee24ebcdf79883228
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92921901"
---
# <a name="use-rule-sets-to-specify-the-c-rules-to-run"></a>使用规则集指定要运行的 C++ 规则

在 Visual Studio 中，你可以创建和修改自定义 *规则集* ，以满足与代码分析相关的特定项目需求。 默认规则集存储在中 *`%VSINSTALLDIR%\Team Tools\Static Analysis Tools\Rule Sets`* 。

**Visual Studio 2017 版本15.7 及更高版本：** 你可以使用任何文本编辑器创建自定义规则集，并在命令行生成中应用它们，无论你使用的是何种生成系统。 有关详细信息，请参阅 [`/analyze:ruleset`](../build/reference/analyze-code-analysis.md)。

若要在 Visual Studio 中创建自定义 c + + 规则集，必须在 Visual Studio IDE 中打开 C/c + + 项目。 然后在规则集编辑器中打开标准规则集，然后添加或删除特定规则，并根据需要更改在代码分析确定违反规则时发生的操作。

若要创建新的自定义规则集，请使用新文件名进行保存。 自定义规则集会自动分配给项目。

## <a name="to-create-a-custom-rule-from-a-single-existing-rule-set"></a>基于单个现有规则集创建自定义规则

::: moniker range="<=msvc-150"

1. 在解决方案资源管理器中，打开项目的快捷菜单，然后选择 " **属性** "。

1. 在 " **属性页** " 对话框中，选择 **配置属性** > **代码分析** > **常规** 属性页。

1. 在 " **规则集** " 下拉列表中，执行下列操作之一：

   - 选择要自定义的规则集。

     \- 或 -

   - 选择 **\<Browse...>** 指定列表中不存在的现有规则集。

1. 选择 " **打开** " 以在规则集编辑器中显示规则。

::: moniker-end
::: moniker range=">=msvc-160"

1. 在解决方案资源管理器中，打开项目的快捷菜单，然后选择 " **属性** "。

1. 在 " **属性页** " 对话框中，选择 **配置属性** > **代码分析** > **Microsoft** 属性页。

1. 在 " **活动规则** " 下拉列表中，执行下列操作之一：

   - 选择要自定义的规则集。

     \- 或 -

   - 选择 **\<Browse...>** 指定列表中不存在的现有规则集。

1. 选择 " **打开** " 以在规则集编辑器中显示规则。

::: moniker-end

## <a name="to-modify-a-rule-set-in-the-rule-set-editor"></a>在规则集编辑器中修改规则集

- 若要更改规则集的显示名称，请在 " **视图** " 菜单上选择 " **属性窗口** "。 在 " **名称** " 框中输入显示名称。 请注意，显示名称可能不同于文件名。

- 若要将组的所有规则添加到自定义规则集，请选中组的复选框。 若要删除组中的所有规则，请清除该复选框。

- 若要向自定义规则集添加特定规则，请选中该规则的复选框。 若要从规则集中删除规则，请清除该复选框。

- 若要更改在代码分析中违反规则时采取的操作，请选择该规则的 " **操作** " 字段，然后选择以下值之一：

     **警告** -生成警告。

     **错误** -生成错误。

     **Info** -生成一条消息。

     **无** -禁用规则。 此操作与从规则集中删除规则的操作相同。

## <a name="to-group-filter-or-change-the-fields-in-the-rule-set-editor-by-using-the-rule-set-editor-toolbar"></a>使用规则集编辑器工具栏对规则集编辑器中的字段进行分组、筛选或更改

- 若要展开所有组中的规则，请选择 " **全部展开** "。

- 若要折叠所有组中的规则，请选择 " **全部折叠** "。

- 若要更改对规则进行分组所依据的字段，请从 " **分组依据** " 列表中选择字段。 若要显示未分组的规则，请选择 **\<None>** 。

- 若要在规则列中添加或删除字段，请选择 **列选项** 。

- 若要隐藏不适用于当前解决方案的规则，请选择 **"隐藏不适用于当前解决方案的规则** "。

- 若要切换显示和隐藏分配了 "错误" 操作的规则，请选择 " **显示可以生成代码分析错误的规则** "。

- 若要切换显示和隐藏分配了 "警告" 操作的规则，请选择 " **显示可以生成代码分析警告的规则** "。

- 若要切换显示和隐藏分配了 " **无** " 操作的规则，请选择 " **显示未启用的规则** "。

- 若要添加或删除当前规则集的 Microsoft 默认规则集，请选择 " **添加或删除子规则集** "。

## <a name="to-create-a-rule-set-in-a-text-editor"></a>在文本编辑器中创建规则集

您可以在文本编辑器中创建自定义规则集，将其存储在具有扩展名的任何位置， *`.ruleset`* 并使用 [`/analyze:ruleset`](../build/reference/analyze-code-analysis.md) 编译器选项应用。

下面的示例演示了一个基本规则集文件，你可以将其用作起始点：

```xml
<?xml version="1.0" encoding="utf-8"?>
<RuleSet Name="New Rule Set" Description="New rules to apply." ToolsVersion="10.0">
  <Rules AnalyzerId="Microsoft.Analyzers.NativeCodeAnalysis" RuleNamespace="Microsoft.Rules.Native">
    <Rule Id="C6001" Action="Warning" />
    <Rule Id="C26494" Action="Warning" />
  </Rules>
</RuleSet>
```

## <a name="ruleset-schema"></a>规则集架构

下面的规则集架构描述了规则集文件的 XML 架构。 规则集架构存储在中 *`%VSINSTALLDIR%\Team Tools\Static Analysis Tools\Schemas\RuleSet.xsd`* 。 你可以使用它以编程方式创作自己的规则集，或验证自定义规则集是否符合正确格式。 有关详细信息，请参阅 [如何：基于 XSD 架构创建 XML 文档](/visualstudio/xml-tools/how-to-create-an-xml-document-based-on-an-xsd-schema)。

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">

  <xs:annotation>
    <xs:documentation xml:lang="en">
            Visual Studio Code Analysis Rule Set Schema Definition Language.
            Copyright (c) Microsoft Corporation. All rights reserved.
        </xs:documentation>
  </xs:annotation>

  <!-- Every time this file changes, be sure to change the Validate method for the corresponding object in the code -->

  <xs:element name="RuleSet" type="TRuleSet">
  </xs:element>

  <xs:complexType name="TLocalization">
    <xs:all>
      <xs:element name="Name" type="TName" minOccurs="0" maxOccurs="1" />
      <xs:element name="Description" type="TDescription" minOccurs="0" maxOccurs="1" />
    </xs:all>
    <xs:attribute name="ResourceAssembly" type="TNonEmptyString" use="required" />
    <xs:attribute name="ResourceBaseName" type="TNonEmptyString" use="required" />
  </xs:complexType>

  <xs:complexType name="TRuleHintPaths">
    <xs:sequence>
      <xs:element name="Path" type="TNonEmptyString" minOccurs="0" maxOccurs="unbounded" />
    </xs:sequence>
  </xs:complexType>
  
  <xs:complexType name="TName">
    <xs:attribute name="Resource" type="TNonEmptyString" use="required" />
  </xs:complexType>

  <xs:complexType name="TDescription">
    <xs:attribute name="Resource" type="TNonEmptyString" use="required" />
  </xs:complexType>

  <xs:complexType name="TInclude">
    <xs:attribute name="Path" type="TNonEmptyString" use="required" />
    <xs:attribute name="Action" type="TIncludeAction" use="required" />
  </xs:complexType>

  <xs:complexType name="TIncludeAll">
    <xs:attribute name="Action" type="TIncludeAllAction" use="required" />
  </xs:complexType>

  <xs:complexType name="TRule">
    <xs:attribute name="Id" type="TNonEmptyString" use="required" />
    <xs:attribute name="Action" type="TRuleAction" use="required" />
  </xs:complexType>

  <xs:complexType name="TRules">
    <xs:sequence>
      <xs:element name="Rule" type="TRule" minOccurs="0" maxOccurs="unbounded" />
    </xs:sequence>
    <xs:attribute name="AnalyzerId" type="TNonEmptyString" use="required" />
    <xs:attribute name="RuleNamespace" type="TNonEmptyString" use="required" />
  </xs:complexType>

  <xs:complexType name="TRuleSet">
    <xs:sequence minOccurs="0" maxOccurs="1">
      <xs:element name="Localization" type="TLocalization" minOccurs="0" maxOccurs="1" />
      <xs:element name="RuleHintPaths" type="TRuleHintPaths" minOccurs="0" maxOccurs="1" />
      <xs:element name="IncludeAll" type="TIncludeAll" minOccurs="0" maxOccurs="1" />
      <xs:choice minOccurs="0" maxOccurs="unbounded">
        <xs:element name="Include" type="TInclude" minOccurs="0" maxOccurs="unbounded" />
        <xs:element name="Rules" type="TRules" minOccurs="0" maxOccurs="unbounded">
          <xs:unique name="UniqueRuleName">
            <xs:selector xpath="Rule" />
            <xs:field xpath="@Id" />
          </xs:unique>
        </xs:element>
      </xs:choice>
    </xs:sequence>
    <xs:attribute name="Name" type="TNonEmptyString" use="required" />
    <xs:attribute name="Description" type="xs:string" use="optional" />
    <xs:attribute name="ToolsVersion" type="TNonEmptyString" use="required" />
  </xs:complexType>

  <xs:simpleType name="TRuleAction">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Error"/>
      <xs:enumeration value="Warning"/>
      <xs:enumeration value="Info"/>
      <xs:enumeration value="Hidden"/>
      <xs:enumeration value="None"/>
    </xs:restriction>
  </xs:simpleType>

  <xs:simpleType name="TIncludeAction">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Error"/>
      <xs:enumeration value="Warning"/>
      <xs:enumeration value="Info"/>
      <xs:enumeration value="Hidden"/>
      <xs:enumeration value="None"/>
      <xs:enumeration value="Default"/>
    </xs:restriction>
  </xs:simpleType>

  <xs:simpleType name="TIncludeAllAction">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Error"/>
      <xs:enumeration value="Warning"/>
      <xs:enumeration value="Info"/>
      <xs:enumeration value="Hidden"/>
    </xs:restriction>
  </xs:simpleType>

  <xs:simpleType name="TNonEmptyString">
    <xs:restriction base="xs:string">
      <xs:minLength value="1" />
    </xs:restriction>
  </xs:simpleType>
  
</xs:schema>

```

架构元素详细信息：

| 架构元素 | 说明 |
|--------------------|--------------|
| `TLocalization` | 本地化信息包括规则集文件的名称、规则集文件的说明、包含本地化资源的资源程序集的名称以及本地化资源的基名称 |
| `TRuleHintPaths` | 用作提示以搜索规则集文件的文件路径 |
| `TName` | 当前规则集文件的名称 |
| `TDescription` | 当前规则集文件的说明 |
| `TInclude` | 包含规则操作的包含规则集的路径 |
| `TIncludeAll` | 所有规则的规则操作 |
| `TRule` | 规则 ID 与规则操作 |
| `TRules` | 一个或多个规则的集合 |
| `TRuleSet` | 规则集文件格式由本地化信息、规则提示路径、包含所有信息、包括信息、规则信息、名称、说明和工具版本信息组成 |
| `TRuleAction` | 描述规则操作（如错误、警告、信息、隐藏或无）的枚举 |
| `TIncludeAction` | 描述规则操作（如错误、警告、信息、隐藏、无或默认）的枚举 |
| `TIncludeAllAction` | 描述规则操作（如错误、警告、信息或隐藏）的枚举 |

若要查看规则集的示例，请参阅 [在文本编辑器中创建规则集](#to-create-a-rule-set-in-a-text-editor)或存储在中的任何默认规则集 `%VSINSTALLDIR%\Team Tools\Static Analysis Tools\Rule Sets` 。
