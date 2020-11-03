---
title: '如果为-else 语句 (c + +) '
description: 如果-else，则使用初始值设定项，并使用-constexpr 语句控制条件分支。
ms.date: 10/02/2020
f1_keywords:
- else_cpp
- if_cpp
helpviewer_keywords:
- if keyword [C++]
- else keyword [C++]
ms.assetid: f8c45cde-6bce-42ae-81db-426b3dbd4caa
ms.openlocfilehash: 20d828bf00a79687fe0a9fffbeb1a9cc56fae08c
ms.sourcegitcommit: 30792632548d1c71894f9fecbe2f554294b86020
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91765292"
---
# <a name="if-else-statement-c"></a>如果为-else 语句 (c + +) 

If-else 语句控制条件分支。 *`if-branch`* 仅当的 *`condition`* 计算结果为非零值 (或) 时，才执行中的语句 **`true`** 。 如果的值 *`condition`* 为非零值，则执行以下语句，并跳过可选的后的语句 **`else`** 。 否则，将跳过下面的语句，如果执行了，则 **`else`** 执行后的语句 **`else`** 。

*`condition`* 计算结果为非零值的表达式为：

- **`true`**
- 非 null 指针，
- 任何非零算术值，或
- 一种类类型，用于定义到算术、布尔或指针类型的明确转换。  (有关转换的信息，请参阅 [标准转换](../cpp/standard-conversions.md)。 ) 

## <a name="syntax"></a>语法

*`init-statement`*:\
&emsp; *`expression-statement`*\
&emsp; *`simple-declaration`*

*`condition`*:\
&emsp; *`expression`*\
&emsp;*`attribute-specifier-seq`* <sub>*opt*</sub> *`decl-specifier-seq`* opt *`declarator`**`brace-or-equal-initializer`*

*`statement`*:\
&emsp; *`expression-statement`*\
&emsp; *`compound-statement`*

*`expression-statement`*:\
&emsp;*`expression`* <sub>*opt*</sub>**`;`**

*`compound-statement`*:\
&emsp;**`{`** *`statement-seq`* <sub>*opt*</sub>**`}`**

*`statement-seq`*:\
&emsp; *`statement`*\
&emsp; *`statement-seq`* *`statement`*

*`if-branch`*:\
&emsp; *`statement`*

*`else-branch`*:\
&emsp; *`statement`*

*`selection-statement`*:\
&emsp;**`if`** **`constexpr`** <sub>*opt*</sub><sup>17</sup> **`(`** *`init-statement`* <sub>*opt*</sub><sup>17</sup> 17 *`condition`* **`)`***`if-branch`*\
&emsp;**`if`** **`constexpr`** <sub>*opt*</sub><sup>17</sup> **`(`** *`init-statement`* <sub>*opt*</sub><sup>17</sup> 17 *`condition`* **`)`** *`if-branch`* **`else`***`else-branch`*

<sup>17</sup> 此可选元素可从 c + + 17 开始使用。

## <a name="if-else-statements"></a>else 语句

在语句的所有形式中， **`if`** *`condition`* 将计算除结构之外的任何值，并包括所有副作用。 控件从语句传递 **`if`** 到程序中的下一条语句，除非执行 *`if-branch`* 或 *`else-branch`* 包含 [`break`](../cpp/break-statement-cpp.md) 、 [`continue`](../cpp/continue-statement-cpp.md) 或 [`goto`](../cpp/goto-statement-cpp.md) 。

**`else`** 语句的子句 `if...else` 与 **`if`** 同一作用域中没有相应语句的最近一个语句相关联 **`else`** 。

### <a name="example"></a>示例

此示例代码演示了 **`if`** 使用和不使用的几个语句 **`else`** ：

```cpp
// if_else_statement.cpp
#include <iostream>

using namespace std;

class C
{
    public:
    void do_something(){}
};
void init(C){}
bool is_true() { return true; }
int x = 10;

int main()
{
    if (is_true())
    {
        cout << "b is true!\n";  // executed
    }
    else
    {
        cout << "b is false!\n";
    }

    // no else statement
    if (x == 10)
    {
        x = 0;
    }

    C* c;
    init(c);
    if (c)
    {
        c->do_something();
    }
    else
    {
        cout << "c is null!\n";
    }
}
```

## <a name="if-statement-with-an-initializer"></a><a name="if_with_init"></a> 带有初始值设定项的 if 语句

从 c + + 17 开始， **`if`** 语句还可以包含 *`init-statement`* 声明和初始化命名变量的表达式。 如果只需要在 if 语句的范围内使用变量，请使用以下形式的 if 语句。 **特定于 Microsoft**的：从 Visual Studio 2017 版本15.3 开始提供此窗体，并且至少需要 [`/std:c++17`](../build/reference/std-specify-language-standard-version.md) 编译器选项。

### <a name="example"></a>示例

```cpp
#include <iostream>
#include <mutex>
#include <map>
#include <string>
#include <algorithm>

using namespace std;

map<int, string> m;
mutex mx;
bool shared_flag; // guarded by mx
void unsafe_operation() {}

int main()
{

    if (auto it = m.find(10); it != m.end())
    {
        cout << it->second;
        return 0;
    }

    if (char buf[10]; fgets(buf, 10, stdin))
    {
        m[0] += buf;
    }

    if (lock_guard<mutex> lock(mx); shared_flag)
    {
        unsafe_operation();
        shared_flag = false;
    }

    string s{ "if" };
    if (auto keywords = { "if", "for", "while" }; any_of(keywords.begin(), keywords.end(), [&s](const char* kw) { return s == kw; }))
    {
        cout << "Error! Token must not be a keyword\n";
    }
}
```

## <a name="a-nameif_constexpr-if-constexpr-statements"></a><a name="if_constexpr"> 如果 constexpr 语句

从 c + + 17 开始，可以使用 **`if constexpr`** 函数模板中的语句进行编译时分支决策，而无需使用多个函数重载。 **特定于 Microsoft**的：从 Visual Studio 2017 版本15.3 开始提供此窗体，并且至少需要 [`/std:c++17`](../build/reference/std-specify-language-standard-version.md) 编译器选项。

### <a name="example"></a>示例

此示例演示如何编写处理参数解包的单个函数。 无需零参数重载：

```cpp
template <class T, class... Rest>
void f(T&& t, Rest&&... r)
{
    // handle t
    do_something(t);

    // handle r conditionally
    if constexpr (sizeof...(r))
    {
        f(r...);
    }
    else
    {
        g(r...);
    }
}
```

## <a name="see-also"></a>请参阅

[选择语句](../cpp/selection-statements-cpp.md)\
[字](../cpp/keywords-cpp.md)\
[`switch`语句 (C++)](../cpp/switch-statement-cpp.md)
