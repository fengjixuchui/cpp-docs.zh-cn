---
title: piecewise_constant_distribution 类
ms.date: 11/04/2016
f1_keywords:
- random/std::piecewise_constant_distribution
- random/std::piecewise_constant_distribution::reset
- random/std::piecewise_constant_distribution::intervals
- random/std::piecewise_constant_distribution::densities
- random/std::piecewise_constant_distribution::param
- random/std::piecewise_constant_distribution::min
- random/std::piecewise_constant_distribution::max
- random/std::piecewise_constant_distribution::operator()
- random/std::piecewise_constant_distribution::param_type
- random/std::piecewise_constant_distribution::param_type::intervals
- random/std::piecewise_constant_distribution::param_type::densities
- random/std::piecewise_constant_distribution::param_type::operator==
- random/std::piecewise_constant_distribution::param_type::operator!=
helpviewer_keywords:
- std::piecewise_constant_distribution [C++]
- std::piecewise_constant_distribution [C++], reset
- std::piecewise_constant_distribution [C++], intervals
- std::piecewise_constant_distribution [C++], densities
- std::piecewise_constant_distribution [C++], param
- std::piecewise_constant_distribution [C++], min
- std::piecewise_constant_distribution [C++], max
- std::piecewise_constant_distribution [C++], param_type
- std::piecewise_constant_distribution [C++], param_type
ms.assetid: 2c9a21fa-623e-4d63-b827-3f1556b6dedb
ms.openlocfilehash: 59911d8a61a05de9ec92f4152b2835da3cf82f7f
ms.sourcegitcommit: ec6dd97ef3d10b44e0fedaa8e53f41696f49ac7b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2020
ms.locfileid: "88832719"
---
# <a name="piecewise_constant_distribution-class"></a>piecewise_constant_distribution 类

生成包含以等概率分布在每个区间中的不等宽区间的分段常数分布。

## <a name="syntax"></a>语法

```cpp
template<class RealType = double>
class piecewise_constant_distribution
   {
public:
   // types
   typedef RealType result_type;
   struct param_type;

   // constructor and reset functions
   piecewise_constant_distribution();
   template <class InputIteratorI, class InputIteratorW>
   piecewise_constant_distribution(
       InputIteratorI firstI, InputIteratorI lastI, InputIteratorW firstW);
   template <class UnaryOperation>
   piecewise_constant_distribution(
      initializer_list<result_type> intervals, UnaryOperation weightfunc);
   template <class UnaryOperation>
   piecewise_constant_distribution(
      size_t count, result_type xmin, result_type xmax, UnaryOperation weightfunc);
   explicit piecewise_constant_distribution(const param_type& parm);
   void reset();

   // generating functions
   template <class URNG>
   result_type operator()(URNG& gen);
   template <class URNG>
   result_type operator()(URNG& gen, const param_type& parm);

   // property functions
   vector<result_type> intervals() const;
   vector<result_type> densities() const;
   param_type param() const;
   void param(const param_type& parm);
   result_type min() const;
   result_type max() const;
   };
```

### <a name="parameters"></a>参数

*RealType*\
浮点结果类型，默认为 **`double`** 。 有关可能的类型，请参阅 [\<random>](../standard-library/random.md) 。

## <a name="remarks"></a>注解

此示例分布包含以等概率分布在每个区间中的不等宽区间。 有关其他采样分布的信息，请参阅 [piecewise_linear_distribution 类](../standard-library/piecewise-linear-distribution-class.md)和 [discrete_distribution](../standard-library/discrete-distribution-class.md)。

下表链接到有关各个成员的文章：

[piecewise_constant_distribution](#piecewise_constant_distribution)\
[param_type](#param_type)

属性函数 `intervals()` 将返回 `vector<result_type>` 以及分布的存储区间集。

属性函数 `densities()` 将返回 `vector<result_type>` 以及每个区间集的存储密度，根据构造函数参数中提供的权重计算存储密度。

属性成员 `param()` 将设置或返回 `param_type` 存储的分布参数包。

`min()` 和 `max()` 成员函数将分别返回最小可能结果和最大可能结果。

`reset()` 成员函数将放弃所有缓存的值，使下一个对 `operator()` 的调用的结果不取决于在调用之前从引擎获得的任何值。

`operator()` 成员函数将根据 URNG 引擎，从当前参数包或指定参数包返回下一个生成的值。

有关分布类及其成员的详细信息，请参阅 [\<random>](../standard-library/random.md) 。

## <a name="example"></a>示例

```cpp
// compile with: /EHsc /W4
#include <random>
#include <iostream>
#include <iomanip>
#include <string>
#include <map>

using namespace std;

void test(const int s) {

    // uncomment to use a non-deterministic generator
    // random_device rd;
    // mt19937 gen(rd());
    mt19937 gen(1701);

    // Three intervals, non-uniform: 0 to 1, 1 to 6, and 6 to 15
    vector<double> intervals{ 0, 1, 6, 15 };
    // weights determine the densities used by the distribution
    vector<double> weights{ 1, 5, 10 };

    piecewise_constant_distribution<double> distr(intervals.begin(), intervals.end(), weights.begin());

    cout << endl;
    cout << "min() == " << distr.min() << endl;
    cout << "max() == " << distr.max() << endl;
    cout << "intervals (index: interval):" << endl;
    vector<double> i = distr.intervals();
    int counter = 0;
    for (const auto& n : i) {
        cout << fixed << setw(11) << counter << ": " << setw(14) << setprecision(10) << n << endl;
        ++counter;
    }
    cout << endl;
    cout << "densities (index: density):" << endl;
    vector<double> d = distr.densities();
    counter = 0;
    for (const auto& n : d) {
        cout << fixed << setw(11) << counter << ": " << setw(14) << setprecision(10) << n << endl;
        ++counter;
    }
    cout << endl;

    // generate the distribution as a histogram
    map<int, int> histogram;
    for (int i = 0; i < s; ++i) {
        ++histogram[distr(gen)];
    }

    // print results
    cout << "Distribution for " << s << " samples:" << endl;
    for (const auto& elem : histogram) {
        cout << setw(5) << elem.first << '-' << elem.first+1 << ' ' << string(elem.second, ':') << endl;
    }
    cout << endl;
}

int main()
{
    int samples = 100;

    cout << "Use CTRL-Z to bypass data entry and run using default values." << endl;
    cout << "Enter an integer value for the sample count: ";
    cin >> samples;

    test(samples);
}
```

```Output
Use CTRL-Z to bypass data entry and run using default values.
Enter an integer value for the sample count: 100
min() == 0
max() == 15
intervals (index: interval):
          0:   0.0000000000
          1:   1.0000000000
          2:   6.0000000000
          3:  15.0000000000
densities (index: density):
          0:   0.0625000000
          1:   0.0625000000
          2:   0.0694444444
Distribution for 100 samples:
    0-1 :::::::
    1-2 ::::::
    2-3 :::::
    3-4 ::::::
    4-5 :::::::
    5-6 ::::::
    6-7 :::
    7-8 ::::::::::
    8-9 ::::::
    9-10 ::::::::::::
    10-11 :::::
    11-12 ::::::
    12-13 :::::::::
    13-14 ::::
    14-15 ::::::::
```

## <a name="requirements"></a>要求

**标头：**\<random>

**命名空间:** std

## <a name="piecewise_constant_distributionpiecewise_constant_distribution"></a><a name="piecewise_constant_distribution"></a> piecewise_constant_distribution：:p iecewise_constant_distribution

构造分布。

```cpp
// default constructor
piecewise_constant_distribution();

// constructs using a range of intervals, [firstI, lastI), with
// matching weights starting at firstW
template <class InputIteratorI, class InputIteratorW>
piecewise_constant_distribution(InputIteratorI firstI, InputIteratorI lastI, InputIteratorW firstW);

// constructs using an initializer list for range of intervals,
// with weights generated by function weightfunc
template <class UnaryOperation>
piecewise_constant_distribution(initializer_list<RealType>
intervals, UnaryOperation weightfunc);

// constructs using an initializer list for range of count intervals,
// distributed uniformly over [xmin,xmax] with weights generated by function weightfunc
template <class UnaryOperation>
piecewise_constant_distribution(size_t count, RealType xmin, RealType xmax, UnaryOperation weightfunc);

// constructs from an existing param_type structure
explicit piecewise_constant_distribution(const param_type& parm);
```

### <a name="parameters"></a>参数

*firstI*\
分布范围中第一个元素的输入迭代器。

*lastI*\
分布范围中最后一个元素的输入迭代器。

*firstW*\
权重范围中第一个元素的输入迭代器。

*间隔*\
具有分布区间的 [initializer_list](../cpp/initializers.md)。

*计*\
分布范围中的元素数。

xmin\
分布范围中的最低值。

xmax\
分布范围中的最高值。 必须大于 *xmin*。

*weightfunc*\
表示分布的概率函数的对象。 参数和返回值都必须可转换为 **`double`** 。

*parm*\
用于构造分布的参数结构。

### <a name="remarks"></a>注解

默认构造函数将设置存储参数，以便存在一个概率密度为 1 的 0 到 1 的区间。

迭代器范围构造函数

```cpp
template <class InputIteratorI, class InputIteratorW>
piecewise_constant_distribution(InputIteratorI firstI, InputIteratorI lastI,
    InputIteratorW firstW);
```

使用序列 [`firstI`，`lastI`) 上迭代器中的区间和以 `firstW` 开始的匹配权重序列来构造分布对象。

初始值设定项列表构造函数

```cpp
template <class UnaryOperation>
piecewise_constant_distribution(initializer_list<result_type>
intervals,
    UnaryOperation weightfunc);
```

使用从函数*weightfunc*生成的初始值设定项列表*间隔*和权重的间隔构造分布对象。

定义为以下内容的构造函数

```cpp
template <class UnaryOperation>
piecewise_constant_distribution(size_t count, result_type xmin, result_type xmax,
    UnaryOperation weightfunc);
```

构造一个在 [] 上均匀分布的 *计数* 间隔的分布对象 `xmin,xmax` ，根据函数 *weightfunc*分配每个间隔权重，而 *weightfunc* 必须接受一个参数并具有一个返回值，这两个参数都可转换为 **`double`** 。 **前提条件：**`xmin < xmax`

定义为以下内容的构造函数

```cpp
explicit piecewise_constant_distribution(const param_type& parm);
```

使用 *parm* 作为存储的参数结构，构造分布对象。

## <a name="piecewise_constant_distributionparam_type"></a><a name="param_type"></a> piecewise_constant_distribution：:p aram_type

存储分布的所有参数。

```cpp
struct param_type {
   typedef piecewise_constant_distribution<result_type> distribution_type;
   param_type();
   template <class IterI, class IterW>
   param_type(IterI firstI, IterI lastI, IterW firstW);
   template <class UnaryOperation>
   param_type(size_t count, result_type xmin, result_type xmax, UnaryOperation weightfunc);
   std::vector<result_type> densities() const;
   std::vector<result_type> intervals() const;

   bool operator==(const param_type& right) const;
   bool operator!=(const param_type& right) const;
   };
```

### <a name="parameters"></a>参数

请参阅 [piecewise_constant_distribution](#piecewise_constant_distribution) 的构造函数参数。

### <a name="remarks"></a>注解

**前提条件：**`xmin < xmax`

在实例化时，可将此结构传递给分布的类构造函数、传递给 `param()` 成员函数以设置现有分布的存储参数，并传递给 `operator()` 以代替存储参数使用。

## <a name="see-also"></a>另请参阅

[\<random>](../standard-library/random.md)\
[piecewise_linear_distribution](../standard-library/piecewise-linear-distribution-class.md)
