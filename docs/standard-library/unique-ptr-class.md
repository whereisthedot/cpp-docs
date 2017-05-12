---
title: "unique_ptr Class | Microsoft Docs"
ms.custom: ""
ms.date: "11/04/2016"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-cpp"
ms.tgt_pltfrm: ""
ms.topic: "article"
f1_keywords: ['unique_ptr', 'memory/std::unique_ptr', 'memory/std::unique_ptr::deleter_type', 'memory/std::unique_ptr::element_type', 'memory/std::unique_ptr::pointer', 'memory/std::unique_ptr::get', 'memory/std::unique_ptr::get_deleter', 'memory/std::unique_ptr::release', 'memory/std::unique_ptr::reset', 'memory/std::unique_ptr::swap']  
dev_langs: 
  - "C++"
helpviewer_keywords: 
  - "unique_ptr class"
ms.assetid: acdf046b-831e-4a4a-83aa-6d4ee467db9a
caps.latest.revision: 22
author: "corob-msft"
ms.author: "corob"
manager: "ghogen"
translation.priority.ht: 
  - "cs-cz"
  - "de-de"
  - "es-es"
  - "fr-fr"
  - "it-it"
  - "ja-jp"
  - "ko-kr"
  - "pl-pl"
  - "pt-br"
  - "ru-ru"
  - "tr-tr"
  - "zh-cn"
  - "zh-tw"
---
# unique_ptr Class
Stores a pointer to an owned object or array. The object/array is owned by no other `unique_ptr`. The object/array is destroyed when the `unique_ptr` is destroyed.  
  
## Syntax  
```  
class unique_ptr {
public:
    unique_ptr();
    unique_ptr(nullptr_t Nptr);
    explicit unique_ptr(pointer Ptr);
    unique_ptr(pointer Ptr,
        typename conditional<is_reference<Del>::value, Del,
        typename add_reference<const Del>::type>::type Deleter);
    unique_ptr(pointer Ptr,
        typename remove_reference<Del>::type&& Deleter);
    unique_ptr(unique_ptr&& Right);
    template <class T2, Class Del2>
    unique_ptr(unique_ptr<T2, Del2>&& Right);
    unique_ptr(const unique_ptr& Right) = delete;
    unique_ptr& operator=(const unique_ptr& Right) = delete;
};

//Specialization for arrays:  
template <class T, class D>
class unique_ptr<T[], D> {
public:
    typedef pointer;
    typedef T element_type;
    typedef D deleter_type;
    constexpr unique_ptr() noexcept;
    template <class U>
    explicit unique_ptr(U p) noexcept;
    template <class U>
    unique_ptr(U p, see below d) noexcept;
    template <class U>
    unique_ptr(U p, see below d) noexcept;
    unique_ptr(unique_ptr&& u) noexcept;
    constexpr unique_ptr(nullptr_t) noexcept : unique_ptr() { }     template <class U, class E>
        unique_ptr(unique_ptr<U, E>&& u) noexcept;
    ~unique_ptr();
    unique_ptr& operator=(unique_ptr&& u) noexcept;
    template <class U, class E>
    unique_ptr& operator=(unique_ptr<U, E>&& u) noexcept;
    unique_ptr& operator=(nullptr_t) noexcept;
    T& operator[](size_t i) const;


    pointer get() const noexcept;
    deleter_type& get_deleter() noexcept;
    const deleter_type& get_deleter() const noexcept;
    explicit operator bool() const noexcept;
    pointer release() noexcept;
    void reset(pointer p = pointer()) noexcept;
    void reset(nullptr_t = nullptr) noexcept;
    template <class U>
    void reset(U p) noexcept = delete;
    void swap(unique_ptr& u) noexcept;  // disable copy from lvalue unique_ptr(const unique_ptr&) = delete;  
    unique_ptr& operator=(const unique_ptr&) = delete;
};
```  
  
#### Parameters  
 `Right`  
 A `unique_ptr`.  
  
 `Nptr`  
 An `rvalue` of type `std::nullptr_t`.  
  
 `Ptr`  
 A `pointer`.  
  
 `Deleter`  
 A `deleter` function that is bound to a `unique_ptr`.  
  
## Exceptions  
 No exceptions are generated by `unique_ptr`.  
  
## Remarks  
 The `unique_ptr` class supersedes `auto_ptr`, and can be used as an element of C++ Standard Library containers.  
  
 Use the [make_unique](../standard-library/memory-functions.md#make_unique) helper function to efficiently create new instances of `unique_ptr`.  
  
 `unique_ptr` uniquely manages a resource. Each `unique_ptr` object stores a pointer to the object that it owns or stores a null pointer. A resource can be owned by no more than one `unique_ptr` object;  when a `unique_ptr` object that owns a particular resource is destroyed, the resource is freed. A `unique_ptr` object may be moved, but not copied;  for more information, see [Rvalue Reference Declarator: &&](../cpp/rvalue-reference-declarator-amp-amp.md).  
  
 The resource is freed by calling a stored `deleter` object of type `Del` that knows how resources are allocated for a particular `unique_ptr`. The default `deleter` `default_delete<T>` assumes that the resource pointed to by `ptr` is allocated with `new`, and that it can be freed by calling `delete _Ptr`. (A partial specialization `unique_ptr<T[]>`manages array objects allocated with `new[]`, and has the default `deleter` `default_delete<T[]>`, specialized to call delete[] `ptr`.)  
  
 The stored pointer to an owned resource, `stored_ptr` has type `pointer`. It is `Del::pointer` if defined, and `T *` if not. The stored `deleter` object `stored_deleter` occupies no space in the object if the `deleter` is stateless. Note that `Del` can be a reference type.  
  
## Members  
  
### Constructors  
  
|||  
|-|-|  
|[unique_ptr](#unique_ptr)|There are seven constructors for `unique_ptr`.|  
  
### Typedefs  
  
|||  
|-|-|  
|[deleter_type](#deleter_type)|A synonym for the template parameter `Del`.|  
|[element_type](#element_type)|A synonym for the template parameter `T`.|  
|[pointer](#pointer)|A synonym for `Del::pointer` if defined, otherwise `T *`.|  
  
### Member Functions  
  
|||  
|-|-|  
|[get](#get)|Returns `stored_ptr`.|  
|[get_deleter](#get_deleter)|Returns a reference to `stored_deleter`.|  
|[release](#release)|stores `pointer()` in `stored_ptr` and returns its previous contents.|  
|[reset](#reset)|Releases the currently owned resource and accepts a new resource.|  
|[swap](#swap)|Exchanges resource and `deleter` with the provided `unique_ptr`.|  
  
### Operators  
  
|||  
|-|-|  
|`operator bool`|The operator returns a value of a type that is convertible to `bool`. The result of the conversion to `bool` is `true` when `get() != pointer()`, otherwise `false`.|  
|`operator->`|The member function returns `stored_ptr`.|  
|`operator*`|The member function returns `*stored_ptr`.|  
|[unique_ptr operator=](#unique_ptr_operator_eq)|Assigns the value of a `unique_ptr` (or a `pointer-type`) to the current `unique_ptr`.|  
  
## Requirements  
 **Header:** \<memory>  
  
 **Namespace:** std  
  
##  <a name="deleter_type"></a>  deleter_type  
 The type is a synonym for the template parameter `Del`.  
  
```  
typedef Del deleter_type;  
```  
  
### Remarks  
 The type is a synonym for the template parameter `Del`.  
  
##  <a name="element_type"></a>  element_type  
 The type is a synonym for the template parameter `Type`.  
  
```  
typedef Type element_type;  
```  
  
### Remarks  
 The type is a synonym for the template parameter `Ty`.  
  
##  <a name="get"></a>  unique_ptr::get  
 Returns `stored_ptr`.  
  
```  
pointer get() const;
```  
  
### Remarks  
 The member function returns `stored_ptr`.  
  
##  <a name="get_deleter"></a>  unique_ptr::get_deleter  
 Returns a reference to `stored_deleter`.  
  
```  
Del& get_deleter();

const Del& get_deleter() const;
```  
  
### Remarks  
 The member function returns a reference to `stored_deleter`.  
  
##  <a name="unique_ptr_operator_eq"></a>  unique_ptr operator=  
 Assigns the address of the provided `unique_ptr` to the current one.  
  
```  
unique_ptr& operator=(unique_ptr&& right);
template <class U, Class Del2>  
unique_ptr& operator=(unique_ptr<Type, Del>&& right);
unique_ptr& operator=(pointer-type);
```  
  
### Parameters  
 A `unique_ptr` reference used to assign the value of to the current `unique_ptr`.  
  
### Remarks  
 The member functions call `reset(right.release())` and move `right.stored_deleter` to `stored_deleter`, then return `*this`.  
  
##  <a name="pointer"></a>  pointer  
 A synonym for `Del::pointer` if defined, otherwise `Type *`.  
  
```  
typedef T1 pointer;  
```  
  
### Remarks  
 The type is a synonym for `Del::pointer` if defined, otherwise `Type *`.  
  
##  <a name="release"></a>  unique_ptr::release  
 Releases ownership of the returned stored pointer to the caller and sets the stored pointer value to `nullptr`.  
  
```  
pointer release();
```  
  
### Remarks  
 Use `release` to take over ownership of the raw pointer stored by the `unique_ptr`. The caller is responsible for deletion of the returned pointer. The `unique-ptr` is set to the empty default-constructed state. You can assign another pointer of compatible type to the `unique_ptr` after the call to `release`.  
  
### Example  
  This example shows how the caller of release is responsible for the object returned:  
  
```cpp  
// stl_release_unique.cpp  
// Compile by using: cl /W4 /EHsc stl_release_unique.cpp  
#include <iostream>  
#include <memory>  
  
struct Sample {  
   int content_;  
   Sample(int content) : content_(content) {  
      std::cout << "Constructing Sample(" << content_ << ")" << std::endl;  
   }  
   ~Sample() {  
      std::cout << "Deleting Sample(" << content_ << ")" << std::endl;  
   }  
};  
  
void ReleaseUniquePointer() {  
   // Use make_unique function when possible.    
   auto up1 = std::make_unique<Sample>(3);  
   auto up2 = std::make_unique<Sample>(42);  
  
   // Take over ownership from the unique_ptr up2 by using release  
   auto ptr = up2.release();  
   if (up2) {  
      // This statement does not execute, because up2 is empty.  
      std::cout << "up2 is not empty." << std::endl;  
   }  
   // We are now responsible for deletion of ptr.  
   delete ptr;  
   // up1 deletes its stored pointer when it goes out of scope.     
}  
  
int main() {  
   ReleaseUniquePointer();  
}  
```  
  
  Computer output:  
  
```Output  
Constructing Sample(3)  
Constructing Sample(42)  
Deleting Sample(42)  
Deleting Sample(3)  
  
```  
  
##  <a name="reset"></a>  unique_ptr::reset  
 Takes ownership of the pointer parameter, and then deletes the original stored pointer. If the new pointer is the same as the original stored pointer, `reset` deletes the pointer and sets the stored pointer to `nullptr`.  
  
```  
void reset(pointer ptr = pointer());
void reset(nullptr_t ptr);
```  
  
### Parameters  
  
|Parameter|Description|  
|---------------|-----------------|  
|`ptr`|A pointer to the resource to take ownership of.|  
  
### Remarks  
 Use `reset` to change the stored [pointer](#pointer) owned by the `unique_ptr` to `ptr` and then delete the original stored pointer. If the `unique_ptr` was not empty, `reset` invokes the deleter function returned by [get_deleter](#get_deleter) on the original stored pointer.  
  
 Because `reset` first stores the new pointer `ptr`, and then deletes the original stored pointer, it's possible for `reset` to immediately delete `ptr` if it is the same as the original stored pointer.  
  
##  <a name="swap"></a>  unique_ptr::swap  
 Exchanges pointers between two `unique_ptr` objects.  
  
```  
void swap(unique_ptr& right);
```  
  
### Parameters  
 `right`  
 A `unique_ptr` used to swap pointers.  
  
### Remarks  
 The member function swaps `stored_ptr` with `right.stored_ptr` and `stored_deleter` with `right.stored_deleter`.  
  
##  <a name="unique_ptr"></a>  unique_ptr::unique_ptr  
 There are seven constructors for `unique_ptr`.  
  
```  
unique_ptr();

unique_ptr(nullptr_t);
explicit unique_ptr(pointer ptr);

unique_ptr(
    Type* ptr,  
    typename conditional<
    is_reference<Del>::value, 
    Del, 
    typename add_reference<const Del>::type>::type _Deleter);

unique_ptr(pointer ptr, typename remove_reference<Del>::type&& _Deleter);
unique_ptr(unique_ptr&& right);
template <class Ty2, Class Del2>  
unique_ptr(unique_ptr<Ty2, Del2>&& right);
```  
  
### Parameters  
  
|Parameter|Description|  
|---------------|-----------------|  
|`ptr`|A pointer to the resource to be assigned to a `unique_ptr.`|  
|`_Deleter`|A `deleter` to be assigned to a `unique_ptr`.|  
|`right`|An `rvalue reference` to a `unique_ptr` from which `unique_ptr` fields are move assigned to the newly constructed `unique_ptr`.|  
  
### Remarks  
 The first two constructors construct an object that manages no resource. The third constructor stores `ptr` in `stored_ptr`. The fourth constructor stores `ptr` in `stored_ptr` and `deleter` in `stored_deleter`.  
  
 The fifth constructor stores `ptr` in `stored_ptr` and moves `deleter` into `stored_deleter`. The sixth and seventh constructors store `right.reset()` in `stored_ptr` and moves `right.get_deleter()` into `stored_deleter`.  
  
##  <a name="dtorunique_ptr"></a>  unique_ptr ~unique_ptr  
 The destructor for `unique_ptr`, destroys a `unique_ptr` object.  
  
```  
~unique_ptr();
```  
  
### Remarks  
 The destructor calls `get_deleter()(stored_ptr)`.  
  
## See Also  
 [\<memory>](../standard-library/memory.md)

