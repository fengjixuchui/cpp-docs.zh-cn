---
title: static_assert
ms.date: 07/29/2019
f1_keywords:
- static_assert_cpp
helpviewer_keywords:
- assertions [C++], static_assert
- static_assert
ms.assetid: 28dd3668-e78c-4de8-ba68-552084743426
ms.openlocfilehash: bf796b853d21d33d97e25c05101b7486e1eb112f
ms.sourcegitcommit: 43cee7a0d41a062661229043c2f7cbc6ace17fa3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92008856"
---
# <a name="static_assert"></a>static_assert

在编译时测试软件断言。 如果指定的常量表达式为 **`false`** ，则编译器将显示指定的消息（如果已提供），并且编译将失败并返回错误 C2338; 否则，声明不起作用。

## <a name="syntax"></a>语法

```
static_assert( constant-expression, string-literal );

static_assert( constant-expression ); // C++17 (Visual Studio 2017 and later)
```

### <a name="parameters"></a>parameters

*常量表达式*\
可以转换为布尔值的整型常量表达式。 如果计算出的表达式为零 (false) ，则显示 *字符串* 参数，且编译失败并出现错误。 如果表达式为非零 () ，则 **`static_assert`** 声明不起作用。

*字符串-文本*\
如果 *常量表达式* 参数为零，则显示一条消息。 消息是编译器的 [基本字符集](../c-language/ascii-character-set.md) 中的字符串;也就是说，不能是 [多字节或宽字符](../c-language/multibyte-and-wide-characters.md)。

## <a name="remarks"></a>备注

声明的 *常量表达式* 参数 **`static_assert`** 表示 *软件断言*。 软件断言指定在程序的某个特定点应满足的条件。 如果条件为 true，则 **`static_assert`** 声明不起作用。 如果条件为 false，则断言失败，编译器会在 *字符串* 参数中显示消息，并且编译将失败并出现错误。 在 Visual Studio 2017 和更高版本中，字符串参数是可选的。

**`static_assert`** 声明在编译时测试软件断言。 与此相反， [Assert 宏和 _assert 和 _wassert 函数](../c-runtime-library/reference/assert-macro-assert-wassert.md) 在运行时测试软件断言，并在空间或时间内产生运行时成本。 **`static_assert`** 声明对调试模板尤其有用，因为模板参数可以包含在*常数表达式*参数中。

**`static_assert`** 当遇到声明时，编译器将检查声明中的语法错误。 如果 *表达式* 参数不依赖于模板参数，则编译器会立即计算此参数。 否则，在对模板进行实例化时，编译器将计算 *常数表达式* 参数。 因此，当遇到声明时，编译器可能一次发布一个诊断消息，而在对模板进行实例化时也是如此。

可以 **`static_assert`** 在命名空间、类或块范围内使用关键字。  (**`static_assert`** 关键字在技术上是声明，尽管它不会将新名称引入到程序中，因为它可以在命名空间范围内使用。 ) 

## <a name="description-of-static_assert-with-namespace-scope"></a>`static_assert`带有命名空间范围的说明

在下面的示例中， **`static_assert`** 声明具有命名空间范围。 由于编译器知道类型 `void *` 的大小，因此可以立即计算表达式。

## <a name="example-static_assert-with-namespace-scope"></a>示例： `static_assert` 命名空间范围

```cpp
static_assert(sizeof(void *) == 4, "64-bit code generation is not supported.");
```

## <a name="description-of-static_assert-with-class-scope"></a>`static_assert`具有类范围的说明

在下面的示例中， **`static_assert`** 声明具有类范围。 **`static_assert`** 验证模板参数是否为*纯旧数据* (POD) 类型。 在声明 **`static_assert`** 声明时，编译器将检查声明，但在中实例化类模板之前，不会计算 *常数表达式* 参数 `basic_string` `main()` 。

## <a name="example-static_assert-with-class-scope"></a>示例： `static_assert` 类作用域

```cpp
#include <type_traits>
#include <iosfwd>
namespace std {
template <class CharT, class Traits = std::char_traits<CharT> >
class basic_string {
    static_assert(std::is_pod<CharT>::value,
                  "Template argument CharT must be a POD type in class template basic_string");
    // ...
    };
}

struct NonPOD {
    NonPOD(const NonPOD &) {}
    virtual ~NonPOD() {}
};

int main()
{
    std::basic_string<char> bs;
}
```

## <a name="description-of-static_assert-with-block-scope"></a>`static_assert`具有块范围的说明

在下面的示例中， **`static_assert`** 声明具有块范围。 **`static_assert`** 验证 VMPage 结构的大小是否等于系统的虚拟内存 pagesize。

## <a name="example-static_assert-at-block-scope"></a>示例： `static_assert` 在块范围

```cpp
#include <sys/param.h> // defines PAGESIZE
class VMMClient {
public:
    struct VMPage { // ...
           };
    int check_pagesize() {
    static_assert(sizeof(VMPage) == PAGESIZE,
        "Struct VMPage must be the same size as a system virtual memory page.");
    // ...
    }
// ...
};
```

## <a name="see-also"></a>请参阅

[断言和用户提供的消息 (C++)](../cpp/assertion-and-user-supplied-messages-cpp.md)<br/>
[ (C/c + + 的 #error 指令) ](../preprocessor/hash-error-directive-c-cpp.md)<br/>
[assert 宏、_assert、_wassert](../c-runtime-library/reference/assert-macro-assert-wassert.md)<br/>
[模板](../cpp/templates-cpp.md)<br/>
[ASCII 字符集](../c-language/ascii-character-set.md)<br/>
[声明和定义](declarations-and-definitions-cpp.md)
