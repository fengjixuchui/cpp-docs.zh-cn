---
title: 泛型接口 (C++/CLI)
ms.date: 10/12/2018
ms.topic: reference
helpviewer_keywords:
- generic interfaces
- interfaces, generic [C++}
ms.assetid: f3da788a-ba83-4db7-9dcf-9b95a8fb9d1a
ms.openlocfilehash: 61ab514d244c8b41d467d382fa97e30556ccbb32
ms.sourcegitcommit: ced5ff1431ffbd25b20d106901955532723bd188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2020
ms.locfileid: "92135523"
---
# <a name="generic-interfaces-ccli"></a>泛型接口 (C++/CLI)

类和接口中的类型参数要遵循的限制相同（请参阅[泛型类 (C++/CLI)](generic-classes-cpp-cli.md)）。

对于泛型类或泛型接口中的函数，控制函数重载的规则是相同的。

显式接口成员实现处理构造接口类型和简单接口类型的方式相同（请参阅下面的示例）。

若要详细了解接口，请参阅 [interface class](interface-class-cpp-component-extensions.md)。

## <a name="syntax"></a>语法

```cpp
[attributes] generic <class-key type-parameter-identifier[, ...]>
[type-parameter-constraints-clauses][accesibility-modifiers] interface class identifier [: base-list] {   interface-body} [declarators] ;
```

## <a name="remarks"></a>备注

*attributes*<br/>
（可选）其他声明性信息。 有关特性和特性类的详细信息，请参阅 **特性**。

class-key**<br/>
**`class`** 或 **`typename`**

type-parameter-identifier(s)**<br/>
标识符的逗号分隔列表。

type-parameter-constraints-clauses**<br/>
采用[泛型类型参数的约束 (C++/CLI)](constraints-on-generic-type-parameters-cpp-cli.md) 中指定的形式

accessibility-modifiers**<br/>
 (可选) 可访问性修饰符 (例如 **public、private**) 。

*identifier*<br/>
接口名称。

base-list**<br/>
（可选）包含一个或多个显式基接口的逗号分隔列表。

interface-body**<br/>
接口成员的声明。

*声明符*<br/>
（可选）基于此类型的变量的声明。

## <a name="example-how-to-declare-and-instantiate-a-generic-interface"></a>示例：如何声明和实例化泛型接口

下面的示例展示了如何声明和实例化泛型接口。 此示例声明泛型接口 `IList<ItemType>`。 然后，两个泛型类（`List1<ItemType>` 和 `List2<ItemType>`）通过不同实现来实现它。

```cpp
// generic_interface.cpp
// compile with: /clr
using namespace System;

// An exception to be thrown by the List when
// attempting to access elements beyond the
// end of the list.
ref class ElementNotFoundException : Exception {};

// A generic List interface
generic <typename ItemType>
public interface class IList {
   ItemType MoveFirst();
   bool Add(ItemType item);
   bool AtEnd();
   ItemType Current();
   void MoveNext();
};

// A linked list implementation of IList
generic <typename ItemType>
public ref class List1 : public IList<ItemType> {
   ref class Node {
      ItemType m_item;

   public:
      ItemType get_Item() { return m_item; };
      void set_Item(ItemType value) { m_item = value; };

      Node^ next;

      Node(ItemType item) {
         m_item = item;
         next = nullptr;
      }
   };

   Node^ first;
   Node^ last;
   Node^ current;

   public:
   List1() {
      first = nullptr;
      last = first;
      current = first;
   }

   virtual ItemType MoveFirst() {
      current = first;
      if (first != nullptr)
        return first->get_Item();
      else
         return ItemType();
   }

   virtual bool Add(ItemType item) {
      if (last != nullptr) {
         last->next = gcnew Node(item);
         last = last->next;
      }
      else {
         first = gcnew Node(item);
         last = first;
         current = first;
      }
      return true;
   }

   virtual bool AtEnd() {
      if (current == nullptr )
        return true;
      else
        return false;
   }

   virtual ItemType Current() {
       if (current != nullptr)
         return current->get_Item();
       else
         throw gcnew ElementNotFoundException();
   }

   virtual void MoveNext() {
      if (current != nullptr)
       current = current->next;
      else
        throw gcnew ElementNotFoundException();
   }
};

// An array implementation of IList
generic <typename ItemType>
ref class List2 : public IList<ItemType> {
   array<ItemType>^ item_array;
   int count;
   int current;

   public:

   List2() {
      // not yet possible to declare an
      // array of a generic type parameter
      item_array = gcnew array<ItemType>(256);
      count = current = 0;
   }

   virtual ItemType MoveFirst() {
      current = 0;
      return item_array[0];
   }

   virtual bool Add(ItemType item) {
      if (count < 256)
         item_array[count++] = item;
      else
        return false;
      return true;
   }

   virtual bool AtEnd() {
      if (current >= count)
        return true;
      else
        return false;
   }

   virtual ItemType Current() {
      if (current < count)
        return item_array[current];
      else
        throw gcnew ElementNotFoundException();
   }

   virtual void MoveNext() {
      if (current < count)
         ++current;
      else
         throw gcnew ElementNotFoundException();
   }
};

// Add elements to the list and display them.
generic <typename ItemType>
void AddStringsAndDisplay(IList<ItemType>^ list, ItemType item1, ItemType item2) {
   list->Add(item1);
   list->Add(item2);
   for (list->MoveFirst(); ! list->AtEnd(); list->MoveNext())
   Console::WriteLine(list->Current());
}

int main() {
   // Instantiate both types of list.

   List1<String^>^ list1 = gcnew List1<String^>();
   List2<String^>^ list2 = gcnew List2<String^>();

   // Use the linked list implementation of IList.
   AddStringsAndDisplay<String^>(list1, "Linked List", "List1");

   // Use the array implementation of the IList.
   AddStringsAndDisplay<String^>(list2, "Array List", "List2");
}
```

```Output
Linked List
List1
Array List
List2
```

## <a name="example-declare-a-generic-interface"></a>示例：声明泛型接口

下面的示例声明一个泛型接口 `IMyGenIface`，以及两个专用化 `IMyGenIface` 的非泛型接口（`IMySpecializedInt` 和 `ImySpecializedString`）。 然后，两个专用化接口由两个类（`MyIntClass` 和 `MyStringClass`）实现。 此示例展示了如何专用化泛型接口、如何实例化泛型接口和非泛型接口，以及如何调用接口中已显式实现的成员。

```cpp
// generic_interface2.cpp
// compile with: /clr
// Specializing and implementing generic interfaces.
using namespace System;

generic <class ItemType>
public interface class IMyGenIface {
   void Initialize(ItemType f);
};

public interface class IMySpecializedInt: public IMyGenIface<int> {
   void Display();
};

public interface class IMySpecializedString: public IMyGenIface<String^> {
   void Display();
};

public ref class MyIntClass: public IMySpecializedInt {
   int myField;

public:
   virtual void Initialize(int f) {
      myField = f;
   }

   virtual void Display() {
      Console::WriteLine("The integer field contains: {0}", myField);
   }
};

public ref struct MyStringClass: IMySpecializedString {
   String^ myField;

public:
   virtual void Initialize(String^ f) {
      myField = f;
    }

   virtual void Display() {
      Console::WriteLine("The String field contains: {0}", myField);
   }
};

int main() {
   // Instantiate the generic interface.
   IMyGenIface<int>^ myIntObj = gcnew MyIntClass();

   // Instantiate the specialized interface "IMySpecializedInt."
   IMySpecializedInt^ mySpIntObj = (IMySpecializedInt^) myIntObj;

   // Instantiate the generic interface.
   IMyGenIface<String^>^ myStringObj = gcnew MyStringClass();

   // Instantiate the specialized interface "IMySpecializedString."
   IMySpecializedString^ mySpStringObj =
            (IMySpecializedString^) myStringObj;

   // Call the explicitly implemented interface members.
   myIntObj->Initialize(1234);
   mySpIntObj->Display();

   myStringObj->Initialize("My string");
   mySpStringObj->Display();
}
```

```Output
The integer field contains: 1234
The String field contains: My string
```

## <a name="see-also"></a>请参阅

[泛型](generics-cpp-component-extensions.md)
