---
title: '&lt;functional&gt;'
ms.date: 02/21/2019
f1_keywords:
- <functional>
- functional/std::<functional>
- std::<functional>
helpviewer_keywords:
- functors
- functional header
ms.assetid: 7dd463e8-a29f-49bc-aedd-8fa53b54bfbc
ms.openlocfilehash: 9f1eaf69f49a449877b9013dca62ab49cb8a5b48
ms.sourcegitcommit: ec6dd97ef3d10b44e0fedaa8e53f41696f49ac7b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2020
ms.locfileid: "88838043"
---
# <a name="ltfunctionalgt"></a>&lt;functional&gt;

定义帮助构造 *函数对象*（也称为 *函子*）及其联编程序的 c + + 标准库函数。 函数对象是用于定义 `operator()` 的类型的对象。 函数对象可以是函数指针，但该对象更常用于存储可在函数调用过程中访问的其他信息。

## <a name="requirements"></a>要求

**标头：**\<functional>

**命名空间:** std

## <a name="remarks"></a>注解

算法需要两种类型的函数对象： *一元* 和 *binary*。 一元函数对象需要一个自变量，二元函数对象需要两个自变量。 函数对象和函数指针均可以作为算法的谓词传递，但函数对象还能自适应，并增加 C++ 标准库的作用域、灵活性和效率。 例如，如果需要在将值传递给算法之前将其绑定到函数，则不能使用函数指针。 函数适配器将函数指针转换为可以绑定到值的自适应函数对象。 标头 \<functional> 还包含成员函数适配器，它们允许将成员函数作为可自适应函数对象调用。 如果函数具有指定其自变量和返回类型的嵌套类型声明，则它是自适应的。 函数对象及其适配器允许 C++ 标准库升级现有应用程序，并帮助将库集成到 C++ 编程环境。

中函数对象的实现 \<functional> 包括 *透明运算符函子*。 这是标准函数对象的专用化，不使用任何模板参数，并可实现函数自变量的完美转发，并完美返回结果。 这些模板专用化不要求在调用算术、比较、逻辑和按位运算符函子时指定参数类型。 可为你自己的类型或类型的异类组合重载算术、比较、逻辑或按位运算符，然后将透明运算符函子用作函数自变量。 例如，如果类型 MyType** 实现 `operator<`，则可以调用 `sort(my_collection.begin(), my_collection.end(), less<>())` 而不是显式指定类型 `sort(my_collection.begin(), my_collection.end(), less<MyType>())`。

C + + 11、c + + 14 和 c + + 17 中添加了以下功能：

- “调用签名”** 是返回类型的名称，后跟一个用圆括号括起来的零个或多个参数类型列表，这些自变量类型以逗号分隔。

- 可调用类型** 可以是指向函数的指针、指向成员函数的指针、指向成员数据的指针或是其对象可以立即显示在函数调用运算符左侧的类类型。

- 可调用对象** 是可调用类型的对象。

- 调用包装器类型** 包含可调用对象并支持转发到该对象的调用操作。

- 调用包装器** 是调用包装器类型的对象。

- 目标对象** 是调用包装器对象持有的可调用对象。

伪函数 `INVOKE(f, t1, t2, ..., tN)` 表示以下内容之一：

- 当 `f` 是指向类 `T` 的成员函数的指针，且 `t1` 是类型 `T` 的对象、对类型 `T` 的对象的引用或对派生自 `T` 的类型的对象的引用时，它表示 `(t1.*f)(t2, ..., tN)`。

- 当 `f` 是指向类 `T` 的成员函数的指针，且 `t1` 不是上一项中描述的任一类型时，它表示 `((*t1).*f)(t2, ..., tN)`。

- 当 N == 1，且 `f` 是指向类 `T` 的成员数据的指针，`t1` 是类型 `T` 的对象、对类型 `T` 的对象的引用或对派生自 `T` 的类型的对象的引用时，它表示 `t1.*f`。

- 当 N == 1，且 `f` 是指向类 `T` 的成员数据的指针，`t1` 不是上一项中描述的任一类型时，它表示 `(*t1).*f`。

- 所有其他情况均为 `f(t1, t2, ..., tN)`。

伪函数 `INVOKE(f, t1, t2, ..., tN, R)` 表示将 `INVOKE(f, t1, t2, ..., tN)` 隐式转换为 `R`。

如果调用包装器具有弱结果类型**，则其成员类型 `result_type` 的类型取决于包装器目标对象的类型 `T`，如下所示：

- 如果 `T` 是指向函数的指针，则 `result_type` 是 `T` 的返回类型的同义词。

- 如果 `T` 是指向成员函数的指针，则 `result_type` 是 `T` 的返回类型的同义词。

- 如果 `T` 是具有成员类型 `result_type` 的类类型，则 `result_type` 是 `T::result_type` 的同义词。

- 否则不存在成员 `result_type`。

每个调用包装器具有一个移动构造函数和一个复制构造函数。 简单调用包装器** 是一种具有赋值运算符的调用包装，其复制构造函数、移动构造函数和赋值运算符不会引发异常。 转发调用包装器** 是一种可以使用任意自变量列表进行调用并以引用方式将自变量传递给已包装可调用对象的调用包装器。 所有右值自变量都以右值引用的方式传递，所有左值自变量都以左值引用的方式传递。

## <a name="members"></a>成员

### <a name="classes"></a>类

|“属性”|说明|
|-|-|
|[bad_function_call](../standard-library/bad-function-call-class.md)|一种类，用于描述引发的异常，以指示对[函数](../standard-library/function-class.md)对象上的 `operator()` 的调用由于该对象为空而失败。|
|[binary_negate](../standard-library/binary-negate-class.md)|提供成员函数的类模板，用于对指定二元函数的返回值求反。<br/>  (在 c + + 17 中弃用。 )  |
|[binder1st](../standard-library/binder1st-class.md)|一个类模板，它提供了一个构造函数，该构造函数通过将二元函数的第一个自变量绑定到指定的值，将二元函数对象转换为一元函数对象。<br/> C + + 11 中已弃用 (，已在 c + + 17 中删除 )  |
|[binder2nd](../standard-library/binder2nd-class.md)|一个类模板，它提供了一个构造函数，该构造函数通过将二元函数的第二个参数绑定到指定的值，将二元函数对象转换为一元函数对象。<br/> C + + 11 中已弃用 (，已在 c + + 17 中删除 )  |
|[boyer_moore_horspool_searcher](../standard-library/boyer-moore-horspool-searcher-class.md)||
|[boyer_moore_searcher](../standard-library/boyer-moore-searcher-class.md)||
|[const_mem_fun_ref_t](../standard-library/const-mem-fun-ref-t-class.md)|一种适配器类，在使用引用自变量进行初始化的情况下，该类允许将不带任何自变量的 const 成员函数作为一元函数对象调用。<br/> C + + 11 中已弃用 (，已在 c + + 17 中删除 )  |
|[const_mem_fun_t](../standard-library/const-mem-fun-t-class.md)|一种适配器类，在使用指针自变量进行初始化的情况下，该类允许将不带任何自变量的 const 成员函数作为一元函数对象调用。<br/> C + + 11 中已弃用 (，已在 c + + 17 中删除 )  |
|[const_mem_fun1_ref_t](../standard-library/const-mem-fun1-ref-t-class.md)|一种适配器类，在使用引用自变量进行初始化的情况下，该类允许将仅带一个自变量的 const 成员函数作为二元函数对象调用。<br/> C + + 11 中已弃用 (，已在 c + + 17 中删除 )  |
|[const_mem_fun1_t](../standard-library/const-mem-fun1-t-class.md)|一种适配器类，在使用指针自变量进行初始化的情况下，该类允许将仅带一个自变量的 const 成员函数作为二元函数对象调用。<br/> C + + 11 中已弃用 (，已在 c + + 17 中删除 )  |
|[default_searcher](../standard-library/default-searcher-class.md)||
|[函数](../standard-library/function-class.md)|一种类，用于包装可调用的对象。|
|[hash](../standard-library/hash-class.md)|一种类，用于计算值的哈希代码。|
|[is_bind_expression](../standard-library/is-bind-expression-class.md)|一种类，用于调用 `bind` 时是否会生成特定的类型。|
|[is_placeholder](../standard-library/is-placeholder-class.md)|一种类，用于测试特定类型是否为占位符。|
|[mem_fun_ref_t](../standard-library/mem-fun-ref-t-class.md)|一种适配器类， `non_const` 在使用引用自变量进行初始化时，该类允许将不带任何参数的成员函数作为一元函数对象调用。<br/> C + + 11 中已弃用 (，已在 c + + 17 中删除 )  |
|[mem_fun_t](../standard-library/mem-fun-t-class.md)|一种适配器类， `non_const` 在使用指针参数进行初始化时，该类允许将不带任何参数的成员函数作为一元函数对象调用。<br/> C + + 11 中已弃用 (，已在 c + + 17 中删除 )  |
|[mem_fun1_ref_t](../standard-library/mem-fun1-ref-t-class.md)|一种适配器类， `non_const` 在使用引用参数进行初始化时，此类允许使用单个自变量的成员函数作为二元函数对象调用。<br/> C + + 11 中已弃用 (，已在 c + + 17 中删除 )  |
|[mem_fun1_t](../standard-library/mem-fun1-t-class.md)|一种适配器类， `non_const` 在使用指针参数进行初始化时，此类允许使用单个自变量的成员函数作为二元函数对象调用。<br/> C + + 11 中已弃用 (，已在 c + + 17 中删除 )  |
|[pointer_to_binary_function](../standard-library/pointer-to-binary-function-class.md)|将二元函数指针转换为自适应二元函数。<br/> C + + 11 中已弃用 (，已在 c + + 17 中删除 )  |
|[pointer_to_unary_function](../standard-library/pointer-to-unary-function-class.md)|将一元函数指针转换为自适应一元函数。<br/> C + + 11 中已弃用 (，已在 c + + 17 中删除 )  |
|[reference_wrapper](../standard-library/reference-wrapper-class.md)|一种类，用于包装引用。|
|[unary_negate](../standard-library/unary-negate-class.md)|提供成员函数的类模板，用于对指定一元函数的返回值求反。<br/>  (在 c + + 17 中弃用。 )   |

### <a name="functions"></a>函数

|名称|说明|
|-|-|
|[bind](../standard-library/functional-functions.md#bind)|将自变量绑定到可调用对象。|
|[bind1st](../standard-library/functional-functions.md#bind1st)|一种帮助程序模板类，用于创建适配器，通过将二元函数的第一个自变量绑定到指定的值，将二元函数对象转换为一元函数对象。<br/> C + + 11 中已弃用 (，已在 c + + 17 中删除 )  |
|[bind2nd](../standard-library/functional-functions.md#bind2nd)|一种帮助程序模板类，用于创建适配器，通过将二元函数的第二个自变量绑定到指定的值，将二元函数对象转换为一元函数对象。<br/> C + + 11 中已弃用 (，已在 c + + 17 中删除 )  |
|[bit_and](../standard-library/functional-functions.md#bit_and)|返回两个参数的按位逻辑 AND（二元运算符 &）。|
|[bit_not](../standard-library/functional-functions.md#bit_not)|返回两个参数的按位逻辑求反（二元运算符 ~）。<br/>  (在 c + + 14 中添加 )  |
|[bit_or](../standard-library/functional-functions.md#bit_or)|返回两个参数的按位逻辑 OR（运算符 |）。|
|[bit_xor](../standard-library/functional-functions.md#bit_xor)|返回两个参数的按位逻辑 XOR（二元运算符 ^）。|
|[cref](../standard-library/functional-functions.md#cref)|从变量构造常量 `reference_wrapper`。|
|[invoke](../standard-library/functional-functions.md#invoke)||
|[mem_fn](../standard-library/functional-functions.md#mem_fn)|生成一个简单的调用包装器。|
|[mem_fun](../standard-library/functional-functions.md#mem_fun)|帮助程序模板函数，在使用指针自变量进行初始化的情况下，用来构造成员函数的函数对象适配器。<br/> C + + 11 中已弃用 (，已在 c + + 17 中删除 )  |
|[mem_fun_ref](../standard-library/functional-functions.md#mem_fun_ref)|帮助程序模板函数，在使用引用自变量进行初始化的情况下，用来构造成员函数的函数对象适配器。|
|[not1](../standard-library/functional-functions.md#not1)|返回一元谓词的补集。<br/>  (在 c + + 17 中弃用。 )  |
|[not2](../standard-library/functional-functions.md#not2)|返回二元谓词的补集。<br/>  (在 c + + 17 中弃用。 )  |
|[not_fn](../standard-library/functional-functions.md#not_fn)|返回其函数对象的结果的补码。<br/>  (在 c + + 17 中添加 )  |
|[ptr_fun](../standard-library/functional-functions.md#ptr_fun)|帮助程序模板函数，用于将一元和二元函数指针分别转换为一元和二元自适应函数。<br/> C + + 11 中已弃用 (，已在 c + + 17 中删除 )  |
|[ref](../standard-library/functional-functions.md#ref)|从变量构造常量 `reference_wrapper` 。|
|[swap](../standard-library/functional-functions.md#swap)|交换两个 `function` 对象。|

### <a name="structs"></a>结构

|名称|说明|
|-|-|
|[binary_function](../standard-library/binary-function-struct.md)|空基类，定义可能由提供二元函数对象的派生类继承的类型。<br/> C + + 11 中已弃用 (，已在 c + + 17 中删除 )  |
|[divides](../standard-library/divides-struct.md)|此类提供预定义的函数对象，后者对指定值类型的元素执行除法算术运算。|
|[equal_to](../standard-library/equal-to-struct.md)|此二元谓词测试指定类型的一个值是否等于该类型的另一个值。|
|[greater](../standard-library/greater-struct.md)|此二元谓词测试指定类型的一个值是否大于该类型的另一个值。|
|[greater_equal](../standard-library/greater-equal-struct.md)|此二元谓词测试指定类型的一个值是否大于或等于该类型的另一个值。|
|[less](../standard-library/less-struct.md)|此二元谓词测试指定类型的一个值是否小于该类型的另一个值。|
|[less_equal](../standard-library/less-equal-struct.md)|此二元谓词测试指定类型的一个值是否小于或等于该类型的另一个值。|
|[logical_and](../standard-library/logical-and-struct.md)|此类提供预定义的函数对象，后者对指定值类型的元素执行合取逻辑运算，并测试结果是 ture 还是 false。|
|[logical_not](../standard-library/logical-not-struct.md)|此类提供预定义的函数对象，后者对指定值类型的元素执行求反逻辑运算，并测试结果是 ture 还是 false。|
|[logical_or](../standard-library/logical-or-struct.md)|此类提供预定义的函数对象，后者对指定值类型的元素执行析取逻辑运算，并测试结果是 ture 还是 false。|
|[minus](../standard-library/minus-struct.md)|此类提供预定义的函数对象，后者对指定值类型的元素执行减法算术运算。|
|[模块](../standard-library/modulus-struct.md)|此类提供预定义的函数对象，后者对指定值类型的元素执行取模算术运算。|
|[multiplies](../standard-library/multiplies-struct.md)|此类提供预定义的函数对象，后者对指定值类型的元素执行乘法算术运算。|
|[negate](../standard-library/negate-struct.md)|此类提供预定义的函数对象，后者返回元素值的负值。|
|[not_equal_to](../standard-library/not-equal-to-struct.md)|此二元谓词测试指定类型的一个值是否不等于该类型的另一个值。|
|[plus](../standard-library/plus-struct.md)|此类提供预定义的函数对象，后对指定值类型的元素执行加法算术运算。|
|[unary_function](../standard-library/unary-function-struct.md)|空基类，定义可能由提供一元函数对象的派生类继承的类型。<br/> C + + 11 中已弃用 (，已在 c + + 17 中删除 )  |

### <a name="objects"></a>对象

|名称|说明|
|-|-|
|[_1.._M](../standard-library/1-object.md)|可替换自变量的占位符。|

### <a name="operators"></a>运算符

|名称|说明|
|-|-|
|[operator = =](../standard-library/functional-operators.md#op_eq_eq)|不允许对可调用对象进行相等性比较。|
|[operator！ =](../standard-library/functional-operators.md#op_neq)|不允许对可调用对象进行不等性比较。|

## <a name="see-also"></a>另请参阅

[标头文件引用](../standard-library/cpp-standard-library-header-files.md)\
[C + + 标准库中的线程安全](../standard-library/thread-safety-in-the-cpp-standard-library.md)\
[C + + 标准库参考](../standard-library/cpp-standard-library-reference.md)
