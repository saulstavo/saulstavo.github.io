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














