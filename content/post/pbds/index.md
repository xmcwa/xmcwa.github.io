---
title: C++ pb_ds 数据结构
description: 
slug: pbds
date: 2023-05-18
image: cover.jpg
categories:
    - programming
tags:
    - STL
---

## 一、可持久化平衡树 rope

gcc 提供了一种 $O(n\sqrt n)$ 的数据结构（内部用块状链表实现），可以当作可持久化平衡树

### 1. 声明

头文件：`#include<ext/rope>`

命名空间：`using namespace __gnu_cxx`

声明变量
```
rope</*类型*/> rp;
rope<int> irp;
rope<long long> lrp;
rope<char> crp;
crope crp2;  // crope 相当于 rope<char>
  ...
```

### 2. 操作

简略版：
```
push_back(x);		// 在末尾添加x
insert(pos,x);		// 在pos插入x
erase(pos,x);		// 从pos开始删除x个
replace(pos,x);		// 从pos开始换成x
substr(pos,x);		// 提取pos开始x个
at(x); rp[x];		// 访问第x个元素（两种形式均可）
clear();		// 清空元素
```

详细版：

*以下所有的 T 都表示 rope 中存的类型，且这里只列举了几个最常用的函数*

1. insert 操作
```
void insert(size_t pos, const rope& r)	// 在 pos 位置插入另一个 rope
void insert(size_t pos, T *s, size_t n)	// 将数组 s 的前n位插入 rope 的下标 pos 处，如没有参数 n 则将数组 s 插入 rope 的下标 pos 处，直到遇到值为 0 的位置
void insert(size_t pos, size_t n, T c)	// 在 pos 位置插入 n 个 c
void insert(size_t pos, T c)		// 在 pos 插入 c
```
2. append 操作（和 insert 类似）
```
rope& append(const rope& r)	//在末尾插入另一个 rope 的所有元素
rope& append(char *s, size_t n)	// 将数组 s 的前 n 个元素插入到末尾
rope& append(size_t n, T c)	// 在末尾插入 n 个 c
rope& append(T c)		// 在末尾插入 c
```
3. erase
```
void erase(size_t pos, size_t n)	// 从 pos 位置开始连续删除 n 个元素，没有参数 n 默认删 1 个
void erase(iter start, iter end)	// 删除 [start, end)，没有参数 end 默认删 1 个
```
4. replace
```
// 若不传 n 则默认是 1
void replace(size_t pos, size_t n, T c)	// 将 pos 位置开始的 n 个元素替换成 c
void replace(size_t pos, size_t n, T *c)// 将 pos 位置开始的 n 个元素替换成 c 数组的前 n 个元素
```
5. substr
```
rope substr(size_t pos, size_t n)	// 提取 pos 开始长度为 n 的一段
rope substr(iter start, iter end)	// 提取 [start, end) 这一段
```
6. at
```
T at(size_t pos)	// 相当于 rp[pos]，不过不能修改
```
特别的，`crope`（即`rope<char>`）类型可以用 `cout`，可以 `+=`

### 3. 可持久化


可以用以下方法实现 $O(1)$ 可持久化
```
rope<T> *rp[p];
rp[0] = new rope<T>();			// 创建一个新的版本
rp[cnt] = new rope<T>(rp[cnt-1])	// 继承上一个版本
ans = rp[cnt]->at(pos)			// 访问某个版本
```
这样我们就得到了一个可持久化线段树

## 二、可并堆 priority_queue

pb_ds 库包含了配对堆（pairing_heap）、二叉堆（binary_heap）、二项堆（binomial_heap）、冗余计数二项堆（redundant-counter binomial_heap）、经改良的斐波那契堆（thin_heap）。各项操作复杂度在 [这里](https://gcc.gnu.org/onlinedocs/libstdc++/ext/pb_ds/pq_performance_tests.html) 都能看到，虽然可以省下手写堆的时间，但是这个库的效率时间和空间都不如手写左偏树，如果用对 tag 也可以过题，不过要谨慎使用

pb_ds 库中的 priority_queue 与 STL 的接口非常相似，需要引入头文件`<ext/pb_ds/priority_queue.hpp>`，可以用如下方式定义

```
__gnu_pbds::priority_queue<int, less<int>, pairing_heap_tag> pq;
```
`pairing_heap_tag` 是指定堆类型的标签，也可以使用 `binary_heap_tag`，`binomial_heap_tag`，`rc_binomial_heap_tag`，`thin_heap_tag`

`priority_queue` 也支持迭代器，声明方法是：
```
__gnu_pbds::priority_queue<T>::point_iterator it;
```
`push` 方法会返回一个迭代器（与 STL 不同），这样你就可以访问内部的元素了，例如：
```
__gnu_pbds::priority_queue<int> p;
__gnu_pbds::priority_queue<int>::point_iterator it = p.push(1);
p.modify(it, 3);	// 修改一个元素
cout << p.top() << endl;
p.pop();
```
pb_ds 的优先队列还支持合并和分裂操作，可以代替左偏树使用，方法是：
```
__gnu_pbds::priority_queue<int> p, q;
  ...
p.join(q)
```

> Photo by [Pawel Czerwinski](https://unsplash.com/@pawel_czerwinski) on [Unsplash](https://unsplash.com/)
