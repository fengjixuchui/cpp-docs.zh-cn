---
title: path 类
ms.date: 09/27/2018
f1_keywords:
- filesystem/std::experimental::filesystem::path
ms.assetid: 8a1227ca-aeb2-4e0e-84aa-86e34e4f4fe8
ms.openlocfilehash: fb56afbc1d29f1d321b394342382f89b06768720
ms.sourcegitcommit: b5854134553db1d99a5761bec131841c374a3098
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2020
ms.locfileid: "91958654"
---
# <a name="path-class"></a>path 类

**Path**类存储类型为的对象 `string_type` ，此对象 `myname` 用于处于阐释，适合用作路径名。 `string_type` 是的同义词 `basic_string<value_type>` ，其中 `value_type` 是 **`wchar_t`** 在 Windows 或 POSIX 上的同义词 **`char`** 。

有关详细信息和代码示例，请参阅 [文件系统导航 (c + +) ](../standard-library/file-system-navigation.md)。

## <a name="syntax"></a>语法

```cpp
class path;
```

### <a name="constructors"></a>构造函数

|构造函数|说明|
|-|-|
|[路径](#path)|构造一个 `path`。|

### <a name="typedefs"></a>Typedef

|类型名称|描述|
|-|-|
|[const_iterator](#const_iterator)|`iterator` 的同义词。|
|[器](#iterator)|指定组件的双向常量迭代器 `path` `myname` 。|
|[string_type](#string_type)|该类型是 `basic_string<value_type>` 的同义词。|

### <a name="member-functions"></a>成员函数

|成员函数|说明|
|-|-|
|[append](#append)|根据需要将指定序列追加到 `mypath` 、转换和插入 preferred_separator。|
|[assign](#assign)|替换为 `mypath` 指定的序列，并根据需要进行转换。|
|[准备](#begin)|返回一个 `path::iterator` ，它指定路径名中的第一个路径元素（如果存在）。|
|[c_str](#c_str)|返回指向中的第一个字符的指针 `mypath` 。|
|[clear](#clear)|执行 `mypath.clear()` 。|
|[并排](#compare)|返回比较值。|
|[concat](#compare)|将指定序列追加到 `mypath` ，转换 (但不根据需要插入分隔符) 。|
|[empty](#empty)|返回 `mypath.empty()`。|
|[end](#end)|返回类型的序列末迭代器 `iterator` 。|
|[extension](#extension)|返回的后缀 `filename()` 。|
|[filename](#filename)|返回 myname 的根目录组件，尤其是 `empty() ? path() : *--end()`。 组件可能为空。|
|[generic_string](#generic_string)|返回 `this->string<Elem, Traits, Alloc>(al)` ，其中（在 Windows 下）任何反斜杠均转换为正斜杠。|
|[generic_u16string](#generic_u16string)|返回 `u16string()` ，其中（在 Windows 下）任何反斜杠均转换为正斜杠。|
|[generic_u32string](#generic_u32string)|返回 `u32string()` ，其中（在 Windows 下）任何反斜杠均转换为正斜杠。|
|[generic_u8string](#generic_u8string)|返回 `u8string()` ，其中（在 Windows 下）任何反斜杠均转换为正斜杠。|
|[generic_wstring](#generic_wstring)|返回 `wstring()` ，其中（在 Windows 下）任何反斜杠均转换为正斜杠。|
|[has_extension](#has_extension)|返回 `!extension().empty()`。|
|[has_filename](#has_filename)|返回 `!filename().empty()`。|
|[has_parent_path](#has_parent_path)|返回 `!parent_path().empty()`。|
|[has_relative_path](#has_relative_path)|返回 `!relative_path().empty()`。|
|[has_root_directory](#has_root_directory)|返回 `!root_directory().empty()`。|
|[has_root_name](#has_root_name)|返回 `!root_name().empty()`。|
|[has_root_path](#has_root_path)|返回 `!root_path().empty()`。|
|[has_stem](#has_stem)|返回 `!stem().empty()`。|
|[is_absolute](#is_absolute)|对于 Windows，函数返回 `has_root_name() && has_root_directory()` 。 对于 POSIX，函数返回 `has_root_directory()` 。|
|[is_relative](#is_relative)|返回 `!is_absolute()`。|
|[make_preferred](#make_preferred)|根据需要将每个分隔符转换为 preferred_separator。|
|[native](#native)|返回 `myname`。|
|[parent_path](#parent_path)|返回的父路径组件 `myname` 。|
|[preferred_separator](#preferred_separator)|常量对象提供首选字符来分隔路径组件，具体取决于主机操作系统。 |
|[relative_path](#relative_path)|返回的相对路径组件 `myname` 。 |
|[remove_filename](#remove_filename)|删除文件名。|
|[replace_extension](#replace_extension)|替换的扩展 `myname` 。 |
|[replace_filename](#replace_filename)|RReplaces 文件名。|
|[root_directory](#root_directory)|返回的根目录组件 `myname` 。 |
|[root_name](#root_name)|返回的根名称组件 `myname` 。 |
|[root_path](#root_path)|返回的根路径组件 `myname` 。|
|[stem](#stem)|返回的 `stem` 组件 `myname` 。|
|[string](#string)|转换中存储的序列 `mypath` 。|
|[swap](#swap)|执行 `swap(mypath, right.mypath)` 。|
|[u16string](#u16string)|将中存储的序列转换 `mypath` 为 utf-16，并将其返回存储在类型的对象中 `u16string` 。|
|[u32string](#u32string)|将中存储的序列转换 `mypath` 为 32 utf-8，并将其返回存储在类型的对象中 `u32string` 。|
|[u8string](#u8string)|将中存储的序列转换 `mypath` 为 utf-8，并将其返回存储在类型的对象中 `u8string` 。|
|[value_type](#value_type)|类型描述了主机操作系统偏好的路径元素。|
|[wstring](#wstring)|将中存储的序列转换 `mypath` 为序列的主机系统所用的编码 **`wchar_t`** ，并将其返回存储在类型的对象中 `wstring` 。|

### <a name="operators"></a>运算符

|操作员|描述|
|-|-|
|[operator =](#op_as)|将路径的元素替换为另一个路径的副本。|
|[运算符 + =](#op_add)|各种 `concat` 表达式。|
|[operator/=](#op_divide)|各种 `append` 表达式。|
|[运算符 string_type](#op_string)|返回 `myname`。|

## <a name="requirements"></a>要求

**标头：**\<filesystem>

**命名空间：** std::experimental::filesystem

## <a name="pathappend"></a><a name="append"></a> path：： append

将指定序列追加到 `mypath` ，并根据需要进行转换和插入 `preferred_separator` 。

```cpp
template <class Source>
path& append(const Source& source);

template <class InIt>
path& append(InIt first, InIt last);
```

### <a name="parameters"></a>参数

*源程序*\
指定的序列。

*1*\
指定序列的开头。

*时间*\
指定序列的末尾。

## <a name="pathassign"></a><a name="assign"></a> path：： assign

替换为 `mypath` 指定的序列，并根据需要进行转换。

```cpp
template <class Source>
path& assign(const Source& source);

template <class InIt>
path& assign(InIt first, InIt last);
```

### <a name="parameters"></a>参数

*源程序*\
指定的序列。

*1*\
指定序列的开头。

*时间*\
指定序列的末尾。

## <a name="pathbegin"></a><a name="begin"></a> path：： begin

返回一个 `path::iterator` ，它指定路径名中的第一个路径元素（如果存在）。

```cpp
iterator begin() const;
```

## <a name="pathc_str"></a><a name="c_str"></a> path：： c_str

返回指向中的第一个字符的指针 `mypath` 。

```cpp
const value_type& *c_str() const noexcept;
```

## <a name="pathclear"></a><a name="clear"></a> path：： clear

执行 `mypath.clear()` 。

```cpp
void clear() noexcept;
```

## <a name="pathcompare"></a><a name="compare"></a> path：： compare

第一个函数返回 `mypath.compare(pval.native())`。 第二个函数返回 `mypath.compare(str)`。 第三个函数返回 `mypath.compare(ptr)` 。

```cpp
int compare(const path& pval) const noexcept;
int compare(const string_type& str) const;
int compare(const value_type *ptr) const;
```

### <a name="parameters"></a>参数

*pval*\
要比较的路径。

*字符串*\
要比较的字符串。

*ptr*\
要比较的指针。

## <a name="pathconcat"></a><a name="concat"></a> path：： concat

将指定序列追加到 `mypath` ，转换 (但不根据需要插入分隔符) 。

```cpp
template <class Source>
path& concat(const Source& source);

template <class InIt>
path& concat(InIt first, InIt last);
```

### <a name="parameters"></a>参数

*源程序*\
指定的序列。

*1*\
指定序列的开头。

*时间*\
指定序列的末尾。

## <a name="pathconst_iterator"></a><a name="const_iterator"></a> path：： const_iterator

`iterator` 的同义词。

```cpp
typedef iterator const_iterator;
```

## <a name="pathempty"></a><a name="empty"></a> 路径：： empty

返回 `mypath.empty()`。

```cpp
bool empty() const noexcept;
```

## <a name="pathend"></a><a name="end"></a> path：： end

返回类型的序列末迭代器 `iterator` 。

```cpp
iterator end() const;
```

## <a name="pathextension"></a><a name="extension"></a> path：： extension

返回的后缀 `filename()` 。

```cpp
path extension() const;
```

### <a name="remarks"></a>注解

返回的后缀 `filename() X` ，以：

如果 `X == path(".") || X == path("..")` 或如果 `X` 不包含任何点，则后缀为空。

否则，前缀以最右侧的点开头（并将其包含在内）。

## <a name="pathfilename"></a><a name="filename"></a> path：： filename

返回 myname 的根目录组件，尤其是 `empty() path() : *--end()`。 组件可能为空。

```cpp
path filename() const;
```

## <a name="pathgeneric_string"></a><a name="generic_string"></a> path：： generic_string

返回 `this->string<Elem, Traits, Alloc>(al)` ，其中（在 Windows 下）任何反斜杠均转换为正斜杠。

```cpp
template <class Elem,
    class Traits = char_traits<Elem>,
    class Alloc = allocator<Elem>>
  basic_string<Elem, Traits, Alloc>
    generic_string(const Alloc& al = Alloc()) const;

string generic_string() const;
```

## <a name="pathgeneric_u16string"></a><a name="generic_u16string"></a> path：： generic_u16string

返回 `u16string()` ，其中（在 Windows 下）任何反斜杠均转换为正斜杠。

```cpp
u16string generic_u16string() const;
```

## <a name="pathgeneric_u32string"></a><a name="generic_u32string"></a> path：： generic_u32string

返回 `u32string()` ，其中（在 Windows 下）任何反斜杠均转换为正斜杠。

```cpp
u32string generic_u32string() const;
```

## <a name="pathgeneric_u8string"></a><a name="generic_u8string"></a> path：： generic_u8string

返回 `u8string()` ，其中（在 Windows 下）任何反斜杠均转换为正斜杠。

```cpp
string generic_u8string() const;
```

## <a name="pathgeneric_wstring"></a><a name="generic_wstring"></a> path：： generic_wstring

返回 `wstring()` ，其中（在 Windows 下）任何反斜杠均转换为正斜杠。

```cpp
wstring generic_wstring() const;
```

## <a name="pathhas_extension"></a><a name="has_extension"></a> path：： has_extension

返回 `!extension().empty()`。

```cpp
bool has_extension() const;
```

## <a name="pathhas_filename"></a><a name="has_filename"></a> path：： has_filename

返回 `!filename().empty()`。

```cpp
bool has_filename() const;
```

## <a name="pathhas_parent_path"></a><a name="has_parent_path"></a> path：： has_parent_path

返回 `!parent_path().empty()`。

```cpp
bool has_parent_path() const;
```

## <a name="pathhas_relative_path"></a><a name="has_relative_path"></a> path：： has_relative_path

返回 `!relative_path().empty()`。

```cpp
bool has_relative_path() const;
```

## <a name="pathhas_root_directory"></a><a name="has_root_directory"></a> path：： has_root_directory

返回 `!root_directory().empty()`。

```cpp
bool has_root_directory() const;
```

## <a name="pathhas_root_name"></a><a name="has_root_name"></a> path：： has_root_name

返回 `!root_name().empty()`。

```cpp
bool has_root_name() const;
```

## <a name="pathhas_root_path"></a><a name="has_root_path"></a> path：： has_root_path

返回 `!root_path().empty()`。

```cpp
bool has_root_path() const;
```

## <a name="pathhas_stem"></a><a name="has_stem"></a> path：： has_stem

返回 `!stem().empty()`。

```cpp
bool has_stem() const;
```

## <a name="pathis_absolute"></a><a name="is_absolute"></a> path：： is_absolute

对于 Windows，函数返回 `has_root_name() && has_root_directory()` 。 对于 POSIX，函数返回 `has_root_directory()` 。

```cpp
bool is_absolute() const;
```

## <a name="pathis_relative"></a><a name="is_relative"></a> path：： is_relative

返回 `!is_absolute()`。

```cpp
bool is_relative() const;
```

## <a name="pathiterator"></a><a name="iterator"></a> path：： iterator

指定的路径组件的双向常量迭代器 `myname` 。

```cpp
class iterator
   {
   // bidirectional iterator for path
   typedef bidirectional_iterator_tag iterator_category;
   typedef path_type value_type;
   typedef ptrdiff_t difference_type;
   typedef const value_type *pointer;
   typedef const value_type& reference;
   // ...
   };
```

### <a name="remarks"></a>注解

类描述了一个双向常量迭代器，该迭代器指定 `path` `myname` 序列中的组件：

1. 根名称（如存在）

1. 根目录（如存在）

1. 父项的其余目录元素 `path` （如果存在），以文件名结尾（如果存在）

对于 `pval` 类型为的对象 `path` ：

1. `path::iterator X = pval.begin()` 指定路径名中的第一个 `path` 元素（如果存在）。

1. `X == pval.end()` 当 `X` 点刚越过组件序列的末尾时，为 true。

1. `*X` 返回与当前组件匹配的字符串

1. `++X` 指定序列中的下一个组件（如果存在）。

1. `--X` 指定序列中的前面的组件（如果存在）。

1. 更改会 `myname` 使在中指定元素的所有迭代器失效 `myname` 。

## <a name="pathmake_preferred"></a><a name="make_preferred"></a> path：： make_preferred

根据需要将每个分隔符转换为 `preferred_separator` 。

```cpp
path& make_preferred();
```

## <a name="pathnative"></a><a name="native"></a> 路径：： native

返回 `myname`。

```cpp
const string_type& native() const noexcept;
```

## <a name="pathoperator"></a><a name="op_as"></a> path：： operator =

将路径的元素替换为另一个路径的副本。

```cpp
path& operator=(const path& right);
path& operator=(path&& right) noexcept;

template <class Source>
path& operator=(const Source& source);
```

### <a name="parameters"></a>参数

*然后*\
要[path](../standard-library/path-class.md)复制到中的路径 `path` 。

*源程序*\
源路径。

### <a name="remarks"></a>注解

第一个成员运算符将复制 `right.myname` 到 `myname` 。 第二个成员运算符移 `right.myname` 到 `myname` 。 第三个成员运算符的行为与相同 `*this = path(source)` 。

## <a name="pathoperator"></a><a name="op_add"></a> path：： operator + =

各种 `concat` 表达式。

```cpp
path& operator+=(const path& right);
path& operator+=(const string_type& str);
path& operator+=(const value_type *ptr);
path& operator+=(value_type elem);

template <class Source>
path& operator+=(const Source& source);

template <class Elem>
path& operator+=(Elem elem);
```

### <a name="parameters"></a>参数

*然后*\
添加的路径。

*字符串*\
添加的字符串。

*ptr*\
添加的指针。

*elem*\
添加的 `value_type` 或 `Elem` 。

*源程序*\
添加的源。

### <a name="remarks"></a>注解

成员函数的行为与以下相应表达式相同：

1. `concat(right);`

1. `concat(path(str));`

1. `concat(ptr);`

1. `concat(string_type(1, elem));`

1. `concat(source);`

1. `concat(path(basic_string<Elem>(1, elem)));`

## <a name="pathoperator"></a><a name="op_divide"></a> path：： operator/=

各种 `append` 表达式。

```cpp
path& operator/=(const path& right);

template <class Source>
path& operator/=(const Source& source);
```

### <a name="parameters"></a>参数

*然后*\
添加的路径。

*源程序*\
添加的源。

### <a name="remarks"></a>注解

成员函数的行为与以下相应表达式相同：

1. `append(right);`

1. `append(source);`

## <a name="pathoperator-string_type"></a><a name="op_string"></a> path：： operator string_type

返回 `myname`。

```cpp
operator string_type() const;
```

## <a name="pathparent_path"></a><a name="parent_path"></a> 路径：:p arent_path

返回的父路径组件 `myname` 。

```cpp
path parent_path() const;
```

### <a name="remarks"></a>注解

返回的父路径组件 `myname` ，尤其是 `myname` 删除后的前缀 `filename().native()` 和任何前面紧邻的目录分隔符。  (相等，则 `begin() != end()` 为，如果是，则为通过连续应用的范围中的所有元素 `[begin(), --end())` `operator/=` 。 ) 组件可能为空。

## <a name="pathpath"></a><a name="path"></a> 路径：:p a

`path`以各种方式构造。

```cpp
path();

path(const path& right);
path(path&& right) noexcept;

template <class Source>
path(const Source& source);

template <class Source>
path(const Source& source, const locale& loc);

template <class InIt>
path(InIt first, InIt last);

template <class InIt>
path(InIt first, InIt last, const locale& loc);
```

### <a name="parameters"></a>参数

*然后*\
构造的路径要作为其副本的路径。

*源程序*\
构造的路径要作为其副本的源。

*loc*\
指定的区域设置。

*1*\
要复制的第一个元素的位置。

*时间*\
要复制的最后一个元素的位置。

### <a name="remarks"></a>注解

构造函数均 `myname` 以各种方式构造：

对于， `path()` 它是 `myname()` 。

`path(const path& right`) `myname(right.myname)` 。

对于， `path(path&& right)` 它是 `myname(right.myname)` 。

对于， `template<class Source> path(const Source& source)` 它是 `myname(source)` 。

为 `template<class Source> path(const Source& source, const locale& loc)` 此 `myname(source)` ，请从中获取所需的任何 codecvt facet `loc` 。

对于， `template<class InIt> path(InIt first, InIt last)` 它是 `myname(first, last)` 。

为 `template<class InIt> path(InIt first, InIt last, const locale& loc)` 此 `myname(first, last)` ，请从中获取所需的任何 codecvt facet `loc` 。

## <a name="pathpreferred_separator"></a><a name="preferred_separator"></a> 路径：:p referred_separator

常量对象提供首选字符来分隔路径组件，具体取决于主机操作系统。

```cpp
#if _WIN32_C_LIB
static constexpr value_type preferred_separator == L'\\';
#else // assume POSIX
static constexpr value_type preferred_separator == '/';
#endif // filesystem model now defined
```

### <a name="remarks"></a>注解

请注意，在 Windows 下的大多数上下文中，同样允许在它的位置使用 L'/'。

## <a name="pathrelative_path"></a><a name="relative_path"></a> path：： relative_path

返回的相对路径组件 `myname` 。

```cpp
path relative_path() const;
```

### <a name="remarks"></a>注解

返回的相对路径组件 `myname` ，尤其是 `myname` 删除后的后缀 `root_path().native()` 和任何紧随其后的冗余目录分隔符。 组件可能为空。

## <a name="pathremove_filename"></a><a name="remove_filename"></a> path：： remove_filename

删除文件名。

```cpp
path& remove_filename();
```

## <a name="pathreplace_extension"></a><a name="replace_extension"></a> path：： replace_extension

替换的扩展 `myname` 。

```cpp
path& replace_extension(const path& newext = path());
```

### <a name="parameters"></a>参数

*newext*\
新扩展。

### <a name="remarks"></a>注解

首先删除中的 `extension().native()` 后缀 `myname` 。 如果 `!newext.empty() && newext[0] != dot` (where `dot` `*path(".").c_str()`) ，则 `dot` 将追加到 `myname` 。 然后将 *newext* 追加到 `myname` 。

## <a name="pathreplace_filename"></a><a name="replace_filename"></a> path：： replace_filename

替换文件名。

```cpp
path& replace_filename(const path& pval);
```

### <a name="parameters"></a>参数

*pval*\
文件名的路径。

### <a name="remarks"></a>注解

成员函数执行：

```cpp
remove_filename();

*this /= pval;
return (*this);
```

## <a name="pathroot_directory"></a><a name="root_directory"></a> path：： root_directory

返回的根目录组件 `myname` 。

```cpp
path root_directory() const;
```

### <a name="remarks"></a>注解

组件可能为空。

## <a name="pathroot_name"></a><a name="root_name"></a> path：： root_name

返回的根名称组件 `myname` 。

```cpp
path root_name() const;
```

### <a name="remarks"></a>注解

组件可能为空。

## <a name="pathroot_path"></a><a name="root_path"></a> path：： root_path

返回的根路径组件 `myname` 。

```cpp
path root_path() const;
```

### <a name="remarks"></a>注解

返回的根路径组件 `myname` ，尤其是 `root_name()`  /  `root_directory` 。 组件可能为空。

## <a name="pathstem"></a><a name="stem"></a> path：：词干

返回的 `stem` 组件 `myname` 。

```cpp
path stem() const;
```

### <a name="remarks"></a>注解

返回的 `stem` 组件 `myname` ，尤其是 `filename().native()` 删除了任何尾随 `extension().native()` 。 组件可能为空。

## <a name="pathstring"></a><a name="string"></a> path：： string

转换中存储的序列 `mypath` 。

```cpp
template \<class Elem, class Traits = char_traits\<Elem>, class Alloc = allocator\<Elem>>
basic_string\<Elem, Traits, Alloc> string(const Alloc& al = Alloc()) const;
string string() const;
```

### <a name="remarks"></a>注解

第一个 (模板) 成员函数按照与相同的方式转换存储的顺序 `mypath` ：

1. 若 `string<char, Traits, Alloc>()`，表示集 `string()`

1. 若 `string<wchar_t, Traits, Alloc>()`，表示集 `wstring()`

1. 若 `string<char16_t, Traits, Alloc>()`，表示集 `u16string()`

1. 若 `string<char32_t, Traits, Alloc>()`，表示集 `u32string()`

第二个成员函数将存储在中的序列转换 `mypath` 为序列的主机系统所采用的编码 **`char`** ，并将其返回存储在类型的对象中 `string` 。

## <a name="pathstring_type"></a><a name="string_type"></a> path：： string_type

该类型是 `basic_string<value_type>` 的同义词。

```cpp
typedef basic_string<value_type> string_type;
```

## <a name="pathswap"></a><a name="swap"></a> path：： swap

执行 `swap(mypath, right.mypath)` 。

```cpp
void swap(path& right) noexcept;
```

## <a name="pathu16string"></a><a name="u16string"></a> path：： u16string

将中存储的序列转换 `mypath` 为 utf-16，并将其返回存储在类型的对象中 `u16string` 。

```cpp
u16string u16string() const;
```

## <a name="pathu32string"></a><a name="u32string"></a> path：： u32string

将中存储的序列转换 `mypath` 为 32 utf-8，并将其返回存储在类型的对象中 `u32string` 。

```cpp
u32string u32string() const;
```

## <a name="pathu8string"></a><a name="u8string"></a> path：： u8string

将中存储的序列转换 `mypath` 为 utf-8，并将其返回存储在类型的对象中 `u8string` 。

```cpp
string u8string() const;
```

## <a name="pathvalue_type"></a><a name="value_type"></a> path：： value_type

类型描述了 `path` 主机操作系统欢迎使用的元素。

```cpp
#if _WIN32_C_LIB
typedef wchar_t value_type;
#else // assume POSIX
typedef char value_type;
#endif // filesystem model now defined
```

## <a name="pathwstring"></a><a name="wstring"></a> path：： wstring

将中存储的序列转换 `mypath` 为序列的主机系统所用的编码 **`wchar_t`** ，并将其返回存储在类型的对象中 `wstring` 。

```cpp
wstring wstring() const;
```

## <a name="see-also"></a>另请参阅

[头文件引用](../standard-library/cpp-standard-library-header-files.md)
