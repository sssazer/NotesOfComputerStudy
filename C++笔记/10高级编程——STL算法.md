## 3.STL-函数对象
### 函数对象
**函数对象概念：**
- 重载函数调用操作符的类，其对象称为函数对象
- 函数对象使用重载的()时，行为类似函数调用，也叫仿函数

    ```C++
    class Add{
    public:
        int operator()(int num1, int num2){
            return num1 + num2;
        }
    }

    ...

    Add myAdd;  // myAdd就是函数对象
    myAdd(10,20);
    ```

**本质：**
- 函数对象(仿函数)本质是一个类，不是一个函数

**函数对象特点：**
- 函数对象在使用时，可以像普通函数那样调用，可以有参数，可以有返回值
- 函数对象超出普通函数的概念，函数对象可以有自己的状态

    ```C++
    class MyPrint{
    public:
        int count; // 用于记录函数被调用了多少次，
        //即记录函数的状态

        MyPrint(){
            this->count = 0;
        }

        void operator()(string info){
            cout << info << endl;
            count++;
        }
    }
    ```
- 函数对象可以作为参数传递

    ```C++
    依旧用上面的MyPrint类：
    void doPrint(MyPrint& mp, string test){
        mp(test);
    }

    ...

    MyPrint myPrint;
    doPrint(myPrint, "Hello C++");
    ```
***
***
## 谓词
**谓词概念：**
- 返回bool类型的仿函数称为谓词(Pred)
- 如果operator()接受一个参数，那么叫做一元谓词

    ```C++
    find_if()函数：查找是否有符合要求的元素
    class Compare{
    public:
        bool operator()(int val){
            return val>5;
        }
    }

    ...

    vector<int> v;

    ...

    find_if(v.begin(),v.end(),Compare()); //Compare()是一个匿名对象

    这个函数会返回找到的第一个符合条件的迭代器，找不到的话返回v.end()
    ```
- 如果operator()接受两个参数，那么叫做二元谓词

    ```C++
    class Compare{
    public:
        bool operator()(int num1, int num2){
            return num1>num2;
        }
    }
    ```
***
***
## 内建函数对象
**概念：**

- STL内建的一些函数对象

**分类：**

- 算术仿函数
- 关系仿函数
- 逻辑仿函数

**包含头文件：**

`#include<functional>`
***
**算术仿函数**

`template<class T>` 
- 加法仿函数(二元仿函数)

    `T plus<T>`
    ```C++
    plus<int> p; // 两个相加数必须是同一类型
    p(10,20);  // 返回30
    ```

- 减法仿函数

    `T minus<T>`

- 乘法仿函数

    `T multiplies<T>`

- 除法仿函数

    `T divides<T>`

- 取模仿函数

    `T modulus<T>`

- 取反仿函数(一元仿函数)

    `T negate<T>`
    ```C++
    negate<int> n;
    n(50);  // 返回-50
    ```
***
**关系仿函数**

`template<class T>`
- 大于

    `bool greater<T>`
    ```C++
    vector<int> v;

    插入元素...

    sort(v.begin(),v.end(),greater<int>()); // greater()是匿名对象
    ```

- 大于等于

    `bool greater_eaque<T>`

- 等于

    `bool equal_to<T>`

- 不等于

    `bool not_equal_to<T>`

- 小于

    `bool less<T>`

- 小于等于

    `less_equal<T>`
***
**逻辑仿函数** 使用比较少，了解即可

`template<class T>`

- 逻辑与

    `bool logical_and<T>`

- 逻辑或

    `bool logical_or<T>`

- 逻辑非

    `bool logical_not<T>`
***
***
## STL-常用算法
**概述：**
- 算法由头文件`<algorithm>` `<functional>` `numeric`组成
- `<algorithm>`是所有STL头文件中最大的一个，范围涉及到比较、交换、查找、遍历、复制、修改等
- `<numeric>` 体积很小，只包括几个在序列上面进行简单数学运算的模板函数
- `<functional>`定义了一些模板类，用以声明函数对象

### 常用遍历算法
- for_each 遍历容器 

    `for_each(iterator beg, iterator end, _func);`
    - beg——开始迭代器
    - end——结束迭代器
    - _func——函数或者函数对象(仿函数)，遍历要进行的操作

    ```C++
    仿函数：
    class Print1{
    public:
        void operator()(int val){
            cout << val << " ";
        }
    }

    普通函数：
    void Print2(int val){
        cout << val << " ";
    }

    vector<int> v;

    插入数据...

    for_each(v.begin(),v.end(),Print1());//传入仿函数

    for_each(v.begin(),v.end(),Print2);//传入普通函数
    ```

- transform 搬运容器中内容到另一个容器中

    `transform(iterator beg1, iterator end1, iterator beg2, _func);`
    - beg1——原容器的开始迭代器
    - end1——原容器的结束迭代器
    - beg2——目标容器的开始迭代器，目标容器一定要有足够的空间
    - _func()——函数或者仿函数，搬运时要进行的操作

    ```C++
    仿函数：不做任何操作
    class Transform{
    public:
        int operator()(int val){
            return val;
        }
    }

    vector<int> v;

    插入数据...

    vector<int> vTarget;
    vTarget.resize(v.size()); // 不开辟空间程序会崩溃
    transfrom(v.begin(),v.end(),vTarget.begin(),Transfrom());
    ```
***
### 常用查找算法
**find 查找指定元素**

找到返回指定元素的迭代器，找不到返回结束迭代器end()

自定义数据类型要重载"=="

`find(iterator beg, iterator end, value);`
- beg——开始迭代器
- end——结束迭代器
- value——要查找的元素
```C++
class Person{
public:
    string name;
    int age;
    Person(string name, int age){
        this->name - name;
        this->age = age;
    }

    bool operator==(Person p){
        if(this->name == p.name && this->age == p.age){
            return true;
        }
        return false;
    }
}

...

vector<Person> v;

...

find(v.begin(),v.end(),p1);
```
***
**find_if 按条件查找元素**

按_Pred指定的规则查找元素，找到第一个符合要求的元素就返回该元素的迭代器，找不到返回结束迭代器

`find_if(iterator beg, iterator end, _Pred);`
- beg——开始迭代器
- end——结束迭代器
- _Pred——函数或者谓词(返回bool类型的仿函数)

```C++
class Person{
public:
    string name;
    int age;
    ...
}
class Greater20{
public:
    bool operator()(Person p){
        return p.age > 20;
    }
}

...

vector<Person>::iterator it = find_if(v.begin(),v.end(),Greater20());//判断是否有年龄大于20岁的人
```
***
**adjacent_find 查找相邻重复元素**

查找相邻重复元素，找到的话返回相邻元素的第一个元素的迭代器，找不到返回结束迭代器

`adjacent_find(iterator beg, iterator end);`
- beg——开始迭代器
- end——结束迭代器
***
**binary_search 查找指定元素是否存在，二分查找**

查找指定的元素，查到返回true，查不到返回false

*注意：在无序序列中不可以使用*、

`bool binary_search(iterator beg, iterator end, value);`
- beg——开始迭代器
- end——结束迭代器
- value——要查找的元素
***
**count 统计元素个数**

返回元素的个数

自定义数据类型要重载"==",而且重载的时候底层要求必须加const`bool operator==(const Person & p);`

`int count(iterator beg, iterator end, value);`
- beg——开始迭代器
- end——结束迭代器
- value——要统计的元素
***
**count_if 统计符合条件的元素个数**

返回符合条件元素的个数，int类型

`int count_if(iterator beg, iterator end, _Pred);`
- beg——开始迭代器
- end——结束迭代器
- _Pred——谓词
***
***
## 常用的排序算法
### sort 排序

`sort(iterator beg, iterator end, _Pred);`
- beg——开始迭代器
- end——结束迭代器
- _Pred——谓词，不填默认为less，即从小到大

***
### random_shuffle 洗牌

指定范围内的元素随机调整顺序

`random_shuffle(iterator beg, iterator end);`
- beg——开始迭代器
- end——结束迭代器
***
### merge 合并
将两个容器中的有序序列合并并存储到新的容器中，合并后的序列依然有序

必须提前给目标容器分配足够的空间`resize(v1.size()+v2.size());`

`merge(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`
- beg1——容器1的开始迭代器
- end1——容器1的结束迭代器
- beg2——容器2的开始迭代器
- end2——容器2的结束迭代器
- dest——目标容器开始迭代器
***
### reverse 反转
`reverse(iterator beg, iterator end);`
- beg——开始迭代器
- end——结束迭代器
***
***
## 拷贝和替换算法
### copy 拷贝
容器内指定范围的元素拷贝到另一容器中

目标容器要提前开辟空间

`copy(iterator beg, iterator end, iterator dest);`
- beg——开始迭代器
- end——结束迭代器
- dest——目标容器的起始迭代器

没啥用，可以直接使用拷贝构造或者重载等号赋值操作
***
### replace 替换
将容器内指定范围的所有指定旧元素修改为新元素

`replace(iterator beg, iterator end, oldvalue, newvalue);`
- beg——开始迭代器
- end——结束迭代器
- oldvalue——旧元素
- newvalue——新元素
***
### replace_if 按条件替换
将区间内满足条件的所有元素替换成指定的新元素

`replace_if(iterator beg, iterator end, _Pred, newvalue);`
- beg——开始迭代器
- end——结束迭代器
- _Pred——谓词，需要替换的元素
- newvalue——要替换的新元素
***
### swap 交换
互换两个相同类型的容器

`swap(container c1, container c2);`
- c1——容器1
- c2——容器2
***
***
## 常用的算术生成算法
- 算术生成算法属于小型算法
- 包含头文件

    `#include<numeric>`
### accumulate 总和
计算容器指定区间内所有元素的总和

`accumulate(iterator beg, iterator end, value);`
- beg——开始迭代器
- end——结束迭代器
- value——起始累加值，一般写0就行

    ```C++
    vector<int> v;

    ...

    accumulate(v.begin(),v.end(),0);
    ```
***
### fill 填充
向容器中填充指定的元素

`fil(iterator beg, iterator end, value);`
- beg——开始迭代器
- end——结束迭代器
- value——要填充的值
***
***
## 常用集合算法

## set_intersection 求交集

返回指向交集最后一个元素位置的迭代器

两个容器中的集合必须是有序序列

目标容器必须提前开辟好空间`target.resize(min(v1.size(),v2.size()))`

`set_intersection(iterator beg1, iterator end1, beg2, end2, dest);`

```C++
vector<int> v1;
vector<int> v2;
vector<int> target;

target.resize(min(v1,v2)); //min需要包含头文件 algorithm
vector<int>::iterator itEnd = set_intersection(v1.begin(),v1.end(),v2.begin(),v2.end(),target.begin());

for_each(target.begin(),itEnd,Print);//要用返回的结束迭代器，不然会把后面的默认数据也打印出来
```
***
### set_union 求并集

返回指向并集最后一个元素位置的迭代器

两个集合必须都是有序序列

目标容器必须提前开辟好空间`target.resize(v1.size()+v2.size());`

`set_union(iterator beg1, iterator end1, beg2, end2, dest);`
***
### set_diffience 求差集

- V1和V2的差集是指V1中有而V2中没有的元素
- V2和V1的差集是指V2中有而V1中没有的元素
- 所以说参数中谁是第一个容器很重要

返回指向差集最后一个元素位置的迭代器

两个集合必须都是有序序列

目标容器必须提前开辟好空间`target.resize(v1.size());`

`set_difference(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`


