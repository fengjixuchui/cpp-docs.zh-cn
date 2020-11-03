---
title: MSVC 新预处理器概述
description: 正在更新 MSVC 预处理器，使其符合 C/c + + 标准。
ms.date: 09/10/2020
helpviewer_keywords:
- preprocessor, experimental
- preprocessor, new
ms.openlocfilehash: 5327a8148f78a07e222fae7fb92e6ed741d12011
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924582"
---
# <a name="msvc-new-preprocessor-overview"></a>MSVC 新预处理器概述

::: moniker range="msvc-140"

Visual Studio 2015 使用不符合标准 c + + 或 C99 的传统预处理器。 从 Visual Studio 2019 16.5 版开始，新的预处理器支持 c + + 20 标准功能是完整功能。 这些更改可通过使用 [/zc：预处理器](../build/reference/zc-preprocessor.md) 编译器开关来使用。 在 Visual Studio 2017 版本15.8 及更高版本中，使用 [/experimental：预处理器](../build/reference/experimental-preprocessor.md) 编译器开关开始提供新预处理器的实验版本。 提供了有关在 Visual Studio 2017 和 Visual Studio 2019 中使用新预处理器的详细信息。 若要查看 Visual Studio 首选项的文档，请使用“版本”选择器控件。 它位于此页面上目录表的顶部。

::: moniker-end

::: moniker range=">=msvc-150"

我们正在更新 Microsoft c + + 预处理器来改善标准一致性、修复长久持续 bug 并更改正式定义的某些行为。 我们还添加了新诊断，以便在宏定义中出现错误时发出警告。

从 Visual Studio 2019 版本16.5 开始，预处理器对 c + + 20 标准的支持是功能完整的。 这些更改可通过使用 [/zc：预处理器](../build/reference/zc-preprocessor.md) 编译器开关来使用。 从 Visual Studio 2017 版本15.8 开始，早期版本中提供了新预处理器的实验版本。 可以通过使用 [/experimental：预处理器](../build/reference/experimental-preprocessor.md) 编译器开关来启用它。 默认预处理器行为与以前版本中的行为相同。

## <a name="new-predefined-macro"></a>新建预定义宏

可以在编译时检测正在使用的预处理器。 检查预定义宏的值 [`_MSVC_TRADITIONAL`](predefined-macros.md) ，以了解传统预处理器是否正在使用中。 此宏由支持它的编译器的版本无条件设置，与调用的预处理器无关。 对于传统预处理器，其值为1。 它是0，适用于相容预处理器。

```cpp
#if defined(_MSVC_TRADITIONAL) && _MSVC_TRADITIONAL
// Logic using the traditional preprocessor
#else
// Logic using cross-platform compatible preprocessor
#endif
```

## <a name="behavior-changes-in-the-new-preprocessor"></a>新预处理器中的行为更改

新预处理器上的初始工作已侧重于使所有宏展开符合标准。 它使你可以将 MSVC 编译器用于当前由传统行为阻止的库。 我们在实际项目中测试了更新的预处理器。 下面是我们发现的一些更常见的重大更改：

### <a name="macro-comments"></a>宏注释

传统预处理器基于字符缓冲区，而不是预处理器标记。 它允许异常行为，如以下预处理器注释技巧，这在符合预处理器的情况下不起作用：

```cpp
#if DISAPPEAR
#define DISAPPEARING_TYPE /##/
#else
#define DISAPPEARING_TYPE int
#endif

// myVal disappears when DISAPPEARING_TYPE is turned into a comment
DISAPPEARING_TYPE myVal;
```

符合标准的修复方法是在 `int myVal` 相应指令内声明 `#ifdef/#endif` ：

```cpp
#define MYVAL 1

#ifdef MYVAL
int myVal;
#endif
```

### <a name="lval"></a>L # val

传统预处理器不正确地将字符串前缀与字符串化运算符的结果组合 [ ( # ) ](stringizing-operator-hash.md) 运算符：

```cpp
 #define DEBUG_INFO(val) L"debug prefix:" L#val
//                                       ^
//                                       this prefix

const wchar_t *info = DEBUG_INFO(hello world);
```

在这种情况下， `L` 前缀是不必要的，因为在宏展开后，相邻字符串文本会合并在一起。 向后兼容的修复是更改定义：

```cpp
#define DEBUG_INFO(val) L"debug prefix:" #val
//                                       ^
//                                       no prefix
```

在方便的宏中，还会出现相同的问题： "stringize" 宽字符串文本的参数：

```cpp
 // The traditional preprocessor creates a single wide string literal token
#define STRING(str) L#str
```

可以通过多种方式解决该问题：

- 使用和的字符串 `L""` 串联 `#str` 来添加前缀。 相邻的字符串在宏展开后合并：

   ```cpp
   #define STRING1(str) L""#str
   ```

- 在 stringized 后添加前缀 `#str` 以附加宏展开

   ```cpp
   #define WIDE(str) L##str
   #define STRING2(str) WIDE(#str)
   ```

- 使用串联运算符 `##` 组合标记。 `##` `#` 但 `#` `##` 在此情况下，所有编译器都看不到运算符的运算顺序，但不指定操作的顺序。

   ```cpp
   #define STRING3(str) L## #str
   ```

### <a name="warning-on-invalid-"></a>对无效的警告 \#\#

如果 [标记粘贴运算符 ( # # ) ](token-pasting-operator-hash-hash.md) 不产生单个有效的预处理标记，则该行为是不确定的。 传统预处理器以静默方式组合标记失败。 新的预处理器匹配大多数其他编译器的行为，并发出诊断。

```cpp
// The ## is unnecessary and does not result in a single preprocessing token.
#define ADD_STD(x) std::##x
// Declare a std::string
ADD_STD(string) s;
```

### <a name="comma-elision-in-variadic-macros"></a>可变参数宏中的逗号省略

传统的 MSVC 预处理器在空替换之前始终删除逗号 `__VA_ARGS__` 。 新的预处理器更严格地遵循其他常用跨平台编译器的行为。 若要移除逗号，可变参数参数必须 (不只为空) 并且必须使用运算符进行标记 `##` 。 请考虑以下示例：

```cpp
void func(int, int = 2, int = 3);
// This macro replacement list has a comma followed by __VA_ARGS__
#define FUNC(a, ...) func(a, __VA_ARGS__)
int main()
{
    // In the traditional preprocessor, the
    // following macro is replaced with:
    // func(10,20,30)
    FUNC(10, 20, 30);

    // A conforming preprocessor replaces the
    // following macro with: func(1, ), which
    // results in a syntax error.
    FUNC(1, );
}
```

在下面的示例中，在调用的 `FUNC2(1)` 宏中缺少对可变参数参数的调用。 在调用 `FUNC2(1, )` 可变参数参数为空，但不缺少 (注意参数列表中的逗号) 。

```cpp
#define FUNC2(a, ...) func(a , ## __VA_ARGS__)
int main()
{
   // Expands to func(1)
   FUNC2(1);

   // Expands to func(1, )
   FUNC2(1, );
}
```

在即将推出的 c + + 20 标准中，此问题已通过添加来解决 `__VA_OPT__` 。 `__VA_OPT__`从 Visual Studio 2019 版本16.5 开始提供了的新的预处理器支持。

### <a name="c20-variadic-macro-extension"></a>C + + 20 可变参数宏扩展

新预处理器支持 c + + 20 可变参数宏参数省略：

```cpp
#define FUNC(a, ...) __VA_ARGS__ + a
int main()
  {
  int ret = FUNC(0);
  return ret;
  }
```

此代码在 c + + 20 标准版之前不相容。 在 MSVC 中，新的预处理器将此 c + + 20 行为扩展 () 的语言标准模式 **`/std:c++14`** **`/std:c++17`** 。 此扩展与其他主要跨平台 c + + 编译器的行为匹配。

### <a name="macro-arguments-are-unpacked"></a>宏参数为 "解压缩"

在传统预处理器中，如果宏将其参数之一转发给另一个依赖宏，则在插入时，该参数不会 "解压缩"。 通常，此优化会被忽略，但可能导致异常行为：

```cpp
// Create a string out of the first argument, and the rest of the arguments.
#define TWO_STRINGS( first, ... ) #first, #__VA_ARGS__
#define A( ... ) TWO_STRINGS(__VA_ARGS__)
const char* c[2] = { A(1, 2) };

// Conforming preprocessor results:
// const char c[2] = { "1", "2" };

// Traditional preprocessor results, all arguments are in the first string:
// const char c[2] = { "1, 2", };
```

展开时 `A()` ，传统预处理器将打包的所有参数转发 `__VA_ARGS__` 到 TWO_STRINGS 的第一个参数，该参数将可变参数参数留 `TWO_STRINGS` 空。 这将导致的结果为 `#first` "1，2" 而不只是 "1"。 如果你要密切关注，你可能会想知道 `#__VA_ARGS__` 传统预处理器扩展中的结果发生了什么：如果可变参数参数为空，则它应导致空字符串 `""` 。 单独的问题导致生成空字符串文本标记。

### <a name="rescanning-replacement-list-for-macros"></a>重新扫描宏的替换列表

替换宏后，将重新扫描生成的令牌，以便替换其他宏标识符。 传统预处理器用于执行重新扫描的算法不一致，如以下示例中所示：

```cpp
#define CAT(a,b) a ## b
#define ECHO(...) __VA_ARGS__
// IMPL1 and IMPL2 are implementation details
#define IMPL1(prefix,value) do_thing_one( prefix, value)
#define IMPL2(prefix,value) do_thing_two( prefix, value)

// MACRO chooses the expansion behavior based on the value passed to macro_switch
#define DO_THING(macro_switch, b) CAT(IMPL, macro_switch) ECHO(( "Hello", b))
DO_THING(1, "World");

// Traditional preprocessor:
// do_thing_one( "Hello", "World");
// Conforming preprocessor:
// IMPL1 ( "Hello","World");
```

尽管此示例看起来有点精心设计，但我们已在实际代码中看到了此示例。

若要查看正在进行的操作，可以从开始细分扩展 `DO_THING` ：

1. `DO_THING(1, "World")` 扩展到 `CAT(IMPL, 1) ECHO(("Hello", "World"))`
1. `CAT(IMPL, 1)` 展开到 `IMPL ## 1` ，将扩展到 `IMPL1`
1. 现在，令牌处于以下状态： `IMPL1 ECHO(("Hello", "World"))`
1. 预处理器查找类似函数的宏标识符 `IMPL1` 。 由于后面不是 `(` ，因此不会将其视为类似于函数的宏调用。
1. 预处理器转到以下标记。 它将查找调用类似函数的宏 `ECHO` `ECHO(("Hello", "World"))` ，该宏将扩展到 `("Hello", "World")`
1. `IMPL1` 永远不会被视为展开，因此扩展的完整结果为： `IMPL1("Hello", "World");`

若要修改宏，使其在新的预处理器和传统预处理器上的行为方式相同，请添加其他间接层：

```cpp
#define CAT(a,b) a##b
#define ECHO(...) __VA_ARGS__
// IMPL1 and IMPL2 are macros implementation details
#define IMPL1(prefix,value) do_thing_one( prefix, value)
#define IMPL2(prefix,value) do_thing_two( prefix, value)
#define CALL(macroName, args) macroName args
#define DO_THING_FIXED(a,b) CALL( CAT(IMPL, a), ECHO(( "Hello",b)))
DO_THING_FIXED(1, "World");

// macro expands to:
// do_thing_one( "Hello", "World");
```

## <a name="incomplete-features-before-165"></a>16.5 之前的未完成功能

从 Visual Studio 2019 版本16.5 开始，新的预处理器是 c + + 20 的功能完整功能。 在以前版本的 Visual Studio 中，新的预处理器主要是完整的，尽管一些预处理器指令逻辑仍然会回退到传统行为。 下面是16.5 之前 Visual Studio 版本中未完成功能的部分列表：

- `_Pragma` 支持
- C + + 20 功能
- 增大阻止 bug：在16.5 版之前，预处理器常量表达式中的逻辑运算符未完全在新的预处理器内实现。 在某些 `#if` 指令上，新的预处理器可能会回退到传统的预处理器。 仅当与传统预处理器不兼容的宏展开时，此效果才明显。 构建提升的预处理器槽时可能会发生这种情况。

::: moniker-end
