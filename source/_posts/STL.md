---
title: STL
date: 2021-02-04 16:01:36
categories: 
tags: 
	- STL
	- C++
---

STL是一个C++模板库，里面包含算法（algorithms）、容器（containers）、函数（functions）、迭代器（iterator）

<!--more-->

### 容器

使用容器时要在头文件中引入

#### 序列式容器

**序列的元素的位置是由进入容器的时间和地点决定的**

##### vector

vector简单点说就是一个动态数组，vector实现动态增长，当插入新元素的时候，如果空间不足，那么vector会重新申请更大的一块内存空间。

````c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <vector>
using namespace std;

void PrintVector(vector<int>& v) {
    for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
        cout << *it << " ";
    }

    cout << endl;
}

//初始化
void test01() {
    vector<int> v1;  //默认构造

    int arr[] = {10, 20, 30, 40};
    vector<int> v2(arr, arr + sizeof(arr) / sizeof(int));
    vector<int> v3(v2.begin(), v2.end());
    vector<int> v4(v3);

    PrintVector(v2);
    PrintVector(v3);
    PrintVector(v4);
}

//常用赋值操作
void test02() {
    int arr[] = {10, 20, 30, 40};
    vector<int> v1(arr, arr + sizeof(arr) / sizeof(int));

    //成员方法
    vector<int> v2;
    v2.assign(v1.begin(), v1.end());

    //重载=
    vector<int> v3;
    v3 = v2;

    int arr1[] = {100, 200, 300, 400};
    vector<int> v4(arr1, arr1 + sizeof(arr1) / sizeof(int));

    PrintVector(v1);
    PrintVector(v2);
    PrintVector(v3);
    PrintVector(v4);

    cout << "---------------" << endl;
    //交换
    v4.swap(v1);
    PrintVector(v1);
    PrintVector(v2);
    PrintVector(v3);
    PrintVector(v4);
}

//大小操作
void test03() {
    int arr[] = {100, 200, 300, 400};
    vector<int> v1(arr, arr + sizeof(arr) / sizeof(int));

    cout << "size: " << v1.size() << endl;
    if (v1.empty()) {
        cout << "空" << endl;
    } else {
        cout << "不空" << endl;
    }

    PrintVector(v1);
    v1.resize(2);
    PrintVector(v1);
    v1.resize(6, 1);  //不写默认零
    PrintVector(v1);

    for (int i = 0; i < 10000; i++) {
        v1.push_back(i);
    }

    cout << v1.size() << endl;      //长度、大小
    cout << v1.capacity() << endl;  //容量
}

//vector存取数据
void test04() {
    int arr[] = {100, 200, 300, 400};
    vector<int> v1(arr, arr + sizeof(arr) / sizeof(int));

    for (int i = 0; i < v1.size(); i++) {
        cout << v1[i] << " ";
    }
    cout << endl;

    cout << "-----------" << endl;
    for (int i = 0; i < v1.size(); i++) {
        cout << v1.at(i) << " ";
    }
    cout << endl;

    cout << "front: " << v1.front() << endl;  //第一个元素
    cout << "back: " << v1.back() << endl;    //最后一个元素
}

//插入和删除
void test05() {
    vector<int> v;
    v.push_back(10);
    v.push_back(20);

    //头插法
    v.insert(v.begin(), 30);

    //尾插法
    v.insert(v.end(), 40);

    PrintVector(v);

    v.insert(v.begin() + 2, 100);  //插到第二个位置
    //vector支持随机访问

    //支持数组下标，一般都支持随机访问
    //迭代器可以直接+-操作

    PrintVector(v);

    //删除
    v.erase(v.begin());
    PrintVector(v);

    v.erase(v.begin() + 1, v.end());
    PrintVector(v);

    v.clear();
    cout << "size: " << v.size() << endl;
}

//巧用swap缩减空间
void test06() {
    //vector添加元素 自动增长 那么，删除元素的时候，会自动减少吗

    vector<int> v;
    for (int i = 0; i < 100000; i++) {
        v.push_back(i);
    }

    cout << "size: " << v.size() << endl;
    cout << "capacity: " << v.capacity() << endl;

    v.resize(10);

    cout << "----------" << endl;
    cout << "size: " << v.size() << endl;
    cout << "capacity: " << v.capacity() << endl;

    //收缩空间
    vector<int>(v).swap(v);
    cout << "----------" << endl;
    cout << "size: " << v.size() << endl;
    cout << "capacity: " << v.capacity() << endl;
    PrintVector(v);
}

void test07() {
    //reserve预留空间 resize区别
    int num = 0;
    int* address = NULL;
    vector<int> v;

    v.reserve(100000);  //预先分配
    for (int i = 0; i < 100000; i++) {
        v.push_back(i);
        if (address != &v[0]) {
            num++;
        }
    }

    cout << num << endl;

    //如果你知道容器大概需要的空间，预先分配空间，可以减少时间浪费吗，提高程序运行效率
}
int main() {
    //test01();
    //test02();
    //test03();
    //test04();
    //test05();
    //test06();
    test07();
    return 0;
}
````



##### deque

deque表示双端队列，即可以从前端或者后端这两端进行数据的插入和删除

* 分段连续的内存空间
* 支持随机访问
* 指定位置插入，会引起数据移动



```c++
#define _CRT_SECURE_NO_WARNINGS
#include <deque>
#include <iostream>
using namespace std;

void PrintDeque(deque<int>& d) {
    for (deque<int>::iterator it = d.begin(); it != d.end(); it++) {
        cout << *it << " ";
    }
}

//初始化
void test01() {
    deque<int> d1;
    deque<int> d2(10, 5);
    deque<int> d3(d2.begin(), d2.end());
    deque<int> d4(d3);

    //打印
    PrintDeque(d4);
    cout << endl;
}

//赋值、大小操作
void test02() {
    deque<int> d1;
    deque<int> d2;
    deque<int> d3;
    d1.assign(10, 5);
    d2.assign(d1.begin(), d1.end());  //迭代器指定区间赋值
    d3 = d2;                          //等号赋值

    d1.swap(d2);  //交换两个空间的元素

    if (d1.empty()) {
        cout << "空" << endl;
    } else {
        cout << "不空" << endl;
    }

    d1.resize(5);  //有十个，后五个扔掉
}

//插入和删除
void test03() {
    deque<int> d1;
    d1.push_back(100);
    d1.push_front(200);
    d1.push_back(300);
    d1.push_back(400);
    d1.push_front(500);
    //500 200 100 300 400

    PrintDeque(d1);

    int val = d1.front();  //拿到第一个数据
    d1.pop_front();        //删除第一个，无返回值

    val = d1.back();
    d1.pop_back();  //删除最后一个元素
}

int main() {
    //test01();
    //test02();
    test03();
    return 0;
}
```



##### list

- 双向链表
- 链表是由一系列节点组成的，节点包括两个域，一个数据域，一个指针域。
- 链表内存是非连续的，添加删除元素 时间复杂度是常数项 不需要移动元素
- 链表需要额外空间保留节点关系
- 不支持随机访问，故不能使用算法提供的sort()来进行排序，但可以调用其成员方法



```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <list>
using namespace std;

//初始化
void test01() {
    list<int> l1;
    list<int> l2(10, 10);
    list<int> l3(l2);
    list<int> l4(l3.begin(), l3.end());

    //打印
    for (list<int>::iterator it = l4.begin(); it != l4.end(); it++) {
        cout << *it << " ";
    }

    cout << endl;
}

//插入和删除
void test02() {
    list<int> l;
    l.push_back(100);
    l.push_front(200);
    l.insert(l.begin(), 300);
    l.insert(l.end(), 200);

    list<int>::iterator it = l.begin();
    it++;
    l.insert(it, 500);

    //删除
    //l.pop_back();
    //l.pop_front();

    //l.erase(l.begin(), l.end());

    l.remove(200);  //删除匹配的所有值

    for (list<int>::iterator it = l.begin(); it != l.end(); it++) {
        cout << *it << " ";
    }

    cout << endl;
}

//赋值操作
void test03() {
    list<int> l;
    l.assign(10, 10);

    list<int> l2;
    l2 = l;

    list<int> l3;
    l3.swap(l);
}

//反转
void test04() {
    list<int> l;
    for (int i = 0; i < 10; i++) {
        l.push_back(i);
    }

    for (list<int>::iterator it = l.begin(); it != l.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;

    l.reverse();  //容器元素反转

    for (list<int>::iterator it = l.begin(); it != l.end(); it++) {
        cout << *it << " ";
    }
}

bool Mycompare(int val1, int val2) {
    return val1 > val2;
}

//排序
void test05() {
    list<int> l;
    for (int i = 0; i < 10; i++) {
        l.push_back(rand() % 10);
    }
    for (list<int>::iterator it = l.begin(); it != l.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;

    l.sort();

    for (list<int>::iterator it = l.begin(); it != l.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;

    //从大到小
    l.sort(Mycompare);

    for (list<int>::iterator it = l.begin(); it != l.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;

    //算法sort  只支持可随机访问的容器 链表没有
    //list的sort是自己的成员函数不是算法
}
int main() {
    //test01();
    //test02();
    //test04();
    test05();

    return 0;
}
```

##### queue

- 先进先出
- 不提供迭代器，不能遍历，不支持随机访问
- push 入队（队尾）
- pop 出队（队头）

```c++
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<queue>
using namespace std;

void test01() {
	queue<int> q;//创建队列

	q.push(10);
	q.push(20);
	q.push(30);
	q.push(40);

	cout << "队尾元素: " << q.back() << endl;

	//输出顺序 10,20,30,40
	while (q.size() > 0){
		cout << q.front() << " ";//输出队头
		q.pop();//删除队头
	}
}

int main() {
	test01();
	return 0;
}
```



##### stack

- 先进后出
- push 压栈
- pop 出栈
- 栈不提供迭代器，不能遍历，不支持随机访问，只能通过top从栈顶获取和删除元素

```c++
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<stack>
using namespace std;

void test01(){

	//初始化
	stack<int> s1;
	stack<int> s2(s1);

	//stack操作
	s1.push(10);
	s1.push(20);
	s1.push(30);
	s1.push(100);
	cout << "栈顶元素：" << s1.top() << endl;
	s1.pop();//删除栈顶元素

	cout << "栈顶元素：" << s1.top() << endl;

	//打印栈容器数据
	while (!s1.empty()) {
		cout << s1.top() << " ";
		s1.pop();
	}

	cout << endl;

	cout << "size:" << s1.size() << endl;
}

int main() {
	test01();
	return 0;
}
```



#### 关联式容器

**容器的规则是固定的，与元素进入容器的时间和地点无关**

##### set/multiset

set表示集合，集合是存储排序键的关联式容器，且每个键值都是唯一的，可以插入或删除但是不能更改

- 以红黑树为底层机制，查找效率非常好
- set中不允许重复元素，multiset中允许重复元素
- 不可通过迭代器改变set元素的值，会破坏set组织

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <set>
using namespace std;
//仿函数  类
class Mycompare {
public:
    bool operator()(int v1, int v2) const {  //此处注意，要加const限定符
        return v1 > v2;
    }
};

//set容器初始化
void test01() {
    set<int, Mycompare> s1;
    s1.insert(8);
    s1.insert(10);
    s1.insert(4);
    s1.insert(6);
    s1.insert(5);
    s1.insert(1);
    s1.insert(3);
    s1.insert(2);  //自动进行排序，默认从小到大

    for (set<int>::iterator it = s1.begin(); it != s1.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;

    //从大到小排序怎么办？
#if 0
	//赋值
	set<int> s2;
	s2 = s1;

	//删除
	s1.erase(s1.begin());
	s1.erase(6);

	for (set<int>::iterator it = s1.begin(); it != s1.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;

	cout << "size = " << s2.size() << endl;
	s2.clear();
	cout << "size = " << s2.size() << endl;

#endif
}

//set查找
void test02() {
    //实值
    set<int> s1;
    s1.insert(8);
    s1.insert(10);
    s1.insert(4);
    s1.insert(6);
    s1.insert(5);
    s1.insert(1);
    s1.insert(3);
    s1.insert(2);

    set<int>::iterator ret = s1.find(4);

    if (ret == s1.end()) {
        cout << "没找到" << endl;
    } else {
        cout << *ret << endl;
    }

    //找到第一个大于等于的元素
    ret = s1.lower_bound(4);
    if (ret == s1.end()) {
        cout << "没找到" << endl;
    } else {
        cout << *ret << endl;
    }

    //找到第一个大于的元素
    ret = s1.upper_bound(4);
    if (ret == s1.end()) {
        cout << "没找到" << endl;
    } else {
        cout << *ret << endl;
    }

    //equal_range 返回lower_bound和upper_bound的值
    pair<set<int>::iterator, set<int>::iterator> pret = s1.equal_range(4);
    if (pret.first == s1.end()) {
        cout << "没找到" << endl;
    } else {
        cout << "找到" << endl;
        cout << *pret.first << endl;
    }

    if (pret.second == s1.end()) {
        cout << "没找到" << endl;
    } else {
        cout << "找到" << endl;
        cout << *pret.second << endl;
    }
}

class Person {
public:
    Person(int id, int age) : id(id), age(age){};

    int id;
    int age;
};

class PersonCompare {
public:
    bool operator()(const Person& p1, const Person& p2) const {
        return p1.age > p2.age;
    }
};
void test03() {
    set<Person, PersonCompare> sp;

    Person p1(10, 20), p2(30, 40), p3(50, 60);

    sp.insert(p1);
    sp.insert(p2);
    sp.insert(p3);

    for (set<Person, PersonCompare>::iterator it = sp.begin(); it != sp.end(); it++) {
        cout << (*it).id << "   " << (*it).age << endl;
    }

    //查找
    Person p4(90, 20);
    set<Person, PersonCompare>::iterator ret = sp.find(p4);  //可找到，对应p1，按照age排序就按照age查找
    if (ret == sp.end()) {
        cout << "没找到" << endl;
    } else {
        cout << "找到" << endl;
        cout << (*ret).id << "   " << (*ret).age << endl;
    }
}
int main() {
    //test01();
    //test02();
    test03();
    return 0;
}
```

###### 对组

将两个值合并成一个值

```c++
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
using namespace std;

void test01() {
	pair<int, int> pair1(10, 20);
	cout << pair1.first << "   " << pair1.second << endl;

	pair<int, string> pair2 = make_pair(10, "aaa");
	cout << pair2.first << "   " << pair2.second << endl;

	pair<int, string> pair3 = pair2;
	cout << pair3.first << "   " << pair3.second << endl;

}

int main() {
	test01();
	return 0;
}
```



##### map/multimap

- map与set区别，map具有键值和实值
- 所有元素根据键值自动排序
- pair的第一个元素成为键值，第二个元素成为实值
- map也是以红黑树为底层实现机制
- 不可以通过map的迭代器修改map键值，键值关系到容器内元素的排列顺序，任意改变键值会破坏容器的排列规则，但是可以改变实值
- multimap允许相同键值存在，map不允许

```c++
#include <iostream>
#include <map>
using namespace std;

//map初始化
void test01() {
    //map容器模板参数：第一个参数key的类型，第二个参数value的类型
    map<int, int> m;

    //插入数据  pair.first  对应key，pair.second   对应value
    //第一种
    pair<map<int, int>::iterator, bool> ret = m.insert(pair<int, int>(10, 10));  //放入匿名对象
    if (ret.second) {
        cout << "第一次插入成功" << endl;
    } else {
        cout << "插入失败" << endl;
    }

    //第二种
    ret = m.insert(make_pair(10, 20));
    if (ret.second) {
        cout << "第二次插入成功" << endl;
    } else {
        cout << "插入失败" << endl;
    }

    //第三种
    m.insert(map<int, int>::value_type(30, 30));

    //第四种
    m[40] = 40;
    m[10] = 20;
    //如果key不存在，创建pair并插入
    //如果key存在，修改value实值

    //打印
    for (map<int, int>::iterator it = m.begin(); it != m.end(); it++) {
        //*it取出来一个pair
        cout << "key = " << (*it).first << ", value = " << (*it).second << endl;
    }

    cout << "m[60] = " << m[60] << endl;
    //访问一个不存再的key，那么map会将这个key插入到map中，对应的value默认为零

    for (map<int, int>::iterator it = m.begin(); it != m.end(); it++) {
        //*it取出来一个pair
        cout << "key = " << (*it).first << ", value = " << (*it).second << endl;
    }
}

class MyKey {
public:
    MyKey(int index, int id) : index(index), id(id) {}

    int index;
    int id;
};

struct mycompare {
    bool operator()(MyKey key1, MyKey key2) const {
        return key1.index > key2.index;
    }
};

void test02() {
    map<MyKey, int, mycompare> m;  //需要排序，自定义类型，给定一个排序方法

    m.insert(make_pair(MyKey(1, 2), 2));

    m.insert(make_pair(MyKey(3, 4), 2));

    for (map<MyKey, int, mycompare>::iterator it = m.begin(); it != m.end(); it++) {
        cout << it->first.index << ";" << it->first.id << ";" << it->second << endl;
    }
}

//equal_range
void test03() {
    map<int, int> m;
    m.insert(make_pair(1, 4));
    m.insert(make_pair(2, 5));
    m.insert(make_pair(3, 6));

    pair<map<int, int>::iterator, map<int, int>::iterator> ret = m.equal_range(2);

    if (ret.first->second) {
        cout << "找到lower_bound" << endl;
    } else {
        cout << "没有找到" << endl;
    }

    if (ret.second->second) {
        cout << "找到upper_bound" << endl;
    } else {
        cout << "没有找到" << endl;
    }
}
int main() {
    //test01();
    //test02();
    test03();
    return 0;
}
```

#### 对比

| 容器         | vector   | deque    | list     | set    | multiset | map           | multimap      |
| ------------ | -------- | -------- | -------- | ------ | -------- | ------------- | ------------- |
| 典型内存结构 | 单端数组 | 双端数组 | 双向链表 | 二叉树 | 二叉树   | 二叉树        | 二叉树        |
| 可随机存取   | 是       | 是       | 否       | 否     | 否       | 对key而言：是 | 否            |
| 元素搜寻速度 | 慢       | 慢       | 非常慢   | 快     | 快       | 对key而言：快 | 对key而言：快 |
| 元素安插移除 | 尾端     | 头尾两端 | 任何位置 | -      | -        | -             | -             |

### 迭代器

可以理解为指针，对指针的操作基本可以对迭代器操作，但实际上，迭代器是一个类，这个类封装了一个指针

### 算法

通过有限的步骤解决问题的方法