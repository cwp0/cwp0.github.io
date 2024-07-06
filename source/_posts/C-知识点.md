---
title: C++知识点
tags:
  - C++
  - 面经
categories:
  - 面经
keywords:
  - 面经
description: C++知识点
abbrlink: 17775
date: 2024-06-27 16:31:43
updated: 2024-06-28 00:12:15
top_img: https://s2.loli.net/2024/06/27/l3jdeWMqxY9Hf5h.png
comments:
cover: https://s2.loli.net/2024/06/27/xvtGedjogpz15iE.png
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
abcjs:
---


### C++常用数据结构
- 数组(Array)：数组是固定大小的连续内存块，用于存储相同类型的数据。
- 动态数组(Vector)：`std::vector` 是一个动态数组，可以根据需要自动调整大小。
- 栈(Stack)：栈是一种后进先出(LIFO)的数据结构。C++ 中可以使用 `std::stack` 实现栈。
- 队列(Queue)：队列是一种先进先出(FIFO)的数据结构。C++ 中可以使用 `std::queue` 实现队列。
- 双端队列(Deque)：双端队列允许在两端进行插入和删除操作。C++ 中可以使用 `std::deque` 实现双端队列。
- 集合(Set)：集合是一种无序且不允许重复元素的数据结构。C++ 中可以使用 `std::set` 实现集合。
- 映射(Map)：映射是一种键值对的数据结构。C++ 中可以使用 `std::map` 实现映射。
- 优先队列(Priority Queue)：优先队列是一种元素带有优先级的队列，C++ 中可以使用 `std::priority_queue` 实现优先队列。
- 哈希表(Unordered Map)：哈希表是一种基于哈希函数的数据结构，用于高效地进行查找、插入和删除操作。C++ 中可以使用 `std::unordered_map` 实现哈希表。

### lambda表达式
C++中的lambda表达式是一种轻量级的匿名函数，允许在需要时内联定义函数，通常用于一次性操作或局部范围内的操作。它们可以捕获周围作用域的变量，并且可以作为参数传递给算法或其他函数。以下是基本用法示例：
```c++
auto multiply = [](int a, int b) -> int {
    return a * b;
};
std::cout << multiply(3, 4) << std::endl; // 输出 12
```

### new/malloc的区别
在 C++ 中，`new` 和 `malloc` 都可以用于动态内存分配，但它们有一些关键的区别：
1. **类型安全**:
   - `new` 是类型安全的，它会根据指定的类型自动进行类型转换。
   - `malloc` 返回的是 `void*`，需要手动进行类型转换。
   ```cpp
   int* p = new int;       // 自动类型转换
   int* q = (int*)malloc(sizeof(int));  // 需要手动类型转换
   ```
2. **构造函数和析构函数**:
   - `new` 会调用对象的构造函数来初始化对象。
   - `delete` 会调用对象的析构函数来清理对象。
   - `malloc` 只分配内存，不会调用构造函数。
   - `free` 只释放内存，不会调用析构函数。
   ```cpp
   class MyClass {
   public:
       MyClass() { /* 构造函数 */ }
       ~MyClass() { /* 析构函数 */ }
   };

   MyClass* obj1 = new MyClass();  // 调用构造函数
   delete obj1;                    // 调用析构函数

   MyClass* obj2 = (MyClass*)malloc(sizeof(MyClass)); // 不调用构造函数
   free(obj2);                                       // 不调用析构函数
   ```
3. **操作符和函数**:
   - `new` 和 `delete` 是运算符。
   - `malloc` 和 `free` 是库函数，定义在 `<cstdlib>` 头文件中。
4. **内存分配失败时的行为**:
   - `new` 在内存分配失败时会抛出 `std::bad_alloc` 异常。
   - `malloc` 在内存分配失败时返回 `NULL`。
   ```cpp
   try {
       int* p = new int[1000000000]; // 如果分配失败会抛出异常
   } catch (std::bad_alloc& e) {
       std::cerr << "Memory allocation failed: " << e.what() << std::endl;
   }

   int* q = (int*)malloc(1000000000 * sizeof(int));
   if (q == NULL) {
       std::cerr << "Memory allocation failed" << std::endl;
   }
   ```
5. **使用场景**:
   - `new` 和 `delete` 更适合用于 C++ 中的面向对象编程，因为它们支持对象的构造和析构。
   - `malloc` 和 `free` 更适合于需要兼容 C 语言代码的场合，或者在特定情况下需要精细控制内存分配。

总结来说，`new` 和 `malloc` 各有其适用的场景，选择使用哪一个取决于具体的需求和代码风格。在 C++ 中，通常建议使用 `new` 和 `delete`，因为它们更符合 C++ 的编程范式，支持对象的生命周期管理。

### malloc的实现原理
`malloc` 是 C 语言中的一个函数，用于在堆内存中动态分配指定大小的内存块。它的全称是 "memory allocation"。`malloc` 的实现涉及操作系统和运行时库的协同工作。下面是 `malloc` 实现的一些基本概念和步骤：
1. **初始化内存池**：当程序启动时，运行时库会从操作系统请求一块内存作为堆的初始大小。这块内存通常使用 `brk` 或 `sbrk` 系统调用来分配。
2. **内存分配**：
    - 当调用 `malloc` 时，它会根据请求的大小在堆中查找合适的空闲块。
    - 这通常使用自由链表(free list)来管理空闲块。自由链表是一种数据结构，存储了所有空闲内存块的信息。
    - 如果找到合适的块，`malloc` 会从自由链表中移除这个块并返回给调用者。如果块太大，可能会拆分块并将剩余部分放回自由链表。
3. **请求更多内存**：
    - 如果当前堆内存不够满足分配请求，`malloc` 会向操作系统请求更多的内存。通常是通过 `sbrk` 或 `mmap` 系统调用。
    - 请求到的内存会被加入到自由链表中，然后重新进行分配。
4. **合并空闲块**：
    - 当内存块被释放(使用 `free` 函数)时，它会被放回自由链表。
    - 为了避免内存碎片化，释放时会尝试将相邻的空闲块合并成一个更大的块。

### C++虚函数
https://blog.csdn.net/qq_42048450/article/details/117282640
在实现c++多态时会用到虚函数。虚函数使用的其核心目的是通过基类访问派生类定义的函数。所谓虚函数就是在基类定义一个未实现的函数名，为了提高程序的可读性，建议后代中虚函数都加上virtual关键字。

```c++
class base
{
   public:
      base();
      virtual void test(); //定义的一个虚函数
   private:
      har *basePStr;
};
```
上述代码在基类中定义了一个test的虚函数，所有可以在其子类重新定义父类的做法这种行为成为覆盖(override),或者为重写。

常见用法：声明基类指针，利用指针指向任意一个子类对象，调用相关的虚函数，动态绑定，由于编写代码时不能确定被调用的是基类函数还是那个派生类函数，所以被称为“”虚“”函数。如果没有使用虚函数的话，即没有利用C++多态性，则利用基类指针调用相应的函数的时候，将总被限制在基类函数本身，而无法调用到子类中被重写过的函数。



