---
title: utility (STL/CLR)
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
- <cliext/utility>
- cliext::pair
- cliext::pair::pair
- cliext::pair::first
- cliext::pair::first_type
- cliext::pair::second
- cliext::pair::second_type
- cliext::pair::swap
- cliext::make_pair
- cliext::pair::operator=
- cliext::pair::operator==
- cliext::pair::operator>=
- cliext::pair::operator>
- cliext::pair::operator!=
- cliext::pair::operator<=
- cliext::pair::operator<
helpviewer_keywords:
- <utility> header [STL/CLR]
- utility header [STL/CLR]
- <cliext/utility> header [STL/CLR]
- first member [STL/CLR]
- first_type member [STL/CLR]
- second member [STL/CLR]
- second_type member [STL/CLR]
- swap member [STL/CLR]
- make_pair function [STL/CLR]
- pair class [STL/CLR]
- pair member [STL/CLR]
- operator== member [STL/CLR]
- operator= member [STL/CLR]
- operator>= member [STL/CLR]
- operator> member [STL/CLR]
- operator!= member [STL/CLR]
- operator<= member [STL/CLR]
- operator< member [STL/CLR]
ms.assetid: fb48cb75-d5ef-47ce-b526-bf60dc86c552
ms.openlocfilehash: faf7f607f9433fa3e4813957b24220a5e66e1e49
ms.sourcegitcommit: 43cee7a0d41a062661229043c2f7cbc6ace17fa3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92008617"
---
# <a name="utility-stlclr"></a>utility (STL/CLR)

包含 STL/CLR 标头 `<cliext/utility>` 以定义模板类 `pair` 和几个支持模板函数。

## <a name="syntax"></a>语法

```cpp
#include <utility>
```

## <a name="requirements"></a>要求

**标头：**\<cliext/utility>

**命名空间：** cliext

## <a name="declarations"></a>声明

|类|说明|
|-----------|-----------------|
|[pair (STL/CLR)](#pair)|包装一对元素。|

|运算符|说明|
|--------------|-----------------|
|[operator== (pair) (STL/CLR)](#op_eq)|配对相等比较。|
|[operator！ = (对)  (STL/CLR) ](#op_neq)|对不相等比较。|
|[运算符< (对)  (STL/CLR) ](#op_lt)|对小于比较。|
|[operator \< = (对)  (STL/CLR) ](#op_lteq)|对小于或等于比较。|
|[运算符> (对)  (STL/CLR) ](#op_gt)|配对大于比较。|
|[运算符>= (对)  (STL/CLR) ](#op_gteq)|大于或等于比较。|

|函数|说明|
|--------------|-----------------|
|[make_pair (STL/CLR)](#make_pair)|从一对值进行配对。|

## <a name="pair-stlclr"></a><a name="pair"></a> 配对 (STL/CLR) 

此模板类描述包装值对的对象。

### <a name="syntax"></a>语法

```cpp
template<typename Value1,
    typename Value2>
    ref class pair;
```

#### <a name="parameters"></a>parameters

*Value1*<br/>
第一个已包装值的类型。

*Value2*<br/>
第二个包装值的类型。

## <a name="members"></a>成员

|类型定义|说明|
|---------------------|-----------------|
|[pair::first_type (STL/CLR)](#first_type)|第一个已包装值的类型。|
|[pair::second_type (STL/CLR)](#second_type)|第二个包装值的类型。|

|Member 对象|说明|
|-------------------|-----------------|
|[pair::first (STL/CLR)](#first)|第一个存储的值。|
|[pair::second (STL/CLR)](#second)|第二个存储的值。|

|成员函数|说明|
|---------------------|-----------------|
|[pair::pair (STL/CLR)](#pair_pair)|构造一个对对象。|
|[pair::swap (STL/CLR)](#swap)|交换两对的内容。|

|运算符|说明|
|--------------|-----------------|
|[pair::operator= (STL/CLR)](#op_as)|替换存储的值对。|

## <a name="remarks"></a>备注

对象存储一对值。 使用此模板类将两个值组合到单个对象中。 此外， `cliext::pair` 此处 (描述的对象) 仅存储托管类型; 用于存储在中声明的一对非托管类型 `std::pair` `<utility>` 。

## <a name="pairfirst-stlclr"></a><a name="first"></a> 对：： first (STL/CLR) 

第一个换行值。

### <a name="syntax"></a>语法

```cpp
Value1 first;
```

### <a name="remarks"></a>备注

对象存储第一个已包装的值。

### <a name="example"></a>示例

```cpp
// cliext_pair_first.cpp
// compile with: /clr
#include <cliext/utility>

int main()
    {
    cliext::pair<wchar_t, int> c1(L'x', 3);
    System::Console::WriteLine("[{0}, {1}]", c1.first, c1.second);

    cliext::pair<wchar_t, int>::first_type first_val = c1.first;
    cliext::pair<wchar_t, int>::second_type second_val = c1.second;
    System::Console::WriteLine("[{0}, {1}]", first_val, second_val);
    return (0);
    }
```

```Output
[x, 3]
```

## <a name="pairfirst_type-stlclr"></a><a name="first_type"></a> 配对：： first_type (STL/CLR) 

第一个已包装值的类型。

### <a name="syntax"></a>语法

```cpp
typedef Value1 first_type;
```

### <a name="remarks"></a>备注

该类型是模板参数 *Value1*的同义词。

### <a name="example"></a>示例

```cpp
// cliext_pair_first_type.cpp
// compile with: /clr
#include <cliext/utility>

int main()
    {
    cliext::pair<wchar_t, int> c1(L'x', 3);
    System::Console::WriteLine("[{0}, {1}]", c1.first, c1.second);

    cliext::pair<wchar_t, int>::first_type first_val = c1.first;
    cliext::pair<wchar_t, int>::second_type second_val = c1.second;
    System::Console::WriteLine("[{0}, {1}]", first_val, second_val);
    return (0);
    }
```

```Output
[x, 3]
```

## <a name="pairoperator-stlclr"></a><a name="op_as"></a> 对：： operator = (STL/CLR) 

替换存储的值对。

### <a name="syntax"></a>语法

```cpp
pair<Value1, Value2>% operator=(pair<Value1, Value2>% right);
```

#### <a name="parameters"></a>parameters

*然后*<br/>
要复制的配对。

### <a name="remarks"></a>备注

成员运算符 *直接* 复制到对象，然后返回 **`*this`** 。 使用此方法可以将存储的值对替换为 *右侧*存储的值对的副本。

### <a name="example"></a>示例

```cpp
// cliext_pair_operator_as.cpp
// compile with: /clr
#include <cliext/utility>

int main()
    {
    cliext::pair<wchar_t, int> c1(L'x', 3);
    System::Console::WriteLine("[{0}, {1}]", c1.first, c1.second);

// assign to a new pair
    cliext::pair<wchar_t, int> c2;
    c2 = c1;
    System::Console::WriteLine("[{0}, {1}]", c2.first, c2.second);
    return (0);
    }
```

```Output
[x, 3]
[x, 3]
```

## <a name="pairpair-stlclr"></a><a name="pair_pair"></a> 对：:p 空中 (STL/CLR) 

构造一个对对象。

### <a name="syntax"></a>语法

```cpp
pair();
pair(pair<Coll>% right);
pair(pair<Coll>^ right);
pair(Value1 val1, Value2 val2);
```

#### <a name="parameters"></a>parameters

*然后*<br/>
要存储的配对。

*val1*<br/>
要存储的第一个值。

*val2*<br/>
要存储的第二个值。

### <a name="remarks"></a>备注

构造函数：

`pair();`

用默认构造的值初始化存储的对。

构造函数：

`pair(pair<Value1, Value2>% right);`

用 `right.` [对：： FIRST (STL/clr) ](#first)和 `right.` [对：： second (STL/clr) ](#second)初始化存储对。

`pair(pair<Value1, Value2>^ right);`

用 `right->` [对：： FIRST (STL/clr) ](#first)和 `right>` [对：： second (STL/clr) ](#second)初始化存储对。

构造函数：

`pair(Value1 val1, Value2 val2);`

用 *val1* 和 *val2*初始化存储对。

### <a name="example"></a>示例

```cpp
// cliext_pair_construct.cpp
// compile with: /clr
#include <cliext/utility>

int main()
    {
// construct an empty container
    cliext::pair<wchar_t, int> c1;
    System::Console::WriteLine("[{0}, {1}]",
        c1.first == L'\0' ? "\\0" : "??", c1.second);

// construct with a pair of values
    cliext::pair<wchar_t, int> c2(L'x', 3);
    System::Console::WriteLine("[{0}, {1}]", c2.first, c2.second);

// construct by copying another pair
    cliext::pair<wchar_t, int> c3(c2);
    System::Console::WriteLine("[{0}, {1}]", c3.first, c3.second);

// construct by copying a pair handle
    cliext::pair<wchar_t, int> c4(%c3);
    System::Console::WriteLine("[{0}, {1}]", c4.first, c4.second);

    return (0);
    }
```

```Output
[\0, 0]
[x, 3]
[x, 3]
[x, 3]
```

## <a name="pairsecond-stlclr"></a><a name="second"></a> 对： second (STL/CLR) 

第二个包装的值。

### <a name="syntax"></a>语法

```cpp
Value2 second;
```

### <a name="remarks"></a>备注

对象存储第二个包装的值。

### <a name="example"></a>示例

```cpp
// cliext_pair_second.cpp
// compile with: /clr
#include <cliext/utility>

int main()
    {
    cliext::pair<wchar_t, int> c1(L'x', 3);
    System::Console::WriteLine("[{0}, {1}]", c1.first, c1.second);

    cliext::pair<wchar_t, int>::first_type first_val = c1.first;
    cliext::pair<wchar_t, int>::second_type second_val = c1.second;
    System::Console::WriteLine("[{0}, {1}]", first_val, second_val);
    return (0);
    }
```

```Output
[x, 3]
```

## <a name="pairsecond_type-stlclr"></a><a name="second_type"></a> 配对：： second_type (STL/CLR) 

第二个包装值的类型。

### <a name="syntax"></a>语法

```cpp
typedef Value2 second_type;
```

### <a name="remarks"></a>备注

该类型是模板参数 *Value2*的同义词。

### <a name="example"></a>示例

```cpp
// cliext_pair_second_type.cpp
// compile with: /clr
#include <cliext/utility>

int main()
    {
    cliext::pair<wchar_t, int> c1(L'x', 3);
    System::Console::WriteLine("[{0}, {1}]", c1.first, c1.second);

    cliext::pair<wchar_t, int>::first_type first_val = c1.first;
    cliext::pair<wchar_t, int>::second_type second_val = c1.second;
    System::Console::WriteLine("[{0}, {1}]", first_val, second_val);
    return (0);
    }
```

```Output
[x, 3]
```

## <a name="pairswap-stlclr"></a><a name="swap"></a> 配对：： swap (STL/CLR) 

交换两对的内容。

### <a name="syntax"></a>语法

```cpp
void swap(pair<Value1, Value2>% right);
```

#### <a name="parameters"></a>parameters

*然后*<br/>
要与其交换内容的配对。

### <a name="remarks"></a>备注

成员函数交换和右之间存储的值对 **`*this`** 。 *right*

### <a name="example"></a>示例

```cpp
// cliext_pair_swap.cpp
// compile with: /clr
#include <cliext/adapter>
#include <cliext/deque>

typedef cliext::collection_adapter<
    System::Collections::ICollection> Mycoll;
int main()
    {
    cliext::deque<wchar_t> d1;
    d1.push_back(L'a');
    d1.push_back(L'b');
    d1.push_back(L'c');
    Mycoll c1(%d1);

// display initial contents " a b c"
    for each (wchar_t elem in c1)
        System::Console::Write("{0} ", elem);
    System::Console::WriteLine();

// construct another container with repetition of values
    cliext::deque<wchar_t> d2(5, L'x');
    Mycoll c2(%d2);
    for each (wchar_t elem in c2)
        System::Console::Write("{0} ", elem);
    System::Console::WriteLine();

// swap and redisplay
    c1.swap(c2);
    for each (wchar_t elem in c1)
        System::Console::Write("{0} ", elem);
    System::Console::WriteLine();

    for each (wchar_t elem in c2)
        System::Console::Write("{0} ", elem);
    System::Console::WriteLine();
    return (0);
    }
```

```Output
a b c
x x x x x
x x x x x
a b c
```

## <a name="make_pair-stlclr"></a><a name="make_pair"></a> STL/CLR (make_pair) 

创建一 `pair` 对值。

### <a name="syntax"></a>语法

```cpp
template<typename Value1,
    typename Value2>
    pair<Value1, Value2> make_pair(Value1 first, Value2 second);
```

#### <a name="parameters"></a>parameters

*Value1*<br/>
第一个已包装值的类型。

*Value2*<br/>
第二个包装值的类型。

*first*<br/>
要换行的第一个值。

second <br/>
要换行的第二个值。

### <a name="remarks"></a>备注

此模板函数返回 `pair<Value1, Value2>(first, second)`。 使用此方法可以构造 `pair<Value1, Value2>` 值对中的对象。

### <a name="example"></a>示例

```cpp
// cliext_make_pair.cpp
// compile with: /clr
#include <cliext/utility>

int main()
    {
    cliext::pair<wchar_t, int> c1(L'x', 3);
    System::Console::WriteLine("[{0}, {1}]", c1.first, c1.second);

    c1 = cliext::make_pair(L'y', 4);
    System::Console::WriteLine("[{0}, {1}]", c1.first, c1.second);
    return (0);
    }
```

```Output
[x, 3]
[y, 4]
```

## <a name="operator-pair-stlclr"></a><a name="op_neq"></a> operator！ = (对)  (STL/CLR) 

对不相等比较。

### <a name="syntax"></a>语法

```cpp
template<typename Value1,
    typename Value2>
    bool operator!=(pair<Value1, Value2>% left,
        pair<Value1, Value2>% right);
```

#### <a name="parameters"></a>parameters

*左中*<br/>
要比较的左对。

*然后*<br/>
要比较的右对。

### <a name="remarks"></a>备注

Operator 函数返回 `!(left == right)` 。 使用此方法可以测试在按元素对两对进行比较*时，* 是否不向*左*排序。

### <a name="example"></a>示例

```cpp
// cliext_pair_operator_ne.cpp
// compile with: /clr
#include <cliext/utility>

int main()
    {
    cliext::pair<wchar_t, int> c1(L'x', 3);
    System::Console::WriteLine("[{0}, {1}]", c1.first, c1.second);
    cliext::pair<wchar_t, int> c2(L'x', 4);
    System::Console::WriteLine("[{0}, {1}]", c2.first, c2.second);

    System::Console::WriteLine("[x 3] != [x 3] is {0}",
        c1 != c1);
    System::Console::WriteLine("[x 3] != [x 4] is {0}",
        c1 != c2);
    return (0);
    }
```

```Output
[x, 3]
[x, 4]
[x 3] != [x 3] is False
[x 3] != [x 4] is True
```

## <a name="operatorlt-pair-stlclr"></a><a name="op_lt"></a> 运算符 &lt; (对)  (STL/CLR) 

对小于比较。

### <a name="syntax"></a>语法

```cpp
template<typename Value1,
    typename Value2>
    bool operator<(pair<Value1, Value2>% left,
        pair<Value1, Value2>% right);
```

#### <a name="parameters"></a>parameters

*左中*<br/>
要比较的左对。

*然后*<br/>
要比较的右对。

### <a name="remarks"></a>备注

Operator 函数返回 `left.first <` `right.first || !(right.first <` `left.first &&` `left.second <` `right.second` 。 用于测试在按元素对两*对进行比较时，是否向**右端*排序。

### <a name="example"></a>示例

```cpp
// cliext_pair_operator_lt.cpp
// compile with: /clr
#include <cliext/utility>

int main()
    {
    cliext::pair<wchar_t, int> c1(L'x', 3);
    System::Console::WriteLine("[{0}, {1}]", c1.first, c1.second);
    cliext::pair<wchar_t, int> c2(L'x', 4);
    System::Console::WriteLine("[{0}, {1}]", c2.first, c2.second);

    System::Console::WriteLine("[x 3] < [x 3] is {0}",
        c1 < c1);
    System::Console::WriteLine("[x 3] < [x 4] is {0}",
        c1 < c2);
    return (0);
    }
```

```Output
[x, 3]
[x, 4]
[x 3] < [x 3] is False
[x 3] < [x 4] is True
```

## <a name="operatorlt-pair-stlclr"></a><a name="op_lteq"></a> operator &lt; = (对)  (STL/CLR) 

对小于或等于比较。

### <a name="syntax"></a>语法

```cpp
template<typename Value1,
    typename Value2>
    bool operator<=(pair<Value1, Value2>% left,
        pair<Value1, Value2>% right);
```

#### <a name="parameters"></a>parameters

*左中*<br/>
要比较的左对。

*然后*<br/>
要比较的右对。

### <a name="remarks"></a>备注

Operator 函数返回 `!(right < left)` 。 用于测试在按元素对两对进行*比较时，是否向**左*排序。

### <a name="example"></a>示例

```cpp
// cliext_pair_operator_le.cpp
// compile with: /clr
#include <cliext/utility>

int main()
    {
    cliext::pair<wchar_t, int> c1(L'x', 3);
    System::Console::WriteLine("[{0}, {1}]", c1.first, c1.second);
    cliext::pair<wchar_t, int> c2(L'x', 4);
    System::Console::WriteLine("[{0}, {1}]", c2.first, c2.second);

    System::Console::WriteLine("[x 3] <= [x 3] is {0}",
        c1 <= c1);
    System::Console::WriteLine("[x 4] <= [x 3] is {0}",
        c2 <= c1);
    return (0);
    }
```

```Output
[x, 3]
[x, 4]
[x 3] <= [x 3] is True
[x 4] <= [x 3] is False
```

## <a name="operator-pair-stlclr"></a><a name="op_eq"></a> operator = = (对)  (STL/CLR) 

配对相等比较。

### <a name="syntax"></a>语法

```cpp
template<typename Value1,
    typename Value2>
    bool operator==(pair<Value1, Value2>% left,
        pair<Value1, Value2>% right);
```

#### <a name="parameters"></a>parameters

*左中*<br/>
要比较的左对。

*然后*<br/>
要比较的右对。

### <a name="remarks"></a>备注

Operator 函数返回 `left.first ==` `right.first &&` `left.second ==` `right.second` 。 使用此方法可以测试在按元素对两对进行*比较时，* 是否*向左*排序。

### <a name="example"></a>示例

```cpp
// cliext_pair_operator_eq.cpp
// compile with: /clr
#include <cliext/utility>

int main()
    {
    cliext::pair<wchar_t, int> c1(L'x', 3);
    System::Console::WriteLine("[{0}, {1}]", c1.first, c1.second);
    cliext::pair<wchar_t, int> c2(L'x', 4);
    System::Console::WriteLine("[{0}, {1}]", c2.first, c2.second);

    System::Console::WriteLine("[x 3] == [x 3] is {0}",
        c1 == c1);
    System::Console::WriteLine("[x 3] == [x 4] is {0}",
        c1 == c2);
    return (0);
    }
```

```Output
[x, 3]
[x, 4]
[x 3] == [x 3] is True
[x 3] == [x 4] is False
```

## <a name="operatorgt-pair-stlclr"></a><a name="op_gt"></a> 运算符 &gt; (对)  (STL/CLR) 

配对大于比较。

### <a name="syntax"></a>语法

```cpp
template<typename Value1,
    typename Value2>
    bool operator>(pair<Value1, Value2>% left,
        pair<Value1, Value2>% right);
```

#### <a name="parameters"></a>parameters

*左中*<br/>
要比较的左对。

*然后*<br/>
要比较的右对。

### <a name="remarks"></a>备注

Operator 函数返回 `right` `<` `left` 。 用于测试在按元素对两对进行比较*时，是否向**左*排序。

### <a name="example"></a>示例

```cpp
// cliext_pair_operator_gt.cpp
// compile with: /clr
#include <cliext/utility>

int main()
    {
    cliext::pair<wchar_t, int> c1(L'x', 3);
    System::Console::WriteLine("[{0}, {1}]", c1.first, c1.second);
    cliext::pair<wchar_t, int> c2(L'x', 4);
    System::Console::WriteLine("[{0}, {1}]", c2.first, c2.second);

    System::Console::WriteLine("[x 3] > [x 3] is {0}",
        c1 > c1);
    System::Console::WriteLine("[x 4] > [x 3] is {0}",
        c2 > c1);
    return (0);
    }
```

```Output
[x, 3]
[x, 4]
[x 3] > [x 3] is False
[x 4] > [x 3] is True
```

## <a name="operatorgt-pair-stlclr"></a><a name="op_gteq"></a> operator &gt; = (对)  (STL/CLR) 

大于或等于比较。

### <a name="syntax"></a>语法

```cpp
template<typename Value1,
    typename Value2>
    bool operator>=(pair<Value1, Value2>% left,
        pair<Value1, Value2>% right);
```

#### <a name="parameters"></a>parameters

*左中*<br/>
要比较的左对。

*然后*<br/>
要比较的右对。

### <a name="remarks"></a>备注

Operator 函数返回 `!(left < right)` 。 用于测试在按元素对两对进行*比较时，是否向**左*排序。

### <a name="example"></a>示例

```cpp
// cliext_pair_operator_ge.cpp
// compile with: /clr
#include <cliext/utility>

int main()
    {
    cliext::pair<wchar_t, int> c1(L'x', 3);
    System::Console::WriteLine("[{0}, {1}]", c1.first, c1.second);
    cliext::pair<wchar_t, int> c2(L'x', 4);
    System::Console::WriteLine("[{0}, {1}]", c2.first, c2.second);

    System::Console::WriteLine("[x 3] >= [x 3] is {0}",
        c1 >= c1);
    System::Console::WriteLine("[x 3] >= [x 4] is {0}",
        c1 >= c2);
    return (0);
    }
```

```Output
[x, 3]
[x, 4]
[x 3] >= [x 3] is True
[x 3] >= [x 4] is False
```
