---
title: "间接寻址运算符：* | Microsoft Docs"
ms.custom: ""
ms.date: "12/03/2016"
ms.prod: "visual-studio-dev14"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-cpp"
ms.tgt_pltfrm: ""
ms.topic: "language-reference"
dev_langs: 
  - "C++"
helpviewer_keywords: 
  - "* 运算符"
  - "间接寻址运算符"
  - "间接寻址运算符, 语法"
  - "运算符 [C++], 间接寻址"
ms.assetid: c50309e1-6c02-4184-9fcb-2e13c1f4ac03
caps.latest.revision: 7
caps.handback.revision: 7
author: "mikeblome"
ms.author: "mblome"
manager: "ghogen"
---
# 间接寻址运算符：*
[!INCLUDE[vs2017banner](../assembler/inline/includes/vs2017banner.md)]

## 语法  
  
```  
  
* cast-expression  
```  
  
## 备注  
 一元间接寻址运算符 \(**\***\) 取消引用指针；即它将指针值转换为左值。  间接寻址运算符的操作数必须是指向类型的指针。  间接寻址表达式的结果是从中派生指针类型的类型。  此上下文中的 **\*** 运算符的使用与作为二元运算符的意义不同，后者是乘法运算符。  
  
 如果操作数指向函数，则结果是函数指示符。  如果它指向存储位置，则结果是指定存储位置的左值。  
  
 可以累计使用间接寻址运算符来取消引用指向指针的指针。  例如：  
  
```  
// expre_Indirection_Operator.cpp  
// compile with: /EHsc  
// Demonstrate indirection operator  
#include <iostream>  
using namespace std;  
int main() {  
   int n = 5;  
   int *pn = &n;  
   int **ppn = &pn;  
  
   cout  << "Value of n:\n"  
         << "direct value: " << n << endl  
         << "indirect value: " << *pn << endl  
         << "doubly indirect value: " << **ppn << endl  
         << "address of n: " << pn << endl  
         << "address of n via indirection: " << *ppn << endl;  
}  
```  
  
 如果该指针的值无效，则结果是未定义的。  以下列表包含使指针值无效的一些最常见条件。  
  
-   该指针为 null 指针。  
  
-   该指针指定引用时不可见的本地项的地址。  
  
-   该指针指定未针对所指向的对象类型正确对齐的地址。  
  
-   该指针指定执行程序未使用的地址。  
  
## 请参阅  
 [使用一元运算符的表达式](../cpp/expressions-with-unary-operators.md)   
 [C\+\+ 运算符](../misc/cpp-operators.md)   
 [C\+\+ 运算符优先级和关联性](../cpp/cpp-built-in-operators-precedence-and-associativity.md)   
 [Address\-of 运算符：&](../cpp/address-of-operator-amp.md)   
 [间接寻址和 Address\-of 运算符](../c-language/indirection-and-address-of-operators.md)