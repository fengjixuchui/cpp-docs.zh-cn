---
title: "从类型库添加类向导 | Microsoft Docs"
ms.custom: ""
ms.date: "11/04/2016"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-cpp"
ms.tgt_pltfrm: ""
ms.topic: "article"
f1_keywords: 
  - "vc.codewiz.class.typelib"
dev_langs: 
  - "C++"
helpviewer_keywords: 
  - "从类型库向导添加类 [C++]"
  - "COM 接口, 添加类"
ms.assetid: 96152afd-9374-4649-a6ab-b0fa2a5592a3
caps.latest.revision: 10
author: "mikeblome"
ms.author: "mblome"
manager: "ghogen"
caps.handback.revision: 10
---
# 从类型库添加类向导
[!INCLUDE[vs2017banner](../../assembler/inline/includes/vs2017banner.md)]

使用该向导从可用的类型库添加 MFC 类。  该向导为从选定的类型库添加的每个接口创建类。  
  
 **从以下来源添加类：**  
 指定类型库的位置，将从该位置创建类。  
  
|选项|说明|  
|--------|--------|  
|**注册表**|类型库已在系统中注册。  已注册的类型库在“可用的类型库”中列出。|  
|**文件**|类型库不一定在系统中注册，但包含在文件中。  必须在“位置”中提供文件的位置。|  
  
 **可用的类型库**  
 列出系统中当前注册的类型库。  从此列表中选定一个类型库，在“接口”列表中显示它的接口。  
  
 有关注册类型库的更多信息，请参见 MSDN Library 中的“分布式 COM 的内部：类型库和语言集成”。  
  
 **位置**  
 指定类型库的位置。  如果单击**“从以下来源添加类”**下的**“文件”**，则可以提供包含类型库的文件的位置。  若要浏览文件的位置，请单击省略号按钮。  
  
 **接口**  
 列出“可用的类型库”列表中当前选定的类型库中的接口。  
  
|传输按钮|说明|  
|----------|--------|  
|**\>**|添加“接口”列表中当前选定的接口。  如果没有选定接口则无效。|  
|**\>\>**|添加“可用的类型库”列表中当前选定的类型库中的所有接口。|  
|**\<**|移除“生成的类”列表中当前选定的类。  如果“生成的类”列表中没有当前选定的类则无效。|  
|**\<\<**|移除“生成的类”列表中的所有类。  如果“生成的类”列表为空则无效。|  
  
 **生成的类**  
 指定要从使用 **\>** 或**\>\>** 按钮添加的接口生成的类名。  可以单击此框选择类，然后使用向上或向下键在列表中滚动，查看单击“完成”时，向导在“类”\(`Class`\) 框中生成的每个类名和在“文件”框中生成的每个文件名。  在该框中，一次只能选择一个类。  
  
 在该列表中选择一个类并单击 **\<**可以移除该类。  不需要通过在“生成的类”框中选定类来移除所有的类；通过单击**\<\<**, 便可以移除“生成的类”框中的所有类**Generated classes** 。  
  
 `Class`  
 指定在单击“完成”时向导添加的“生成的类”框中选定的类名。  可以在“类”\(`Class`\) 框中编辑该名称。  
  
 **文件**  
 设置新类的头文件名。  默认情况下，此名称基于在“生成的类”中提供的名称。  单击省略号按钮将该文件名保存到所选位置，或将类声明追加到现有文件。  如果选择现有文件，则直到在向导中单击“完成”时，向导才将其保存到所选位置。  
  
 向导不覆盖文件。  如果选择现有文件的名称，则单击“完成”时，向导会提示您指出是否应向该文件的内容中追加类声明。  单击**“是”**追加该文件；单击**“否”**返回到向导并指定另一个文件名。  
  
## 请参阅  
 [类型库中的 MFC 类](../../mfc/reference/adding-an-mfc-class-from-a-type-library.md)   
 [自动化客户端：使用类型库](../../mfc/automation-clients-using-type-libraries.md)