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

gcc 提供了一种 $O(n\sqrt n)$ 的数据结构（内部用块状链表实现），可以当作可持久化平衡树。

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
// pos 也可以是迭代器
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
7. push/pop
```
void pop_back()
void pop_front()
void push_back(T c)
void push_front(T c)
```
特别的，`crope`（即`rope<char>`）类型可以用 `cout`，可以 `+=`

### 3. 迭代器

有如下几种，其中 `const` 表示值不可修改，`reverse` 表示反向迭代器
```
const_iterator begin()
const_iterator end()
const_reverse_iterator rbegin()
const_reverse_iterator rend()
iterator mutable_begin()
iterator mutable_end()
reverse_iterator mutable_rbegin()
reverse_iterator mutable_rend()
```

### 4. 可持久化


可以用以下方法实现 $\textrm O(1)$ 可持久化
```
rope<T> *rp[p];
rp[0] = new rope<T>();			// 创建一个新的版本
rp[cnt] = new rope<T>(rp[cnt-1])	// 继承上一个版本
ans = rp[cnt]->at(pos)			// 访问某个版本
```
这样我们就得到了一个可持久化线段树

## 二、可并堆 priority_queue

### 1.定义

pb_ds 库包含了配对堆（pairing_heap）、二叉堆（binary_heap）、二项堆（binomial_heap）、冗余计数二项堆（redundant-counter binomial_heap）、经改良的斐波那契堆（thin_heap）。各项操作复杂度在 [这里](https://gcc.gnu.org/onlinedocs/libstdc++/ext/pb_ds/pq_performance_tests.html) 都能看到，虽然可以省下手写堆的时间，但是这个库的效率时间和空间都不如手写左偏树，如果用对 tag 也可以过题，不过要谨慎使用

pb_ds 库中的 priority_queue 与 STL 的接口非常相似，需要引入头文件`<ext/pb_ds/priority_queue.hpp>`，可以用如下方式定义

```
priority_queue<int, less<int>, pairing_heap_tag> pq;
```
`pairing_heap_tag` 是指定堆类型的标签，也可以使用 `binary_heap_tag`，`binomial_heap_tag`，`rc_binomial_heap_tag`，`thin_heap_tag`

### 2. 操作

一些比较基本的操作：
```cpp
q.size() 	// q 的大小
q.empty() 	// 是否为空
q.push(x) 	// 插入一个 x
q.top() 	// 堆顶元素
q.modify(it, x) 	// 修改 it 位置的值
q.pop() 	// 弹出堆顶
q.erase(it) 	// 删除一个元素
q.erase_if(pred) 	// 删除所有满足 pred 的元素，返回删除的元素个数
q.clear() 	// 清空
```

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

### 3. 合并与分裂

pb_ds 的优先队列还支持合并和分裂操作，可以代替左偏树使用，方法是：
```
__gnu_pbds::priority_queue<int> p, q;
  ...
p.join(q);
```
其中 `pairing_heap` 合并的复杂度是 $\textrm O(\log n)$ 的，此时优先队列 `q` 内所有元素就被合并进优先队列 `p` 中，且优先队列 `q` 被清空。

`split` 函数比较复杂，原型如下：
```cpp
template<class Pred>
inline void split(Pred prd, priority_queue &other)
```
`split` 操作需要传入两个参数，第一个参数是分裂的条件，只有满足 `prd` 的元素才会被分裂到 `other` 中，分裂时需要保证 `other` 的 `tag` 与当前的相同。

### 4.复杂度

各个操作的复杂度如下：

|/|push|pop|modify|erase|join|
|---|---|---|---|---|---|
| `std::priority_queue` | $\textrm O(n)$ worst, $\textrm O(\log n)$ amortized | $\textrm O(\log n)$ | $\textrm O(n\log n)$ | $\textrm O(n\log n)$ | $\textrm O(n\log n)$ |
| `pairing_heap_tag` | $\textrm O(1)$ | $\textrm O(n)$ worst, $\textrm O(\log n)$ amortized | $\textrm O(n)$ worst, $\textrm O(\log n)$ amortized | $\textrm O(n)$ worst, $\textrm O(\log n)$ amortized | $\textrm O(1)$ |
| `binary_heap_tag` | $\textrm O(n)$ worst, $\textrm O(\log n)$ amortized | $\textrm O(n)$ worst, $\textrm O(\log n)$ amortized | $\textrm O(n)$ | $\textrm O(n)$ | $\textrm O(n)$ |
| `binomial_heap_tag` | $\textrm O(\log n)$ worst, $\textrm O(1)$ amortized | $\textrm O(\log n)$ | $\textrm O(\log n)$ | $\textrm O(\log n)$ | $\textrm O(\log n)$ |
| `rc_binomial_heap_tag` | $\textrm O(1)$ | $\textrm O(\log n)$ | $\textrm O(\log n)$ | $\textrm O(\log n)$ | $\textrm O(\log n)$ |
| `thin_heap_tag` | $\textrm O(1)$ | $\textrm O(n)$ worst, $\textrm O(\log n)$ amortized | $\textrm O(\log n)$ worst, $\textrm O(1)$ amortized | $\textrm O(n)$ worst, $\textrm O(\log n)$ amortized | $\textrm O(n)$ |

## 三、Associative Container

关联容器中元素是按关键字保存和访问的，支持高效关键字查找和访问。顺序容器中元素是按他们在容器中的位置保存访问的

关联容器有两个主要类型：`set` 和 `map`。

+ set：每个元素包含一个关键字，可以查询一个值是否存在。
+ map：关键字-值对，也被称为关联数组，通过关键字而不是位置查找。

`pb_ds` 中提供了多种关联容器，他们的继承关系如下图所示：

![](https://cdn.luogu.com.cn/upload/image_hosting/ke8khcu0.png)

所有的数据结构都继承自 `container_base`，也因此他们都具有类似的接口。`pb_ds` 中常用的关联容器有哈希表，平衡树和字典树等。

## 四、哈希表 hash_table

`pb_ds` 提供了两种哈希表
+ `cc_hash_table` 使用拉链法
+ `gp_hash_table` 使用探查法，个人测试这个更快一些

`hash_table` 的大部分操作与 `std::unordered_map` 类似，定义时一般只需用到前四个模板参数，前两个分别表示键和值的类型。如果想作为 `set` 使用，就在第二个参数填 `null_type`（较低版本的 g++ 写为 `null_mapped_type`）

第三个参数表示哈希函数，一般可以省略，默认值为 `__gnu_cxx::hash<Key>`，第四个参数是判断是否相等的函数，同样也可以省略。

```cpp
cc_hash_table<int, string> ht;
```

一个小例子：
```cpp
cc_hash_table<int, char> c;
c[2] = 'b';
assert(c.find(1) == c.end());
```
`hash_table` 会自动去重，如果不希望去重的话可以将 `Key` 的类型改为 `pair<T,int>` 类型，在插入时取一个随机数，如 `ht.insert(make_pair(x, rand()))`，这个技巧在 `pb_ds` 平衡树中也可以用到。

## 五、平衡树 tree

### 1. 声明

需要
```cpp
#include<ext/pb_ds/assoc_container.hpp>
#include<ext/pb_ds/tree_policy.hpp>
using namespace __gnu_pbds;
```
定义一个树：`tree<int, null_type, less<int>, rb_tree_tag, null_tree_node_update> tr;`

前两个参数分别是键和值的类型，第三个参数是仿函数 `cmp_fn` 表示比较规则。

`rb_tree_tag` 表示使用红黑树，也可以改为 `splay_tree_tag` 或 `ov_tree_tag`，其中红黑树的运行效率比较好。

`null_tree_node_update` 表示不更新任何节点信息，如果要使用 `find_by_order()` 等函数，请改为 `tree_order_statistics_node_update`。

### 2. 操作

以下 `x` 小于 `y` 均表示 `cmp_fn(x, y)` 成立

1. 插入与删除
```cpp
tr.insert(x)  // 插入 x
tr.erase(x)  // 删除迭代器 x 位置的元素
tr.erase_if(prd) // 删除满足 prd 的元素
```
2. 全局操作
```cpp
tr.size()  // tr 的大小
tr.empty()  // tr 是否为空
tr.clear()  // 清空 tr
```
3. 查找
```cpp
tr.find(x)  // 返回 x 的迭代器，没找到返回 tr.end()
tr.lower_bound(x)  // 第一个大于等于 x 的数，返回迭代器
tr.upper_bound(x)  // 第一个大于 x 的数，返回迭代器
```
4. 排名相关
```cpp
// 需要 tree_order_statistics_node_update
tr.find_by_order(x)  // 第 x 个数的迭代器，从 0 开始编号，没找到返回 tr.end()
tr.order_of_key(x)  // 返回小于 x 数的个数+1，x 不在树中也能查
```

### 3. 分裂与合并

```cpp
tr.split(r_key, other)
```
将 `tr` 分裂成两棵树，第一棵树包含不超过 `r_key` 的元素，`other` 中包含大于 `r_key` 的元素。两棵树的 `tag` 必须一致。

```cpp
tr.join(other)
```
将 `other` 合并到 `tr` 中，然后清空 `other`。需要保证 `other` 中的数全部大于或全部小于 `tr` 中的数（即取值范围不相交），并且两棵树的 `tag` 相同。