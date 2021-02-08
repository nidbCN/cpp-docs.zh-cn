---
title: multiset 类
description: C + + 标准模板库的 API 参考 (STL) `multiset` 类，用于存储和检索集合中的数据，其中所含元素的值不需要是唯一的，它们作为数据自动排序所依据的键值。
ms.date: 9/9/2020
f1_keywords:
- set/std::multiset
- set/std::multiset::allocator_type
- set/std::multiset::const_iterator
- set/std::multiset::const_pointer
- set/std::multiset::const_reference
- set/std::multiset::const_reverse_iterator
- set/std::multiset::difference_type
- set/std::multiset::iterator
- set/std::multiset::key_compare
- set/std::multiset::key_type
- set/std::multiset::pointer
- set/std::multiset::reference
- set/std::multiset::reverse_iterator
- set/std::multiset::size_type
- set/std::multiset::value_compare
- set/std::multiset::value_type
- set/std::multiset::begin
- set/std::multiset::cbegin
- set/std::multiset::cend
- set/std::multiset::clear
- set/std::multiset::contains
- set/std::multiset::count
- set/std::multiset::crbegin
- set/std::multiset::crend
- set/std::multiset::emplace
- set/std::multiset::emplace_hint
- set/std::multiset::empty
- set/std::multiset::end
- set/std::multiset::equal_range
- set/std::multiset::erase
- set/std::multiset::find
- set/std::multiset::get_allocator
- set/std::multiset::insert
- set/std::multiset::key_comp
- set/std::multiset::lower_bound
- set/std::multiset::max_size
- set/std::multiset::rbegin
- set/std::multiset::rend
- set/std::multiset::size
- set/std::multiset::swap
- set/std::multiset::upper_bound
- set/std::multiset::value_comp
helpviewer_keywords:
- std::multiset [C++]
- std::multiset [C++], allocator_type
- std::multiset [C++], const_iterator
- std::multiset [C++], const_pointer
- std::multiset [C++], const_reference
- std::multiset [C++], const_reverse_iterator
- std::multiset [C++], difference_type
- std::multiset [C++], iterator
- std::multiset [C++], key_compare
- std::multiset [C++], key_type
- std::multiset [C++], pointer
- std::multiset [C++], reference
- std::multiset [C++], reverse_iterator
- std::multiset [C++], size_type
- std::multiset [C++], value_compare
- std::multiset [C++], value_type
- std::multiset [C++], begin
- std::multiset [C++], cbegin
- std::multiset [C++], cend
- std::multiset [C++], clear
- std::multiset [C++], contains
- std::multiset [C++], count
- std::multiset [C++], crbegin
- std::multiset [C++], crend
- std::multiset [C++], emplace
- std::multiset [C++], emplace_hint
- std::multiset [C++], empty
- std::multiset [C++], end
- std::multiset [C++], equal_range
- std::multiset [C++], erase
- std::multiset [C++], find
- std::multiset [C++], get_allocator
- std::multiset [C++], insert
- std::multiset [C++], key_comp
- std::multiset [C++], lower_bound
- std::multiset [C++], max_size
- std::multiset [C++], rbegin
- std::multiset [C++], rend
- std::multiset [C++], size
- std::multiset [C++], swap
- std::multiset [C++], upper_bound
- std::multiset [C++], value_comp
ms.openlocfilehash: e89c1ed44a7f467223d443b2ba24bb88227b1bab
ms.sourcegitcommit: 651f817a6c8e92211168d34f0542350559f436d5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/08/2021
ms.locfileid: "99823301"
---
# <a name="multiset-class"></a>`multiset` 类

C++ 标准库多重集合类用于存储和检索集合中的数据，此集合中包含的元素值无需唯一，并且用作数据自动排序所依据的键值。 不能直接更改中的元素的键值 `multiset` 。 必须先删除旧值，才能插入具有新值的元素。

## <a name="syntax"></a>语法

```cpp
template <class Key, class Compare =less <Key>, class Allocator =allocator <Key>>
class multiset
```

### <a name="parameters"></a>parameters

*`Key`*\
要在 `multiset` 中存储的元素数据类型。

*`Compare`*\
提供一个函数对象的类型，该函数对象可将两个元素值作为排序键进行比较，以确定它们在中的相对顺序 `multiset` 。 小于二进制谓词 \<Key> 是默认值。

在 c + + 14 中，可以通过指定 `std::less<>` 没有类型参数的或谓词来启用异类查找 `std::greater<>` 。 有关详细信息，请参阅 [关联容器中的异类查找](../standard-library/stl-containers.md#sequence_containers) 。

*`Allocator`*\
表示存储分配器对象的类型，该对象封装有关 `multiset` 的内存分配和解除分配的详细信息。 默认值为 `allocator<Key>`。

## <a name="remarks"></a>备注

C + + 标准库 `multiset` 类是：

- 大小可变的关联容器，支持基于关联键值高效检索元素值。

- 可逆，因为它提供双向迭代器来访问其元素。

- 有序，因为它的元素在容器中根据指定的比较函数按键值排序。

- 多个，它的元素不需要具有唯一键，因此一个键值可具有多个关联的元素值。

- 简单的关联容器，因为它的元素值即为它的键值。

- 类模板，因为它提供的功能是通用的，因此与作为元素包含的特定数据类型无关。 可使用的数据类型作为类模板以及比较函数和分配器中的参数指定。

类提供的迭代器 `multiset` 是双向迭代器，但类成员函数 [`insert`](#insert) 和具有将 [`multiset`](#multiset) 较弱输入迭代器作为模板参数的版本，较弱输入迭代器的功能要求比双向迭代器类保证的功能要求更少。 不同的迭代器概念形成一个系列，通过它们的功能优化相关联。 每个迭代器概念有它自己的一套要求，使用这些概念的算法必须根据迭代器类型提供的要求限制它们的假设。 可以假定输入迭代器可取消引用以引用某个对象，并可递增到序列中的下一迭代器。 这是最小的功能集，但足以按有意义的方式提供类成员函数的上下文中的迭代器范围 [`First`, `Last`)。

容器类型选择通常应根据应用程序所需的搜索和插入的类型。 关联容器针对查找、插入和移除操作进行了优化。 显式支持这些操作的成员函数是有效的，执行这些操作的时间与容器中元素数的对数成正比。 插入元素不会使迭代器失效，移除元素仅会使指向已移除元素的迭代器失效。

`multiset`当应用程序满足将值与其键关联的条件时，应为选择的关联容器。 的元素 `multiset` 可以是多个，并用作其自己的排序键，因此键不是唯一的。 此类结构的模型是排序列表，如关键字排序列表，其中关键字可以出现多次。 如果不允许关键字多次出现，则应使用集作为适当的容器结构。 如果将唯一定义作为值附加到唯一关键字的列表，则映射应为包含此数据的适当结构。 如果定义不唯一，则是 `multimap` 所选容器。

`multiset`通过调用存储的类型的函数对象，对它控制的序列进行排序 *`Compare`* 。 此存储的对象是比较函数，可通过调用成员函数访问 [`key_comp`](#key_comp) 。 通常，元素只需小于比较才能建立此顺序：因此，给定任意两个元素，可以确定它们是等效的 (，这二者都不小于另一个) 或一个小于另一个。 这将导致在非等效元素之间进行排序。 在技术性更强的说明中，比较函数是一个二元谓词，在标准数学的意义上引发严格弱排序。 二元谓词 *f* (*x*， *y*) 是具有两个参数对象（ *x* 和 *y* ）和一个返回值（或）的函数对象 **`true`** **`false`** 。 如果二元谓词是具有自反、对称性和可传递的，且等效可传递，对集进行的排序将为严格弱排序，其中两个对象 x 和 y 定义为在 *f* (*x、y*) 和 *f* (*y，x*) 均为 false 时等效。 如果键之间的更强相等条件取代了等效性，则排序将为总排序（即所有元素彼此排序），并且匹配的键将难以彼此辨别。

在 c + + 14 中，可以通过指定 `std::less<>` 没有类型参数的或谓词来启用异类查找 `std::greater<>` 。 有关详细信息，请参阅 [关联容器中的异类查找](../standard-library/stl-containers.md#sequence_containers) 。

### <a name="constructors"></a>构造函数

|构造函数|说明|
|-|-|
|[多重](#multiset)|构造一个空的或者是指定 `multiset` 的全部或部分副本的 `multiset`。|

### <a name="typedefs"></a>Typedef

| 类型名称 | 描述 |
|--|--|
| [`allocator_type`](#allocator_type) | `allocator` 对象的 `multiset` 类的 typedef。 |
| [`const_iterator`](#const_iterator) | 可读取中的元素的双向迭代器的 typedef **`const`** `multiset` 。 |
| [`const_pointer`](#const_pointer) | 指向中的元素的指针的 typedef **`const`** `multiset` 。 |
| [`const_reference`](#const_reference) | 对 **`const`** 中存储的 `multiset` 用于读取和执行操作的元素的引用的 typedef **`const`** 。 |
| [`const_reverse_iterator`](#const_reverse_iterator) | 可读取中的任何元素的双向迭代器的 typedef **`const`** `multiset` 。 |
| [`difference_type`](#difference_type) | `multiset` 中迭代器指向的元素间范围内元素数量的有符号整数 typedef。 |
| [`iterator`](#iterator) | 可读取或修改 `multiset` 中任何元素的双向迭代器的 typedef。 |
| [`key_compare`](#key_compare) | 可比较两个键以确定 `multiset` 中两个元素的相对顺序的函数对象的 typedef。 |
| [`key_type`](#key_type) | 可比较两个排序键以确定 `multiset` 中两个元素的相对顺序的函数对象的 typedef。 |
| [`pointer`](#pointer) | 指向 `multiset` 中的元素的指针的 typedef。 |
| [`reference`](#reference) | 对 `multiset` 中存储的元素的引用的 typedef。 |
| [`reverse_iterator`](#reverse_iterator) | 可读取或修改反向 `multiset` 中的元素的双向迭代器的 typedef。 |
| [`size_type`](#size_type) | 可表示 `multiset` 中元素数量的无符号整数类型。 |
| [`value_compare`](#value_compare) | 可将两个元素作为排序键比较以确定它们在 `multiset` 中的相对顺序的函数对象的 typedef。 |
| [`value_type`](#value_type) | 一个 typedef，描述当 `multiset` 作为值时存储为元素的对象。 |

### <a name="member-functions"></a>成员函数

| 成员函数 | 说明 |
|--|--|
| [`begin`](#begin) | 返回一个迭代器，此迭代器指向 `multiset` 中的第一个元素。 |
| [`cbegin`](#cbegin) | 返回一个常量迭代器，此迭代器用于发现 `multiset` 中的第一个元素。 |
| [`cend`](#cend) | 返回一个常量迭代器，此迭代器用于发现 `multiset` 中最后一个元素之后的位置。 |
| [`clear`](#clear) | 清除 `multiset` 的所有元素。 |
| [`contains`](#contains)<sup>C + + 20</sup> | 检查中是否存在具有指定键的元素 `multiset` 。 |
| [`count`](#count) | 返回 `multiset` 中其键与指定为参数的键匹配的元素数量。 |
| [`crbegin`](#crbegin) | 返回一个常量迭代器，此迭代器用于发现反向 `multiset` 中的第一个元素。 |
| [`crend`](#crend) | 返回一个常量迭代器，此迭代器用于发现反向 `multiset` 中最后一个元素之后的位置。 |
| [`emplace`](#emplace) | 将就地构造的元素插入到 `multiset`。 |
| [`emplace_hint`](#emplace_hint) | 将就地构造的元素插入到 `multiset`，附带位置提示。 |
| [`empty`](#empty) | 测试 `multiset` 是否为空。 |
| [`end`](#end) | 返回一个迭代器，此迭代器指向 `multiset` 中最后一个元素之后的位置。 |
| [`equal_range`](#equal_range) | 返回一对迭代器。 此迭代器对中的第一个迭代器指向 `multiset` 中其键大于指定键的第一个元素。 此迭代器对中的第二个迭代器指向 `multiset` 中其键等于或大于指定键的第一个元素。 |
| [`erase`](#erase) | 从 `multiset` 中的指定位置移除一个元素或元素范围，或者移除与指定键匹配的元素。 |
| [`find`](#find) | 返回一个迭代器，此迭代器指向 `multiset` 中其键与指定键相等的元素的第一个位置。 |
| [`get_allocator`](#get_allocator) | 返回用于构造 `allocator` 的 `multiset` 对象的副本。 |
| [`insert`](#insert) | 将一个元素或元素范围插入到 `multiset`。 |
| [`key_comp`](#key_comp) | 提供一个函数对象，此函数对象可比较两个排序键以确定 `multiset` 中两个元素的相对顺序。 |
| [`lower_bound`](#lower_bound) | 返回一个迭代器，此迭代器指向 `multiset` 中其键等于或大于指定键的第一个元素。 |
| [`max_size`](#max_size) | 返回 `multiset` 的最大长度。 |
| [`rbegin`](#rbegin) | 返回一个迭代器，此迭代器指向反向 `multiset` 中的第一个元素。 |
| [`rend`](#rend) | 返回一个迭代器，此迭代器指向反向 `multiset` 中最后一个元素之后的位置。 |
| [`size`](#size) | 返回 `multiset` 中的元素数量。 |
| [`swap`](#swap) | 交换两个 `multiset` 的元素。 |
| [`upper_bound`](#upper_bound) | 返回一个迭代器，此迭代器指向 `multiset` 中其键大于指定键的第一个元素。 |
| [`value_comp`](#value_comp) | 检索用于对 `multiset` 中的元素值进行排序的比较对象副本。 |

### <a name="operators"></a>运算符

|操作员|说明|
|-|-|
|[`operator=`](#op_eq)|将一个 `multiset` 中的元素替换为另一 `multiset` 副本。|

## <a name="requirements"></a>要求

**标头：**`<set>`

**命名空间：** `std`

## <a name="multisetallocator_type"></a><a name="allocator_type"></a> `multiset::allocator_type`

表示对象的分配器类的类型 `multiset`

```cpp
typedef Allocator allocator_type;
```

### <a name="remarks"></a>备注

`allocator_type` 是模板参数 `Allocator` 的同义词。

有关 `Allocator` 的详细信息，请参阅 [multiset 类](../standard-library/multiset-class.md)主题的备注部分。

### <a name="example"></a>示例

[`get_allocator`](#get_allocator)有关使用的示例，请参阅示例`allocator_type`

## <a name="multisetbegin"></a><a name="begin"></a> `multiset::begin`

返回一个迭代器，此迭代器用于发现 `multiset` 中的第一个元素。

```cpp
const_iterator begin() const;

iterator begin();
```

### <a name="return-value"></a>返回值

一个双向迭代器，用于寻址中的第一个元素 `multiset` 或空多重集之后的位置。

### <a name="example"></a>示例

```cpp
// multiset_begin.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main( )
{
   using namespace std;
   multiset <int> ms1;
   multiset <int>::iterator ms1_Iter;
   multiset <int>::const_iterator ms1_cIter;

   ms1.insert( 1 );
   ms1.insert( 2 );
   ms1.insert( 3 );

   ms1_Iter = ms1.begin( );
   cout << "The first element of ms1 is " << *ms1_Iter << endl;

   ms1_Iter = ms1.begin( );
   ms1.erase( ms1_Iter );

   // The following 2 lines would err as the iterator is const
   // ms1_cIter = ms1.begin( );
   // ms1.erase( ms1_cIter );

   ms1_cIter = ms1.begin( );
   cout << "The first element of ms1 is now " << *ms1_cIter << endl;
}
```

```Output
The first element of ms1 is 1
The first element of ms1 is now 2
```

## <a name="multisetcbegin"></a><a name="cbegin"></a> `multiset::cbegin`

返回一个 **`const`** 迭代器，该迭代器用于寻址范围内的第一个元素。

```cpp
const_iterator cbegin() const;
```

### <a name="return-value"></a>返回值

**`const`** 双向访问迭代器，指向范围的第一个元素，或刚超出空范围末尾 (空范围) 的位置 `cbegin() == cend()` 。

### <a name="remarks"></a>备注

如果返回值为 `cbegin` ，则不能修改范围中的元素。

可以使用此成员函数替代 `begin()` 成员函数，以保证返回值为 `const_iterator`。 它一般与 [auto](../cpp/auto-cpp.md) 类型推导关键字联合使用，如下例所示。 在此示例中，将视为 `Container` 支持和的任何类型的可修改 (非 **`const`**) 容器 `begin()` `cbegin()` 。

```cpp
auto i1 = Container.begin();
// i1 is Container<T>::iterator
auto i2 = Container.cbegin();

// i2 is Container<T>::const_iterator
```

## <a name="multisetcend"></a><a name="cend"></a> `multiset::cend`

返回一个 **`const`** 迭代器，该迭代器用于寻址范围内最后一个元素之外的位置。

```cpp
const_iterator cend() const;
```

### <a name="return-value"></a>返回值

**`const`** 双向访问迭代器，它指向刚超出范围末尾的位置。

### <a name="remarks"></a>备注

`cend` 用于测试迭代器是否超过了其范围的末尾。

可以使用此成员函数替代 `end()` 成员函数，以保证返回值为 `const_iterator`。 它一般与 [auto](../cpp/auto-cpp.md) 类型推导关键字联合使用，如下例所示。 在此示例中，将视为 `Container` 支持和的任何类型的可修改 (非 **`const`**) 容器 `end()` `cend()` 。

```cpp
auto i1 = Container.end();
// i1 is Container<T>::iterator
auto i2 = Container.cend();

// i2 is Container<T>::const_iterator
```

返回的值 `cend` 不应被取消引用。

## <a name="multisetclear"></a><a name="clear"></a> `multiset::clear`

清除 `multiset` 的所有元素。

```cpp
void clear();
```

### <a name="example"></a>示例

```cpp
// multiset_clear.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main( )
{
   using namespace std;
   multiset <int> ms1;

   ms1.insert( 1 );
   ms1.insert( 2 );

   cout << "The size of the multiset is initially "
        << ms1.size( ) << "." << endl;

   ms1.clear( );
   cout << "The size of the multiset after clearing is "
        << ms1.size( ) << "." << endl;
}
```

```Output
The size of the multiset is initially 2.
The size of the multiset after clearing is 0.
```

## <a name="multisetconst_iterator"></a><a name="const_iterator"></a> `multiset::const_iterator`

一种类型，它提供可读取中的元素的双向迭代器 **`const`** `multiset` 。

```cpp
typedef implementation-defined const_iterator;
```

### <a name="remarks"></a>备注

类型 `const_iterator` 不能用于修改元素的值。

### <a name="example"></a>示例

[`begin`](#begin)有关使用的示例，请参阅的示例 `const_iterator` 。

## <a name="multisetconst_pointer"></a><a name="const_pointer"></a> `multiset::const_pointer`

一种类型，它提供指向中的元素的指针 **`const`** `multiset` 。

```cpp
typedef typename allocator_type::const_pointer const_pointer;
```

### <a name="remarks"></a>备注

类型 `const_pointer` 不能用于修改元素的值。

在大多数情况下，应使用 [迭代器](#iterator) 访问对象中的元素 `multiset` 。

## <a name="multisetconst_reference"></a><a name="const_reference"></a> `multiset::const_reference`

一种类型，它提供对 **`const`** 存储在中的元素 `multiset` 的引用，以便读取和执行 **`const`** 操作。

```cpp
typedef typename allocator_type::const_reference const_reference;
```

### <a name="example"></a>示例

```cpp
// multiset_const_ref.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main( )
{
   using namespace std;
   multiset <int> ms1;

   ms1.insert( 10 );
   ms1.insert( 20 );

   // Declare and initialize a const_reference &Ref1
   // to the 1st element
   const int &Ref1 = *ms1.begin( );

   cout << "The first element in the multiset is "
        << Ref1 << "." << endl;

   // The following line would cause an error because the
   // const_reference can't be used to modify the multiset
   // Ref1 = Ref1 + 5;
}
```

```Output
The first element in the multiset is 10.
```

## <a name="multisetconst_reverse_iterator"></a><a name="const_reverse_iterator"></a> `multiset::const_reverse_iterator`

一种类型，它提供可读取中的任何元素的双向迭代器 **`const`** `multiset` 。

```cpp
typedef std::reverse_iterator<const_iterator> const_reverse_iterator;
```

### <a name="remarks"></a>备注

类型 `const_reverse_iterator` 无法修改元素的值，它用于反向循环访问 `multiset` 。

### <a name="example"></a>示例

[`rend`](#rend)有关如何声明和使用的示例，请参见的示例 `const_reverse_iterator` 。

## <a name="multisetcontains"></a><a name="contains"></a> `multiset::contains`

检查中是否存在具有指定键的元素 `multiset` 。

```cpp
bool contains(const Key& key) const;
template<class K> bool contains(const K& key) const;
```

### <a name="parameters"></a>parameters

*`K`*\
键的类型。

*`key`*\
要查找的元素的键值。

### <a name="return-value"></a>返回值

`true` 如果在容器中找到元素，则为; 否则为。 `false` 否则为。

### <a name="remarks"></a>备注

`contains()` 是 c + + 20 中的新增项。 若要使用它，请指定 [/std： c + + 最新](../build/reference/std-specify-language-standard-version.md) 编译器选项。

`template<class K> bool contains(const K& key) const` 如果是透明的，则仅参与重载决策 `key_compare` 。 有关详细信息，请参阅 [关联容器中的异类查找](./stl-containers.md#heterogeneous-lookup-in-associative-containers-c14) 。

### <a name="example"></a>示例

```cpp
// Requires /std:c++latest
#include <set>
#include <iostream>

int main()
{
    std::multiset<int> theMultiSet = {1, 2};

    std::cout << std::boolalpha; // so booleans show as 'true' or 'false'
    std::cout << theMultiSet.contains(2) << '\n';
    std::cout << theMultiSet.contains(3) << '\n';

    return 0;
}
```

```Output
true
false
```

## <a name="multisetcount"></a><a name="count"></a> `multiset::count`

返回 `multiset` 中其键与指定为参数的键匹配的元素数量。

```cpp
size_type count(const Key& key) const;
```

### <a name="parameters"></a>parameters

*`key`*\
要从中进行匹配的元素的键 `multiset` 。

### <a name="return-value"></a>返回值

中 `multiset` 其排序键与参数键匹配的元素的数目。

### <a name="remarks"></a>备注

成员函数返回以下范围内的元素 *x* 的数量

\[ lower_bound (*`key`*) ，upper_bound (*`key`*) ) 

### <a name="example"></a>示例

下面的示例演示了 `multiset` ：： count 成员函数的用法。

```cpp
// multiset_count.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main()
{
    using namespace std;
    multiset<int> ms1;
    multiset<int>::size_type i;

    ms1.insert(1);
    ms1.insert(1);
    ms1.insert(2);

    // Elements don't need to be unique in multiset,
    // so duplicates are allowed and counted.
    i = ms1.count(1);
    cout << "The number of elements in ms1 with a sort key of 1 is: "
         << i << "." << endl;

    i = ms1.count(2);
    cout << "The number of elements in ms1 with a sort key of 2 is: "
         << i << "." << endl;

    i = ms1.count(3);
    cout << "The number of elements in ms1 with a sort key of 3 is: "
         << i << "." << endl;
}
```

```Output
The number of elements in ms1 with a sort key of 1 is: 2.
The number of elements in ms1 with a sort key of 2 is: 1.
The number of elements in ms1 with a sort key of 3 is: 0.
```

## <a name="multisetcrbegin"></a><a name="crbegin"></a> `multiset::crbegin`

返回一个常量迭代器，此迭代器用于发现反向多重集中的第一个元素。

```cpp
const_reverse_iterator crbegin() const;
```

### <a name="return-value"></a>返回值

一个发现反向多重集中的第一个元素或发现非反向多重集中的最后一个元素的的常量反向双向迭代器。

### <a name="remarks"></a>备注

`crbegin` 用于反向多重集，正如 begin 用于一样 `multiset` 。

如果返回值为 `crbegin` ，则 `multiset` 无法修改对象。

`crbegin` 可用于向后循环访问 `multiset`。

### <a name="example"></a>示例

```cpp
// multiset_crbegin.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main( )
{
   using namespace std;
   multiset <int> ms1;
   multiset <int>::const_reverse_iterator ms1_crIter;

   ms1.insert( 10 );
   ms1.insert( 20 );
   ms1.insert( 30 );

   ms1_crIter = ms1.crbegin( );
   cout << "The first element in the reversed multiset is "
        << *ms1_crIter << "." << endl;
}
```

```Output
The first element in the reversed multiset is 30.
```

## <a name="multisetcrend"></a><a name="crend"></a> `multiset::crend`

返回一个常量迭代器，此迭代器用于发现反向多重集中最后一个元素之后的位置。

```cpp
const_reverse_iterator crend() const;
```

### <a name="return-value"></a>返回值

用于发现反向多重集中最后一个元素之后的位置（非反向多重集中第一个元素之前的位置）的常量反向双向迭代器。

### <a name="remarks"></a>备注

`crend` 用于反向多重集，正如 [`end`](#end) 用于一样 `multiset` 。

如果返回值为 `crend` ，则 `multiset` 无法修改对象。

`crend` 可用于测试反向迭代器是否已到达其 `multiset` 的末尾。

返回的值 `crend` 不应被取消引用。

### <a name="example"></a>示例

```cpp
// multiset_crend.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main() {
   using namespace std;
   multiset <int> ms1;
   multiset <int>::const_reverse_iterator ms1_crIter;

   ms1.insert( 10 );
   ms1.insert( 20 );
   ms1.insert( 30 );

   ms1_crIter = ms1.crend( ) ;
   ms1_crIter--;
   cout << "The last element in the reversed multiset is "
        << *ms1_crIter << "." << endl;
}
```

## <a name="multisetdifference_type"></a><a name="difference_type"></a> `multiset::difference_type`

一种有符号整数类型，此类型可用于表示 `multiset` 中迭代器指向的元素间范围内的元素数量。

```cpp
typedef typename allocator_type::difference_type difference_type;
```

### <a name="remarks"></a>备注

`difference_type` 是通过容器迭代器减少或递增时返回的类型。 `difference_type` 通常用于表示迭代器 `first` 和 `last` 之间的范围 [ `first`, `last`) 内元素的数目，包括 `first` 指向的元素以及那一系列元素，但不包括 `last` 指向的元素。

尽管适用 `difference_type` 于满足输入迭代器要求的所有迭代器（包括可逆容器支持的双向迭代器的类，如集），但迭代器之间的减法仅受随机访问容器（如 vector）提供的随机访问迭代器支持。

### <a name="example"></a>示例

```cpp
// multiset_diff_type.cpp
// compile with: /EHsc
#include <iostream>
#include <set>
#include <algorithm>

int main( )
{
   using namespace std;

   multiset <int> ms1;
   multiset <int>::iterator ms1_Iter, ms1_bIter, ms1_eIter;

   ms1.insert( 20 );
   ms1.insert( 10 );
   ms1.insert( 20 );

   ms1_bIter = ms1.begin( );
   ms1_eIter = ms1.end( );

   multiset <int>::difference_type   df_typ5, df_typ10, df_typ20;

   df_typ5 = count( ms1_bIter, ms1_eIter, 5 );
   df_typ10 = count( ms1_bIter, ms1_eIter, 10 );
   df_typ20 = count( ms1_bIter, ms1_eIter, 20 );

   // The keys, and hence the elements, of a multiset aren't unique
   cout << "The number '5' occurs " << df_typ5
        << " times in multiset ms1.\n";
   cout << "The number '10' occurs " << df_typ10
        << " times in multiset ms1.\n";
   cout << "The number '20' occurs " << df_typ20
        << " times in multiset ms1.\n";

   // Count the number of elements in a multiset
   multiset <int>::difference_type  df_count = 0;
   ms1_Iter = ms1.begin( );
   while ( ms1_Iter != ms1_eIter)
   {
      df_count++;
      ms1_Iter++;
   }

   cout << "The number of elements in the multiset ms1 is: "
        << df_count << "." << endl;
}
```

```Output
The number '5' occurs 0 times in multiset ms1.
The number '10' occurs 1 times in multiset ms1.
The number '20' occurs 2 times in multiset ms1.
The number of elements in the multiset ms1 is: 3.
```

## <a name="multisetemplace"></a><a name="emplace"></a> `multiset::emplace`

使用位置提示就地插入构造的元素（不执行复制或移动操作）。

```cpp
template <class... Args>
iterator emplace(Args&&... args);
```

### <a name="parameters"></a>parameters

*`args`*\
用于构造要插入到中的元素的转发参数 `multiset` 。

### <a name="return-value"></a>返回值

指向新插入的元素的迭代器。

### <a name="remarks"></a>备注

对容器元素的引用不会因为此函数而失效，但是它可能会使所有指向容器的迭代器都失效。

在定位期间，如果引发异常，则不会修改容器的状态。

### <a name="example"></a>示例

```cpp
// multiset_emplace.cpp
// compile with: /EHsc
#include <set>
#include <string>
#include <iostream>

using namespace std;

template <typename S> void print(const S& s) {
    cout << s.size() << " elements: ";

    for (const auto& p : s) {
        cout << "(" << p << ") ";
    }

    cout << endl;
}

int main()
{
    multiset<string> s1;

    s1.emplace("Anna");
    s1.emplace("Bob");
    s1.emplace("Carmine");

    cout << "multiset modified, now contains ";
    print(s1);
    cout << endl;

    s1.emplace("Bob");

    cout << "multiset modified, now contains ";
    print(s1);
    cout << endl;
}
```

## <a name="multisetemplace_hint"></a><a name="emplace_hint"></a> `multiset::emplace_hint`

使用位置提示就地插入构造的元素（不执行复制或移动操作）。

```cpp
template <class... Args>
iterator emplace_hint(
    const_iterator where,
    Args&&... args);
```

### <a name="parameters"></a>parameters

*`args`*\
用于构造要插入到中的元素的转发参数 `multiset` 。

*`where`*\
开始搜索正确插入点的位置。  (如果该点紧接 *`where`* 在之前，则插入可能发生在分期常量时间内，而非对数时间内。 ) 

### <a name="return-value"></a>返回值

指向新插入的元素的迭代器。

### <a name="remarks"></a>备注

对容器元素的引用不会因为此函数而失效，但是它可能会使所有指向容器的迭代器都失效。

在定位期间，如果引发异常，则不会修改容器的状态。

有关代码示例，请参见 [`set::emplace_hint`](../standard-library/set-class.md#emplace_hint) 。

## <a name="multisetempty"></a><a name="empty"></a> `multiset::empty`

测试 `multiset` 是否为空。

```cpp
bool empty() const;
```

### <a name="return-value"></a>返回值

**`true`** 如果 `multiset` 为空，则为; **`false`** 如果不为空，则为 `multiset` 。

### <a name="example"></a>示例

```cpp
// multiset_empty.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main()
{
   using namespace std;
   multiset <int> ms1, ms2;
   ms1.insert ( 1 );

   if ( ms1.empty( ) )
      cout << "The multiset ms1 is empty." << endl;
   else
      cout << "The multiset ms1 is not empty." << endl;

   if ( ms2.empty( ) )
      cout << "The multiset ms2 is empty." << endl;
   else
      cout << "The multiset ms2 is not empty." << endl;
}
```

```Output
The multiset ms1 is not empty.
The multiset ms2 is empty.
```

## <a name="multisetend"></a><a name="end"></a> `multiset::end`

返回超过末尾迭代器。

```cpp
const_iterator end() const;

iterator end();
```

### <a name="return-value"></a>返回值

超过末尾迭代器。 如果 `multiset` 为空，则 `multiset::end() == multiset::begin()`。

### <a name="remarks"></a>备注

**`end`** 用于测试迭代器是否已超过其多重集的末尾。

返回的值 **`end`** 不应被取消引用。

有关代码示例，请参见 [`multiset::find`](#find) 。

## <a name="multisetequal_range"></a><a name="equal_range"></a> `multiset::equal_range`

返回一对迭代器，这两个迭代器分别用于发现 `multiset` 中其键大于指定键的第一个元素，以及 `multiset` 中其键等于或大于指定键的第一个元素。

```cpp
pair <const_iterator, const_iterator> equal_range (const Key& key) const;

pair <iterator, iterator> equal_range (const Key& key);
```

### <a name="parameters"></a>parameters

*`key`*\
要与要搜索的元素的排序键进行比较的参数键 `multiset` 。

### <a name="return-value"></a>返回值

一对迭代器，以便第一个迭代器是键的， [`lower_bound`](#lower_bound) 第二个迭代器是 [`upper_bound`](#upper_bound) 键的。

若要访问成员函数返回的 `pr` 对的第一个迭代器，请使用 `pr`. **`first`**，若要取消引用下限迭代器，请使用 \* ( `pr` 。 **`first`**). 若要访问成员函数返回的 `pr` 对的第二个迭代器，请使用 `pr`. **`second`**，若要取消引用上限迭代器，请使用 \* ( `pr` 。 **`second`**).

### <a name="example"></a>示例

```cpp
// multiset_equal_range.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main( )
{
   using namespace std;
   typedef multiset<int, less<int> > IntSet;
   IntSet ms1;
   multiset <int> :: const_iterator ms1_RcIter;

   ms1.insert( 10 );
   ms1.insert( 20 );
   ms1.insert( 30 );

   pair <IntSet::const_iterator, IntSet::const_iterator> p1, p2;
   p1 = ms1.equal_range( 20 );

   cout << "The upper bound of the element with "
        << "a key of 20 in the multiset ms1 is: "
        << *( p1.second ) << "." << endl;

   cout << "The lower bound of the element with "
        << "a key of 20 in the multiset ms1 is: "
        << *( p1.first ) << "." << endl;

   // Compare the upper_bound called directly
   ms1_RcIter = ms1.upper_bound( 20 );
   cout << "A direct call of upper_bound( 20 ) gives "
        << *ms1_RcIter << "," << endl
        << "matching the 2nd element of the pair"
        << " returned by equal_range( 20 )." << endl;

   p2 = ms1.equal_range( 40 );

   // If no match is found for the key,
   // both elements of the pair return end( )
   if ( ( p2.first == ms1.end( ) ) && ( p2.second == ms1.end( ) ) )
      cout << "The multiset ms1 doesn't have an element "
              << "with a key less than 40." << endl;
   else
      cout << "The element of multiset ms1 with a key >= 40 is: "
                << *( p1.first ) << "." << endl;
}
```

```Output
The upper bound of the element with a key of 20 in the multiset ms1 is: 30.
The lower bound of the element with a key of 20 in the multiset ms1 is: 20.
A direct call of upper_bound( 20 ) gives 30,
matching the 2nd element of the pair returned by equal_range( 20 ).
The multiset ms1 doesn't have an element with a key less than 40.
```

## <a name="multiseterase"></a><a name="erase"></a> `multiset::erase`

从 `multiset` 中的指定位置移除一个元素或元素范围，或者移除与指定键匹配的元素。

```cpp
iterator erase(
    const_iterator Where);

iterator erase(
    const_iterator First,
    const_iterator Last);

size_type erase(
    const key_type& Key);
```

### <a name="parameters"></a>parameters

*`Where`*\
要移除的元素的位置。

*`First`*\
要移除的第一个元素的位置。

*`Last`*\
要移除的刚超出最后一个元素的位置。

*`key`*\
要移除的元素的关键值。

### <a name="return-value"></a>返回值

对于前两个成员函数，是一种双向迭代器，它指定已移除的任何元素之外保留的第一个元素; `multiset` 如果此类元素不存在，则为的结束元素。

对于第三个成员函数，返回已从中移除的元素的数目 `multiset` 。

### <a name="remarks"></a>备注

有关代码示例，请参见 [`set::erase`](../standard-library/set-class.md#erase) 。

## <a name="multisetfind"></a><a name="find"></a> `multiset::find`

返回一个迭代器，该迭代器引用中其 `multiset` 键与指定键等效的元素的位置。

```cpp
iterator find(const Key& key);

const_iterator find(const Key& key) const;
```

### <a name="parameters"></a>parameters

*`key`*\
要由要搜索的元素的排序键匹配的键值 `multiset` 。

### <a name="return-value"></a>返回值

一个迭代器，它引用具有指定键的元素的位置，或在)  ( 中最后一个元素之后的位置（ `multiset` `multiset::end()` 如果找不到该键的匹配项）。

### <a name="remarks"></a>备注

此成员函数返回一个迭代器，该迭代器引用中 `multiset` 其键与二元谓词下的参数等效的元素 *`key`* ，该谓词基于小于比较关系进行排序。

如果将的返回值 `find` 分配给 `const_iterator` ，则 `multiset` 无法修改对象。 如果将的返回值 `find` 分配给 `iterator` ，则 `multiset` 可以修改该对象。

### <a name="example"></a>示例

```cpp
// compile with: /EHsc /W4 /MTd
#include <set>
#include <iostream>
#include <vector>
#include <string>

using namespace std;

template <typename T> void print_elem(const T& t) {
    cout << "(" << t << ") ";
}

template <typename T> void print_collection(const T& t) {
    cout << t.size() << " elements: ";

    for (const auto& p : t) {
        print_elem(p);
    }
    cout << endl;
}

template <typename C, class T> void findit(const C& c, T val) {
    cout << "Trying find() on value " << val << endl;
    auto result = c.find(val);
    if (result != c.end()) {
        cout << "Element found: "; print_elem(*result); cout << endl;
    } else {
        cout << "Element not found." << endl;
    }
}

int main()
{
    multiset<int> s1({ 40, 45 });
    cout << "The starting multiset s1 is: " << endl;
    print_collection(s1);

    vector<int> v;
    v.push_back(43);
    v.push_back(41);
    v.push_back(46);
    v.push_back(42);
    v.push_back(44);
    v.push_back(44); // attempt a duplicate

    cout << "Inserting the following vector data into s1: " << endl;
    print_collection(v);

    s1.insert(v.begin(), v.end());

    cout << "The modified multiset s1 is: " << endl;
    print_collection(s1);
    cout << endl;
    findit(s1, 45);
    findit(s1, 6);
}
```

## <a name="multisetget_allocator"></a><a name="get_allocator"></a> `multiset::get_allocator`

返回用于构造的分配器对象的副本 `multiset` 。

```cpp
allocator_type get_allocator() const;
```

### <a name="return-value"></a>返回值

使用的分配器 `multiset` 。

### <a name="remarks"></a>备注

类的分配器 `multiset` 指定类管理存储的方式。 C++ 标准库容器类提供的默认分配器足以满足大多编程需求。 编写和使用你自己的分配器类是高级 C++ 主题。

### <a name="example"></a>示例

```cpp
// multiset_get_allocator.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main( )
{
   using namespace std;
   multiset <int>::allocator_type ms1_Alloc;
   multiset <int>::allocator_type ms2_Alloc;
   multiset <double>::allocator_type ms3_Alloc;
   multiset <int>::allocator_type ms4_Alloc;

   // The following lines declare objects
   // that use the default allocator.
   multiset <int> ms1;
   multiset <int, allocator<int> > ms2;
   multiset <double, allocator<double> > ms3;

   cout << "The number of integers that can be allocated"
        << endl << "before free memory is exhausted: "
        << ms2.max_size( ) << "." << endl;

   cout << "The number of doubles that can be allocated"
        << endl << "before free memory is exhausted: "
        << ms3.max_size( ) <<  "." << endl;

   // The following lines create a multiset ms4
   // with the allocator of multiset ms1
   ms1_Alloc = ms1.get_allocator( );
   multiset <int> ms4( less<int>( ), ms1_Alloc );
   ms4_Alloc = ms4.get_allocator( );

   // Two allocators are interchangeable if
   // storage allocated from each can be
   // deallocated with the other
   if( ms1_Alloc == ms4_Alloc )
   {
      cout << "Allocators are interchangeable."
           << endl;
   }
   else
   {
      cout << "Allocators are not interchangeable."
           << endl;
   }
}
```

## <a name="multisetinsert"></a><a name="insert"></a> `multiset::insert`

将一个元素或元素范围插入到 `multiset`。

```cpp
// (1) single element
pair<iterator, bool> insert(
    const value_type& Val);

// (2) single element, perfect forwarded
template <class ValTy>
pair<iterator, bool>
insert(
    ValTy&& Val);

// (3) single element with hint
iterator insert(
    const_iterator Where,
    const value_type& Val);

// (4) single element, perfect forwarded, with hint
template <class ValTy>
iterator insert(
    const_iterator Where,
    ValTy&& Val);

// (5) range
template <class InputIterator>
void insert(
    InputIterator First,
    InputIterator Last);

// (6) initializer list
void insert(
    initializer_list<value_type>
IList);
```

### <a name="parameters"></a>parameters

*`Val`*\
要插入到中的元素的值 `multiset` 。

*`Where`*\
开始搜索正确插入点的位置。  (如果该点紧接 *`Where`* 在之前，则插入可能发生在分期常量时间内，而非对数时间内。 ) 

*`ValTy`*\
一个模板参数，该参数指定 `multiset` 可用于构造的元素的参数类型 [`value_type`](../standard-library/map-class.md#value_type) ，并将 *`Val`* 作为参数完美转发。

*`First`*\
要复制的第一个元素的位置。

*`Last`*\
要复制的最后一个元素以外的位置。

*1*\
满足的要求的模板函数自变量 [`input_iterator_tag`](../standard-library/input-iterator-tag-struct.md) ，该类型指向可用于构造 [1](../standard-library/map-class.md#value_type) 个对象的类型的元素。

*`IList`*\
[`initializer_list`](../standard-library/initializer-list.md)要从中复制元素的。

### <a name="return-value"></a>返回值

单个元素插入成员函数 (1) 和 (2) ，返回一个迭代器，该迭代器指向将新元素插入到的位置 `multiset` 。

带有提示的单个元素的成员函数 (3) 和 (4) 返回一个迭代器，该迭代器指向将新元素插入到的位置 `multiset` 。

### <a name="remarks"></a>备注

指针或引用不会因为此函数而失效，但是它可能会使所有指向容器的迭代器都失效。

在只插入一个元素的过程中，如果引发异常，则不会修改容器的状态。 在插入多个元素的过程中，如果引发异常，则会使容器处于未指定但有效的状态。

[`value_type`](../standard-library/map-class.md#value_type)容器的是属于该容器的 typedef; 对于集， `multiset<V>::value_type` 是类型 `const V` 。

范围成员函数 (5) 将元素值序列插入到 `multiset` 中，它对应于迭代器在范围中所处理的每个元素 `[First, Last)` ; 因此，不 `Last` 会插入。 容器成员函数 `end()` 是指容器中最后一个元素之后的位置，例如，`s.insert(v.begin(), v.end());` 语句会将 `v` 的所有元素插入到 `s` 中。

初始值设定项列表成员函数 (6) 使用将 [`initializer_list`](../standard-library/initializer-list.md) 元素复制到中 `multiset` 。

若要插入就地构造的元素（即不执行复制或移动操作），请参阅 [`multiset::emplace`](#emplace) 和 [`multiset::emplace_hint`](#emplace_hint) 。

### <a name="example"></a>示例

```cpp
// multiset_insert.cpp
// compile with: /EHsc
#include <set>
#include <iostream>
#include <string>
#include <vector>

using namespace std;

template <typename S> void print(const S& s) {
    cout << s.size() << " elements: ";

    for (const auto& p : s) {
        cout << "(" << p << ") ";
    }

    cout << endl;
}

int main()
{

    // insert single values
    multiset<int> s1;
    // call insert(const value_type&) version
    s1.insert({ 1, 10 });
    // call insert(ValTy&&) version
    s1.insert(20);

    cout << "The original multiset values of s1 are:" << endl;
    print(s1);

    // intentionally attempt a duplicate, single element
    s1.insert(1);
    cout << "The modified multiset values of s1 are:" << endl;
    print(s1);
    cout << endl;

    // single element, with hint
    s1.insert(s1.end(), 30);
    cout << "The modified multiset values of s1 are:" << endl;
    print(s1);
    cout << endl;

    // The templatized version inserting a jumbled range
    multiset<int> s2;
    vector<int> v;
    v.push_back(43);
    v.push_back(294);
    v.push_back(41);
    v.push_back(330);
    v.push_back(42);
    v.push_back(45);

    cout << "Inserting the following vector data into s2:" << endl;
    print(v);

    s2.insert(v.begin(), v.end());

    cout << "The modified multiset values of s2 are:" << endl;
    print(s2);
    cout << endl;

    // The templatized versions move-constructing elements
    multiset<string>  s3;
    string str1("blue"), str2("green");

    // single element
    s3.insert(move(str1));
    cout << "After the first move insertion, s3 contains:" << endl;
    print(s3);

    // single element with hint
    s3.insert(s3.end(), move(str2));
    cout << "After the second move insertion, s3 contains:" << endl;
    print(s3);
    cout << endl;

    multiset<int> s4;
    // Insert the elements from an initializer_list
    s4.insert({ 4, 44, 2, 22, 3, 33, 1, 11, 5, 55 });
    cout << "After initializer_list insertion, s4 contains:" << endl;
    print(s4);
    cout << endl;
}
```

## <a name="multisetiterator"></a><a name="iterator"></a> `multiset::iterator`

一种类型，它提供可读取中的任何元素的常量 [双向迭代器](../standard-library/bidirectional-iterator-tag-struct.md) `multiset` 。

```cpp
typedef implementation-defined iterator;
```

### <a name="example"></a>示例

有关如何声明和使用的示例，请参阅 [begin](#begin) 的示例 `iterator` 。

## <a name="multisetkey_comp"></a><a name="key_comp"></a> `multiset::key_comp`

检索用于对 `multiset` 中的键进行排序的比较对象副本。

```cpp
key_compare key_comp() const;
```

### <a name="return-value"></a>返回值

返回用于对 `multiset` 其元素进行排序的函数对象，即模板参数 `Compare` 。

有关 `Compare` 的详细信息，请参阅 [multiset 类](../standard-library/multiset-class.md)主题的备注部分。

### <a name="remarks"></a>备注

存储对象用于定义以下成员函数：

**bool operator**( **const Key&** *x*, **const Key&** *y*);

如果在排序顺序中 *x* 严格位于 *y* 之前，则返回 true。

[`key_compare`](#key_compare)和 [`value_compare`](#value_compare) 都是模板参数的同义词 ``Compare`` 。 这两种类型都是为类集和多重集提供的，它们是相同的，与类 map 和多重映射的兼容性不同。

### <a name="example"></a>示例

```cpp
// multiset_key_comp.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main( )
{
   using namespace std;

   multiset <int, less<int> > ms1;
   multiset <int, less<int> >::key_compare kc1 = ms1.key_comp( ) ;
   bool result1 = kc1( 2, 3 ) ;
   if( result1 == true )
   {
      cout << "kc1( 2,3 ) returns value of true, "
           << "where kc1 is the function object of s1."
           << endl;
   }
   else
   {
      cout << "kc1( 2,3 ) returns value of false "
           << "where kc1 is the function object of ms1."
           << endl;
   }

   multiset <int, greater<int> > ms2;
   multiset <int, greater<int> >::key_compare kc2 = ms2.key_comp( ) ;
   bool result2 = kc2( 2, 3 ) ;
   if( result2 == true )
   {
      cout << "kc2( 2,3 ) returns value of true, "
           << "where kc2 is the function object of ms2."
           << endl;
   }
   else
   {
      cout << "kc2( 2,3 ) returns value of false, "
           << "where kc2 is the function object of ms2."
           << endl;
   }
}
```

```Output
kc1( 2,3 ) returns value of true, where kc1 is the function object of s1.
kc2( 2,3 ) returns value of false, where kc2 is the function object of ms2.
```

## <a name="multisetkey_compare"></a><a name="key_compare"></a> `multiset::key_compare`

一种提供函数对象的类型，该函数对象可比较两个排序键以确定 `multiset` 中两个元素的相对顺序。

```cpp
typedef Compare key_compare;
```

### <a name="remarks"></a>备注

`key_compare` 是模板参数 `Compare` 的同义词。

有关的详细信息 `Compare` ，请参阅[ `multiset` 类](../standard-library/multiset-class.md)主题的 "备注" 部分。

### <a name="example"></a>示例

[`key_comp`](#key_comp)有关如何声明和使用的示例，请参见的示例 `key_compare` 。

## <a name="multisetkey_type"></a><a name="key_type"></a> `multiset::key_type`

一种提供函数对象的类型，该函数对象可比较排序键以确定中两个元素的相对顺序 `multiset` 。

```cpp
typedef Key key_type;
```

### <a name="remarks"></a>备注

`key_type` 是模板参数 `Key` 的同义词。

有关的详细信息 `Key` ，请参阅[ `multiset` 类](../standard-library/multiset-class.md)主题的 "备注" 部分。

### <a name="example"></a>示例

[`value_type`](#value_type)有关如何声明和使用的示例，请参见的示例 ``key_type`` 。

## <a name="multisetlower_bound"></a><a name="lower_bound"></a> `multiset::lower_bound`

返回一个迭代器，此迭代器指向 `multiset` 中其键等于或大于指定键的第一个元素。

```cpp
const_iterator lower_bound(const Key& key) const;

iterator lower_bound(const Key& key);
```

### <a name="parameters"></a>parameters

*`key`*\
要与要搜索的元素的排序键进行比较的参数键 `multiset` 。

### <a name="return-value"></a>返回值

一个 `iterator` 或 `const_iterator` ，它用于发现中其 `multiset` 键等于或大于参数键的元素的位置，或 `multiset` 如果未找到键的匹配项，则寻址中最后一个元素之后的位置。

### <a name="example"></a>示例

```cpp
// multiset_lower_bound.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main( )
{
   using namespace std;
   multiset <int> ms1;
   multiset <int> :: const_iterator ms1_AcIter, ms1_RcIter;

   ms1.insert( 10 );
   ms1.insert( 20 );
   ms1.insert( 30 );

   ms1_RcIter = ms1.lower_bound( 20 );
   cout << "The element of multiset ms1 with a key of 20 is: "
        << *ms1_RcIter << "." << endl;

   ms1_RcIter = ms1.lower_bound( 40 );

   // If no match is found for the key, end( ) is returned
   if ( ms1_RcIter == ms1.end( ) )
      cout << "The multiset ms1 doesn't have an element "
           << "with a key of 40." << endl;
   else
      cout << "The element of multiset ms1 with a key of 40 is: "
           << *ms1_RcIter << "." << endl;

   // The element at a specific location in the multiset can be
   // found using a dereferenced iterator addressing the location
   ms1_AcIter = ms1.end( );
   ms1_AcIter--;
   ms1_RcIter = ms1.lower_bound( *ms1_AcIter );
   cout << "The element of ms1 with a key matching "
        << "that of the last element is: "
        << *ms1_RcIter << "." << endl;
}
```

```Output
The element of multiset ms1 with a key of 20 is: 20.
The multiset ms1 doesn't have an element with a key of 40.
The element of ms1 with a key matching that of the last element is: 30.
```

## <a name="multisetmax_size"></a><a name="max_size"></a> `multiset::max_size`

返回 `multiset` 的最大长度。

```cpp
size_type max_size() const;
```

### <a name="return-value"></a>返回值

的最大可能长度 `multiset` 。

### <a name="example"></a>示例

```cpp
// multiset_max_size.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main( )
{
   using namespace std;
   multiset <int> ms1;
   multiset <int>::size_type i;

   i = ms1.max_size( );
   cout << "The maximum possible length "
        << "of the multiset is " << i << "." << endl;
}
```

## <a name="multisetmultiset"></a><a name="multiset"></a> `multiset::multiset`

构造一个空的或者是其他某个 `multiset` 的全部或部分副本的 `multiset`。

```cpp
multiset();

explicit multiset (
    const Compare& Comp);

multiset (
    const Compare& Comp,
    const Allocator& Al);

multiset(
    const multiset& Right);

multiset(
    multiset&& Right);

multiset(
    initializer_list<Type> IList);

multiset(
    initializer_list<Type> IList,
    const Compare& Comp);

multiset(
    initializer_list<Type> IList,
    const Compare& Comp,
    const Allocator& Al);

template <class InputIterator>
multiset (
    InputIterator First,
    InputIterator Last);

template <class InputIterator>
multiset (
    InputIterator First,
    InputIterator Last,
    const Compare& Comp);

template <class InputIterator>
multiset (
    InputIterator First,
    InputIterator Last,
    const Compare& Comp,
    const Allocator& Al);
```

### <a name="parameters"></a>parameters

*`Al`*\
要用于此多重集对象的存储分配器类，默认为 `Allocator`。

*`Comp`*\
用于对 `multiset` 中元素排序的类型 `const Compare` 的比较函数，默认为 `Compare`。

*`Right`*\
`multiset`构造的多重集要作为其副本的。

*`First`*\
要复制的范围元素中的第一个元素的位置。

*`Last`*\
要复制的元素范围以外的第一个元素的位置。

*`IList`*\
`initializer_list`要从中复制元素的。

### <a name="remarks"></a>备注

所有构造函数都存储一种类型的分配器对象，该对象管理的内存存储， `multiset` 且稍后可通过调用返回 [`get_allocator`](#get_allocator) 。 此分配器参数在类声明中常省略，并预处理用于代替备用分配器的宏。

所有构造函数对它们的多重集进行初始化。

所有构造函数都存储一个类型为 Compare 的函数对象，该对象用于在的键之间建立顺序， `multiset` 且稍后可通过调用返回 [`key_comp`](#key_comp) 。

前三个构造函数指定一个空的初始多重集，第二个指定比较函数的类型 (*`Comp`*) 将用于建立元素的顺序，第三个构造函数显式指定要使用)  (的分配器类型 *`Al`* 。 关键字 **`explicit`** 取消了某些类型的自动类型转换。

第四个构造函数指定的副本 `multiset` *`Right`* 。

第五个构造函数 `multiset` 通过移动指定的副本 *`Right`* 。

第六个、第七个和第八个构造函数指定要从中复制元素的 initializer_list。

接下来的三个构造函数 `[First, Last)` `multiset` 在指定比较函数的类型和分配器时复制的范围，并增加明确。

### <a name="example"></a>示例

```cpp
// multiset_ctor.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main()
{
    using namespace std;
    //multiset <int>::iterator ms1_Iter, ms2_Iter, ms3_Iter;
    multiset <int>::iterator ms4_Iter, ms5_Iter, ms6_Iter, ms7_Iter;

    // Create an empty multiset ms0 of key type integer
    multiset <int> ms0;

    // Create an empty multiset ms1 with the key comparison
    // function of less than, then insert 4 elements
    multiset <int, less<int> > ms1;
    ms1.insert(10);
    ms1.insert(20);
    ms1.insert(20);
    ms1.insert(40);

    // Create an empty multiset ms2 with the key comparison
    // function of greater than, then insert 2 elements
    multiset <int, less<int> > ms2;
    ms2.insert(10);
    ms2.insert(20);

    // Create a multiset ms3 with the
    // allocator of multiset ms1
    multiset <int>::allocator_type ms1_Alloc;
    ms1_Alloc = ms1.get_allocator();
    multiset <int> ms3(less<int>(), ms1_Alloc);
    ms3.insert(30);

    // Create a copy, multiset ms4, of multiset ms1
    multiset <int> ms4(ms1);

    // Create a multiset ms5 by copying the range ms1[ first,  last)
    multiset <int>::const_iterator ms1_bcIter, ms1_ecIter;
    ms1_bcIter = ms1.begin();
    ms1_ecIter = ms1.begin();
    ms1_ecIter++;
    ms1_ecIter++;
    multiset <int> ms5(ms1_bcIter, ms1_ecIter);

    // Create a multiset ms6 by copying the range ms4[ first,  last)
    // and with the allocator of multiset ms2
    multiset <int>::allocator_type ms2_Alloc;
    ms2_Alloc = ms2.get_allocator();
    multiset <int> ms6(ms4.begin(), ++ms4.begin(), less<int>(), ms2_Alloc);

    cout << "ms1 =";
    for (auto i : ms1)
        cout << " " << i;
    cout << endl;

    cout << "ms2 =";
    for (auto i : ms2)
        cout << " " << i;
   cout << endl;

   cout << "ms3 =";
   for (auto i : ms3)
       cout << " " << i;
    cout << endl;

    cout << "ms4 =";
    for (auto i : ms4)
        cout << " " << i;
    cout << endl;

    cout << "ms5 =";
    for (auto i : ms5)
        cout << " " << i;
    cout << endl;

    cout << "ms6 =";
    for (auto i : ms6)
        cout << " " << i;
    cout << endl;

    // Create a multiset by moving ms5
    multiset<int> ms7(move(ms5));
    cout << "ms7 =";
    for (auto i : ms7)
        cout << " " << i;
    cout << endl;

    // Create a multiset with an initializer_list
    multiset<int> ms8({1, 2, 3, 4});
    cout << "ms8=";
    for (auto i : ms8)
        cout << " " << i;
    cout << endl;
}
```

## <a name="multisetoperator"></a><a name="op_eq"></a> `multiset::operator=`

使用另一个 `multiset` 的元素替换该 `multiset` 的元素。

```cpp
multiset& operator=(const multiset& right);

multiset& operator=(multiset&& right);
```

### <a name="parameters"></a>parameters

*`Right`*\
从中复制或移动元素的 `multiset`。

### <a name="remarks"></a>备注

`operator=` 根据 *`Right`* `multiset` 所用 (lvalue 或 rvalue) 的引用类型，将中的元素复制或移动到此中。 放弃 `operator=` executes 之前此 `multiset` 中的元素。

### <a name="example"></a>示例

```cpp
// multiset_operator_as.cpp
// compile with: /EHsc
#include <multiset>
#include <iostream>

int main( )
   {
   using namespace std;
   multiset<int> v1, v2, v3;
   multiset<int>::iterator iter;

   v1.insert(10);

   cout << "v1 = " ;
   for (iter = v1.begin(); iter != v1.end(); iter++)
      cout << *iter << " ";
   cout << endl;

   v2 = v1;
   cout << "v2 = ";
   for (iter = v2.begin(); iter != v2.end(); iter++)
      cout << *iter << " ";
   cout << endl;

// move v1 into v2
   v2.clear();
   v2 = move(v1);
   cout << "v2 = ";
   for (iter = v2.begin(); iter != v2.end(); iter++)
      cout << *iter << " ";
   cout << endl;
   }
```

## <a name="multisetpointer"></a><a name="pointer"></a> `multiset::pointer`

一种类型，它提供指向 `multiset` 中的某个元素的指针。

```cpp
typedef typename allocator_type::pointer pointer;
```

### <a name="remarks"></a>备注

类型 **`pointer`** 可用于修改元素的值。

在大多数情况下，应使用 [迭代器](#iterator) 访问对象中的元素 `multiset` 。

## <a name="multisetrbegin"></a><a name="rbegin"></a> `multiset::rbegin`

返回一个迭代器，此迭代器用于发现反向多重集中的第一个元素。

```cpp
const_reverse_iterator rbegin() const;

reverse_iterator rbegin();
```

### <a name="return-value"></a>返回值

一个反向双向迭代器，用于寻址反向 `multiset` 或处理非反向中最后一个元素之后的第一个元素 `multiset` 。

### <a name="remarks"></a>备注

`rbegin` 用于反向，正 `multiset` 如 rbegin 与一起使用一样 `multiset` 。

如果将的返回值 `rbegin` 分配给 `const_reverse_iterator` ，则 `multiset` 无法修改对象。 如果将的返回值 `rbegin` 分配给 `reverse_iterator` ，则 `multiset` 可以修改该对象。

`rbegin` 可用于向后循环访问 `multiset`。

### <a name="example"></a>示例

```cpp
// multiset_rbegin.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main( )
{
   using namespace std;
   multiset <int> ms1;
   multiset <int>::iterator ms1_Iter;
   multiset <int>::reverse_iterator ms1_rIter;

   ms1.insert( 10 );
   ms1.insert( 20 );
   ms1.insert( 30 );

   ms1_rIter = ms1.rbegin( );
   cout << "The first element in the reversed multiset is "
        << *ms1_rIter << "." << endl;

   // begin can be used to start an iteration
   // through a multiset in a forward order
   cout << "The multiset is:";
   for ( ms1_Iter = ms1.begin( ) ; ms1_Iter != ms1.end( ); ms1_Iter++ )
      cout << " " << *ms1_Iter;
   cout << endl;

   // rbegin can be used to start an iteration
   // through a multiset in a reverse order
   cout << "The reversed multiset is:";
   for ( ms1_rIter = ms1.rbegin( ) ; ms1_rIter != ms1.rend( ); ms1_rIter++ )
      cout << " " << *ms1_rIter;
   cout << endl;

   // a multiset element can be erased by dereferencing to its key
   ms1_rIter = ms1.rbegin( );
   ms1.erase ( *ms1_rIter );

   ms1_rIter = ms1.rbegin( );
   cout << "After the erasure, the first element "
        << "in the reversed multiset is "<< *ms1_rIter << "."
        << endl;
}
```

```Output
The first element in the reversed multiset is 30.
The multiset is: 10 20 30
The reversed multiset is: 30 20 10
After the erasure, the first element in the reversed multiset is 20.
```

## <a name="multisetreference"></a><a name="reference"></a> `multiset::reference`

一种类型，此类型提供对存储在 `multiset` 中的元素的引用。

```cpp
typedef typename allocator_type::reference reference;
```

### <a name="example"></a>示例

```cpp
// multiset_ref.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main( )
{
   using namespace std;
   multiset <int> ms1;

   ms1.insert( 10 );
   ms1.insert( 20 );

   // Declare and initialize a reference &Ref1 to the 1st element
   const int &Ref1 = *ms1.begin( );

   cout << "The first element in the multiset is "
        << Ref1 << "." << endl;
}
```

```Output
The first element in the multiset is 10.
```

## <a name="multisetrend"></a><a name="rend"></a> `multiset::rend`

返回一个迭代器，此迭代器用于发现反向 `multiset` 中最后一个元素之后的位置。

```cpp
const_reverse_iterator rend() const;

reverse_iterator rend();
```

### <a name="return-value"></a>返回值

一个反向双向迭代器，用于发现反向 (中最后一个元素之后的位置 `multiset` （非反向) 中第一个元素之前的位置） `multiset` 。

### <a name="remarks"></a>备注

`rend` 用于反向多重集，正如 [`end`](#end) 用于一样 `multiset` 。

如果将的返回值 `rend` 分配给 `const_reverse_iterator` ，则 `multiset` 无法修改对象。 如果将的返回值 `rend` 分配给 `reverse_iterator` ，则 `multiset` 可以修改该对象。

`rend` 可用于测试反向迭代器是否已到达其多重集末尾。

返回的值 `rend` 不应被取消引用。

### <a name="example"></a>示例

```cpp
// multiset_rend.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main() {
   using namespace std;
   multiset <int> ms1;
   multiset <int>::iterator ms1_Iter;
   multiset <int>::reverse_iterator ms1_rIter;
   multiset <int>::const_reverse_iterator ms1_crIter;

   ms1.insert( 10 );
   ms1.insert( 20 );
   ms1.insert( 30 );

   ms1_rIter = ms1.rend( ) ;
   ms1_rIter--;
   cout << "The last element in the reversed multiset is "
        << *ms1_rIter << "." << endl;

   // end can be used to terminate an iteration
   // through a multiset in a forward order
   cout << "The multiset is: ";
   for ( ms1_Iter = ms1.begin( ) ; ms1_Iter != ms1.end( ); ms1_Iter++ )
      cout << *ms1_Iter << " ";
   cout << "." << endl;

   // rend can be used to terminate an iteration
   // through a multiset in a reverse order
   cout << "The reversed multiset is: ";
   for ( ms1_rIter = ms1.rbegin( ) ; ms1_rIter != ms1.rend( ); ms1_rIter++ )
      cout << *ms1_rIter << " ";
   cout << "." << endl;

   ms1_rIter = ms1.rend( );
   ms1_rIter--;
   ms1.erase ( *ms1_rIter );

   ms1_rIter = ms1.rend( );
   --ms1_rIter;
   cout << "After the erasure, the last element in the "
        << "reversed multiset is " << *ms1_rIter << "." << endl;
}
```

## <a name="multisetreverse_iterator"></a><a name="reverse_iterator"></a> `multiset::reverse_iterator`

一种类型，此类型提供可读取或修改反向多重集中的元素的双向迭代器。

```cpp
typedef std::reverse_iterator<iterator> reverse_iterator;
```

### <a name="remarks"></a>备注

类型 `reverse_iterator` 用于反向循环访问 `multiset` 。

### <a name="example"></a>示例

[`rbegin`](#rbegin)有关如何声明和使用的示例，请参阅示例 `reverse_iterator` 。

## <a name="multisetsize"></a><a name="size"></a> `multiset::size`

返回 `multiset` 中的元素数量。

```cpp
size_type size() const;
```

### <a name="return-value"></a>返回值

的当前长度 `multiset` 。

### <a name="example"></a>示例

```cpp
// multiset_size.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main( )
{
   using namespace std;
   multiset <int> ms1;
   multiset <int> :: size_type i;

   ms1.insert( 1 );
   i = ms1.size( );
   cout << "The multiset length is " << i << "." << endl;

   ms1.insert( 2 );
   i = ms1.size( );
   cout << "The multiset length is now " << i << "." << endl;
}
```

```Output
The multiset length is 1.
The multiset length is now 2.
```

## <a name="multisetsize_type"></a><a name="size_type"></a> `multiset::size_type`

可表示 `multiset` 中元素数量的无符号整数类型。

```cpp
typedef typename allocator_type::size_type size_type;
```

### <a name="example"></a>示例

[`size`](#size)有关如何声明和使用的示例，请参阅示例。`size_type`

## <a name="multisetswap"></a><a name="swap"></a> `multiset::swap`

交换两个多重集的元素。

```cpp
void swap(
    multiset<Key, Compare, Allocator>& right);
```

### <a name="parameters"></a>parameters

*`Right`*\
参数多重集提供与目标多重集进行交换的元素。

### <a name="remarks"></a>备注

成员函数不会使在彼此交换元素的两个多重集中指定元素的任何引用、指针或迭代器无效。

### <a name="example"></a>示例

```cpp
// multiset_swap.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main( )
{
   using namespace std;
   multiset <int> ms1, ms2, ms3;
   multiset <int>::iterator ms1_Iter;

   ms1.insert( 10 );
   ms1.insert( 20 );
   ms1.insert( 30 );
   ms2.insert( 100 );
   ms2.insert( 200 );
   ms3.insert( 300 );

   cout << "The original multiset ms1 is:";
   for ( ms1_Iter = ms1.begin( ); ms1_Iter != ms1.end( ); ms1_Iter++ )
      cout << " " << *ms1_Iter;
   cout << "." << endl;

   // This is the member function version of swap
   ms1.swap( ms2 );

   cout << "After swapping with ms2, list ms1 is:";
   for ( ms1_Iter = ms1.begin( ); ms1_Iter != ms1.end( ); ms1_Iter++ )
      cout << " " << *ms1_Iter;
   cout << "." << endl;

   // This is the specialized template version of swap
   swap( ms1, ms3 );

   cout << "After swapping with ms3, list ms1 is:";
   for ( ms1_Iter = ms1.begin( ); ms1_Iter != ms1.end( ); ms1_Iter++ )
      cout << " " << *ms1_Iter;
   cout   << "." << endl;
}
```

```Output
The original multiset ms1 is: 10 20 30.
After swapping with ms2, list ms1 is: 100 200.
After swapping with ms3, list ms1 is: 300.
```

## <a name="multisetupper_bound"></a><a name="upper_bound"></a> `multiset::upper_bound`

返回一个迭代器，此迭代器指向 `multiset` 中其键大于指定键的第一个元素。

```cpp
const_iterator upper_bound(const Key& key) const;

iterator upper_bound(const Key& key);
```

### <a name="parameters"></a>parameters

*`key`*\
要与要搜索的元素的排序键进行比较的参数键 `multiset` 。

### <a name="return-value"></a>返回值

一个 **迭代器** 或 `const_iterator` ，用于发现中其 `multiset` 键大于参数键的元素的位置，或 `multiset` 如果未找到键的匹配项，则寻址中最后一个元素之后的位置。

### <a name="example"></a>示例

```cpp
// multiset_upper_bound.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main( )
{
   using namespace std;
   multiset <int> ms1;
   multiset <int> :: const_iterator ms1_AcIter, ms1_RcIter;

   ms1.insert( 10 );
   ms1.insert( 20 );
   ms1.insert( 30 );

   ms1_RcIter = ms1.upper_bound( 20 );
   cout << "The first element of multiset ms1 with a key greater "
           << "than 20 is: " << *ms1_RcIter << "." << endl;

   ms1_RcIter = ms1.upper_bound( 30 );

   // If no match is found for the key, end( ) is returned
   if ( ms1_RcIter == ms1.end( ) )
      cout << "The multiset ms1 doesn't have an element "
              << "with a key greater than 30." << endl;
   else
      cout << "The element of multiset ms1 with a key > 40 is: "
           << *ms1_RcIter << "." << endl;

   // The element at a specific location in the multiset can be
   // found using a dereferenced iterator addressing the location
   ms1_AcIter = ms1.begin( );
   ms1_RcIter = ms1.upper_bound( *ms1_AcIter );
   cout << "The first element of ms1 with a key greater than"
        << endl << "that of the initial element of ms1 is: "
        << *ms1_RcIter << "." << endl;
}
```

```Output
The first element of multiset ms1 with a key greater than 20 is: 30.
The multiset ms1 doesn't have an element with a key greater than 30.
The first element of ms1 with a key greater than
that of the initial element of ms1 is: 20.
```

## <a name="multisetvalue_comp"></a><a name="value_comp"></a> `multiset::value_comp`

检索用于对 `multiset` 中的元素值进行排序的比较对象副本。

```cpp
value_compare value_comp() const;
```

### <a name="return-value"></a>返回值

返回用于对 `multiset` 其元素进行排序的函数对象，即模板参数 `Compare` 。

有关的详细信息 `Compare` ，请参阅[ `multiset` 类](../standard-library/multiset-class.md)主题的 "备注" 部分。

### <a name="remarks"></a>备注

存储对象用于定义以下成员函数：

**bool 运算符** ( **const key&** `_xVal` ， **const key&** `_yVal`) ;

如果 `_xVal` `_yVal` 在排序顺序中先于且不等于，则返回 true。

[`key_compare`](#key_compare)和 [`value_compare`](#value_compare) 都是模板参数的同义词 `Compare` 。 这两种类型都是为和类集提供的 `multiset` ，它们在何处是相同的，以便与类映射和不同的类兼容 `multimap` 。

### <a name="example"></a>示例

```cpp
// multiset_value_comp.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main( )
{
   using namespace std;

   multiset <int, less<int> > ms1;
   multiset <int, less<int> >::value_compare vc1 = ms1.value_comp( );
   bool result1 = vc1( 2, 3 );
   if( result1 == true )
   {
      cout << "vc1( 2,3 ) returns value of true, "
           << "where vc1 is the function object of ms1."
           << endl;
   }
   else
   {
      cout << "vc1( 2,3 ) returns value of false, "
           << "where vc1 is the function object of ms1."
           << endl;
   }

   set <int, greater<int> > ms2;
   set<int, greater<int> >::value_compare vc2 = ms2.value_comp( );
   bool result2 = vc2( 2, 3 );
   if( result2 == true )
   {
      cout << "vc2( 2,3 ) returns value of true, "
           << "where vc2 is the function object of ms2."
           << endl;
   }
   else
   {
      cout << "vc2( 2,3 ) returns value of false, "
           << "where vc2 is the function object of ms2."
           << endl;
   }
}
```

```Output
vc1( 2,3 ) returns value of true, where vc1 is the function object of ms1.
vc2( 2,3 ) returns value of false, where vc2 is the function object of ms2.
```

## <a name="multisetvalue_compare"></a><a name="value_compare"></a> `multiset::value_compare`

提供函数对象的类型，该函数对象可比较两个排序键以确定它们在中的相对顺序 `multiset` 。

```cpp
typedef key_compare value_compare;
```

### <a name="remarks"></a>备注

`value_compare` 是模板参数 `Compare` 的同义词。

[`key_compare`](#key_compare)和 `value_compare` 都是模板参数的同义词 `Compare` 。 这两种类型都是为和类集提供的 `multiset` ，它们在何处是相同的，以便与类映射和不同的类兼容 `multimap` 。

有关 `Compare` 的详细信息，请参阅 [multiset 类](../standard-library/multiset-class.md)主题的备注部分。

### <a name="example"></a>示例

[`value_comp`](#value_comp)有关如何声明和使用的示例，请参见的示例 `value_compare` 。

## <a name="multisetvalue_type"></a><a name="value_type"></a> `multiset::value_type`

一种类型，它描述作为一个元素存储的对象作为 `multiset` 其容量中的作为一个值。

```cpp
typedef Key value_type;
```

### <a name="remarks"></a>备注

`value_type` 是模板参数 `Key` 的同义词。

[`key_type`](#key_type)和 `value_type` 都是模板参数的同义词 `Key` 。 这两种类型都是为类集和多重集提供的，它们是相同的，与类 map 和多重映射的兼容性不同。

有关 `Key` 的详细信息，请参阅该主题的备注部分。

### <a name="example"></a>示例

```cpp
// multiset_value_type.cpp
// compile with: /EHsc
#include <set>
#include <iostream>

int main( )
{
   using namespace std;
   multiset <int> ms1;
   multiset <int>::iterator ms1_Iter;

   multiset <int> :: value_type svt_Int;   // Declare value_type
   svt_Int = 10;             // Initialize value_type

   multiset <int> :: key_type skt_Int;   // Declare key_type
   skt_Int = 20;             // Initialize key_type

   ms1.insert( svt_Int );         // Insert value into s1
   ms1.insert( skt_Int );         // Insert key into s1

   // a multiset accepts key_types or value_types as elements
   cout << "The multiset has elements:";
   for ( ms1_Iter = ms1.begin( ) ; ms1_Iter != ms1.end( ); ms1_Iter++ )
      cout << " " << *ms1_Iter;
   cout << "." << endl;
}
```

```Output
The multiset has elements: 10 20.
```

## <a name="see-also"></a>另请参阅

[容器](./stl-containers.md)\
[C + + 标准库中的线程安全](../standard-library/thread-safety-in-the-cpp-standard-library.md)\
[C + + 标准库参考](../standard-library/cpp-standard-library-reference.md)