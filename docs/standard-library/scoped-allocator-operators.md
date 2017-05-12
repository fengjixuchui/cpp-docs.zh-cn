---
title: "&lt;scoped_allocator&gt; 运算符 | Microsoft Docs"
ms.custom: 
ms.date: 11/04/2016
ms.reviewer: 
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: article
f1_keywords: []
ms.assetid: 4dfe0805-cc6e-479f-887f-a1c164f73837
caps.latest.revision: 10
manager: ghogen
translationtype: Machine Translation
ms.sourcegitcommit: 3168772cbb7e8127523bc2fc2da5cc9b4f59beb8
ms.openlocfilehash: c8b64ba6276ecfa1078faf3b75a3cf36e9fc5072
ms.lasthandoff: 02/24/2017

---
# <a name="ltscopedallocatorgt-operators"></a>&lt;scoped_allocator&gt; 运算符
|||  
|-|-|  
|[operator!=](#operator_neq)|[operator==](#operator_eq_eq)|  
  
##  <a name="operator_neq"></a>operator!=  
 测试两个 `scoped_allocator_adaptor` 对象是否不相等。  
  
```cpp  
template <class Outer, class... Inner>  
bool operator!=(
    const scoped_allocator_adaptor<Outer, Inner...>& left,  
    const scoped_allocator_adaptor<Outer, Inner...>& right) noexcept;  
```  
  
### <a name="parameters"></a>参数  
 `left`  
 左 `scoped_allocator_adaptor` 对象。  
  
 `right`  
 正确的 `scoped_allocator_adaptor` 对象。  
  
### <a name="return-value"></a>返回值  
 `!(left == right)`  
  
##  <a name="operator_eq_eq"></a>operator==  
 测试两个 `scoped_allocator_adaptor` 对象是否相等。  
  
```cpp  
template <class Outer, class... Inner>  
bool operator==(
    const scoped_allocator_adaptor<Outer, Inner...>& left,  
    const scoped_allocator_adaptor<Outer, Inner...>& right) noexcept;  
```  
  
### <a name="parameters"></a>参数  
 `left`  
 左 `scoped_allocator_adaptor` 对象。  
  
 `right`  
 正确的 `scoped_allocator_adaptor` 对象。  
  
### <a name="return-value"></a>返回值  
 `left.outer_allocator() == right.outer_allocator() && left.inner_allocator() == right.inner_allocator()`  
  
## <a name="see-also"></a>另请参阅  
 [<scoped_allocator>](../standard-library/scoped-allocator.md)

