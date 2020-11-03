---
title: CObList 类
ms.date: 11/04/2016
f1_keywords:
- CObList
- AFXCOLL/CObList
- AFXCOLL/CObList::CObList
- AFXCOLL/CObList::AddHead
- AFXCOLL/CObList::AddTail
- AFXCOLL/CObList::Find
- AFXCOLL/CObList::FindIndex
- AFXCOLL/CObList::GetAt
- AFXCOLL/CObList::GetCount
- AFXCOLL/CObList::GetHead
- AFXCOLL/CObList::GetHeadPosition
- AFXCOLL/CObList::GetNext
- AFXCOLL/CObList::GetPrev
- AFXCOLL/CObList::GetSize
- AFXCOLL/CObList::GetTail
- AFXCOLL/CObList::GetTailPosition
- AFXCOLL/CObList::InsertAfter
- AFXCOLL/CObList::InsertBefore
- AFXCOLL/CObList::IsEmpty
- AFXCOLL/CObList::RemoveAll
- AFXCOLL/CObList::RemoveAt
- AFXCOLL/CObList::RemoveHead
- AFXCOLL/CObList::RemoveTail
- AFXCOLL/CObList::SetAt
helpviewer_keywords:
- CObList [MFC], CObList
- CObList [MFC], AddHead
- CObList [MFC], AddTail
- CObList [MFC], Find
- CObList [MFC], FindIndex
- CObList [MFC], GetAt
- CObList [MFC], GetCount
- CObList [MFC], GetHead
- CObList [MFC], GetHeadPosition
- CObList [MFC], GetNext
- CObList [MFC], GetPrev
- CObList [MFC], GetSize
- CObList [MFC], GetTail
- CObList [MFC], GetTailPosition
- CObList [MFC], InsertAfter
- CObList [MFC], InsertBefore
- CObList [MFC], IsEmpty
- CObList [MFC], RemoveAll
- CObList [MFC], RemoveAt
- CObList [MFC], RemoveHead
- CObList [MFC], RemoveTail
- CObList [MFC], SetAt
ms.assetid: 80699c93-33d8-4f8b-b8cf-7b58aeab64ca
ms.openlocfilehash: a13363ef9b200051c26781ab6e9870a10de06d88
ms.sourcegitcommit: 19016630f9d35f365e9ba249e0f3617515d7ca33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92274580"
---
# <a name="coblist-class"></a>CObList 类

支持 `CObject` 按顺序或指针值访问的不唯一指针的有序列表。

## <a name="syntax"></a>语法

```
class CObList : public CObject
```

## <a name="members"></a>成员

### <a name="public-constructors"></a>公共构造函数

|“属性”|说明|
|----------|-----------------|
|[CObList：： CObList](#coblist)|为指针构造空列表 `CObject` 。|

### <a name="public-methods"></a>公共方法

|“属性”|说明|
|----------|-----------------|
|[CObList：： AddHead](#addhead)|将一个元素 (或另一个列表中的所有元素) 添加到列表的开头 (会成为新的 head) 。|
|[CObList：： AddTail](#addtail)|将一个元素 (或另一个列表中的所有元素) 添加到列表的尾部 (会生成新的尾部) 。|
|[CObList：： Find](#find)|获取由指针值指定的元素的位置。|
|[CObList：： FindIndex](#findindex)|获取以零为基的索引指定的元素的位置。|
|[CObList：： GetAt](#getat)|获取给定位置处的元素。|
|[CObList：： GetCount](#getcount)|返回此列表中的元素数。|
|[CObList：： GetHead](#gethead)|返回 (不能为空) 列表的头元素。|
|[CObList：： GetHeadPosition](#getheadposition)|返回列表头元素的位置。|
|[CObList：： GetNext](#getnext)|获取用于循环访问的下一个元素。|
|[CObList：： GetPrev](#getprev)|获取用于循环访问的上一个元素。|
|[CObList：： GetSize](#getsize)|返回此列表中的元素数。|
|[CObList：： GetTail](#gettail)|返回列表的尾元素， (不能为空) 。|
|[CObList：： GetTailPosition](#gettailposition)|返回列表的尾元素的位置。|
|[CObList：： InsertAfter](#insertafter)|将新元素插入到给定位置之后。|
|[CObList：： InsertBefore](#insertbefore)|将新元素插入到给定位置之前。|
|[CObList：： IsEmpty](#isempty)|测试空列表条件 (不) 任何元素。|
|[CObList：： RemoveAll](#removeall)|从此列表中移除所有元素。|
|[CObList：： RemoveAt](#removeat)|从此列表中移除按位置指定的元素。|
|[CObList：： RemoveHead](#removehead)|从列表头中删除元素。|
|[CObList：： RemoveTail](#removetail)|从列表的末尾移除元素。|
|[CObList：： SetAt](#setat)|设置位于给定位置的元素。|

## <a name="remarks"></a>备注

`CObList` 列表的行为类似于双向链接列表。

POSITION 类型的变量是列表的键。 您可以使用 POSITION 变量作为迭代器来按顺序遍历列表，并将其作为书签来保存位置。 但位置不同于索引。

元素插入速度非常快，在列表头、尾部和已知位置。 需要顺序搜索按值或索引查找元素。 如果列表很长，则此搜索可能会很慢。

`CObList` 合并 IMPLEMENT_SERIAL 宏，以支持其元素的序列化和转储。 如果 `CObject` 使用重载的插入运算符或成员函数将指针列表存储到存档中，则 `Serialize` `CObject` 将依次序列化每个元素。

如果需要转储列表中的各个 `CObject` 元素，则必须将转储上下文的深度设置为1或更大。

`CObList`删除对象时，或删除其元素时，只 `CObject` 会删除指针，而不会删除它们引用的对象。

您可以从派生您自己的类 `CObList` 。 新的列表类，旨在保存指向派生自的对象的指针 `CObject` ，并添加新的数据成员和新的成员函数。 请注意，结果列表并不是严格类型安全的，因为它允许插入任何 `CObject` 指针。

> [!NOTE]
> 如果打算序列化列表，则必须在派生类的实现中使用 [IMPLEMENT_SERIAL](run-time-object-model-services.md#implement_serial) 宏。

有关使用的详细信息 `CObList` ，请参阅文章 [集合](../../mfc/collections.md)。

## <a name="inheritance-hierarchy"></a>继承层次结构

[CObject](../../mfc/reference/cobject-class.md)

`CObList`

## <a name="requirements"></a>要求

**标头：** afxcoll。h

## <a name="coblistaddhead"></a><a name="addhead"></a> CObList：： AddHead

将一个或一组新元素添加到此列表的开头。

```
POSITION AddHead(CObject* newElement);
void AddHead(CObList* pNewList);
```

### <a name="parameters"></a>参数

*（Newelement*<br/>
`CObject`要添加到此列表中的指针。

*pNewList*<br/>
指向其他列表的指针 `CObList` 。 *PNewList*中的元素将添加到此列表中。

### <a name="return-value"></a>返回值

第一个版本返回新插入元素的位置值。

下表显示了与类似的其他成员函数 `CObList::AddHead` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|**将 AddHead ( void** <strong>\*</strong>`newElement` **) ;**<br /><br /> **Void AddHead ( CPtrList** <strong>\*</strong>`pNewList` **) ;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**将 AddHead (Const CString&** `newElement`**) ;**<br /><br /> **将 AddHead (LPCTSTR** `newElement`**) ;**<br /><br /> **Void AddHead (CStringList** <strong>\*</strong>`pNewList` **) ;**|

### <a name="remarks"></a>备注

此列表在操作之前可以为空。

### <a name="example"></a>示例

  有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

[!code-cpp[NVC_MFCCollections#89](../../mfc/codesnippet/cpp/coblist-class_1.cpp)]

此程序的结果如下所示：

```Output
AddHead example: A CObList with 2 elements
a CAge at $44A8 40
a CAge at $442A 21
```

## <a name="coblistaddtail"></a><a name="addtail"></a> CObList：： AddTail

将一个或一组新元素添加到此列表的尾部。

```
POSITION AddTail(CObject* newElement);
void AddTail(CObList* pNewList);
```

### <a name="parameters"></a>参数

*（Newelement*<br/>
`CObject`要添加到此列表中的指针。

*pNewList*<br/>
指向其他列表的指针 `CObList` 。 *PNewList*中的元素将添加到此列表中。

### <a name="return-value"></a>返回值

第一个版本返回新插入元素的位置值。

### <a name="remarks"></a>备注

此列表在操作之前可以为空。

下表显示了与类似的其他成员函数 `CObList::AddTail` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|**将 AddTail ( void** <strong>\*</strong>`newElement` **) ;**<br /><br /> **Void AddTail ( CPtrList** <strong>\*</strong>`pNewList` **) ;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**将 AddTail ( Const CString&** `newElement`**) ;**<br /><br /> **将 AddTail ( LPCTSTR** `newElement`**) ;**<br /><br /> **Void AddTail ( CStringList** <strong>\*</strong>`pNewList` **) ;**|

### <a name="example"></a>示例

  有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

[!code-cpp[NVC_MFCCollections#90](../../mfc/codesnippet/cpp/coblist-class_2.cpp)]

此程序的结果如下所示：

```Output
AddTail example: A CObList with 2 elements
a CAge at $444A 21
a CAge at $4526 40
```

## <a name="coblistcoblist"></a><a name="coblist"></a> CObList：： CObList

构造空的 `CObject` 指针列表。

```
CObList(INT_PTR nBlockSize = 10);
```

### <a name="parameters"></a>参数

*nBlockSize*<br/>
用于扩展列表的内存分配粒度。

### <a name="remarks"></a>备注

随着列表的增长，内存是以 *nBlockSize* 项的单位分配的。 如果内存分配失败， `CMemoryException` 则会引发。

下表显示了与类似的其他成员函数 `CObList::CObList` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|**CPtrList ( INT_PTR** `nBlockSize`**= 10 ) ;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**CStringList ( INT_PTR** `nBlockSize`**= 10 ) ;**|

### <a name="example"></a>示例

  下面是派生类的列表，该列表 `CObject` `CAge` 用于所有集合示例：

[!code-cpp[NVC_MFCCollections#91](../../mfc/codesnippet/cpp/coblist-class_3.h)]

下面是 `CObList` 构造函数用法的示例：

[!code-cpp[NVC_MFCCollections#92](../../mfc/codesnippet/cpp/coblist-class_4.cpp)]

## <a name="coblistfind"></a><a name="find"></a> CObList：： Find

按顺序搜索列表，查找与 `CObject` 指定指针匹配的第一个指针 `CObject` 。

```
POSITION Find(
    CObject* searchValue,
    POSITION startAfter = NULL) const;
```

### <a name="parameters"></a>参数

*searchValue*<br/>
要在此列表中找到的对象指针。

*startAfter*<br/>
搜索的起始位置。

### <a name="return-value"></a>返回值

可用于迭代或对象指针检索的位置值;如果找不到该对象，则为 NULL。

### <a name="remarks"></a>备注

请注意，将对指针值进行比较，而不是对象的内容。

下表显示了与类似的其他成员函数 `CObList::Find` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|**POSITION 查找 ( void** <strong>\*</strong>`searchValue` **，POSITION** `startAfter` **= NULL ) const;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**定位查找 ( LPCTSTR** `searchValue`**、位置** `startAfter`**= NULL ) const;**|

### <a name="example"></a>示例

有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

[!code-cpp[NVC_MFCCollections#93](../../mfc/codesnippet/cpp/coblist-class_5.cpp)]

## <a name="coblistfindindex"></a><a name="findindex"></a> CObList：： FindIndex

使用 *nIndex* 的值作为列表中的索引。

```
POSITION FindIndex(INT_PTR nIndex) const;
```

### <a name="parameters"></a>参数

*nIndex*<br/>
要查找的列表元素的从零开始的索引。

### <a name="return-value"></a>返回值

可用于迭代或对象指针检索的位置值;如果 *nIndex* 太大，则为 NULL。 如果 *nIndex* 为负，则框架 (生成一个断言。 ) 

### <a name="remarks"></a>备注

它从列表的开头开始顺序扫描，在第 *n*个元素处停止。

下表显示了与类似的其他成员函数 `CObList::FindIndex` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|**将 FindIndex ( INT_PTR** `nIndex`**) const;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**将 FindIndex ( INT_PTR** `nIndex`**) const;**|

### <a name="example"></a>示例

有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

[!code-cpp[NVC_MFCCollections#94](../../mfc/codesnippet/cpp/coblist-class_6.cpp)]

## <a name="coblistgetat"></a><a name="getat"></a> CObList：： GetAt

POSITION 类型的变量是列表的键。

```
CObject*& GetAt(POSITION position);
const CObject*& GetAt(POSITION position) const;
```

### <a name="parameters"></a>参数

*position*<br/>
由 previous `GetHeadPosition` 或成员函数调用返回的位置值 `Find` 。

### <a name="return-value"></a>返回值

请参阅 [GetHead](#gethead)的返回值说明。

### <a name="remarks"></a>备注

它与索引不同，不能自行操作位置值。 `GetAt` 检索 `CObject` 与给定位置关联的指针。

您必须确保您的位置值表示列表中的有效位置。 如果无效，则 Microsoft 基础类库断言的调试版本。

下表显示了与类似的其他成员函数 `CObList::GetAt` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|**const void \*& GETAT ( position** *位置* **) const;**<br /><br /> **void \*& GETAT ( 位置***位置* **) ;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**Const CString& GetAt ( position** *位置* **) const;**<br /><br /> **CString& GetAt ( 位置***位置* **) ;**|

### <a name="example"></a>示例

  请参阅 [FindIndex](#findindex)的示例。

## <a name="coblistgetcount"></a><a name="getcount"></a> CObList：： GetCount

获取此列表中的元素数。

```
INT_PTR GetCount() const;
```

### <a name="return-value"></a>返回值

一个包含元素计数的整数值。

下表显示了与类似的其他成员函数 `CObList::GetCount` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|**INT_PTR GetCount ( ) const;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**INT_PTR GetCount ( ) const;**|

### <a name="example"></a>示例

有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

[!code-cpp[NVC_MFCCollections#95](../../mfc/codesnippet/cpp/coblist-class_7.cpp)]

## <a name="coblistgethead"></a><a name="gethead"></a> CObList：： GetHead

获取 `CObject` 表示此列表头元素的指针。

```
CObject*& GetHead();
const CObject*& GetHead() const;
```

### <a name="return-value"></a>返回值

如果通过指向的指针访问列表 `const CObList` ，则 `GetHead` 返回 `CObject` 指针。 这只允许在赋值语句右侧使用该函数，从而保护列表不被修改。

如果直接访问该列表或通过指向的指针访问该列表 `CObList` ，则 `GetHead` 返回对指针的引用 `CObject` 。 这允许将函数用于赋值语句的任意一侧，从而允许修改列表项。

### <a name="remarks"></a>备注

在调用之前，必须确保列表不为空 `GetHead` 。 如果列表为空，则 Microsoft 基础类库断言的调试版本。 使用 [IsEmpty](#isempty) 验证列表中是否包含元素。

下表显示了与类似的其他成员函数 `CObList::GetHead` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|**const void \*& GetHead ( ) const; void \*& GetHead ( ) ;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**const CString& GetHead ( ) const;GetHead ( ) 的 CString&;**|

### <a name="example"></a>示例

有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

下面的示例演示如何 `GetHead` 在赋值语句的左侧使用。

[!code-cpp[NVC_MFCCollections#96](../../mfc/codesnippet/cpp/coblist-class_8.cpp)]

## <a name="coblistgetheadposition"></a><a name="getheadposition"></a> CObList：： GetHeadPosition

获取此列表的头元素的位置。

```
POSITION GetHeadPosition() const;
```

### <a name="return-value"></a>返回值

可用于迭代或对象指针检索的位置值;如果列表为空，则为 NULL。

下表显示了与类似的其他成员函数 `CObList::GetHeadPosition` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|**将 GetHeadPosition ( ) const;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**将 GetHeadPosition ( ) const;**|

### <a name="example"></a>示例

有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

[!code-cpp[NVC_MFCCollections#97](../../mfc/codesnippet/cpp/coblist-class_9.cpp)]

## <a name="coblistgetnext"></a><a name="getnext"></a> CObList：： GetNext

获取由 *rPosition*标识的列表元素，然后将 *rPosition* 设置为 `POSITION` 列表中下一项的值。

```
CObject*& GetNext(POSITION& rPosition);
const CObject* GetNext(POSITION& rPosition) const;
```

### <a name="parameters"></a>参数

*rPosition*<br/>
对上一个 `GetNext` 、 `GetHeadPosition` 或其他成员函数调用返回的位置值的引用。

### <a name="return-value"></a>返回值

请参阅 [GetHead](#gethead)的返回值说明。

### <a name="remarks"></a>备注

`GetNext`如果使用或调用建立初始位置，则可以在向前迭代循环中使用 `GetHeadPosition` `Find` 。

您必须确保您的位置值表示列表中的有效位置。 如果无效，则 Microsoft 基础类库断言的调试版本。

如果检索到的元素是列表中的最后一个，则 *rPosition* 的新值将设置为 NULL。

可以在迭代期间删除某个元素。 请参阅 [RemoveAt](#removeat)的示例。

> [!NOTE]
> 从 MFC 8.0 起，此方法的 const 版本已更改为返回 `const CObject*` 而不是 `const CObject*&` 。  进行此更改是为了使编译器符合 c + + 标准。

下表显示了与类似的其他成员函数 `CObList::GetNext` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|`void*& GetNext( POSITION&` `rPosition` `);`<br /><br /> `const void* GetNext( POSITION&` `rPosition` `) const;`|
|[CStringList](../../mfc/reference/cstringlist-class.md)|`CString& GetNext( POSITION&` `rPosition` `);`<br /><br /> `const CString& GetNext( POSITION&` `rPosition` `) const;`|

### <a name="example"></a>示例

  有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

[!code-cpp[NVC_MFCCollections#98](../../mfc/codesnippet/cpp/coblist-class_10.cpp)]

此程序的结果如下所示：

```Output
a CAge at $479C 40
a CAge at $46C0 21
```

## <a name="coblistgetprev"></a><a name="getprev"></a> CObList：： GetPrev

获取由 *rPosition*标识的列表元素，然后将 *rPosition* 设置为列表中上一项的位置值。

```
CObject*& GetPrev(POSITION& rPosition);
const CObject* GetPrev(POSITION& rPosition) const;
```

### <a name="parameters"></a>参数

*rPosition*<br/>
对上一个 `GetPrev` 或其他成员函数调用返回的位置值的引用。

### <a name="return-value"></a>返回值

请参阅 [GetHead](#gethead)的返回值说明。

### <a name="remarks"></a>备注

`GetPrev`如果使用或调用建立初始位置，则可以在反向迭代循环中使用 `GetTailPosition` `Find` 。

您必须确保您的位置值表示列表中的有效位置。 如果无效，则 Microsoft 基础类库断言的调试版本。

如果检索到的元素是列表中的第一个元素，则 *rPosition* 的新值将设置为 NULL。

> [!NOTE]
> 从 MFC 8.0 起，此方法的 const 版本已更改为返回 `const CObject*` 而不是 `const CObject*&` 。  进行此更改是为了使编译器符合 c + + 标准。

下表显示了与类似的其他成员函数 `CObList::GetPrev` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|`void*& GetPrev( POSITION&` `rPosition` `);`<br /><br /> `const void* GetPrev( POSITION&` `rPosition` `) const;`|
|[CStringList](../../mfc/reference/cstringlist-class.md)|`CString& GetPrev( POSITION&` `rPosition` `);`<br /><br /> `const CString& GetPrev( POSITION&` `rPosition` `) const;`|

### <a name="example"></a>示例

  有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

[!code-cpp[NVC_MFCCollections#99](../../mfc/codesnippet/cpp/coblist-class_11.cpp)]

此程序的结果如下所示：

```Output
a CAge at $421C 21
a CAge at $421C 40
```

## <a name="coblistgetsize"></a><a name="getsize"></a> CObList：： GetSize

返回列表元素的数目。

```
INT_PTR GetSize() const;
```

### <a name="return-value"></a>返回值

列表中的项数。

### <a name="remarks"></a>备注

调用此方法可检索列表中的元素数。

下表显示了与类似的其他成员函数 `CObList::GetSize` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|**INT_PTR GetSize ( ) const;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**INT_PTR GetSize ( ) const;**|

### <a name="example"></a>示例

有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

[!code-cpp[NVC_MFCCollections#100](../../mfc/codesnippet/cpp/coblist-class_12.cpp)]

## <a name="coblistgettail"></a><a name="gettail"></a> CObList：： GetTail

获取 `CObject` 表示此列表的尾元素的指针。

```
CObject*& GetTail();
const CObject*& GetTail() const;
```

### <a name="return-value"></a>返回值

请参阅 [GetHead](#gethead)的返回值说明。

### <a name="remarks"></a>备注

在调用之前，必须确保列表不为空 `GetTail` 。 如果列表为空，则 Microsoft 基础类库断言的调试版本。 使用 [IsEmpty](#isempty) 验证列表中是否包含元素。

下表显示了与类似的其他成员函数 `CObList::GetTail` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|**const void \*& GetTail ( ) const; void \*& GetTail ( ) ;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**const CString& GetTail ( ) const;GetTail ( ) 的 CString&;**|

### <a name="example"></a>示例

有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

[!code-cpp[NVC_MFCCollections#101](../../mfc/codesnippet/cpp/coblist-class_13.cpp)]

## <a name="coblistgettailposition"></a><a name="gettailposition"></a> CObList：： GetTailPosition

获取此列表的尾元素的位置;如果列表为空，则 **为 NULL** 。

```
POSITION GetTailPosition() const;
```

### <a name="return-value"></a>返回值

可用于迭代或对象指针检索的位置值;如果列表为空，则为 NULL。

下表显示了与类似的其他成员函数 `CObList::GetTailPosition` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|**将 GetTailPosition ( ) const;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**将 GetTailPosition ( ) const;**|

### <a name="example"></a>示例

有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

[!code-cpp[NVC_MFCCollections#102](../../mfc/codesnippet/cpp/coblist-class_14.cpp)]

## <a name="coblistinsertafter"></a><a name="insertafter"></a> CObList：： InsertAfter

将元素添加到此列表中位于指定位置的元素之后。

```
POSITION InsertAfter(
    POSITION position,
    CObject* newElement);
```

### <a name="parameters"></a>参数

*position*<br/>
先前的 `GetNext` 、 `GetPrev` 或 `Find` 成员函数调用返回的位置值。

*（Newelement*<br/>
要添加到此列表中的对象指针。

下表显示了与类似的其他成员函数 `CObList::InsertAfter` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|位置**InsertAfter ( 位置***位置* **，void** <strong>\*</strong> `newElement` **) ;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**位置 InsertAfter ( 位置***位置* **，const CString&** `newElement` **) ;**<br /><br /> **位置 InsertAfter ( 位置***位置* **，LPCTSTR** `newElement` **) ;**|

### <a name="return-value"></a>返回值

与 *position* 参数相同的位置值。

### <a name="example"></a>示例

  有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

[!code-cpp[NVC_MFCCollections#103](../../mfc/codesnippet/cpp/coblist-class_15.cpp)]

此程序的结果如下所示：

```Output
InsertAfter example: A CObList with 3 elements
a CAge at $4A44 40
a CAge at $4A64 65
a CAge at $4968 21
```

## <a name="coblistinsertbefore"></a><a name="insertbefore"></a> CObList：： InsertBefore

将元素添加到此列表中指定位置处的元素之前。

```
POSITION InsertBefore(
    POSITION position,
    CObject* newElement);
```

### <a name="parameters"></a>参数

*position*<br/>
先前的 `GetNext` 、 `GetPrev` 或 `Find` 成员函数调用返回的位置值。

*（Newelement*<br/>
要添加到此列表中的对象指针。

### <a name="return-value"></a>返回值

可用于迭代或对象指针检索的位置值;如果列表为空，则为 NULL。

下表显示了与类似的其他成员函数 `CObList::InsertBefore` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|位置**InsertBefore ( 位置***位置* **，void** <strong>\*</strong> `newElement` **) ;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**位置 InsertBefore ( 位置***位置* **，const CString&** `newElement` **) ;**<br /><br /> **位置 InsertBefore ( 位置***位置* **，LPCTSTR** `newElement` **) ;**|

### <a name="example"></a>示例

  有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

[!code-cpp[NVC_MFCCollections#104](../../mfc/codesnippet/cpp/coblist-class_16.cpp)]

此程序的结果如下所示：

```Output
InsertBefore example: A CObList with 3 elements
a CAge at $4AE2 40
a CAge at $4B02 65
a CAge at $49E6 21
```

## <a name="coblistisempty"></a><a name="isempty"></a> CObList：： IsEmpty

指示此列表是否不包含任何元素。

```
BOOL IsEmpty() const;
```

### <a name="return-value"></a>返回值

如果此列表为空，则为非零值;否则为0。

下表显示了与类似的其他成员函数 `CObList::IsEmpty` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|**BOOL IsEmpty ( ) const;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**BOOL IsEmpty ( ) const;**|

### <a name="example"></a>示例

  请参阅 [RemoveAll](#removeall)的示例。

## <a name="coblistremoveall"></a><a name="removeall"></a> CObList：： RemoveAll

删除此列表中的所有元素并释放关联的 `CObList` 内存。

```cpp
void RemoveAll();
```

### <a name="remarks"></a>备注

如果该列表已为空，则不会生成任何错误。

在从中删除元素时 `CObList` ，将从列表中删除对象指针。 你负责删除对象本身。

下表显示了与类似的其他成员函数 `CObList::RemoveAll` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|**void RemoveAll ( ) ;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**void RemoveAll ( ) ;**|

### <a name="example"></a>示例

有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

[!code-cpp[NVC_MFCCollections#105](../../mfc/codesnippet/cpp/coblist-class_17.cpp)]

## <a name="coblistremoveat"></a><a name="removeat"></a> CObList：： RemoveAt

从此列表中移除指定的元素。

```cpp
void RemoveAt(POSITION position);
```

### <a name="parameters"></a>参数

*position*<br/>
要从列表中移除的元素的位置。

### <a name="remarks"></a>备注

在从中删除元素时 `CObList` ，将从列表中删除对象指针。 你负责删除对象本身。

您必须确保您的位置值表示列表中的有效位置。 如果无效，则 Microsoft 基础类库断言的调试版本。

下表显示了与类似的其他成员函数 `CObList::RemoveAt` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|**Void RemoveAt ( 位置***位置* **) ;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**Void RemoveAt ( 位置***位置* **) ;**|

### <a name="example"></a>示例

  在列表迭代期间删除元素时请小心。 下面的示例演示了一种可保证[GetNext](#getnext)的有效**位置**值的删除技术。

有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

[!code-cpp[NVC_MFCCollections#106](../../mfc/codesnippet/cpp/coblist-class_18.cpp)]

此程序的结果如下所示：

`RemoveAt example: A CObList with 2 elements`

`a CAge at $4C1E 65`

`a CAge at $4B22 21`

## <a name="coblistremovehead"></a><a name="removehead"></a> CObList：： RemoveHead

从列表的开头移除元素，并返回指向它的指针。

```
CObject* RemoveHead();
```

### <a name="return-value"></a>返回值

`CObject`之前位于列表的开头的指针。

### <a name="remarks"></a>备注

在调用之前，必须确保列表不为空 `RemoveHead` 。 如果列表为空，则 Microsoft 基础类库断言的调试版本。 使用 [IsEmpty](#isempty) 验证列表中是否包含元素。

下表显示了与类似的其他成员函数 `CObList::RemoveHead` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|**void \* RemoveHead ( ) ;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**CString RemoveHead ( ) ;**|

### <a name="example"></a>示例

有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

[!code-cpp[NVC_MFCCollections#107](../../mfc/codesnippet/cpp/coblist-class_19.cpp)]

## <a name="coblistremovetail"></a><a name="removetail"></a> CObList：： RemoveTail

删除列表末尾的元素，并返回指向它的指针。

```
CObject* RemoveTail();
```

### <a name="return-value"></a>返回值

指向位于列表末尾的对象的指针。

### <a name="remarks"></a>备注

在调用之前，必须确保列表不为空 `RemoveTail` 。 如果列表为空，则 Microsoft 基础类库断言的调试版本。 使用 [IsEmpty](#isempty) 验证列表中是否包含元素。

下表显示了与类似的其他成员函数 `CObList::RemoveTail` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|**void \* RemoveTail ( ) ;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**CString RemoveTail ( ) ;**|

### <a name="example"></a>示例

有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

[!code-cpp[NVC_MFCCollections#108](../../mfc/codesnippet/cpp/coblist-class_20.cpp)]

## <a name="coblistsetat"></a><a name="setat"></a> CObList：： SetAt

设置位于给定位置的元素。

```cpp
void SetAt(
    POSITION pos,
    CObject* newElement);
```

### <a name="parameters"></a>参数

pos<br/>
要设置的元素的位置。

*（Newelement*<br/>
`CObject`要写入到列表中的指针。

### <a name="remarks"></a>备注

POSITION 类型的变量是列表的键。 它与索引不同，不能自行操作位置值。 `SetAt` 将 `CObject` 指针写入到列表中的指定位置。

您必须确保您的位置值表示列表中的有效位置。 如果无效，则 Microsoft 基础类库断言的调试版本。

下表显示了与类似的其他成员函数 `CObList::SetAt` 。

|类|成员函数|
|-----------|---------------------|
|[CPtrList](../../mfc/reference/cptrlist-class.md)|**Void SetAt ( 位置** `pos`**，Const CString&** `newElement`**) ;**|
|[CStringList](../../mfc/reference/cstringlist-class.md)|**Void SetAt ( 位置** `pos`**，LPCTSTR** `newElement`**) ;**|

### <a name="example"></a>示例

  有关类的列表，请参阅 [CObList：： CObList](#coblist) `CAge` 。

[!code-cpp[NVC_MFCCollections#109](../../mfc/codesnippet/cpp/coblist-class_21.cpp)]

此程序的结果如下所示：

```Output
SetAt example: A CObList with 2 elements
a CAge at $4D98 40
a CAge at $4DB8 65
```

## <a name="see-also"></a>另请参阅

[CObject 类](../../mfc/reference/cobject-class.md)<br/>
[层次结构图](../../mfc/hierarchy-chart.md)<br/>
[CStringList 类](../../mfc/reference/cstringlist-class.md)<br/>
[CPtrList 类](../../mfc/reference/cptrlist-class.md)
