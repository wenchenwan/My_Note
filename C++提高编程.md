# C++提高编程

C++泛型编程和STL技术做详细讲解，探讨C++更深层的应用

## 1 模板

### 1.1模板的概念

---------------------------------------

### 1.2函数模板

- C++泛型编程也是一种重要的思想，主要利用的技术就是模板

- C++提供了两种模板**类模板**和**函数模板**

#### 1.2.1函数模板语法

建立一个通用函数，其函数的返回值类型和形参类型可以不具体给定，用一个虚拟代表来表示

**语法**

```c++
template<typename T>
//函数的声明或者定义
```

**解释**

template 声明创建模板

typename 表面其后面的符号表示一种数据类型，可以用class代替

T 通用的数据类型，名称可以代替，通常为大写字母

**实例**

```c++
#include<iostream>

using namespace std;
//交换A和B数值

template<typename T>
void MySwap(T& a, T& b) {
	T temp;
	temp = a;
	a = b;
	b = temp;
}
int main() {
	int a = 10;
	int b = 20;
	cout << "a = " << a << endl;
	cout << "b = " << b << endl;


	MySwap(a, b);//隐式的调用
	MySwap<int>(a, b);//显示的调用
	cout << "a = " << a << endl;
	cout << "b = " << b << endl;

	return 0;
}
```

**两种方式使用函数模板**

1. 自动类型推导
2. 显示指定类型

```c++
mySwap<int>(a,b);
```

**将类型参数化**

#### 1.2.2函数模板的注意事项

**注意事项：**

- 自动类型推导必须要推导出来一致的数据类型T，才可以使用
- 函数模板必须要确定出T的类型才能使用

**实例：**

```c++
#include<iostream>

using namespace std;
template<typename T>
void Swap(T &a, T &b) {
	T temp;
	temp = a;
	a = b;
	b = temp;
}

//函数模板必须要确定出数据类型，才能使用
template<typename T>
void func() {
	cout << "func()的调用" << endl;

}


int main() {

	int a = 1;
	int b = 2;
	double c = 3;
	cout << "a = " << a << endl;
	cout << "b = " << b << endl;

	Swap(a, b);
	cout << "a = " << a << endl;
	cout << "b = " << b << endl;
	//Swap(b, c);  //推导不出一致的数据类型
	//func();
	func<int>();//利用显示的指定类型的方式，才能调用模板函数
	return 0;
}
```

#### 1.2.3函数模板数组排序的实现

```c++
#include<iostream>

using namespace std;

template<typename T>
void Sort(T* a,int n) {

	for (int i = 0; i < n; i++) {//选择排序
		int max = i;
		for (int j = i+1; j < n; j++) {
			if (a[max] < a[j]) {
				max = j;
			}
		}
		if (max != i) {
			T temp;
			temp = a[max];
			a[max] = a[i];
			a[i] = temp;
		}
	}
}

template<typename T>
void ShowArray(T* arr, int len) {
	for (int i = 0; i < len; i++) {
		cout << arr[i] << " ";
	}
	cout << endl;
}
void test01() {
	char charArray[] = "youxiaoqing";
	
	int Chlen = sizeof(charArray) / sizeof(char);
	Sort(charArray,Chlen);
	ShowArray(charArray, Chlen);
}
void test02() {
	int IntArr[] = { 1,2,3,4,5,6,7,8,11,3,34,556,76,4,34,5,53,34 };
	int len = sizeof(IntArr) / sizeof(int);
	Sort(IntArr, len);
	ShowArray(IntArr, len);
}




int main() {
	//test01();
	test02();
	return 0;
}
```

#### 1.2.4普通函数和函数模板的区别

**区别：**

- 普通函数在调用的时候，可以发生自动类型转换
- 函数模板调用时，如果利用自动类型推导，就不会发生隐式的类型转换
- 如果利用显示的指定类型的方式，可以发生隐式的类型转换

**实例:**

```c++
#include<iostream>

using namespace std;
template<typename T>
T Add_T(T a, T b) {
	return a + b;
}

double MyIntAdd(double a, double b) {
	return a + b;
}

void test01() {
	int A = 20;
	char B = 'a';
	
	cout << "输出为 ：" << MyIntAdd(A, B) << endl;
}
void test02() {
	int a = 10;
	char b = 'q';
	Add_T<int>(a, b);//显示指定类型实现类型转换
	cout << "运行结果为 ： " << Add_T<int>(a, b) << endl;
}

int main() {
	test01();
	return 0;
}
```

**总结：建议使用显示的类型指定方法去确定自己的通用类型T**

#### 1.2.5普通函数和函数模板的调用规则

**如下:**

1. 如果函数模板和普通函数都可以实现，优先调用普通函数
2. 可以通过空模板参数列表来强制调用函数模板
3. 函数模板也可以发生重载
4. 如果函数模板可以产生更好的匹配，优先调用函数模板

**实例：**

```c++
#include<iostream>

using namespace std;
void MyPrint(int a, int b)
{
	cout << "调用的普通函数" << endl;
}

template<typename T>
void MyPrint(T a, T b)
{
	cout << "调用的函数模板" << endl;
}

template<typename T>
void MyPrint(T a, T b, T c) {
	cout << "重载的函数模板 " << endl;
}

void test01() {
	int a = 0, b = 0;
	double c = 0, d = 0;
	//MyPrint(a,b);
	MyPrint(c, d);

	//通过空模板的参数列表强制调用函数模板
	MyPrint<>(a, b);
	MyPrint<int>(a, b, c);

	//如果函数模板产生更好的匹配，优先调用函数模板
}

int main() {
	test01();
	return 0;
}
```

**总结:提供了函数模板，不要再提供单独的函数实现**

#### 1.2.6函数模板的局限性

**局限性：**

- 模板的通用性并不是万能的

**例如**

```c++
template<typename T>
void f(T a,T b){
	a = b;
}
```

若a，b为数组则就无法实现

**例如：**

```c++
template<typename T>
void f(T a,T b){
	if(a>b){......}
}
```

若A,B为自定义数据类型，则无法匹配

```c++
#include<iostream>

using namespace std;
//对比两个数据是否相等的函数
template<typename T>
bool MyCompare(T &a,T &b) {
	if (a == b) {
		cout << "两个值相等" << endl;
		return true;
	}
	else {
		cout << "两个值不相等" << endl;
		return false;
	}

}



class Person {
public:
	string m_Name;
	int m_Age;
	Person(string name,int age) {
		m_Name = name;
		m_Age = age;
	}
};

//template<> bool MyCompare(Person& a, Person& b)
bool MyCompare(Person& a, Person& b) {
	if ((a.m_Age == b.m_Age) && (a.m_Name == b.m_Name)) {
		return true;
	}
	else
	{
		return false;
	}
}

void test01() {
	int a = 10;
	int b = 10;
	double c = 10.4;
	double d = 10.2;
	MyCompare(a, b);
	MyCompare<double>(d, c);
}
//1.利用运算符重载，重载等号
//2.利用具体化的person的版本实现代码，具体化优先调用

void test02() {
	Person p1("Tom", 20);
	Person p2("Jerry", 19);
	bool ret= MyCompare(p1, p2);
	if (ret == true) {
		cout << "p1 = p2" << endl;
	}
	else
	{
		cout << "p1 != p2" << endl;
	}

}
int main() {

	//test01();
	test02();
	return 0;
}
```

- 可以利用具体化的模板来解决自定义类型的通用化
- 学习模板不是为了写模板，而是在stl能够运用系统提供的模板

### 1.3类模板

#### 1.3.1类模板基本语法

类模板的作用：

- 建立一个通用类，类中的成员数据类型可以不具体指定，用一个虚拟的类型来代表



------

**语法：**

```c++
template<typename T>
类
```

一般在声明类模板时候，使用class关键字

**示例：**

```c++
#include<iostream>

using namespace std;

//类模板
template<class NameType,class AgeType>
class Person {
public:
	Person(NameType name,AgeType age) {
		this->m_Name = name;
		this->m_Age = age;
	}

	void ShowPerson(Person p) {
		cout << "this->m_Name =" << this->m_Name << endl;
		cout << "this->m_Age = " << this->m_Age << endl;
	}
	NameType m_Name;
	AgeType m_Age;
};

void test01() {
	Person<string, int>p1("Tom", 20);
	p1.ShowPerson(p1);
	Person<string, int>p2("Jerry", 19);
	p2.ShowPerson(p2);
}

int main() {

	test01();
	return 0;
}
```

#### 1.3.2 函数模板和类模板的区别

类模板与函数模板的主要有两点：

1. 类模板没有自动类型推导的使用方式
2. 类模板在模板参数列表中可以有默认参数



**示例：**

```c++
#include<iostream>

using namespace std;

template<class NameType,class AgeType = int>
class Person {
public:
	Person(NameType name,AgeType age) {
		this->m_Name = name;
		this->m_Age = age;
	}

	void ShowPerson(Person p) {
		cout << "this->m_Name =" << this->m_Name << endl;
		cout << "this->m_Age = " << this->m_Age << endl;
	}
	NameType m_Name;
	AgeType m_Age;
};
//1.类模板没有自动类型推导
void test01() {
	//Person p1("孙悟空", 1000);
	Person<string, int> p1("孙悟空", 1000);//只能显示指定类型
}
//2.类模板在模板参数列表中可以有默认参数
void test02() {
	Person<string> p2("猪八戒", 999);
	p2.ShowPerson(p2);
}

int main() {
	test02();
	return 0;
}
```

#### 1.3.3类模板中的成员函数创建时机

类模板中的成员函数和普通类中的成员函数创建时机是有区别的：

- 普通类中的成员函数一开始就可以创建
- 类模板中的成员函数在调用时才会创建

**实例：**

```c++
#include<iostream>

using namespace std;

template<class NameType,class AgeType>
class Person {
public:
	Person(NameType name, AgeType age) {
		this->m_age = age;
		this->m_name = name;
	}
	NameType m_name;
	AgeType m_age;
};


class People1 {
public:
	void Show1() {
		cout << "People1调用" << endl;
	}
};

class People2 {
public:
	void Show2() {
		cout << "People2调用" << endl;
	}
};

template<class T>
class MyClass {
public:
	T obj;
	void func1() {
		obj.Show1();
	}

	void func2() {
		obj.Show2();
	}
};

void test01()
{
	MyClass<People1>p1;
	p1.func1();
	//p1.func2();

	MyClass<People2>p2;
	//p2.func1();
	p2.func2();

}
int main() {

	test01();
	return 0;
}
```

总结:类模板中的成员函数并不是一开始就创建的，在调用时才回去创建



#### 1.3.4类模板对象做函数参数

类模板实例化对象，向函数传递参数

**有三种传入的方式：**

- 指定传入的类型   ---直接显示对象的类型
- 参数化模板           ---将对象中的参数变为模板进行传递
- 整个类模板化       ---将这个对象类型 模板化进行传递



实例：

```c++
#include<iostream>

using namespace std;

template<class T1,class T2>
class Person {
public:
	Person(T1 name, T2 age) {
		m_Name = name;
		m_Age = age;
	}
	void ShowPerson() {
		cout << "m_Name =" << this->m_Name << endl;
		cout << "m_Age =" << this->m_Age << endl;
	}


	T1 m_Name;
	T2 m_Age;
};
//01.直接显示对象的类型
void print1(Person<string, int> &p) {
	p.ShowPerson();
}

void test01() {
	Person<string,int> p1("孙悟空", 1000);
	print1(p1);
}

//2.参数化模板
template<class T1,class T2>
void print2(Person<T1,T2>&p) {
	p.ShowPerson();
	cout << "T1的类型为 " << typeid(T1).name() << endl;
	cout << "T2的类型为 " << typeid(T2).name() << endl;

}
void test02() {
	Person<string,int> p2("猪八戒", 999);
	print2(p2);
}

//3.整个类模板化
template<class T>
void print3(T &p) {
	p.ShowPerson();
	cout << "T的类型为 " << typeid(T).name() << endl;

}
void test03() {
	Person<string, int> p3("唐僧", 30);
	print3(p3);
}
int main() {
	//test01();
	//test02();
	test03();

	return 0;
}
```

总结：

- 通过类模板创建的对象，可以有三种方式向函数传参
- 使用比较广泛的是第一种：指定传入类型

#### 1.3.5类模板与继承

当类模板遇到继承时候，需要注意以下几点：

- 当子类继承的父类是一个类模板的时候，子类在声明的时候，要指定出父类中的T的类型
- 如果不指定，编译器无法给子类分配内存
- 如果想灵活的指定父类中T的类型，子类也需要变为类模板



实例：

```c++
#include<iostream>

using namespace std;

template<class T>
class Base {
public:
	T m;
};

//class Son :public Base { //只有在必须知道父类的T的类型时候，才能继承给子类
//
//};
class Son :public Base<int> {

};
//如果想灵活的指定父类中的T类型子类也需要为模板
template<class T1,class T2>
class Son2 :public Base<T2> {
public:
	void Show() {
		cout << " T1的数据类型 " << typeid(T1).name() << endl;
		cout << " T2的数据类型 " << typeid(T2).name() << endl;
	}
	T1 obj;
	
};

void test02() {
	Son2<int, char>s2;
	s2.Show();
}

int main() {

	test02();
	return 0;
}
```

#### 1.3.6类模板的成员函数的外部实现

使用模板类的成员函数在类的外部实现

```c++
#include<iostream>

using namespace std;


template<class T1,class T2>
class Person {
public:
	Person(T1 name,T2 id);
	void Show();

	T1 m_name;
	T2 m_id;
};
//构造函数的外部实现
template<class T1,class T2>
Person<T1, T2>::Person(T1 name,T2 id) {
	this->m_id = id;
	this->m_name = name;
}

//成员函数的外部实现
template<class T1,class T2>
void Person<T1,T2>::Show() {
	cout << "m_name = " << this->m_name << endl;
	cout << "m_age = " << this->m_id << endl;
}

void test01() {
	Person<string, int>p1("Tom", 100);
	p1.Show();
}
int main() {
	test01();
	return 0;
}
```

#### 1.3.7 类模板分文件编写

- 掌握类模板成员函数分文件编写时的问题以及解决方案

**问题：**

- 类模板中的成员函数创建时机是在调用阶段，导致分文文件编写连接不到

**解决**

- 解决方式1:直接包含.cpp文件
- 将声明和实现写到同一个文件里，并改为.hpp, hpp是约定的名称并不是强制

**示例：**

person.hpp

```c++
#pragma once
#include<iostream>
using namespace std;
#include<string>


template<class T1, class T2>
class Person {
public:
	Person(T1 name, T2 age);
	void Show();
	T1 m_Name;
	T2 m_Age;
};

template<class T1, class T2>
Person<T1, T2>::Person(T1 name, T2 age) {
	this->m_Name = name;
	this->m_Age = age;
}

template<class T1, class T2>
void Person<T1, T2>::Show() {
	cout << "姓名 ：" << this->m_Name << endl;
	cout << "年龄 ：" << this->m_Age << endl;
}

```

当编译器看到.h头文件，不会生成函数的

person.cpp

```c++
#include"Person.hpp"

template<class T1, class T2>
Person<T1, T2>::Person(T1 name, T2 age) {
	this->m_Name = name;
	this->m_Age = age;
}

template<class T1, class T2>
void Person<T1, T2>::Show() {
	cout << "姓名 ：" << this->m_Name << endl;
	cout << "年龄 ：" << this->m_Age << endl;
}

```

最终实现

```c++
//#include"person.cpp"
//直接包含源文件

#include"Person.hpp"
//template<class T1,class T2>
//class Person {
//public:
//	Person(T1 name, T2 age);
//	void Show();
//	T1 m_Name;
//	T2 m_Age;
//};

//template<class T1,class T2>
//Person<T1, T2>::Person(T1 name, T2 age) {
//	this->m_Name = name;
//	this->m_Age = age;
//}
//
//template<class T1,class T2>
//void Person<T1, T2>::Show() {
//	cout << "姓名 ：" << this->m_Name << endl;
//	cout << "年龄 ：" << this->m_Age << andl;
//}

void test01() {
	Person<string, int> p1("张三", 20);
	p1.Show();

}
int main() {
	test01();
}
```

#### 1.3.8类模板和友元

全局函数的类内实现：直接在类内声明友元即可

全局函数的类外实现：需要提前让编译器知道全局函数的存在，同时让编译器知道类的存在

示例

```c++
#include<iostream>

using namespace std;
//通过全局函数，打印person信息
template<class T1,class T2>
class Person;

template<class T1, class T2>
void PrintPerson(Person<T1, T2> p) {
	cout << "姓名：" << p.m_Name << "年龄：" << p.m_Age << endl;
}


template<class T1,class T2>
class Person {
public:
	//全局函数类内实现
	//friend void PrintPerson(Person<T1,T2> p) {
	//	cout << "姓名：" << p.m_Name << "年龄：" << p.m_Age << endl;
	//}

	//全局函数类外实现
	//声明需要加一个空模板的参数列表
	//如果全局函数是类外实现，需要让编译器知道这个函数的存在
	//friend void PrintPerson(Person<T1, T2> p);
	friend void PrintPerson<>(Person<T1, T2> p);
	Person(T1 name, T2 age);
private:
	T1 m_Name;
	T2 m_Age;
};

template<class T1,class T2>
Person<T1, T2>::Person(T1 name, T2 age) {
	this->m_Name = name;
	this->m_Age = age;
}



void test01() {
	Person<string, int>p1("Tom", 20);
	PrintPerson(p1);
}
int main() {

	test01();
	return 0;
}
```

#### 1.3.9类模板案例

要求：描述一个通用的数组类

- 可以对内置的数据类型和自定义的数据类型进行存储
- 将数组中的数据存放到堆区
- 构造函数可以传入数组的容量
- 提供对应的拷贝构造函数已经operator=防止浅拷贝的问题
- 提供尾插法和尾删法对数组的数据进行增加和删除
- 通过下标的方式访问数组中的元素
- 可以获得数组中当前元素的个数和数组的容量

![image-20201124105720069](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201124105720069.png)

**示例：**

Myarray.hpp代码

```c++
#pragma once
#include<iostream>

using namespace std;

template<class T>
class MyArray {
public:
	MyArray(int capacity) {
		//cout << "有参构造函数的调用" << endl;
		this->m_Capacity = capacity;//容量
		this->m_Size = 0;//数组的大小
		this->array = new T[this->m_Capacity];
	}

	MyArray(const MyArray& arr) {
		//cout << "拷贝构造函数的调用" << endl;
		this->m_Size = arr.m_Size;
		this->m_Capacity = arr.m_Capacity;

		this->array = new T[arr.m_Capacity];
		for (int i = 0; i < this->m_Size; i++) {
			this->array[i] = arr.array[i];
		}
	}
	//返回自身的引用，可以实现链式法则
	MyArray& operator=(const MyArray& arr) {
		//cout << "拷贝operator=的调用" << endl;
		//先判断原来的堆区是否有数据，如果有数据，则释放
		if (this->array != NULL) {
			delete[] this->array;
			this->array = NULL;
		}
			this->m_Capacity = arr.m_Capacity;
			this->m_Size = arr.m_Size;
			this->array = new T[this->m_Capacity];
			for (int i = 0; i < this->m_Size; i++) {
				this->array[i] = arr.array[i];
			}
		return *this;
	}

	//尾插法
	void PushBack(const T& num) {
		//判断容量是否等于元素个数
		if (this->m_Capacity == this->m_Size) {
			cout << "容量已经满了" << endl;
			return;
		}
		this->array[this->m_Size] = num;
		this->m_Size++;
	}
	void PopBack() {
		if (this->m_Size == 0) {
			cout << "元素已满" << endl;
			return;
		}
		this->m_Size--;
	}

	T& operator[](int index) {
		//T a;
		//if (index<0 && index>=this->m_Size) {
		//	return a;
		//}

		return this->array[index];
	}
	int ArrSize() {
		return this->m_Size;
	}
	int ArrCapacity() {
		return this->m_Capacity;
	}


	~MyArray() {
		//cout << "析构函数的调用" << endl;
		if (this->array != NULL) {
			delete[] array;
			array = NULL;
		}
	}
private:
	T* array; //指针指向堆区开辟的数组
	int m_Capacity;
	int m_Size;
};


//测试自定义数据类型
class Person {
public:
	Person(){}

	Person(string name, int age) {
		this->name = name;
		this->age = age;
	}
	void Print() {
		cout << "this->age = " << this->age << endl;
		cout << "this->name =" << this->name << endl;
	}
	int age;
	string name;

};

```

09 类模板案例 数组封装.cpp

```c++
#include<iostream>
#include"Myarray.hpp"
using namespace std;
class Person;

void PrintArr(MyArray<int>& arr) {
	for (int i = 0; i < arr.ArrCapacity(); i++) {
		cout << "arr1[" << i << "] =" << arr[i] << endl;
	}
}

void test01() {
	MyArray<int>arr1(5);
	MyArray<Person>arr4(10);
	//MyArray<int>arr2(arr1);
	//MyArray<int>arr3(100);

	//arr3 = arr1;

	for (int i = 0; i < arr1.ArrCapacity(); i++) {
		arr1.PushBack(i);
	}
	PrintArr(arr1);


	cout << "arr1的大小" << arr1.ArrSize() << endl;
	cout << "arr1的容量" << arr1.ArrCapacity() << endl;

	arr1.PopBack();
	cout << "arr1的大小" << arr1.ArrSize() << endl;
	cout << "arr1的容量" << arr1.ArrCapacity() << endl;

	Person p("张三", 20);
	arr4.PushBack(p);
	arr4[0].Print();
}


int main() {
	test01();

	return 0;
}
```

## 2 STL初识

### 2.1 STL的诞生

- 长久以来软件界一直希望可以建立起来一种可复用的东西
- C++的面向对象和泛型编程思想，就是为了提高复用性
- 大多情况下，数据结构和算法未能提供一套标准，导致被迫从事大量重复工作
- 为了建立一套标准的数据结构和算法，建立了STL

### 2.2 STL的基本概念

- STL(standard template library 标准模板库)
- STL广义上分为**容器（container）、算法（algorithm）和迭代器（iterator）**
- 容器和算法之间通过迭代器无缝衔接
- STL几乎所有的代码都采用了模板类或者模板函数

### 2.3 STL的三大组件

STL六大组件为：**容器、迭代器、算法、仿函数、适配器、空间配置器**



1. 容器：各种数据结构vector、list、deque、set、map等
2. 算法：各种常用的算法 sort，find、for_each
3. 迭代器：扮演容器和算法之间的胶合剂
4. 仿函数：行为类似于函数，可以作为算法的某种策略
5. 适配器：一种用来修饰容器或者仿函数或者迭代器接口的东西
6. 空间配置器：负责空间配置和管理

### 2.4 STL中的容器、算法、迭代器

**容器**

序列式容器：强调值的排序，序列中每个元素均有固定的位置

关联式容器：二叉树结构，个元素之间没有严格的物理上的顺序关系

**算法**

质变算法：运算过程过更改区间元素的内容

非质变算法：运算过程中不更改区间元素的内容

**迭代器**

算法通过迭代器来访问容器中的元素

![image-20201124211906021](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201124211906021.png)



### 2.5 容器算法迭代器初识

STL中最常用的迭代器是Vector



#### 2.5.1 Vector存放内置数据类型

容器 vector

算法 for_each

迭代器 vector<int>::iterator

```c++
#include<vector>
#include<algorithm>
#include<iostream>

using namespace std;
void Myprint(int val) {
	cout << val << endl;

}

void test01() {
	//创建一个vector容器
	vector<int> v;
	v.push_back(10);
	v.push_back(30);
	v.push_back(40);
	v.push_back(50);
	v.push_back(50);
	v.push_back(60);
	v.push_back(70);
	//通过迭代器访问
	//第一种
	//vector<int>::iterator itBegain = v.begin();//起始迭代器
	//vector<int>::iterator itEnd = v.end();//指向容器中最后一个元素的下一个位置

	//for (; itBegain != itEnd; itBegain++) {
	//	cout << *itBegain << endl;
	//}
	//第二种
	//for (vector<int>::iterator Vbegin = v.begin(); Vbegin != v.end(); Vbegin++) {
	//	cout << *Vbegin << endl;
	//}
	//第三种
	for_each(v.begin(),v.end(),Myprint);
}

int main() {
	test01();
	return 0;
}
```



#### 2.5.2 vector存放自定义数据类型

```C++
#include<iostream>
#include<vector>
using namespace std;

class Person {
public:
	Person(string name, int age) {
		this->m_Name = name;
		this->m_Age = age;
	}
	void Show() {
		cout << "this->m_Name =" << this->m_Name << endl;
		cout << "this->m_Age =" << this->m_Age << endl;
	}

	int m_Age;
	string m_Name;

};
void test01() {
	Person p1("张三", 20);
	Person p2("李四", 30);
	Person p3("王五", 50);
	Person p4("赵六", 30);
	vector<Person> v;
	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);
	int i = 0;
	for (vector<Person>::iterator itPerson = v.begin(); itPerson != v.end(); itPerson++) {
		/*v[i].Show();
		i++;*/
		(*itPerson).Show();
	}
	
}
void test02() {
	Person p1("张三", 20);
	Person p2("李四", 30);
	Person p3("王五", 50);
	Person p4("赵六", 30);
	vector<Person*> v;
	v.push_back(&p1);
	v.push_back(&p2);
	v.push_back(&p3);
	v.push_back(&p4);
	for (vector<Person*>::iterator itPerson = v.begin(); itPerson != v.end(); itPerson++) {
		(**itPerson).Show();
	}
}
int main() {
	//test01();
	test02();
	return 0;
}
```

#### 2.5.3 vector容器嵌套

类似于多维数组

```c++
#include<iostream>
#include<vector>
using namespace std;

void test01() {
	vector<int> v1;
	vector<int> v2;
	vector<int> v3;
	vector<vector<int>> V;
	for (int i = 1; i < 200; i = i + 10) {
		v1.push_back(i);
	}
	for (int i = 1; i < 200; i = i + 10) {
		v2.push_back(i);
	}
	for (int i = 1; i < 200; i = i + 10) {
		v3.push_back(i);
	}
	V.push_back(v1);
	V.push_back(v2);
	V.push_back(v3);
	for (vector<vector<int>>::iterator ItV = V.begin(); ItV != V.end(); ItV++) {
		for (vector<int>::iterator Itv = (*ItV).begin(); Itv != (*ItV).end(); Itv++) {
			cout << "vector内容为：" << *Itv << endl;
		}
		cout << endl;
	}

}
int main() {
	test01();
	return 0;
}
```

### 3 STL 常用容器

#### 3.1 string容器

##### 3.1.1 string基本概念

**本质:**

- string是C++风格的字符串，string本质上是一个类

**string和char*区别**

- char*是一个指针
- string是一个类，类内部封装了char* ，管理这个字符串，是一个char*的容器

**特点**

string类内部封装了很多方法

例如:copy，replace，insert，delete

string类来管理char* 分配的内存，不用担心越界和取值越界，由内部负责



##### 3.1.2 string构造函数

构造函数原型

```c++
string();//创建空的字符串
string(char* str) //使用字符串str初始化
string(const string& str) //使用string对象初始化
string(int n,char c) //使用n个字符初始化

```

示例：

```c++
#include<iostream>
#include<string>
using namespace std;

//string();//创建空的字符串
//string(char* str) //使用字符串str初始化
//string(const string& str) //使用string对象初始化
//string(int n, char c) //使用n个字符初始化

void test01() {
	string s1;
	const char* str = "Hello world！";
	string s2(str);
	cout << "s2 = " << s2 << endl;

	string s3(s2);
	cout << "s3 = " << s3 << endl;
}
int main() {

	test01();



	return 0;
}
```

##### 3.1.3 string赋值操作

赋值函数的原型



```c++
string& operator=(const char* s);
string& operator=(const string& s);
string& operator=(char c);
string& assign(const char* s);
string& assign(const char* s,int n);//把字符串s的前n个字符赋值给当前字符串
string& assign(const string& s);
string& assign(char c,int n);//将n个字符c赋值给当前字符串
```

示例：

```c++
#include<iostream>
#include<string>

using namespace std;
//string& operator=(const char* s);
//string& operator=(const string& s);
//string& operator=(char c);
//string& assign(const char* s);
//string& assign(const char* s, int n);//把字符串s的前n个字符赋值给当前字符串
//string& assign(const string& s);
//string& assign(char c, int n);//将n个字符c赋值给当前字符串
void test01() {
	string s1;
	s1 = "hello world";
	cout << s1 << endl;

	string s2;
	s2 = s1;
	cout << s2 << endl;

	string s3;
	s3 = 'a';
	cout << s3 << endl;

	string s4;
	s4.assign("hello Tom");
	cout << s4 << endl;

	string s5;
	s5.assign("abcabcabcabc", 3);
	cout << s5 << endl;

	string s6;
	s6.assign(s5);
	cout << s6 << endl;

	string s7;
	s7.assign(5, 'c');
	cout << s7 << endl;
}

int main() {
	test01();
	return 0;
}
```





##### 3.1.4 string容器字符串的拼接



**函数原型：**

- ```c++
  string& operator+=(const char* str);
  ```

- ```c++
  string& operator+=(const char c);
  ```

- ```c++
  string& operator+=(const string& str);
  ```

- ```c++
  string& append(const char* s);
  ```

- ```c++
  string& append(const char* s,int n);//将s的前n个字符追加到当前字符串的结尾
  ```

- ```c++
  string& append(const string& s);
  ```

- ```c++
  string& append(const string& s,int pos,int n);//字符串s从pos开始的n个字符连接到字符串的
  ```

  

示例：

```c++
#include<iostream>
#include<string>
using namespace std;

void test01() {
	string str("wen");
	str += 'c';
	cout << str << endl;

	const char* str2 = "hello";
	str += str2;
	cout << str << endl;

	string s("叠加");
	str += s;
	cout << str << endl;

	str.append("aaaa");
	cout << str << endl;

	str.append("555666", 3);
	cout << str << endl;

	str.append(s);
	cout << str << endl;

	string s2("abcdefghigklmn");
	str.append(s2,3,6);
	cout << str << endl;

}
int main() {

	test01();
	return 0;
}
```

##### 3.1.5 string查找和替换

函数原型

**查找**

```c++
int find(const string& str, int pos = 0) const;//查找str第一次出现的位置，从pos开始
int find(const char* s, int pos = 0) const;
int find(const char* s, int pos, int n) const;//从pos开始查找s的前n个字符第一次位置
int find(const char c, int pos = 0) const;
int rfind(const string& str, int pos = npos) const;//查找str最后一次出现的位置，从pos开始查找
int rfind(const char* s, int pos = npos) const;
int rfind(const char* s, int pos int n) const;
int rfind(const char c, int pos=0) const;
string& replace(int pos, int n, const string& str);//从pos开始的n个字符替换为str
string& replace(int pos, int n, const char* s)//从pos开始的n个字符替换为s
```

示例：

```c++
#include<iostream>
#include<string>
using namespace std;

void test01() {
	string str("Hello I am liu you are");
	string str1("are");
	cout << "liu的位置为：" <<str.find("liu", 0) << endl;

	cout << "are的位置为：" << str.find("are", 0) << endl;

	cout << "are的第一个元素的位置" << str.find("are", 0, 1) << endl;

	cout << "m的第一个元素的位置" << str.find('m', 0) << endl;

	cout << "a最后一次出现的位置" << str.rfind('a') << endl;

	str.replace(1, 2, str1);
	cout << str << endl;
}

int main() {

	test01();
	return 0;
}
```



##### 3.1.6 string 字符串的比较

**比较方式：**

- 字符串比较是按ASCII码进行对比

= 返回0

(>)返回1

< 返回-1



函数原型：

- int compare(const string &s) const;
- int compare(const char* s) const;



示例

```c++
#include<string>
#include<iostream>

using namespace std;
void test01() {
	string str1("wen");
	string str2("chen");
	string str3("wen");
	const char* s = "wen";
	if (str1.compare(str2) == 0) {
		cout << "相等" << endl;
	}
	else
	{
		if (str1.compare(str2) == 1) {
			cout << "大于" << endl;
		}
		else {
			cout << "小于" << endl;
		}
	}
}


int main() {
	test01();

	return 0;
}
```



##### 3.1.7string字符串存取

string种单个字符存取的方式



- char& operator[](int n);
- char& at(int n);

**示例：**

```c++
#include<string>
#include<iostream>

using namespace std;
void test01() {

	string str = "hello world C++";
	for (int i = 0; i < str.size(); i++) {
		cout << str[i] << " ";
		str[i] = 'a';
	}
	cout << endl;
	cout << str;
	cout << endl;
	for (int i = 0; i < str.size(); i++) {
		cout << str.at(i) << " ";
	}
	cout << endl;
}


int main() {

	test01();
	return 0;
}
```



##### 3.1.8 字符串的插入和删除

函数原型

```c++
string& insert(int pos, const char* s);
string& insert(int pos, const string& str);
string& insert(int pos, int n,char c);//插入n个字符c

string& erase(int pos, int n = npos);
```



示例

```c++
#include<iostream>
#include<string>
using namespace std;

void test01() {
	string str1("wenchenwan");
	string str2("xiaoqing");
	cout << str1 << endl;
	str1.insert(2, "wewe");
	cout << str1 << endl;
	str1.insert(1, str2);
	cout << str1 << endl;
	str1.insert(0, 5, 'r');
	cout << str1 << endl;
	str1.erase(3, 2);
	cout << str1 << endl;
}

int main() {
	test01();
	return 0;
}
```



##### 3.1.9 string子串

函数原型：

```c++
string& substr(int pos,inr n=npos) const;//从pos开始的n个字符串组成的字串
```

示例

```c++
#include<iostream>
#include<string>

using namespace std;
void test01() {
	string str("wenchenwan");
	string s = str.substr(3, 3);
	cout << s << endl;
}
void test02() {
	string email = "wenchenwan@outlook.com";
	int i = email.find('@');
	string s1 = email.substr(0, i);
	cout << "用户名" << s1 <<endl;
}

int main() {

	//test01();
	test02();
	return 0;
}
```

#### 3.2 vector容器

##### 3.2.1vector基本概念

![image-20201126094848484](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201126094848484.png)

##### 3.2.2 vector构造函数

![image-20201126094946617](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201126094946617.png)

**示例：**

```C++
void test01(){
    vector<int> v;
    for (int i =0 ; i<10 ;i++) {
        v.push_back(i);
        cout << v.at(i) << endl;
    }
}

void test02(){
    vector<int> v;
    for (int i =0 ; i<10 ;i++) {
        v.push_back(i);
        //cout << v.at(i) << endl;
    }
    Show(v);
    vector<int>v2(v.begin(),v.end());
    Show(v2);

    vector<int>v3(10,100);
    Show(v3);

    vector<int> v4(v3);
    Show(v4);
}

void Show(vector<int>&v){
    for (vector<int>::iterator ItV = v.begin();ItV != v.end();ItV++) {
        qDebug() << (*ItV) << endl;
    }
}

```

##### 3.2.3 vector赋值操作

**函数原型**

- ```c++
  vector& operator=(const vector &vec)；
  ```

  

- ```c++
  assign(beg,end);//将[beg,end)区间中的数据拷贝赋值给本身
  ```

  

- ```c++
  assign(n,elem);//将n个elem拷贝给本身
  ```

  

示例：

```c++
#include<iostream>
#include<vector>
#include"MyVector.h"

using namespace std;

void PrintVector(vector<int>& v) {
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}
void test01() {
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 100; i = i + 10) {
		v1.push_back(i);
	}
	for (int i = 0; i < 100; i = i + 10) {
		v2.push_back(i);
	}
	v1[2] = 123;
	v2 = v1;
	
	v1.assign(10,5);//拷贝十个五给自身
	v1.assign(v2.begin(), v2.end()-1);
	PrintVector(v1);
	PrintVector(v2);
}
```

##### 3.2.4 vector容量与大小

**函数原型**

```c++
empty();//判断是否为空
capacity();//得到vector容量大小
size();//返回容器中元素的个数
resize(int num);//重新分配容器的长度，变长用默认值填充，变短则删除多的元素
resize(int num,elem);//默认填充为elem
```

```C++
#include"MyVector.h"


void test02() {
	vector<int> v;
	cout << v.empty() << endl;
	v.resize(10);
	PrintVector(v);

	cout << v.empty() << endl;
	PrintVector(v);

	v.resize(15, 5);
	PrintVector(v);
}
```

##### 3.2.5 vector插入和删除

**函数原型：**

```c++
pushback(ele);
popback();
insert(const_iterator pos,ele);//迭代器指向的位置，插入一个ele元素
insert(const_iterator pos,int count, ele)//迭代器指向的位置插入count个ele元素
erase(const_iterator pos);
erase(const_iterator start,const_iterator end);//删除迭代器从start到end之间的元素
clear()//删除容器中所有元素
```

**示例：**

```c++
#include"MyVector.h"

void test03() {
	vector<int> v;
	for (int i = 0; i < 100; i = i + 7) {
		v.push_back(i * 2);
	}
	PrintVector(v);
	v.pop_back();
	PrintVector(v);
	vector<int>::iterator It = v.begin();
	It++; It++;
	v.insert(It, 4);
	//cout << *It << endl;
	PrintVector(v);
	v.insert(v.begin(), 2, 100);
	PrintVector(v);
	v.erase(v.begin());
	PrintVector(v);

	v.erase(v.begin(), v.begin() + 3);
	PrintVector(v);

	v.clear();
	PrintVector(v);
}
```

##### 3.2.6 vector数据存取

**原型：**

- at(int dex);
- operator[];
- front();//返回第一个元素
- back()//返回最后一个元素

##### 3.2.7 vector 互换容器



**函数原型：**

- swap(vec); //将vec本身元素互换
- 可以进行内存收缩

![image-20201126112606801](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201126112606801.png)

**示例：**

```c++
#include"MyVector.h"

void test04() {
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i);
	}
	for (int j = 0; j < 100; j += 10) {
		v2.push_back(j);
	}
	PrintVector(v1);
	PrintVector(v2);

	v1.swap(v2);
	PrintVector(v1);
	PrintVector(v2);

	vector<int> v3;
	for (int i=0; i < 100000; i++) {
		v3.push_back(i);
	}
	cout << "v3的大小为：" << v3.size() << endl;
	cout << "v3的容量为：" << v3.capacity() << endl;
	v3.resize(3);
	cout << "v3的大小为：" << v3.size() << endl;
	cout << "v3的容量为：" << v3.capacity() << endl;
	vector<int>(v3).swap(v3);
	cout << "v3的大小为：" << v3.size() << endl;
	cout << "v3的容量为：" << v3.capacity() << endl;
}
//swap实现收缩内存

```

**vector<int> (v3)**是一个匿名对象，调用swap，然后完成以后系统会自动释放匿名对象



##### 3.2.8 预留空间

功能：

- 减少vector在动态扩展容量时的扩展次数

函数原型：

- ```c++
  reserve(int len);//容器预留n个元素长度，预留位置不初始化，元素不可访问
  ```

**示例:**

```c++
#include"MyVector.h"

void test05() {
	vector<int> v;
	int num = 0;
	int* p = NULL;
	v.reserve(100000);
	for (int i = 0; i < 100000; i++) {
		v.push_back(i);
		if (p != &v[0]) {
			p = &v[0];
			num++;
		}
	}
	cout << "num = " <<num << endl;
}
```





---------

#### 3.3 deque容器

##### 3.3.1 deque容器的基本概念



功能

- 双端数组，可以对头端和尾端进行插入和删除操作

deque和vector的区别

- vector对头部的插入和删除效率太低
- deque相对而言，对于头部的插入和删除比vector的效率高
- vector访问数组元素会比deque快，这与两者的内部实现有关

![image-20201203201317505](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201203201317505.png)

deque内部工作原理

内部有一个中控器，维护每段缓冲区中的内容，缓冲区中存放真是数据

中控器维护的是每段缓冲区的地址，是的deque时，像一片连续的内存空间

![image-20201203201704677](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201203201704677.png)

deque容器的迭代器也支持随机访问

##### 3.3.2 deque的构造函数

功能描述

- deque容器构造



**函数原型：**

- ```c++
  - deque<T> deqT; //默认构造函数
  - deque(beg,end);//构造函数将beg到end之间的元素拷贝给自身【beg，end）
  - deque(n, elem);//构造函数将n个elem拷贝给本身
  - deque(const deque &deq);//拷贝构造函数
  ```

示例

```c++
#include<iostream>
using namespace std;
#include<deque>


void ShowDeque(deque<int>& Deq) {
	for (deque<int>::iterator It = Deq.begin(); It != Deq.end(); It++) {
		cout << "*It = " << *It << endl;
	}
	cout << endl;
}

void ShowDeque1(const deque<int>& deq) {
	for (deque<int>::const_iterator It = deq.begin(); It != deq.end(); ++It) {
		//*It = 10;//有了const就不可修改
		cout << "*It = " << *It << endl;
	}
	cout << endl;
}
void test01() {
	deque<int> DequeInt;

	
	for (int i = 0; i < 10; i++) {
		DequeInt.push_back(i * 10 - 10);
		DequeInt.push_front(i * 10 + 10);
	}
	ShowDeque(DequeInt);

	deque<int>D1(DequeInt.begin(),DequeInt.end());
	ShowDeque(D1);
	
	deque<int>D2(10, 100);
	ShowDeque(D2);

	deque<int>D3(D2);
	ShowDeque(D3);
}





int main() {

	test01();
	return 0;
}
```

##### 3.3.3 deque赋值操作

**函数原型**

- deque& operator=(const deque &dqe);
- assign(beg,end);
- assign(n,elem);

**示例**

```c++
#include<iostream>
using namespace std;
#include<deque>

void ShowDeque1(const deque<int>& deq) {
	for (deque<int>::const_iterator It = deq.begin(); It != deq.end(); ++It) {
		//*It = 10;//有了const就不可修改
		cout << "*It = " << *It << endl;
	}
	cout << endl;
}

void test01() {
	deque<int> deq;
	for (int i = 0; i < 10; i++) {
		deq.push_back(i);
	}
	ShowDeque1(deq);

	deque<int> d2;
	d2 = deq;
	ShowDeque1(d2);

	deque<int> d3;
	d3.assign(d2.begin(),d2.end());
	ShowDeque1(d3);

	deque<int> d4;
	d4.assign(10, 6);
	ShowDeque1(d4);

}
int main() {
	test01();

	return 0;

}
```

##### 3.3.4  deque容器大小操作

**函数原型**

- deque.empty(); //判断容器是否为空
- deque.size(); //返回容器中元素的个数
- deque.resize(num); //重新指定容器的长度为num，容器变长用默认值填充新位置
- deque.resize(num,elem); //重新指定容器的长度num，若容器变长，用elem值填充新位置

```c++
#include<iostream>
using namespace std;
#include<deque>


void ShowDeq(const deque<int> &deq) {
	for (deque<int>::const_iterator It = deq.begin(); It != deq.end(); ++It) {
		//*It = 10;//有了const就不可修改
		cout << "*It = " << *It << endl;
	}
	cout << endl;


}
void test01() {
	deque<int> d1;
	for (int i = 0; i < 10; i++) {
		d1.push_back(i);
	}
	ShowDeq(d1);
	deque<int> d2;
	if (d2.empty()) {
		cout << " d2为空 " << endl;
	}
	for (int i = 0; i < 10; i++) {
		d2.push_back(i * 10);
	}
	cout << " d2.size() " << d2.size() << endl;

	d2.resize(5);
	cout << " d2.size() " << d2.size() << endl;

	d2.resize(15,10);
	cout << " d2.size() " << d2.size() <<endl;
}




int main() {
	test01();
	return 0;
}
```

Ps:只有大小没有容量

##### 3.3.5 deque容器插入和删除

函数原型：

**两端插入操作：**

- ```c++
  push_back(elem);
  push_front(elem);
  ```

- ```c++
  pop_back();
  pop_front();
  ```

**指定位置操作：**

```c++
insert(pos,elem);
insert(pos,n,elem);//在pos处插入n个elem元素
insert(pos,beg,end);//在pos处插入n【beg,end】区间的数据
clear();//清空容器中的所有元素
erase(beg,end);//清除[beg,end)之间的数据，返回下一个数据的位置
erase(pos);//清楚pos位置的数据，返回下一个数据的位置
```

**示例：**

```c++
#include<iostream>
#include<deque>
using namespace std;

void ShowDeq(const deque<int>& deq) {
	for (deque<int>::const_iterator It = deq.begin(); It != deq.end(); ++It) {
		//*It = 10;//有了const就不可修改

		cout << "*It = " << *It << endl;
	}
	cout << endl;
}

void test01() {
	deque<int> d1;
	for (int i = 0; i < 10; i++) {
		d1.push_back(i);
		d1.push_front(i);
	}
	ShowDeq(d1);
	d1.pop_front();
	ShowDeq(d1);
	d1.pop_back();
	ShowDeq(d1);

	d1.insert(d1.begin(), 4);
	ShowDeq(d1);
	
	d1.insert(d1.begin(), 3,100);
	ShowDeq(d1);

	deque<int> d2;
	d2.insert(d2.begin(), d1.begin(), d1.end());
	ShowDeq(d2);

	d2.erase(d2.begin());
	ShowDeq(d2);

	deque<int>::iterator It = d2.begin();
	It++;
	d2.erase(It, ++It);
	ShowDeq(d2);

	d1.clear();
	ShowDeq(d1);
}

int main() {

	test01();
	return 0;
}
```

##### 3.3.6 deque容器数据存取

**函数原型**：

```
at(int index);
operator[];
front();
back();
```

示例：

```c++
#include<iostream>
#include<deque>
using namespace std;

void ShowDeq(const deque<int>& deq) {
	for (deque<int>::const_iterator It = deq.begin(); It != deq.end(); ++It) {
		//*It = 10;//有了const就不可修改

		cout << "*It = " << *It << endl;
	}
	cout << endl;
}

void test01() {
	deque<int> d1;
	for (int i = 0; i < 5; i++) {
		d1.push_back(i);
	}
	ShowDeq(d1);
	cout << "d1.at(1) = " << d1.at(1)<< endl;
	cout << "d1.at(2) = " << d1.at(2)<< endl;

	cout << "d1[1] = " << d1[1] << endl;
	cout << "d1[2] = " << d1[2] << endl;

	cout << "d1.front() = " << d1.front() << endl;
	cout << "d1.back() = " << d1.back() << endl;

}

int main() {
	test01();
	return 0;
}
```



3.3.7 deque容器排序

**原型：**

```c++
sort(iterator beg, iterator end);//对beg和end之间的元素进行排序
```

**示例：**

```c++
#include<iostream>
#include<deque>
#include<algorithm>
using namespace std;

void ShowDeq(const deque<int>& deq) {
	for (deque<int>::const_iterator It = deq.begin(); It != deq.end(); ++It) {
		//*It = 10;//有了const就不可修改

		cout << "*It = " << *It << endl;
	}
	cout << endl;
}

void test01() {
	deque<int> d1;
	for (int i = 0; i < 10; i++) {
		d1.push_back(i);
	}
	for (int i = 10; i < 20; i++) {
		d1.push_front(i);
	}
	ShowDeq(d1);
	//支持随机访问的迭代器的容器，都可以利用sort算法进行排序
	//vector容器也支持排序算法
	sort(d1.begin(), d1.end());
	ShowDeq(d1);

}

int main() {
	test01();
	return 0;
}
```

#### 3.4 案例-评委打分

##### 3.4.1 案例描述

有五名选手ABCDE,10个评委对每一位选手进行打分，去掉最高分和最低分，取平均分。



##### 3.4.2 实现步骤

1. 创建五名选手放入vector中
2. 遍历vector容器，取出来每一个选手，执行for循环，可以把十个评委打分存到deque容器中
3. sort()对deque容器中的数据进行排序，去掉最高分和最低分
4. deque容器遍历一遍，累加总分
5. 获取平均分

示例：

person.h

```c++
#pragma once
#include<iostream>
using namespace std;

class Person
{
public:
	string m_Name;
	int m_Score;
	Person(string& name, int score);
};

```

person.cpp

```c++
#include "Person.h"

Person::Person(string& name, int score) {
	m_Name = name;
	m_Score = score;
}
```

main.cpp

```c++
#include"Person.h"
#include<vector>
#include<deque>
#include<algorithm>

void CreatePerson(vector<Person> &v) {
	string nameSeed = "ABCDE";
	for (int i = 0; i < 5; i++) {
		string name = "选手";
		name += nameSeed[i];

		int score = 0;
		v.push_back(Person(name, score));
	}
}

void Show(vector<Person>& v) {

	for (vector<Person>::iterator It = v.begin(); It != v.end(); It++) {
		cout << "姓名：" <<(*It).m_Name << " ";
		cout << "得分：" << (*It).m_Score << endl;
	}
}

void setScore(vector<Person>& v) {
	for (vector<Person>::iterator It = v.begin(); It != v.end(); It++) {
		deque<int> deq;
		cout << "请为" << (*It).m_Name << "打分" << endl;
		cout << "score :" << " ";
		for (int i = 0; i < 10; i++) {
			cout << "请第" << i+1 << "个评委打分" << endl;
			cout << "score " << i+1 <<":";
			int score;
			//cin >> score;
			score = rand() % 41 + 60;
			cout << score << endl;
			deq.push_back(score);
		}
		sort(deq.begin(),deq.end());
		deq.pop_back();
		deq.pop_front();
		
		for (deque<int>::iterator Itq = deq.begin(); Itq != deq.end(); ++Itq) {
			(*It).m_Score = ((*It).m_Score + (*Itq))/2;
		}
	}
}

int main() {
	//1.创建5名选手
	vector<Person> v;
	CreatePerson(v);

	for (vector<Person>::iterator It = v.begin(); It != v.end(); It++) {
		cout <<(*It).m_Name << " ";
		cout << (*It).m_Score << endl;
	}
	//2.给五名选手打分
	setScore(v);
	Show(v);

	return 0;
}
```

#### 3.5 stack容器

##### 3.5.1 stack容器的基本概念

**概念：**

stack是一种先进后出的数据结构（first in last out），她只有一个出口

![image-20201204103239317](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201204103239317.png)



只有栈顶的元素才能被使用，栈不允许有遍历操作

##### 3.5.2 stack的基本接口

构造函数：

- ```c++
  stack<T> stk;
  ```

- ```c++
  stack(const stack &stk);//拷贝构造函数
  ```

赋值操作：

- ```c++
  stack operator=(const stack &stk);
  ```

数组存取

- ```c++
  push(elem);
  pop();
  top();//返回栈顶元素
  ```

大小操作：

- ```c++
  empty();//判断栈是否为空
  size();//返回栈的大小
  ```

**示例：**

```c++
#include<iostream>
using namespace std;
#include<stack>

void test01() {
	stack<int> stk;
	stack<int> stk1;
	stk1 = stk;
	for (int i = 0; i < 10; i++) {
		stk.push(i);
	}
	cout << "栈的大小为：" << stk.size() << endl;
	while (stk.top() != NULL) {
		if (stk.empty()) {
			cout << "栈为空" << endl;
		}
		cout << "top = " << stk.top() << endl;
		stk.pop();
	}
	
}

int main() {
	test01();
	return 0;
}

```

#### 3.6 queue容器

##### 3.6.1 queue常用的一些概念

**概念:**queue是一种先进先出的（first in first out）的数据结构，它有两个出口

![image-20201204105434755](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201204105434755.png)

队列容器允许从一端新增元素，从另外一段移除元素

队列中只有队头和队尾可以被外界使用，因此队列不允许有遍历的行为

队列中进数据称为---入队 push

队列中出数据称为---出队pop

##### 3.6.2 queue 常用的接口

构造函数

- ```c++
  queue<T> que;
  queue(const queue &que);//拷贝构造函数
  ```

赋值操作

- ```c++
  queue operator=(const queue &que);
  ```

数据存取

- ```c++
  push(elem);
  pop();
  front();//返回第一个元素
  back();//返回最后一个元素
  ```

大小操作

- ```c++
  empty();//判断队列是否为空
  size();//返回队列的大小
  ```

代码示例

```c++
#include<iostream>
using namespace std;
#include<queue>

void test01() {
	queue<int> q1;
	q1.push(10);
	q1.push(20);
	q1.push(30);
	cout << "q1的最后一个元素是：" << q1.back() << endl;
	cout << "q1的第一个元素是：" << q1.front() << endl;
	cout << "q1的大小为：" << q1.size() << endl;
	q1.pop();
	cout << "q1的最后一个元素是：" << q1.back() << endl;
	cout << "q1的第一个元素是：" << q1.front() << endl;
	cout << "q1的大小为：" << q1.size() << endl;
	q1.push(30);
	cout << "q1的最后一个元素是：" << q1.back() << endl;
	cout << "q1的第一个元素是：" << q1.front() << endl;
	cout << "q1的大小为：" << q1.size() << endl;
	while (!q1.empty())
	{
		q1.pop();
	}
	cout << "q1的大小为：" << q1.size() << endl;
}

int main() {
	test01();
	return 0;
}
```



#### 3.7 list容器

##### 3.7.1 list的基本概念

功能：将数据链式存储

链表(list)：是一种物理上非连续的存储结构数据元素的逻辑顺序是通过链表的指针实现的

链表由一系列的节点组成

节点的组成包括数据域和指针域

STL中链表是一个双向循环链表

![image-20201204170353282](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201204170353282.png)

由于链表的存储方式不是连续的内存空间，因此链表list中的迭代器只支持前移和后移，属于双向迭代器



list的优点

- 采用动态存储分配，不会造成内存的浪费和溢出
- 链表执行插入和删除操作十分方便，修改指针即可，不需要移动大量元素

list的缺点：

- 链表灵活，但是空间(指针域)和时间(遍历时间)额外消耗比较大

list有一个性质

插入和删除都不会让原来的list迭代器失效，在vector下不成立

**总结:**list和vector是STL中最常用的两个容器



##### 3.7.2 list构造函数

**函数原型：**

```c++
list<T> lst;
list(beg,end);//将beg到end的元素拷贝给自身
list(n,elem);//将n个elem元素赋值给自身
list(const list &lst);//拷贝构造函数
```

**示例：**

```c++
#include<iostream>
using namespace std;
#include<list>

void Printlist(list<int>& lst) {
	cout << "lst的值为：";
	for (list<int>::iterator It = lst.begin(); It != lst.end(); It++) {
		cout << (*It) << " ";
	}
	cout << endl;
}

void test01() {
	list<int> lst;
	lst.push_back(10);
	lst.push_back(20);
	lst.push_back(30);
	Printlist(lst);
	list<int> lst1(lst.begin(), lst.end());
	Printlist(lst1);

	list<int> lst2(lst);
	Printlist(lst2);
	
	list<int> lst3(5, 6);
	Printlist(lst3);

}

int main() {
	test01();
	return 0;
}
```





##### 3.7.3 list赋值与交换

**函数原型：**

```c++
assign(beg,end);//将beg到end的元素拷贝给自身
assign(n,elem);//将n个elem元素赋值给自身
list& operator=(const list &lst);
swap(list);//将lst的元素与本身交换
```

**示例：**

```c++
#include<iostream>
#include<list>
using namespace std;

void printlist(list<int>& lst) {
	cout << "lst的值为：";
	for (list<int>::iterator it = lst.begin(); it != lst.end(); it++) {
		cout << (*it) << " ";
	}
	cout << endl;
}

void test01() {
	list<int> lst;
	lst.push_back(10);
	lst.push_back(20);
	lst.push_back(30);
	printlist(lst);
	list<int> lst1;
	lst1.assign(lst.begin(), lst.end());
	printlist(lst1);

	list<int> lst2;
	lst2.assign(10, 100);
	printlist(lst2);
	
	lst2.swap(lst);
	printlist(lst2);
	printlist(lst);
	
	lst1 = lst;
	printlist(lst1);


}

int main() {
	test01();
	return 0;
}
```



##### 3.7.4 list容器大小操作

**函数原型：**

```c++
size();
empty();
resize();
resize(num,elem);
```

示例：

```c++
#include<iostream>
#include<list>c++
using namespace std;

void printlist(const list<int>& lst) {
	cout << "lst的值为：";
	for (list<int>::const_iterator it = lst.begin(); it != lst.end(); it++) {
		cout << (*it) << " ";
	}
	cout << endl;
}

void test01() {
	list<int> lst;
	lst.push_back(10);
	lst.push_back(10);
	lst.push_back(10);
	lst.push_back(10);

	if (!lst.empty()) {
		cout << "list不为空" << endl;
	}
	
	cout << "list的大小为" << lst.size() << endl;
	
	lst.resize(10);
	printlist(lst);
	
	lst.resize(15, 5);
	printlist(lst);


}

int main() {
	test01();
	return 0;
}
```

##### 3.7.5 list的插入和删除

**函数原型：**

```c++
push_back(elem);
pop_back();
push_front(elem);
pop_front();
insert(pos,elem);//只能使用迭代器，返回新数据的位置
insert(pos,beg,end)//在pos处插入beg到end之间的元素（无返回值）
clear();
erase(beg,end);//删除beg到end之间的数据，返回下一个数据的位置
erase(pos);//删除pos位置的数据，返回下一个数据的位置
remove(elem);//移除list中所有与elem匹配的元素
```

**示例：**

```c++
#include<iostream>
#include<list>
using namespace std;

void printlist(const list<int>& lst) {
	cout << "lst的值为：";
	for (list<int>::const_iterator it = lst.begin(); it != lst.end(); it++) {
		cout << (*it) << " ";
	}
	cout << endl;
}

void test01() {
	list<int> lst1;
	lst1.push_back(-2);
	lst1.push_back(-2);
	lst1.push_back(-2);
	lst1.push_back(-2);

	list<int> lst;
	for (int i = 0; i < 10; i++) {
		lst.push_back(i * 10 / 3);
		lst.push_front(i * 10 / 4);
	}
	printlist(lst);
	
	lst.remove(10);
	printlist(lst);
	
	for (list<int>::iterator It = lst.begin(); It != lst.end(); It++) {
		if ((*It) < 5) {
			lst.insert(It,-1);
			lst.insert(It, lst1.begin(), lst1.end());
		}
	}
	printlist(lst);
	
	lst.erase(lst.begin());
	printlist(lst);

}

int main() {
	test01();
	return 0;
}
```

##### 3.7.6 容器的数据存取

**函数原型：**

```c++
front();
back();
```

**注意:**list迭代器不支持随机访问，只能通过迭代器来**++/--**实现访问，不能对对迭代器进行+1，+2，+3这种操作
It = It +1 ;//不可以c++

**示例：**

```c++
#include<iostream>
#include<list>
using namespace std;

void printlist(const list<int>& lst) {
	cout << "lst的值为：";
	for (list<int>::const_iterator it = lst.begin(); it != lst.end(); it++) {
		cout << (*it) << " ";
	}
	cout << endl;
}

void test01() {
	list<int> lst;
	for (int i = 0; i < 5; i++) {
		lst.push_back(i);
	}
	printlist(lst);

	cout << "lst.back() = " << lst.back() << " "
		<< "lst.front() = " << lst.front() << endl;

}

int main() {
	test01();
	return 0;
}
```

##### 3.7.7 list排序和反序

**函数原型：**

```c++
sort();
reverse();
```

**注意** ：所有不支持随机访问的容器不能使用标准的排序算法sort();

**示例：**

```c++
#include<iostream>
#include<list>
using namespace std;

void printlist(const list<int>& lst) {
	cout << "lst的值为：";
	for (list<int>::const_iterator it = lst.begin(); it != lst.end(); it++) {
		cout << (*it) << " ";
	}
	cout << endl;
}

void test01() {
	list<int> lst;
	lst.push_back(10);
	lst.push_back(20);
	lst.push_back(30);
	lst.push_back(15);

	printlist(lst);
	
	lst.sort();
	printlist(lst);
	
	lst.reverse();
	printlist(lst);

}

int main() {
	test01();
	return 0;
}
```

##### 3.7.8 list容器排序案例

**要求：**

1. 按person自定义数据类型来排序，person属性由姓名年龄身高
2. 排序规则，按照年龄进行升序排列，如果年龄相同按身高降序

**示例：**

```c++
#include<iostream>
#include<list>
using namespace std;

class Person {
public:
	Person(string name, int age, double hgt) {
		this->m_Name = name;
		this->m_Age = age;
		this->m_Higt = hgt;
	}
	string m_Name;
	int m_Age;
	double m_Higt;
};



void prinPerson(const Person& person) {
	cout << "name :" <<person.m_Name << " "
		 << "age :" << person.m_Age << " "
		 << "hight :" <<person.m_Higt << endl;
}

//指定排序规则
bool CompareAge(Person& p1, Person& p2) {
	if (p1.m_Age == p2.m_Age) {
		//年龄升序
		return p1.m_Higt > p2.m_Higt;
	}
	else
	{
		//身高升序
		return p1.m_Age > p2.m_Age;
	}
}
void ShowPersonList(list<Person>& lst) {
	for (list<Person>::iterator It = lst.begin(); It != lst.end(); It++) {
		prinPerson(*It);
	}
}
void test01() {
	

	list<Person> P;
	string name_seed = "ABCDEFGHIGKLMNOPQ";
	for (int i = 0; i < 10; i++) {
		string name = "name";
		name = name + name_seed[i];
		int age = rand() % 100;
		double hgt = rand() % 100 + 100;
		Person p(name, age, hgt);
		P.push_back(p);
	}
	ShowPersonList(P);
	cout << "__________________" << endl;
	cout << "排序后" << endl;
	P.sort(CompareAge);
	ShowPersonList(P);

}

int main() {
	test01();
	return 0;
}
```

**注意:**
在对自定义数据类型进行排序的时候，需要指定排序的规则，否则编译器无法识别排序规则

#### 3.8  set容器

##### 3.8.1 set的基本概念

**简介：**

- 所有元素插入时都会被自动排序

**本质：**

- set/multiset属于关联式容器，底层使用二叉树实现



**set和multiset的区别：**

- set不允许元素中有重复的元素
- muliti中允许有重复的元素



##### 3.8.2 set的构造和赋值

构造：

- ```c++
  set<T> st;
  set(const set &st);//拷贝构造
  ```

赋值：

- ```c++
  set &operator=(const set &st);
  ```



示例：

```c++
#include<iostream>
#include<set>
using namespace std;

void PrintSet(const set<int>& st) {
	for (set<int>::const_iterator It = st.begin(); It != st.end(); It++) {
		cout << (*It) << " ";
	}
	cout << endl;
}


void test01() {
	set<int> s1;
	
	//插入数据只有insert方法
	int i = 110;
	while (i=i-10)
	{
		s1.insert(i);
		if (i < 0)
			break;
	}
	PrintSet(s1);
	s1.insert(100);//不能插入相同的元素

	PrintSet(s1);
	set<int> s2(s1);
	set<int> s3;
	PrintSet(s2);
	s3 = s2;
	PrintSet(s3);

}

int main() {
	test01();
	return 0;
}
```

##### 3.8.3 set的大小和交换



**函数原型：**

- ```c++
  size();
  ```

- ```c++
  empty();
  ```

- ```c++
  swap(st);
  ```

  

**示例：**

```c++
#include<iostream>
#include<set>
using namespace std;

void PrintSet(const set<int>& st) {
	for (set<int>::const_iterator It = st.begin(); It != st.end(); It++) {
		cout << (*It) << " ";
	}
	cout << endl;
}


void test01() {
	set<int> s1;

	//插入数据只有insert方法
	int i = 110;
	while (i = i - 10)
	{
		s1.insert(i);
		if (i < 0)
			break;
	}
	cout << "s1 为：";
	PrintSet(s1);
	cout << "s1.size() = " << s1.size() << endl;
	set<int> s2;
	if (s2.empty())
		cout << "s2为空" << endl;

	s2.insert(5);
	s2.insert(15);
	s2.insert(25);
	s2.insert(35);
	s2.insert(45);
	cout << "s2 为：";
	PrintSet(s2);
	cout << "交换后" << endl;
	s1.swap(s2);
	cout << "s1 为：";
	PrintSet(s1);
	cout << "s2 为：";
	PrintSet(s2);

}

int main() {
	test01();
	return 0;
}
```

##### 3.8.4 set容器的插入和删除



**函数原型：**

- ```c++
  insert();
  ```

- ```c++
  clear();
  ```

- ```c++
  erase(pose);//传入一个位置（迭代器），删除迭代器指向的元素，并返回下一个元素的迭代器
  ```

- ```c++
  erase(beg,end);//删除[beg,end)之间的所有元素，返回下一个元素的迭代器
  ```

- ```c++
  erase(elem);//删除容器中的elem元素；
  ```

**示例:**

```c++
#include<iostream>
#include<set>
using namespace std;

void PrintSet(const set<int>& st) {
	for (set<int>::const_iterator It = st.begin(); It != st.end(); It++) {
		cout << (*It) << " ";
	}
	cout << endl;
}


void test01() {
	set<int> s1;

	//插入数据只有insert方法
	int i = 110;
	while (i = i - 10)
	{
		s1.insert(i);
		if (i < 0)
			break;
	}
	PrintSet(s1);
	set<int>::iterator It = s1.begin();
	It++;
	It++;
	cout << "It的值" << *It << endl;
	cout << "s1.erase(It)的返回值" << *s1.erase(It) << endl;
	PrintSet(s1);
	It = s1.begin();
	s1.erase(s1.begin(), ++(++(++It)));
	PrintSet(s1);

	s1.erase(80);
	PrintSet(s1);


}

int main() {
	test01();
	return 0;
}
```



##### 3.8.5 set容器的查找和统计



**函数原型：**

- ```c++
  find(elem);//返回elem的迭代器，未查找到返回end
  ```

- ```c++
  count(key);//统计key出现的次数
  ```



示例：

```c++
#include<iostream>
#include<set>
using namespace std;

void PrintSet(const set<int>& st) {
	for (set<int>::const_iterator It = st.begin(); It != st.end(); It++) {
		cout << (*It) << " ";
	}
	cout << endl;
}
void PrintMulitiSet(const multiset<int>& st) {
	for (multiset<int>::const_iterator It = st.begin(); It != st.end(); It++) {
		cout << (*It) << " ";
	}
	cout << endl;
}


void test01() {
	set<int> s1;

	//插入数据只有insert方法
	int i = 110;
	while (i = i - 10)
	{
		s1.insert(i);
		if (i < 0)
			break;
	}
	PrintSet(s1);
	if (s1.find(100)!=s1.end()) {
		cout << "已经查找到该元素" << endl;
	}
	multiset<int> muSt;
	muSt.insert(10);
	muSt.insert(30);
	muSt.insert(20);
	muSt.insert(10);
	muSt.insert(50);
	muSt.insert(10);
	PrintMulitiSet(muSt);
	cout << "10出现的次数：" << muSt.count(10) << endl;

}

int main() {
	test01();
	return 0;
}
```

##### 3.8.6 multiset和set的区别

**区别：**

- set不可以插入相同的值，而multiset可以插入相同的元素
- set插入元素会返回插入的结果，表示插入成功
- multiset不会检测数据的插入结果，因此可以插入重复的数据



**示例：**

```c++
#include<iostream>
#include<set>
using namespace std;

void PrintSet(const set<int>& st) {
	for (set<int>::const_iterator It = st.begin(); It != st.end(); It++) {
		cout << (*It) << " ";
	}
	cout << endl;
}


void test01() {
	set<int> s1;
	pair<set<int>::iterator,bool> ret = s1.insert(10);
	if (ret.second) {
		cout << "插入成功" << endl;
	}
	else
	{
		cout << "插入失败" << endl;
	}
	ret = s1.insert(10);
	if (ret.second) {
		cout << "插入成功" << endl;
	}
	else
	{
		cout << "插入失败" << endl;
	}
	multiset<int> ms;
	ms.insert(10);//返回值为iterator迭代器


}

int main() {
	test01();
	return 0;
}
```

##### 3.8.7 pair使用和pair对组的创建

**功能描述：**

成对出现的数据，利用pair返回两个数据



**两种创建方法：**

- ```c++
  pair(type,type) p(val1,val2);
  ```

- ```c++
  pair<type,type> p = make_pair(val1,val2);
  ```

**示例：**

```c++
#include<iostream>
using namespace std;

void test01() {
	pair<string, int> p1("老柳", 20);
	pair<string, double> p2 = make_pair("老张", 20.3);

	cout << "姓名：" << p1.first << " "<< "年龄：" << p1.second << endl;
	cout << "姓名：" << p2.first << " " << "身高：" << p2.second << endl;
}


int main() {
	test01();
	return 0;
}
```

3.8.8 set 容器排序

学习目标：

- set容器默认提供排序规则从小到大，掌握如何改变排序规则

主要技术点：

- 利用仿函数改变排序规则

示例一 set存放内置数据类型

```c++
#include<set>
#include<iostream>
using namespace std;
void PrintSet(const set<int>& st) {
	for (set<int>::const_iterator It = st.begin(); It != st.end(); It++) {
		cout << (*It) << " ";
	}
	cout << endl;
}

//仿函数

class MyCompare {
public:
	bool operator()(const int v1,const int v2) const{
		return v1 > v2;
	}
};

void test01() {
	set<int> s1;
	s1.insert(10);
	s1.insert(30);
	s1.insert(20);
	s1.insert(40);
	PrintSet(s1);

	//给定排序规则
	set<int,MyCompare> s2;
	s2.insert(10);
	s2.insert(30);
	s2.insert(20);
	s2.insert(40);
	//PrintSet(s2);
	for (set<int, MyCompare>::iterator It = s2.begin(); It != s2.end(); It++) {
		cout << (*It) <<" ";
	}
	cout << endl;
}

int main() {
	test01();
	return 0;
}
```

**示例二 set存放自定义数据类型**

```c++
#include<iostream>
#include<set>
using namespace std;

class Person {
public:
	Person(string name, int age) {
		this->M_age = age;
		this->M_name = name;
	}

	string M_name;
	int M_age;
};
class MyCompare {
public:
	bool operator()(const Person& v1, const Person& v2) const {
		return v1.M_age > v2.M_age;
	}

};
void test01() {
	//自定义数据类型都会指定排序方式
	set<Person,MyCompare>s1;
	Person p1("张三", 20);
	Person p2("李四", 30);
	Person p3("王五", 25);

	s1.insert(p1);
	s1.insert(p2);
	s1.insert(p3);
	for (set<Person,MyCompare>::iterator It = s1.begin(); It != s1.end(); It++) {
		cout << (*It).M_name << " " <<(*It).M_age << endl;
	}

}
int main() {
	test01();
	return 0;
}
```

对于自定义数据类型必须指定一个排序规则才能使用



#### 3.9 map/multimap容器

##### 3.9.1 map基本概念

**简介：**

- map中的所有元素都是pair
- pair中的第一个元素为key（键值），第二个元素为value（实值）
- 所有的元素会根据元素的键值自动排序

**本质:**

- map/multimap属于关联式容器，底层使用二叉树实现

**优点：**

- 可以根据键值迅速查找到value值

**map和multimap的区别**

- map不允许有重复的key值元素
- multimap允许容器中有重复的key值元素



##### 3.9.2 map的构造和析构



**函数原型：**

- ```c++
  map<T1,T2> mp;
  ```

- ```c++
  map(const map &mp);
  ```

**赋值：**

- ```c++
  map &operator=(const map &mp);
  ```



**示例：**

```c++
#include<iostream>
#include<map>
using namespace std;
void PrintMap(map<int, string> Map) {
	for (map<int, string>::iterator It = Map.begin(); It != Map.end(); It++) {
		cout << (*It).first << " " << (*It).second << endl;
	}
	cout << endl;
}

void test01() {

	map<int, string>mp;
	mp.insert(pair<int, string>(10, "小刘"));
	mp.insert(pair<int, string>(20, "小张"));
	mp.insert(pair<int, string>(30, "小吴"));
	mp.insert(pair<int, string>(40, "小李"));

	PrintMap(mp);

	map<int, string>mp1(mp);
	PrintMap(mp1);
	map<int, string>mp2;
	mp2 = mp;
	PrintMap(mp2);
}

int main() {
	test01();
	return 0;
}
```



**map中所有的元素都成对出现，插入时候需要使用对组**



##### 3.9.3 map容器大小和交换

 **函数原型：**

- ```c++
  size();
  ```

- ```c++
  empty();
  ```

- ```c++
  swap();
  ```

**示例：**

```c++
#include<iostream>
#include<map>
using namespace std;

void PrintMap(map<int, string> Map) {
	for (map<int, string>::iterator It = Map.begin(); It != Map.end(); It++) {
		cout << (*It).first << " " << (*It).second << endl;
	}
	cout << endl;
}


void test02() {
	map<int, string> mp;
	mp.insert(pair<int, string>(05, "小技"));
	mp.insert(pair<int, string>(10, "小刘"));
	mp.insert(pair<int, string>(20, "小张"));
	mp.insert(pair<int, string>(30, "小吴"));
	mp.insert(pair<int, string>(40, "小李"));

	if (!mp.empty()) {
		cout << "mp不为空" << endl;
	}

	cout << "mp的大小为：" << mp.size() << endl;
	PrintMap(mp);

	map<int, string> mp1;
	mp1.insert(pair<int, string>(100, "唐三藏"));
	mp1.swap(mp);
	PrintMap(mp);

}


int main() {
	test02();
	return 0;
}
```

##### 3.9.4 map容器插入和删除

**函数原型：**

- ```c++
  insert(elem);
  ```

- ```c++
  clear();
  ```

- ```c++
  erase(pose);//pos为迭代器所指的元素，返回下一个元素的迭代器
  ```

- ```c++
  erase(beg,end);//删除[beg,end)的所有元素，返回下一个元素的迭代器
  ```

- ```c++
  erase(key);//删除容器中的key元素
  ```



**示例：**

```c++
#include<iostream>
#include<map>
using namespace std;

void PrintMap(map<int, string> Map) {
	for (map<int, string>::iterator It = Map.begin(); It != Map.end(); It++) {
		cout << (*It).first << " " << (*It).second << endl;
	}
	cout << endl;
}



void test01() {
		map<int, string> mp;
		mp.insert(pair<int, string>(05, "小技"));
		mp.insert(pair<int, string>(10, "小刘"));
		mp.insert(pair<int, string>(20, "小张"));
		mp.insert(pair<int, string>(30, "小吴"));
		mp.insert(pair<int, string>(40, "小李"));
		map<int, string>::iterator it = mp.begin();
		it++;
		PrintMap(mp);
		//mp.erase(it);
		//PrintMap(mp);
		mp.erase(mp.begin(), ++it);
		PrintMap(mp);

		mp.erase(40);
		PrintMap(mp);

		mp.clear();
		PrintMap(mp);
}

int main() {

	test01();
	return 0;
}
```



##### 3.9.5 map容器统计和查找

**函数原型：**

- ```c++
  find(key);//若查找到，则返回该元素的迭代器，否则返回set.end
  ```

- ```c++
  count(key);//统计key元素的个数
  ```



**示例：**

```c++
#include<iostream>
#include<map>
using namespace std;

void PrintMap(map<int, string> Map) {
	for (map<int, string>::iterator It = Map.begin(); It != Map.end(); It++) {
		cout << (*It).first << " " << (*It).second << endl;
	}
	cout << endl;
}



void test01() {
		map<int, string> mp;
		mp.insert(pair<int, string>(05, "小技"));
		mp.insert(pair<int, string>(10, "小刘"));
		mp.insert(pair<int, string>(20, "小张"));
		mp.insert(pair<int, string>(30, "小吴"));
		mp.insert(pair<int, string>(40, "小李"));
		map<int, string>::iterator it = mp.begin();
		it++;
		PrintMap(mp);
		if(mp.find(20) != mp.end()){
            cout << (*mp.find(20)).first << " " << (*mp.find(20)).second << endl;
        }
		

		cout << "key为20出现的次数" << mp.count(20) << endl;
}

int main() {

	test01();
	return 0;
}
```

##### 3.9.6 map容器排序



示例

```
#include<iostream>
#include<map>
using namespace std;

class MyCompare {
public:
	bool operator()(int v1,int v2) const{
		return v1 > v2;
	}
};

//指定排序类型规则

void PrintMap(map<int, string, MyCompare> Map) {
	for (map<int, string>::iterator It = Map.begin(); It != Map.end(); It++) {
		cout << (*It).first << " " << (*It).second << endl;
	}
	cout << endl;
}



void test01() {
	map<int, string, MyCompare> mp;
	mp.insert(pair<int, string>(05, "小技"));
	mp.insert(pair<int, string>(10, "小刘"));
	mp.insert(pair<int, string>(20, "小张"));
	mp.insert(pair<int, string>(30, "小吴"));
	mp.insert(pair<int, string>(40, "小李"));
	map<int, string>::iterator it = mp.begin();
	it++;
	PrintMap(mp);



}

int main() {

	test01();
	return 0;
}
```



**总结：**

- 利用仿函数指定map的排序规则
- 对于自定义的数据类型，map必须指定排序规则，同set容器

#### 3.10 案例-员工分组

##### 3.10.1案例描述

- 公司招聘了10个员工（ABCDEFGHIJ），10名员工进入公司以后，需要指派员工去哪个部门工作
- 员工信息有：姓名 工资组成；部门分为：策划 美术 研发
- 随机给十名员工进行部门和工资分配
- 通过multimap进行信息插入，key（部门编号）value（员工）
- 分部门显示员工信息



##### 3.10.2 实现步骤

1. 创建十名员工放进vector
2. 遍历vector容器，取出每一个员工进行随机分组
3. 分组后，将员工部门编号作为key，集体员工的作为value,放入multimap容器里面
4. 分部门显示员工信息

**代码案例：**

```c++
#include<iostream>
#include<map>
#include<vector>
#include<ctime>

using namespace std;

class Mycompare {
public:
	bool operator()(int v1, int v2) const {
		return v1 < v2;
	}
};


class Worker {
public:
	Worker() {

	}
	Worker(string name, int salary) {
		this->m_Name = name;
		this->Salary = salary;
	}
	string m_Name;
	int Salary;
};

void CreateWorker(vector<Worker> &v) {

	string name_seed = "ABCDEFGHIJKL";
	for (int i = 0; i < 10; i++) {
		Worker w;
		string name = "Worker";
		name = name + name_seed[i];
		w.m_Name = name;
		w.Salary = rand() % 500 + 2500;
		v.push_back(w);

	}

}
void PrintWorker(const vector<Worker>& v) {
	for (vector<Worker>::const_iterator it = v.begin(); it != v.end(); it++) {
		cout << "姓名：" << (*it).m_Name << " 薪资：" << (*it).Salary << endl;
	}
}

void SetGroup(multimap<int, Worker, Mycompare> &mulWorker, vector<Worker> &v) {
	for (vector<Worker>::iterator it = v.begin(); it != v.end(); it++) {
		int id = rand() % 3;
		mulWorker.insert(pair<int, Worker>(id, (*it)));
	}
}

void PrintGroup(const multimap<int, Worker,Mycompare> G1) {
/*	for (multimap<int, Worker, Mycompare>::const_iterator it = G1.begin(); it != G1.end(); it++) 
		if ((*it).first == 0)
			cout << "策划  " << "人员信息：" << "姓名: " << (*it).second.m_Name << "薪资" << (*it).second.Salary << endl;
	for (multimap<int, Worker, Mycompare>::const_iterator it = G1.begin(); it != G1.end(); it++)
		if ((*it).first == 1)
			cout << "美术  " << "人员信息：" << "姓名: " << (*it).second.m_Name << "薪资" << (*it).second.Salary << endl;
	for (multimap<int, Worker, Mycompare>::const_iterator it = G1.begin(); it != G1.end(); it++)
		if ((*it).first == 1)
			cout << "研发  " << "人员信息：" << "姓名: " << (*it).second.m_Name << "薪资" << (*it).second.Salary << endl;
*/
	multimap<int, Worker, Mycompare>::const_iterator pos = G1.find(0);
	int index = 0;
	int count = G1.count(0);
	cout << "策划部门" << endl;
	for (; pos != G1.end() && index < count; pos++, index++) {
		cout << "姓名: " << (*pos).second.m_Name << "薪资" << (*pos).second.Salary << endl;
	}
	cout << "--------------------------------" << endl;

	pos = G1.find(1);
	index = 0;
	count = G1.count(1);
	cout << "策划部门" << endl;
	for (; pos != G1.end() && index < count; pos++, index++) {
		cout << "姓名: " << (*pos).second.m_Name << "薪资" << (*pos).second.Salary << endl;
	}
	cout << "--------------------------------" << endl;

	pos = G1.find(2);
	index = 0;
	count = G1.count(2);
	cout << "策划部门" << endl;
	for (; pos != G1.end() && index < count; pos++, index++) {
		cout << "姓名: " << (*pos).second.m_Name << "薪资" << (*pos).second.Salary << endl;
	}
	cout << "--------------------------------" << endl;
}

void test01() {

	srand((unsigned int)time(NULL));
	//创建员工
	vector<Worker> v1;
	CreateWorker(v1);

	//员工分组
	multimap<int, Worker,Mycompare> mWorker;
	SetGroup(mWorker,v1);
	PrintWorker(v1);
	PrintGroup(mWorker);

}

int main() {

	test01();
	return 0;
}
```



### 4 STL-函数对象

#### 4.1 函数对象

##### 4.1.1 函数对象的基本概念

**概念：**

- **重载函数调用操作符**的类，其对象称为**函数对象**
- **函数对象**使用（）重载时，行为类似函数的调用，也叫**仿函数**

**本质**

函数对象（仿函数）是一个类，不是一个函数

##### 4.1.2 函数对象的使用

**特点：**

- 函数对象在使用时候，可以像普通函数那样调用
- 函数对象超出普通函数的概念，函数对象可以有自己的状态
- 函数对象可以做参数传递



**示例：**

```c++
#include<iostream>

using namespace std;

class MyAdd {
public:
	int operator()(int v1, int v2) {
		return v1 + v2;
	}
};

class MyPrint {
public:
	MyPrint() {
		this->count = 0;
	}
	void operator()(string tset) {
		count++;
		cout << tset << endl;
	}
	//函数对象超出普通函数的概念，函数对象可以有自己的状态
	int count ;
};

//函数对象作为参数传递
void DoPrint(MyPrint & mp,string test) {
	mp(test);
}


void test01() {

	MyAdd myadd;
	cout << myadd(10, 20);
	
}

void test02() {
	
	MyPrint myprint;
	myprint("Hello World");
	myprint("Hello World");
	myprint("Hello World");
	myprint("Hello World");
	myprint("Hello World");
	cout << "调用的次数为：" << myprint.count << endl;
}

void test03() {
	MyPrint myPint;
	DoPrint(myPint, "hello C++");
}

int main() {
	test03();
	return 0;
}
```



#### 4.2 谓词

##### 4.2.1 谓词的基本概念

**概念**

- 返回bool类型的仿函数称为谓词
- 如果operator()接受一个参数，叫做一元谓词
- 如果operator()接受两个参数，叫做二元谓词

##### 4.2.2 一元谓词

**示例：**

```c++
#include<iostream>
#include<vector>
#include<algorithm>

using namespace std;

class GreaterFive {
public:
	bool operator()(int val) {
		if (val > 5) {
			return true;
		}
		return false;
	}
};
void test01() {
	vector<int>v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}
	//查找容器中 有没有大于5的数字
	//GreaterFive 匿名函数对象
	vector<int>::iterator it = find_if(v.begin(),v.end(),GreaterFive());
	if (it == v.end()) {
		cout << "没有查询到" << endl;
	}
	else
	{
		cout << "已经查询到" << *it << endl;
	}
}

int main() {

	test01();
	return 0;
}
```

##### 4.2.3 二元谓词

**示例：**

```c++
#include<iostream>
#include<vector>
#include<algorithm>

using namespace std;

class MyCompare {
public:
	bool operator()(int v1, int v2) {
		return v1 > v2;
	}
};
void test01() {
	vector<int>v;
	v.push_back(20);
	v.push_back(10);
	v.push_back(30);
	v.push_back(50);
	v.push_back(40);
	//MyCompare()匿名函数对象

	sort(v.begin(), v.end(),MyCompare());
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;

}

int main() {

	test01();
	return 0;
}
```

#### 4.3 内建函数对象

##### 4.3.1 内建函数对象的意义

**概念：**

1. STL内建了一些函数对象

**分类：**

1. 算数仿函数
2. 关系仿函数
3. 逻辑仿函数

**用法：**

- 这些仿函数所产生的对象，用法和一般的函数完全相同

- 使用内建函数对象，需要引入头文件*#include<functional>*

  

##### 4.3.2 算术仿函数

**功能描述：**

- 实现四则运算
- 其中negate是一元运算，其他都是二元运算

**仿函数原型：**

- ```c++
  template<class T> T plus<T>
  ```

- ```c++
  template<class T> T minus<T>
  ```

- ```c++
  template<class T> T mutiplies<T>
  ```

- ```c++
  template<class T> T divides<T>
  ```

- ```c++
  template<class T> T modulus<T>
  ```

- ```c++
  template<class T> T negate<T>
  ```

**示例：**

```c++
#include<iostream>
using namespace std;
#include<functional>



void test01() {
	negate<int>n;
	cout << n(50) << endl;

	plus<int> p;
	p(100, 10);
	cout << p(100, 10) << endl;

}

int main() {
	test01();
	return 0;
}
```



##### 4.3.3 关系仿函数



**函数原型：**

- ```c++
  template<class T> bool equal_to<T>
  ```

- ```c++
  template<class T> bool not_equal_to<T>
  ```

- ```c++
  template<class T> bool greater<T>
  ```

- ```c++
  template<class T> bool greater_equal<T>
  ```

- ```C++
  template<class T> bool less<T>
  ```

- ```c++
  template<class T> bool less_equal<T>
  ```



**示例：**

```c++
#include<iostream>
using namespace std;
#include<functional>
#include<vector>
#include<algorithm>

class MyCompare {
public:
	bool operator()(int v1, int v2) {
		return v1 > v2;
	}
};

void test01() {
	greater<int> p;
	if (!p(10, 20)) {
		cout << "大于" << endl;
	}

	vector<int> v;
	v.push_back(50);
	v.push_back(10);
	v.push_back(30);
	v.push_back(40);
	v.push_back(20);
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << (*it) << " ";
	}
	cout << endl;
	//sort(v.begin(),v.end(),MyCompare());

	//for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
	//	cout << (*it) << " ";
	//}
	sort(v.begin(), v.end(), less<int>());
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << (*it) << " ";
	}
}

int main() {
	test01();
	return 0;
}
```



##### 4.3.4 逻辑仿函数

**函数原型：**

- ```c++
  template<class T> bool logical_and<T>
  ```

- ```c++
  template<class T> bool logical_or<T>
  ```

- ```c++
  template<class T> bool logical_not<T>
  ```

```c++
#include<iostream>
using namespace std;
#include<functional>
#include<vector>
#include<algorithm>



void test01() {
	vector <bool> v;
	v.push_back(true);
	v.push_back(false);
	v.push_back(true);
	v.push_back(true);
	v.push_back(false);

	
	for (vector<bool>::iterator it = v.begin(); it != v.end(); it++) {
		cout << (*it) << " ";
	}
	cout << endl;

	vector <bool> v2;
	v2.resize(v.size());
	transform(v.begin(), v.end(), v2.begin(), logical_not<bool>());

	for (vector<bool>::iterator it = v2.begin(); it != v2.end(); it++) {
		cout << (*it) << " ";
	}
	cout << endl;

}

int main() {
	test01();
	return 0;
}
```

应用比较少了解即可

--------











### 5 STL 常用算法

**概述：**

- 算法主要由头文件<algorithm><functional><numeric>组成
- <algorithm>是所有STL头文件中最大的一个，涉及比较 交换 查找 遍历操作 赋值 修改等
- <numeric>体积很小，只包括几个在序列上进行简单数学运算的模板函数
- <functional>定义了一些类模板，用来声明函数对象

#### 5.1 常用遍历算法

**学习目标：**

- 掌握常用的遍历算法

算法简介

- ```c++
  for_each //遍历容器
  ```

- ```c++
  transform //搬运容器到另外一个容器中
  ```



##### 5.1.1 for_each

**示例：**

```c++
#include<iostream>
using namespace std;
#include<vector>
#include<algorithm>

void Print01(int val) {
	cout << val << " ";
}

class Print02 {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};


void test01() {

	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}
	//1.普通函数
	//for_each(v.begin(), v.end(), Print01);
	//2.仿函数
	for_each(v.begin(), v.end(), Print02());
}

int main() {
	test01();
	return 0;
}
```

##### 5.1.2 transform

**功能：**

- 将一个容器搬移到另外一个容器中

**函数原型：**

- ```c++
  transform(iterator beg1,iterator end1,iterator beg2,_func);
  //beg1 原容器起始迭代器
  //end1 原容器的结束迭代器
  //beg2 目标容器其实迭代器
  //_func() 函数或者函数对象
  ```

**示例：**

```c++
#include<iostream>
using namespace std;
#include<algorithm>
#include<vector>


class Transform {
public:
	int operator()(int val) {
		return val+100;
	}
};

class Print {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};
void test01() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}

	vector<int> Vtarget;
	Vtarget.resize(v.size());
	transform(v.begin(), v.end(), Vtarget.begin(), Transform());
	//Transform()将处理后返回的的元素存入目标的vector中
	for_each(Vtarget.begin(), Vtarget.end(), Print());

}

int main() {

	test01();
	return 0;
}
```

#### 5.2 常用的查找算法

**算法简介：**

- find //查找元素
- find_if //按条件查找元素
- adjacent_find //查找相邻重复元素
- binary_search //二分查找
- count //统计元素个数
- count_if //按条件统计元素个数



##### 5.2.1 find

**功能描述**

- 查找到指定元素，找到返回指定元素的迭代器,找不到返回元素的尾部迭代器

**函数原型**

- ```c++
  find(iterator beg,iterator end,value);
  ```

**示例：**

```c++
#include<iostream>
using namespace std;
#include<vector>
#include<algorithm>

class Person {
public:
	Person(string name, int age) {
		this->m_Age = age;
		this->m_Name = name;
	}
	bool operator==(const Person &Per) {
		if ((this->m_Name == Per.m_Name) && (this->m_Age == Per.m_Age)) {
			return true;
		}
		else
		{
			return false;
		}
	}
	string m_Name;
	int m_Age;
};
//内置数据类型
void test01() {

	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}

	vector<int>::iterator it = find(v.begin(), v.end(), 4);
	if (it != v.end()) {
		cout << "查找到该元素 :" << *it << endl;
	}

}
//自定义数据类型
void test02() {
	vector<Person> v1;

	Person p1("孙悟空", 1000);
	Person p2("猪八戒", 900);
	Person p3("唐僧", 100);
	Person p4("白龙马", 100);
	v1.push_back(p1);
	v1.push_back(p2);
	v1.push_back(p3);
	v1.push_back(p4);

	vector<Person>::iterator it = find(v1.begin(), v1.end(), p3);
	if (it != v1.end()) {
		cout << "姓名" << (*it).m_Name << " " << "年龄" << (*it).m_Age << endl;
	}
	else
	{
		cout << "未查找到" << endl;
	}
}

int main() {
	//test01();
	test02();
	return 0;
}
```

##### 5.2.2 find_if

**功能描述**

- 按条件查找

**函数原型**

```c++
find_if(iterator beg,iterator end,_Pred);
//_Pred函数或者谓词（返回bool类型的仿函数）
```



示例：

```c++
#include<iostream>
#include<vector>
using namespace std;
#include<algorithm>

class Person {
public:
	Person(string name, int age) {
		this->m_Age = age;
		this->m_Name = name;
	}
	bool operator==(const Person &Per) {
		if ((this->m_Name == Per.m_Name) && (this->m_Age == Per.m_Age)) {
			return true;
		}
		else
		{
			return false;
		}
	}
	string m_Name;
	int m_Age;
};

class FindP {
public:
	bool operator()(Person p) {
		if (p.m_Age > 500) {
			return true;
		}
		else
		{
			return false;
		}
	}
};

class Find {
public:
	bool operator()(int val) {
		return val < 5;
	}
};

void test01() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}
	vector<int>::iterator it = find_if(v.begin(), v.end(), Find());
	if (it != v.end()) {
		cout << "查找到该元素 :" << *it << endl;
	}
}

void test02() {
	vector<Person> v1;

	Person p1("孙悟空", 1000);
	Person p2("猪八戒", 900);
	Person p3("唐僧", 100);
	Person p4("白龙马", 100);
	v1.push_back(p1);
	v1.push_back(p2);
	v1.push_back(p3);
	v1.push_back(p4);

	vector<Person>::iterator it = find_if(v1.begin(), v1.end(), FindP());
	if (it != v1.end()) {
		cout << "姓名" << (*it).m_Name << " " << "年龄" << (*it).m_Age << endl;
	}
	else
	{
		cout << "未查找到" << endl;
	}
}

int main() {
	test02();
	//test01();
	return 0;
}
```

##### 5.2.3 adjacent_find

**功能描述**

- 查找相邻的重复元素

**函数原型**

```c++
adjacent_find(iterator beg,iterator end);
//查找相邻的重复元素，并返回第一个元素的迭代器
```



**示例：**

```c++
#include<iostream>
#include<algorithm>
using namespace std;
#include<vector>

void test01() {

	vector<int> v;
	v.push_back(1);
	v.push_back(2);
	v.push_back(0);
	v.push_back(3);
	v.push_back(4);
	v.push_back(5);
	
	vector<int>::iterator it = adjacent_find(v.begin(), v.end());
	if (it != v.end()) {
		cout << "查找到相邻重复元素" << *it << endl;
	}
	else
	{
		cout << "未查找到" << endl;
	}
}

int main() {
	test01();
	return 0;
}
```

##### 5.2.4 binary_search

查找指定的元素是否存在

函数原型：

```c++
bool binary_search(iterator beg,iterator end,value);
//查找指定的元素，查找到返回true，未查找到返回false
//在无需序列中不可用
```

**示例：**

```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

void test01() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i * 10);
	}

	if (binary_search(v.begin(), v.end(), 50)) {
		cout << "50在该容器中" << endl;
	}
	else
	{
		cout << "50不在该容器中" << endl;
	}
}

int main() {
	test01();
	return 0;
}

```

##### 5.2.5 count

**统计元素出现的次数**



**函数原型：**

```c++
int count(iterator beg,iterator end,value);
```



**示例：**

```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

class Person {
public:
	Person(string name, int age) {
		this->m_Age = age;
		this->m_Name = name;
	}
	bool operator==(const Person &Per) {
		if ((this->m_Name == Per.m_Name) && (this->m_Age == Per.m_Age)) {
			return true;
		}
		else
		{
			return false;
		}
	}
	string m_Name;
	int m_Age;
};

void test01() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i * 10);
	}
	v.push_back(40);
	cout << "40的元素个数" << count(v.begin(), v.end(), 40) << endl;

}
void test02() {
	vector<Person> v1;

	Person p1("孙悟空", 1000);
	Person p2("猪八戒", 900);
	Person p5("猪八戒", 900);
	Person p6("猪八戒", 900);
	Person p3("唐僧", 100);
	Person p4("白龙马", 100);
	v1.push_back(p1);
	v1.push_back(p2);
	v1.push_back(p3);
	v1.push_back(p4);
	v1.push_back(p5);
	v1.push_back(p6);

	cout << "猪八戒出现的次数" <<count(v1.begin(), v1.end(), p2);
}

int main() {
	//test01();
	test02();
	return 0;
}

```

##### 5.2.6 count_if

**按条件进行统计**

**函数原型**

```c++
count_if(iterator beg,iterator end,_Pred);
//_Pred返回bool的函数或者谓词
```

**示例：**

```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

class Person {
public:
	Person(string name, int age) {
		this->m_Age = age;
		this->m_Name = name;
	}
	bool operator==(const Person& Per) {
		if ((this->m_Name == Per.m_Name) && (this->m_Age == Per.m_Age)) {
			return true;
		}
		else
		{
			return false;
		}
	}
	string m_Name;
	int m_Age;
};
class MyCount {
public:
	bool operator()(int val) {
		if (val > 40) {
			return true;
		}
		else
		{
			return false;
		}
	}
};

class Mycount1 {
public:
	bool operator()(Person p) {
		if (p.m_Age == 900) {
			return true;
		}
		else
		{
			return false;
		}
	}
};


void test01() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i * 10);
	}
	v.push_back(40);
	cout << "40的元素个数" << count_if(v.begin(), v.end(), MyCount()) << endl;

}
void test02() {
	vector<Person> v1;

	Person p1("孙悟空", 1000);
	Person p2("猪八戒", 900);
	Person p5("猪八戒", 900);
	Person p6("猪八戒", 900);
	Person p3("唐僧", 100);
	Person p4("白龙马", 100);
	v1.push_back(p1);
	v1.push_back(p2);
	v1.push_back(p3);
	v1.push_back(p4);
	v1.push_back(p5);
	v1.push_back(p6);

	cout << "猪八戒出现的次数" << count_if(v1.begin(), v1.end(), Mycount1());
}

int main() {
	test01();
	test02();
	return 0;
}

```

#### 5.3常用的排序算法

**算法简介：**

- ```c++
  sort();//对容器元素进行排序
  ```

- ```c++
  random_shuffle();//洗牌指定范围的元素随机调整位置
  ```

- ```c++
  merge();//容器元素合并，储存到另一个容器中
  ```

- ```c++
  reverse();//反转指定范围的元素
  ```

##### 5.3.1 sort



**函数原型:**

```c++
sort(iterator beg,iterator end,_Pred);
```

**示例：**

```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<functional>
using namespace std;



void Print(int val) {
	cout << val << " ";
}


void test01() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i * 10);
	}
	v.push_back(40);
	sort(v.begin(), v.end());
	for_each(v.begin(), v.end(), Print);
	sort(v.begin(), v.end(), greater<int>());
	for_each(v.begin(), v.end(), Print);

}


int main() {
	test01();
	return 0;
}

```

##### 5.3.2 random_shuffle

**洗牌指定范围的元素随机调整位置**

**函数原型：**

```c++
random_shuffle(iterator beg,iterator end);
```



**示例**

```c++
#include<iostream>
using namespace std;
#include<vector>
#include<algorithm>

void Print(int val) {
	cout << val << " " << endl;
}
void test01() {
	vector<int> v;
	for (int i = 0; i < 20; i++) {
		v.push_back(i);
	}
	for_each(v.begin(), v.end(), Print);
	cout << endl;

	random_shuffle(v.begin(), v.end());
	for_each(v.begin(), v.end(), Print);
	cout << endl;

}

int main() {
	test01();
	return 0;
}
```

##### 5.3.3 merge

**将两个容器合并，并存储到一个新的容器中**

**函数原型：**

```c++
merge(iterator beg1,iterator end1,iterator beg2,iterator end2,iterator dest);
//dest 目标容器开始的迭代器
```



**示例:**

```c++
#include<iostream>
#include<ctime>
using namespace std;
#include<vector>
#include<algorithm>

void Print(int val) {
	cout << val << " " << endl;
}
void test01() {
	srand((unsigned int)time(NULL));
	vector<int> v;
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 20; i++) {
		v.push_back(i);
		v1.push_back(i * 10);
	}
	for_each(v.begin(), v.end(), Print);
	cout << endl;
	for_each(v1.begin(), v1.end(), Print);
	cout << endl;

	v2.resize(v.size() + v1.size());
	merge(v.begin(), v.end(), v1.begin(), v1.end(), v2.begin());
	for_each(v2.begin(), v2.end(), Print);

}

int main() {
	test01();
	return 0;
}
```



##### 5.3.4 reverse

**将原来的容器元素颠倒**

**函数原型：**

```c++
reverse(iterator beg,iterator end);
```



**示例：**

```c++
#include<iostream>
#include<ctime>
using namespace std;
#include<vector>
#include<algorithm>

void Print(int val) {
	cout << val << " " << endl;
}
void test01() {
	srand((unsigned int)time(NULL));
	vector<int> v;
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 20; i++) {
		v.push_back(i);
		v1.push_back(i * 10);
	}

	v2.resize(v.size() + v1.size());
	merge(v.begin(), v.end(), v1.begin(), v1.end(), v2.begin());
	for_each(v2.begin(), v2.end(), Print);
	cout << "----------------------" << endl;
	reverse(v2.begin(),v2.end());
	for_each(v2.begin(), v2.end(), Print);
}

int main() {
	test01();
	return 0;
}
```

#### 5.4 常用的拷贝和替换函数

**算法简介：**

1. copy
2. replace
3. replace_if
4. swap

##### 5.4.1 copy

**函数原型**

```c++
copy(iterator beg,iterator end,dest);
//dest目标容器的首个迭代器
```

##### 5.4.2 replace

**函数原型：**

```c++
replace(iterator beg,iterator end,oldvalue,newvalue);
//将区间中的旧的元素换为新的元素
```

##### 5.4.3 replace_if

**函数原型**

```c++
replace_if(iterator beg,iterator end,_Pred，newvalue);
//_Pred是谓词
```

##### 5.4.4 swap

**函数原型**

```c++
swap(container c1,container c2);
```





**代码示例**

```c++
#include<iostream>
#include<ctime>
using namespace std;
#include<vector>
#include<algorithm>


class Replace {
public:
	bool operator()(int val) {
		if (val == 20) {
			return true;
		}
		else
		{
			return false;
		}
	}
};


void Print(int val) {
	cout << val << " " ;
}
void test01() {
	srand((unsigned int)time(NULL));
	vector<int> v;
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 20; i++) {
		v.push_back(i);
		v1.push_back(10);
	}

	v2.resize(v.size());
	copy(v.begin(), v.end(), v2.begin());
	for_each(v2.begin(), v2.end(), Print);
	cout << endl;
	replace(v1.begin(), v1.end(), 10, 20);
	for_each(v1.begin(), v1.end(), Print);
	cout << endl;

	replace_if(v1.begin(), v1.end(), Replace(), 30);
	for_each(v1.begin(), v1.end(), Print);
	cout << endl;

	swap(v1,v2);
	for_each(v1.begin(), v1.end(), Print);
	cout << endl;
}

int main() {
	test01();
	return 0;
}
```

#### 5.5 常用算数生成算法

**注意：**

- 算数生成算法属于小型算法，使用时候包含头文件#include<numeric>

**算法简介：**

- accumulate //计算容器中的元素总和
- fill //向容器中添加元素

##### 5.5.1 accumulate

**计算容器中的元素总和**

**函数原型：**

```c++
int accumulate(iterator beg,iterator end,value);
//value起始值
```

##### 5.5.2 fill

**向容器中添加元素**

**函数原型：**

```c++
fill (iterator beg,iterator end,value);
```



**代码示例：**

```c++
#include<iostream>
using namespace std;
#include<vector>
#include<numeric>
#include<algorithm>
void Print(int val) {
	cout << val << " ";
}

void test01() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}
	int count;
	count = accumulate(v.begin(), v.end(), 0);
	cout << "合计数据为:" << count << endl;

	fill(v.begin(), v.end(), 10);
	for_each(v.begin(), v.end(), Print);

}

int main() {
	test01();
	return 0;
}
```

#### 5.6 常用集合算法

**算法简介：**

- set_intersection //求两个集合的交集
- set_union //求两个集合的并集
- set_difference //求两个集合的差集

##### 5.6.1 set_intersection

**函数原型：**

```c++
set_intersection(iterator beg1,iterator end1,inerator beg2,iterator end2,iterator dest);
//求交集的两个容器必须是有序序列
```

##### 5.6.2 set_union

**函数原型：**

```c++
set_union(iterator beg1,iterator end1,inerator beg2,iterator end2,iterator dest);
//求并集的两个容器必须是有序序列
```

##### 5.6.2 set_difference

**函数原型：**

```c++
set_union(iterator beg1,iterator end1,inerator beg2,iterator end2,iterator dest);
//求差集的两个容器必须是有序序列
```

**代码示例**

```c++
#include<iostream>
#include<vector>
using namespace std;
#include<algorithm>
#include<functional>
#include<iterator>

void Print(int val) {
	cout << val << " ";
}

void test01() {
	vector<int> v1;
	vector<int> v2;
	vector<int> v3;
	vector<int> v4;
	vector<int> v5;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i);
		v2.push_back(10 * i);
	}
	//v1.push_back(10);
	//v1.push_back(20);
	//v1.push_back(30);
	//v1.push_back(40);
	//v1.push_back(50);
	/*为何这么插入不能使用,必须是有序序列*/
	//v2.push_back(70);
	//v2.push_back(60);
	//v2.push_back(50);
	//v2.push_back(40);
	//v2.push_back(30);

	v5.resize(min(v1.size(), v2.size()));
	v4.resize(min(v1.size(), v2.size()));
	v3.resize(v1.size()+v2.size());
	vector<int>::iterator Endit = set_union(v1.begin(), v1.end(), v2.begin(), v2.end(), v3.begin());
	for_each(v3.begin(), Endit, Print);
	cout << endl;

	Endit = set_intersection(v1.begin(), v1.end(), v2.begin(), v2.end(), v4.begin());
	for_each(v4.begin(), Endit, Print);
	cout << endl;

	Endit = set_difference(v1.begin(), v1.end(), v2.begin(), v2.end(), v5.begin());
	for_each(v5.begin(), Endit, Print);
}

int main() {
	test01();
	return 0;
}
```

注意：

**所有的求集合的算法都返回的是新的目标容器的末尾的迭代器**