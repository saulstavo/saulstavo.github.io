title: C++常用语法
tags: [C++]
categories:
  - 编程语言
author: Saulstavo
date: 2024-08-23 17:36:00  
---

#### map
原理：红黑树  
* 头文件
#include <map>
* 创建
```c++
std::map<KeyType, ValueType> mapName;  
std::map<KeyType, ValueType> mapName= {{1, "one"},{2, "two"}};  
```
* 插入元素
```c++
// 使用insert方法
myMap.insert(std::pair<int, std::string>(1, "one"));
myMap.insert(std::make_pair(2, "two"));
// 使用下标运算符
myMap[3] = "three";
```
* 访问
```c++
// 使用下标运算符
std::cout << myMap[1] << std::endl;
// 使用迭代器
for (const auto& pair : myMap) {
    std::cout << pair.first << " -> " << pair.second << std::endl;
}
```
如果在使用迭代器时想记录循环次数，可以另外加变量解决  
* 查找
```c++
// 使用find方法，返回一个迭代器指向找到的元素，如果没有找到，则返回map::end
std::map<int, std::string>::iterator it = myMap.find(1);
if (it != myMap.end()) {
    // 找到了元素
    std::cout << "Found: " << it->first << " -> " << it->second << std::endl;
} else {
    // 没有找到元素
    std::cout << "Element not found" << std::endl;
}
```
* 排序
1. 结构体运算符重载
```c++
typedef struct node{
	int score;
	string name;
	bool operator <(const node &s)const{
		if(score!=s.score)return score>s.score;
        return name>s.name;
	}
}node;
```
1. 修改map排序规则
```c++
// 自定义比较函数对象
struct Rule {
    bool operator()(const std::string& a, const std::string& b) const {
        return a.length() < b.length(); // 按字符串长度升序排序
    }
};
std::map<std::string, int, Rule> myMap; // 定义时候最后加上规则
```
> 基于红黑树，本身map中的key就是有序的。查找key的效率O(logn)

#### unordered_map
原理：哈希表  
查找插入删除效率：O(1)，比基于红黑树的map的效率O(logn)更快  
**所有操作同map**  
> std::unordered_map 本身并不是排序的，它的元素是通过哈希函数存储在哈希表中的，而不是按照键的顺序排列。因此，如果想按照键的顺序遍历，需要使用map  

#### 运算符重载
有多种方法，建议写到结构体或类内部
```c++
typedef struct node{
	int score;
	string name;
	bool operator <(const node &s)const{
		if(score!=s.score)return score>s.score;
        return name>s.name;
	}
}node;
```
**对于A operator+(const A& a)const {...}而言**  
const A& a：  
这是函数的参数列表，其中 a 是一个引用参数，类型为 A 的常量引用。const 关键字表示在函数内部不能修改 a 引用的对象。使用引用而不是值传递可以避免不必要的对象拷贝，提高效率。  
const：  
这个 const 出现在参数列表后面，表示这个成员函数不会修改调用它的对象（即 this 指针所指向的对象）。这样的成员函数被称为常量成员函数。  

#### string
字符串，可以当作字符数组访问修改。有时也可以把string作为栈使用。  
std::string str1 = "hello"; std::string str3 = str1; // 复制 str1 中的内容到 str3  
* 查找
```c++
// 查找子串首次出现的位置, size_t有的在#include<cstddef>，有的本来就有
size_t pos = str.find("C++");
if (pos != std::string::npos) //如果找到就打印 ，==npos就是找不到
{    
    std::cout << "'C++' found at position: " << pos << std::endl;
} 
else {
    std::cout << "'C++' not found." << std::endl;
}
```
* 子串
```c++
// 提取子字符串
std::string sub = str.substr(pos, 3); // 从位置 pos 开始，提取 3 个字符
std::cout << "Substring extracted: " << sub << std::endl;
```
* 替换
```c++
// 替换字符串中的某些部分
str.replace(pos, 3, "Java"); // 用 "Java" 替换 "C++"
std::cout << "String after replace: " << str << std::endl;
```
* 插入
```c++
// 在指定位置插入字符串
str.insert(7, "beautiful "); // 在位置 7 插入 "beautiful "
std::cout << "String after insert: " << str << std::endl;
```
* 删除
```c++
// 删除字符串中的某些部分
str.erase(7, 10); // 从位置 7 开始，删除 10 个字符
std::cout << "String after erase: " << str << std::endl;
```
* 获取长度
```c++
// 获取字符串长度
std::cout << "String length: " << str.size() << std::endl;
```
* 比较字符串
```c++
// 比较两个字符串
std::string str2 = "Hello, World!";
int result = str.compare(str2); //此函数的功能和C语言的strcmp（）一样，但用法有差异
if (result == 0) {
    std::cout << "str is equal to str2" << std::endl;
} 
else if (result < 0) 
{
    std::cout << "str is less than str2" << std::endl;
} 
else 
{
    std::cout << "str is greater than str2" << std::endl;
}
```
* 追加
```c++
// 在字符串末尾追加内容
str.append(" Welcome!"); //此函数实现得有些冗余，其实+=和这个函数实现的效果完全一样（个人更喜欢用+= 简单）
std::cout << "String after append: " << str << std::endl;
```
* 使用迭代器遍历/修改
迭代器的底层还是指针  
```c++
// 修改字符串中的字符
for (string::iterator it = str.begin(); it != str.end(); ++it) {
    if (*it == 'C') {
        *it = 'D';  // 修改 'C' 为 'D'
    }
}
```
#### set和unordered_set
类似于map和unordered_map，set底层是红黑树，查找插入删除效率O(logn)，unordered_set底层是哈希表，查找插入删除效率O(1).  

#### stack
std::stack<int> stk(10, 0); // 创建一个包含10个0的stack

int top = stk.top(); // 获取stack顶部的元素

bool empty = stk.empty(); // 检查stack是否为空

stk.push(42); // 在stack的顶部添加一个元素

stk.push(42); // 在stack的顶部添加一个元素

stk.pop(); // 删除stack顶部的元素

stk.top() = 42; // 修改stack顶部的元素

bool contains = stk.contains(42); // 检查stack中是否包含值为42的元素

stk.sort(); // 默认根据元素值进行升序排序

size_t size = stk.size(); // 获取stack中的元素数量

size_t capacity = stk.capacity(); // 获取stack的容量

#### vector

* 复制
vector<T> v2(v1); //v2中包含v1所有元素的副本
vector<T> v2=v1; //等价于v2(v1)

* 初始化定义
vector<int> a(10, 1) ; //定义一个初始大小为 10 且初始值都为 1 的向量

v.size() //返回向量v中的元素个数

v.empty() //若v中不包含任何元素，返回真；否则返回假

v.push_back(t) //向v的尾端添加一个值为t的元素

v.front() //访问v的第一个元素

v.back() //访问v的最后一个元素

push_back() //把传送为参数的对象添加到vector的尾部

pop_back() //删除vector尾最后一个元素

v.erase(v.begin()) //将起始位置的元素删除

v.erase(v.begin(), v.begin()+3) //将(v.begin(), v.begin()+3)之间的3个元素删除

v.erase(v.begin() + 3) //删除第4个元素

clear() //清除所有元素

insert() //插入一个或多个对象

v.insert(v.begin(), 1000); //将1000插入到向量v的起始位置前

v.insert(v.begin() + 1,9); //在第二个位置插入新元素

v.insert(v.begin(), 3, 1000) //位置0开始，连续插入3个1000

v.insert(v.begin()+1, 7,8) //位置1开始，连续插入7个8

#### string
1. 在字符串指定位置插入字符
```c++
string s = "123";
    s.insert(0,"@");//第一个参数是插入的位置下标，第二个是插入的字符串
    s.insert(1,"###");
cout << s << endl;
```
输出：@###123

2. 删除字符串中的所有指定字符
```c++
int i=s.find(x);
 while(i!=string::npos)//-1
 {
     s.erase(i,x.length());
     i=s.find(x);
 }
string& erase(size_t pos=0, size_t len = npos);
```

其中，参数pos表示要删除字符串的起始位置，其默认值是0；len表示要删除字符串的长度，其默认值是string::npos。返回值是删除后的字符串。

3. 替换字符串中的所有指定字符
法一、for循环直接赋值
```c++
for(int k=0;k<s.length();k++)
     if(s[k]==x)
         s[k]=y;
 cout<<"Replace->"<<s<<endl;
```
法二、algorithm里面的replace函数
```c++
string s = "121145";
    //char a = '1';
    replace(s.begin(), s.end(), '1', '#');
    cout << s << endl;
```
4. 输出字符串的长度
s.length();
s.size();

5. 反转输出字符串（这一步仅输出，不改变字符串）
法一、for循环输出
```c++
for(int k=s.length()-1;k>=0;k--)
     cout<<s[k];
 cout<<endl;
```
法二、reverse函数
```c++
string s2(s1);
reverse(s2.begin(), s2.end());//头文件是不是也在算法里面？
cout << s2 << endl;
```

6、输出字符串从位置i到j的子串
法一、for循环
cin>>i>>j;
cout<<"Sub->";
for(int k=i;k<=j;k++)
     cout<<s[k];
 cout<<endl;

法二、substr函数
int a, b;
cin >> a;
cin >> b;

string t = s1.substr(a, b-a+1);
cout<<"Sub->";
cout << t << endl;
7、匹配子串，输出首次匹配到子串的第一个字符位置
法一、手动输出-1
cin>>st;//string st
if(s.find(st)!=string::npos)
     cout<<"Find->"<<s.find(st)<<endl;
else
     cout<<"Find->-1"<<endl;
cout<<endl;

法二、find没匹配到会自己变成-1
string a;
cin >> a;
int x = -1;
x = s1.find(a);
cout<<"Find->";
cout << x << endl;

8、补充
1 .string的定义和初始化：
（1）string s1; —> // 默认初始化，s1是一个空字符串
（2）string s2 = s1; —> // s2是s1的副本，注意s2只是与s1的值相同，并不指向同一段地址
（3）string s3 = "hiya"; —> // s3是该字符串字面值的副本
（4）string s4(10, 'c'); —> // s4的内容是: “cccccccccc”










