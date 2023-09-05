# STL
## 1. STL初识
### STL基本概念
**目的：** 提升代码复用性

**定义：**

- STL(Standard Template Library, 标准模板库)
- STL从广义上分为
  - 容器(container)
  - 算法(algorithm)
  - 迭代器(iterator)
- *容器*和*算法*之间通过*迭代器*进行无缝连接
- STL几乎所有的代码都采用了模板类或者模板函数
***
**STL六大组件**

1. 容器：各种数据结构，如vector、list、deque、set、map等，用来存放数据
2. 算法：各种常用的算法，如sort、find、copy、for_each等
3. 迭代器：扮演了容器与算法之间的胶合剂
4. 仿函数：行为类似函数，可作为算法的某种策略
5. 适配器：一种用来修饰容器或者仿函数或迭代器接口的东西
6. 空间配置器：负责空间的配置与管理

***

**容器：**

    就是将运用最广泛的一些数据结构实现出来
    常用的数据结构有：数组，链表，树，栈，队列，集合，映射表等

分为两种：
1. 序列式容器：强调值的排序，序列式容器中每个元素均有固定的位置
2. 关联式容器：比如二叉树，各元素之间没有严格的物理上得顺序关系
***
**算法(Algorithms)：**

    通过有限的步骤解决逻辑或数学上的问题

分为两种：
1. 质变算法：是指运算过程中会更改区间内元素的内容，比如拷贝、替换、删除等
2. 非质变算法：是指运算过程中不会更改区间的元素内容，比如查找、计数、遍历、寻找极值等

***
**迭代器：**

    容器和算法之间的粘合剂
    提供一种方法，使之能够遍历容器内的各个元素而又无需暴露该容器的内部表示方式
    每个容器都有自己专属的迭代器

算法要通过迭代器才能访问容器中的元素
使用类似于指针，可以先理解为指针

迭代器种类：
- 输入迭代器
- 输出迭代器
- 前向迭代器
- 双向迭代器
- 随机访问迭代器

## 2.容器
### vector容器(数组)

**基础操作：**

- **包含头文件**

    `include<vector>`

- **创建容器：**

    `vector<int> v;`

- **向容器末端插入数据：**

    `v.push_back(10);`

- **创建首元素指针：**

    *pBegin解引用出来的其实就是尖括号中的数据类型

    `vector<int>::iterator pBegin = v.begin();`


- **创建末尾元素的下一个元素的指针：**

    `vector<int>::iterator pEnd = v.end();`

- **遍历vector：**

  - 方式一——自己写while循环遍历

      ```C++
      先创建好首尾元素指针
      while(pBegin != pEnd){
          cout << *pBegin++ << endl;
      }
      ```
  - 方式二——和方式一类似，用for循环简化代码

      ```C++
      for(vector<int>::iterator it = v.begin();it != v.end(); it++){
          cout << *it << endl;
      }
      ```
      ```C++
      如果想防止通过迭代器修改原vector内容，
      可以使用只读迭代器const_iterator
      void PrintVector(const deque<int> &v){
          for(deque<int>::const_iterator it = v.begin(); it!=v.end();it++){
              cout << *it << "";
          }
          cout << endl;
      }  
      ```

  - 方式三——用STL提供的标准遍历算法

      需要包含头文件algorithm

      ```C++
      #include<algorithm>
      void MyPrint(int i){
          cout << i << endl;
      }
      for_each(v.begin(), v.end(),MyPrint);
      ```

- **容器嵌套：**

    相当于二维数组

    `vector<vector<int>> v;`
***
**基本概念：**
- 功能
  - vector数据结构和数组非常相似，也称为单端数组
- 与普通函数的区别
  - vector可以动态扩展

        动态扩展并不是在原空间之后接续新空间，
        而是找更大的内存空间，
        然后将原数据拷贝新空间，释放原空间

***
**构造函数：**

- 默认构造函数

    `vector<T> v;`
    ```C++
    vector<int> v1;
    ```

- 以 *[v.begin(),v,end())* 中(前闭后开)区间中的元素创建vector

    `vector(v.begin(), v.end());`
    ```C++
    vector<int> v2(v1.begin(), v1.begin()+5);
    ```

    特别地，不止可以用另一个vector来创建vector

    ```C++
    unordered_set<int> s;
    vector<int> v(s.begin(), s.end());
    ```

- 以n个elem创建vector

    `vector(n,elem);`
    ```C++
    十个100
    vector<int> v3(10, 100);
    ```

- 拷贝构造函数

    `vector(const vector &vec);`
    ```C++
    vector<int> v4(v3);
    ```
***
**vector赋值操作：**
- 赋值运算符重载

    `vector& operator=(const vector &vec);`
    ```C++
    vector<int> v1;
    v1 = v2;
    ```

- assign( )成员函数——将 *[v.begin(),v,end())* 中(前闭后开)区间中的元素赋值给vector

    `assign(beg,end);`
    ```C++
    v1.assign(v.begin(), v.end() );
    ```

- assign( )成员函数——将n个elem赋值给vector

    `assign(n,elem);`

***
**vector容量和大小：**

- 判断容器是否为空

    `empty();`

- 获取容器的容量

    `capacity();`

- 返回容器中当前含有的元素个数

    `size();`

- 重新指定容器的长度为num
  - 若容器变长，则以elem值填充新位置(不写elem则以默认值填充新位置)
  - 若容器变短，则末尾超出容器长度的元素被删除

    `resize(int num, elem);`

**vector插入和删除**
- 尾插,尾部插入元素ele

    `push_back(ele)`

- 尾删，删除最后一个元素

    `pop_back();`

- 向迭代器指向的位置pos插入元素ele

    `insert(const_iterator pos, ele);`
    ```C++
    在整个vector的开头插入10
    vector<int> v;
    v.insert(v.begin(),10);
    ```

- 向迭代器指向的位置pos插入count个元素ele

    `insert(const_iterator pos, int count, ele);`

- 删除迭代器指向的元素

    `erase(const_iterator pos);`

- 删除迭代器从start到end之间的元素

    `erase(const_iterator start, const_iterator end);`

- 删除容器中所有元素

    `clear();`

**数据存取(读取和修改)：**
- at( )成员函数

    `at(int idx);`

- 重载[]运算符

    `operator[];`

- 返回容器中第一个数据元素

    `front();`

- 返回容器中最后一个数据元素

    `back();`


**vector互换容器：**
- 实现两个容器内的元素进行互换

    `swap(vec);`
    ```c++
    巧用swap可以收缩内存空间
    vector<int> v;
    for(int i=0;i<10000;i++){
        v.push_back(i);
    }
    v.resize(3);
    此时v的大小是3，但容量是10000+，浪费空间
    
    收缩空间具体操作：
    vector<int>(v).swap(v);
    
    前半部分 vector<int>(v) 
        通过拷贝构造创建一个匿名对象，此时该匿名对象会以v的大小3来创建
    后半部分 .swap(v)
        让这个匿名对象和原v对象交换，那么v就是大小3容量也是3，
        而匿名对象执行完后会自动释放空间
    ```
***
**vector预留空间：**
- 用来减少vector在动态扩展容量时的扩展次数
  
    预留len个元素长度，预留位置不初始化，元素不可访问(即只分配了内存)

    `reserve(int len);`
    ```C++
    查看分配100000数据到底重新分配了多少次内存
    vector<int> v;
    v.reserve(100000);
    int num = 0;  // 记录分配内存的次数
    int *p = NULL;
    for(int i=0;i<100000;i++){
        v.push_back(i);
        if(p != &v[0]){  // p不指向vector首地址说明重新分配了一次内存
            p = &v[0];
            num++;
        }
    }
    ```
***
***
### string容器(字符串)

**本质：**

- string本质上是一个类，内部封装了char*指针
- 其实就是一个char*的容器

**特点：**

- 封装了很多成员方法
- 不用担心越界问题，类内部会负责

**包含头文件：**

`#include<string>`


***
**构造函数 ：**

- 创建空字符串

    `string()`

- 使用C语言风格字符串char*来初始化字符串

    `string(const char*s)`
    ```C++
    const char* str = "hello world";
    string s(str);
    ```
- 拷贝构造，复制一个字符串

    `string(const string& str)`
    ```C++
    string s1(s);
    ```

- 直接将n个字符组成字符串

    `string(int n, char c)`
    ```C++
    string s(5,'a');
    生成字符串为"aaaaa"，只能是一个字符重复几次
    ```
***
**赋值操作：**


- 赋值运算符重载
  - `string& operator=(const char* s);`
    
    将char*类型字符串赋值给当前字符串
    ```C++
    string s1;
    s1 = "HelloWorld!";
    ```

  - `string& operator=(const string &s);`
    
    将字符串s赋值给当前字符串
    ```C++
    string s2;
    s2 = s1;
    ```

  - `string& operator=(char c);`

    将字符赋值给当前的字符串
    ```C++
    string s3;
    s3 = 'a';
    ```
- 成员函数assign()
  - `string& assign(const char *s, int n);`

    把char*类型字符串前n个字符赋给当前字符串，不写n是赋值全部字符
    ```C++
    string str1;
    str1.assign("Hello C++", 5);
    ```

  - `string& assign(const string &s);`

    把字符串s赋值给当前字符串
    ```C++
    string str2;
    str2.assign(str1);
    ```

  - `string& assign(int n, char c);`

    用n个字符c赋给当前字符串
    ```C++
    string str3;
    str3.assign(10,'w');
    ```
***
**字符串拼接操作：**

- 重载+=运算符
  - `string& operator+=(const char* str);`
  - `string& operator+=(const char c);`
  - `string& operator+=(const string& str);`

    ```C++
    string str1 = "我";
    str1 += "爱你";
    str1 += '!';
    ```

- append()成员函数
  - `string& append(const char *s);`
  - `string& append(const char *s, int n);`

    将前n个字符连接到当前字符串结尾

  - `string& append(const string &s);`
  - `string& append(const string &s, int pos, int n)`

    将字符串s中从pos开始(从0开始数)的n个字符连接到字符串结尾
    ```C++
    string str = "I";
    string str1 = "I LOL";
    str.append(str1, 2,3); // 截取LOL
    ```
***
**字符串查找和替换操作：**

- find( )函数
  - 查找str第一次出现的位置，从pos开始从左往右查找

    返回第一次出现的位置，从0开始索引，没有的话返回-1

    `int find(const string& str, int pos = 0);` 从pos开始查找str第一次出现的位置

    `int find(const char*s, int pos = 0);`

    `int find(const char*s, int pos, int n);` 从pos位置开始查找s的前n个字符第一次出现的位置

    `int find(const char c, int pos = 0);`查找字符c第一次出现的位置

- rfind( )函数
  - 和find的区别就是从右往左找

    `int rfind(const string& str, int pos = npos);`

    `int rfind(const char* s, int pos = npos);`

    `int rfind(const char c, int pos, int n);`

    `int rfind(const char c, int pos = 0);`查找字符c最后一次出现的位置

- replace( )函数
  - 替换从pos开始(从0开始索引)的n个字符为字符串str

    `string& replace(int pos, int n, const string& str);`

    `string& replace(int pos, int n, const char* s);`
***
**字符串的比较：**

- 函数原型

    `int compare(const string&s);`

    `int compare(const char* s);`

- 比较方式：
  - 按照字符的ascii码进行对比

    相等：返回0

    调用的字符串大于传入的字符串：返回1
    
    调用的字符串小于传入的字符串：返回-1
***
**单个字符存取(读写)**

- 重载中括号

    `char& operator[](int n)`
    ```C++
    stirng str("HelloWorld");
    cout << str[0] << endl;
    str[0] = 'x';
    ```

- at()成员函数

    `char& at(int n);`
    ```C++
    cout << str.at(0) << endl;
    str.at(0) = 'x';
    ```
***
**字符串的插入和删除**

- insert( )插入函数

  - 在第pos个位置(从0开始索引)插入字符串

    `string& insert(int pos, const char* s);`

    `string& insert(int pos, const string& str);`

    `string& insert(int pos, int n, char c);`在指定位置插入n个字符c
    ```C++
    string str("Hello");
    str.insert(1,"111");
    str === "H111ello";
    ```

- erase( )删除函数

  - 删除从Pos开始(从0开始索引)的n个字符
    `string& erase(int pos, int n = npos);`
    ```C++
    str.erase(1,3);
    ```
***
**获取子串：**
- 返回由pos开始(从0开始索引)的n个字符组成的字符串

    `string substr(int pos = 0, int n = npos);`
    ```C++
    从邮箱中获取用户名：
    string email = "zhangsan@sina.com";
    string user_name = email.substr(0,emial.find('@'));
    ```

***
***
### deque容器

**基本概念：**
- 双端数组，可以对头端进行插入删除操作
- 在头部操作数据比vector快，访问数据没有vector快
- 由内部的中控器维护每段缓冲区，缓冲区中存放数个真实数据
- 中控器是一个数组，这个数组中存放的是每个缓冲区的地址
***
**包含头文件：**

`#include<deque>`
***

**构造函数：和vector类似**
- 默认构造

    `deque<T> deqT;`

- 将[begin,end)区间中的元素拷贝给本身(bgein和end是迭代器)

    `deque(begin,end);`

- 将n个elem拷贝给本身构造

    `deque(n,elem);`

- 拷贝构造

    `deque(const deque& deq);`
***
**赋值操作：和vector类似**
- 重载赋值运算符

    `deque& opoerator=(const deque &deq);`

- 将[beg,end)区间中的数据拷贝赋值

    `assign(beg,end);`

- 将n个elem拷贝赋值

    `assign(n,elem);`
***
**大小操作：**
- 获取当前元素个数

    `deque.size();`

- 判断是否为空

    `deque.empty();`

- 重新指定大小

    `deque.resize(int n, elem);`

- 没有deque.capacity()方法，因为deque没有容量的概念
***
**插入和删除：与vector相同**
- `push_back(elem);`尾插
- `push_front(elem);`头插
- `pop_back();`尾删
- `pop_front();`头删
- `insert(pos,elem);`pos是迭代器,返回新数据的位置
- `insert(pos,n,elem);`
- `insert(pos,beg,end);`beg,end是迭代器
- `clear();`
- `erase(beg,end);`返回下一个数据的位置
- `erase(pos);`返回下一个数据的位置
***
**数据存取**
- at(int index)
- 重载[]运算符
- `deque.front()`访问头部元素
- `deque.back()` 访问尾部元素
***
**deque排序：**
- 利用算法实现对deque容器的排序,默认是升序，从小到大

    `sort(iterator beg, iterator end);`

***
***
### stack容器

**概念：** 栈，先进后出(First In Last Out)

**包含头文件：**

`#include<stack>`

**构造函数：**
- 默认构造

    `stack<T> stk;`

- 拷贝构造函数

    `stack(const stack &stk);`

**赋值操作：**
- 重载等号操作符

    `stack& operator=(const stack &stk);`

**数据存取：**
- 入栈

    `push(elem)`

- 出栈

    `void pop()`

- 获取栈顶元素
  
    `top()`

**获取栈的大小：**
- 判断栈是否为空

    `empty()`

- 返回栈的大小

    `size()`
***
***
### queue容器
**概念：** 队列，先进先出(First In First Out)。队头：front，队尾：back

**包含头文件：**

`#include<queue>`

**构造函数：**
- 默认构造

    `queue<T> que;`

- 拷贝构造

    `queue(const queue &que);`

**赋值操作：**
- 重载等号运算符

    `queue& operator=(const queue &que);`

**数据存取：**
- 入队

    `push(elem)`

- 出队

    `void pop()`

- 返回队尾(最后一个元素)

    `back();`

- 返回队头(第一个元素)

    `front();`

**大小操作：**
- 判断堆栈是否为空

    `empty()`

- 返回栈的大小

    `size()`
***
***
### list容器
**基本概念：** 
- 组成：由一系列结点组成
- 结点：包括数据域和指针域
- STL中的链表是双向循环链表

**包含头文件：**

`#include<list>`

**迭代器：**

是双向迭代器，只能前移一位和后移一位，不支持随机读取
- 迭代器
  
    `list<T>::iterator it;`

- 只读迭代器

    `list<T>::const_iterator it;`
***
**构造函数：**
- 默认构造

    `list<T> lst;`

- beg和end是两个迭代器

    `list(beg,end);`

- 将n个elem拷贝给list

    `list(n,elem)`

- 拷贝构造

    `list(const list &lst)`
***
**赋值和交换：**
- 重载等号运算符

    `list& operator=(const list &lst)`

- assign() 成员函数

    `assign(beg,end);`

    `assign(n,elem);`

- 将lst与本身的元素互换

    `swap(lst);`
***
**list大小操作：**
- 返回容器中元素的个数

    `size()`

- 判断容器是否为空

    `empty()`

- 重新指定容器的长度

    `resize(num)`

    `resize(num,elem)`

**插入和删除数据：** 
- 尾插

    `push_back(elem)`

- 尾删

    `pop_back()`

- 头插

    `push_front()`

- 头删

    `pop_front()`

- 指定位置插入

    `iterator insert(pos,elem)` 返回指向插入元素的迭代器

    `void insert(pos, n, elem)`

    `void insert(pos, beg, end)`

- 清除容器中所有数据

    `clear()`

- 删除指定位置数据

    `iterator erase(iterator beg, iterator end)`返回下一个数据的位置

    `iterator erase(iterator pos)`删除pos位置的数据，返回下一个数据的位置

- 移除容器中所有与elem值匹配的元素

    `remove(T elem);`
***
**数据存取：**

    list容器受底层链表结构的限制，迭代器不能随机访问
    所以不支持 at(index) 和 重载中括号 这种直接通过索引来读取数据

- 返回第一个元素

    `front()`

- 返回最后一个元素

    `back()`

- 不支持随机(跳跃式)访问

    ```C++
    list<int>::iterator it;
    it++;  // 支持
    it = it + 1;  // 不支持，报错
    ```
***
**反转和排序：**
- 反转链表,成员函数reverse()

    `reverse()`

- 排序链表，成员函数sort()

    不是标准算法中的sort算法


    - 升序(默认)
        
        `sort()`
    
    - 降序
    
        ```C++
        myCompare(int v1, int v2){
            return v1 > v2;
        }
    
        ...
    
        sort(myCompare);
        ```
    
    - 如果list的数据类型是自定义数据类型也要传入回调函数
***
***
### set/multiset容器
**基本概念：**
- 特点：集合，所有元素都会在插入时自动被排序，set中的值不能更改

    特别地：unordered_set不会排序，需要引入`#include<unordered_set>`，底层结构是哈希表

- 本质：set/multiset属于*关联式容器*，底层结构是红黑树实现

- 区别：
  - set不允许容器中有重复的元素
  - multiset允许容器中有重复的元素

- 包含头文件

    `#include<set>`

- 迭代器

    `set<T>::iterator it;`
    
    map和set都不能对end迭代器解引用
***
**构造与赋值：**
- 默认构造函数

    `set<T> st;`

- 拷贝构造函数

    `set(const set &st);`

- 赋值，重载等号操作符

    `set& operator=(const set &st);`
***
**大小和交换：**
- 返回容器中元素的数目

    `int size();`

- 判断容器是否为空

    `bool empty();`

- 交换两个集合容器

    `swap(st);`

**插入和删除：**

    在容器中没有提供尾插、尾删、头插、头删的方法
- 插入元素

    `insert(elem);`

- 清除所有元素

    `clear();`

- 删除元素

    `iterator erase(pos);` 删除pos迭代器所指的元素，返回被删除元素的下一个元素的迭代器

    `iterator erase(beg, end);`删除区间(左闭右开)内所有元素，返回下一个元素的迭代器

    `erase(elem);`删除容器中值为elem的元素
***
**查找和统计：**
- 查找key是否存在，返回指向该元素的迭代器，若不存在，返回set.end()

    `find(key)`
    ```C++
    set<int> s;

    ...

    set<int>::iterator pos = s.find();
    if(pos != s.end()){
        cout << "找到了" << *pos << endl;
    }
    ```

- 统计key的元素个数，对于set只会返回 0 或 1

    `count(key)`
***
**set和multiset的区别：**
- set不可以插入重复数据，而multiset可以
- set插入数据的同时会返回结果，表示插入是否成功

    `pair<iterator,bool>`返回的数据类型
    ```C++
    set<int> s;
    pair<set<int>::iterator, bool> ret = s.insert(10);
    
    if(ret.second){
        cout << "插入成功" << endl;
    }
    ```
- multiset不会检测数据，只会返回指向插入位置的迭代器

***
**pair对组的创建：**
- 成对出现的数据，利用对组可以返回两个数据
- 不需要包含头文件
- 创建方式

    `pair<type, type> p (value1, value2);`

    `pair<type, type> p = make_pair(value1, value2);`

    ```C++
    pair<string, int> p ("Tom", 20);
    p.first;  
    p.second;
    ```
***
**set容器排序：**

    set容器默认排序规则为从小到大，
    利用仿函数(重载小括号)改变排序规则
- 内置数据类型

    ```C++
    class MyCompare{
    public:
        bool operator()(int v1, int 2){
            return v1 > v2; //降序排列
        }
    }
    
    ...
    
    set<int, MyCompare> s2;
    ```
- 自定义数据类型，必须要指定排序规则

    ```C++
    class Person{
    public:
        string name;
        int age;
        ...
    };
    
    class comparePerson{
    public:
        bool operator()(const Person &p1, const Person &p2){
            return p1.age > p2.age; // 年龄降序
        }
    };
    
    ...
    
    set<Person, comparePerson> s;
    
    ...
    
    for(set<Person, comparePerson>::iterator it = s.begin();it!=s.end();it++){
        ...
    }
    ```
***
***
### map/multimap容器
**基本概念：**
- map中所有元素都是pair——字典，以键值对的形式存储数据，第一个元素为key(键值)，第二个元素为value(实值)
- 所有元素根据元素的键自动排序
- 属于关联式容器，底层结构是红黑树
- 特别地：unordered_map不会排序，底层是哈希表

**优点：**
- 可以根据key值快速找到value值

**区别：**
- map不允许容器中有重复的key，multimap中允许

**包含头文件：**

`include<map>`

**迭代器：**

`map<T1,T2>::iterator it;`
***
**构造和赋值：**
- 默认构造

    `map<T1, T2> mp;`

- 拷贝构造

    `map(const map &mp);`

- 赋值：重载等号操作符

    `map& operator=(const map &mp);`
***
**遍历**

```C++
unordered_map<int, int> mp;
for(auto& [k,v] : mp){
    cout << k << v << endl;
}
```

***
**大小和交换：**
- 返回容器中元素的个数

    `size()`

- 判断容器是否为空

    `bool empty()`

- 交换两个集合容器

    `swap(mp);`
***
**插入和删除：**
- 在容器中插入元素

    `insert(elem)`
    ```C++
    map<int,int> m;
    m.insert(pair<int,int>(1,10));

    m.insert(make_pair(2,10));

    m.insert(map<int,int>::value_type(3,30));

    m[4] = 40;
    []不建议插入，可以通过[]来利用key访问value
    即 m[key] = value;
    但是如果key不存在会创建出来一个value为0的元素
    ```

- 清除所有元素

    `clear()`

- 删除pos迭代器所指的元素，返回下一个元素的迭代器

    `erase(pos)`

- 删除区间[beg,end)的所有元素，返回下一个元素的迭代器

    `erase(beg,end);`

- 删除容器中键为key的元素

    `erase(key);`
***
**查找和统计：**
- 查找：存在返回指向该键的迭代器，不存在返回set.end()

    `find(key)`

- 统计键为key的元素个数,对于map结果为 0 或 1

    `count(key)`
***
**map容器排序：**

    map容器默认排序规则为按照key值从小到大排序
    利用仿函数可以改变排序规则

```C++
class MyCompare{
public:
    bool operator()(int v1, int v2){
        return v1 > v2; // 降序排列
    }
}

...

map<int,int , MyCompare> m;
```

## 3. 适配器

### priority_queue

```C++
template<
	class T,
	class Container = std::vector<T>,
	class Compare = std::less<typename Container::value_type>
>
        
priority_queue<int, vector<int>, greater<int>> pq;
```

在容器上面套一个优先队列的壳子，只能拿到最后一个元素，即top()元素。默认用less从小到大排序，只能拿到最大的元素

- top()：获取队头元素
- pop()：移除队头元素
- push()：插入元素，插入后会自动重新排序

 
