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



