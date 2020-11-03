---
title: '&lt;tuple&gt; 函数'
ms.date: 11/04/2016
f1_keywords:
- tuple/std::get
- tuple/std::make_tuple
- tuple/std::tie
ms.assetid: bc6be38f-5258-4c14-b81b-63caa335fd44
helpviewer_keywords:
- std::get [C++]
- std::make_tuple [C++]
- std::tie [C++]
- std::get [C++]
- std::make_tuple [C++]
- std::tie [C++]
ms.openlocfilehash: 46c386ecffb8fbbf7c07d40b334afd91d261ebcf
ms.sourcegitcommit: 19016630f9d35f365e9ba249e0f3617515d7ca33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92274521"
---
# <a name="lttuplegt-functions"></a>&lt;tuple&gt; 函数

## <a name="apply"></a><a name="apply"></a> 应用

```cpp
template <class F, class Tuple> constexpr decltype(auto) apply(F&& f, Tuple&& t);
```

### <a name="remarks"></a>备注

使用元组*t*调用函数*F* 。

## <a name="forward_as_tuple"></a><a name="forward"></a> forward_as_tuple

```cpp
template <class... TTypes>
    constexpr tuple<TTypes&&...> forward_as_tuple(TTypes&&...) noexcept;
```

### <a name="return-value"></a>返回值

返回 `tuple<TTypes&&...>(std::forward<TTypes>(t)...)`。

### <a name="remarks"></a>备注

构造对 *t* 中的参数的引用元组，该元组适合作为函数的参数转发。

## <a name="get"></a><a name="get"></a> 获取

按索引或类型（在 C++14 中）从 `tuple` 对象获取元素。

```cpp
// by index:
// get reference to Index element of tuple
template <size_t Index, class... Types>
   constexpr tuple_element_t<Index, tuple<Types...>>& get(tuple<Types...>& Tuple) noexcept;

// get const reference to Index element of tuple
template <size_t Index, class... Types>
   constexpr const tuple_element_t<Index, tuple<Types...>>& get(const tuple<Types...>& Tuple) noexcept;

// get rvalue reference to Index element of tuple
template <size_t Index, class... Types>
   constexpr tuple_element_t<Index, tuple<Types...>>&& get(tuple<Types...>&& Tuple) noexcept;

// (C++14) by type:
// get reference to T element of tuple
template <class T, class... Types>
   constexpr T& get(tuple<Types...>& Tuple) noexcept;

// get const reference to T element of tuple
template <class T, class... Types>
   constexpr const T& get(const tuple<Types...>& Tuple) noexcept;

// get rvalue reference to T element of tuple
template <class T, class... Types>
   constexpr T&& get(tuple<Types...>&& Tuple) noexcept;
```

### <a name="parameters"></a>参数

*编入*\
要获取的元素的索引。

*各种*\
元组中声明的序列顺序，按声明顺序排列。

*关心*\
要获取的元素的类型。

*聚合*\
一个 `std::tuple` 包含任意数量的元素的。

### <a name="remarks"></a>备注

模板函数返回对索引 *索引*处的值或对象中的类型 *T* 的引用 `tuple` 。

如果元组中包含的 T 类型元素的个数不为一个，则调用 `get<T>(Tuple)` 将生成编译器错误。

### <a name="example"></a>示例

```cpp
#include <tuple>
#include <iostream>
#include <string>

using namespace std;

int main() {
    tuple<int, double, string> tup(0, 1.42, "Call me Tuple");

    // get elements by index
    cout << " " << get<0>(tup);
    cout << " " << get<1>(tup);
    cout << " " << get<2>(tup) << endl;

    // get elements by type
    cout << " " << get<int>(tup);
    cout << " " << get<double>(tup);
    cout << " " << get<string>(tup) << endl;
}
```

```Output
0 1.42 Call me Tuple
0 1.42 Call me Tuple
```

## <a name="make_from_tuple"></a><a name="make_from_tuple"></a> make_from_tuple

```cpp
template <class T, class Tuple> constexpr T make_from_tuple(Tuple&& t);
```

### <a name="remarks"></a>备注

与 `return make_from_tuple_impl<T>(forward<Tuple>(t), make_index_sequence<tuple_size_v<decay_t<Tuple>>>{})` 相同。

## <a name="make_tuple"></a><a name="make_tuple"></a> make_tuple

从元素值中生成一个 `tuple`。

```cpp
template <class T1, class T2, ..., class TN>
   tuple<V1, V2, ..., VN> make_tuple(const T1& t1, const T2& t2, ..., const TN& tN);
```

### <a name="parameters"></a>参数

*TN*\
第 N 个函数参数的类型。

*tN*\
第 N 个函数参数的值。

### <a name="remarks"></a>备注

模板函数返回 `tuple<V1, V2, ..., VN>(t1, t2, ..., tN)` ，其中每个类型 `Vi` 都是 `X&` 对应的类型 `Ti` `cv` `reference_wrapper<X>` ; 否则为 `Ti` 。

`make_tuple` 的优势之一在于要存储的对象类型由编译器自动确定，而不必显式指定。 使用 `make_tuple<int, int>(1, 2)` 时请不要使用显式模板参数（如 `make_tuple`），因为它冗长而多余并会增加复杂的右值引用问题，可能会导致编译失败。

### <a name="example"></a>示例

```cpp
// std__tuple__make_tuple.cpp
// compile by using: /EHsc
#include <tuple>
#include <iostream>

typedef std::tuple<int, double, int, double> Mytuple;
int main() {
    Mytuple c0(0, 1, 2, 3);

// display contents " 0 1 2 3"
    std::cout << std::get<0>(c0) << " ";
    std::cout << std::get<1>(c0) << " ";
    std::cout << std::get<2>(c0) << " ";
    std::cout << std::get<3>(c0) << std::endl;

    c0 = std::make_tuple(4, 5, 6, 7);

// display contents " 4 5 6 7"
    std::cout << std::get<0>(c0) << " ";
    std::cout << std::get<1>(c0) << " ";
    std::cout << std::get<2>(c0) << " ";
    std::cout << std::get<3>(c0) << std::endl;

    return (0);
}
```

```Output
0 1 2 3
4 5 6 7
```

## <a name="swap"></a><a name="swap"></a> 购

```cpp
template <class... Types>
    void swap(tuple<Types...>& x, tuple<Types...>& y) noexcept(see below );
```

## <a name="tie"></a><a name="tie"></a> 相等

从元素引用中生成一个 `tuple`。

```cpp
template <class T1, class T2, ..., class TN>
tuple<T1&, T2&, ..., TN&> tie(T1& t1, T2& t2, ..., TN& tN);
```

### <a name="parameters"></a>参数

*TN*\
第 N 个元组元素的基类型。

### <a name="remarks"></a>备注

此模板函数返回 `tuple<T1&, T2&, ..., TN&>(t1, t2, ..., tN)`。

### <a name="example"></a>示例

```cpp
// std__tuple__tie.cpp
// compile with: /EHsc
#include <tuple>
#include <iostream>

typedef std::tuple<int, double, int, double> Mytuple;
int main() {
    Mytuple c0(0, 1, 2, 3);

// display contents " 0 1 2 3"
    std::cout << " " << std::get<0>(c0);
    std::cout << " " << std::get<1>(c0);
    std::cout << " " << std::get<2>(c0);
    std::cout << " " << std::get<3>(c0);
    std::cout << std::endl;

    int v4 = 4;
    double v5 = 5;
    int v6 = 6;
    double v7 = 7;
    std::tie(v4, v5, v6, v7) = c0;

// display contents " 0 1 2 3"
    std::cout << " " << v4;
    std::cout << " " << v5;
    std::cout << " " << v6;
    std::cout << " " << v7;
    std::cout << std::endl;

    return (0);
}
```

```Output
0 1 2 3
0 1 2 3
```

## <a name="tuple_cat"></a><a name="tuple_cat"></a> tuple_cat

```cpp
template <class... Tuples> constexpr tuple<CTypes...> tuple_cat(Tuples&&...);
```

### <a name="return-value"></a>返回值

通过初始化每个 type 元素构造的元组对象。

## <a name="tuple_element_t"></a><a name="tuple_element_t"></a> tuple_element_t

```cpp
template <size_t I, class T>
    using tuple_element_t = typename tuple_element<I, T>::type;
```

## <a name="tuple_size_v"></a><a name="tuple_size_v"></a> tuple_size_v

```cpp
template <class T>
    inline constexpr size_t tuple_size_v = tuple_size<T>::value;
```
