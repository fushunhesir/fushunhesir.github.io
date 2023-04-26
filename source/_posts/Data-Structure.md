---
title: Data Structure
date: 2023-04-02 13:11:54
tags:
---

# 数据结构

## 绪论

### 数据结构基本概念

* **基本概念和术语**
  * **数据**：不是简单指数字，而是代指一切能够输入计算机内部的符号，并且该符号承载了信息以及事物的属性
  * **数据元素**：是一个整体结构，内部包含**数据项**，数据项是组成数据元素的最小单位。比如：学生（学号、姓名、班级）
  * **数据对象**：具有相同性质的数据元素的**集合**，如：*数据库中元组和关系，元组是数据元素，关系是数据对象*
  * **数据类型**
    * **原子类型**：不可再分的类型，如：基本数据类型int， double
    * **结构类型**：可再分的类型，如：struct
    * **抽象数据类型**：既有数据集合，又有对该集合操作的类型，**如**：类
  * **数据结构**
    * **本质**：==数据元素的集合==。集合内的数据元素之间存在特定的关系，**如**：树，图，链表等。**简单理解，就是以什么形式(结构)组织数据元素，以适用于特定的应用场景**
    * **三要素**：**逻辑结构**，物理结构(**存储结构**)，对数据元素的操作(**数据运算**)
* **数据结构三要素**
  * **逻辑结构**：独立于物理存储和计算机的结构，是人为设计的抽象结构。主要分为**线性结构，非线性结构**
  * **存储结构**
    * **顺序存储**：物理上连续的存储，**优点**：占用空间小，可以随机存取     **缺点**：产生外部碎片
    * **链式存储**：物理上不一定连续的存储，**优点**：充分利用存储，不会产生碎片，**缺点**：指针占用额外空间，且只能顺序存取
    * **索引存储**：创建索引表辅助。**优点**：检索速度快，**缺点**：索引表占用额外空间，插入和删除需要修改索引表，增加额外开销
    * **哈希存储**：通过散列函数计算存储地址。**优点**：所有操作都很快。**缺点**：可能出现冲突，解决冲突会增加时间开销

### 复杂度度量

*度量算法好坏的尺子*

* **时间复杂度**：随着输入规模的变化，**最坏情况下**的执行时间的**变化趋势**
  * **渐近复杂度**：关注大规模输入，重视总体变化趋势和增长速度
    * **渐近上界($\Omicron$)**：正的常系数可视为1，低次项可省略。**保守估计**
    * **渐近下界($\Omega$)**：代表执行时间的下界。**乐观估计**
    * **准确确界($\Theta$)**：上界函数等于下界函数，代表对算法执行时间的准确估计。
* **复杂度分析**
  * **常数时间复杂度**
  * **对数时间复杂度**
  * **线性时间复杂度**
  * **多项式时间复杂度**
  * **指数时间复杂度**
    * *具有指数时间复杂度的算法一般没有实际意义*
    * **难解问题**：没有多项式复杂度的问题
* **输入规模**：严格定义为**用以描述输入所需的空间规模**

### 递归

*分支转向是算法的灵魂*

* **递归**：函数(过程)的自我调用。简洁准确地描述问题

* **递归基**：平凡情况。递归基能够保证递归算法的有穷性。

* **线性递归**
  * **简介**：*算法向深层递归调用，每一次至多调用自身一次，从而形成线性次序。*
  * **特征**：线性递归问题，总可以分解为两个子问题，一个是独立的某个元素，另一个是和原问题结构一致的子问题。经过合并之后得到问题解。**这类问题每次将问题规模缩减一个常数，最终抵达递归基，也称为减而治之**
  
* **递归分析**
  * **递推方程**：写出递推公式，确定平凡模式条件，结合数学归纳得出递归复杂度
  * **递归跟踪**：以图的形式刻画递归过程，从而分析递归复杂度
  
* **递归模式**
  * **多递归基**
  * **多向递归**
  
* **二分递归**
  
  * **分而治之**
  
    *将问题分解为若干规模更小的子问题*
  
    * **难点**：需要对问题重新表述，保证子问题与原问题接口的形式一致
    * **注意**：对算法的渐近复杂度几乎无影响
  
  * **分而治之适用条件**
  
    * 子问题划分和子问题合并这两个计算必须能够高效实现
    * 各个子问题独立求解，并不相互依赖
  
  * **优化策略**
  
    * 对于子问题有重合的问题，可以选择使用制表或者动态规划来进行优化
  
* **抽象数据类型(ADT)**
  
  * 外部特性与内部实现分离，提供一致且标准的对外接口
  

## 向量

*物理存储顺序与逻辑顺序相同的数据结构*

### 从数组到向量

* **向量**：根据面向对象思想，对线性数组的抽象与泛化。其中的数据元素不再只是基本数据类型，也不保证每个元素都有某个数值

### 接口

* **操作接口**

  ![image-20230417130608296](https://cdn.jsdelivr.net/gh/fushunhesir/blog-images@main/imgs/DS_vector_interface.png)

### 构造与析构

*指导计算机创建和回收对象*

* **构造函数**：告诉计算机如何配置一个对象的内存
* **析构函数**：告诉计算机如何回收一个对象的内存

### 动态空间管理

*高效利用空间*

* **静态空间管理**：在向量的生命周期内无法改变容量
* **动态空间管理**：在向量的生命周期内容量可以动态变化
  * **可扩充向量**：一种动态空间管理方法。申请新的更大的内存，然后复制数据，回收原来的内存
  * **基本问题**
    * **新容量取多少**：2倍原内存
* **分摊分析**
  * 将某些时间复杂度不确定的操作，将其所消耗的时间分摊到所有操作上

### 常规向量

#### 直接引用元素

```c++
template <typename T>
T& Vector<T>::operator[](rank i) {return _elem[i];}
```

#### 置乱算法

```c++
template <typename T>
T& Vector<T>::permute(rank lo, rank hi){
	T* base = _elem + lo;
  for(int i = hi - lo; i > 0; i--){
		swap(base[i-1], base[rand() % i]);
  }
}
```

#### 顺序查找

```c++
template <typename T>
rank Vector<T>::find(const T& e, rank lo, rank hi) const{
  while(lo < hi-- && e != _elem[hi]);
  return hi < lo ? -1 : hi;
}
```

#### 插入

```C++
template <typename T>
rank Vector<T>::insert(const T& e, rank r){
	expand(); // 必要时扩容
	for(int i = _size; i > r; i--) _elem[i] = _elem[i-1];
	_elem[r] = e;
	_size++;
	return r;
}
```

**时间复杂度**：$O(n)$

#### 删除

```c++
template <typename T>
rank Vector<T>::remove(rank lo, rank hi){
  if(lo == hi) return 0;
  while(hi < _size) _elem[lo++] = _elem[hi++];
  _size = lo;
  shrink();
  return hi - lo;
}
```

**时间复杂度**：$O(n)$

#### 唯一化

```C++
template <typename T>
int Vector<T>::deduplicate(rank lo, rank hi) {
  int old_size = _size;
  if(lo == hi) return 0;
  for(int i = 1; i < hi;){
    find(_elem[i], lo, i) < 0 ?
      i++ : remove(i);
  }
  return old_size - _size;
}
```

### 有序向量

#### 唯一化

```C++
template <typename T>
int Vector<T>::deduplicate(){
  int i = 0, j = 0;
  while(++j < _size)
    if(_elem[j] != _elem[i])
      _elem[++i] = _elem[j];
  _size = i;
  return j - i;
}
```

**时间复杂度**：$O(n)$

#### 查找

* **二分查找**

  * **版本A**

    ```c++
    template <typename T>
    rank Vector<T>::search(const T& e, rank lo, rank hi) const{
    	while(lo < hi) {
        rank mid = (lo + hi) >> 1;
    	  if(_elem[mid] == e) return mid;
      	else if(_elem[mid] < e) lo = mid + 1;
      	else hi = mid;
      }
      return -1;
    }
    ```

  * **版本B**

    ```C++
    template <typename T>
    rank Vector<T>::search(const T& e, rank lo, rank hi) const{
        while(1 < hi - lo) {
            rank mid = (lo + hi) >> 1;
            if(e < _elem[mid]) hi = mid;
            else lo = mid;
        }
        return _elem[lo] == e ? lo : -1;
    }
    ```

  * **版本C**

    ```C++
    template <typename T>
    rank Vector<T>::Search(const T& e, rank lo, rank hi){
    	while(lo < hi) {
            rank mid = (hi + lo) >> 1;
            if(e < _elem[mid]) hi = mid : lo = mid + 1;
        }
        return --lo;
    }
    ```

* **斐波那契查找**

  ```C++
  template <typename T>
  rank Vector<T>::fib_search(const T& e, rank lo, rank hi) {
    Fib fib(hi - lo);
    while(lo < hi){
      while(fib.get() > hi - lo) mid = fib.pre();
      if(e < _elem[mid]) hi = mid;
      else if(e > _elem[mid]) lo = mid + 1;
      else return mid;
    }
    return -1;
  }
  ```

  

### 排序与下界

#### 排序及其分类

* **数据处理规模或存储特点**：外部排序和内部排序
* **数据的输入形式**：离线和在线
* **随机策略**：确定式和随机式

#### 比较树

* **定义**：根绝算法的比较与比对流程将算法抽象为一棵比较树
* **作用**：估计**基于比较式算法**的复杂度下界
* **方法**：根据算法**可能的输出结果数量**，反推比较树的**最大高度**，从而估计算法的复杂度下界

### 排序器

* **稳定性**：排序完成后，值相同的元素在向量中的相对位置不发生改变

* **冒泡排序**

  ```c++
  template <typename T>
  rank Vector<T>::bubble_sort(rank lo, rank hi){
    while(!bubble(lo, hi--));
  }
  
  template <typename T>
  rank Vector<T>::bubble(rank lo, rank hi){
    bool sorted = true;
    while(++lo < hi){
      if(_elem[lo - 1] > _elem[i]){
        sorted = false;
        swap(_elem[i], _elem[i-1]);
      }
    }
    return sorted;
  }
  ```

  * *冒泡排序算法具有稳定性*

* **归并排序**

  ```c++
  template <typename T>
  void Vector<T>::merge_sort(rank lo, rank hi) {
    if(hi - lo < 2) return ;
    mid = (lo + hi) >> 1;
    merge_sort(lo, mid);
    merge_sort(mid, hi);
    merge(lo, mid, hi);
  }
  
  template <typename T>
  void Vector<T>::merge(rank lo, rank mid, rank hi) {
  	T* A = _elem + lo;
    int lb = mid - lo;
    T* B = new T[lb];
    for(int i = 0; i < hi - lo; i++) B[i] = A[i];
    int lc = hi - mid;
    T* C = _elem + mid;
    for(int i = 0, j = 0, k = 0; (i < lb || j < lc);){
      if(i < lb && (!(j < lc) || B[i] <= C[j])) A[k++] = B[i++];
      else if(j < lc && (!(i < lb) || C[j] < B[i])) A[k++] = C[j++];
    }
    delete[] B;
  }
  ```
  
  * *第一个能够在最坏情况下保证$O(n\log n)$复杂度的确定性算法*
  * *时间复杂度*：$O(n\log n)$，*即使在链表上也能保证这个时间复杂度*
  * *其中`merge()`时间复杂度为$O(n)$*

## 列表

*熟知的代表是链表，逻辑顺序和物理顺序不一定一致的数据结构*

### 无序列表

* **头尾节点**

  * 作为哨兵存在，对外不可见。头节点指向首节点，尾节点的前驱指向末节点。

    <img src="https://cdn.jsdelivr.net/gh/fushunhesir/blog-images@main/imgs/%E5%88%97%E8%A1%A8%E7%BB%93%E6%9E%84.png" alt="image-20230425125649923" style="zoom:50%;" />


* **默认构造方法**

  ```c++
  template <typename T>
  void List<T>::init(){
    header = new ListNode<T>;
    tail = new ListNode<T>;
    header->succ = tail;
    header->pred = nullptr;
    tail->pred = header;
    tail->succ = nullptr;
    _size = 0;
  }
  ```

* **位置代替秩**

  ```c++
  #define ListNodePos(T) ListNode<T>*
  
  template <typename T>
  T& List<T>::operator[](rank r) const {
    ListNodePos(T) p = first();
    while(r--) p = p->succ;
    return p->data;
  }
  ```

* **查找**

  ```c++
  #define ListNodePos(T) ListNode<T>*
  
  template <typename T>
  ListNodePos(T) List<T>::find(T const&e, int n, ListNodePos(T) p) const {
    while(n--)
      if(e == (p = p->pred)->data) return p;
    return nullptr;
  }
  ```

* **插入**

  ```c++
  #define ListNodePos(T) ListNode<T>*
  
  template <typename T>
  ListNodePos(T) List<T>::insert_as_first(T const&e){
    _size++;
    return header->insert_as_succ(e);
  }
  
  template <typename T>
  ListNodePos(T) List<T>::insert_as_Last(T const&e){
    _size++;
    return tail->insert_as_pred(e);
  }
  
  template <typename T>
  ListNodePos(T) List<T>::insert_before(T const&e, ListNodePos(T) p){
    _size++;
    return p->insert_as_pred(e);
  }
  
  template <typename T>
  ListNodePos(T) List<T>::insert_after(T const&e, ListNodePos(T) p){
    _size++;
    return p->insert_as_succ(e);
  }
  ```

  * **其中辅助函数为节点本身提供的插入函数**

    ```c++
    #define ListNodePos(T) ListNode<T>*
    
    template <typename T>
    ListNodePos(T) ListNode<T>::insert_as_succ(T const&e){
      ListNode<T> new_node = new ListNode<T>(e, pred, this);
      pred->succ = new_node;
      this->pred = new_node;
      return new_node;
    }
    
    template <typename T>
    ListNodePos(T) ListNode<T>::insert_as_pred(T const&e){
      ListNode<T> new_node = new ListNode<T>(e, this, succ);
      this->succ = new_node;
      succ->pred = new_node;
      return new_node;
    }
    ```

* **复制构造函数**

  ```c++
  #define ListNodePos(T) ListNode<T>*
  
  template <typename T>
  void List<T>::copy_nodes(ListNodePos(T) p, int n){
    init();
    while(n--) { insert_as_last(p->data); p = p->succ;}
  }
  
  template <typename T>
  List<T>::List(ListNodePos(T) p, int n) {
  	copy_nodes(p, n);
  }
  
  template <typename T>
  List<T>::List(List<T> l) {
  	copy_nodes(l.first(), l._size);
  }
  
  template <typename T>
  List<T>::List(List<T> l, rank r, int n) {
  	copy_nodes(l[r], n);
  }
  ```

* **删除**

  ```c++
  template <typename T>
  T List<T>::remove(ListNodePos(T) p) {
  	T e = p->data;
  	(p->pred)->succ = p->succ;
  	(p->succ)->pred = p->pred;
  	delete p;
  	_size--;
  	return e;
  }
  ```

* 
