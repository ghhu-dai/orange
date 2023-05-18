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

迭代器令算法不依赖于容器，但算法依赖于元素类型的操作

算法不会改变底层容器的**大小**





### 10.2 初识泛型算法



大多数标准库算法都对一个范围内的元素进行操作，用两个迭代器表示 范围





#### 10.2.1 	只读算法



```cpp
// 如，find ,count, accumulate（定义在numeric头文件 中）,equal

accumulate: // 求范围内的和
    
	int sum = accumulate(vec.cbegin(),vec.cend(),0); // 求和，和的初值设定为0
	// accumulate的第三个参数的类型决定了函数中使用哪个加法运算符以及返回值 类型

	string sum = accumulate(v.cbegin(),v,cend(),string(""));
	// string定义了+，字符串字面值 const char * 没有+运算符，所以不能用 "" 替换 string("")

equal: // 确定两个序列对应元素是否相同 
    
	equal(roster1.cbegin(),roster1.cend(),roster2.cbegin());
	// 只接受单一迭代器来表示 第二个序列的算法，都假定第二个序列至少与第一个序列一样长
	

```





#### 10.2.2 写容器元素的算法



```cpp
// 如：fill
fill(vec.begin(),vec.end(),0); // 将范围内的元素都重置为0
```

算法不检查写操作

```cpp
// fill_n(dest,n,val); 假定dest指向一个元素，而从这开始的元素至少包含n个元素
vector<int> vec; // 空容器
fill_n(vec.begin(),vec.size(),0); // 将所有元素重置为0
fill_n(vec.begin(),10,0); // 错误，容器中没有这么多的元素
```



关于`back_inserter`

头文件：`iterator`

```cpp
vector<int> vec; // 空向量
fill_n(back_inserter(vec),10,0); // 正确，添加10个元素到vec
// 相当于每次赋值都会在vec上调用push_back
```



拷贝算法：

```cpp
// copy
int a1[10] = {0,1,2,3,4,5,6,7,8,9};
int a2[sizeof(a1)/sizeof(*a1)];
auto ret = copy(begin(a1),end(a1),a2); // ret指向拷贝到a2尾元素之后的位置

// replace
replace(ilst.begin(),ilst.end(), 0, 42); // 将范围内的0换为42

// replace_copy
replace_copy(ilst.cbegin(),ilst.cend(),back_inserter(ivec), 0, 42);
// 此调用后，ilst并未改变，ivec包含ilst 的一分拷贝，不过原来在ilst中值 为0 的元素在ivec中变为42
```







#### 10.2.3　重排容器元素的算法



`sort`:利用元素类型的`<`运算符实现排序                 

```cpp
// 利用sort,unique,vector.erase消除重复单词

void elimDups(vector<string> &words){
    // 按字典序排序words，以便查找 重复单词
	sort(words.begin(),words.end());
    // 重排输入范围，全每个单词只出现一次，排列在范围的前部，返回指向不重复区域之后 的一个位置的迭代器
    auto end_unique = unique(words.begin(),words.end());
    // 使用向量操作erase删除数组 ,erase删除元素不会减小和容量
    words.erase(end_unique,words.end());
}
```

 

<img src="D:\A_Git_learn\typora\c++primer\img\unique_before.png" alt="unidque前" style="zoom:68%;" />

`unique`后

<img src="D:\A_Git_learn\typora\c++primer\img\unique_after.png" alt="d" style="zoom:68%;" />







### 10.3 定制操作

#### 10.3.1 向算法传递函数 

谓词：一元谓词，二元谓词

```cpp
bool isShorter(const string&s1, const string &s2){ // 这是一个二元谓词
    return s1.size()<s2.size();
}

// 按单词长短来排序
sort(words.begin(),words.end(),isShorter);
```

稳定排序算法

```cpp
// stable_sort 可以维持原有的相对顺序

elimDups(words); // 将words按字典序重排，并消除重复单词

stable_sort(words.begin(),words.end(),isShorter); // 传递函数没有() 

for(const auto &s :words){ // 无须拷贝字符串
    cout<<s<<" ";
}
cout<<endl;
```



#### 10.3.2 lambda表达式

使用谓词无法解决复杂排序问题时，可用`lambda`表达式

关于lambda表达式

```cpp
// lambda 表达式可以理解为一个未命名的内联函数 ，有一个捕获列表，一个返回类型（必须是尾置返回），一个参数列表，一个函数体，与函数不同的是它可以定义在函数内部,
[capture list](parameter list)-> return type {function body}
// 其中 [parameter list] -> return type 可以省略

auto f = []{ return 42 }; // 参数列表为空,return type
cout<< f() <<endl; // 调用方式同普通 函数 一样

// 向lambda传递参数，因为lambda不能有默认参数，实参形参数目相等

//使用捕获列表，一个lambda只有在其捕获列表中捕获一个它所在的函数中的局部变量，才能在函数体中使用该变量，lambda可以直接使用static变量和在它所在函数之外声明的名字



```

例 ：定义函数 biggies:求大于等于一个给定长度的单词有多少，并输出 

```cpp


void biggies(vector<string> &words,vector<string>::size_type sz){
    // 将words按字典序排序 ，删除重复单词
    elimDups(words); 
    
    // 按长度排序 ，长度相同的按字典序排序 
    stable_sort(words.begin(),words.end(),isShorter); 
    
    // 获取 一个迭代器，指向第一个满足size()>=sz的元素
    // 利用find_if(iter1,iter2,一元谓词)，+ lambda表达式
    auto wc = find_if(words.begin(),words.end(),[sz](const string &a){
        return a.size()>=sz;
    });
    
    // 计算满足size>=sz的数目
    auto count = words.size()-wc;
    
    // 打印长度大于等于给定值 的单词，每个单词后面接一个空格
    for_each(wc,words.end(),[](const string &s){
        cout<<s<<" ";
    }); // for_each接受一个可调用对象，这里是lambda表达式，对输入范围内的每个元素都调用它
    cout<<endl;
    
}
```



#### 10.3.3 lambda捕获和返回



**值 捕获**：被 捕获的变量是在lambda创建时拷贝，而不是调用时拷贝

**引用 捕获**： 常用于对输入输出 流进行引用捕获

**建议**：少捕获，尽量不要捕获指针或引用

**隐式捕获**：

**可变**`lambda`

```cpp
// 默认情况下，lambda表达和其函数体是const的，不可修改捕获的值 ，可使用mutable使其可变
void fcn3{
    size_t v1 = 42;
    auto f =[v1]()mutable{
        return ++val;
    };
    v1 = 0;
    auto j = f(); // j 为43
}
```



指定`lambda`返回类型

```cpp
transform(vi.begin(), vi.end(), vi.begin(),[](int i)->int{ // 不能推断lambda的返回类型，必须指定
    if(i<0) return -i;
    else return i;
});
// transform接受三个迭代器，一个可调用对象，前两个迭代器表示输入序列，第三个迭代器表示目的位置，算法对输入序列中每个元素调用可调用对象，并将结果写到目的位置
```





#### 10.3.4 参数绑定

标准库`bind`函数 `auto newCallable = bind(callable,arg_list)`

```cpp
// 定义check_size
bool check_size(const string &s, string::size_type sz){
    return s.size() >= sz;
}

// 利用bind将
auto wc = find_if(words.begin(),words.end(),[sz](const sring &a){ return a.size()>=sz });
// 替换为；
auto wc = find_if(words.begin(),words.end(),bind(check_size,_1,sz));
// bind 调用生成一个可调用对象，将check_size第二个参数绑定到sz的值，_1是占位符在args_list的第一个位置，对应check_size第一个参数
// 这样将二元谓词转换为一元谓词，满足find_if的调用
```

使用`placeholders`名字

```cpp
using std::placeholders::_1;
// 等价于：
using namespace std::placeholders; // 更全面包含所有名字


// bind ， placeholders命令空间都定义在functional头文件中
```



`bind`的参数

```cpp
// g 是一个有两个参数的可调用的对象
auto g = bind(f,a,b,_2,c,_1);
// 新的可调用对象将自己的参数作为第三个和第五个参数传递给f，第一二四个参数分别被 绑定到a,b,c上


// 用bind重排参数顺序
sort(words.begin(),words.end(),isShorter);// 单词从短到长排
sort(words.begin(),words.end(),bind(isShorter,_2,_1)); // 单词从长到短排

// 绑定引用参数：使用标准库ref函数 ,有cref版本，头文件：functional
// ：默认情况下，bind的非占位符参数以拷贝方式到bind返回的可调用对象
ostream　&print(ostream &os,const string &s,char c ){
    return os<<s<<c;
}

for_each(words.begin(),words.end(),
         bind(print,ref(os),_1,' ')); // _1表示 在实际调用中其可被容器中的每个元素替代， ' '是print的第三个参数
```





### 10.4 再探迭代器

头文件`iterator`: 

​	`insert iterator`, `stream iterator,` `reverse iterator `,` move iterator`

#### 104.1 插入迭代器

```cpp
back_inserter;
front_inserter;
inserter; // 创建一个使用insert的迭代器，此函数 必须接受第二个参数

```

`inserter`

```cpp
it = c.insert(it,val);
++it; // 使it指向本来的元素(即：问题在指定位置前加入)
// 等价于：
*it = val; // 如果it是insert生成的迭代器.
```

`back_inserter`

```cpp
list<int> lst = {1,2,3,4};
list<int> lst2 ,lst3; // 空list

// 拷贝完成后lst2包含4，3，2，1
copy(lst.cbegin(),lst.cend(),front_inserter(lst2));

// 拷贝完成后lst3包含1,2,3,4
copy(lst.cbegin(),lst.cend(),inserter(lst3,lst3.begin()));
```





#### 10.4.2 `iostream`迭代器

`iostream`类型不是容器

`istream_iterator`：读取输入流

```cpp
istream_iterator<int> in_iter(cin),eof; // in_iter从cin中读取int，eof是空的istream_iterator，当作尾后迭代器使用
vector<int> vec(in_iter,eof); 


// iostream_iterator前置和后置++,表示从输入流读取下一个值(>>)


// 使用算法流操作迭代器，如accumulate
istream_iterator<int> in(cin),eof;
cout<<accumulate(in,eof,0)<<endl;


// istream_iterator允许使用懒惰求值
"当将一个istream_iterator绑定到一个流时，标准库并不保证迭代器立即从流读取数据，具体实现可以推迟从流中读取数据 ，直到使用迭代器时才真正读取； 标准库中的实现保证的是，在第一次解引用迭代器之前 ，从流中读取数据的操作已经完成了"
```



`ostream_iterator`：向一个输出 流写数据 

```cpp
ostream_iterator<T> out(os); // out将类型为T的值写到输出流os中
ostream_iterator<T> out(os,d); // out将类型为T的值写到os中，每个值后面都输出一个d，d是c风格字符串


// 用ostream_iterator来输出值 的序列

ostream_iterator<int> out_iter(cout," ");

for(auto e: vec)
    *out_iter++ = e;
cout<<endl; 
// 该循环等价于
copy(vec.begin(), vec.end(),out_iter);
cout<<endl;
```



使用流迭代器来处理类类型

```cpp
// 重写书店程序
istream_iterator<Sales_item> item_iter(cin),eof;
ostream_iterator<Sales_item> out_iter(out,"\n");
Sales_item sum = *item_iter++;
while(item_iter!=eof){
    if( item_iter->isbn() == sum->isbn() ){
        sum += *item_iter++;
    }else{
        out_iter = sum;
        sum = *itrm_iter++;
    }
}
out_iter = sum; // 打印最后一组纪录
```







#### 10.4.3 反向迭代器

只有支持`++`也支持`--`的迭代器才能定义反向迭代器

与算法配合

```cpp
sort(vec.begin(),vec.end()); // 正常序排序 
sort(vec.rbegin(),vec.rend()); // 倒序排序
```

在一个逗号分隔的列表中查找 第一个元素

```cpp
// 在一个逗号分隔的列表中查找第一个元素
auto comma = find(line.cbegin(),line.cend(),','); // 返回范围内第一个','的迭代器
cout<<string(line.cbegin(),comma) << endl;

```

在一个逗号分隔的列表中查找第一个元素

```cpp
auto rcomma = find(line.cbegin(),line.cend(),','); // 返回范围内最后一个','的迭代器,rcomma也是 一个反向迭代器
// 错误：将逆序打印输出单词的字符
cout<<string(line.crbegin(),rcomma)<<endl; // 如：FIRST,MIDDLE,LAST -> TSAL

// 利用reverse_iterator.base成员函数得到正向迭代器
cout<<string(rcomma.base(),line.cend())<<endl;
```



![](.\img\反向迭代器.png){width=50% height=50%}

普通迭代器和反向迭代器反映了左闭合区间：`[line.crbegin(),rcomma)` 和 `[ rcomma.base(),line.cend() )`包含相同的元素范围,所以`rcomma.base()` 和`rcomma`指向不同的元素 







### 10.5 泛型算法结构 

任何算法的最基本的特性是它要求其迭代器提供哪些操作

算法要求的迭代器操作可以分为5个迭代器类别：

| 迭代器类别     | 操作                           |
| -------------- | ------------------------------ |
| 输入迭代器     | 只读，不写，单遍扫描，只能递增 |
| 输出迭代器     | 只写，不读，单遍扫描，只能递增 |
| 前向迭代器     | 可读写，多遍扫描，只能递增     |
| 双向迭代器     | 可读写，多遍扫描，可递增 递减  |
| 随机访问迭代器 | 支持全部操作                   |



#### 10.5.1 5类迭代器类

。。。





#### 10.5.2 算法形参模式

```cpp
alg(beg,end,other args);
alg(beg,end,dest,other args);
alg(beg,end,beg2,other args);
alg(beg,end,beg2,end2,other args);
```



#### 10.5.3 算法命名规范

一些算法使用重载形式传递一个谓词

```cpp
unique(beg,end); // 使用==运算符比较元素
unique(beg,end,comp); // 使用comp比较元素
```

`_if`版本的算法

```cpp
find(beg,end,val); // 查找输入范围内val第一次出现的位置
find_if(beg,end,pred); // 查找第一个令pred为真的元素
```

区分拷贝元素的版本和不拷贝的版本

```cpp
reverse(beg,end); // 反转输入范围中元素的顺序 
reverse_copy(beg,end,dest); // 将元素按逆序拷贝到dest（额外目的空间）
```

```cpp
remove_if(v1.begin(),v1.end(),[](int i){return i % 2;}); // 删除v1中奇数元素
remove_copy_if(v1.beg(),v1.end(),back_inserter(v2),[](int i){return i % 2});
```





### 10.6 特定容器算法

对于`list`和`forward_list`应该优先使用成员函数版本的算法而不是通用算法

`list`和`forward_list`成员函数版本的算法

| 这些操作都返回void     |                                                              |
| ---------------------- | ------------------------------------------------------------ |
| `lst.merge(lst2)`      | 将lst2的元素合并入`lst`,`lst`,`lst2`的元素都必须是有序的(默认<)，合并后`lst2`为空 |
| `lst.merge(lst2,comp)` |                                                              |
| `lst.remove(val)`      | 调用`erase`删除掉与给定值 相等或令一元谓词为真的每个元素     |
| `lst.remove_if(pred)`  |                                                              |
| `lst.reverse()`        | 反转链表中的元素                                             |
| `lst.sort()`           | 使用<或给定比较操作排序元素                                  |
| `lst.sort(comp)`       |                                                              |
| `lst.unique()`         | 调用`erase`删除同一个值 的连续拷贝版本1使用==，版本2使用给定的二元谓词 |
| `lst.unique(pred)`     |                                                              |

`splice`成员（链表特有）

| listt 和 forward_list的splice成员函数 的参数， |                                                              |
| ---------------------------------------------- | ------------------------------------------------------------ |
| `list.splice(args)`或`flst.splice_after(args)` |                                                              |
| `(p,lst2)`                                     | p是一个指向`lst`中元素的迭代器，或一个指向`flst`首前位置的迭代器，函数 将`lst2`的所有元素移动到`lst`中p之前 的位置或是`flst`中p之后的位置，将元素从lst2中删除，lst2的类型必须与lst或flst相同且不能是同一个链表 |
| `(p,lst2,p2)`                                  | 将p2指向的元素移动到`lst`中或将p2之后 的元素移到到`flst`中，`lst2`可以是`lst`或`flst`相同的链表 |
| `(p,lst2,b,e)`                                 | b和e必须表示 `lst2`中的合法范围，将给定范围中的元素从 `list2`移到`lst`或`flst`，`lst2`与`lst`或`flst`可以是相同的链表，但p 不能指向给定范围中的元素 |

链特有的操作会改变容器





---







## 第11章  关联容器

| 关联容器类型         |                                   |
| -------------------- | --------------------------------- |
| 按关键字有序保存元素 |                                   |
| `map`                | 关联数组 ：保存 关键字-值 对      |
| `set`                | 关键字即值 ：只保存关键字的容器   |
| `multimap`           | 关键字可重复出现 的`map`          |
| `multiset`           | 关键字可重复出现 的`set`          |
| 无序 集合            |                                   |
| `unordered_map`      | 用哈希函数组织 的`map`            |
| `unordered_set`      | 用哈希函数 组织 的`set`           |
| `unordered_multimap` | 哈希组织 的`map`:关键字可重复出现 |
| `unordered_multiset` | 哈希组织 的`set`:关键字可重复出现 |
|                      |                                   |





###  11.1 使用关联容器

关联数组 ：`map`，通过关键字来查找 值 ，而非 下标

`set`是关键字的简单集合，适合用于查找 值 是否存在

如：作用`map`作单词计数

```cpp
map<string,size_t> word_count; // string 到 size_t 的空map
string word;
while(cin>>word){
    ++word_count[word]; // 提取word的计数器并加1
}
for(const auto &w : word_count) // w 是 pair类型
    cout<<w.first<<" occurs"<<w.second<<
    ((w.second>1)?"times":"time")<<endl;

// 如果 word_count[word]不存在，会创建一个新元素word->0
```

利用`set`忽略常见单词

```cpp
map<string,size_t> word_count;
set<string> exclude = {"the","but",...};
string word;
while(cin>>word){
    if(exclude.find(word) == exclude.end()) // 只统计不在exclude中的单词
        ++word_count[word];
}
```





### 11.2 关联容器概述

关联容器不支持顺序容器的位置相关的操作，如：`push_front`,`push_back`

#### 11.2.1 定义关联容器

对于`map`,`set`而言，关键字必须 是唯一的，而`multset`,`multmap`允许多个元素有相同的关键字

`{key,value}`

#### 11.2.2 关键字类型的要求

有序类型的关键字类型：具有严格弱序

#### 11.2.3 `pair`类型

头文件：`utility`

```cpp
make_pair(v1,v2); // 返回一个用v1和v2初始化的pair,pair的类型从v1,v2类型推断

p1 == p2; //当first和second成员分别相等，两个pair相等，相等性判断利用元素的==运算符实现
p1 != p2;
```







### 11.3 关联容器操作

| 关联容器的额外类型别名 |                                                              |
| ---------------------- | ------------------------------------------------------------ |
| `key_type`             | 此容器类型的关键字类型                                       |
| `mapped_type`          | 每个关键字关联的类型，只适用于`map`                          |
| `value_type`           | 对于`set` 与 `key_type`相同<br />对于`map`，为`pair<const key_type, mapped_type>` |



#### 11.3.1 关联容器迭代器

对`map`解引用的`value_type`是一个`pair`，其中关键字成员的值 是`const`的

`set`的迭代器是`const`的，只能读取元素，不能修改

遍历关联容器，例 ：单词计数程序 

```cpp
auto map_it = word_count.cbegin();
while(map_it != word_count.cend()){
    cout<<map_it->first<<" occurs"<<ma_it->seconde<<" times"<<endl;
    ++map_it;
}
```

关联容器和算法

```cpp
// 由于关键字是const特性，通常不对关联容器使用泛型算法
```





#### 11.3.2 添加元素

```cpp
c.insert(p,v);
c.emplace(p,args);

// 注意：p迭代器作为一个提示表示从哪里开始搜索新元素应该存储的位置
```



对`set`添加元素

```cpp
//insert 两个重载版本 , insert(beg,end)和insert({})
set<int> set2;
vector<int> ivec = {2,4,6,8,2,4,6,8};
set2.insert(ivec.cbegin(),ivec.cend()); // set2有4个元素
set2.insert({1,3,5,7,1,3,5,7}); // set 有8个元素
```

向`map`添加元素

```cpp
word_count.insert({word,1}); // 最简单
word_count.insert(make_pair(word,1));
word_count.insert(pair<string,sise_t>(word,1));
word_count.inset(map<string,size_t>::value_type(word,1)); 
```

检测`insert`的返回值 ：

​	`insert`/`emplace`的返回值 是一个`pair`，`pair.first`是一个指向具有给定关键字的元素的迭代器，	                 	`pair.second`是一个`bool`值 ，指出元素是否插入成功

​	对于向`multiset`或`multimap`添加元素的情况，无需返回一个`bool`值 





#### 11.3.3 删除元素

 

| 从关联容器删除元素 |                                                              |
| ------------------ | ------------------------------------------------------------ |
| `c.erase(K)`       | 从c 中删除关键字为k的元素，返回一个`size_type`值 ，指出删除的元素的数量 |
| `c.erase(p)`       | 从c中删除迭代器p指定的元素，返回p之后 元素的迭代器           |
| `c.erase(b,e)`     | 删除迭代器对b,e所表示 范围中的元素，返回e                    |



#### 11.3.4 `map`的下标操作

`map`和`unordered_map`容器提供了下标运算符和`at`函数 

```cpp
c[k]; // 返回关键字为k 的元素，若添加一个关键字为k的元素对其进行值 初始化，这与vector很不一样
c.at(k); // 如果 k还在c中，抛出一个out_of_range异常
```

<font color = green>注意</font> : 通常情况下，解引用一个迭代器所返回的类型和下标运算符 返回的类型是一样的，但对`map`来说，下标操作返回的是`mapped_type`对象，解引用一个迭代器返回的是`value_type(pair)`对象





#### 11.3.5 访问元素

在一个关联容器中查找元素的操作

| 下标和at操作只适用于非`const`和`map`和`unordered_map`   ？ |                                                              |
| ---------------------------------------------------------- | ------------------------------------------------------------ |
| `c.find(k)`                                                | 返回一个迭代器，指向第一个关键字为k 的元素，若k不在容器中，则返回尾后迭代器 |
| `c.count(k)`                                               | 返回关键字等于k的元素的数量 ，对于不允许重复关键字的容器，返回值 永远是0或1 |
| `lower_bound`，`upper_bound`不适用于无序容器               |                                                              |
| `c.lower_bound(k)`                                         | 返回一个迭代器，指向第一个关键字不小于k的元素                |
| `c.upper_bound(k)`                                         | 返回一个迭代器，指向第一个关键字大于k的元素                  |
|                                                            |                                                              |
| `c.equal_range(k)`                                         | 返回一个迭代器`pair`，表示 关键字等于k的元素的范围，若k不在，则pair的两个成员均等于`c.end()` |

对`map`使用`find`代替下标操作

​	当只是检查`map`容器中元素是否存在时，用`find`

遍历 某一个关键字所占元素的范围 的三种方法

```cpp
multimap<string,string> authors;
string search_item("Alain de Botton"); //要查找 的作者

// 方法1：find+count
auto entries = authors.count(search_item); // 元素的数量 
auto iter = authors.find(sarch_item); // 此作者的第一本书
while(entries){
	cout<<iter->second << endl; // 打印每个题目
    ++iter; // 前进到下一本书
    --entries; // 记录已经打了多少本书
}

// 方法二：lower_bound+upper_bound
for(auto beg = authors.lower_bound(search_item),end = authors.upper_bound(search_item); beg!=end; ++beg)
    cout<<beg->second<<endl;

// 方法三：equal_range
for(auto pos = authors.equal_range(search_item);pos.first!=pos.second;++pos.first)
    cout<<pos.first->second<<endl;
```







#### 11.3.6 一个单词转换的`map`(案例程序)

建立转换映射

```cpp
map<string,string> buildMap(ifstream &map_file){
    map<string,string> trans_map; //保存转换规则 
    string key; 	// 要转换的单词
    string value; 	// 替换后的内容
    // 读取第一个单词，行中*剩余内容*存入value
    while(map_file>>key && getline(map_file,value))
        if(value.size()>1) // 检查是否有转换规则 
            trans_map[key] = value.substr(1); // getline会读入空格，这里从1开始跳过空格
    	else
            throw runtime_error("no rule for"+key);
    return trans_map;
}
```

生成转换文本

```cpp

```

