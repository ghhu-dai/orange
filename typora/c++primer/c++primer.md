[TOC]







# 第二部分 ：C++标准库

## 第8章：IO库

```cpp
// 已介绍的IO库设施
istream; // 输入流类型，提供输入操作
ostream; // 输出流
cin;
cout;
cerr;
>>;
<<;
getline; // 从一个给定的istream读取一行数据一个给定的string对象中
```

### 8.1 IO类 

根据继承机制，普通流、文件流、string流、char及宽字符版本流有一些通用的标准库特性

#### 8.1.1 IO对象无拷贝或赋值

以引用方式传递和返回流，不能`const`

#### 8.1.2条件状态

* <font color = green>一个流一旦发生错误，后续IO操作都会失败，使用流之前应当检查它的状态</font>

  ```cpp
  while(cin>>word){
      // ok 读取操作成功
  }
  ```

* 到达文件结束位置 `eofbit`被置位，`goodbit`值为0表示流未发生错误

* 若`badbit`,`failbit`,`eofbit`任意一个被置位，检测流状态的条件会失败

* 可利用`s.fail() `和`s.good()`确定流的有效状态

* 管理条件状态

  ```cpp
  // s.rdstate() 返回流的当前状态,返回类型为strm::iostream
  // s.clear()清除（复位）错误标志位
  auto old_state =  cin.rdstate(); // 记录cin原始状态
  cin.clear(); // 使cin有效
  process_input(cin); // 使用cin
  cin.setstate(old_state); // 恢复cin原始状态
  ```

* `clear()`的重载版本

  ```cpp
  cin.clear(cin.rdstsate() & ~cin.failbit & ~cin.badbit);
  // 复位failbit 和 badbit其他标志位不变
  ```

#### 8.1.3 管理输出流缓冲

1. <font color = green>管理输出流缓冲区</font>

   ```cpp
   endl; // 换行并刷新缓冲区
   flush; // 刷新缓冲区不添加额外字符
   ends; // 添加空字符并刷新缓冲区
   ```

2. <font color = green>`unitbuf`操纵符</font>

   ```cpp
   cout<<unitbuf; // 任何输出都会全部刷新，无缓冲
   cout<<nounitbuf; // 回到正常缓冲方式
   
   // 如果 程序崩溃，输出流缓冲区不会被刷新
   ```

3. <font color = green>关联输入和输出流</font>

   ```cpp
   // 当一个输入流关联到一个输出流我时，任何读取输入流数据的操作都会先刷新关联的输出流，标准库的cin和cout是关联在一起的
   
   cin.tie(&cout); // tie返回关联的流
   
   ```

### 8.2 文件传输系统

头文件`fstream` :`ifstream`,`ofstream`,`fstream`



相关操作：

```cpp
fstream fstrm; // 创建未绑定的文件流
fstream fstrm(s); // 创建fstream并打开文件s，open自动调用
// s是string或指向c风格字符串的指针，表示文件名

fstream fstrm(s,mode); // 按指定mode打开文件
fstrm.open(s); // 打开s并与fstrm绑定
fstrm.close();
fstrm.isopen(); // 判断与fstrm关联的文件是否成功打开且尚未关闭
```

#### 8.2.1 使用文件流对象

```cpp
ifstream in(ifile); // 初始化为从文件读取数据
oftream out; // 输出文件流未关联到任何文件

// 调用open时要检测是否打开成功
// 当一个fstream对象销毁时，close会自动调用->文件自动关闭
```

#### 8.2.2 文件模式

```cpp
// 每次调用open时都会确定文件模式

in; // 以读方式打开，ifstream关联文件默认in打开
out; // 以写方式打开，ofstream关联文件默认out打开
app; // 追加写入
ate; // 打开文件后立即定位到文件末尾
trunc; // 截断文件
binary; // 二进制形式IO
```

以`out`模式打开的文件会丢失已有数据，欲保留需要显式指定`app`模式

### 8.3 string 流

头文件`sstream `：`istringstream`,`ostringstream`,`stringstream`

相关操作：

```cpp
stream strm;
stream strm(s); // 保存strign s的一个拷贝
strm.str(); // 返回strm保存的string的拷贝
strm.str(s); // 将string s 拷贝到strm中返回void，会清原先有的字符串
```

---



## 第9章 顺序容器

该顺序不依赖元素 的值，而是与元素加入到容器时的位置相对应，顺序容器篮球快速顺序访问，但不利于添加删除元素和非顺序访问元素

---



###  9.1 顺序容器概述

```cpp
vector; // 可变大小数组 ，支持快速随机访问，在尾部之外的位置插入和删除元素可能会很慢 

deque; // 双端队列，支持快速随机访问，在头尾位置插入和删除元素很快

list; // 双向链表，只支持双向顺序访问，在list中任何位置进行插入和删除操作都很快，不可随机访问
forward_list; // 单向链表，只支持单向顺序访问，在链表任何位置进行插入删除操作速度都很快，不可随机访问

array; // 固定大小数组，支持快速随机访问，不能插入和删除元素

string; // 类vector，随机访问快，在尾部插入和删除更快
```



<font color = gareen>顺序容器的选择</font>

```cpp
1. 默认选vector;
2. 小元素多，且重视空间的额外开销，不要用list,forward_list;
3. 要随机访问元素，用vector,deque;
4. 需要在容器的中间插入和删除元素，用list,forward_list;
5. 只需要在头尾操作，用deque;
6. 更加复杂的使用场景，可以考虑多种容器组合的方式;
```



### 9.2 容器库概览

<font color = green>所有容器都适用的操作，不只针对于顺序容器</font>

```cpp
// 类型别名
iterator; 
const_iterator; 

size_type; // capacity
difference_type; // 两个迭代器之间的距离

value_type;
reference; // 元素的左值类型，等同于：value_type&
const_reference;

// 构造函数 
C c; 
C c1(c2);
C c(b,e); // b,e是两个迭代器
C c{a,b,..}; // 列表初始化


// 赋值与swap
c1 = c2;
c1 = {d,d};

a.swap(b);
swap(a,b); // 为泛型编程作准备，最好用这种方法，而不是上面的方法

// 大小
c.size(); // 元素的数目，forward_list不适用
c.max_size(); // 容量
c.empty();

// 添加和删除元素,不适用array
c.inset(args); // 将args中的元素拷贝到c
c.emplace(inits); // 使用inits构造c中的一个元素
c.erase(args); // 删除args指定的元素
c.clear(); // 清空

// 关系运算符
==, != ;
<,<=,>,>= ; // 无序关联容器不支持

// 获取迭代器
c.begin(),c.end();
c.cbegin(),c.cend();

// 反向容器的额外成员 , 不支持forward_list
reverse_iterator; 
const_reverse_iterator;
c.rbegin(),c.rend();
c.crbegin(),c.crend();

#include <iostream>
#include <vector>

int main() {
  std::vector<int> v = {1, 2, 3, 4, 5};
  
  for (auto rit = v.rbegin(); rit != v.rend(); ++rit) {
    std::cout << *rit << " ";
  }
  
  return 0;
} // 输出 5，4，3，2，1

```



 #### 9.2.1 迭代器

标准库容器的所有迭代器，都支持`++`运算符，`forward_list`不支持`--`运算符

迭代器范围是左闭合区间`[ )`

#### 9.2.2 容器类型成员

```cpp
size_type;
iterator;
const_iterator;
反向迭代器等
    
value_type; // 返回容器元素类型
reference; // 引用元素类型
const_reference;
// value_type,reference,const_reference在泛型编程中比较有用
```

#### 9.2.3 begin和end成员

当`auto`与`begin`和`end`结合使用的时候，获得的迭代器类型依赖于容器 类型，以c开头的版本还是可以得到`constant_iterator`而不管容器的类型是什么

当不需要写操作时，应使用`cbegin`和`cend`

#### 9.2.4 容器定义和初始化

```cpp
C c; 
C c1(c2); // 等同于： C c1 = c2; 必须是相同类型的容器和相同类型的元素，array还必须是相同的大小
C c{a,b,c...}; // 列表初始化等同于：C c = {a,b,c}; 元素类型必须 相容，array列表元素数目必须 小于等于array大小

C c(b,e); // 元素类型要相容；

// 只有顺序容器（不包括array）的构造函数才能接受大小参数
C seq(n); 
C seq(n,t);
```



关于`array`,标准库`array`具有固定大小，大小也是 `array`类型的一部分

```cpp
array<int,10> ial; // 默认初始化
array<int,10> ia2 = {0,1,2,3,4,5,6,7,8,9}; // 列表初始化
array<int,10> ia3 = {42}; // ia3[0]为42，剩余元素为0

// 不能对内置数组类型进行拷贝和赋值，但可以对array作这些操作
int digs[10] = {0,1,2,3,4,5,6,7,8,9};
int cpy[10] = digs; // 错误
array<int,10> digits = {0,1,2,3,4,5,6,7,8,9};
array<int,10> copy = digits; // 正确，只要数组类型匹配即合法
```

#### 9.2.5 赋值和swap

赋值运算

```cpp
c1 = c2; // 如果 两个容器大小不同，赋值运算后两者的大小与右边的容器的原大小相同
c1 = {a,b,c}; // c1的大小将变成3,这点与列表初始化不同，所以这种用法不能对array使用

array<int,10> a1 = {0,1,2,3,4,5,6,7,8,9};
array<int,10> a2 = {0};
a1 = a2; // 替换a1的元素
a2 = {0}; // 错误，不能用花括号列表为array赋值


// 关于assign
seq.assign(b,e); // b,e不能指向seq中的元素，因为旧元素会被替换
seq.assign(li); // 将seq中的元素替换为初始化列表il的元素
seq.assign(n,t); // 将seq中的元素替换为n个值 为 t 的元素
// assign允许从一个不同但相容的类型赋值
list<string> names;
vector<const char*> oldstyle;
naems = oldstyle; // 错误，容器类型不匹配
// 正确：可以将const char* 转换为string
names.assign(oldstyle.cbegin(),oldstyle.cend());
```



关于`swap`

1. 交换两个容器的内部结构 ，不是交换容器内的元素，操作会很快，而且除string外，指向容器的迭代器，引用，指针不会失效，仍指向交换之前所指向的那些元素，但他们已属于不同的容器（什么ntr!可恶啊~），如iter之前指向svec1[3],之后 指向svec2[3],

2. 对于一个`string`,调用swap会使迭代器，引用和指针失效

3. 对于array,swap会真正交换他们的元素，操作时间与array的元素数目成正比，操作后，迭代器，引用，指针绑定的元素不变，但元素值 已经交换



#### 9.2.6 容器大小操作

```cpp
size();
empty();
max_size(); // 容量

// forward_list 不支持size,支持max_size和empty
```

#### 9.2.7 关系运算符

每个容器类型都有：== ，!=

无序关联容器外的容器支持：<, <= , >, >=

容器的关系运算符使用元素的关系运算符完成比较

---



### 9.3 顺序容器操作

#### 9.3.1 向顺序 容器添加元素（非array）

```cpp
// forward_list 有自己专有版本的insert,emplace;不支持push_back,emplace_back;
//vector,string不支持push_front,emplace_front
c.push_back(t);
c.emplace_back(args);
c.push_front(t);
c.emplace_front(args);

c.insert(p,t); // 在p指向的元素之前创建一个值为t的元素，迭代器不能指向目标容器，返回添加元素的迭代器
c.emplace(p,args); // 在p指向的元素之前 创建一个由args构造的元素
c.insert(p,n,t);
c.insert(p,b,e);
c.insert(p,il);


// emplace 函数在容器中直接构造元素，传递给emplace函数 的参数必须与元素类型的构造函数相匹配
```





#### 9.3.2 访问元素

```cpp
.front();
.back(); // .end()指向的元素前一个
// 包括array的所有顺序容器都支持.front(),.back()，forward_list不支持.back();

c[n];
c.at(n); // 通过at用下标访问元素，如果 下标越界，会抛出out_of_range异常

// at,back,front返回的都是元素引用，例 ：
if(!c.empty()){
    c.front() = 42;
    auto &v = c.back();
    v = 1024; // v是指向最后一个元素的引用，改变了c中的元素
    auto v2 = c.back();
    v2 = 0; // v2不是引用，未改变c中的元素
}
```

####  9.3.3 删除元素

```cpp
// forward_list有特殊版本的erase,不支持pop_back;
// vector,string不支持pop_front
c.pop_back();
c.pop_front();
c.erase(p); // 删除p指向的元素的，返回之后元素的迭代器
c.erase(b,e);
c.clear();


// 例 ：删除一个list中的所有奇数元素
list<int> lst = {0,1,2,3,4,5,6,7,8,9};
auto it = lst.begin();
while(it != lst.end() ){
    if(*it %2 ){
        it = lst.erase(it);
    } else {
        ++it;
    }
}
```

#### 9.3.4 特殊的forward_list操作

```cpp
lst.before_begin(); // 返回指向链表首元素之前 不存在的元素，此迭代器不可解引用
lst.cbefore_begin();

lst.insert_after(p,t); // 在p迭代器之后 插入元素,返回指向新元素的迭代器
lst.insert_after(p,n,t);
lst.insert_after(p,b,e);
lst.insert_after(p,il);

emplace_after(p,args); // 在p迭代器之后构造元素,返回指向新元素的迭代器

lst.erase_after(p); // 删除p指向元素之后的元素，返回再下一个的迭代器
lst.erase_after(b,e);


//在forward_list中添加删除元素，必须 关注两个迭代器，一个是要处理的元素，另一个是前驱
// 例 ：从list中删除奇数元素
forward_list<int> lst = {0,1,2,3,4,5,6,7,8,9};
auto prev = lst.before_begin();
auto curr = lst.begin();
while(curr != lst.end()){
    if(*cur  % 2){
        curr = lst.erase_after(prev);
    } else{
        prev = curr;
        ++curr;
    }
}
```

#### 9.3.5 改变容器大小

当前大小 大于 所要求的大小，容器后面的的元素会被删除；当前大小 小于 新大小，会在容器后添加新元素

```cpp
c.resize(n); 
c.resize(n,t); // 如果 需要新添加值 ，值为t
```

#### 9.3.6 容器操作可能 使迭代器失效

建议：在每次改变容器的操作后都重新定位 迭代器，引用，指针

```cpp
// 例：删除偶数元素，复制奇数元素
vector<int>vi = {0,1,2,3,4,5,6,7,8,9};
auto iter = vi.begin();
while(iter != vi.end()){
    if(*iter % 2){
        iter = vi.insert(iter,*iter); // 复奇数元素并更新迭代器iter
        iter += 2;
    }else {
        iter = vi.erase(iter); // 删除偶数元素
    }
}
```

不要保存end()返回的迭代器,尤其是`vector`,`string`,`deque`

```cpp
// 死循环版本
auto begin = v.begin();
auto end = v.end();
while(begin != end){
	//一些容器操作
}

// 修正版
auto begin = vi.begin();
while(begin != vi.end()){
    //
}
```

---



### 9.4 vector对象是如何 增长的

vector和string的实现通常会分配比新的空间需求更大的内存空间

窗口大小管理操作

```cpp
// shrink_to_fit只适用于vector,string,deque
c.shrink_to_fit(); // 请将capacity减少为与size()相同的大小

// capacity和reserve只适用于vector,string
c.capacity();
c.reserve(n); // 分配至少可能容纳n个元素的内存空间，只有需要的内存空间大于当前容量时才会执行操作
```

### 9.5 额外的string操作

#### 9.5.1额外构造函数 ：

```cpp
string s(cp,n); // s是cp前n个字符的拷贝

string s(s2,pos2); // s是string s2 从下标pos2开始的字符 的拷贝

string s(s2,pos,len2); 
```

```cpp
s.substr(pos,n); // 返回一个string，包含s中从pos开始的n个字符的拷贝
```

#### 9.5.5 改变string的其他方法

appnd,replace函数

```cpp
s.append("dd"); // 尾部追加
s.replace(pos,3,"dai"); // 等价于erase , insert的联合操作,从pos开始，删除3个字符，插入"dai"

// append,replace的重载版本很丰富，基本可以使用之前介绍的所有版本
```

#### 9.5.3 string搜索操作

<font color = blue>`string`搜索函数 返回`string::size_type`·值 ，该类型是一个`unsigned`类型，尽量不要用`int`或其他带 符号类型来保存搜索函数的返回值 </font>

```cpp
s.fing(args); // 查找s中args第一次出现的位置
s.rfind(args); // 查找最后一次出现 的位置
s.find_first_of(args); // 查找 args中任何一个字符 第一次出现 的位置
s.find_last_of(args); // 查找 args 中任何一个字符 最后一次出现 的位置
s.find_first_not_of(args); // 查找第一个不在args中的字符 
s.find_last_not_of(args); // 查找 最后一个不在args中的字符

// args必须是以下形式之一
c,pos; // c是字符 
s2,pos; // s2是字符串
cp,pos; // 从s 中pos位置开始查找指针cp指向的以空字符结束的c风格字符串
cp,pos,n; // n表示 前n个字符 
```





#### 9.5.4 compare函数 

```cpp
s.compare(args);

// args的参数形式：
s2;
pos1,n1,s2;
pos1,n1,s2,pos2,n2; // 将s1中从pos1开始的n1个字符 与 s2中从pos2开始的n2个字符 进行比较

cp;
pos1,n1,cp;
pos1,n1,cp,n2; // 将s中从pos1开始的n1个字符 与cp指针指向的地址开始的n2个字符进行比较
```







#### 9.5.5 数值转换

```cpp
// string 和 数值之间的转换


// 例：
int i = 42;
string s = to_string(i); // 将整数 i 转换为字符 表示 
double d = stod(s); // 将字符s转换为浮点数
```

---



### 9.6 容器适配器

适配器接受一种已有的容器类型，使其行为看起来像一种不同的类型，如stack适配器接受一个顺序容器（arrary,forwd_list除外 ），使其操作起来像一个stack一样

```cpp
// 所有容器适配器都支持的操作和类型

size_type;//  一种类型，足以保存当前类型的最大对象的大小
value_type; // 元素类型
container_type; // 实现适配器的底层容器类型

A a; // 创建一个名为a 的空适配器
A a(c); // 创建一个名为a的适配器，带有容器 c 的一个拷贝

每个适配器都支持所有关系运算符:==,!=,<,<=,>,>=,返回底层容器的比较结果;

a.empyt();
a.size(); // 返回a中 的元素数目

swap(a,b); // 要求a,b有相同类型，包括底层容器
a.swap(b);     
    	
```



定义一个适配器

```cpp
// 默认情况下，stack,queue基于deque实现，priority_queue基于vector实现，也可以重载默认容器类型

// 在vector上实现的空栈
stack<string,vector<string>> str_stk;

// str_stk2在vector上实现，初始化时保存svec的拷贝
stack<string,vector<string>> str_stk2(svec);

```

所有适配器都要求容器具有添加，删除，访问尾元素 的能力，所以不能以`array`,`forward_list`为基础构建 适配器

| 适配器         | 底层容器特性                               | 底层容器                          |
| -------------- | ------------------------------------------ | --------------------------------- |
| stack          | `push_back`   `pop_back`  `back`           | array, forward_list以外的任何容器 |
| queue          | `back`  `push_back`  `front`  `push_front` | list, **deque**                   |
| priority_queue | `front` `push_back`  `pop_back`            | **vector** ,deque                 |



栈适配器

```cpp
// 栈的封装操作
s.pop();
s.push(item);
s.emplace(args);
s.top();
```

队列适配器：

`queue`和`priority_queue`都定义在`queue`头文件中

```cpp
// queue priority_queue的封装操作
q.pop(); // 返回queue的首元素，或priority_queue的最高优先级的元素，但不删除此元素

q.front();

q.back(); // 只适用于queue;
q.top(); // 返回最高优先级元素，只适用于priority_queue

q.push(item); // 在queue的末尾或priority_queue中恰当的位置创建一个元素
q.emplace(args);
```

---





## 第10章：泛型算法

独立于任何特定的容器

### 10.1 概述

头文件：`algorithm`,`numeric`,后者是数值泛型算法

 