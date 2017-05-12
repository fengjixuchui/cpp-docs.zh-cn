---
title: "COLUMN_ENTRY_PS | Microsoft Docs"
ms.custom: ""
ms.date: "11/04/2016"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-cpp"
ms.tgt_pltfrm: ""
ms.topic: "article"
f1_keywords: 
  - "COLUMN_ENTRY_PS"
dev_langs: 
  - "C++"
helpviewer_keywords: 
  - "COLUMN_ENTRY_PS 宏"
ms.assetid: 563c12b0-3376-49d5-a14f-aa68d1e63a7a
caps.latest.revision: 7
author: "mikeblome"
ms.author: "mblome"
manager: "ghogen"
caps.handback.revision: 7
---
# COLUMN_ENTRY_PS
[!INCLUDE[vs2017banner](../../assembler/inline/includes/vs2017banner.md)]

对行集合的绑定到行集合中的特定列。  
  
## 语法  
  
```  
  
COLUMN_ENTRY_PS(  
nOrdinal  
,   
nPrecision  
,   
nScale  
,   
data  
 )  
  
```  
  
#### 参数  
 请参阅在*OLE DB Programmer's Reference* 中 [DBBINDING](https://msdn.microsoft.com/en-us/library/ms716845.aspx)。  
  
 `nOrdinal`  
 \[in\] 列号。  
  
 `nPrecision`  
 \[in\] 要绑定列的最大精度。  
  
 `nScale`  
 \[in\] 要绑定列的小数位数。  
  
 `data`  
 \[in\] 用户记录的相应数据成员。  
  
## 备注  
 允许您指定要绑定列的精度和小数位数。  使用方法如下：  
  
-   在 [BEGIN\_COLUMN\_MAP](../../data/oledb/begin-column-map.md) [END\_COLUMN\_MAP](../../data/oledb/end-column-map.md) 宏和之间。  
  
-   在 [BEGIN\_ACCESSOR](../../data/oledb/begin-accessor.md) [END\_ACCESSOR](../../data/oledb/end-accessor.md) 宏和之间。  
  
-   在 [BEGIN\_PARAM\_MAP](../../data/oledb/begin-param-map.md) [END\_PARAM\_MAP](../../data/oledb/end-param-map.md) 宏和之间。  
  
## 要求  
 **标头:** atldbcli.h  
  
## 请参阅  
 [OLE DB 使用者模板的宏和全局函数](../../data/oledb/macros-and-global-functions-for-ole-db-consumer-templates.md)   
 [BEGIN\_ACCESSOR](../../data/oledb/begin-accessor.md)   
 [BEGIN\_ACCESSOR\_MAP](../../data/oledb/begin-accessor-map.md)   
 [BEGIN\_COLUMN\_MAP](../../data/oledb/begin-column-map.md)   
 [COLUMN\_ENTRY](../../data/oledb/column-entry.md)   
 [COLUMN\_ENTRY\_EX](../../data/oledb/column-entry-ex.md)   
 [COLUMN\_ENTRY\_LENGTH](../../data/oledb/column-entry-length.md)   
 [COLUMN\_ENTRY\_PS\_LENGTH](../../data/oledb/column-entry-ps-length.md)   
 [COLUMN\_ENTRY\_LENGTH\_STATUS](../../data/oledb/column-entry-length-status.md)   
 [COLUMN\_ENTRY\_PS\_LENGTH\_STATUS](../../data/oledb/column-entry-ps-length-status.md)   
 [COLUMN\_ENTRY\_STATUS](../../data/oledb/column-entry-status.md)   
 [COLUMN\_ENTRY\_PS\_STATUS](../../data/oledb/column-entry-ps-status.md)   
 [END\_ACCESSOR](../../data/oledb/end-accessor.md)   
 [END\_ACCESSOR\_MAP](../../data/oledb/end-accessor-map.md)   
 [END\_COLUMN\_MAP](../../data/oledb/end-column-map.md)