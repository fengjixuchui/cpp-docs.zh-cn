---
title: "is_trivially_copy_constructible 类 | Microsoft Docs"
ms.custom: 
ms.date: 11/04/2016
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-cpp
ms.tgt_pltfrm: 
ms.topic: article
f1_keywords:
- is_trivially_copy_constructible
- type_traits/std::is_trivially_copy_constructible
dev_langs:
- C++
helpviewer_keywords:
- is_trivially_copy_constructible
ms.assetid: 4274cef5-afdd-4f2d-bc83-7562e7944ddf
caps.latest.revision: 24
author: corob-msft
ms.author: corob
manager: ghogen
translation.priority.mt:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
translationtype: Machine Translation
ms.sourcegitcommit: 51fbd09793071631985720550007dddbe16f598f
ms.openlocfilehash: a9f40518942be5632f13dee3865b603f04cbca84
ms.lasthandoff: 02/24/2017

---
# <a name="istriviallycopyconstructible-class"></a>is_trivially_copy_constructible 类
测试类型是否包含普通复制构造函数。  
  
## <a name="syntax"></a>语法  
  
```
template <class T>
struct is_trivially_copy_constructible;
```  
  
#### <a name="parameters"></a>参数  
 `T`  
 要查询的类型。  
  
## <a name="remarks"></a>备注  
 如果类型 `T` 是具有普通复制构造函数的类，则类型谓词的实例为 true；否则为 false。  
  
 如果类 `T` 的复制构造函数经隐式声明，则为普通类，类 `T` 不具有虚拟函数或虚拟基，类 `T` 的所有直接基具有普通复制构造函数，类类型的所有非静态数据成员的类都具有普通复制构造函数，且类的类型数组的所有非静态数据成员的类具有普通复制构造函数。  
  
## <a name="requirements"></a>要求  
 **标头：**\<type_traits>  
  
 **命名空间：** std  
  
## <a name="see-also"></a>另请参阅  
 [<type_traits>](../standard-library/type-traits.md)



