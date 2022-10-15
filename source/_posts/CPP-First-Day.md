---
title: C++学习第一天
date: 2018-06-15 19:39:22
tags:
- C++
- Mistake
- Code
categories:
- Code
---
今天，在编写第一个C++程序时，出现了一个编译错误：  
![1]
<!--more-->
以下是原来的代码：  
```c++
#include <iostream>
int main()
{
  using namespace std::cout;
  using namespace std::cin;
  using namespace std::endl;

  int dogs = 1;

  cout << "I have " << dogs << " dogs." << endl;
  cout << "Then you need to add some dogs, input the number which you want to add: ";
  cin >> dogs;
  dogs += 1;
  cout << "Now I have " << dogs << " dogs, thx." << endl;
  return 0;
}
```
起初并不知道因何而起，因为记忆中《C++ Primer Plus》上明确说明可以使用以下两种形式：  
```c++
using namespace std;
```
和  
```c++
using namespace std::cout;
```
查阅资料后发现，如果想仅使用某个函数，不需要`namespace`，只需要：  
```c++
using std::cout;
```
即可。个人理解为，`namespace`既名为命名空间，其一定用于包含整个类库，而非某个函数，而只有`using`就比较好理解了，是使用此函数。  
修改后：  
```c++
#include <iostream>
int main()
{
  using std::cout;
  using std::cin;
  using std::endl;

  int dogs = 1;

  cout << "I have " << dogs << " dogs." << endl;
  cout << "Then you need to add some dogs, input the number which you want to add: ";
  cin >> dogs;
  dogs += 1;
  cout << "Now I have " << dogs << " dogs, thx." << endl;
  return 0;
}
```
编译成功、运行效果：  
![2]

同时，又查了一下，Java中是否有此类实现，因为在之前使用Java时，都是直接使用包名.类名.方法名调用，比如：  

```java
iPhone X = new com.Apple.getNewDevice(com.Apple.iOS.iPhone);
```
资料给出的答案是，在Java中，import的功能在此方面等于using namespace，比如如下Java代码：
```java
import com.Apple.*
...
iPhone X = new getNewDevice(iOS.iPhone);
```
至于更深的部分，比如作用域，在此留一个坑，不填，等以后遇到此类项目再议。  
最后，通过查阅资料2，发现了一个问题，关于`using`的使用，还可以如下：
```c++
using myAlias = System.Security.Crypography;
```
暂不清楚为何，猜测为类引用，先往后学习，可能学习到`using`的那一章会有详细讲解。

---
参考资料：
1: [using namespace std::cout?][Refer1]  
2: [Difference between namespace in C# and package in Java][Refer2]  
3: [java的包和命名空间有什么区别?][Refer3]  



[Refer1]: http://www.cplusplus.com/forum/general/54292/
[Refer2]: https://stackoverflow.com/questions/9249357/difference-between-namespace-in-c-sharp-and-package-in-java
[Refer3]: https://zhidao.baidu.com/question/88333685.html
