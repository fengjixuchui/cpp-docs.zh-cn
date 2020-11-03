---
title: tuple_element 类
ms.date: 11/04/2016
f1_keywords:
- utility/std::tuple_element
helpviewer_keywords:
- std::tuple_element
ms.assetid: 4c51a6c1-ce81-462f-8c6c-291d69f2b77c
ms.openlocfilehash: be9d9fe56d35e96e4179eb511edccd475a369f32
ms.sourcegitcommit: 43cee7a0d41a062661229043c2f7cbc6ace17fa3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92008285"
---
# <a name="tuple_element-class"></a>tuple_element 类

包装 `tuple` 元素。 专用化包装 `array` 元素和 `pair` 元素。

## <a name="syntax"></a>语法

```cpp
// CLASS tuple_element (find element by index)
template <size_t Index, class Tuple>
   struct tuple_element;

// tuple_element for const
template <size_t Index, class Tuple>
   struct tuple_element<Index, const Tuple>;

// tuple_element for volatile
template <size_t Index, class Tuple>
   struct tuple_element<Index, volatile Tuple>;

// tuple_element for const volatile
template <size_t Index, class Tuple>
   struct tuple_element<Index, const volatile Tuple>;

// Helper typedef
template <size_t Index, class Tuple>
   using tuple_element_t = typename tuple_element<Index, Tuple>::type;

// Specialization for arrays
template <size_t Index, class Elem, size_t Size>
   struct tuple_element<Index, array<Elem, Size>>;

// Specializations for pairs
// struct to determine type of element 0 in pair
template <class T1, class T2>
   struct tuple_element<0, pair<T1, T2>>;

// struct to determine type of element 1 in pair
template <class T1, class T2>
   struct tuple_element<1, pair<T1, T2>>;
```

### <a name="parameters"></a>parameters

*编入*\
指定元素的索引。

*聚合*\
元组的类型。

*Elem*\
数组元素的类型。

*规格*\
数组大小。

*T1*\
对中第一个元素的类型。

*T2*\
一对中第二个元素的类型。

## <a name="remarks"></a>备注

类模板 `tuple_element` 具有嵌套的 typedef `type` ，它是元组类型*元组*的索引*索引*处的类型的同义词。

此 typedef `tuple_element_t` 是 `tuple_element<Index, Tuple>::type` 的方便别名。

数组的类模板专用化提供了一个接口 `array` ，用作元素的元组 `Size` ，其中每个元素具有相同的类型。 每个专用化都有一个嵌套的 typedef， `type` 它是的 *索引* 元素类型的同义词 `array` ，保留了任何常量可变的限定。

每个 `pair` 类型的模板专用化都具有一个成员 typedef `type`，它是对中指定位置处元素类型的同义词，保留有任何恒定和/或可变的限定。 此 typedef `tuple_element_t` 是 `tuple_element<N, pair<T1, T2>>::type` 的方便别名。

使用[Get Function &lt; 实用工具 &gt; ](../standard-library/utility-functions.md#get)返回指定位置或指定类型的元素。

## <a name="example-get-an-element-from-a-tuple"></a>示例：从元组获取元素

```cpp
#include <tuple>
#include <string>
#include <iostream>

using namespace std;
typedef tuple<int, double, string> MyTuple;

int main() {
    MyTuple c0{ 0, 1.5, "Tail" };

    tuple_element_t<0, MyTuple> val = get<0>(c0); //get by index
    tuple_element_t<1, MyTuple> val2 = get<1>(c0);
    tuple_element_t<2, MyTuple> val3 = get<string>(c0); // get by type

    cout << val << " " << val2 << " " << val3 << endl;
}
```

```Output
0 1.5 Tail
```

## <a name="example-get-an-element-from-an-array"></a>示例：从数组获取元素

```cpp
#include <array>
#include <iostream>

using namespace std;
typedef array<int, 4> MyArray;

int main()
{
    MyArray c0 { 0, 1, 2, 3 };

    for (const auto& e : c0)
    {
        cout << e << " ";
    }
    cout << endl;

    // display first element "0"
    tuple_element<0, MyArray>::type val = c0.front();
    cout << val << endl;
}
```

```Output
0 1 2 3
0
```

## <a name="example-get-an-element-from-a-pair"></a>示例：从成对获取元素

```cpp
#include <utility>
#include <iostream>

using namespace std;

typedef pair<int, double> MyPair;
int main() {
    MyPair c0(0, 1.333);

    // display contents "0 1"
    cout << get<0>(c0) << " ";
    cout << get<1>(c0) << endl;

    // display first element "0 " by index
    tuple_element<0, MyPair>::type val = get<0>(c0);
    cout << val << " ";

    // display second element by type
    tuple_element<1, MyPair>::type val2 = get<double>(c0);
    cout << val2 << endl;
}
```

```Output
0 1.333
0 1.333
```

## <a name="requirements"></a>要求

**标头：**\<tuple>

**标头：** \<array> 数组专用化的 () 

**标头：** \<utility> 对专用化的 () 

**命名空间:** std
