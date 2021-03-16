# C++学习笔记

[TOC]



## 1函数重载

**实例**

```c++
#include<iostream>

using namespace std;

//函数重载
//参数个数不同，参数类型不同，参数顺序不同
float func(int a, double b) {
	return a + b;
}

int func(int a, int b, int c = 10) {
	return a + b + c;
}

int main() {
	cout << func(1, 23) << endl;
	cout << func(1, 2.5) << endl;
	system("pause");
}
```

## 2.函数重载的注意事项

```c++
#include<iostream>

using namespace std;

//函数重载的注意事项
//1.引用作为重载的参数条件
void func(int& a) {
	cout << "int &a" << endl;
}

void func(const int& a) {
	cout << "const int &a" << endl;
}

//2、函数重载遇到默认参数
void func2(int a) {
	cout << "func2(int a)的调用" << endl;
}

void func2(int a, int b=10) {
	cout << "func2(int a, int b=10)的调用" << endl;
}
int  main()
{
	int a = 10;
	const int b = 100;
	func(a);
	func(b);

	//func2(10);//函数重载参数出现二义性,函数重载不要有默认参数
	func2(10, 20);
	system("pause");
	return 0;
}
```

## 3.C++struct和class区别

struct默认权限为公有

class默认权限是私有

## 4.对象的初始化和清理

### 1.构造函数(有参无参)

初始化自动调用

可以有参数

#### a.普通构造函数

#### b.拷贝构造函数

```c++
Person（const Person &p）{
    
}
```

#### c.调用构造函数的方法

1.括号法

person p1；无参数

person p2(10); 有参数

person p3(p1);拷贝构造函数

2.显示法

person p1；

person p2 = **person（10）；**匿名对象

person p3 =person （p2）；

**#不要用拷贝构造函数初始化匿名对象#**

**PS：不要使用p1(),系统会把它视作为函数声明** 

3.隐式转换法

person p4 = 10； === person p4（10）//有参构造

person p4 =p5； 拷贝构造

**拷贝构造函数 的调用时机**

1.使用已经创建的对象初始化新对象

2.值传递的方式给函数参数传值

3.值方式返回局部对象

### 2.析构实现、

在对象释放的时候自动释放

不可以有参数

### 3深拷贝与浅拷贝

浅拷贝带来的问题，就是会造成堆区的内存重复释放（使用new开辟的内存区），可以利用深拷贝来解决，就是利用new在堆区重新开辟一段内存，将新的指针指向它。

```c++
class person{

	person(){}
	person(const person& p){
	cout << "拷贝构造函数" << endl;
    m_age = p.mage;
    m_height = new int(*p.m_height);
        
	}

	~person(){
        cout << "析构函数"<<endl;
        if(m_height != NULL){
            delete m_height
        }
        
    }

public:
	int m_age;
	int *m_height;
}

void test01（）{
    person p1(18,180);
    person p2(p1);
    
    cout << "p1的年龄" << p1.m_age <<"身高"<<*p1.m_height<<endl;
    cout << "p2的年龄" << p2.m_age <<"身高"<<*p2.m_height<<endl;
}

int main(){
	test01();
    
    system("pause");
    return 0;
}
```

如果自己有在堆区开辟的属性，一定要提供拷贝构造函数。防止系统在浅拷贝中出现问题。

### 4初始化列表

```c++
class person{

public:
	person(int a,int b,int c){
	m_a =a;
	m_b =b;
	m_c =c;
	}
	int m_a;
    int m_b;
    int m_c;
}

class person{
public:
	//person():m_a(10),m_b(20),m_c(30){
    person(int a,int b,int c):m_a(a),m_b(b),m_c(c){
	}
	int m_a;
    int m_b;
    int m_c;
}
```

### 5类对象作为属性

当其他类作为我本类的属性，在构造对象的时候会先去构造其他类的对象；

析构的顺序，会先释放本类的对象，然后释放其他类的对象；

### 6静态成员函数

1.所有的对象都共享同一个函数

2.静态成员函数只能访问静态成员变量

3.静态成员函数也具有访问权限

4.静态成员在类内声明，类外初始化

```c++
class person{

public:
	static void func(){
        M_a =100;
        //M_b = 0;不能这样调用，静态成员函数不能访问
        cout << "static void 别调用" << endl; 
	}
    static int M_a;
    int M_b;
}

int person::M_a = 10//静态成员在类内声明，类外初始化
void test01(){
    //1通过对象调用
    person P;
    P.func();
    //2通过类名调用
    person::func();
}
```

**可以通过类名或者通过对象来访问**



## 5.C++对象模型和this指针

### 5.1成员变量和成员函数分开存储

​	//空对象的内存为1
​	//C++ 为空对象也分配了一个字节空间，是为了空对象占内存的位置

​	//若类成员不为空，则所占内存的大小为实际的内存大小，非静态成员存储在对象上

​	//静态成员变量和函数不属于某一个对象，非静态的成员函数也不再某一对象上

### 5.2this指针

this指针指向被调用成员函数所属的对象

this指针是隐含每一个非静态成员函数的一种指针

this指针不需要定义，直接使用即可

> `this指针的用途：`
>
> 当形参和成员变量同名时，可以用this指针区分
>
> 在类的非静态成员返回对象本身时，可以使用return *this；

链式编程思想

```c++
//定义一个对象自己调用
class Person{
    public:
    	Person& PersonAddAge(Person &p)
		//如果按值的方式传递，则返回的不再是p2本身，而是一个新的对象，this默认传入了
            
        {
            this->age +=p.age;
            
            return *this;
        }
            
        int age;
	}

void test01()
{
    Person p1(10);
    Person p2(10);
    
    p2.PersonAddAge(p1).PersonAddAge(p1).PersonAddAge(p1);
    cout << "p2的年纪为" << p2.age << endl;
}

int main()
{
    test01();
    
    system("pause");
    
    return 0;
}
```

### 5.3空指针调用成员函数

```c++
class Person{
	public:
	void ShoeClassName(){
	cout << "这是我的Person类" << endl;
	}
	void ShowAge(){
	cout << "这是我的age" << age << endl;//age == this->age;
	}
	int age;
}

void test01(){
	Person *p1 = NULL;
	p1.ShowClassName();
    if(p!=NULL)
		p1.ShowAge();//c传入的指针为空
}
int main(){
	test01();
	
	system("pause");
	return 0;
}
```

### 5.3const 修饰的成员函数

**常函数**

- 成员函数后加const以后，我们就把其叫做常函数

- 常函数内不可以修改成员属性

- 成员属性的声明与加mutable后，在常函数中依然可以修改

**常对象**

- 声明对象时候加const就是常对象
- 常对象只能调用常函数

  **实例**

```c++
class Person{
    public:
    	//Person *const this；
    	//在后边加了const就类似于const Person *const this；修饰this指针，不可以修改this指针指向的值
    	void showPerson() const {//常函数
			//this->M_A = 100;
            //this = NULL;//this指针不可以修改指针的指向
            this->M_B = 100;//有mutable修饰以后就可以修改this指针指向的值
        	}
    	void func(){
			}
    	int M_A;
        mutable int M_B;
};

void test01(){
    Person p;
    p.showPerson();
}

//常对象
void test01(){
    const Person p;
    //p.M_A = 100； //不可以修改
    p.M_B = 100； //可以修改
    p.showPerson();//常对象只能调用常函数
    //p.func()//常对象不能调用普通函数，因为普通函数会对对象的属性进行修改。
        
}
```

## 6.友元

**友元的三种实现：**

- 全局友元函数
- 类做友元
- 成员友元函数

### 6.1全局函数做友元

```c++
#include<iostream>

using namespace std;

class Building {

	friend void GoodGay(Building& building);//全局友元函数，就可以访问私有变量
public:
	Building()
	{
		m_BedRoom = "卧室";
		m_SittingRoom = "客厅";
	}
public:
	string m_SittingRoom;
private:
	string m_BedRoom;
};

void GoodGay(Building &building) {

	cout << "好基友的全局函数正在访问" << building.m_SittingRoom << endl;
	cout << "好基友的全局函数正在访问" << building.m_BedRoom << endl;

}

void test01()
{
	Building building;
	GoodGay(building);
}

int main() {

	test01();
	system("pause");
	return 0;
}
```

### 6.2类做友元

```c++
#include<iostream>

using namespace std;





class Building {

	friend class GoodGay;//将GoodGay设置为Building的友元类
public:
	Building();

public:
	string m_SittingRoom;
private:
	string m_BedRoom;
};

class GoodGay {


public:
	Building* building;

	GoodGay();
	void Visit();//参观函数，访问building属性


};


void test01()
{
	GoodGay gg;
	gg.Visit();
}

int main() {

	test01();
	system("pause");
	return 0;
}

GoodGay::GoodGay() {

	building = new Building;
}

void GoodGay::Visit() {

	cout << "好基友的全局函数正在访问" << building->m_SittingRoom << endl;
	cout << "好基友的全局函数正在访问" << building->m_BedRoom << endl;


}

Building::Building() {

		m_BedRoom = "卧室";
		m_SittingRoom = "客厅";
}
```

### 6.3成员函数做友元

```c++
#include<iostream>

using namespace std;

class Building;

class GoodGay {


public:
	Building* building;

	GoodGay();
	void Visit();//参观函数，访问building属性的所有属性
	void Visit2();//只能访问building的共有变量

};

class Building {

	friend void GoodGay::Visit();//声明为友元函数
public:
	Building();

public:
	string m_SittingRoom;
private:
	string m_BedRoom;
};




void test01()
{
	GoodGay gg;
	gg.Visit();
	gg.Visit2();
}

int main() {

	test01();
	system("pause");
	return 0;
}

GoodGay::GoodGay() {

	building = new Building;
}

void GoodGay::Visit() {

	cout << "好基友的全局函数正在访问" << building->m_SittingRoom << endl;
	cout << "好基友的全局函数正在访问" << building->m_BedRoom << endl;
}

Building::Building() {

	m_BedRoom = "卧室";
	m_SittingRoom = "客厅";
}

void GoodGay::Visit2() {
	cout << "好基友的全局函数正在访问" << building->m_SittingRoom << endl;
	//cout << "好基友的全局函数正在访问" << building->m_BedRoom << endl;
}
```

## 7.运算符重载

### 7.1加号运算符重载

```
#include<iostream>

using namespace std;
//加法运算重载


class Person {
public:
	/*&Person operator+(Person& p) {
		Person temp;
		temp.m_A = this->m_A + p.m_A;
		temp.m_B = this->m_B + p.m_B;

		return temp;
		}*/
		int m_A;
		int m_B;
};

/*void test01() {
	Person p1;
	p1.m_A = 10;
	p1.m_B = 10;
	Person p2;
	p2.m_A = 20;
	p2.m_B = 20;

	Person p3 = p1 + p2;
	cout << "p3.m_A = " << p3.m_A << endl;
	cout << "p3.m_B = " << p3.m_B << endl;
}*/
//1、成员函数重载加号

//2、通过全局函数重载加号
Person operator+(Person& p1, Person& p2) {
	Person temp;
	temp.m_A = p1.m_A + p2.m_A;
	temp.m_B = p1.m_B + p2.m_B;

	return temp;
}
Person operator+(Person& p1, int num) {//运算符函数重载
	Person temp;
	temp.m_A = p1.m_A + num;
	temp.m_B = p1.m_B + num;

	return temp;
}

void test02() {
	Person p1;
	p1.m_A = 10;
	p1.m_B = 10;
	Person p2;
	p2.m_A = 20;
	p2.m_B = 20;

	Person p3 = p1 + p2;
	Person p4 = p1 + 100;
	cout << "p3.m_A = " << p3.m_A << endl;
	cout << "p3.m_B = " << p3.m_B << endl;

	cout << "p4.m_A = " << p4.m_A << endl;
	cout << "p4.m_B = " << p4.m_B << endl;
}
int main() {

	//test01();
	test02();
	system("pause");
	return 0;
}
```

**运算符重载也可以重载函数**

### 7.2重载左移运算符

```c++
#include<iostream>

using namespace std;
//加法运算重载


class Person {
	friend ostream& operator<<(ostream& cout, Person& p);
public:

	//void operator<<(Person &person) { //p.operator<<(p1)
		//operator<<(cout)   p << cout
		//一般不会使用成员函数来重载左移运算符，因为不能实现cout在左侧
	//}

	int m_A;
	int m_B;
private:
	int m_C =0;
};

//只能利用全局函数来重载左移运算符

ostream &operator<<(ostream &cout,Person &p ) {

	cout << p.m_A << endl;
	cout << p.m_B << endl;
	cout << p.m_C << endl;
	return cout;

}


void test02() {
	Person p1;
	p1.m_A = 10;
	p1.m_B = 10;
	Person p2;
	p2.m_A = 20;
	p2.m_B = 20;

	cout << "p1.m_A = " << p1.m_A << endl;
	cout << "p1.m_B = " << p1.m_B << endl;

	cout << "p2.m_A = " << p2.m_A << endl;
	cout << "p2.m_B = " << p2.m_B << endl;

	cout << p1 << endl << "hello world";
}
int main() {

	test02();
	system("pause");
	return 0;
}
```

### 7.3重载++运算符

```c++
#include<iostream>

using namespace std;
//加法运算重载


class Person {

	friend ostream& operator<<(ostream& cout, Person p);
public:
	//重载++运算符
	//Person operator++() {//如果按值传递，则最后加出来的值不会再加到原来的变量上，按引用则仍然可以加到原来的变量上
	Person& operator++(){

		this->m_A = this->m_A++;
		this->m_B = this->m_B++;

		return *this;
	}

	Person& operator++(int) {//后置++
		//先记录
		//Person new *temp = *this;
		Person* temp = new Person;
		temp = this;

		//后递增
		this->m_A = this->m_A + 1;
		this->m_B = this->m_B + 1;
		this->m_C = this->m_C + 1;
		//再返回

		return *temp;

	}
	
public:

	int m_A;
	int m_B;
private:
	int m_C =0;
};

ostream& operator<<(ostream& cout, Person p) {
	
	cout << p.m_C;
	return cout;

}

//Person operator++(Person &p ) {
//
//	p.m_A = p.m_A++;
//	p.m_B = p.m_B++;
//
//	return p;
//
//}


void test02() {
	Person p1;
	p1.m_A = 10;
	p1.m_B = 10;
	Person p2;
	p2.m_A = 20;
	p2.m_B = 20;

	++(++p1);//按值传递p1还是11；按引用传递则值为12
	(p2++)++;
	cout << "p1.m_A = " << p1.m_A << endl;
	cout << "p1.m_B = " << p1.m_B << endl;

	cout << "p2.m_A = " << p2.m_A << endl;
	cout << "p2.m_B = " << p2.m_B << endl;

}
int main() {

	test02();
	system("pause");
	return 0;
}
```

### 7.4重载赋值运算符

```c++
#include<iostream>

using namespace std;
//加法运算重载


class Person {
	
public:

	Person(int age) {
		m_Age = new int(age);
	}

	~Person() {
		if (m_Age != NULL) {
			delete m_Age;
			m_Age = NULL;
		}	
	}


	int *m_Age;
	int m_A;
	Person &operator=(Person &p) {//如果直接按值传递，则会创建默认拷贝构造函数，
								//从而使得对象的属性进行浅拷贝，在函数结束的时候会出现重复释放堆区内存的情况
		if (this->m_Age != NULL) {//因为在对象自身的属性中已经包括有堆区的属性，所以需要先判断，然后释放
			delete m_Age;
			this->m_Age = NULL;
		}
		this->m_Age = new int(*p.m_Age);
		return *this;
	}
	int m_B;
private:
	int m_C =0;
	
};

//只能利用全局函数来重载左移运算符


void test02() {
	Person p1(18);
	Person p2(20);
	Person p3(30);
	p1 = p2 = p3;
	cout << "p1的年纪为 " << *p1.m_Age << endl;
	cout << "p2的年纪为 " << *p2.m_Age << endl;
	cout << "p3的年纪为 " << *p3.m_Age << endl;
	//释放p1然后释放P2的时候导致堆区的内存重复释放；

}
int main() {

	test02();
	system("pause");
	return 0;
}
```

**如果直接按值传递，则会创建默认拷贝构造函数，从而使得对象的属性进行浅拷贝，在函数结束的时候会出现重复释放堆区内存的情况**

### **7.5关系运算符的重载**



```c++
#include<iostream>

using namespace std;
//加法运算重载


class Person {
	
public:

	bool operator>(Person p) {
		if (this->m_Age>p.m_Age) {
			return true;
		}
		else
		{
			return false;
		}
	}

	bool operator<(Person p) {
		if (this->m_Age < p.m_Age)
		{
			return true;
		}
		else
		{
			return false;
		}
	}

	bool operator==(Person p) {
		if (this->m_Age == p.m_Age)
		{
			return true;
		}
		else
		{
			return false;
		}
	}

	Person(int age) {
		m_Age = age;
	}

	~Person() {
	}


	int m_Age;
	int m_B;
private:
	int m_C =0;
	
};

//只能利用全局函数来重载左移运算符


void test02() {
	Person p1(18);
	Person p2(20);
	Person p3(30);
	if (p2 > p1)
		cout << "P1大于P2" << endl;
	if (p2 < p1)
		cout << "P1小于P2" << endl;
	if (p1 == p2)
		cout << "P1等于P2" << endl;
	cout << "p1的年纪为 " << p1.m_Age << endl;
	cout << "p2的年纪为 " << p2.m_Age << endl;
	cout << "p3的年纪为 " << p3.m_Age << endl;
	//释放p1然后释放P2的时候导致堆区的内存重复释放；

}
int main() {

	test02();
	system("pause");
	return 0;
}
```

### 7.6函数运算符的重载

```c++
#include<iostream>

using namespace std;
//加法运算重载


class MyPrint {

public:

	void operator()(string str) {

		cout << str << endl;

	}

};

class MyAdd {

public:

	int operator()(int num1,int num2) {

		return num1 + num2;
	}
};

//只能利用全局函数来重载左移运算符
void PrintStr(string str1) {

	cout << str1 << endl;
	return;

}

void test02() {
	MyPrint obj1;
	MyAdd myAdd;
	int ret = myAdd(100,200);

	cout << "myAdd(100,200)" << ret << endl;
	//仿函数
	cout << MyAdd()(100, 100) << endl;//匿名函数对象
	obj1("hello world!!!!");
	obj1("hello world!!!!");
	PrintStr("hello world!!!!");

}
int main() {

	test02();
	system("pause");
	return 0;
}
```

## 8.继承

### 8.1继承的基本知识

```c++
#include<iostream>

using namespace std;

//BasePage
class BasePage {
public:
		void Header() {
			cout << "首页 公开课 登录 ......" << endl;

		}
		void Footer() {
			cout << "帮助中心、交流合作、站内地图......" << endl;
		}

		void Left() {
			cout << "Python C++ JAVA......" << endl;

		}
};
//C++
class Java : public BasePage {
public:
	void Content() {
		cout << "Java 学科视频 " << endl;
	}
};

class Python : public BasePage {
public:
	void Content() {
		cout << "Python 学科视频" << endl;

	}
};
class Cpp : public BasePage {
public:
	void Content() {
		cout << "C++ 学科视频" << endl;
	}
};

void test01() {
	Java ja;
	ja.Header();
	ja.Footer();
	ja.Left();
	ja.Content();
	Cpp cpp;
	cpp.Content();
	Python py;
	py.Content();
}
int main() {

	test01();
	return 0;
}
```

### 8.2继承的方式

1. 公共继承
2. 保护继承
3. 私有继承

![image-20201116203715227](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201116203715227.png)

- 父类的私有内容，无论如何都无法访问
- 若共有继承，父类的权限不变，私有属性不能访问
- 若为保护继承，则父类的保护和公有都是保护权限
- 若为私有继承，则所有的属性都为私有权限

### 8.3.继承中的对象模型

**从父类继承过来的对象哪些属于子类对象中？**

利用开发人员命令提示工具查看类的继承关系

![image-20201116210406332](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201116210406332.png)

![image-20201116210234935](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201116210234935.png)

```c++
#include<iostream>

using namespace std;

//继承中的对象模型

class Base {
public:
	int m_A;
private:
	int m_B;
protected:
	int m_C;
};

class Son :public Base {
public:
	int m_D;
};

void test01() {
	Son son1;
	//16父类中所有非静态的成员都会被子类继承下去
	//父类的私有变量只是访问不到，但是它是确实存在的
	cout << "son1 的大小为" << sizeof(Son) << endl;
}

int main() {
	test01();
	return 0;
}
```

### 8.4继承中的构造与析构

子类在继承父类以后，当创建子类对象时候，也会调用父类的构造函数（子类和父类的析构和构造顺序）

**先有基类的构造，再有派生类的构造，先析构派生类，再析构基类的对象**

![image-20201116211329141](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201116211329141.png)

```c++
#include<iostream>

using namespace std;

class Base {
public:
	Base() {
		cout << "Base构造函数被调用" << endl;
	}
	~Base() {
		cout << "Base析构函数被调用" << endl;
	}
};


class Son :public Base {
public:
	Son() {
		cout << "Son的构造函数被调用" << endl;

	}
	~Son() {
		cout << "Son的析构函数被调用" << endl;
	}
};

void test01() {
	Son son;
	return;
}
int main() {

	test01();

	return 0;
}
```

### 8.5继承中的同名属性的处理

> **访问子类的同名成员 直接访问**

> **访问父类的同名成员 需要加作用域**
>
> **只要出现一个子类成员函数，就会将所有的父类成员同名函数将全被隐藏，如果要调用父类的成员函数，需要添加作用域**

```c++
#include<iostream>

using namespace std;

class Base {
public:
	int m_A = 10;

	void func() {
		cout << "Base中的func()被调用" << endl;
	}
	void func(int a) {
		cout << "Base中的func()被调用" << a << endl;
	}
};

class Son :public Base {
public:
	int m_A = 20;
	void func() {
		cout << "Son中的func()被调用" << endl;
	}
	

};
void test01() {
	Son son;
	son.func();
	//son.func(10);//**只要出现一个子类成员函数，就会将所有的父类成员同名函数将全被隐藏**
	son.Base::func(10);
	son.Base::func();
	cout << "son中的m_A数值为" << son.m_A << endl;
	cout << "Base中的m_A数值为" << son.Base::m_A << endl;

}
int main() {
	
	test01();
	return 0;

}
```

### 8.6继承中的静态成员函数的处理方式（static）

访问子类成员：直接访问

访问父类成员：需要加作用域

**静态成员在类内声明，类外初始化**

父类中的静态成员访问   Son::Base::m_A;//第一个::通过类名访问，第二个代表父类的作用域

### **8.7多继承语法**

c++允许一个类去继承多个类

语法：

```c++
class 子类 : 继承方式 父类1，继承方式 父类2，...
```

多继承可能会引起同名成员函数的出现，需要加作用域，实际中不建议多继承

```C++
#include<iostream>

using namespace std;

class Base1 {
public:
	int m_A = 10;
};

class Base2 {
public:
	int m_A = 20;
};

class Son :public Base1, public Base2 {
public:
	int m_A = 30;
};

void test01() {

	Son son;
	cout << "Son 中的m_A为" << son.m_A << endl;
	cout << "Base1 中的m_A为" << son.Base1::m_A << endl;
	cout << "Base2 中的m_A为" << son.Base2::m_A << endl;
}
int main()
{

	test01();
	return 0;
}
```

### 8.8菱形继承问题

```c++
#include<iostream>

using namespace std;

class Anmial {
public:
	int m_Age;
};




//利用虚继承解决菱形继承问题，在继承前加上virtual关键字
class Sheep :virtual public Anmial{


};

class Tuo :virtual public Anmial{

};

class SheepTuo :public Sheep, public Tuo {};



void test01() {
	SheepTuo St1;
	//当菱形继承具的父类具有相同的属性，需要加以作用域进行区分
	//菱形继承导致数据有双份，导致资源浪费
	St1.Sheep::m_Age = 10;
	St1.Tuo::m_Age = 18;

	cout << "St1.Sheep::m_Age" << St1.Sheep::m_Age << endl;
	cout << "St1.Tuo::m_Age" << St1.Tuo::m_Age << endl;
	cout << "St1.m_Age" << St1.m_Age << endl;
}
int main() {

	test01();

	return 0;
}
```

## 9.多态

### 9.1多态的基本概念

```c++
#include<iostream>

using namespace std;
//动态多态需要满足的条件
//1、子类和父类之间有继承关系
//2、子类要重写父类的虚函数

//动态多态的使用
//1、父类的引用或者指针指向子类的对象(执行子类的对象)

class Animal {
public:
	//虚函数
	virtual void Speak() {//virtual 实现地址在运行阶段，进行晚绑定
		cout << "动物在说话" << endl;

	}
};

class Cat :public Animal {
public:
	void Speak() {
		cout << "小猫在说话" << endl;
	}
};

//地址早绑定
//如果想让猫说话，则在执行阶段进行晚绑定

void DoSpeak(Animal &animal) {//父类的对象去接受一个子类的对象
	animal.Speak();
}

void test01() {
	Cat cat;
	DoSpeak(cat);
}
int main() {
	test01();
	return 0;

}
```

**动态多态需要满足的条件**
**1、子类和父类之间有继承关系**
**2、子类要重写父类的虚函数**

**动态多态的使用**
**1、父类的引用或者指针指向子类的对象(执行子类的对象)**

### 9.2多态的原理刨析

![image-20201117213530595](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201117213530595.png)

![image-20201117213600978](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201117213600978.png)

```c++
#include<iostream>

using namespace std;

class Animal {
public:
	//虚函数
	//加了virtual关键字，创建对象时，就会你生成一个指针vfptr指向vftable（vftable中存储的就是函数的入口地址），大小为4
	virtual void Speak() {//virtual 实现地址在运行阶段，进行晚绑定
		cout << "动物在说话" << endl;

	}
};

class Cat :public Animal {
public:
	void Speak() {
		cout << "小猫在说话" << endl;
	}
};

//地址早绑定
//如果想让猫说话，则在执行阶段进行晚绑定

void DoSpeak(Animal& animal) {//父类的对象去接受一个子类的对象
	animal.Speak();
}

void test01() {
	Cat cat;
	DoSpeak(cat);
}

void test02() {
	Cat cat1;
	cout << "cat 类的大小为 " << sizeof(cat1) << endl;
}
int main() {
	//test01();

	test02();
	return 0;

}
```

### 9.3多态的案例：计算器

**多态的优点**

- 代码组织结构清晰
- 可读性强
- 利于前期和后期的维护和扩展

```C++
#include<iostream>

using namespace std;

class Calculator {//普通实现
public:
	int get_result(string oper) {
		if (oper == "+") {
			return m_Num1 + m_Num2;
		}if (oper == "-") {
			return m_Num1 - m_Num2;
		}if (oper == "*") {
			return m_Num1 * m_Num2;
		}if (oper == "/") {
			return m_Num1 / m_Num2;
		}

	}
	int m_Num1;
	int m_Num2;


};

//实现计算机的抽象类
class AbstractCalculatior {
public:
	virtual int getResult() {
		return 0;
	}

	int m_Num1;
	int m_Num2;
};

class AddCalculatior :public AbstractCalculatior {
public:
	int getResult() {
		return m_Num1 + m_Num2;
	}

};

class SubCalculatior :public AbstractCalculatior {
public:
	int getResult() {
		return m_Num1 - m_Num2;
	}

};

class ChenCalculatior :public AbstractCalculatior {
public:
	int getResult() {
		return m_Num1 * m_Num2;
	}

};

void test01() {
	Calculator cal;
	cal.m_Num1 = 10;
	cal.m_Num2 = 20;

	cout << "cal.m_Num1 + cal.m_Num2  =" << cal.get_result("+") << endl;
	cout << "cal.m_Num1 - cal.m_Num2  =" << cal.get_result("-") << endl;
	cout << "cal.m_Num1 * cal.m_Num2  =" << cal.get_result("*") << endl;
	cout << "cal.m_Num1 / cal.m_Num2  =" << cal.get_result("/") << endl;
	//如果进行扩展，就需要修改源码
	//在实际的开发中，提倡开闭原则
	//开闭原则：对扩展开放，对修改进行关闭

	//多态带来好处
	//1、组织结构清晰
	//2、可读性强
	//3、后期进行扩展
}

void test02() {
	//多态的使用条件，父类的指针或者引用指向子类的对象
	//

	AbstractCalculatior* abc = new AddCalculatior;

	abc->m_Num1 = 10;
	abc->m_Num2 = 20;

	cout << abc->m_Num1 << " + " << abc->m_Num2 << " = " << abc->getResult() << endl;

	delete abc;

	abc = new SubCalculatior;
	abc->m_Num1 = 10;
	abc->m_Num2 = 20;
	cout << abc->m_Num1 << " - " << abc->m_Num2 << " = " << abc->getResult() << endl;
	delete abc;

	abc = new ChenCalculatior;
	abc->m_Num1 = 10;
	abc->m_Num2 = 20;
	cout << abc->m_Num1 << " * " << abc->m_Num2 << " = " << abc->getResult() << endl;
	delete abc;
}
int main() {
	//test01();
	test02();

	return 0;
}
```

### 9.4纯虚函数与抽象类

通常在父类中的中虚函数的实现是毫无意义的，主要是调用子类重写的内容

可以用**纯虚函数**代替虚函数

纯虚函数语法：

```c++
virtual 返回值类型 函数名 （参数列表） = 0;
```

当我们的类中有纯虚函数时候，被称为抽象类



**抽象类的特点：**

- 无法实例化对象
- 子类必须重写父类中的纯虚函数，否则也属于抽象类

```c++
#include<iostream>

using namespace std;

class Base {//抽象类
public:
	virtual void func() = 0;//纯虚函数
};


class Son : public Base {

};

class Son1 :public Base {
	void func() {
		
		cout << "func() 函数的调用 " << endl;

	}
};
void test01() {
	//Son b;
	//Base a;  //抽象类无法实例化对象
	Son1 c;		//子类必须要实例化父类的纯虚函数，不然不能实例化对象
	//Base* base = new Son1;//多态
	//base->func();
	Base& base = c;
	base.func();
}

int main() {
	test01();
	return 0;
}
```

### 9.5多态案例

```c++
#include<iostream>

using namespace std;

class Base {
public:
	//煮水
	virtual void boiled() = 0;
	//泡茶
	virtual void Pao() = 0;
	//装杯
	virtual void Cup() = 0;
	//打包
	virtual void Pack() = 0;

	void Make() {
		boiled();
		Pao();
		Cup();
		Pack();
	}

};

class Tea : public Base
{
public:
	void boiled() {
		cout << "煮茶水" << endl;
	}
	void Pao() {
		cout << "泡茶" << endl;
	}
	void Cup() {
		cout << "装杯子" << endl;
	}
	void Pack() {
		cout << "打包茶" << endl;
	}

};
class MilkTea : public Base
{
public:
	void boiled() {
		cout << "煮茶水+牛奶" << endl;
	}
	void Pao() {
		cout << "泡茶+混合牛奶" << endl;
	}
	void Cup() {
		cout << "装杯子" << endl;
	}
	void Pack() {
		cout << "打包奶茶" << endl;
	}

};


void test01() {
	Base* base = new Tea;
	base->Make();
	delete base;
	base = new MilkTea;
	base->Make();
	delete base;
	cout << "------------------------------------" << endl;
	MilkTea milke;
	Base& base1 = milke;
	base1.Make();
}

int main() {

	test01();
	return 0;
}
```

### 9.5虚析构与纯虚析构

多态在使用时候，如果子类有属性开辟到了堆区（new），那么父类的指针在释放时候，就无法调用子类的析构代码

解决方法：将父类中的析构函数改为**虚析构**或者**纯虚析构**

虚析构与纯虚析构的共性：

- 可以解决父类指针释放子类对象的问题
- 都需要具体的函数实现

虚函数与纯虚函数的区别：

- 如果是纯虚析构，则属于抽象类，不能直接实例化对象

虚析构的语法：

```c++
virtual ~类名（）{}
```

 纯虚析构函数语法：

```
virtual ~类名（） = 0；
```

```c++
#include<iostream>

using namespace std;

class Animal {
public:
	Animal() {
		cout << "animal 的构造函数调用" << endl;
	}
	//利用虚构造函数可以解决父类指针释放子类对象不干净的问题；
	//virtual ~Animal() {
	//	cout << "animal 的析构函数调用" << endl;
	//}

	//纯虚析构要有声明，同时也需要有实现
	//有了纯虚析构就不能实例化对象
	virtual ~Animal() = 0;

	virtual void speak() = 0;
};
Animal::~Animal() {
	cout << "animal 的析构函数调用" << endl;
}

class Cat :public Animal {
public:
	void speak() {
		cout << *m_Name << " 小猫在说话" << endl;
	}
	string* m_Name;
	Cat(string name) {
		cout << "cat 构造函数的调用" << endl;
		m_Name = new string(name);
	}

	~Cat() {
		if (m_Name != NULL) {
			cout << " Cat 析构函数被调用" << endl;
			delete m_Name;
			m_Name = NULL;
		}
	}
};


void test02() {

	Animal *animal = new Cat("Tom");
	animal->speak();
	//父类指针在析构的时候，不会调用子类中的析构函数，子类如果有堆区的属性，会出现内存泄漏问题

	delete animal;

}

int main() {
	test02();

	return 0;
}
```

**总结**：

- 虚析构和纯虚析构都是为了解决通过父类对象来释放子类对象的问题
- 如果子类中没有堆区的数据，则可以不用写虚析构函数
- 拥有纯虚构函数的类也属于抽象类

### 9.6电脑组装案例——多态

![image-20201118200915394](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201118200915394.png)

**示例**：

```c++
#include<iostream>

using namespace std;
//抽象不同的零件类
class CPU {
public:
	virtual void caculate() = 0;
};

class GPU {
public:
	virtual void display() = 0;
};

class Memory {
public:
	virtual void storage() = 0;
};

class Computer {
public:
	Computer(CPU *cpu, GPU *gpu, Memory *memory) {

		m_cpu = cpu;
		m_gpu = gpu;
		m_memory = memory;

	}
	~Computer() {
		if (m_cpu != NULL) {
			delete m_cpu;
			m_cpu = NULL;
		}	
		if (m_gpu != NULL) {
			delete m_gpu;
			m_gpu = NULL;
		}
			
		if (m_memory != NULL) {
			delete m_memory;
			m_memory = NULL;
		}
			

		cout << "内存已经释放" << endl;
	}
	void Work() {

		m_cpu->caculate();
		m_gpu->display();
		m_memory->storage();
	}

private:
	CPU* m_cpu;
	GPU* m_gpu;
	Memory* m_memory;

};

class IntelCPU :public CPU {
public:
	void caculate() {
		cout << " intel 的CPU工作起来了" << endl;
	}
};

class IntelGPU :public GPU {
public:
	void display() {
		cout << "intel 的GPU开始显示" << endl;
	}
};

class IntelMemory :public Memory {
public:
	void storage() {
		cout << " intel 的内存条开始工作 " << endl;
	}
};


class LenovoCPU :public CPU {
public:
	void caculate() {
		cout << " Lenovo 的CPU工作起来了" << endl;
	}
};

class LenovoGPU :public GPU {
public:
	void display() {
		cout << "Lenovo 的GPU开始显示" << endl;
	}
};

class LenovoMemory :public Memory {
public:
	void storage() {
		cout << " Lenovo 的内存条开始工作 " << endl;
	}
};

void test01() {
	//第一台电脑
	CPU* intelCPU = new IntelCPU;
	GPU* intelGPU = new IntelGPU;
	Memory* intelmemory = new IntelMemory;

	Computer* computer1 = new Computer(intelCPU, intelGPU, intelmemory);
	computer1->Work();
	delete computer1;//走computer的析构函数

	cout << "---------------------" << endl;
	//第二台电脑组装
	Computer* computer2 = new Computer(new LenovoCPU, new LenovoGPU, new LenovoMemory);
	computer2->Work();
	delete computer2;
	cout << "---------------------" << endl;


	//第三台电脑组装
	Computer* computer3 = new Computer(new IntelCPU, new LenovoGPU, new IntelMemory);
	computer3->Work();
	delete computer3;
}
int main() {

	test01();
	return 0;
}
```

## 10.文件

### 10.1文件的写

```c++
#include<iostream>
#include<fstream>

using namespace std;
void test01() {
	ofstream File;
	File.open("test.txt", ios::out);

	if (!File.is_open()) {
		cout << "文件打开失败" << endl;

	}
	string name = "小王";
	int age = 19;
	int hight = 180;
	File << "姓名 ： " << name << endl;
	File << "年龄 ： " << age << endl;
	File << "身高 ： " << hight << endl;
    File.close();
}
int main() {

	test01();
	return 0;
}
```



### 10.2文件的读

文件的四种读取方法：

```c++
#include<iostream>
#include<fstream>
#include<string>

using namespace std;

//void test01() {
//	fstream File;
//	File.open("test.txt", ios::in);
//	char s[1000];
//	while (File.getline(s, 1000)) {
//		cout << s <<endl;
//	}
//}
//void test02() {
//	fstream F1;
//	F1.open("test.txt", ios::in);
//	if (!F1.is_open()) {
//		cout << " 文件打开失败 " << endl;
//	}
//	char buffer[1024] = {};
//	while (F1>>buffer)
//	{
//		cout << buffer << endl;
//	}
//
//}

//void test03() {
//	string str;
//	fstream F1;
//	F1.open("test.txt", ios::in);
//	while (getline(F1,str))
//	{
//		cout << str << endl;
//	}
//}

void test04() {
	fstream F3;
	F3.open("test.txt",ios::in);
	char s;
	while ((s = F3.get()) != EOF) {
		cout << s ;
	}
	F3.close();
}
int main() {
	
	//test01();
	//test02();
	//test03();
	test04();
	return 0;

}
```

### 10.3 文件写（二进制）

二进制方式写文件主要利用流对象调用函数write

函数原型：

```c++
ostream write(const char * buffer,int len)
```

参数解释：字符指针buffer 指向内存中的一段存储空间。len是读写的字节常数



```c++
#include<iostream>
#include<fstream>

using namespace std;

class Person {
public:
	char m_Name[64];
	int m_Age;

};

int main()
{
	fstream file;
	file.open("person.txt", ios::out|ios::binary);

	Person p = { "张三",20 };

	file.write((const char *)&p,sizeof(p));//写抽象数据类型
	cout << " 写入完成 " << endl;
	return 0;
}
```



### 10.3 文件读（二进制）

二进制读文件主要利用流对象调用成员函数read

函数原型

```c++
istream& read(char *buffer,int len)
```

字符指针buffer指向内存空间的一段存储空间，len是读取的字节长度

实例：

```c++
#include<iostream>
#include<fstream>

using namespace std;

class Person {
public:
	char m_Name[64];
	int m_Age;

};

void test01() {
	Person p;
	fstream ifs;
	ifs.open("person.txt", ios::in | ios::binary);
	if (!ifs.is_open()) {
		cout << "文件打开失败" << endl;
	}
	ifs.read((char*)&p,sizeof(ifs));
	cout << "姓名" << p.m_Name << " 年龄 " << p.m_Age << endl;
	
}
int main() {
	test01();
	return 0;
}
```

