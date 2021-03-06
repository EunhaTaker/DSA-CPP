## 目录

<!-- #### 数组 -->

* [vector](#vector)
  * [有序vector](#sorted-vector)
    *  [集合](#set)
  * [字符串](#string)
  * [完全二叉堆](#heap)
  * 栈


<!-- #### 链表 -->
- [ ] list
  - [ ] 队列

<!-- #### 树 -->
* [二叉树](#bintree)
  * [二叉搜索树](#BST)
    * AVL树
    * 伸展树
  * 森林

## 详情
<a name="vector"></a>
### vector

```CPP
// 自动扩容，采用2倍扩容策略
// RANK为秩类型，默认为int
```
+ 构造
  ```CPP
  // 默认构造，按容量初始化空vector
  Vector(RANK capacity=DEFAULT_CAP);
  // 拷贝构造
  Vector(Vector<T> const& V, RANK lo=0, RANK hi=-1);
  // 传入长度和默认值
  Vector(RANK, const T& default_value);
  // 传入序列及区间，复制到本vector进行构建
  Vector(const T *A, RANK lo, RANK hi, bool heapFlag=false);
  // 使用列表初始化容器进行初始化
  Vector(const initializer_list<T>&);
  ```
+ 重载运算符
  ```CPP
  // []  循秩访问（允许负数索引）
  T e = arr[index];   // 读
  arr[index] = e;     // 改
  // +   返回两个vector合并产生的副本
  Vector<T> v = v1 + v2;
  // +=  在末端追加（vec表示Vector<int>)
  vec += 5;   // 追加单个元素
  vec += {5, 6};  // 追加初始化序列
  vec += vec2;     // 追加另一Vector<int>
  // 注意：vec+=x 不等价于 vec=vec+x，后者会额外创建副本，对于Vector的所有派生类，类似的操作符都有这种区别
  ```
+ 访问
  ```CPP
  // 循秩访问（允许负数索引，索引范围：[-size, size) ）
  T get(int index);
  // 在区间内按值查找值的索引（失败返回-1）
  virtual RANK find(const T& value, RANK lo=0, RANK hi=-1);
  // 截取区间
  Vector<T> sub(RANK lo, RANK hi);
  ```
+ 修改
  ```CPP
  void put(RANK idx, const T& value);
  ```
+ 插入
  ```CPP
  // 插入单个元素
  void insert(RANK idx, const T& value);
  // 插入一段vector
  void insert(RANK idx, const Vector<T> &v);
  ```
+ 末端追加
  ```CPP
  // 追加单个元素
  void append(const T& value);
  // 追加数组区间
  void extend(const T* A, RANK lo, RANK hi);
  // 追加一段序列
  void extend(const initializer_list<T>&);
  // 追加另一Vector
  void extend(const Vector<T> &);
  // 返回两个vector合并产生的副本
  auto concat(Vector<T> v);   // auto是为了便于派生类使用
  ```
+ 删除
  ```CPP
  // 按索引删除
  T pop(RANK idx);
  // 按值删除
  RANK remove(const T& value);
  // 删除区间
  bool delRng(RANK lo, RANK hi);
  // 清空
  void clear();
  ```
+ 获取信息
  ```CPP
  // 长度
  RANK size();
  // 是否为空
  bool empty();
  ```
+ 排序
  ```CPP
  void sort(RANK lo=0, RANK hi=-1);
  ```
+ 去重
  ```CPP
  virtual RANK unique();
  ```
+ 遍历操作
  ```CPP
  template<typename VST>  // VST是函数指针
  void map(VST &visit);
  ```

<a name="sorted-vector"></a>
### sorted vector

```CPP
// 继承自vector，禁用部分父类api
```

+ 构造
  ```CPP
  // 默认构造，按容量初始化空SortedVector
  SortedVector(RANK capacity=DEFAULT_CAP);
  // 拷贝构造--复制SortedVector
  SortedVector(const SortedVector<T> & sv, RANK lo=0, RANK hi=-1);
  // 构造--根据Vector
  SortedVector(const Vector<T>& sv, RANK lo=0, RANK hi=-1);
  // 构造--根据初始化列表
  SortedVector(const initializer_list<T>&);
  ```
+ 扩充
  ```CPP
  // 新增元素
  RANK add(const T& e);
  // 合并另一有序数组
  void extend(const SortedVector<T> &);
  // 合并Vector
  void extend(const Vector<T> &);
  // 合并两个SortedVector生成副本
  auto concat(const Vector<T> &);   // auto是为了便于派生类使用
  ```
+ 访问
  ```CPP
  // 继承，循秩访问（允许负数索引，索引范围：[-size, size) ）
  T get(int index);
  // 二分查找，失败时返回-1
  virtual RANK find(const T& value, RANK lo=0, RANK hi=-1);
  // 继承，截取区间
  Vector<T> sub(RANK lo, RANK hi);
  ```
+ 重载运算符
  ```CPP
  // 继承，[]  循秩访问（允许负数索引）
  T e = arr[index];
  // +   返回两个有序vector合并产生的副本
  SortedVector<T> v = v1 + v2;
  // +=  在末端追加（vec表示Vector<int>)
  vec += 5;   // 追加单个元素
  vec += vec2;     // 追加另一SortedVector<int>
  ```
+ 删除
  ```CPP
  // 继承，按索引删除
  T pop(RANK idx);
  // 继承，按值删除
  RANK remove(const T& value);
  // 继承，删除区间
  bool delRng(RANK lo, RANK hi);
  // 继承，清空
  void clear();
  ```
+ 获取信息
  ```CPP
  // 继承，长度
  RANK size();
  // 继承，是否为空
  bool empty();
  ```
+ 去重
  ```CPP
  virtual RANK unique();
  ```
+ 遍历操作
  ```CPP
  template<typename VST>  // 继承，VST是函数指针
  void map(VST &visit);
  ```


<a name="set"></a>
### set

```CPP
// 继承自sorted vector，禁用部分父类api
```
+ 构造
  ```CPP
  // 默认构造：根据容量创建空集合
  Set(RANK capacity=DEFAULT_CAP);
  // 拷贝构造
  Set(const Vector<T>& set);
  // 利用初始化列表创建集合
  Set(const initializer_list<T>& il);
  ```
+ 增删
  ```CPP
  // 新增元素
  bool add(const T& e);
  // 按值删除
  void remove(const T& value);
  // 继承，清空
  void clear();
  ```
+ 重载运算符
  ```CPP
  // 判断两集合相等 ==
  set == set1;  // bool
  // 真含于 <
  set < set1;  // bool
  // 含于 <=
  set <= set1;  // bool
  // 真包含 >
  set > set1;   // bool
  // 包含
  set >= set1;  // bool
  // 是否含有(元素)
  set >> elem;  // bool，等价操作：set.contain(elem);
  // 元素是否属于集合
  elem << set;  // bool, 同样等价于：set.contain(elem);
  // 并集 |
  set | set1;
  // 取并集并更新于自身 |=
  set |= set1;
  // 交集 &
  set & set1;
  // 取交集并更新于自身 &=
  set &= set1;
  // 差集 -
  set - set1;
  // 取差集并更新于自身 -=
  set -= set1;
  // 对称差 ^
  set ^ set1;
  // 取对称差并更新于自身 ^=
  set ^= set1;
  ```
+ 获取信息
  ```CPP
  // 继承，长度
  RANK size();
  // 继承，是否为空
  bool empty();
  ```
+ 遍历操作
  ```CPP
  template<typename VST>  // 继承，VST是函数指针
  void map(VST &visit);
  ```

<a name="string"></a>
### string

```CPP
// 继承自vector，禁用部分父类api
```
+ 构造
  ```CPP
  // 默认构造，复制字符数组生成
  String(const char* A="", RANK lo=0, RANK hi=-1);
  // 拷贝构造
  String(const String&);
  ```
+ 查找
  ```CPP
  // 查找字符数组
  RANK find(const char*);
  // 查找String
  RANK find(const String&);
  ```
+ 替换
  ```CPP
  // 全替换（返回替换次数）
  int replace(const char*, const char*);    // 两参数可分别替换为String类型
  // 插入
  void insert(RANK, const char*);
  void insert(RANK, const String&);
  ```
+ 子串
  ```CPP
  // 截取
  String sub(RANK, RANK);
  // 截去
  String cut(RANK, RANK);
  ```
+ 重载运算符
  ```CPP
  // +
  str + 'a';    // 字符
  str + "abc";  // 字符数组
  str + str1;   // string
  /* += 提供与+同样的支持，区别是不产生副本 */
  // ==
  str1 == str2;   // 字符串判等
  // <<
  cout<<str;      // 输出流
  // []  循秩访问（允许负数索引）
  char e = str[index];   // 读
  str[index] = e;     // 改
  ```
+ 数组操作
  ```CPP
  // 是否空
  bool empty() const;
  // 大小
  RANK size() const;
  // 按索引获取值
  T& get(RANK idx) const;
  // 修改
  void put(RANK idx, const T& value);
  ```

<a name="heap"></a>
### compl-heap
+ 继承自vector
- 实现功能
  + 获取最大元素
  + 删除最大元素
  + 插入元素

<!-- <a name="bintree"></a>
### bintree
- binNode实现：
  + 是否是父亲的左孩子
  + 左/右插入
  + 更新此节点及祖先节点高度
  + 先序遍历
  + 中序遍历
  + 打印以该节点为根的树
  + 顺/逆时针旋转
- binTree实现：
  + 查找
  + 删除

<a name="BST"></a>
### BST
+ 继承自binTree
- 实现功能：
  + 二叉查找
  + 获取最小/最大节点
  + 插入
  + 删除 -->

<!-- <a name="AVL"></a>
### AVL
+ 继承自BST
- 实现功能：
  +  -->