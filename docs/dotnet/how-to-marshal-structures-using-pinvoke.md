---
title: 如何：使用 PInvoke 封送结构
ms.custom: get-started-article
ms.date: 11/04/2016
helpviewer_keywords:
- data marshaling [C++], structures
- platform invoke [C++], structures
- interop [C++], structures
- marshaling [C++], structures
ms.assetid: 35997e6f-9251-4af3-8c6e-0712d64d6a5d
ms.openlocfilehash: e132505ef536a79c67afdd76443c2637c08f923b
ms.sourcegitcommit: 43cee7a0d41a062661229043c2f7cbc6ace17fa3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92008755"
---
# <a name="how-to-marshal-structures-using-pinvoke"></a>如何：使用 PInvoke 封送结构

本文档说明如何通过使用 P/Invoke 从托管函数调用接受 C 样式结构的本机函数。 尽管我们建议你使用 c + + 互操作功能而不是 P/Invoke，因为 P/Invoke 提供少量的编译时错误报告、不是类型安全的，如果非托管 API 打包为 DLL 并且源代码不可用，则 P/Invoke 是唯一的选择。 否则，请参阅以下文档：

- [使用 c + + 互操作 (隐式 PInvoke) ](../dotnet/using-cpp-interop-implicit-pinvoke.md)

- [如何：使用 PInvoke 封送字符串](../dotnet/how-to-marshal-strings-using-pinvoke.md)

默认情况下，本机和托管结构在内存中的布局方式不同，因此成功地跨托管/非托管边界传递结构需要额外的步骤来保持数据的完整性。

本文档说明定义本机结构的托管等效项所需的步骤，以及生成的结构如何传递到非托管函数。 本文档假定使用简单结构（不包含字符串或指针的结构）。 有关非直接实现互操作性的信息，请参阅 [使用 c + + 互操作 (隐式 PInvoke) ](../dotnet/using-cpp-interop-implicit-pinvoke.md)。 P/Invoke 不能将非直接复制到本机类型作为返回值。 直接复制类型在托管和非托管代码中具有相同的表示形式。 有关详细信息，请参阅 [本机和非直接复制类型](/dotnet/framework/interop/blittable-and-non-blittable-types)。

封送处理跨托管/非托管边界的简单直接复制结构需要定义每个本机结构的托管版本。 这些结构可以具有任何合法名称;除了其数据布局外，两个结构的本机版本和托管版本之间没有关系。 因此，托管版本包含的字段大小与本机版本相同，这一点非常重要。  (没有任何机制可确保结构的托管版本和本机版本都是等效的，因此在运行时不会明显导致不兼容。 编程人员应负责确保两个结构具有相同的数据布局。 ) 

由于出于性能方面的考虑，托管结构的成员有时会重新排列，因此，需要使用 <xref:System.Runtime.InteropServices.StructLayoutAttribute> 属性指示结构按顺序排列。 将结构打包设置显式设置为与本机结构所使用的设置相同。  (尽管默认情况下，Visual C++ 对这两个托管代码使用8字节结构打包。 ) 

1. 接下来，使用 <xref:System.Runtime.InteropServices.DllImportAttribute> 声明与任何接受该结构的非托管函数相对应的入口点，但在函数签名中使用结构的托管版本，如果对这两个版本的结构使用同一名称，则这是一个差异点。

1. 现在，托管代码可以将结构的托管版本传递给非托管函数，就好像它们实际上是托管函数。 可以通过值或引用传递这些结构，如以下示例中所示。

## <a name="unmanaged-and-a-managed-modules"></a>非托管模块和托管模块

下面的代码由一个非托管模块和一个托管模块组成。 非托管模块是一个 DLL，它定义了一个名为 Location 的结构和一个名为 GetDistance 的函数，该函数接受 Location 结构的两个实例。 第二个模块是一个托管命令行应用程序，它导入 GetDistance 函数，但根据位置结构 MLocation 的托管等效项进行定义。 在实践中，同一名称可能同时用于这两种版本的结构：不过，此处使用了不同的名称来说明 DllImport 原型是根据托管版本定义的。

请注意，不会向托管代码公开使用传统 #include 指令的任何部分。 事实上，DLL 仅在运行时进行访问，因此在编译时不会检测到用 DllImport 导入的函数的问题。

### <a name="example-unmanaged-dll-module"></a>示例：非托管 DLL 模块

```cpp
// TraditionalDll3.cpp
// compile with: /LD /EHsc
#include <iostream>
#include <stdio.h>
#include <math.h>

#define TRADITIONALDLL_EXPORTS
#ifdef TRADITIONALDLL_EXPORTS
   #define TRADITIONALDLL_API __declspec(dllexport)
#else
   #define TRADITIONALDLL_API __declspec(dllimport)
#endif

#pragma pack(push, 8)
struct Location {
   int x;
   int y;
};
#pragma pack(pop)

extern "C" {
   TRADITIONALDLL_API double GetDistance(Location, Location);
   TRADITIONALDLL_API void InitLocation(Location*);
}

double GetDistance(Location loc1, Location loc2) {
   printf_s("[unmanaged] loc1(%d,%d)", loc1.x, loc1.y);
   printf_s(" loc2(%d,%d)\n", loc2.x, loc2.y);

   double h = loc1.x - loc2.x;
   double v = loc1.y = loc2.y;
   double dist = sqrt( pow(h,2) + pow(v,2) );

   return dist;
}

void InitLocation(Location* lp) {
   printf_s("[unmanaged] Initializing location...\n");
   lp->x = 50;
   lp->y = 50;
}
```

### <a name="example-managed-command-line-application-module"></a>示例：托管命令行应用程序模块

```cpp
// MarshalStruct_pi.cpp
// compile with: /clr
using namespace System;
using namespace System::Runtime::InteropServices;

[StructLayout(LayoutKind::Sequential, Pack=8)]
value struct MLocation {
   int x;
   int y;
};

value struct TraditionalDLL {
   [DllImport("TraditionalDLL3.dll")]
   static public double GetDistance(MLocation, MLocation);
   [DllImport("TraditionalDLL3.dll")]
   static public double InitLocation(MLocation*);
};

int main() {
   MLocation loc1;
   loc1.x = 0;
   loc1.y = 0;

   MLocation loc2;
   loc2.x = 100;
   loc2.y = 100;

   double dist = TraditionalDLL::GetDistance(loc1, loc2);
   Console::WriteLine("[managed] distance = {0}", dist);

   MLocation loc3;
   TraditionalDLL::InitLocation(&loc3);
   Console::WriteLine("[managed] x={0} y={1}", loc3.x, loc3.y);
}
```

```Output
[unmanaged] loc1(0,0) loc2(100,100)
[managed] distance = 141.42135623731
[unmanaged] Initializing location...
[managed] x=50 y=50
```

## <a name="see-also"></a>请参阅

[在 c + + 中使用显式 PInvoke (DllImport 特性) ](../dotnet/using-explicit-pinvoke-in-cpp-dllimport-attribute.md)
