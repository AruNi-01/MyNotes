

# 													C++基础

本阶段主要是对C++有初步的了解，能够有基础的编程能力

 ### 1、常量（不可变）

#### 1.1 #define宏常量：

​	一般形式：**#define 常量名 常量值**

+ 通常在文件上方定义，表示一个常量

#### 1.2 const修饰的变量

一般形式：**const 数据类型 常量名 = 常量值;**

+ 通常在变量定义前加关键词const，修饰该变量为常量，不可修改

  

### 2、sizeof 关键字

**作用：**统计数据类型所占内存大小

**语法：**sizeof(数据类型/变量名)

**示例：**

```c++
int main()
{
    cout << "short 类型所占内存空间为：" << sizeof(short) << endl;
    cout << "int 类型所占内存空间为：" << sizeof (int) << endl;
    system("pause");
    return 0;
}
```



### 3、转义字符

**作用：**用于表示一些不能显示出来的ASCII字符

现阶段常用的转义字符：\n (换行)—010		\\\\ \(反斜杠)—092		\t (水平制表符)—009

**示例：**

```c++
int main()
{
	cout << "abc\n" << endl;
	cout << "\\" << endl;
	cout << "abc\t123" <<endl;
	system(pause);
	return 0;
}
```



### 4、字符与字符串

#### 4.1 字符

**形式：**char 变量名='字符值' ;

**示例：**`char ch='a' ;`

#### 4.2 字符串

**两种风格**

+ C风格：`char 变量名[] = "字符串值" ;`

示例：

```c++
int main()
{
    char str[] = "hello world";
    cout << str << endl;
    system(pause);
    return 0;
}
```

+ C++风格：`string 变量名 = "字符串值" ;`          注：需要加入头文件 ==#include<string>==

示例：

```c++
int main()
{
    string str = "hello world";
    cout << str << endl;
    system(pause);
    return 0;
}
```



### 5、布尔类型bool  （占1个字节）

**作用：**布尔数据类型代表真或假的值

bool类型只有两个值：

+ true —— 真（本质是1）
+ false —— 假（本质是0）

示例：

```c++
int main()
{
    bool flag = true;
    cout << flag << endl;	//输出1
    
    flag = false;
    cout << flag << endl;	//输出0
    
    system(pause);
    return 0;
}
```



### 6、数据的输入

**作用：**从键盘获取数据

**语法：**`cin >> 变量;`

示例：

```c++
int main()
{
    int a=0;
    cout << "请输入变量的值："<< endl;
    cin >> a;
    cout << a << endl;
    
    system(pause);
    return 0;
}
```



### 7、一些基础的算法

#### 7.1 逆序输出

**代码：**

```c++
#include<iostream>
using namespace std;
int main()
{
	int a[] = { 1,2,3,4,5 };
	cout << "顺序输出：" << endl;
	for (int i = 0; i < 5; i++)
	{
		cout <<  a[i] << endl;
	}

	//首尾交换顺序
	int star = 0, end = sizeof(a) / sizeof(a[0]) - 1;
	while (star < end)
	{
		int temp = a[star];
		a[star] = a[end];
		a[end] = temp;
		star++;
		end--;
	}

	cout << "逆序输出：" << endl;
	for (int i = 0; i < 5; i++)
	{
		cout << a[i] << endl;
	}

	system("pause");
	return 0;
}
```



#### 7.2 冒泡排序

**代码：**

```c++
#include<iostream>
using namespace std;
int main()
{
	int a[9] = { 2,4,6,1,7,9,4,5,6 };
	cout << "排序前：" << endl;
	for (int i = 0; i < 9; i++)
	{
		cout << a[i] << '\t';
	}
	cout << endl;
	for (int i = 0; i < 8; i++)		//总排序轮数=元素个数-1
	{
		for (int j = 0; j < 9 - i - 1; j++)		//对比次数=元素个数-当前轮数-1
		{
			if (a[j] > a[j+1])   //当前一个数大于后一个数，交换位置
			{
				int temp = a[j];
				a[j] = a[j+1];
				a[j+1] = temp;
			}
		}
	}
	cout << "排序后（升序）：" << endl;
	for (int i = 0; i < 9; i++)
	{
		cout << a[i] << '\t';
	}
	cout << endl;
	system("pause");
	return 0;
}
```

#### 7.3 选择排序

**代码：**

```c++
#include<iostream>
using namespace std;
int main()
{
	int a[9] = { 2,6,9,1,5,7,4,8,7 };
	cout << "排序前：" << endl;
	for (int i = 0; i < 9; i++)
	{
		cout << a[i] << '\t';
	}
	cout << endl;

	for (int i = 0; i < 8; i++)		//总排序次数=元素个数-1
	{
		for (int j = i+1; j < 9; j++)		//将x依次与后面的数比较，每次都要比较元素个数次
		{
			if (a[i] > a[j])				//当选择的这个数比下一个数大时，交换位置
			{
				int temp = a[i];
				a[i] = a[j];
				a[j] = temp;
			}
		}
	}

	cout << "排序后（升序）：" << endl;
	for (int i = 0; i < 9; i++)
	{
		cout << a[i] << '\t';
	}
	cout << endl;
	system("pause");
	return 0;
}
```



### 8、二维数组名的作用

#### 8.1 查看数组占用内存空间大小

```c++
int arr[2][3]=
{
    {1,2,3},
    {4,5,6}
}
cout<<"二维数组占用内存空间为："<<sizeof(arr)<<endl;
cout<<"二维数组第一行占用内存为："<<sizeof(arr[0])<<endl;
cout<<"二维数组第一个元素占用内存为："<<sizeof(arr[0][0])<<endl;
```

#### 8.2 求数组的行数和列数

```c++
cout<<"二维数组行数为："<<sizeof(arr)/sizeof(arr[0])<endl;
cout<<"二维数组列数为："<<sizeof(arr[0])/sizeof(arr[0][0])<<endl;
```



### 9、函数

**语法：**

```
返回值类型  函数名  参数列表
{
	函数体语句
	
	return 表达式
} 
```

**示例（求和函数）：**

```c++
#include<iostream>
using namespace std;
int add(int num1, int num2)
{
	int sum = num1 + num2;
	return sum;
}
int main()
{
	int a, b;
	cin >> a;
	cin >> b;
	int c = add(a, b);
	cout << a << "+" << b << "=" << c << endl;
	system("pause");
	return 0;
}
```

  

### 10、函数的分文件编写

**步骤：**

1、创建 .h后缀名的头文件

2、创建 .cpp后缀名的源文件

3、在头文件中写函数的声明

4、在源文件中写函数的定义

**示例：**

写一个交换数据的函数：

  ![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/94273734a6968caed77c495b0a3b62b7.png)

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/13d1e265dbd2dc82b7bcb515a7b94f7b.png)

![](D:\Snipaste截图\Snipaste_2021-07-27_16-40-17.png)



### 11、指针

#### 11.1 指针的基本概念

##### 11.1.1 定义指针：

```c++
数据类型 *变量名;
```

##### 11.1.2 使用指针：

```c++
int a=10;
int *p;
p=&a;		//p指向a的地址	*p指向a地址里的值（*p表示指针的解引用）
```

![](D:\Snipaste截图\Snipaste_2021-07-27_17-03-08.png)

##### 11.1.3指针所占内存大小：

在32位操作系统中，无论什么类型的指针都占4个字节

在64位操作系统中，无论什么类型的指针都占8个字节

#### 11.2 空指针

**空指针：**指针变量指向内存中编号为0的空间

**用途：**初始化指针变量

**注意：**空指针指向的内存是不可以访问的

```c++
int main()
{
    int *p=NULL;		//指针变量p指向内存地址编号为0的空间
    cout<<*p<<endl;		//报错，内存编号0~255为系统占用内存，不允许用户访问
    system("pause");
    return 0;
}
```

#### 11.3 const修饰指针

const修饰指针有三种情况：、

```c++
int a=10,b=10;
int *p=&a;
```

##### 11.3.1 const修饰指针	----常量指针

```c++
const int *p=&a;
```

**特点：**指针的指向可以修改，但是指针指向的值不可以修改。

例：

```c++
*p=20;		//错误
p=&b;		//正确
```



##### 11.3.2 const修饰常量	----指针常量

```c++
int * const p=&a;
```

**特点：**指针的指向不可以改，指针指向的值可以改。

例：

```c++
*p=20;		//正确
p=&b;		//错误
```



##### 11.3.3 const既修饰指针，又修饰常量

```c++
const int * const p=&a;
```

**特点：**指针的指向和指针指向的值都不可以改

例：

```c++
*p=20;		//错误
p=&b;		//错误
```



#### 11.4 指针和数组

 **作用：**利用指针访问数组中的元素

**示例：**

```c++
int main()
{
    int a[]={1,2,3,4,5,6,7,8,9,10};
    int *p=a;		//指向数组的指针，指向数组a的首地址a[0]
    for(int i=0;i<10;i++)
    {
        cout<<*p<<endl;		//利用指针遍历数组
        p++;
    }
    system("pause");
    return 0;
}
```



#### 11.5 指针和函数

**作用：**利用指针作函数参数，可以修改实参的值

**示例：**

```c++
void swap1(int a,int b)			//值传递
{
    int temp=a;
    a=b;
    b=temp;
}

void swap2(int *p1,int *p2)		//地址传递
{
    int temp=*p1;
    *p1=*p2;
    *p2=temp;
}
int main()
{
    int a=10,b=20;
    swap1(a,b);			//值传递不会改变实参	
    
    swap2(&a,&b);		//地址传递会改变实参
    system("pause");
    return 0;
}
```



#### 11.6 指针，数组，函数

**案例描述：**封装一个函数，利用冒泡排序法，实现对整型数组的升序排序。

**示例：**

```c++
#include<iostream>
using namespace std;
void sort(int* p, int len)		//排序函数
{
	for (int i = 0; i < len-1; i++)
	{
		for (int j = 0; j < len - i - 1; j++)
		{
			if (p[j] > p[j + 1])
			{
				int temp = p[j];
				p[j] = p[j + 1];
				p[j + 1] = temp;
			}
		}
	}
}

void print(int* p, int len)		//打印函数
{
	for (int i = 0; i < len; i++)
	{
		cout << *p << endl;
		p++;
	}
}

int main()
{
	int a[] = { 2,4,1,9,7,6,8,10,3,5 };
	int len = sizeof(a) / sizeof(a[0]);
	sort(a, len);
	print(a, len);
	system("pause");
	return 0;
}
```



### 12、结构体

#### 12.1 结构体的基本概念

结构体属于用户自定义的数据类型，允许用户存储不同的数据类型

#### 12.2 结构体定义和使用

**语法：**`struct 结构体名 {结构体成员列表};`

示例：

```c++
struct Student
{
    string name;
    int age;
    int score;
};
```

通过结构体创建变量的方式有三种：

- struct 结构体名 变量名

```c++
int main()
{
    struct Student stu1;
    stu1.name="张三";
    stu1.age=18;
    stu1.score=95;
}
```

- struct 结构体名 变量名={成员1值，成员2值...}

```c++
int main()
{
    struct Student stu2={"李四",19,90};
}
```

- 定义结构体时顺便创建变量

```c++
struct Student
{
    string name;
    int age;
    int score;
}stu3;		//在此创建变量
int main()
{
    stu3.name="王五";
    stu3.age=16;
    stu3.score=85;
}
```



#### 12.3 结构体数组

**作用：**将自定义的结构体放入到数组中方便维护

**语法：**`struct 结构体名 数组名[元素个数]={ {}，{}，...{}};`

示例：

```c++
struct Student
{
    string name;
    int age;
    int score;
};
int main()
{
    struct Student stu[3];
    stu[0].name="张三";
    stu[0].age=18;
    stu[0].score=95;
}
```



#### 12.4 结构体指针

**作用：**通过指针访问结构体中的成员

- 利用操作符 `->` 可以通过结构体指针访问结构体属性

**示例：**

```c++
struct Student
{
    string name;
    int age;
    int score;
};
int main()
{
    struct Student stu={"张三"，18，95};
    struct Student *p=&stu;
    cout<<"名字:"<<p->name<<"年龄:"p->age<<"分数:"<<p->score<<endl;
}
```



#### 12.5 结构体嵌套结构体

**作用：**结构体中的成员可以是另一个结构体

例：教师的信息中有所带学生的信息

```c++
#include<iostream>
using namespace std;
#include<string.h>
struct Student
{
	string name;
	int age;
	int score;
};

struct Teacher
{
	int id;
	string name;
	int age;
	struct Student stu;		//子结构体 学生
};

int main()
{
	struct Teacher t;
	t.id = 1001;
	t.name = "陆老师";
	t.age = 23;

	t.stu.name = "小王";
	t.stu.age = 18;
	t.stu.score = 90;

	system("pause");
	return 0;
}
```



#### 12.6 结构体做函数参数

**作用：**将结构体作为参数向函数中传递

传递方式：

- 值传递（函数中若形参改变了，但在main函数中实参不会改变）
- 地址传递（函数中若形参改变了，在main函数中实参也会改变）

**示例：**

```c++
#include<iostream>
using namespace std;
#include<string.h>
struct Student
{
	string name;
	int age;
	int score;
};
void print1(struct Student s)		//值传递，形参不会影响实参
{
	cout << s.name << '\t' << s.age << '\t' << s.score << endl;
}

void print2(struct Student* p)		//地址传递，形参会影响实参
{
	cout << p->name << '\t' << p->age << '\t' << p->score << endl;
}
int main()
{
	struct Student s;
	s.name = "张三";
	s.age = 18;
	s.score = 95;
	print1(s);
	print2(&s);
	system("pause");
	return 0;
}
```



#### 12.7 const的使用情景

**作用：**当结构体做函数参数是地址传递时，用const来防止误操作（在函数中修改了结构体的值）

**示例：**（以打印函数为例）

```c++
void print(const struct Student *p)
{
    p->age=100;		//报错，加了const修饰，其结构体中的值不可修改
    cout << p->name << p->age << p->score;
}
```



#### 12.8 结构体案例

##### 12.8.1 案例1

**案例描述：**

学校正在做毕设项目，每名老师带领5个学生，总共有3名老师，需求如下：

​		设计学生和老师的结构体，其中在老师的结构体中，有老师姓名和一个存放5名学生的数组作为成员。学生的成员有姓名、考试分数，创建数组存放3名老师，通过函数给每个老师及所带的学生赋值，最终打印出老师数据以及老师所带的学生数据。

**代码：**

```c++
#include<iostream>
using namespace std;
#include<string.h>
struct Student
{
	string name;
	int score;
};
struct Teacher
{
	string name;
	struct Student stu[5];
};
void InputInfor(struct Teacher t[], int len)
{
	for (int i = 0; i < len; i++)
	{
		cout << "请输入第" << i+1 << "个老师的名字：" ;
		cin >> t[i].name;
		for (int j = 0; j < 5; j++)
		{
			cout << "请输入该老师所带学生的姓名：";
			cin >> t[i].stu[j].name;
			cout << "请输入该老师所带学生的分数：";
			cin >> t[i].stu[j].score;
			cout << endl;
		}
		cout << endl;
	}
}
void print(struct Teacher t[], int len)
{
	for (int i = 0; i < len; i++)
	{
		cout << "老师姓名：" << t[i].name << endl;
		for(int j = 0; j < 5; j++)
		{
			cout << "\t学生姓名：" << t[i].stu[j].name << '\t' << "学生分数：" << t[i].stu[j].score << endl;
		}
	}
}
int main()
{
	struct Teacher t[3];
	int len = sizeof(t) / sizeof(t[0]);
	InputInfor(t, len);
	cout << endl;
	print(t, len);
	system("pause");
	return 0;
}
```

**演示：**

![](D:\Snipaste截图\Snipaste_2021-07-31_18-39-49.png)





### 课设—通讯录管理系统

**功能需求：**

- 添加联系人：向通讯录中添加新人(最多联系人为1000个)，信息包括姓名、性别、年龄、电话、地址
- 显示联系人：显示通讯录中所有联系人信息
- 删除联系人：按照姓名进行删除指定联系人
- 查找联系人：按照姓名查看指定联系人信息
- 修改联系人：按照姓名重新修改指定联系人
- 清空联系人：清空通讯录中所有信息
- 退出通讯录：退出当前使用的通讯录

**代码：**

```c++
#include<iostream>
using namespace std;
#include<string.h>
#define Max 1000		//宏定义一个Max，可方便修改通讯录的最大容量
struct PersonInfor
{
	string name;
	string sex;
	int age;
	string number;
	string address;
};
struct AddressList		//再定义一个通讯录结构体，方便管理联系人信息的结构体和记录通讯录人数
{
	struct PersonInfor Array[Max];
	int size;
};
void showMenu()
{
	cout << "*******************************************" << endl;
	cout << "************* 通讯录管理系统 **************" << endl;
	cout << "***  1、添加联系人 \t 2、显示联系人  ***" << endl;
	cout << "***  3、删除联系人 \t 4、查找联系人  ***" << endl;
	cout << "***  5、修改联系人 \t 6、清空联系人  ***" << endl;
	cout << "***\t\t 0、退出通讯录 \t\t***" << endl;
	cout << "*******************************************" << endl;
}
void addPerson(struct AddressList *p)		//添加联系人
{
	if (p->size >= Max)
	{
		cout << "通讯录已达到最大人数限制,无法添加" << endl;
		return ;
	}
	else
	{
		cout << "姓名：";
		cin >> p->Array[p->size].name;
		cout << "性别：";
		cin >> p->Array[p->size].sex;
		cout << "年龄：";
		cin >> p->Array[p->size].age;
		cout << "电话：";
		cin >> p->Array[p->size].number;
		cout << "住址：";
		cin >> p->Array[p->size].address;
		cout << "添加成功！" << endl;
		p->size++;			//更新通讯录人数
	}
	system("pause");
}
void showPerson(struct AddressList* p)
{
	if (p->size == 0)
	{
		cout << "通讯录无联系人！" << endl;
	}
	else
	{
		cout << "姓名\t" << "性别\t" << "年龄\t" << "电话\t\t" << "住址" << endl;
		for (int i = 0; i < p->size; i++)
		{
			cout << p->Array[i].name << "\t" << p->Array[i].sex << "\t" << p->Array[i].age << "\t" << p->Array[i].number << "\t" << p->Array[i].address << endl;
		}
	}
	system("pause");
}
int reserchPerson(struct AddressList* p,string name)	//封装一个查找函数，若有此人返回其在数组中的位置,无此人返回-1
{
	for (int i = 0; i < p->size; i++)
	{
		if (p->Array[i].name == name)
		{
			return i;
		}
	}
	return -1;
}
void deletePerson(struct AddressList* p)		//删除联系人
{
	string name;
	cout << "请输入要删除的联系人姓名：";
	cin >> name;
	int ret=reserchPerson(p,name);
	if (ret != -1)
	{
		for (int i = ret; i < p->size; i++)		//采用往前覆盖式删除
		{
			p->Array[i] = p->Array[i + 1];		//数据前移
		}
		p->size--;
		cout << "删除成功！" << endl;
	}
	else
	{
		cout << "无此人，删除失败！" << endl;
	}
	system("pause");
}
void modifyPerson(struct AddressList* p)		//修改联系人
{
	string name;
	cout << "请输入要修改的联系人姓名：";
	cin >> name;
	int ret = reserchPerson(p, name);
	if (ret != -1)
	{
		cout << "姓名：";
		cin >> p->Array[ret].name;
		cout << "性别：";
		cin >> p->Array[ret].sex;
		cout << "年龄：";
		cin >> p->Array[ret].age;
		cout << "电话：";
		cin >> p->Array[ret].number;
		cout << "住址：";
		cin >> p->Array[ret].address;
		cout << "修改成功！" << endl;
	}
	else
	{
		cout << "无此人，修改失败！" << endl;
	}
	system("pause");
}
void clearAddress(struct AddressList* p)		//清空通讯录
{
	p->size = 0;					//只需将人数归零，因为显示联系人时是用人数来输出的
	cout << "通讯录清空成功！" << endl;
	system("pause");
}
int main()
{
	struct AddressList people;		//创建通讯录结构体变量people
	people.size = 0;		//初始化通讯录人数
	int input;
	while (1)
	{
		showMenu();
		cin >> input;
		switch (input)
		{
			case 1:
				{
					addPerson(&people);
					system("cls");
					break;
				}
			case 2:
				{
					showPerson(&people);
					system("cls");
					break;
				}
			case 3:
				{
					deletePerson(&people);
					system("cls");
					break;
				}
			case 4:
				{
					string name;
					cout << "请输入要查找的联系人姓名：";
					cin >> name;
					int ret = reserchPerson(&people, name);
					if (ret != -1)
					{
						cout << "您查找的联系人信息如下：" << endl;
						cout << "姓名\t" << "性别\t" << "年龄\t" << "电话\t\t" << "住址" << endl;
						cout << people.Array[ret].name << "\t" << people.Array[ret].sex << "\t" << people.Array[ret].age << "\t" << people.Array[ret].number << "\t" << people.Array[ret].address << endl;
					}
					else
					{
						cout << "查无此人！" << endl;
					}
					system("pause");
					system("cls");
					break;
				}
			case 5:
				{
					modifyPerson(&people);
					system("cls");
					break;
				}
			case 6:
				{
					cout << "确定要清空通讯录吗？  （1：是   2：否）" << endl;
					int a;
					cin >> a;
					if (a == 1)
					{
						clearAddress(&people);
					}
					system("cls");
					break;
				}
			case 0:
				{
					cout << "确定要退出通讯录管理系统吗？  （1：是   2：否）" << endl;
					int a;
					cin >> a;
					if (a == 1)
					{
						system("pause");
						return 0;
					}
					system("cls");
					break;
				}
			default:
				break;
		}
	}
	system("pause");
	return 0;
}
```





#  	C++核心编程

本阶段主要针对C++面型对象编程技术，探讨C++中的核心和精髓

### 1、内存分区模型

C++程序在执行时，将内存大方向划分为**4个区域**：

- 代码区：存放函数体的二进制代码，由操作系统进行管理。
- 全局区：存放全局变量和静态变量以及常量。
- 栈区：由编译器自动分配释放，存放函数的参数值，局部变量等。
- 堆区：由程序员分配和释放，若程序员不释放，程序结束时由操作系统回收。



**内存四区的意义：**

不同区域存放的数据，赋予不同的生命周期，给我们更大的灵活编程。



#### 1.1 程序运行前

在程序编译后，生成了exe可执行程序，未执行该程序前分为两个区域

**代码区：**

存放CPU执行的机器指令

代码区时**共享**的，共享的目的时对于频繁被执行的程序，只需要在内存中有一份代码即可

代码区是**只读**的，便其只读的原因是防止程序意外地修改了它的指令

**全局区：**

全局变量和静态变量存放在此（静态变量：在普通变量前加==static==即可）

全局区还包括了常量区，字符串常量和其他常量也存放在此

==该区域地数据在程序结束后由操作系统释放==

==注：==const修饰的全局变量放在全局区，const修饰的局部变量放在栈区

| 全局区                                                       | 不在全局区          |
| ------------------------------------------------------------ | ------------------- |
| 全局变量、静态变量   static关键字、常量、字符串常量、const修饰的全局变量 | const修饰的局部变量 |



#### 1.2 程序运行后

**栈区：**

由编译器自动分配释放，存放函数的参数值(包括形参数据)，局部变量等

注意事项：不要返回局部变量的地址，栈区开辟的数据由编译器自动释放

**例：**

```c++
#include<iostream>
using namespace std;
int* func()
{
	int a = 10;		//局部变量，存放在栈区，栈区的数据在函数执行完后自动释放
	return &a;		//返回局部变量的地址
}
int main()
{
	int* p = func();
	cout << *p << endl;		//第一次可以打印正确的数字，是因为编译器做了保留
	cout << *p << endl;		//第二次就不再保留了，就会出现乱码
	system("pause");
	return 0;
}
```

![](D:\Snipaste截图\Snipaste_2021-08-02_21-00-54.png)



**堆区：**

由程序员分配释放，若程序员不释放，程序结束时由操作系统回收

在C++中主要利用==new==在堆区开辟内存

**示例：**

```c++
#include<iostream>
using namespace std;
int* func()
{
	int* p = new int(10);	//利用new关键字，可以将数据开辟到堆区
	return p;
}
int main()
{
	int* p = func();
	cout << *p << endl;	
	cout << *p << endl;			//两次打印的都是10
    //利用delete释放堆区数据
    delete p;
    cout << *p << endl;			//报错，释放的空间不可访问了
	system("pause");
	return 0;
}
```



#### 1.3 new操作符

C++中利用==new==操作符在堆区开辟数据  (C语言中利用malloc)

堆区开辟的数据，由程序员手动开辟，手动释放，释放利用操作符==delete==

**开辟数组：**

```c++
#include<iostream>
using namespace std;
int main()
{
    int* arr = new int[10];		//堆区开辟数组
    for (int i = 0; i < 10; i++)		//对数组赋值
    {
        arr[i] = i;
    }
    for (int i = 0; i < 10; i++)
    {
        cout << arr[i] << endl;
    }
    delete[] arr;		//释放数组时，要在delete后加 []
    system("pause");
    return 0;
}
```



**C++中的new和delete与C语言中的malloc和free的联系与区别：**

相同点：都可用于申请动态内存和释放内存

不同点：

- 操作对象有所不同。malloc和free是C++/C语言的标准库函数，new和delete是C++的运算符。

- 用法不同。`int *p=(int*) malloc(sizeof(int));`          `free(p);`

  ​		  		 `int *p=new int;`			`delete p;`

因为new内置了sizeof、类型转换和类型安全检查功能。

new/delete的功能完全覆盖了malloc/free，为什么C++还要保留，malloc/free呢？

因为C++程序经常要调用C函数，而C程序只能用malloc/free管理动态内存。



### 2、引用

#### 2.1 引用的基本使用

**作用：**给变量起别名

**语法：**`数据类型  &别名 = 原名;`（原名和别名都是同一块地址，一变全变）

```c++
int main()
{
    int a=10;
    int &b=a;
    cout<<"a="<<a<<endl;		//a=10
    cout<<"b="<<b<<endl;		//b=10
    b=100;
    cout<<"a="<<a<<endl;		//a=100
    cout<<"b="<<b<<endl;		//b=100
    system("pause");
    return 0;
}
```

#### 2.2 引用注意事项

- 引用必须初始化 -- `int &b;      //错误`
- 引用在初始化后就不能改变了 

```c++
int main()
{
    int a=10;
    int &b=a;
    int c=20;
    b=c;		//赋值操作，不是更改引用
    cout<<"a="<<a<<endl; 
    cout<<"b="<<b<<endl;
    cout<<"c="<<c<<endl;		//输出的a，b，c都等于20
}
```

#### 2.3 引用做函数参数

**作用：**函数传参时，可以利用引用的技术让形参修饰实参

**优点：**可以简化指针修改实参

**示例：**

```c++
#include<iostream>
using namespace std;
void swap(int& a, int& b)
{
	int temp = a;
	a = b;
	b = temp;
}
int main()
{
	int a = 10, b = 20;
	swap(a, b);
	cout << "a=" << a << endl;		//a=20
	cout << "b=" << b << endl;		//b=10
	system("pause");
	return 0;
}
```

#### 2.4 引用做函数返回值

**作用：**引用是可以作为函数的返回值存在的

**注意：**==不要返回局部变量引用==

**用法：**函数调用作为左值

**示例：**

```c++
#include<iostream>
using namespace std;
int& test01()
{
	int a = 10;		//局部变量，存放在栈区，栈区开辟的数据由编译器自动释放
	return a;
}
int& test02()
{
	static int a = 10;		//静态变量，存放在全局区，数据在程序结束后由系统释放
	return a;
}
int main()
{
	int& ref01 = test01();
	cout << "ref01=" << ref01 << endl;		//ref01=10   编译器会保留一次
	cout << "ref01=" << ref01 << endl;		//乱码

	int& ref02 = test02();		//ref02为别名
	cout << "ref02=" << ref02 << endl;		//ref02=10
	cout << "ref02=" << ref02 << endl;		//ref02=10
	
	test02() = 1000;		//函数调用作为左值，返回变量a，在对a赋值（原名赋值）
	cout << "ref02=" << ref02 << endl;		//ref02=1000 （别名输出）
	cout << "ref02=" << ref02 << endl;		//ref02=1000

	system("pause");
	return 0;
}
```

#### 2.5 引用的本质

**本质：**引用的本质在C++内部实现是一个指针常量

**示例：**

```c++
int main()
{
    int a=10;
    int& ref=a;	//自动转换为int* const ref=&a; 指针常量不可修改，所以引用不可修改
    ref=20;		//内部发现ref是引用，自动转换为： *ref=20;
    cout<<"a="<<a<<endl;		//a=20
    cout<<"ref="<<ref<<endl;	//ref=20
    
}
```

结论：C++推荐使用引用技术，因为语法方便，引用本质是指针常量，但是所有的指针操作编译器都帮我们做了

#### 2.6 常量引用

**作用：**常量引用主要用来修饰形参，防止误操作

在函数形参列表中，可以加==const修饰形参==，防止形参改变实参

**示例：**

```c++
#include<iostream>
using namespace std;
void print(int &s)
{
	s = 1000;		//当形参中的s被修改时，实参中的a也会随之改变
	cout << "s=" << s << endl;		//s=1000
}
int main()
{
	int a = 100;
	print(a);
	cout << "a=" << a << endl;		//a=1000
	system("pause");
	return 0;
}
```

```c++
但将上面的void print(int &s)改为void print(const int &s)
实参a就不会被改变，所以const可以在其他函数中防止误操作
```

```c++
int& ref=10;	//错误，引用本身需要一个合法的内存空间
const int& ref=10;		//正确，编译器会将代码优化为：int temp=10;  const int& ref=temp;

ref=100;		//错误，加了const后不可以修改变量
```



### 3、函数提高

#### 3.1 函数默认参数

在C++中，函数的形参列表中的形参时可以有默认值的

**语法：**`返回值类型  函数名  (参数=默认值) {} ` 

**注意：**

- 如果某个位置已经有了默认参数，那么从这个位置往后，从左到右都必须有默认值

  `int func(int a,int b=10,int c)     //错误，b往后都应有默认值`

- 函数声明和函数实现不能同时有默认参数

  ```c++
  int func(int a=10,int b=10)		//函数声明有了默认值
  int main()
  {
      ······
  }
  int func(int a,int b)			//函数实现就不能有默认值
  {
      ······
  }
  ```

- 当实参与默认值不同时，以实参为准

  ```c++
  int func(int a,int b=20,int c=30)
  {
      return a+b+c;
  }
  int main()
  {
      cout<<func(10,10)<<endl;		//输出：50
      system("pause");
      return 0;
  }
  ```

#### 3.2 函数占位参数

C++中函数的形参列表里可以有占位参数，用来做占位，调用函数时必须填补该位置

**语法：**`返回值类型  函数名  (数据类型) {}`

在现阶段函数的占位参数存在意义不大，但是后面的课程中会用到该技术

**示例：**

```c++
void func(int a,int)
{
    ······
}
int main()
{
    func(10,10);		//必须传个整型数值给占位参数
}
```

占位参数还可以有默认参数

```c++
void func(int a,int = 10)
{
    ······
}
int main()
{
    func(10);			//占位参数有默认参数时，就可以不用实参
}
```

#### 3.3 函数重载

##### 3.3.1 函数重载概述

**作用：**函数名可以相同，提高复用性

**函数重载满足条件：**

- 同一个作用域下
- 函数名称相同
- 函数参数的**类型不同**或者**个数不同**或者**顺序不同**

```c++
void func(double a)
{
    ·······
}
void func(int a)
{
    ······
}
void func(double a,int b)
{
    ······
}
void func(int a,double b)
{
    ······
}
```

注：函数的返回值不可以作为函数重载的条件。编译器在编译时，不会判断函数的返回类型，虽然返回类型不同，但是只有函数调用后，编译器才会去验证返回类型

```c++
void func(int a)
{
    ······
}
int func(int a)		//报错，此函数重载仅仅是返回值不同
{
    ······
}
```

##### 3.3.2 函数重载的注意事项

- 引用作为重载条件

  ```c++
  void func(int &a)		
  {
      ······
  }
  void func(const int &a)		//const int &a=10;   合法
  {
      ······
  }
  int main()
  {
      int a=10;
      func(a);		//调用func(int &a)
      
      func(10);		//调用func(const int &a)，因为int &a=10是不合法的
  }
  ```

- 函数重载碰到函数默认参数

  ```c++
  void func(int a)
  {
      ······
  }
  void func(int a,int b=10)
  {
      ······
  }
  int main()
  {
      func(10);		//错误，两个函数都可以被调用，编译器不知道调用哪个
      func(10,20);	//正确，调用第二个函数
  }
  ```



### 4、类和对象

C++面向对象的三大特征：==封装、继承、多态==

C++认为万事万物都皆为对象，对象上有其属性和行为

#### 4.1 封装

##### 4.1.1 封装的意义

封装是C++面向对象三大特征之一

封装的意义：

- 将属性和行为作为一个整体，表现生活中的事物

  在设计类的时候，属性和行为写在一起，表现事物

  

  **语法：**`class  类名{ 访问权限： 属性 / 行为 };`

  **示例：**设计一个圆类，求圆的周长

  ```c++
  #include<iostream>
  using namespace std;
  const double PI = 3.14;
  class circle 
  {
  public:               //pubilc是一个公共的访问权限
      int r;            //属性（一般是一些变量）
      double lenglh()   //行为（一般是一个函数）
      {
          return 2 * PI * r;
      }
  };
  int main()
  {
      circle c1;         //通过圆类，创建圆的对象c1(一个具体的对象)
      c1.r = 10;         //给圆对象的半径赋值
      cout << "圆的周长为：" << c1.lenglh() << endl;
      system("pause");
      return 0;
  }
  ```

  

  ==类中的属性和行为，统称为 成员。    成员属性--成员变量	    成员行为--成员方法==

  

- 将属性和行为加以权限控制

  类在设计时，可以把属性和行为放在不同的权限下，加以控制

  访问权限有三种：

  1. **public**		公共权限（类内可以访问，类外可以访问）
  2. **protected**          保护权限（类内可以访问，类外不可以访问。 子类可以访问父类的保护内容）
  3. **private**      私有权限（类内可以访问，类外不可以访问。 子类不可以访问父类的私有内容）



##### 4.1.2 class和struct的区别

在C++中，class和struct唯一的区别就在于**默认的访问权限不同**

区别：

- class  默认权限为==私有==
- struct  默认权限为==公共==

##### 4.1.3 成员属性设置为私有

**优点：**

- 将所有成员属性设置为私有，可以自己控制读写权限
- 对于写权限，我们可以检测数据的有效性

```c++
#include<iostream>
using namespace std;
#include<string.h>
class Person
{
public:							//设置一个公共成员，用来给private中的成员读写
    //名字可读可写
    void setName(string name)
    {
        m_Name=name;
    }
    string getName()
    {
        return m_Name;
    }
    
    //年龄只可读
    int getAge()
    {
        m_Age=18;
        return m_Age;
    }
    
    //性别只可写
    void setSex(string sex)
    {
        m_Sex=sex;
    }
private:				//私有成员，可通过类内的公共成员来控制读写
    string m_Name;
    int m_Age;
    string m_Sex;
};
int main()
{
    Person p;
    p.setName("张三");
    cout<<"姓名为："<<p.getName()<<endl;
    cout<<"年龄为："<<p.getAge()<<endl;
    p.setSex("男");
    
    system("pause");
    return 0;
}
```

##### 4.1.4 练习案例

###### 案例1：设计立方体类

设计立方体类(Cube)

求出立方体的面积和体积

分别用全局函数和成员函数判断两个立方体是否相等

**代码：**

```c++
#include<iostream>
using namespace std;
class Cube
{
public:
	void setL(int l)
	{
		m_L = l;
	}
	int getL()
	{
		return m_L;
	}
	void setW(int w)
	{
		m_W = w;
	}
	int getW()
	{
		return m_W;
	}
	void setH(int h)
	{
		m_H = h;
	}
	int getH()
	{
		return m_H;
	}
	int getS()
	{
		return 2 * (m_L * m_W + m_L * m_H + m_W * m_H);
	}
	int getV()
	{
		return m_L * m_W * m_H;
	}

	bool judgeByName(Cube& c)		//利用成员函数判断,只用传一个参数，因为调用时要用一个成员
	{
		if (m_L == c.getL() && m_W == c.getW() && m_H == c.getH())
		{
			return true;
		}
		return false;
	}
private:
	int m_L;
	int m_W;
	int m_H;
};

bool judge(Cube& c1, Cube& c2)		//利用全局函数判断
{
	if (c1.getL() == c2.getL() && c1.getW() == c2.getW() && c1.getH() == c2.getH())
	{
		return true;
	}
	return false;
}
int main()
{
	Cube c1;
	c1.setL(10);
	c1.setW(10);
	c1.setH(10);
	cout << "面积为：" << c1.getS() << endl;
	cout << "体积为：" << c1.getV() << endl;
	Cube c2;
	c2.setL(10);
	c2.setW(10);
	c2.setH(10);
	
	//利用全局函数判断
	bool ret = judge(c1, c2);
	if (ret)
	{
		cout << "c1和c2相等" << endl;
	}
	else
	{
		cout << "c1和c2不相等" << endl;
	}

	//利用成员函数判断
	ret = c1.judgeByName(c2);
	if (ret)
	{
		cout << "c1和c2相等" << endl;
	}
	else
	{
		cout << "c1和c2不相等" << endl;
	}

	system("pause");
	return 0;
}
```

###### 案例2：点和圆的关系

设计一个圆形类(Circle)，和一个点类(Point)，计算点和圆的关系

![](D:\Snipaste截图\Snipaste_2021-08-15_16-25-30.png)

```c++
#include<iostream>
using namespace std;
#include<math.h>
class Point			//点类
{
public:
	void setX(int x)
	{
		m_X = x;
	}
	void setY(int y)
	{
		m_Y = y;
	}
	int getX()
	{
		return m_X;
	}
	int getY()
	{
		return m_Y;
	}
private:
	int m_X;
	int m_Y;
};
class Circle		//圆类
{
public:
	void setR(int r)
	{
		m_R = r;
	}
	int getR()
	{
		return m_R;
	}
	void setCenter(Point center)
	{
		m_Center = center;
	}
	Point getCenter()
	{
		return m_Center;
	}
private:
	int m_R;
	Point m_Center;		//让另一个类作为该类的成员
};
void Judge(Circle& c, Point& p)		//判断点到圆心的距离和半径的大小
{
	int distance = sqrt(pow(c.getCenter().getX() - p.getX(), 2) + pow(c.getCenter().getY() - p.getY(), 2));
	int R = c.getR();
	if (distance == R)
	{
		cout << "点在圆上" << endl;
	}
	if (distance > R)
	{
		cout << "点在圆外" << endl;
	}
	if (distance < R)
	{
		cout << "点在圆内" << endl;
	}
}
int main()
{
	Circle c;		//创建圆c
	c.setR(10);
	Point center;	//创建圆心center
	center.setX(10);
	center.setY(0);
	c.setCenter(center);	//圆c的圆心
	Point p;		//创建点
	p.setX(10);
	p.setY(9);
	Judge(c, p);
	system("pause");
	return 0;
}
```

**注：**在开发过程中，所有类不可能写在一个文件中，类一般要拆分成很多个文件编写。如下：

<img src="D:\Snipaste截图\Snipaste_2021-08-15_16-56-49.png"  />

![](D:\Snipaste截图\Snipaste_2021-08-15_16-57-13.png)

![](D:\Snipaste截图\Snipaste_2021-08-15_16-57-31.png)

![](D:\Snipaste截图\Snipaste_2021-08-15_16-57-43.png)

![](D:\Snipaste截图\Snipaste_2021-08-15_16-54-29.png)

**重点：**另一个类可以作为其他类的成员，类似结构体。 将类编写在多个文件中。



#### 4.2 对象的初始化和清理

- C++中的面向对象来源于生活，每个对象也都会有初始设置以及对象销毁前的清理数据的设置

##### 4.2.1 构造函数和析构函数

对象的初始化和清理是两个非常重要的问题

​		一个对象或者变量没有初始状态，对其使用后果也是未知的

​		同样的使用完一个对象或变量，没有及时清理，也会造成一定的安全问题

C++利用了**构造函数**和**析构函数**解决上诉问题，这两个函数会被编译器自动调用，完成对象初始化和清理工作。对象的初始化和清理工作时编译器强制要我们做的事，**因此如果我们不提供构造和析构，编译器会提供编译器提供的构造函数和析构函数是空实现。**

- 构造函数：主要作用在于创建对象时为对象的成员属性赋值，构造函数由编译器自动调用，无需手动调用。
- 析构函数：主要作用在于对象销毁前系统自动调用，执行一些清理工作。



**构造函数语法：**`类名() {}`

	1. 构造函数，没有返回值也不写void
	2. 函数名称与类名相同
	3. 构造函数可以有参数，因此可以发生重载
	4. 程序在调用对象时候会自动调用构造，无需手动调用，而且 **只会调用一次**



**析构函数语法：**`~类名() {}`

	1. 析构函数，没有返回值也不写void
	2. 函数名称与类名相同，在名称前加上符号~
	3. 析构函数不可以有参数，因为不可以发生重载
	4. 程序在对象销毁前会自动调用析构，无需手动调用，而且 **只会调用一次*

**示例：**

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	Person()
	{
		cout << "Person的构造函数调用" << endl;
	}

	~Person()
	{
		cout << "Person的析构函数调用" << endl;
	}
};
void test01()
{
	Person p;	//局部变量，在栈区的数据，test01执行完后，释放这个对象
}

int main()
{
	test01();
	system("pause");
	return 0;
}

/*
输出：   Person的构造函数调用
		Person的析构函数调用
		
		test01调用完后会释放这个对象，此函数结束后就会执行析构函数
*/
```

```c++
int main()
{
	Person p;
	system("pause");
	return 0;
}

/*
输出：	Person的构造函数调用
		
	    函数调用完后不会释放这个对象，整个main函数执行完了才会执行析构函数
*/
```



##### 4.2.2 构造函数的分类及调用

两种分类方式：

​		按参数分为：有参构造和无参构造

​		按类型分为：普通构造和拷贝构造



三种调用方法：

​		括号法，显示法，隐式转换法

**示例：**

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	int age;
	Person()	//无参构造函数
	{
		cout << "Person的无参构造函数调用" << endl;
	}

	Person(int a)	//有参构造函数
	{
		age = a;
		cout << "Person的有参构造函数调用" << endl;
	}

	//拷贝构造函数
	Person(const Person& p)  //用const修饰防止误修改
	{
		age = p.age;
	}

	~Person()
	{
		cout << "Person的析构函数调用" << endl;
	}
};
void test01()
{
	//1.括号法
	Person p1;
	Person p2(18);
	Person p3(p2);
	cout << p2.age << p3.age << endl;  //输出p2 p3都是18

	//注：调用默认构造函数时，不要加()  编译器会认为时一个函数声明

	//2.显示法
	Person p1;
	Person p2 = Person(18);
	Person p3 = Person(p2);

	//注：1. Person(10);  为匿名对象。当前行执行结束后，系统会立即回收匿名对象(调用析构函数)
	//       2. Person(p3);  不要利用拷贝构造函数初始化匿名对象，编译器会认为Person(p3)=Person p3;(重定义错误)

	//3.隐式转换法
	Person p4 = 18;		//相当于写了 Person p4 = Person(18); 有参构造 
	Person p5 = p4;		//拷贝构造
}

int main()
{
	test01();
	system("pause");
	return 0;
}
```



**附：转换构造函数**

一个构造函数接收一个不同于其类类型的形参，可以视为将其形参转换成类的一个对象，像这样的构造函数称为转换构造函数。因此转换构造函数的作用就是将一个其他类型的数据转换成一个类的对象。

除了创建类对象之外，转换构造函数还为编译器提供了执行隐式类型转换的方法。只要在需要类的类型值的地方，给定构造函数的形参类型的值，就将由编译器执行这种类型的转换。

转换构造函数是构造函数的一个特例，当一个构造函数的参数只有一个，而且是一个其他类型的 const 引用时，它就是一个转换构造函数。

例如：

```c++
class T1{};
class T2
{  
public:
	T2(const T1 &t);   // 从 T1 转换到 T2 的转换构造函数
};
```

有了转换构造函数，就可以实现不同类型之间的类型转换了，比如：

```c++
/* 类定义同上 */
int main()
{  
T1 t1;
T2 t2= (T2)t1;   // 用类型转换语法，从 T1 转换到 T2  
T2 t3(t1);   // 或者直接调用转换构造函数
}
```

\> 注意：转换构造函数只能有一个参数。如果有多个参数，就不是转换构造函数。

==若要将T1转换为T2，则需要在T1的类中声明T2为友元类==

**示例：**

```c++
#include <iostream>
#include <string>
using namespace std;

// 前置声明 Teacher 类
class Teacher;

class Student
{
  friend Teacher;   //声明Teacher类为Student类的友元，否则转换拷贝函数的形参中无法访问变量
private:
  int number;
  string name;
  string sex;
public:
  Student(int num, string nam, string se);
  void Print();
};
//学生类的定义
Student::Student(int num, string nam, string se)
{
  number = num;
  name = nam;
  sex = se;
}
void Student::Print()
{
  cout << "学生：" << name << "，" << "编号：" << number << "，" << "性别：" << sex << endl;
}


class Teacher
{
  //教师类的声明
private:
  int number;
  string name;
  string sex;
public:
  Teacher(int num, string nam, string se);
  void Print();
  Teacher(const Student& t);
};
//教师类的定义
Teacher::Teacher(int num, string nam, string se)
{
  number = num;
  name = nam;
  sex = se;
}
void Teacher::Print()
{
  cout << "教师：" << name << "，" << "编号：" << number << "，" << "性别：" << sex << endl;
}
Teacher::Teacher(const Student& t)   //将Student类的数据转换到Teacher类上
{
  this->name = t.name;
  this->number = t.number;
  this->sex = t.sex;
}

int main()
{
  int number;
  string name, sex;
  cin >> number >> name >> sex;
  Student st(number, name, sex);
  st.Print();
  Teacher t = (Teacher)st;
  t.Print();
}
```



##### 4.2.3 拷贝构造函数调用时机

C++中拷贝构造函数的调用时机：

1. 使用一个已经创建完毕的对象来初始化一个新对象（赋值操作）

   ```c++
   Person p1(18);
   ```

2. 值传递的方式给函数参数传值（函数调用）

   ```c++
   void f1(Person p)
   {
       
   }
   void test()
   {
       Person P;
       f1(P);
   }
   ```

3. 值方式返回局部对象（return）

   ```c++
   Person f2()
   {
       Person p1;
       return p1;
   }
   void test()
   {
       Person p=f2();
   }
   ```



##### 4.2.4 构造函数的调用规则

默认情况下，C++编译器会至少给一个==类==添加3个函数

1. 默认构造函数（无参，函数体为空）
2. 默认析构函数（无参，函数体为空）
3. 默认拷贝构造函数，对属性进行值拷贝

构造函数调用规则如下：

- 如果用户定义有参构造函数，C++不在提供默认无参构造，但是会提供默认拷贝构造
- 如果用户定义拷贝构造函数，C++不会再提供其他构造函数



##### 4.2.5 深拷贝与浅拷贝

浅拷贝：简单的复制拷贝操作

深拷贝：在堆区重新申请空间，进行拷贝操作

**示例：**

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	int m_Age;
	int *m_Height;	//堆区开辟的是内存，用指针接收
	Person()
	{
		cout << "默认构造函数调用" << endl;
	}
	Person(int age,int height)
	{
		m_Age = age;
		m_Height = new int(height);
		cout << "有参构造函数调用" << endl;
	}
	Person(const Person& p)
	{
		cout << "拷贝构造函数调用" << endl;
		m_Age = p.m_Age;
	  //m_Height = p.m_Height; 编译器默认实现的代码（浅拷贝）
		m_Height = new int(*p.m_Height);	//深拷贝操作
	}	//若不写此深拷贝操作的函数，使用默认的浅拷贝函数，则会报错
	~Person()
	{
		//析构代码，将堆区开辟的数据释放干净
		if (m_Height != NULL)
		{
			delete m_Height;
			m_Height = NULL;
		}
		cout << "析构函数调用" << endl;
	}
};
void test01()
{
	Person p1(18, 180);
	Person p2(p1);
	cout << "p1的年龄：" << p1.m_Age << "身高：" << *p1.m_Height << endl;
	cout << "p2的年龄：" << p2.m_Age << "身高：" << *p2.m_Height << endl;
}
int main()
{
	test01();
	system("pause");
	return 0;
}
```

==解释：==若不利用深拷贝在堆区创建新内存，会导致浅拷贝带来的重复释放堆区内存的问题，会报错。

当p1和p2进行析构代码时，释放的时同一段内存，p2先释放了这段内存，p1再去释放的时候就是非法操作了。(释放内存时先进后出，所以是p2先释放)

![](D:\Snipaste截图\Snipaste_2021-10-10_20-29-58.png)

![](D:\Snipaste截图\Snipaste_2021-10-10_20-32-17.png)

总结：如果属性有在堆区开辟的，一定要自己提供拷贝构造函数，防止浅拷贝带来的问题。



##### 4.2.6 初始化链表

**作用：**C++提供了初始化列表语法，用来初始化属性

**语法：**`构造函数():  属性1(值1) , 属性2(值2),······  {}`

**示例：**

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	int m_A, m_B, m_C;
	//Person(int a, int b, int c)		//传统初始化
	//{
	//	m_A = a;
	//	m_B = b;
	//	m_C = c;
	//}
	Person(int a,int b,int c) :m_A(a), m_B(b), m_C(c)		//初始化列表初始化属性
	{

	}
};
void test01()
{
	//Person p1(10,20,30);
	Person p1(10, 20, 30);
	cout << "m_A=" << p1.m_A << endl;
	cout << "m_B=" << p1.m_B << endl;
	cout << "m_C=" << p1.m_C << endl;
}
int main()
{
	test01();
	system("pause");
	return 0;
}
```

==注意冒号的位置==



##### 4.2.7 类对象作为类成员

C++类中的成员可以是另一个类的对象，我们称该成员为 对象成员

例如：

```c++
class A{}
class B
{
    A a;
}
```

B类中有对象A作为成员，A为对象成员

**示例：**

```c++
#include<iostream>
using namespace std;
#include<string>
class Phone		//手机 类
{
public:
	string m_PName;		//手机类中有手机的名字
	Phone(string pName)
	{
		m_PName = pName;
	}
};
class Person		//人 类
{
public:
	string m_Name;		//人 类中有人的名字，拥有的手机
	Phone m_Phone;	
	Person(string name, string pName) :m_Name(name),m_Phone(pName)	//相当于Phone m_Phone = pName;(隐式转换法)
	{

	}
};
void test01()
{
	Person p("张三", "iPhone X");
	cout << p.m_Name << "拥有" << p.m_Phone.m_PName << endl;
}
int main()
{
	test01();
	system("pause");
	return 0;
}
```

==注意：==构造时先构造类中的其他对象，在构造本身。析构时顺序相反。



##### 4.2.8 静态成员

静态成员就是在成员变量和成员函数前加上关键字static，称为静态成员。

静态成员分为：

- 静态成员变量
  - 所有对象共享一份数据
  - 在编译阶段分配内存
  - 类内声明，类外初始化
- 静态成员函数
  - 所有对象共享一个函数
  - 静态成员函数只能访问静态成员变量

**静态成员变量示例：**

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	static int m_A;
};
//类内声明，类外初始化
int Person::m_A = 10;	//Person作用域下的m_A(因为此为全局区，所以写作用域防止混淆)
void test01()
{
	Person p1;
	cout << p1.m_A << endl;		//输出10
	Person p2;
	cout << p2.m_A << endl;		//输出10
	p2.m_A = 20;
	cout << p1.m_A << endl;		//输出20
}
int main()
{
	test01();
	system("pause");
	return 0;
}
```

静态成员变量不属于某个对象上，所有对象都共享同一份数据，因此静态成员变量有两种访问方式。

- 通过对象进行访问—Person p； cout<<p.m_A<<endl;
- 通过类名进行访问—cout<<Person: :m_A<<endl;

==注：==静态成员变量也是有访问权限的，如果变量定义在private下，则在类外访问不到该变量。



**静态成员函数示例：**

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	static int m_A;
	int m_B;
	static void func()
	{
		m_A = 10;	//静态成员函数可以访问静态成员变量
   		   //m_B = 20;	//报错，静态成员函数不可以访问非静态成员变量
		cout << "调用静态成员函数" << endl;
	}
};
void func();
void test01()
{
	//通过对象访问
	Person p;
	p.func();
	//通过类名访问
	Person::func();
}
int main()
{
	test01();
	system("pause");
	return 0;
}
```

==注：==静态成员函数同样也有访问权限。



#### 4.3 C++对象模型和this指针

##### 4.3.1 成员变量和成员函数分开存储

在C++中，类内的成员变量和成员函数分开存储

**只有非静态成员变量才属于类的对象上**

- 若定义一个空类，则该空对象占的内存空间为1字节

- 若类内有一个int类型，属于类的对象上，则该类占4字节，就没有了空对象的1字节
- 若类内有静态成员变量(static)，不属于类的对象上，则不计该变量的内存
- 若定义一个(非)静态成员==函数==，也不属于类的对象上，不计该函数的内存

##### 4.3.2 this指针概念

C++通过提供特殊的对象指针—this指针。来区分是哪个对象调用自己的。

**this指针指向被调用的成员函数所属的对象**

- this指针是隐含每一个非静态成员函数内的一种指针

- this指针不需要定义，直接使用即可

this指针的用途：

- 当形参和成员变量同名时，可用this指针来区分

  ```c++
  #include<iostream>
  using namespace std;
  class Person
  {
  public:
  	int age;
  	Person(int age)
  	{
  		this->age = age;	//this指针指向 被调用的成员函数 所属的对象（p1在调用成员函数，所以this指向p1）
  	}
  	void PersonAddAge(Person& p)
  	{
  		this->age += p.age;		//20行p2在调用该成员函数，this指向p2
  	}
  };
  void test01()
  {
  	Person p1(18);
  	Person p2(20);
  	p2.PersonAddAge(p1);
  	cout << "p2的年龄为：" << p2.age << endl;	//输出38
  }
  int main()
  {
  	test01();
  	system("pause");
  	return 0;
  }
  ```

  

- 在类的非静态成员函数中返回对象本身，可使用`return *this;`

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	int age;
	Person(int age)
	{
		this->age = age;	//this指针指向 被调用的成员函数 所属的对象（p1在调用成员函数，所以this指向p1）
	}
	Person &PersonAddAge(Person& p)	//若不采用引用，则返回的是值，不是p2的本体，构造函数创建了一个新的副本去存放该数据。采用引用就不会创建新的对象，一直返回的都是p2
	{
		this->age += p.age;		//20行p2在调用该成员函数，this指向p2
		return *this;	//this指向p2，则*this指向p2这个对象的本体
	}
};
void test01()
{
	Person p1(18);
	Person p2(20);
	p2.PersonAddAge(p1).PersonAddAge(p1).PersonAddAge(p1);	//链式编程思想
	cout << "p2的年龄为：" << p2.age << endl;	//输出74。若&PersonAddAge(Person& p)不采用引号，则输出还是38
}
int main()
{
	test01();
	system("pause");
	return 0;
}
```



##### 4.3.3 空指针访问成员函数

C++中空指针也是可以调用成员函数的，但是也要注意有没有用到this指针

如果用到this指针，需要加以判断保证代码的健壮性

**示例：**

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	int m_Age;
	void showClassName()
	{

	}
	void showPersonAge()
	{
		if (this == NULL)	//防止传入的指针为空
		{
			return;
		}
		cout << "age=" << this->m_Age << endl;
	}
};
void test01()
{
	Person* p = NULL;
	p->showClassName();
	p->showPersonAge();		//报错，this指针指向NULL，无法访问m_Age
}
int main()
{
	test01();
	system("pause");
	return 0;
}
```



##### 4.3.4 const修饰成员函数

**常函数：**

- 成员函数后加const后我们成这个函数为常函数

- 常函数内不可以修改成员属性

- 成员属性声明时加关键字mutable后，在常函数中也可以修改

  ```c++
  class Person
  {
  public:
  	int m_A;
  	void showPerson() const
  	{
  		m_A = 100;	//报错，相当于this->m_A=100;而this指针本质是指针常量，其指向不可修改
  	}
  };
  //this指针为指针常量，相当于Person *const this;其指向不可修改
  //若想让this指针指向的值也不可以修改，则为const Person *const this;相当于在成员函数后加const
  ```

  ```c++
  class Person
  {
  public:
  	mutable int m_B;
  	void showPerson() const
  	{
  		m_B = 100;	//正确
  	}
  };
  ```

**常对象：**

- 声明对象前加const后称该对象为常对象

- 常对象只能调用常函数

  ```c++
  class Person
  {
  public:
  	void showPerson() const
  	{
  		
  	}
  	void func()
  	{
  
  	}
  };
  void test01()
  {
  	const Person p;
  	p.showPerson();
  	p.func();	//报错，常对象不可以调用非 常函数
  }
  ```



#### 4.4 友元

在程序里，有些私有属性想让类外特殊的一些函数或者类进行访问，则需要用到友元技术，友元的目的就是让一个函数或者类访问另一个类中私有成员

友元的关键字为：==friend==

友元的三种实现

- 全局函数做友元
- 类做友元
- 成员函数做友元



##### 4.4.1 全局函数做友元

**示例：**

```c++
#include<iostream>
using namespace std;
#include<string>
class Building
{
	//全局函数ftd是Building类的友元，可以访问Building类中的私有成员
	friend void frd(Building* bulid);
public:
	Building()
	{
		m_LivingRoom = "客厅";
		m_BedRoom = "卧室";
	}
public:
	string m_LivingRoom;
private:
	string m_BedRoom;
};
void frd(Building *build)
{
	cout << "好朋友在访问：" << build->m_LivingRoom;
	cout << "好朋友在访问：" << build->m_BedRoom;	//若不用友元，则访问不到私有成员m_BedRoom
}
void test01()
{
	Building b;
	frd(&b);
}
int main()
{
	test01();
	system("pause");
	return 0;
}
```



##### 4.4.2 类做友元

```c++
#include<iostream>
using namespace std;
#include<string>
class Buliding;  //声明（重要，不然会报错）
class Friend
{
public:
	Friend();
	void visit(); //参观函数，访问Building中的属性
	Building* build;
};
class Building
{
	friend class Friend; //Friend类做为Building的友元，让其可以访问Building的私有成员
public:
	Building();
public:
	string m_LivingRoom;
private:
	string m_BedRoom;
};

//类外写成员函数：
Friend::Friend()	//在Friend类作用域下，它的构造函数
{
	//创建Building类
	build = new Building;	//使build指针指向new出来的内存
}
Building::Building()
{
	m_LivingRoom = "客厅";
	m_BedRoom = "卧室";
}
void Friend::visit()	//实现Friend作用域下的visit函数
{
	cout << "好朋友在访问：" << build->m_LivingRoom << endl;
	cout << "好朋友在访问：" << build->m_BedRoom << endl;
}

void test01()
{
	Friend s;
	s.visit();
}
int main()
{
	test01();
	system("pause");
	return 0;
}
```



##### 4.4.3 成员函数做友元

```c++
#include<iostream>
using namespace std;
#include<string>
class Building;	//声明
class Friend
{
public:
	Friend();
	void visit();	//让visit函数可以访问Building中的私有成员
	void visit2();	//让visit2不可访问
	Building* build;
};
class Building
{
	friend void Friend::visit();	//Friend作用域下的visit函数为Building类的友元，让visit可以访问到Building中的私有成员
public:
	Building();
public:
	string m_LivingRoom;
private:
	string m_BedRoom;
};

//类外写成员函数：
Friend::Friend()	//在Friend类作用域下，它的构造函数
{
	//创建Building类
	build = new Building;	//使build指针指向new出来的内存
}
Building::Building()
{
	m_LivingRoom = "客厅";
	m_BedRoom = "卧室";
}
void Friend::visit()
{
	cout << "好朋友在访问：" << build->m_LivingRoom << endl;
	cout << "好朋友在访问：" << build->m_BedRoom << endl;
}
void Friend::visit2()
{
	cout << "好朋友在访问：" << build->m_LivingRoom << endl;
	//cout << "好朋友在访问：" << build->m_BedRoom << endl;	报错，visit2无访问权限
}

void test01()
{
	Friend s;
	s.visit();
	s.visit2();
}
int main()
{
	test01();
	system("pause");
	return 0;
}
```



#### 4.5 运算符重载

运算符重载概念：对已有的运算符重新进行定义，赋予其另一种功能，以适应不同的数据类型

##### 4.5.1 加号运算符(+)重载

作用：实现两个自定义数据类型相加的运算

使用到的函数名：`operator+`,以此来简化调用时的代码。

**示例：**

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	int m_A;
	int m_B;
	//成员函数重载 +号 运算符
	/*Person operator+(Person& p)
	{
		Person temp;
		temp.m_A = this->m_A + p.m_A;
		temp.m_B = this->m_B + p.m_B;
		return temp;
	}*/
};

//全局函数重载＋号
Person operator+(Person& p1, Person& p2)
{
	Person temp;
	temp.m_A = p1.m_A + p2.m_A;
	temp.m_B = p2.m_B + p2.m_B;
	return temp;
}

void test01()
{
	Person p1;
	p1.m_A = 10;
	p1.m_B = 20;
	Person p2;
	p2.m_A = 10;
	p2.m_B = 20;
	Person p3;
	p3 = p1 + p2;
	cout << "p3.m_A=" << p3.m_A << endl;
	cout << "p3.m_B=" << p3.m_B << endl;
}
int main()
{
	test01();
	system("pause");
	return 0;
}
```

- 当发生成员函数重载时，`p3=p1+p2;`本质是`p3=p1+operator+(p2);`
- 当发生全局函数重载时，`p3=p1+p2;`本质是`p3=operator+(p1,p2);`
- 运算符重载也可以发生函数重载



##### 4.5.2 左移运算符(<<)重载(写在全局函数)

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	int m_A;
	int m_B;
	
};
ostream &operator<<(ostream& c, Person& p)	//ostream为标准输出流，cout的类型
{
	cout << "m_A=" << p.m_A << " m_B=" << p.m_B;
	return c;
}
void test01()
{
	Person p;
	p.m_A = 10;
	p.m_B = 20;
	cout << p << endl;		
	/*本质是：1. cout << p 相当于 operator<<(cout,p);
			  2. 返回cout后，cout << endl 相当于 operator << (cout, endl);（链式编程思想）
			  注：endl不是Person类型，则会调用默认的operator<<函数
	*/
}
int main()
{
	test01();
	system("pause");
	return 0;
}
```



##### 4.5.3 递增运算符(++)重载

作用：通过重载递增运算符，实现自己的整型数据

```c++
#include<iostream>
using namespace std;
class Myint
{
	friend ostream& operator<<(ostream& c, Myint myint);
private:
	int m_Num;
public:
	Myint()
	{
		m_Num = 0;
	}

	//重载前置++运算符(++i)
	Myint &operator++()		//返回引用是为了一直对一个数据进行递增操作
	{
		m_Num++;	//先进行++运算
		return *this;	//将自身做返回
	}

	//重载后置++运算符(i++) (不返回引用，temp是局部变量，函数运行完就释放了，不可返回，否则会报错)
	Myint operator++(int)	  //int代表占位参数，可以用于区分前置和后置递增(后置++用)
	{
		Myint temp = *this;		//先记录当前结果
		m_Num++;				//后递增
		return temp;			//再将记录结果做返回
	}
};
ostream &operator<<(ostream& c, Myint myint)	//ostream为标准输出流，cout的类型
{
	cout << myint.m_Num;
	return c;
}
void test01()
{
	Myint myint;
	cout << ++myint << endl;
}
int main()
{
	test01();
	system("pause");
	return 0;
}
```

若15行不返回引用，返回值的话，则

`cout<<++(++myint)<<endl;`	//第一个重载后返回的是一个新的变量，对新的变量再进行重载

`cout<<myint<<endl;`

输出为：

2

1

==返回引用则是一直对一个数据进行重载，不会创建新的对象==

重点：**前置递增返回引用，后置递增返回值**



##### 4.5.4 赋值运算符(=)重载

C++编译器至少给一个类添加4个函数

1. 默认构造函数
2. 默认析构函数
3. 默认拷贝函数，对属性进行值拷贝
4. 赋值运算符 operator= ，对属性进行拷贝

注：如果类中有属性指向堆区，做赋值操作时也会出现深浅拷贝问题

**示例：**

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	int *m_Age;
	Person(int age)
	{
		m_Age = new int(age);
	}
	Person &operator=(Person &p)	//因多重赋值时用到链式编程思想，所以用引用，返回本身
	{
		//m_Age=p.m_Age;		//编译器提供的是浅拷贝
		
		//应先判断是否有属性在堆区，有先释放干净，再深拷贝
		if (m_Age != NULL)
		{
			delete m_Age;
			m_Age = NULL;
		}
		m_Age = new int(*p.m_Age);
		return *this;
	}
};
void test01()
{
	Person p1(18);
	Person p2(20);
	Person p3(30);
	p3 = p2 = p1;	//链式编程思想
	cout << *p1.m_Age << endl;
	cout << *p2.m_Age << endl;
	cout << *p3.m_Age << endl;
}
int main()
{
	test01();
	return 0;
}
```



##### 4.5.5 关系运算符(==/!=)重载

作用：重载关系运算符，可以让两个自定义类型的对象进行比对操作

**示例：**

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	string m_Name;
	int m_Age;
	Person(string name, int age)
	{
		m_Name = name;
		m_Age = age;
	}
	bool operator==(Person &p)
	{
		if (this->m_Name == p.m_Name && this->m_Age == p.m_Age)
			return true;
		return false;
	}
	
};
void test01()
{
	Person p1("Tom", 18);
	Person p2("Tom", 18);
	if (p1 == p2)
	{
		cout << "p1和p2相等" << endl;
	}
}
int main()
{
	test01();
	return 0;
}
```



##### 4.5.6 函数调用运算符(())重载

- 函数调用运算符()也可以重载
- 由于重载后使用的方式非常像函数的调用，因此称为仿函数
- 仿函数没有固定的写法，非常灵活

**示例：**

```c++
#include<iostream>
using namespace std;
#include<string>
class Myprint
{
public:
	
	void operator()(string test)
	{
		cout << test << endl;
	}
	int operator()(int a, int b)
	{
		return a + b;
	}
};
void test01()
{
	Myprint print;
	print.operator()("hello world");
}
void test02()
{
	Myprint print;
	cout << "两数相加结果为：" << print.operator()(1, 2) << endl;

	//匿名函数对象
	cout << Myprint()(1, 2) << endl;	//匿名对象，执行完本行后立即被释放
}
int main()
{
	test01();
	test02();
	return 0;
}
```



#### 4.6 继承

**继承是面向对象的三大特性之一**

定义一些类时，下级别的成员除了拥有上一级的共性，还有自己的特性。

这个时候就可以考虑用继承技术，减少重复的代码



##### 4.6.1 继承的基本语法

```c++
class A
{
  ....  
};
class B:public A
{
    ....
};
```

A类称为父类，或基类

B类称为子类，或派生类

**派生类中的成员，包含两大部分：**

一类是从基类继承过来的，一类是自己增加的成员

从基类继承过来的表现其共性，而新增的成员体现了其个性



##### 4.6.2 继承方式

语法：`class 子类 ： 继承方式  父类`

继承有三种继承方式：

- 公共继承	---除了私有权限，子类继承后的权限与父类一致

- 保护继承    ---除了私有权限，子类中继承后的权限全为protected

- 私有继承    ---除了私有权限，子类中继承后的权限全为private

  三种继承方式都无法访问到父类中的私有成员，但都继承了(只是没有访问权限)

具体继承方式如下：

```c++
class A
{
public:
    int a;
protected:
    int b;
private:
    int c;
};
```

```c++
公共继承：
class B:public A
{
public:
    int a;
protected:
    int b;
不可以访问：
    int c;
};
```

```c++
保护继承：
class B:public A
{
protected:
    int a;
    int b;
不可以访问：
    int c;
};
```

```c++
私有继承：
class B:public A
{
private:
    int a;
    int b;
不可以访问：
    int c;
}
```



##### 4.6.3 继承中的对象模型

子类继承父类后，父类的私有成员也被子类继承了，只是访问不到。



##### 4.6.4 继承中构造和析构顺序

子类继承父类后，当创建子类对象，也会调用父类的构造函数

**构造函数顺序：**

​				先父类，再子类

**析构函数顺序：**

​				先释放子类，后释放父类



##### 4.6.5 继承同名成员的处理方式

当子类与父类出现同名成员时：

- 访问子类同名成员   直接访问即可
- 访问父类同名成员   需要加作用域

- 子类与父类拥有同名的成员函数，加作用域可以访问父类中的同名函数



##### 4.6.6 继承同名静态成员处理方式

继承中同名静态成员处理方式与前面一致

**静态成员的访问方式有两种：**

- 通过对象访问

  ```c++
  class Base
  {
  public:
      int m_A;
  };
  class Son : public Base
  {
      int m_A;
  };
  Son s;			//创建一个Son的对象
  s.m_A;				//Son 下的 m_A
  s.Base::m_A;		//Base 下的 m_A
  ```

- 通过类名访问

  ```c++
  class Base
  {
  public:
      int m_A;
  };
  class Son : public Base
  {
      int m_A;
  };
  Son::m_A;			//Son 下的 m_A
  Son::Base::m_A;		//Base 下的 m_A
  //第一个::代表通过类名方式访问，第二个::代表访问父类作用域下的成员
  ```



##### 4.6.7 多继承语法

C++允许一个类继承多个类

语法：`class 子类  :  继承方式  父类1 , 继承方式  父类2`

多继承可能会引发父类中有同名成员函数出现，需要加作用域区分



C++在实际开发中不建议用多继承



##### 4.6.8 菱形继承

**菱形继承概念：**

​		两个派生类继承同一个基类

​		又有某个类同时继承这两个派生类

这种继承被称为菱形继承，或者钻石继承



菱形继承有一个最大的问题，就是当两个派生类有相同的数据时，菱形底部的类到底是继承这两个派生类中的哪一个，为了解决这个问题，需要利用**虚继承**。

在这两个派生类中加关键字：**virtual**

菱形顶部的类叫虚基类



示例：

​		定义一个Animal类，该类下有羊和驼两个子类，羊驼又为羊和驼的子类，此为一个菱形继承关系。

在羊类中定义一个m_Age，驼类中定义一个m_Age，Animal中也定义了一个m_Age。



其底层原理如下图：

![](D:\Snipaste截图\Snipaste_2021-10-29_21-19-28.png)



#### 4.7 多态

##### 4.7.1 多态的基本语法(虚函数)

多态是C++面向对象三大特性之一

多态分为两类：

- 静态多态：函数重载和运算符重载属于静态多态，复用函数名
- 动态多态：派生类和虚函数实现运行时的多态

静态多态和动态多态的区别：

- 静态多态的函数地址早绑定 --- 编译阶段确定函数地址
- 动态多态的函数地址晚绑定 --- 运行阶段确定函数地址

**示例：**

```c++
#include<iostream>
using namespace std;
class Animal
{
public:
	void speak()
	{
		cout << "动物在说话" << endl;
	}
};
class Cat :public Animal
{
public:
	void speak()
	{
		cout << "猫在说话" << endl;
	}
};	
void doSpeak(Animal& animal)	//执行说话的函数
{
	animal.speak();		
}
void test01()
{
	Cat cat;
	doSpeak(cat);	//C++中允许父子之间的类型转换(能从Cat类型传到Animal类型)
}
int main()
{
	test01();
	return 0;
}
//输出：动物在说话
//因为这是静态多态，地址早绑定，在编译阶段就已经确定了函数地址
```

如果想执行猫在说话，则需要地址晚绑定，让父类Animal地址晚绑定，利用虚函数实现

```c++
class Animal
{
public:
	virtual void speak()  //加关键字virtual，让Animal地址晚绑定（子类可加可不加virtual）
	{
		cout << "动物在说话" << endl;
	}
};
```

修改后的代码：

```c++
#include<iostream>
using namespace std;
class Animal
{
public:
	virtual void speak()  //加关键字virtual，让Animal地址晚绑定
	{
		cout << "动物在说话" << endl;
	}
};
class Cat :public Animal
{
public:
	void speak()
	{
		cout << "猫在说话" << endl;
	}
};	
class Dog :public Animal
{
public:
	void speak()
	{
		cout << "狗在说话" << endl;
	}
};
void doSpeak(Animal& animal)	//执行说话的函数
{
	animal.speak();		
}
void test01()
{
	Cat cat;
	doSpeak(cat);	//C++中允许父子之间的类型转换(能从Cat类型传到Animal类型)

	Dog dog;
	doSpeak(dog);
}
int main()
{
	test01();
	return 0;
}
//输出： 猫在说话
// 狗在说话
```

**动态多态满足条件：**

1. 有继承关系
2. 子类重写父类虚函数（返回类型，函数名，参数列表都一致）

**动态多态的使用：**

父类的指针或引用，指向子类对象      `void doSpeak(Animal& animal)`     (子类Cat或Dog传给animal)



##### 4.7.2 多态的底层原理

具体原理如下图：

<img src="D:\Snipaste截图\Snipaste_2021-11-11_21-48-39.png"  />



利用VS的开发人员命令提示符查看：

当父类中有虚函数：（父类中含有一个vfptr，占4字节，若父类中无虚函数，则此类只占1字节(占位字节)）

![](D:\Snipaste截图\Snipaste_2021-11-11_21-48-53.png)

当子类不重写父类虚函数时，子类的类图：

![](D:\Snipaste截图\Snipaste_2021-11-11_21-49-04.png)

子类重写父类虚函数时，子类的类图：

![](D:\Snipaste截图\Snipaste_2021-11-11_21-49-16.png)

##### 4.7.3 多态案例1 --- 计算器类

多态的优点：

- 代码组织结构清晰
- 可读性强
- 利于前期和后期的扩展和维护

**示例：**

```c++
#include<iostream>
using namespace std;
#include<string>
class Base		//定义一个基类，提供虚函数，让子类利用多态
{
public:
	float m_Num1, m_Num2;
	virtual int getResult()		
	{
		return 0;
	}
};
class Add :public Base
{
public:
	int getResult()
	{
		return m_Num1 + m_Num2;
	}
};
class Sub :public Base
{
public:
	int getResult()
	{
		return m_Num1 - m_Num2;
	}
};
class Mul :public Base
{
public:
	int getResult()
	{
		return m_Num1 * m_Num2;
	}
};
class Div :public Base
{
public:
	int getResult()
	{
		return m_Num1 / m_Num2;
	}
};
void test01()
{
	//加法：
	Base* p;		//定义一个父类的指针
	p = new Add;	//new一个Add的对象，然后将指针指向子类对象 (多态的使用)
	p->m_Num1 = 10;
	p->m_Num2 = 5;
	cout << p->m_Num1 << "+" << p->m_Num2 << "=" << p->getResult() << endl;
	delete p;	//new出来的对象存放在堆区，使用完释放

	//减法：
	p = new Sub;	//不用重新定义父类指针，释放内存后不会销毁指针
	p->m_Num1 = 12;
	p->m_Num2 = 2;
	cout << p->m_Num1 << "-" << p->m_Num2 << "=" << p->getResult() << endl;
	delete p;
}
int main()
{
	test01();
	return 0;
}
```

利用多态后：

- 需要增添计算器功能时，直接加一个子类即可，不用在源代码上加。
- 若某个功能出现问题，则可快速定位到该类，不用修改源代码。



##### 4.7.4 纯虚函数和抽象类

在多态中，通常父类中虚函数的实现是毫无意义的，主要都是调用子类重写的内容

因此可以将虚函数改为纯虚函数

纯虚函数的语法：`virtual 返回值类型 函数名 (参数列表) = 0;`

当类中有了纯虚函数，这个类也称为抽象类

**抽象类的特点：**

- 无法实例化对象(无法定义该类的对象)
- 子类必须重写抽象类中的纯虚函数，否则也属于抽象类，也无法实例化对象

**示例：**将上面例子的

```c++
virtual int getResult()		
	{
		return 0;
	}
```

改为

```c++
virtual int getResult()=0;
```

即可





##### 4.7.5 多态案例2 --- 制作饮品

```c++
#include<iostream>
using namespace std;
class BaseDrinking
{
public:
	virtual void Boil() = 0;			//煮水
	virtual void Brew() = 0;			//冲泡
	virtual void PutInCup() = 0;		//倒入杯中
	virtual void PutSomething() = 0;	//加入辅料

	void makeDrink()					//用一个函数来调动所有的工序
	{
		Boil();
		Brew();
		PutInCup();
		PutSomething();
	}
};
class Coffee :public BaseDrinking	//制作咖啡
{
public:
	void Boil()
	{
		cout << "将水煮开" << endl;
	}
	void Brew()
	{
		cout << "冲泡咖啡" << endl;
	}
	void PutInCup()
	{
		cout << "倒入杯中" << endl;
	}
	void PutSomething()
	{
		cout << "加入糖和牛奶" << endl;
	}
};
class Tea :public BaseDrinking		//制作茶
{
public:
	void Boil()
	{
		cout << "将水煮开" << endl;
	}
	void Brew()
	{
		cout << "冲泡茶叶" << endl;
	}
	void PutInCup()
	{
		cout << "倒入杯中" << endl;
	}
	void PutSomething()
	{
		cout << "加入其他辅料" << endl;
	}
};

//若要添加其他饮品，则只需直接添加子类即可

void doWork(BaseDrinking* p)	//写一个制作函数，形参为父类指针，这样传进来的是什么子类就对应做什么饮品
{
	p->makeDrink();
	delete p;
}

void test01()
{
	doWork(new Coffee);			//new一个子类对象，传给父类指针
	cout << "-------------" << endl;
	doWork(new Tea);
}

int main()
{
	test01();
	return 0;
}
```

输出：

```c++
将水煮开
冲泡咖啡
倒入杯中
加入糖和牛奶
-------------
将水煮开
冲泡茶叶
倒入杯中
加入其他辅料
```



**优点：**通过传入不同的对象，转接到同一个接口(doWork 函数)，来实现不同的制作工序



##### 4.7.6 虚析构和纯虚析构

多态使用时，如果**子类中有属性开辟到堆区**，那么父类指针在释放时无法调用到子类的析构代码，导致子类如果有堆区属性，则会有内存泄漏的可能。

解决方式：将父类中的析构函数改为虚析构或者纯虚析构



虚析构和纯虚析构共性：

- 可以解决父类指针释放子类对象
- 都需要有具体的函数实现

虚析构和纯虚析构区别：

- 如果是纯虚析构，该类属于抽象类，无法实例化对象



虚析构语法：

`virtual ~类名(){}`

纯析构语法：

`virtual ~类名()=0;`

`父类类名::~类名(){}`

纯虚析构需要在类外具体实现



##### 4.7.7 多态案例3 --- 电脑组装

案例描述：

电脑主要组成部件为CPU（用于计算），显卡（用于显示），内存条（用于存储）

将每个零件封装出抽象基类，并且提供不同的厂商生产不同的零件，例如Intel和Lenovo

创建电脑类提供让电脑工作的函数，并且调用每个零件工作的接口

测试时组装三台不同的电脑进行工作



代码：

```c++
#include<iostream>
using namespace std;

//抽象CPU类
class CPU
{
public:
	//抽象的计算函数
	virtual void calculate() = 0;
};

//抽象显卡类
class VideoCard
{
public:
	//抽象的显示函数
	virtual void display() = 0;
};

//抽象内存条类
class Memory
{
public:
	//抽象的存储函数
	virtual void storage() = 0;
};

//电脑类
class Computer
{
public:
	Computer(CPU* cpu, VideoCard* vc, Memory* mem)
	{
		m_cpu = cpu;
		m_vc = vc;
		m_mem = mem;
	}

	//提供工作的函数
	void work()
	{
		//让零件工作起来，调用接口
		m_cpu->calculate();

		m_vc->display();

		m_mem->storage();
	}

	//提供析构函数 释放3个电脑零件
	~Computer()
	{

		//释放CPU零件
		if (m_cpu != NULL)
		{
			delete m_cpu;
			m_cpu = NULL;
		}

		//释放显卡零件
		if (m_vc != NULL)
		{
			delete m_vc;
			m_vc = NULL;
		}

		//释放内存条零件
		if (m_mem != NULL)
		{
			delete m_mem;
			m_mem = NULL;
		}
	}

private:

	CPU* m_cpu; //CPU的零件指针
	VideoCard* m_vc; //显卡零件指针
	Memory* m_mem; //内存条零件指针
};

//具体厂商
//Intel厂商
class IntelCPU :public CPU
{
public:
	virtual void calculate()
	{
		cout << "Intel的CPU开始计算了！" << endl;
	}
};

class IntelVideoCard :public VideoCard
{
public:
	virtual void display()
	{
		cout << "Intel的显卡开始显示了！" << endl;
	}
};

class IntelMemory :public Memory
{
public:
	virtual void storage()
	{
		cout << "Intel的内存条开始存储了！" << endl;
	}
};

//Lenovo厂商
class LenovoCPU :public CPU
{
public:
	virtual void calculate()
	{
		cout << "Lenovo的CPU开始计算了！" << endl;
	}
};

class LenovoVideoCard :public VideoCard
{
public:
	virtual void display()
	{
		cout << "Lenovo的显卡开始显示了！" << endl;
	}
};

class LenovoMemory :public Memory
{
public:
	virtual void storage()
	{
		cout << "Lenovo的内存条开始存储了！" << endl;
	}
};


void test01()
{
	//第一台电脑零件
	CPU* intelCpu = new IntelCPU;
	VideoCard* intelCard = new IntelVideoCard;
	Memory* intelMem = new IntelMemory;

	cout << "第一台电脑开始工作：" << endl;
	//创建第一台电脑
	Computer* computer1 = new Computer(intelCpu, intelCard, intelMem);
	computer1->work();
	delete computer1;

	cout << "-----------------------" << endl;
	cout << "第二台电脑开始工作：" << endl;
	//第二台电脑组装
	Computer* computer2 = new Computer(new LenovoCPU, new LenovoVideoCard, new LenovoMemory);;
	computer2->work();
	delete computer2;

	cout << "-----------------------" << endl;
	cout << "第三台电脑开始工作：" << endl;
	//第三台电脑组装
	Computer* computer3 = new Computer(new LenovoCPU, new IntelVideoCard, new LenovoMemory);;
	computer3->work();
	delete computer3;

}
int main()
{
	test01();
	return 0;
}
```

输出：

```c++
第一台电脑开始工作：
Intel的CPU开始计算了！
Intel的显卡开始显示了！
Intel的内存条开始存储了！
-----------------------
第二台电脑开始工作：
Lenovo的CPU开始计算了！
Lenovo的显卡开始显示了！
Lenovo的内存条开始存储了！
-----------------------
第三台电脑开始工作：
Lenovo的CPU开始计算了！
Intel的显卡开始显示了！
Lenovo的内存条开始存储了！
```





### 5 文件操作

程序运行时产生的数据都属于临时数据，程序一旦运行结束都会被释放

通过文件可以将数据持久化

C++中对文件操作需要包含头文件`#include<fstream>`



文件类型分为两种：

- 文本文件 --- 文件以文本的ASCII码形式存储在计算机中
- 二进制文件 --- 文件以文本的二进制形式存储在计算机中，用户一般不能直接读懂他们



操作文件的三大类：

1. ofstream：写操作
2. ifstream：读操作
3. fstream：读写操作





#### 5.1 文本文件

##### 5.1.1 写文件

写文件步骤如下：

1. 包含头文件  ---  `#include<fstream>`
2. 创建流对象  ---  `ofstream ofs;`
3. 打开文件  ---  `ofs.open("文件路径",打开方式)`
4. 写数据  ---  `ofs<<"写入的数据"`
5. 关闭文件  ---  `ofs.close();`

文件的打开方式：

| 打开方式    |            解释            |
| :---------- | :------------------------: |
| ios::in     |     为读文件而打开文件     |
| ios::out    |     为写文件而打开文件     |
| ios::ate    |      初始位置：文件尾      |
| ios::app    |       追加方式写文件       |
| ios::trunc  | 如果文件存在先删除，再创建 |
| ios::binary |         二进制方式         |

注意：文件打开方式可以配合使用，利用    |   操作符

例如：用二进制方式写文件：`ios::binary | ios::out`

示例：

```c++
#include<iostream>
using namespace std;
#include<fstream>
void test01()
{
	ofstream ofs;
	ofs.open("test.txt", ios::out);		//若不写文件路径，只提供文件名，则会在此项目的目录下创建该文件
	ofs << "姓名：张三" << endl;
	ofs << "年龄：18" << endl;
	ofs << "性别：男" << endl;
	ofs.close();
}
int main()
{
	test01();
	return 0;
}

```



##### 5.1.2 读文件

读文件与写文件步骤类似，但是读取方式比较多



读文件的步骤如下：

1. 包含头文件  ---  `#include<fstream>`
2. 创建流对象  ---  `ifstream ifs;`
3. 打开文件并判断文件是否打开成功  ---  `ifs.open("文件路径",打开方式);`   (利用is_open()函数判断)
4. 读数据   ---  四种方式读取
5. 关闭文件  ---  `ifs.close();`

读数据的四种方式：

1. ```c++
   char buf[1024]={0};
   while(ifs>>buf)
   {
       cout<<buf<<endl;
   }
   ```

2. ```c++
   char arr[1024]={0};
   while(ifs.getline(buf,sizeof(buf)))
   {
       cout<<buf<<endl;
   }
   ```

3. ```c++
   string buf;
   while(getline(ifs,buf))
   {
       cout<<buf<<endl;
   }
   ```

4. ```c++
   char c;
   while((c=ifs.get())!=EOF)	//EOF:end of file
   {
       cout<<c;
   }
   //此方式为一个一个字符的读，不推荐使用
   ```

   

示例：(把刚刚写的文件读出来)

```c++
#include<iostream>
using namespace std;
#include<fstream>
#include<string>
void test01()
{
	ifstream ifs;
	ifs.open("test.txt", ios::in);
	if (!ifs.is_open())			//判断文件是否打开成功(is_open()的返回值为布尔类型)
	{
		cout << "文件打开失败" << endl;
		return;
	}
	string buf;
	while (getline(ifs, buf))
	{
		cout << buf << endl;
	}
	ifs.close();
}
int main()
{
	test01();
	return 0;
}

```

输出：

姓名：张三
年龄：18
性别：男



#### 5.2 二进制文件

以二进制的方式对文件进行读写操作

打开方式要指定为： **ios::binary**



##### 5.2.1 写文件

二进制方式写文件主要利用流对象调用成员函数write

函数原型：`ostream& write(const char* buffer,int len);`

参数解释：字符指针buffer指向内存中一段存储空间。len是读写的字节数

示例：

```c++
#include<iostream>
using namespace std;
#include<fstream>
#include<string>
class Person
{
public:
	char m_Name[64];
	int m_Age;
};
void test01()
{
	ofstream ofs("Person.txt", ios::out | ios::binary);	//可将一二步合并
	Person p = { "张三",18 };
	ofs.write((const char*)&p, sizeof(Person));		//将Person类型强转为const char* 类型，&p为引用的方式
	ofs.close();
}
int main()
{
	test01();
	return 0;
}
```



##### 5.2.2 读文件

二进制方式写文件主要利用流对象调用成员函数read

函数原型：`istream& read(char* buffer,int len);`

参数解释：字符指针buffer指向内存中一段存储空间。len是读写的字节数

示例：(将刚刚写的文件读出来)

```c++
#include<iostream>
using namespace std;
#include<fstream>
#include<string>
class Person
{
public:
	char m_Name[64];
	int m_Age;
};
void test01()
{
	ifstream ifs;
	ifs.open("Person.txt", ios::in | ios::binary);
	if (!ifs.is_open())
	{
		cout << "文件打开失败" << endl;
		return;
	}
	Person p1;
	ifs.read((char*)&p1, sizeof(Person));
	cout << "姓名：" << p1.m_Name << "  年龄：" << p1.m_Age << endl;
	ifs.close();
}
int main()
{
	test01();
	return 0;
}
```

输出：

姓名：张三  年龄：18







# C++提高编程

* 本阶段主要针对C++==泛型编程==和==STL==技术做详细讲解，探讨C++更深层的使用

## 1、模板

### 1.1 模板的概念

模板的特点：

* 模板不可以直接使用，它只是一个框架
* 模板的通用并不是万能的



### 1.2 函数模板

* C++另一种编程思想称为 ==泛型编程== ，主要利用的技术就是模板


* C++提供两种模板机制:**函数模板**和**类模板** 



#### 1.2.1 函数模板语法

函数模板作用：

建立一个通用函数，其函数返回值类型和形参类型可以不具体制定，用一个**虚拟的类型**来代表。

**语法：** 

```C++
template<typename T>
函数声明或定义
```

**解释：**

template  ---  声明创建模板

typename  --- 表面其后面的符号是一种数据类型，可以用class代替

T    ---   通用的数据类型，名称可以替换，通常为大写字母

**示例：**

```c++
//利用模板提供通用的交换函数
template<typename T>
void mySwap(T& a, T& b)
{
	T temp = a;
	a = b;
	b = temp;
}
```

总结：

* 函数模板利用关键字 template
* 使用函数模板有两种方式：自动类型推导、显示指定类型
* 模板的目的是为了提高复用性，将类型参数化



#### 1.2.2 函数模板注意事项

注意事项：

* 自动类型推导，必须推导出一致的数据类型T,才可以使用


* 模板必须要确定出T的数据类型，才可以使用



#### 1.2.3 函数模板案例

案例描述：

* 利用函数模板封装一个排序的函数，可以对**不同数据类型数组**进行排序
* 排序规则从大到小，排序算法为**选择排序**
* 分别利用**char数组**和**int数组**进行测试

**示例：**

```c++
//交换的函数模板
template<typename T>
void mySwap(T &a, T&b)
{
	T temp = a;
	a = b;
	b = temp;
}


template<class T> // 也可以替换成typename
//利用选择排序，进行对数组从大到小的排序
void mySort(T arr[], int len)
{
	for (int i = 0; i < len; i++)
	{
		int max = i; //最大数的下标
		for (int j = i + 1; j < len; j++)
		{
			if (arr[max] < arr[j])
			{
				max = j;
			}
		}
		if (max != i) //如果最大数的下标不是i，交换两者
		{
			mySwap(arr[max], arr[i]);
		}
	}
}
template<typename T>
void printArray(T arr[], int len) {

	for (int i = 0; i < len; i++) {
		cout << arr[i] << " ";
	}
	cout << endl;
}
void test01()
{
	//测试char数组
	char charArr[] = "bdcfeagh";
	int num = sizeof(charArr) / sizeof(char);
	mySort(charArr, num);
	printArray(charArr, num);
}

void test02()
{
	//测试int数组
	int intArr[] = { 7, 5, 8, 1, 3, 9, 2, 4, 6 };
	int num = sizeof(intArr) / sizeof(int);
	mySort(intArr, num);
	printArray(intArr, num);
}

int main() {

	test01();
	test02();

	system("pause");

	return 0;
}
```



#### 1.2.4 普通函数与函数模板的区别

**普通函数与函数模板区别：**

* 普通函数调用时可以发生自动类型转换（隐式类型转换）
* 函数模板调用时，如果利用自动类型推导，不会发生隐式类型转换
* 如果利用显示指定类型的方式，可以发生隐式类型转换

**示例：**

```C++
//普通函数
int myAdd01(int a, int b)
{
	return a + b;
}

//函数模板
template<class T>
T myAdd02(T a, T b)  
{
	return a + b;
}

//使用函数模板时，如果用自动类型推导，不会发生自动类型转换,即隐式类型转换
void test01()
{
	int a = 10;
	int b = 20;
	char c = 'c';
	
	cout << myAdd01(a, c) << endl; //正确，将char类型的'c'隐式转换为int类型  'c' 对应 ASCII码 99

	//myAdd02(a, c); // 报错，使用自动类型推导时，不会发生隐式类型转换

	myAdd02<int>(a, c); //正确，如果用显示指定类型，可以发生隐式类型转换
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：建议使用显示指定类型的方式，调用函数模板，因为可以自己确定通用类型T



#### 1.2.5 普通函数与函数模板的调用规则

调用规则如下：

1. 如果函数模板和普通函数都可以实现，优先调用普通函数
2. 可以通过空模板参数列表来强制调用函数模板
3. 函数模板也可以发生重载
4. 如果函数模板可以产生更好的匹配,优先调用函数模板

**示例：**

```C++
//普通函数与函数模板调用规则
void myPrint(int a, int b)
{
	cout << "调用的普通函数" << endl;
}

template<typename T>
void myPrint(T a, T b) 
{ 
	cout << "调用的模板" << endl;
}

template<typename T>
void myPrint(T a, T b, T c) 
{ 
	cout << "调用重载的模板" << endl; 
}

void test01()
{
	//1、如果函数模板和普通函数都可以实现，优先调用普通函数
	// 注意 如果告诉编译器  普通函数是有的，但只是声明没有实现，或者不在当前文件内实现，就会报错找不到
	int a = 10;
	int b = 20;
	myPrint(a, b); //调用普通函数

	//2、可以通过空模板参数列表来强制调用函数模板
	myPrint<>(a, b); //调用函数模板

	//3、函数模板也可以发生重载
	int c = 30;
	myPrint(a, b, c); //调用重载的函数模板

	//4、 如果函数模板可以产生更好的匹配,优先调用函数模板
	char c1 = 'a';
	char c2 = 'b';
	myPrint(c1, c2); //调用函数模板
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：既然提供了函数模板，最好就不要提供普通函数，否则容易出现二义性



#### 1.2.6 模板的局限性

**局限性：**

* 模板的通用性并不是万能的

**例如：**

```C++
	template<class T>
	void f(T a, T b)
	{ 
    	a = b;
    }
```

在上述代码中提供的赋值操作，如果传入的a和b是一个数组，就无法实现了



再例如：

```C++
	template<class T>
	void f(T a, T b)
	{ 
    	if(a > b) { ... }
    }
```

在上述代码中，如果T的数据类型传入的是像Person这样的自定义数据类型，也无法正常运行



因此C++为了解决这种问题，提供模板的重载，可以为这些**特定的类型**提供**具体化的模板**

类的具体化模板的语法为：`template<> 返回类型 函数名(类名 &p1, 类名 &p2)`

**示例：**

```C++
#include<iostream>
using namespace std;
#include <string>

class Person
{
public:
	Person(string name, int age)
	{
		this->m_Name = name;
		this->m_Age = age;
	}
	string m_Name;
	int m_Age;
};

//普通函数模板
template<class T>
bool myCompare(T& a, T& b)
{
	if (a == b)
	{
		return true;
	}
	else
	{
		return false;
	}
}


//具体化，显示具体化的原型和定义以template<>开头，并通过名称来指出类型
//具体化优先于常规模板
template<> bool myCompare(Person &p1, Person &p2)			//j
{
	if ( p1.m_Name  == p2.m_Name && p1.m_Age == p2.m_Age)
	{
		return true;
	}
	else
	{
		return false;
	}
}

void test01()
{
	int a = 10;
	int b = 20;
	//内置数据类型可以直接使用通用的函数模板
	bool ret = myCompare(a, b);
	if (ret)
	{
		cout << "a == b " << endl;
	}
	else
	{
		cout << "a != b " << endl;
	}
}

void test02()
{
	Person p1("Tom", 10);
	Person p2("Tom", 10);
	//自定义数据类型，不会调用普通的函数模板
	//可以创建具体化的Person数据类型的模板，用于特殊处理这个类型
	bool ret = myCompare(p1, p2);
	if (ret)
	{
		cout << "p1 == p2 " << endl;
	}
	else
	{
		cout << "p1 != p2 " << endl;
	}
}

int main() {

	test01();

	test02();

	system("pause");

	return 0;
}
```

总结：

* 利用具体化的模板，可以解决自定义类型的通用化
* 学习模板并不是为了写模板，而是在STL能够运用系统提供的模板



### 1.3 类模板

#### 1.3.1 类模板语法

类模板作用：

* 建立一个通用类，类中的成员 数据类型可以不具体制定，用一个**虚拟的类型**来代表。

**语法：** 

```c++
template<typename T>
类
```

**解释：**

template  ---  声明创建模板

typename  --- 表面其后面的符号是一种数据类型，可以用class代替

T    ---   通用的数据类型，名称可以替换，通常为大写字母



**示例：**

```C++
#include <string>
//类模板
template<class NameType, class AgeType> 
class Person
{
public:
	Person(NameType name, AgeType age)
	{
		this->mName = name;
		this->mAge = age;
	}
	void showPerson()
	{
		cout << "name: " << this->mName << " age: " << this->mAge << endl;
	}
public:
	NameType mName;
	AgeType mAge;
};

void test01()
{
	// 指定NameType 为string类型，AgeType 为 int类型
	Person<string, int>P1("孙悟空", 999);
	P1.showPerson();
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：类模板和函数模板语法相似，在声明模板template后面加类，此类称为类模板



#### 1.3.2 类模板与函数模板区别



类模板与函数模板区别主要有两点：

1. 类模板没有自动类型推导的使用方式
2. 类模板在模板参数列表中可以有默认参数




**示例：**

```C++
#include <string>
//类模板
template<class NameType, class AgeType = int> 
class Person
{
public:
	Person(NameType name, AgeType age)
	{
		this->mName = name;
		this->mAge = age;
	}
	void showPerson()
	{
		cout << "name: " << this->mName << " age: " << this->mAge << endl;
	}
public:
	NameType mName;
	AgeType mAge;
};

//1、类模板没有自动类型推导的使用方式
void test01()
{
	// Person p("孙悟空", 1000); // 错误 类模板使用时候，不可以用自动类型推导
	Person <string ,int>p("孙悟空", 1000); //必须使用显示指定类型的方式，使用类模板
	p.showPerson();
}

//2、类模板在模板参数列表中可以有默认参数
void test02()
{
	Person <string> p("猪八戒", 999); //类模板中的模板参数列表 可以指定默认参数
	p.showPerson();
}

int main() {

	test01();

	test02();

	system("pause");

	return 0;
}
```

总结：

* 类模板使用只能用显示指定类型方式
* 类模板中的模板参数列表可以有默认参数



**附加**

在C++11标准中，函数模板也支持参数列表可以有默认参数，但是在语法上，类模板在为多个默认模板参数声明指定默认值时，必须遵循 “ **从右往左** ” 的规则进行指定。

**示例：**

```c++
//类模板：
template <typename T1, typename T2 = int>
class DefClass1 {};
  
template <typename T1 = int, typename T2>
class DefClass2 {};  // ERROR: 无法通过编译：因为模板参数的默认值没有遵循“由右往左”的规则

template <typename T, int i = 0>
class DefClass3 {};

template <int i = 0, typename T>
class DefClass4 {};  // ERROR: 无法通过编译：因为模板参数的默认值没有遵循“由右往左”的规则

//函数模板：
template <typename T1 = int, typename T2>
void DefFunc1(T1 a, T2 b) {}; // OK 函数模板不用遵循“由右往左”的规则

template <int i = 0, typename T>
void DefFunc2(T a) {};  // OK 函数模板不用遵循“由右往左”的规则
```



函数模板的参数推导规则也并不复杂。简单地讲：**如果能够从函数实参中推导出类型的话，那么默认模板参数就不会被使用，反之，默认模板参数则可能会被使用**。

**示例：**

```c++
template <class T, class U = double>
void f(T t = 0, U u = 0) {};
void g()
{
    f(1, 'c'); // f<int, char>(1, 'c')
    f(1);      // f<int, double>(1, 0), 使用了默认模板参数double
    f();       // 错误: T无法被推导出来
    f<int>();  // f<int, double>(0, 0), 使用了默认模板参数double
    f<int, char>(); // f<int, char>(0, 0)
}
```



#### 1.3.3 类模板中成员函数创建时机

类模板中成员函数和普通类中成员函数创建时机是有区别的：

* 普通类中的成员函数一开始就可以创建
* 类模板中的成员函数在调用时才创建



#### 1.3.4 类模板对象做函数参数

学习目标：

* 类模板实例化出的对象，向函数传参的方式

一共有三种传入方式：

1. 指定传入的类型   --- 直接显示对象的数据类型
2. 参数模板化           --- 将对象中的参数变为模板进行传递
3. 整个类模板化       --- 将这个对象类型 模板化进行传递



**示例：**

```C++
#include<iostream>
using namespace std;
#include <string>
//类模板
template<class NameType, class AgeType = int> 
class Person
{
public:
	Person(NameType name, AgeType age)
	{
		this->mName = name;
		this->mAge = age;
	}
	void showPerson()
	{
		cout << "name: " << this->mName << " age: " << this->mAge << endl;
	}
public:
	NameType mName;
	AgeType mAge;
};

//1、指定传入的类型
void printPerson1(Person<string, int> &p) 
{
	p.showPerson();
}
void test01()
{
	Person <string, int >p("孙悟空", 100);
	printPerson1(p);
}

//2、参数模板化
template <class T1, class T2>
void printPerson2(Person<T1, T2>&p)
{
	p.showPerson();
    //利用typeid(变量名).name()查看该变量的类型
	cout << "T1的类型为： " << typeid(T1).name() << endl;
	cout << "T2的类型为： " << typeid(T2).name() << endl;
}
void test02()
{
	Person <string, int >p("猪八戒", 90);
	printPerson2(p);
}

//3、整个类模板化
template<class T>
void printPerson3(T & p)
{
	cout << "T的类型为： " << typeid(T).name() << endl;
	p.showPerson();

}
void test03()
{
	Person <string, int >p("唐僧", 30);
	printPerson3(p);
}

int main() {

	test01();
	test02();
	test03();

	system("pause");

	return 0;
}
```

总结：

* 通过类模板创建的对象，可以有三种方式向函数中进行传参
* 使用比较广泛是第一种：指定传入的类型



#### 1.3.5 类模板与继承

当类模板碰到继承时，需要注意一下几点：

* 当子类继承的父类是一个类模板时，子类在声明的时候，要指定出父类中T的类型
* 如果不指定，编译器无法给子类分配内存
* 如果想灵活指定出父类中T的类型，子类也需变为类模板

**示例：**

```c++
#include<iostream>
using namespace std;
#include<string>

template<class T>
class Base
{
	T m;
};

//class Son:public Base  //错误，c++编译需要给子类分配内存，必须知道父类中T的类型才可以向下继承
class Son :public Base<int> //必须指定一个类型
{
};
void test01()
{
	Son c;
}

//类模板继承类模板 ,可以用T2指定父类中的T类型
template<class T1, class T2>
class Son2 :public Base<T2>
{
public:
	Son2()
	{
		cout << typeid(T1).name() << endl;
		cout << typeid(T2).name() << endl;
	}
};

void test02()
{
	Son2<int, char> child1;		//char传给T2,T2指定父类中的T类型，故父类中的m为char类型
}


int main() 
{
	test01();
	test02();
	system("pause");
	return 0;
}
```



#### 1.3.3 类模板成员函数类外实现

学习目标：能够掌握类模板中的成员函数类外实现

**示例：**

```C++
#include <string>

//类模板中成员函数类外实现
template<class T1, class T2>
class Person 
{
public:
	//成员函数类内声明
	Person(T1 name, T2 age);
	void showPerson();

public:
	T1 m_Name;
	T2 m_Age;
};

//构造函数 类外实现
template<class T1, class T2>
Person<T1, T2>::Person(T1 name, T2 age) 
{
	this->m_Name = name;
	this->m_Age = age;
}

//成员函数 类外实现
template<class T1, class T2>
void Person<T1, T2>::showPerson() 
{
	cout << "姓名: " << this->m_Name << " 年龄:" << this->m_Age << endl;
}

void test01()
{
	Person<string, int> p("Tom", 20);
	p.showPerson();
}

int main() 
{
	test01();
	system("pause");
	return 0;
}
```

总结：类模板中成员函数类外实现时，需要加上==模板参数列表==



#### 1.3.3 类模板分文件编写

学习目标：

* 掌握类模板成员函数分文件编写产生的问题以及解决方式

问题：

* 类模板中成员函数创建时机是在调用阶段，导致分文件编写时链接不到


解决：

* 解决方式1：在需要调用的cpp文件中直接包含.cpp源文件
* 解决方式2：将声明和实现写到同一个头文件中，并更改后缀名为.hpp （hpp是约定的名称，并不是强制）

**示例：**(用上面1.3.3的示例说明)

在头文件中创建Person.h    创建类模板，声明构造函数和void showPerson()函数

在源文件中创建Person.cpp    实现构造函数和void showPerson()函数

此时，我们调用test01()代码

```c++
void test01()
{
	Person<string, int> p("Tom", 20);
	p.showPerson();
}
```

则会报错，因为类模板是在调用时才创建，在包含头文件`#include"Person.h"`时，编译器并不会知道Person.cpp中的函数实现。

**解决方法1：**

把`#include"Person.h"`改为`#include"Person.cpp"`，直接让编译器知道该函数。

一般不用此方法，因为在写程序时通常都是包含.h文件的

**解决方法2：**

将.h和.cpp文件写成一个.hpp头文件，模板类的函数声明和定义都在此头文件中完成

，当要用的时候直接包含`#include"Person.hpp"`头文件即可



#### 1.3.4 类模板与友元

学习目标：

* 掌握类模板配合友元函数的类内和类外实现

全局函数类内实现 - 直接在类内声明友元即可

全局函数类外实现 - 需要提前让编译器知道全局函数的存在



**示例：**(将上述例子的showPerson函数友元化)

类内直接声明友元

```c++
	friend void printPerson(Person<T1, T2>& p)
	{
		cout << "姓名： " << p.m_Name << " 年龄：" << p.m_Age << endl;
	}
```

类外：

```c++
#include<iostream>
using namespace std;
#include<string>

//2、全局函数在类外实现

template<class T1, class T2>
class Person;					//提前让编译器知道Person类存在(因为printPerson2函数提前了)

template<class T1, class T2>	//先让编译器知道有printPerson2函数的存在，不然在声明友元时会报错，“printPerson2” : 未声明的标识符
void printPerson2(Person<T1, T2>& p)
{
	cout << "姓名： " << p.m_Name << " 年龄：" << p.m_Age << endl;
}

template<class T1, class T2>
class Person
{
	//全局函数配合友元  类外实现
	friend void printPerson2<>(Person<T1, T2>& p);		//加上空模板参数列表<>，让编译器知道这是类函数模板的声明

public:
	Person(T1 name, T2 age)
	{
		this->m_Name = name;
		this->m_Age = age;
	}
private:
	T1 m_Name;
	T2 m_Age;
};

void test02()
{
	Person <string, int >p("Jerry", 30);
	printPerson2(p);
}

int main() 
{
	test02();
	system("pause");
	return 0;
}
```

总结：建议全局函数做类内实现，用法简单，而且编译器可以直接识别



#### 1.3.5 类模板案例 --- 数组类封装

案例描述:  实现一个通用的数组类，要求如下：

* 可以对内置数据类型以及自定义数据类型的数据进行存储
* 将数组中的数据存储到堆区
* 构造函数中可以传入数组的容量
* 提供对应的拷贝构造函数以及operator=防止浅拷贝问题
* 提供尾插法和尾删法对数组中的数据进行增加和删除
* 可以通过下标的方式访问数组中的元素
* 可以获取数组中当前元素个数和数组的容量



**示例：**

myArray.hpp头文件中的代码：

```c++
#pragma once
#include <iostream>
using namespace std;

template<class T>
class MyArray
{
public:
    
	//构造函数
	MyArray(int capacity)
	{
		this->m_Capacity = capacity;
		this->m_Size = 0;
		pAddress = new T[this->m_Capacity];
	}

	//拷贝构造
	MyArray(const MyArray & arr)
	{
		this->m_Capacity = arr.m_Capacity;
		this->m_Size = arr.m_Size;
		this->pAddress = new T[this->m_Capacity];
		for (int i = 0; i < this->m_Size; i++)
		{
			//如果T为对象，而且还包含指针，必须需要重载 = 操作符，因为这个等号不是 构造 而是赋值，
			// 普通类型可以直接= 但是指针类型需要深拷贝
			this->pAddress[i] = arr.pAddress[i];
		}
	}

	//重载= 操作符  防止浅拷贝问题
	MyArray& operator=(const MyArray& myarray) {

		if (this->pAddress != NULL) {
			delete[] this->pAddress;
			this->m_Capacity = 0;
			this->m_Size = 0;
		}

		this->m_Capacity = myarray.m_Capacity;
		this->m_Size = myarray.m_Size;
		this->pAddress = new T[this->m_Capacity];
		for (int i = 0; i < this->m_Size; i++) {
			this->pAddress[i] = myarray[i];
		}
		return *this;
	}

	//重载[] 操作符  arr[0]
	T& operator [](int index)
	{
		return this->pAddress[index]; //不考虑越界，用户自己去处理
	}

	//尾插法
	void Push_back(const T & val)
	{
		if (this->m_Capacity == this->m_Size)
		{
			return;
		}
		this->pAddress[this->m_Size] = val;
		this->m_Size++;
	}

	//尾删法
	void Pop_back()
	{
		if (this->m_Size == 0)
		{
			return;
		}
		this->m_Size--;
	}

	//获取数组容量
	int getCapacity()
	{
		return this->m_Capacity;
	}

	//获取数组大小
	int	getSize()
	{
		return this->m_Size;
	}


	//析构
	~MyArray()
	{
		if (this->pAddress != NULL)
		{
			delete[] this->pAddress;
			this->pAddress = NULL;
			this->m_Capacity = 0;
			this->m_Size = 0;
		}
	}

private:
	T * pAddress;  //指向一个堆空间，这个空间存储真正的数据
	int m_Capacity; //容量
	int m_Size;   // 大小
};
```



数组类封装.cpp源文件中：

```c++
#include<iostream>
using namespace std;
#include "myArray.hpp"
#include <string>

void printIntArray(MyArray<int>& arr) {
	for (int i = 0; i < arr.getSize(); i++) {
		cout << arr[i] << " ";
	}
	cout << endl;
}

//测试内置数据类型
void test01()
{
	MyArray<int> array1(10);
	for (int i = 0; i < 10; i++)
	{
		array1.Push_back(i);
	}
	cout << "array1打印输出：" << endl;
	printIntArray(array1);
	cout << "array1的大小：" << array1.getSize() << endl;
	cout << "array1的容量：" << array1.getCapacity() << endl;

	cout << "--------------------------" << endl;

	MyArray<int> array2(array1);
	array2.Pop_back();
	cout << "array2打印输出：" << endl;
	printIntArray(array2);
	cout << "array2的大小：" << array2.getSize() << endl;
	cout << "array2的容量：" << array2.getCapacity() << endl;
}

//测试自定义数据类型
class Person {
public:
	Person() {} 
		Person(string name, int age) {
		this->m_Name = name;
		this->m_Age = age;
	}
public:
	string m_Name;
	int m_Age;
};

void printPersonArray(MyArray<Person>& personArr)
{
	for (int i = 0; i < personArr.getSize(); i++) {
		cout << "姓名：" << personArr[i].m_Name << " 年龄： " << personArr[i].m_Age << endl;
	}

}

void test02()
{
	//创建数组
	MyArray<Person> pArray(10);
	Person p1("孙悟空", 30);
	Person p2("韩信", 20);
	Person p3("妲己", 18);
	Person p4("王昭君", 15);
	Person p5("赵云", 24);

	//插入数据
	pArray.Push_back(p1);
	pArray.Push_back(p2);
	pArray.Push_back(p3);
	pArray.Push_back(p4);
	pArray.Push_back(p5);

	printPersonArray(pArray);

	cout << "pArray的大小：" << pArray.getSize() << endl;
	cout << "pArray的容量：" << pArray.getCapacity() << endl;

}

int main() {

	//test01();
	test02();
	system("pause");
	return 0;
}
```



## 2、STL初识

### 2.1 STL的诞生

* 长久以来，软件界一直希望建立一种可重复利用的东西

* C++的**面向对象**和**泛型编程**思想，目的就是**复用性的提升**

* 大多情况下，数据结构和算法都未能有一套标准,导致被迫从事大量重复工作

* 为了建立数据结构和算法的一套标准,诞生了**STL**

  


### 2.2 STL基本概念

* STL(Standard Template Library,**标准模板库**)
* STL 从广义上分为: **容器(container) 算法(algorithm) 迭代器(iterator)**
* **容器**和**算法**之间通过**迭代器**进行无缝连接。
* STL 几乎所有的代码都采用了模板类或者模板函数



### 2.3 STL六大组件

STL大体分为六大组件，分别是:**容器、算法、迭代器、仿函数、适配器（配接器）、空间配置器**

1. 容器：各种数据结构，如vector、list、deque、set、map等,用来存放数据。
2. 算法：各种常用的算法，如sort、find、copy、for_each等
3. 迭代器：扮演了容器与算法之间的胶合剂。
4. 仿函数：行为类似函数，可作为算法的某种策略。
5. 适配器：一种用来修饰容器或者仿函数或迭代器接口的东西。
6. 空间配置器：负责空间的配置与管理。



### 2.4  STL中容器、算法、迭代器

**容器：**置物之所也

STL**容器**就是将运用**最广泛的一些数据结构**实现出来

常用的数据结构：数组, 链表,树, 栈, 队列, 集合, 映射表 等

这些容器分为**序列式容器**和**关联式容器**两种:

​	**序列式容器**:强调值的排序，序列式容器中的每个元素均有固定的位置。
​	**关联式容器**:二叉树结构，各元素之间没有严格的物理上的顺序关系



**算法：**问题之解法也

有限的步骤，解决逻辑或数学上的问题，这一门学科我们叫做算法(Algorithms)

算法分为:**质变算法**和**非质变算法**。

质变算法：是指运算过程中会更改区间内的元素的内容。例如拷贝，替换，删除等等

非质变算法：是指运算过程中不会更改区间内的元素内容，例如查找、计数、遍历、寻找极值等等



**迭代器：**容器和算法之间粘合剂

提供一种方法，使之能够依序寻访某个容器所含的各个元素，而又无需暴露该容器的内部表示方式。

每个容器都有自己专属的迭代器

迭代器使用非常类似于指针，初学阶段我们可以先理解迭代器为指针



迭代器种类：

| 种类           | 功能                                                     | 支持运算                                |
| -------------- | -------------------------------------------------------- | --------------------------------------- |
| 输入迭代器     | 对数据的只读访问                                         | 只读，支持++、==、！=                   |
| 输出迭代器     | 对数据的只写访问                                         | 只写，支持++                            |
| 前向迭代器     | 读写操作，并能向前推进迭代器                             | 读写，支持++、==、！=                   |
| 双向迭代器     | 读写操作，并能向前和向后操作                             | 读写，支持++、--，                      |
| 随机访问迭代器 | 读写操作，可以以跳跃的方式访问任意数据，功能最强的迭代器 | 读写，支持++、--、[n]、-n、<、<=、>、>= |

常用的容器中迭代器种类为双向迭代器，和随机访问迭代器



### 2.5 容器算法迭代器初识

了解STL中容器、算法、迭代器概念之后，我们利用代码感受STL的魅力

STL中最常用的容器为Vector，可以理解为数组，下面我们将学习如何向这个容器中插入数据、并遍历这个容器



#### 2.5.1 vector存放内置数据类型

容器：     `vector`

算法：     `for_each`

迭代器： `vector<int>::iterator`

**示例：**

```c++
#include <vector>
#include <algorithm>

void MyPrint(int val)
{
	cout << val << endl;
}

void test01() {

	//创建vector容器对象，并且通过模板参数指定容器中存放的数据的类型
	vector<int> v;
	//向容器中放数据
	v.push_back(10);
	v.push_back(20);
	v.push_back(30);
	v.push_back(40);

	//每一个容器都有自己的迭代器，迭代器是用来遍历容器中的元素
	//v.begin()返回迭代器，这个迭代器指向容器中第一个数据
	//v.end()返回迭代器，这个迭代器指向容器元素的最后一个元素的下一个位置
	//vector<int>::iterator 拿到vector<int>这种容器的迭代器类型

	vector<int>::iterator pBegin = v.begin();
	vector<int>::iterator pEnd = v.end();

	//第一种遍历方式：
	while (pBegin != pEnd) {
		cout << *pBegin << endl;
		pBegin++;
	}

	
	//第二种遍历方式：
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << endl;
	}
	cout << endl;

	//第三种遍历方式：
	//使用STL提供标准遍历算法  头文件 algorithm
	for_each(v.begin(), v.end(), MyPrint);
}

int main() {

	test01();

	system("pause");

	return 0;
}
```



#### 2.5.2 Vector存放自定义数据类型

学习目标：vector中存放自定义数据类型，并打印输出

**示例：**

```c++
#include <vector>
#include <string>

//自定义数据类型
class Person {
public:
	Person(string name, int age) {
		mName = name;
		mAge = age;
	}
public:
	string mName;
	int mAge;
};
//存放对象
void test01() {

	vector<Person> v;

	//创建数据
	Person p1("aaa", 10);
	Person p2("bbb", 20);
	Person p3("ccc", 30);
	Person p4("ddd", 40);
	Person p5("eee", 50);

	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);
	v.push_back(p5);

	for (vector<Person>::iterator it = v.begin(); it != v.end(); it++) {		//it是一个指针
		cout << "Name:" << (*it).mName << " Age:" << (*it).mAge << endl;	//*it就是<>里的内容
		//或者：用 it->mNane  也行
	}
}


//放对象指针
void test02() {

	vector<Person*> v;

	//创建数据
	Person p1("aaa", 10);
	Person p2("bbb", 20);
	Person p3("ccc", 30);
	Person p4("ddd", 40);
	Person p5("eee", 50);

	v.push_back(&p1);
	v.push_back(&p2);
	v.push_back(&p3);
	v.push_back(&p4);
	v.push_back(&p5);

	for (vector<Person*>::iterator it = v.begin(); it != v.end(); it++) {
		cout << "Name:" << (*it)->mName << " Age:" << (*it)->mAge << endl;
        	//*it 是<>里的Person* 类型
	}
}


int main() {

	test01();
    
	test02();

	system("pause");

	return 0;
}
```



#### 2.5.3 Vector容器嵌套容器



学习目标：容器中嵌套容器，我们将所有数据进行遍历输出



**示例：**

```c++
#include <vector>

//容器嵌套容器
void test01() {

	vector< vector<int> >  v;

	vector<int> v1;
	vector<int> v2;
	vector<int> v3;
	vector<int> v4;

	for (int i = 0; i < 4; i++) {
		v1.push_back(i + 1);
		v2.push_back(i + 2);
		v3.push_back(i + 3);
		v4.push_back(i + 4);
	}

	//将容器元素插入到vector v中
	v.push_back(v1);
	v.push_back(v2);
	v.push_back(v3);
	v.push_back(v4);

	//遍历所有数据
	for (vector<vector<int>>::iterator it = v.begin(); it != v.end(); it++) {
		//（*it）---vector<int>类型
		for (vector<int>::iterator vit = (*it).begin(); vit != (*it).end(); vit++) {
			cout << *vit << " ";      //（*it）---int类型
		}
		cout << endl;
	}

}

int main() {

	test01();

	system("pause");

	return 0;
}
```





## 3、 STL- 常用容器

### 3.1 string容器



#### 3.1.1 string基本概念

**本质：**

* string是C++风格的字符串，而string本质上是一个类



**string和char * 区别：**

* char * 是一个指针
* string是一个类，类内部封装了char\*，管理这个字符串，是一个char*型的容器。



**特点：**

string 类内部封装了很多成员方法

例如：查找find，拷贝copy，删除delete 替换replace，插入insert

string管理char*所分配的内存，不用担心复制越界和取值越界等，由类内部进行负责



#### 3.1.2 string构造函数

构造函数原型：

* `string();`          				//创建一个空的字符串 例如: string str;
  `string(const char* s);`	        //使用字符串s初始化
* `string(const string& str);`    //使用一个string对象初始化另一个string对象
* `string(int n, char c);`           //使用n个字符c初始化 

**示例：**

```C++
#include <string>
//string构造
void test01()
{
	string s1; //创建空字符串，调用无参构造函数
	cout << "str1 = " << s1 << endl;

	const char* str = "hello world";
	string s2(str); //把c_string转换成了string

	cout << "str2 = " << s2 << endl;

	string s3(s2); //调用拷贝构造函数
	cout << "str3 = " << s3 << endl;

	string s4(10, 'a');
	cout << "str3 = " << s3 << endl;		//aaaaaaaaaa
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：string的多种构造方式没有可比性，灵活使用即可



#### 3.1.3 string赋值操作

功能描述：

* 给string字符串进行赋值

赋值的函数原型：

* `string& operator=(const char* s);`             //char*类型字符串 赋值给当前的字符串
* `string& operator=(const string &s);`         //把字符串s赋给当前的字符串
* `string& operator=(char c);`                          //字符赋值给当前的字符串
* `string& assign(const char *s);`                  //把字符串s赋给当前的字符串
* `string& assign(const char *s, int n);`     //把字符串s的前n个字符赋给当前的字符串
* `string& assign(const string &s);`              //把字符串s赋给当前字符串
* `string& assign(int n, char c);`                  //用n个字符c赋给当前字符串

**示例：**

```C++
//赋值
void test01()
{
	string str1;
	str1 = "hello world";
	cout << "str1 = " << str1 << endl;

	string str2;
	str2 = str1;
	cout << "str2 = " << str2 << endl;

	string str3;
	str3 = 'a';
	cout << "str3 = " << str3 << endl;

	string str4;
	str4.assign("hello c++");
	cout << "str4 = " << str4 << endl;

	string str5;
	str5.assign("hello c++",5);
	cout << "str5 = " << str5 << endl;


	string str6;
	str6.assign(str5);
	cout << "str6 = " << str6 << endl;

	string str7;
	str7.assign(5, 'x');
	cout << "str7 = " << str7 << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

​	string的赋值方式很多，`operator=`  这种方式是比较实用的



#### 3.1.4 string字符串拼接

**功能描述：**

* 实现在字符串末尾拼接字符串

**函数原型：**

* `string& operator+=(const char* str);`                   //重载+=操作符
* `string& operator+=(const char c);`                         //重载+=操作符
* `string& operator+=(const string& str);`                //重载+=操作符
* `string& append(const char *s); `                               //把字符串s连接到当前字符串结尾
* `string& append(const char *s, int n);`                 //把字符串s的前n个字符连接到当前字符串结尾
* `string& append(const string &s);`                           //同operator+=(const string& str)
* `string& append(const string &s, int pos, int n);`//字符串s中从pos开始的n个字符连接到字符串结尾

**示例：**


```C++
//字符串拼接
void test01()
{
	string str1 = "我";

	str1 += "爱玩游戏";

	cout << "str1 = " << str1 << endl;
	
	str1 += ':';

	cout << "str1 = " << str1 << endl;

	string str2 = "LOL DNF";

	str1 += str2;

	cout << "str1 = " << str1 << endl;

	string str3 = "I";
	str3.append(" love ");
	str3.append("game abcde", 4);
	//str3.append(str2);
	str3.append(str2, 4, 3); // 从下标4位置开始 ，截取3个字符，拼接到字符串末尾
	cout << "str3 = " << str3 << endl;
}
int main() {

	test01();

	system("pause");

	return 0;
}
```



#### 3.1.5 string查找和替换

**功能描述：**

* 查找：查找指定字符串是否存在
* 替换：在指定的位置替换字符串

**函数原型：**

* `int find(const string& str, int pos = 0) const;`              //查找str第一次出现位置,从pos开始查找
* `int find(const char* s, int pos = 0) const; `                     //查找s第一次出现位置,从pos开始查找
* `int find(const char* s, int pos, int n) const; `               //从pos位置查找s的前n个字符第一次位置
* `int find(const char c, int pos = 0) const; `                       //查找字符c第一次出现位置
* `int rfind(const string& str, int pos = npos) const;`      //查找str最后一次位置,从pos开始查找
* `int rfind(const char* s, int pos = npos) const;`              //查找s最后一次出现位置,从pos开始查找
* `int rfind(const char* s, int pos, int n) const;`              //从pos查找s的前n个字符最后一次位置
* `int rfind(const char c, int pos = 0) const;  `                      //查找字符c最后一次出现位置
* `string& replace(int pos, int n, const string& str); `       //替换从pos开始n个字符为字符串str
* `string& replace(int pos, int n,const char* s); `                 //替换从pos开始的n个字符为字符串s

**示例：**

```C++
//查找和替换
void test01()
{
	//查找
	string str1 = "abcdefgde";

	int pos = str1.find("de");

	if (pos == -1)
	{
		cout << "未找到" << endl;
	}
	else
	{
		cout << "pos = " << pos << endl;     //pos=3
	}
	

	pos = str1.rfind("de");			//从右往左查找,但是返回的位置还是从左往左计算

	cout << "pos = " << pos << endl;		//pos=7

}

void test02()
{
	//替换
	string str1 = "abcdefgde";
	str1.replace(1, 3, "1111");		//从第一个位置起，三个字符替换成1111

	cout << "str1 = " << str1 << endl; 		//str1=a1111efgde
} 

int main() {

	//test01();
	test02();

	system("pause");

	return 0;
}
```

总结：

* find查找是从左往后，rfind从右往左
* find找到字符串后返回查找的第一个字符位置，找不到返回-1
* replace在替换时，要指定从哪个位置起，多少个字符，替换成什么样的字符串



####    3.1.6 string字符串比较

**功能描述：**

* 字符串之间的比较

**比较方式：**

* 字符串比较是按字符的ASCII码进行对比

= 返回   0

\> 返回   1 

< 返回  -1

**函数原型：**

* `int compare(const string &s) const; `  //与字符串s比较
* `int compare(const char *s) const;`      //与字符串s比较

**示例：**

```C++
//字符串比较
void test01()
{

	string s1 = "hello";
	string s2 = "aello";

	int ret = s1.compare(s2);

	if (ret == 0) {
		cout << "s1 等于 s2" << endl;
	}
	else if (ret > 0)
	{
		cout << "s1 大于 s2" << endl;
	}
	else
	{
		cout << "s1 小于 s2" << endl;
	}

}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：字符串对比主要是用于比较两个字符串是否相等，判断谁大谁小的意义并不是很大



#### 3.1.7 string字符存取

string中单个字符存取方式有两种

* `char& operator[](int n); `     //通过[]方式取字符
* `char& at(int n);   `                    //通过at方法获取字符

**示例：**

```C++
void test01()
{
	string str = "hello world";

	for (int i = 0; i < str.size(); i++)
	{
		cout << str[i] << " ";
	}
	cout << endl;

	for (int i = 0; i < str.size(); i++)
	{
		cout << str.at(i) << " ";
	}
	cout << endl;


	//字符修改
	str[0] = 'x';
	str.at(1) = 'x';
	cout << str << endl;
	
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：string字符串中单个字符存取有两种方式，利用 [ ] 或 at



#### 3.1.8 string插入和删除

**功能描述：**

* 对string字符串进行插入和删除字符操作

**函数原型：**

* `string& insert(int pos, const char* s);  `                //插入字符串
* `string& insert(int pos, const string& str); `        //插入字符串
* `string& insert(int pos, int n, char c);`                //在指定位置插入n个字符c
* `string& erase(int pos, int n = npos);`                    //删除从Pos开始的n个字符 

**示例：**

```C++
//字符串插入和删除
void test01()
{
	string str = "hello";
	str.insert(1, "111");
	cout << str << endl;		//h111ello

	str.erase(1, 3);  //从1号位置开始3个字符
	cout << str << endl;		//hello (把刚刚插入的111删除)
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

**总结：**插入和删除的起始下标都是从0开始



#### 3.1.9 string子串

**功能描述：**

* 从字符串中获取想要的子串

**函数原型：**

* `string substr(int pos = 0, int n = npos) const;`   //返回由pos开始的n个字符组成的字符串

**示例：**

```C++
//子串
void test01()
{

	string str = "abcdefg";
	string subStr = str.substr(1, 3);
	cout << "subStr = " << subStr << endl;		//subStr=bcd

	string email = "hello@sina.com";
	int pos = email.find("@");		//pos=5
	string username = email.substr(0, pos);
	cout << "username: " << username << endl;	//username:hello

}

int main() {

	test01();

	system("pause");

	return 0;
}
```

**总结：**灵活的运用求子串功能，可以在实际开发中获取有效的信息





### 3.2 vector容器

#### 3.2.1 vector基本概念

**功能：**

* vector数据结构和**数组非常相似**，也称为**单端数组**

**vector与普通数组区别：**

* 不同之处在于数组是静态空间，而vector可以**动态扩展**

**动态扩展：**

* 并不是在原空间之后续接新空间，而是找更大的内存空间，然后将原数据拷贝新空间，释放原空间

![](D:\Snipaste截图\Snipaste_2021-11-26_23-21-35.png)

- vector容器的迭代器是支持随机访问的迭代器



#### 3.2.2 vector构造函数

**功能描述：**

* 创建vector容器

**函数原型：**

* `vector<T> v; `               		     //采用模板实现类实现，默认构造函数
* `vector(v.begin(), v.end());   `       //将v[begin(), end())区间中的元素拷贝给本身。
* `vector(n, elem);`                            //构造函数将n个elem拷贝给本身。
* `vector(const vector &vec);`         //拷贝构造函数。

**示例：**


```C++
#include <vector>

void printVector(vector<int>& v) {

	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test01()
{
	vector<int> v1; //无参构造
	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}
	printVector(v1);

	vector<int> v2(v1.begin(), v1.end());
	printVector(v2);

	vector<int> v3(10, 100);
	printVector(v3);				//v3为10个100的vector容器
	
	vector<int> v4(v3);
	printVector(v4);
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

**总结：**vector的多种构造方式没有可比性，灵活使用即可



#### 3.2.3 vector赋值操作

**功能描述：**

* 给vector容器进行赋值

**函数原型：**

* `vector& operator=(const vector &vec);`//重载等号操作符


* `assign(beg, end);`       //将[beg, end)区间中的数据拷贝赋值给本身。
* `assign(n, elem);`        //将n个elem拷贝赋值给本身。

**示例：**

```C++
#include <vector>

void printVector(vector<int>& v) {

	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//赋值操作
void test01()
{
	vector<int> v1; //无参构造
	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}
	printVector(v1);

	vector<int>v2;
	v2 = v1;
	printVector(v2);

	vector<int>v3;
	v3.assign(v1.begin(), v1.end());
	printVector(v3);

	vector<int>v4;
	v4.assign(10, 100);
	printVector(v4);
}

int main() {

	test01();

	system("pause");

	return 0;
}

```

总结： vector赋值方式比较简单，使用operator=，或者assign都可以



#### 3.2.4  vector容量和大小

**功能描述：**

* 对vector容器的容量和大小操作

**函数原型：**

* `empty(); `                            //判断容器是否为空

* `capacity();`                      //容器的容量

* `size();`                              //返回容器中元素的个数

* `resize(int num);`             //重新指定容器的长度为num，若容器变长，则以默认值填充新位置。

  ​					      //如果容器变短，则末尾超出容器长度的元素被删除。

* `resize(int num, elem);`  //重新指定容器的长度为num，若容器变长，则以elem值填充新位置。

  ​				              //如果容器变短，则末尾超出容器长度的元素被删除

**示例：**


```C++
#include <vector>

void printVector(vector<int>& v) {

	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test01()
{
	vector<int> v1;
	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}
	printVector(v1);
	if (v1.empty())
	{
		cout << "v1为空" << endl;
	}
	else
	{
		cout << "v1不为空" << endl;
		cout << "v1的容量 = " << v1.capacity() << endl;
		cout << "v1的大小 = " << v1.size() << endl;
	}

	//resize 重新指定大小 ，若指定的更大，默认用0填充新位置，可以利用重载版本替换默认填充
	v1.resize(15,10);
	printVector(v1);

	//resize 重新指定大小 ，若指定的更小，超出部分元素被删除
	v1.resize(5);
	printVector(v1);
}

int main() {

	test01();

	system("pause");

	return 0;
}

```

总结：

* 判断是否为空  --- empty
* 返回元素个数  --- size
* 返回容器容量  --- capacity
* 重新指定大小  ---  resize



#### 3.2.5 vector插入和删除

**功能描述：**

* 对vector容器进行插入、删除操作

**函数原型：**

* `push_back(ele);`                                         //尾部插入元素ele
* `pop_back();`                                                //删除最后一个元素
* `insert(const_iterator pos, ele);`        //迭代器指向位置pos插入元素ele
* `insert(const_iterator pos, int count,ele);`//迭代器指向位置pos插入count个元素ele
* `erase(const_iterator pos);`                     //删除迭代器指向的元素
* `erase(const_iterator start, const_iterator end);`//删除迭代器从start到end之间的元素
* `clear();`                                                        //删除容器中所有元素

**示例：**

```C++
#include <vector>

void printVector(vector<int>& v) {

	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//插入和删除
void test01()
{
	vector<int> v1;
	//尾插
	v1.push_back(10);
	v1.push_back(20);
	v1.push_back(30);
	v1.push_back(40);
	v1.push_back(50);
	printVector(v1);
	//尾删
	v1.pop_back();
	printVector(v1);        		//10 20 30 40
	//插入
	v1.insert(v1.begin(), 100);
	printVector(v1);			//100 10 20 30 40

	v1.insert(v1.begin(), 2, 1000);
	printVector(v1);			//1000 1000 100 10 20 30 40

	//删除
	v1.erase(v1.begin());
	printVector(v1);			//1000 100 10 20 30 40

	//清空
	v1.erase(v1.begin(), v1.end());
	v1.clear();
	printVector(v1);			
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

* 尾插  --- push_back
* 尾删  --- pop_back
* 插入  --- insert    (位置迭代器)
* 删除  --- erase  （位置迭代器）
* 清空  ---  clear  



#### 3.2.6 vector数据存取

**功能描述：**

* 对vector中的数据的存取操作

**函数原型：**

* `at(int idx); `     //返回索引idx所指的数据
* `operator[]; `       //返回索引idx所指的数据
* `front(); `            //返回容器中第一个数据元素
* `back();`              //返回容器中最后一个数据元素

**示例：**

```C++
#include <vector>

void test01()
{
	vector<int>v1;
	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}

	for (int i = 0; i < v1.size(); i++)
	{
		cout << v1[i] << " ";
	}
	cout << endl;

	for (int i = 0; i < v1.size(); i++)
	{
		cout << v1.at(i) << " ";
	}
	cout << endl;

	cout << "v1的第一个元素为： " << v1.front() << endl;
	cout << "v1的最后一个元素为： " << v1.back() << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

* 除了用迭代器获取vector容器中元素，[ ]和at也可以
* front返回容器第一个元素
* back返回容器最后一个元素



#### 3.2.7 vector互换容器

**功能描述：**

* 实现两个容器内元素进行互换

**函数原型：**

* `swap(vec);`  // 将vec与本身的元素互换

**示例：**

```C++
#include <vector>

void printVector(vector<int>& v) {

	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test01()
{
	vector<int>v1;
	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}
	printVector(v1);

	vector<int>v2;
	for (int i = 10; i > 0; i--)
	{
		v2.push_back(i);
	}
	printVector(v2);

	//互换容器
	cout << "互换后" << endl;
	v1.swap(v2);
	printVector(v1);
	printVector(v2);
}

void test02()
{
	vector<int> v;
	for (int i = 0; i < 100000; i++) {
		v.push_back(i);
	}

	cout << "v的容量为：" << v.capacity() << endl;
	cout << "v的大小为：" << v.size() << endl;

	v.resize(3);			//从新指定大小

	cout << "v的容量为：" << v.capacity() << endl;		//135000
	cout << "v的大小为：" << v.size() << endl;        		//3

	//收缩内存
	vector<int>(v).swap(v); //匿名对象

	cout << "v的容量为：" << v.capacity() << endl;		//3
	cout << "v的大小为：" << v.size() << endl;			//3
}

int main() {

	test01();

	test02();

	system("pause");

	return 0;
}

```

总结：swap可以使两个容器互换，可以达到实用的收缩内存效果



```c++
v.resize(3);			//从新指定大小
//收缩内存
vector<int>(v).swap(v); //匿名对象
```

图解如下：

![](D:\Snipaste截图\Snipaste_2021-11-26_23-46-55.png)



#### 3.2.8 vector预留空间

**功能描述：**

* 减少vector在动态扩展容量时的扩展次数



**函数原型：**

* `reserve(int len);`//容器预留len个元素长度，预留位置不初始化，元素不可访问。

**示例：**

```C++
#include <vector>

void test01()
{
	vector<int> v;

	//预留空间
	v.reserve(100000);

	int num = 0;
	int* p = NULL;
	for (int i = 0; i < 100000; i++) {
		v.push_back(i);
		if (p != &v[0]) {
			p = &v[0];
			num++;
		}
	}

	cout << "num:" << num << endl;
}

int main() {

	test01();
    
	system("pause");

	return 0;
}
```

总结：如果数据量较大，可以一开始利用reserve预留空间



### 3.3 list容器

#### 3.3.1 list基本概念

**功能：**将数据进行链式存储

**链表**（list）是一种物理存储单元上非连续的存储结构，数据元素的逻辑顺序是通过链表中的指针链接实现的

链表的组成：链表由一系列**结点**组成

结点的组成：一个是存储数据元素的**数据域**，另一个是存储下一个结点地址的**指针域**

STL中的链表是一个双向循环链表

![](D:\Snipaste截图\Snipaste_2021-11-27_13-53-14.png)

**示例：**

```C++
#include <list>

void printList(const list<int>& L) {

	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test01()
{
	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	L1.push_back(40);

	printList(L1);

	list<int>L2(L1.begin(),L1.end());
	printList(L2);

	list<int>L3(L2);
	printList(L3);

	list<int>L4(10, 1000);
	printList(L4);
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：list构造方式同其他几个STL常用容器，熟练掌握即可



#### 3.3.2  list构造函数

**功能描述：**

* 创建list容器



**函数原型：**

* `list<T> lst;`                               //list采用模板类实现,对象的默认构造形式：
* `list(beg,end);`                           //构造函数将[beg, end)区间中的元素拷贝给本身。
* `list(n,elem);`                             //构造函数将n个elem拷贝给本身。
* `list(const list &lst);`            //拷贝构造函数。





**示例：**

```C++
#include <list>

void printList(const list<int>& L) {

	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test01()
{
	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	L1.push_back(40);

	printList(L1);

	list<int>L2(L1.begin(),L1.end());
	printList(L2);

	list<int>L3(L2);
	printList(L3);

	list<int>L4(10, 1000);
	printList(L4);
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：list构造方式同其他几个STL常用容器，熟练掌握即可



#### 3.3.3 list 赋值和交换

**功能描述：**

* 给list容器进行赋值，以及交换list容器

**函数原型：**

* `assign(beg, end);`            //将[beg, end)区间中的数据拷贝赋值给本身。
* `assign(n, elem);`              //将n个elem拷贝赋值给本身。
* `list& operator=(const list &lst);`         //重载等号操作符
* `swap(lst);`                         //将lst与本身的元素互换。

**示例：**

```C++
#include <list>

void printList(const list<int>& L) {

	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//赋值和交换
void test01()
{
	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	L1.push_back(40);
	printList(L1);

	//赋值
	list<int>L2;
	L2 = L1;
	printList(L2);

	list<int>L3;
	L3.assign(L2.begin(), L2.end());
	printList(L3);

	list<int>L4;
	L4.assign(10, 100);
	printList(L4);

}

//交换
void test02()
{

	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	L1.push_back(40);

	list<int>L2;
	L2.assign(10, 100);

	cout << "交换前： " << endl;
	printList(L1);
	printList(L2);

	cout << endl;

	L1.swap(L2);

	cout << "交换后： " << endl;
	printList(L1);
	printList(L2);

}

int main() {

	//test01();

	test02();

	system("pause");

	return 0;
}
```

总结：list赋值和交换操作能够灵活运用即可



#### 3.3.4 list 大小操作

**功能描述：**

* 对list容器的大小进行操作



**函数原型：**

* `size(); `                             //返回容器中元素的个数

* `empty(); `                           //判断容器是否为空

* `resize(num);`                   //重新指定容器的长度为num，若容器变长，则以默认值填充新位置。

  ​					    //如果容器变短，则末尾超出容器长度的元素被删除。

* `resize(num, elem); `       //重新指定容器的长度为num，若容器变长，则以elem值填充新位置。

  //如果容器变短，则末尾超出容器长度的元素被删除。



**示例：**

```C++
#include <list>

void printList(const list<int>& L) {

	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//大小操作
void test01()
{
	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	L1.push_back(40);

	if (L1.empty())
	{
		cout << "L1为空" << endl;
	}
	else
	{
		cout << "L1不为空" << endl;
		cout << "L1的大小为： " << L1.size() << endl;
	}

	//重新指定大小
	L1.resize(10);
	printList(L1);

	L1.resize(2);
	printList(L1);
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

- 判断是否为空   --- empty
- 返回元素个数   --- size
- 重新指定个数   --- resize



#### 3.3.5 list 插入和删除

**功能描述：**

* 对list容器进行数据的插入和删除

**函数原型：**

* push_back(elem);//在容器尾部加入一个元素
* pop_back();//删除容器中最后一个元素
* push_front(elem);//在容器开头插入一个元素
* pop_front();//从容器开头移除第一个元素
* insert(pos,elem);//在pos位置插elem元素的拷贝，返回新数据的位置。
* insert(pos,n,elem);//在pos位置插入n个elem数据，无返回值。
* insert(pos,beg,end);//在pos位置插入[beg,end)区间的数据，无返回值。
* clear();//移除容器的所有数据
* erase(beg,end);//删除[beg,end)区间的数据，返回下一个数据的位置。
* erase(pos);//删除pos位置的数据，返回下一个数据的位置。
* remove(elem);//删除容器中所有与elem值匹配的元素。

**示例：**

```C++
#include <list>

void printList(const list<int>& L) {

	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//插入和删除
void test01()
{
	list<int> L;
	//尾插
	L.push_back(10);
	L.push_back(20);
	L.push_back(30);
	//头插
	L.push_front(100);
	L.push_front(200);
	L.push_front(300);

	printList(L);

	//尾删
	L.pop_back();
	printList(L);

	//头删
	L.pop_front();
	printList(L);

	//插入
	list<int>::iterator it = L.begin();
	L.insert(++it, 1000);
	printList(L);

	//删除
	it = L.begin();
	L.erase(++it);
	printList(L);

	//移除
	L.push_back(10000);
	L.push_back(10000);
	L.push_back(10000);
	printList(L);
	L.remove(10000);		//容器中所有10000都移除了
	printList(L);
    
    //清空
	L.clear();
	printList(L);
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

* 尾插   --- push_back
* 尾删   --- pop_back
* 头插   --- push_front
* 头删   --- pop_front
* 插入   --- insert
* 删除   --- erase
* 移除   --- remove
* 清空   --- clear



#### 3.3.6 list 数据存取

**功能描述：**

* 对list容器中数据进行存取

**函数原型：**

* `front();`        //返回第一个元素。
* `back();`         //返回最后一个元素。

**示例：**

```C++
#include <list>

//数据存取
void test01()
{
	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	L1.push_back(40);

	
	//cout << L1.at(0) << endl;//错误 不支持at访问数据
	//cout << L1[0] << endl; //错误  不支持[]方式访问数据
	cout << "第一个元素为： " << L1.front() << endl;
	cout << "最后一个元素为： " << L1.back() << endl;

	//list容器的迭代器是双向迭代器，不支持随机访问
	list<int>::iterator it = L1.begin();
	//it = it + 1;//错误，不可以跳跃访问，即使是+1
}

int main() {

	test01();

	system("pause");

	return 0;
}

```

总结：

* list容器中不可以通过[]或者at方式访问数据
* 返回第一个元素   --- front
* 返回最后一个元素   --- back



#### 3.3.7 list 反转和排序

**功能描述：**

* 将容器中的元素反转，以及将容器中的数据进行排序

**函数原型：**

* `reverse();`   //反转链表
* `sort();`        //链表排序

**示例：**

```C++
void printList(const list<int>& L) {

	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

bool myCompare(int val1 , int val2)
{
	return val1 > val2;
}

//反转和排序
void test01()
{
	list<int> L;
	L.push_back(90);
	L.push_back(30);
	L.push_back(20);
	L.push_back(70);
	printList(L);

	//反转容器的元素
	L.reverse();
	printList(L);

	//排序
        //sort(L.begin(),L.end());		报错
        //所有不支持随机访问迭代器的容器，不可以用标准算法，但是内部会提供对应一些算法
	L.sort(); //默认的排序规则 从小到大
	printList(L);

	L.sort(myCompare); //指定规则，从大到小
	printList(L);
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

* 反转   --- reverse
* 排序   --- sort （成员函数）



#### 3.3.8 排序案例

案例描述：将Person自定义数据类型进行排序，Person中属性有姓名、年龄、身高

排序规则：按照年龄进行升序，如果年龄相同按照身高进行降序

**示例：**

```C++
#include <list>
#include <string>
class Person {
public:
	Person(string name, int age , int height) {
		m_Name = name;
		m_Age = age;
		m_Height = height;
	}

public:
	string m_Name;  //姓名
	int m_Age;      //年龄
	int m_Height;   //身高
};


bool ComparePerson(Person& p1, Person& p2) {

	if (p1.m_Age == p2.m_Age) {
		return p1.m_Height  > p2.m_Height;
	}
	else
	{
		return  p1.m_Age < p2.m_Age;
	}

}

void test01() {

	list<Person> L;

	Person p1("刘备", 35 , 175);
	Person p2("曹操", 45 , 180);
	Person p3("孙权", 40 , 170);
	Person p4("赵云", 25 , 190);
	Person p5("张飞", 35 , 160);
	Person p6("关羽", 35 , 200);

	L.push_back(p1);
	L.push_back(p2);
	L.push_back(p3);
	L.push_back(p4);
	L.push_back(p5);
	L.push_back(p6);

	for (list<Person>::iterator it = L.begin(); it != L.end(); it++) {
		cout << "姓名： " << it->m_Name << " 年龄： " << it->m_Age 
              << " 身高： " << it->m_Height << endl;
	}

	cout << "---------------------------------" << endl;
	L.sort(ComparePerson); //排序

	for (list<Person>::iterator it = L.begin(); it != L.end(); it++) {
		cout << "姓名： " << it->m_Name << " 年龄： " << it->m_Age 
              << " 身高： " << it->m_Height << endl;
	}
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

* 对于自定义数据类型，必须要指定排序规则，否则编译器不知道如何进行排序


* 高级排序只是在排序规则上再进行一次逻辑规则制定，并不复杂





### 附--pair对组创建

**功能描述：**

* 成对出现的数据，利用对组可以返回两个数据





**两种创建方式：**

* `pair<type, type> p ( value1, value2 );`
* `pair<type, type> p = make_pair( value1, value2 );`





**示例：**

```C++
#include <string>

//对组创建
void test01()
{
	pair<string, int> p(string("Tom"), 20);
	cout << "姓名： " <<  p.first << " 年龄： " << p.second << endl;

	pair<string, int> p2 = make_pair("Jerry", 10);
	cout << "姓名： " << p2.first << " 年龄： " << p2.second << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

两种方式都可以创建对组，记住一种即可





### 3.4 map/ multimap容器

#### 3.4.1 map基本概念

**简介：**

* map中所有元素都是pair
* pair中第一个元素为key（键值），起到索引作用，第二个元素为value（实值）
* 所有元素都会根据元素的键值自动排序



**本质：**

* map/multimap属于**关联式容器**，底层结构是用二叉树实现。



**优点：**

* 可以根据key值快速找到value值



map和multimap**区别**：

- map不允许容器中有重复key值元素
- multimap允许容器中有重复key值元素




#### 3.4.2  map构造和赋值

**功能描述：**

* 对map容器进行构造和赋值操作

**函数原型：**

**构造：**

* `map<T1, T2> mp;`                     //map默认构造函数: 
* `map(const map &mp);`             //拷贝构造函数



**赋值：**

* `map& operator=(const map &mp);`    //重载等号操作符



**示例：**

```C++
#include <map>

void printMap(map<int,int>&m)
{
	for (map<int, int>::iterator it = m.begin(); it != m.end(); it++)
	{
		cout << "key = " << it->first << " value = " << it->second << endl;
	}
	cout << endl;
}

void test01()
{
	map<int,int>m; //默认构造
	m.insert(pair<int, int>(1, 10));		//map中所有元素都是成对出现，插入数据时候要使用对组pair
	m.insert(pair<int, int>(2, 20));
	m.insert(pair<int, int>(3, 30));
	printMap(m);

	map<int, int>m2(m); //拷贝构造
	printMap(m2);

	map<int, int>m3;
	m3 = m2; //赋值
	printMap(m3);
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：map中所有元素都是成对出现，插入数据时候要使用对组











#### 3.4.3 map大小和交换

**功能描述：**

* 统计map容器大小以及交换map容器





函数原型：

- `size();`          //返回容器中元素的数目
- `empty();`        //判断容器是否为空
- `swap(st);`      //交换两个集合容器





**示例：**

```C++
#include <map>

void printMap(map<int,int>&m)
{
	for (map<int, int>::iterator it = m.begin(); it != m.end(); it++)
	{
		cout << "key = " << it->first << " value = " << it->second << endl;
	}
	cout << endl;
}

void test01()
{
	map<int, int>m;
	m.insert(pair<int, int>(1, 10));
	m.insert(pair<int, int>(2, 20));
	m.insert(pair<int, int>(3, 30));

	if (m.empty())
	{
		cout << "m为空" << endl;
	}
	else
	{
		cout << "m不为空" << endl;
		cout << "m的大小为： " << m.size() << endl;
	}
}


//交换
void test02()
{
	map<int, int>m;
	m.insert(pair<int, int>(1, 10));
	m.insert(pair<int, int>(2, 20));
	m.insert(pair<int, int>(3, 30));

	map<int, int>m2;
	m2.insert(pair<int, int>(4, 100));
	m2.insert(pair<int, int>(5, 200));
	m2.insert(pair<int, int>(6, 300));

	cout << "交换前" << endl;
	printMap(m);
	printMap(m2);

	cout << "交换后" << endl;
	m.swap(m2);
	printMap(m);
	printMap(m2);
}

int main() {

	test01();

	test02();

	system("pause");

	return 0;
}
```

总结：

- 统计大小   --- size
- 判断是否为空   --- empty
- 交换容器   --- swap











#### 3.4.4 map插入和删除

**功能描述：**

- map容器进行插入数据和删除数据





**函数原型：**

- `insert(elem);`           //在容器中插入元素。
- `clear();`                    //清除所有元素
- `erase(pos);`              //删除pos迭代器所指的元素，返回下一个元素的迭代器。
- `erase(beg, end);`    //删除区间[beg,end)的所有元素 ，返回下一个元素的迭代器。
- `erase(key);`            //删除容器中值为key的元素。



**示例：**

```C++
#include <map>

void printMap(map<int,int>&m)
{
	for (map<int, int>::iterator it = m.begin(); it != m.end(); it++)
	{
		cout << "key = " << it->first << " value = " << it->second << endl;
	}
	cout << endl;
}

void test01()
{
	//插入
	map<int, int> m;
	//第一种插入方式
	m.insert(pair<int, int>(1, 10));
	//第二种插入方式
	m.insert(make_pair(2, 20));
	//第三种插入方式
	m.insert(map<int, int>::value_type(3, 30));
	//第四种插入方式
	m[4] = 40; 
	printMap(m);

	//删除
	m.erase(m.begin());
	printMap(m);

	m.erase(3);
	printMap(m);

	//清空
	m.erase(m.begin(),m.end());
	m.clear();
	printMap(m);
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

* map插入方式很多，记住其一即可

- 插入   --- insert 
- 删除   --- erase
- 清空   --- clear













#### 3.4.5 map查找和统计

**功能描述：**

- 对map容器进行查找数据以及统计数据



**函数原型：**

- `find(key);`                  //查找key是否存在,若存在，返回该键的元素的迭代器；若不存在，返回set.end();
- `count(key);`                //统计key的元素个数



**示例：**

```C++
#include <map>

//查找和统计
void test01()
{
	map<int, int>m; 
	m.insert(pair<int, int>(1, 10));
	m.insert(pair<int, int>(2, 20));
	m.insert(pair<int, int>(3, 30));

	//查找
	map<int, int>::iterator pos = m.find(3);		//用一个迭代器去接收查找的数据

	if (pos != m.end())
	{
		cout << "找到了元素 key = " << (*pos).first << " value = " << (*pos).second << endl;
	}
	else
	{
		cout << "未找到元素" << endl;
	}

	//统计
	int num = m.count(3);
	cout << "num = " << num << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

- 查找   ---  find    （返回的是迭代器）
- 统计   ---  count  （对于map，结果为0或者1）















#### 3.4.6 map容器排序

**学习目标：**

- map容器默认排序规则为 按照key值进行 从小到大排序，掌握如何改变排序规则



**主要技术点:**

- 利用仿函数，可以改变排序规则



**示例：**

```C++
#include <map>

class MyCompare {
public:
	bool operator()(int v1, int v2) {
		return v1 > v2;
	}
};

void test01() 
{
	//默认从小到大排序
	//利用仿函数实现从大到小排序
	map<int, int, MyCompare> m;		//在创建时就改变排序规则，因为当map容器插完时，已经默认排好序了

	m.insert(make_pair(1, 10));
	m.insert(make_pair(2, 20));
	m.insert(make_pair(3, 30));
	m.insert(make_pair(4, 40));
	m.insert(make_pair(5, 50));

	for (map<int, int, MyCompare>::iterator it = m.begin(); it != m.end(); it++) {
		cout << "key:" << it->first << " value:" << it->second << endl;
	}
}
int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

* 利用仿函数可以指定map容器的排序规则
* 对于自定义数据类型，map必须要指定排序规则,同set容器





### 3.10 案例-员工分组

#### 3.10.1 案例描述

* 公司今天招聘了10个员工（ABCDEFGHIJ），10名员工进入公司之后，需要指派员工在那个部门工作
* 员工信息有: 姓名  工资组成；部门分为：策划、美术、研发
* 随机给10名员工分配部门和工资
* 通过multimap进行信息的插入  key(部门编号) value(员工)
* 分部门显示员工信息

#### 3.10.2 实现步骤

1. 创建10名员工，放到vector中
2. 遍历vector容器，取出每个员工，进行随机分组
3. 分组后，将员工部门编号作为key，具体员工作为value，放入到multimap容器中
4. 分部门显示员工信息

**案例代码：**

```C++
#include<iostream>
using namespace std;
#include <vector>
#include <string>
#include <map>
#include <ctime>

/*
- 公司今天招聘了10个员工（ABCDEFGHIJ），10名员工进入公司之后，需要指派员工在那个部门工作
- 员工信息有: 姓名  工资组成；部门分为：策划、美术、研发
- 随机给10名员工分配部门和工资
- 通过multimap进行信息的插入  key(部门编号) value(员工)
- 分部门显示员工信息
*/

#define CEHUA  0
#define MEISHU 1
#define YANFA  2

class Worker
{
public:
	string m_Name;
	int m_Salary;
};

void createWorker(vector<Worker>&v)
{
	string nameSeed = "ABCDEFGHIJ";
	for (int i = 0; i < 10; i++)
	{
		Worker worker;
		worker.m_Name = "员工";
		worker.m_Name += nameSeed[i];

		worker.m_Salary = rand() % 10000 + 10000; // 10000 ~ 19999
		//将员工放入到容器中
		v.push_back(worker);
	}
}

//员工分组
void setGroup(vector<Worker>&v,multimap<int,Worker>&m)
{
	for (vector<Worker>::iterator it = v.begin(); it != v.end(); it++)
	{
		//产生随机部门编号
		int deptId = rand() % 3; // 0 1 2 

		//将员工插入到分组中
		//key部门编号，value具体员工
		m.insert(make_pair(deptId, *it));
	}
}

void showWorkerByGourp(multimap<int,Worker>&m)
{
	// 0  A  B  C   1  D  E   2  F G ...
	cout << "策划部门：" << endl;

	multimap<int,Worker>::iterator pos = m.find(CEHUA);
	int count = m.count(CEHUA); // 统计策划部门具体人数
	int index = 0;
	for (; pos != m.end() && index < count; pos++ , index++)
	{
		cout << "姓名： " << pos->second.m_Name << " 工资： " << pos->second.m_Salary << endl;
	}

	cout << "----------------------" << endl;
	cout << "美术部门： " << endl;
	pos = m.find(MEISHU);
	count = m.count(MEISHU); // 统计具体人数
	index = 0;
	for (; pos != m.end() && index < count; pos++, index++)
	{
		cout << "姓名： " << pos->second.m_Name << " 工资： " << pos->second.m_Salary << endl;
	}

	cout << "----------------------" << endl;
	cout << "研发部门： " << endl;
	pos = m.find(YANFA);
	count = m.count(YANFA); // 统计具体人数
	index = 0;
	for (; pos != m.end() && index < count; pos++, index++)
	{
		cout << "姓名： " << pos->second.m_Name << " 工资： " << pos->second.m_Salary << endl;
	}

}

int main() {

	srand((unsigned int)time(NULL));	//按时间随机

	//1、创建员工
	vector<Worker>vWorker;
	createWorker(vWorker);

	//2、员工分组
	multimap<int, Worker>mWorker;
	setGroup(vWorker, mWorker);


	//3、分组显示员工
	showWorkerByGourp(mWorker);

	////测试
	//for (vector<Worker>::iterator it = vWorker.begin(); it != vWorker.end(); it++)
	//{
	//	cout << "姓名： " << it->m_Name << " 工资： " << it->m_Salary << endl;
	//}

	system("pause");

	return 0;
}
```

总结：

* 当数据以键值对形式存在，可以考虑用map 或 multimap







## 4、 STL- 函数对象

### 4.1 函数对象

#### 4.1.1 函数对象概念

**概念：**

* 重载**函数调用操作符**的类，其对象常称为**函数对象**
* **函数对象**使用重载的()时，行为类似函数调用，也叫**仿函数**

**本质：**

函数对象(仿函数)是一个**类**，不是一个函数



#### 4.1.2  函数对象使用

**特点：**

* 函数对象在使用时，可以像普通函数那样调用, 可以有参数，可以有返回值
* 函数对象超出普通函数的概念，函数对象可以有自己的状态
* 函数对象可以作为参数传递

**示例:**

```C++
#include <string>

//1、函数对象在使用时，可以像普通函数那样调用, 可以有参数，可以有返回值
class MyAdd
{
public :
	int operator()(int v1,int v2)
	{
		return v1 + v2;
	}
};

void test01()
{
	MyAdd myAdd;
	cout << myAdd(10, 10) << endl;
}

//2、函数对象可以有自己的状态
class MyPrint
{
public:
	MyPrint()
	{
		count = 0;
	}
	void operator()(string test)
	{
		cout << test << endl;
		count++; //统计使用次数
	}

	int count; //内部自己的状态
};
void test02()
{
	MyPrint myPrint;
	myPrint("hello world");
	myPrint("hello world");
	myPrint("hello world");
	cout << "myPrint调用次数为： " << myPrint.count << endl;
}

//3、函数对象可以作为参数传递
void doPrint(MyPrint &mp , string test)
{
	mp(test);
}

void test03()
{
	MyPrint myPrint;
	doPrint(myPrint, "Hello C++");
}

int main() {

	//test01();
	//test02();
	test03();

	system("pause");

	return 0;
}
```

总结：

* 仿函数写法非常灵活，可以作为参数进行传递。







### 4.2  谓词

#### 4.2.1 谓词概念

**概念：**

* 返回bool类型的仿函数称为**谓词**
* 如果operator()接受一个参数，那么叫做一元谓词
* 如果operator()接受两个参数，那么叫做二元谓词



#### 4.2.2 一元谓词

**示例：**

```C++
#include <vector>
#include <algorithm>

//1.一元谓词
struct GreaterFive{
	bool operator()(int val) {
		return val > 5;
	}
};

void test01() {

	vector<int> v;
	for (int i = 0; i < 10; i++)
	{
		v.push_back(i);
	}

	vector<int>::iterator it = find_if(v.begin(), v.end(), GreaterFive());
	if (it == v.end()) {
		cout << "没找到!" << endl;
	}
	else {
		cout << "找到:" << *it << endl;		//找到：6
	}

}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：参数只有一个的谓词，称为一元谓词



#### 4.2.3 二元谓词

**示例：**

```C++
#include <vector>
#include <algorithm>
//二元谓词
class MyCompare
{
public:
	bool operator()(int num1, int num2)
	{
		return num1 > num2;
	}
};

void test01()
{
	vector<int> v;
	v.push_back(10);
	v.push_back(40);
	v.push_back(20);
	v.push_back(30);
	v.push_back(50);

	//默认从小到大
	sort(v.begin(), v.end());
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
	cout << "----------------------------" << endl;

	//使用函数对象改变算法策略，排序从大到小
	sort(v.begin(), v.end(), MyCompare());
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：参数只有两个的谓词，称为二元谓词





### 4.3 内建函数对象

#### 4.3.1 内建函数对象意义

**概念：**

* STL内建了一些函数对象

**分类:**

* 算术仿函数

* 关系仿函数

* 逻辑仿函数

**用法：**

* 这些仿函数所产生的对象，用法和一般函数完全相同
* 使用内建函数对象，需要引入头文件 `#include<functional>`





#### 4.3.2 算术仿函数

**功能描述：**

* 实现四则运算
* 其中negate是一元运算，其他都是二元运算



**仿函数原型：**

* `template<class T> T plus<T>`                //加法仿函数
* `template<class T> T minus<T>`              //减法仿函数
* `template<class T> T multiplies<T>`    //乘法仿函数
* `template<class T> T divides<T>`         //除法仿函数
* `template<class T> T modulus<T>`         //取模仿函数
* `template<class T> T negate<T>`           //取反仿函数



**示例：**

```C++
#include <functional>
//negate
void test01()
{
	negate<int> n;
	cout << n(50) << endl;
}

//plus
void test02()
{
	plus<int> p;
	cout << p(10, 20) << endl;
}

int main() {

	test01();
	test02();

	system("pause");

	return 0;
}
```

总结：使用内建函数对象时，需要引入头文件 `#include <functional>`



#### 4.3.3 关系仿函数

**功能描述：**

- 实现关系对比



**仿函数原型：**

* `template<class T> bool equal_to<T>`                    //等于
* `template<class T> bool not_equal_to<T>`            //不等于
* `template<class T> bool greater<T>`                      //大于
* `template<class T> bool greater_equal<T>`          //大于等于
* `template<class T> bool less<T>`                           //小于
* `template<class T> bool less_equal<T>`               //小于等于



**示例：**

```C++
#include <functional>
#include <vector>
#include <algorithm>

class MyCompare
{
public:
	bool operator()(int v1,int v2)
	{
		return v1 > v2;
	}
};
void test01()
{
	vector<int> v;

	v.push_back(10);
	v.push_back(30);
	v.push_back(50);
	v.push_back(40);
	v.push_back(20);

	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;

	//自己实现仿函数
	//sort(v.begin(), v.end(), MyCompare());
	//STL内建仿函数  大于仿函数
	sort(v.begin(), v.end(), greater<int>());

	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：关系仿函数中最常用的就是greater<>大于









## 5、 STL- 常用算法



**概述**:

* 算法主要是由头文件`<algorithm>` `<functional>` `<numeric>`组成。



* `<algorithm>`是所有STL头文件中最大的一个，范围涉及到比较、 交换、查找、遍历操作、复制、修改等等
* `<numeric>`体积很小，只包括几个在序列上面进行简单数学运算的模板函数
* `<functional>`定义了一些模板类,用以声明函数对象。





### 5.1 常用遍历算法

**学习目标：**

* 掌握常用的遍历算法



**算法简介：**

* `for_each`     //遍历容器
* `transform`   //搬运容器到另一个容器中



#### 5.1.1 for_each

**功能描述：**

* 实现遍历容器

**函数原型：**

* `for_each(iterator beg, iterator end, _func);  `

  // 遍历算法 遍历容器元素

  // beg 开始迭代器

  // end 结束迭代器

  // _func 函数或者函数对象

**示例：**

```C++
#include <algorithm>
#include <vector>

//普通函数
void print01(int val) 
{
	cout << val << " ";
}
//函数对象
class print02 
{
 public:
	void operator()(int val) 
	{
		cout << val << " ";
	}
};

//for_each算法基本用法
void test01() {

	vector<int> v;
	for (int i = 0; i < 10; i++) 
	{
		v.push_back(i);
	}

	//遍历算法
	for_each(v.begin(), v.end(), print01);
	cout << endl;

	for_each(v.begin(), v.end(), print02());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

**总结：**for_each在实际开发中是最常用遍历算法，需要熟练掌握



#### 5.1.2 transform

**功能描述：**

* 搬运容器到另一个容器中

**函数原型：**

* `transform(iterator beg1, iterator end1, iterator beg2, _func);`

//beg1 源容器开始迭代器

//end1 源容器结束迭代器

//beg2 目标容器开始迭代器

//_func 函数或者函数对象

**示例：**

```C++
#include<vector>
#include<algorithm>

//常用遍历算法  搬运 transform

class TransForm
{
public:
	int operator()(int val)
	{
		return val;
	}

};

class MyPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int>v;
	for (int i = 0; i < 10; i++)
	{
		v.push_back(i);
	}

	vector<int>vTarget; //目标容器

	vTarget.resize(v.size()); // 目标容器需要提前开辟空间

	transform(v.begin(), v.end(), vTarget.begin(), TransForm());

	for_each(vTarget.begin(), vTarget.end(), MyPrint());
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

**总结：** 搬运的目标容器必须要提前开辟空间，否则无法正常搬运



### 5.2 常用查找算法

学习目标：

- 掌握常用的查找算法

**算法简介：**

- `find`                     //查找元素
- `find_if`               //按条件查找元素
- `adjacent_find`    //查找相邻重复元素
- `binary_search`    //二分查找法
- `count`                   //统计元素个数
- `count_if`             //按条件统计元素个数




#### 5.2.1 find

**功能描述：**

* 查找指定元素，找到返回指定元素的迭代器，找不到返回结束迭代器end()



**函数原型：**

- `find(iterator beg, iterator end, value);  `

  // 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置

  // beg 开始迭代器

  // end 结束迭代器

  // value 查找的元素





**示例：**

```C++
#include <algorithm>
#include <vector>
#include <string>
void test01() {

	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i + 1);
	}
	//查找容器中是否有 5 这个元素
	vector<int>::iterator it = find(v.begin(), v.end(), 5);
	if (it == v.end()) 
	{
		cout << "没有找到!" << endl;
	}
	else 
	{
		cout << "找到:" << *it << endl;
	}
}

class Person {
public:
	Person(string name, int age) 
	{
		this->m_Name = name;
		this->m_Age = age;
	}
	//重载==
	bool operator==(const Person& p) 
	{
		if (this->m_Name == p.m_Name && this->m_Age == p.m_Age) 
		{
			return true;
		}
		return false;
	}

public:
	string m_Name;
	int m_Age;
};

void test02() {

	vector<Person> v;

	//创建数据
	Person p1("aaa", 10);
	Person p2("bbb", 20);
	Person p3("ccc", 30);
	Person p4("ddd", 40);

	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);

	vector<Person>::iterator it = find(v.begin(), v.end(), p2);
	if (it == v.end()) 
	{
		cout << "没有找到!" << endl;
	}
	else 
	{
		cout << "找到姓名:" << it->m_Name << " 年龄: " << it->m_Age << endl;
	}
}
```

总结： 利用find可以在容器中找指定的元素，返回值是**迭代器**













#### 5.2.2 find_if

**功能描述：**

* 按条件查找元素

**函数原型：**

- `find_if(iterator beg, iterator end, _Pred);  `

  // 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置

  // beg 开始迭代器

  // end 结束迭代器

  // _Pred 函数或者谓词（返回bool类型的仿函数）



**示例：**

```C++
#include <algorithm>
#include <vector>
#include <string>

//内置数据类型
class GreaterFive
{
public:
	bool operator()(int val)
	{
		return val > 5;
	}
};

void test01() {

	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i + 1);
	}

	vector<int>::iterator it = find_if(v.begin(), v.end(), GreaterFive());
	if (it == v.end()) {
		cout << "没有找到!" << endl;
	}
	else {
		cout << "找到大于5的数字:" << *it << endl;
	}
}

//自定义数据类型
class Person {
public:
	Person(string name, int age)
	{
		this->m_Name = name;
		this->m_Age = age;
	}
public:
	string m_Name;
	int m_Age;
};

class Greater20
{
public:
	bool operator()(Person &p)
	{
		return p.m_Age > 20;
	}

};

void test02() {

	vector<Person> v;

	//创建数据
	Person p1("aaa", 10);
	Person p2("bbb", 20);
	Person p3("ccc", 30);
	Person p4("ddd", 40);

	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);

	vector<Person>::iterator it = find_if(v.begin(), v.end(), Greater20());
	if (it == v.end())
		cout << "未找到！" << endl;
	else
		cout << "找到年龄大于20岁的人为  姓名：" << it->m_Name << "  年龄：" << it->m_Age << endl;
	cout << endl;

	//上面方法只能输出第一个年龄大于20岁的人，下面的方法可以输出全部年龄大于20岁的人
	cout << "年龄大于20岁的人有：" << endl;
	for (vector<Person>::iterator iter = v.begin(); iter != v.end(); iter++)	//迭代器iter确定查找的区间
	{
		vector<Person>::iterator it2 = find_if(iter, v.end(), Greater20());		//迭代器it2用于查找区间内满足条件的数据
		if (it2 != v.end())
		{
			cout << "姓名：" << it2->m_Name << "  年龄：" << it2->m_Age << endl;
			iter = it2;		//更新iter的起始位置为当前符合条件的位置，当前循环走完后iter++后，iter的位置就是当前符合条件位置的后一个位置
		}
	}
}

int main() {

	//test01();

	test02();

	system("pause");

	return 0;
}
```

总结：find_if按条件查找使查找更加灵活，提供的仿函数可以改变不同的策略















#### 5.2.3 adjacent_find

**功能描述：**

* 查找相邻重复元素



**函数原型：**

- `adjacent_find(iterator beg, iterator end);  `

  // 查找相邻重复元素,返回相邻元素的第一个位置的迭代器

  // beg 开始迭代器

  // end 结束迭代器

  



**示例：**

```C++
#include <algorithm>
#include <vector>

void test01()
{
	vector<int> v;
	v.push_back(1);
	v.push_back(2);
	v.push_back(5);
	v.push_back(2);
	v.push_back(4);
	v.push_back(4);
	v.push_back(3);

	//查找相邻重复元素
	vector<int>::iterator it = adjacent_find(v.begin(), v.end());
	if (it == v.end()) {
		cout << "找不到!" << endl;
	}
	else {
		cout << "找到相邻重复元素为:" << *it << endl;
	}
}
```

总结：面试题中如果出现查找相邻重复元素，记得用STL中的adjacent_find算法









#### 5.2.4 binary_search

**功能描述：**

* 查找指定元素是否存在



**函数原型：**

- `bool binary_search(iterator beg, iterator end, value);  `

  // 查找指定的元素，查到 返回true  否则false

  // 注意: 在**无序序列中不可用**

  // beg 开始迭代器

  // end 结束迭代器

  // value 查找的元素





**示例：**

```C++
#include <algorithm>
#include <vector>

void test01()
{
	vector<int>v;

	for (int i = 0; i < 10; i++)
	{
		v.push_back(i);
	}
	//二分查找
	bool ret = binary_search(v.begin(), v.end(),2);
	if (ret)
	{
		cout << "找到了" << endl;
	}
	else
	{
		cout << "未找到" << endl;
	}
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

**总结：**二分查找法查找效率很高，值得注意的是查找的容器中元素必须的有序序列









#### 5.2.5 count

**功能描述：**

* 统计元素个数



**函数原型：**

- `count(iterator beg, iterator end, value);  `

  // 统计元素出现次数

  // beg 开始迭代器

  // end 结束迭代器

  // value 统计的元素





**示例：**

```C++
#include <algorithm>
#include <vector>

//内置数据类型
void test01()
{
	vector<int> v;
	v.push_back(1);
	v.push_back(2);
	v.push_back(4);
	v.push_back(5);
	v.push_back(3);
	v.push_back(4);
	v.push_back(4);

	int num = count(v.begin(), v.end(), 4);

	cout << "4的个数为： " << num << endl;
}

//自定义数据类型
class Person
{
public:
	Person(string name, int age)
	{
		this->m_Name = name;
		this->m_Age = age;
	}
	bool operator==(const Person & p)
	{
		if (this->m_Age == p.m_Age)		//统计年龄相同的
		{
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

void test02()
{
	vector<Person> v;

	Person p1("刘备", 35);
	Person p2("关羽", 35);
	Person p3("张飞", 35);
	Person p4("赵云", 30);
	Person p5("曹操", 25);

	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);
	v.push_back(p5);
    
    Person p("诸葛亮",35);

	int num = count(v.begin(), v.end(), p);	
	cout << "num = " << num << endl;
}
int main() {

	//test01();

	test02();

	system("pause");

	return 0;
}
```

**总结：** 统计自定义数据类型时候，需要配合重载 `operator==`

















#### 5.2.6 count_if

**功能描述：**

* 按条件统计元素个数

**函数原型：**

- `count_if(iterator beg, iterator end, _Pred);  `

  // 按条件统计元素出现次数

  // beg 开始迭代器

  // end 结束迭代器

  // _Pred 谓词

  

**示例：**

```C++
#include <algorithm>
#include <vector>

class Greater4
{
public:
	bool operator()(int val)
	{
		return val >= 4;
	}
};

//内置数据类型
void test01()
{
	vector<int> v;
	v.push_back(1);
	v.push_back(2);
	v.push_back(4);
	v.push_back(5);
	v.push_back(3);
	v.push_back(4);
	v.push_back(4);

	int num = count_if(v.begin(), v.end(), Greater4());

	cout << "大于4的个数为： " << num << endl;
}

//自定义数据类型
class Person
{
public:
	Person(string name, int age)
	{
		this->m_Name = name;
		this->m_Age = age;
	}

	string m_Name;
	int m_Age;
};

class AgeLess35
{
public:
	bool operator()(const Person &p)
	{
		return p.m_Age < 35;
	}
};
void test02()
{
	vector<Person> v;

	Person p1("刘备", 35);
	Person p2("关羽", 35);
	Person p3("张飞", 35);
	Person p4("赵云", 30);
	Person p5("曹操", 25);

	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);
	v.push_back(p5);

	int num = count_if(v.begin(), v.end(), AgeLess35());
	cout << "小于35岁的个数：" << num << endl;
}


int main() {

	//test01();

	test02();

	system("pause");

	return 0;
}
```

**总结：**按值统计用count，按条件统计用count_if



### 5.3 常用排序算法

**学习目标：**

- 掌握常用的排序算法

**算法简介：**

- `sort`             //对容器内元素进行排序
- `random_shuffle`   //洗牌   指定范围内的元素随机调整次序
- `merge `           // 容器元素合并，并存储到另一容器中
- `reverse`       // 反转指定范围的元素





#### 5.3.1 sort

**功能描述：**

* 对容器内元素进行排序





**函数原型：**

- `sort(iterator beg, iterator end, _Pred);  `

  // 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置

  //  beg    开始迭代器

  //  end    结束迭代器

  // _Pred  谓词





**示例：**

```c++
#include <algorithm>
#include <vector>
#include<functional>

void myPrint(int val)
{
	cout << val << " ";
}

void test01() {
	vector<int> v;
	v.push_back(10);
	v.push_back(30);
	v.push_back(50);
	v.push_back(20);
	v.push_back(40);

	//sort默认从小到大排序
	sort(v.begin(), v.end());
	for_each(v.begin(), v.end(), myPrint);
	cout << endl;

	//从大到小排序
	sort(v.begin(), v.end(), greater<int>()); 		//需要包含头文件#include<functional>  因为用了				greater<int>()，内置类型的由大到小
	for_each(v.begin(), v.end(), myPrint);
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

**总结：**sort属于开发中最常用的算法之一，需熟练掌握













#### 5.3.2 random_shuffle

**功能描述：**

* 洗牌   指定范围内的元素随机调整次序



**函数原型：**

- `random_shuffle(iterator beg, iterator end);  `

  // 指定范围内的元素随机调整次序

  // beg 开始迭代器

  // end 结束迭代器

  

**示例：**

```c++
#include <algorithm>
#include <vector>
#include <ctime>		//包含时间和日期的库

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	srand((unsigned int)time(NULL));
	vector<int> v;
	for(int i = 0 ; i < 10;i++)
	{
		v.push_back(i);
	}
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;

	//打乱顺序
	random_shuffle(v.begin(), v.end());
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

**总结：**random_shuffle洗牌算法比较实用，使用时记得加随机数种子















#### 5.3.3 merge

**功能描述：**

* 两个容器元素合并，并存储到另一容器中



**函数原型：**

- `merge(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);  `

  // 容器元素合并，并存储到另一容器中

  // 注意: 两个容器必须是**有序的**

  // beg1   容器1开始迭代器
  // end1   容器1结束迭代器
  // beg2   容器2开始迭代器
  // end2   容器2结束迭代器
  // dest    目标容器开始迭代器

  

**示例：**

```c++
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 10 ; i++) 
    {
		v1.push_back(i);
		v2.push_back(i + 1);
	}

	vector<int> vtarget;
	//目标容器需要提前开辟空间
	vtarget.resize(v1.size() + v2.size());
	//合并  需要两个有序序列
	merge(v1.begin(), v1.end(), v2.begin(), v2.end(), vtarget.begin());
	for_each(vtarget.begin(), vtarget.end(), myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

**总结：**merge合并的两个容器必须的有序序列











#### 5.3.4 reverse

**功能描述：**

* 将容器内元素进行反转



**函数原型：**

- `reverse(iterator beg, iterator end);  `

  // 反转指定范围的元素

  // beg 开始迭代器

  // end 结束迭代器

  

**示例：**

```c++
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v;
	v.push_back(10);
	v.push_back(30);
	v.push_back(50);
	v.push_back(20);
	v.push_back(40);

	cout << "反转前： " << endl;
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;

	cout << "反转后： " << endl;

	reverse(v.begin(), v.end());
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

**总结：**reverse反转区间内元素，面试题可能涉及到



### 5.4 常用拷贝和替换算法

**学习目标：**

- 掌握常用的拷贝和替换算法

**算法简介：**

- `copy`                      // 容器内指定范围的元素拷贝到另一容器中
- `replace`                // 将容器内指定范围的旧元素修改为新元素
- `replace_if `          // 容器内指定范围满足条件的元素替换为新元素
- `swap`                     // 互换两个容器的元素




#### 5.4.1 copy

**功能描述：**

* 容器内指定范围的元素拷贝到另一容器中



**函数原型：**

- `copy(iterator beg, iterator end, iterator dest);  `

  // 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置

  // beg  开始迭代器

  // end  结束迭代器

  // dest 目标起始迭代器



**示例：**

```c++
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i + 1);
	}
	vector<int> v2;
	v2.resize(v1.size());		//提前开辟v2的空间
	copy(v1.begin(), v1.end(), v2.begin());

	for_each(v2.begin(), v2.end(), myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

**总结：**利用copy算法在拷贝时，目标容器记得提前开辟空间















#### 5.4.2 replace

**功能描述：**

* 将容器内指定范围的旧元素修改为新元素



**函数原型：**

- `replace(iterator beg, iterator end, oldvalue, newvalue);  `

  // 将区间内旧元素 替换成 新元素

  // beg 开始迭代器

  // end 结束迭代器

  // oldvalue 旧元素

  // newvalue 新元素



**示例：**

```c++
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v;
	v.push_back(20);
	v.push_back(30);
	v.push_back(20);
	v.push_back(40);
	v.push_back(50);
	v.push_back(10);
	v.push_back(20);

	cout << "替换前：" << endl;
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;

	//将容器中的20 替换成 2000
	cout << "替换后：" << endl;
	replace(v.begin(), v.end(), 20,2000);
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

**总结：**replace会替换区间内满足条件的元素













#### 5.4.3 replace_if

**功能描述:**  

* 将区间内满足条件的元素，替换成指定元素



**函数原型：**

- `replace_if(iterator beg, iterator end, _pred, newvalue);  `

  // 按条件替换元素，满足条件的替换成指定元素

  // beg 开始迭代器

  // end 结束迭代器

  // _pred 谓词

  // newvalue 替换的新元素



**示例：**

```c++
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

class ReplaceGreater30
{
public:
	bool operator()(int val)
	{
		return val >= 30;
	}

};

void test01()
{
	vector<int> v;
	v.push_back(20);
	v.push_back(30);
	v.push_back(20);
	v.push_back(40);
	v.push_back(50);
	v.push_back(10);
	v.push_back(20);

	cout << "替换前：" << endl;
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;

	//将容器中大于等于的30 替换成 3000
	cout << "替换后：" << endl;
	replace_if(v.begin(), v.end(), ReplaceGreater30(), 3000);
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

**总结：**replace_if按条件查找，可以利用仿函数灵活筛选满足的条件







#### 5.4.4 swap

**功能描述：**

* 互换两个容器的元素



**函数原型：**

- `swap(container c1, container c2);  `

  // 互换两个容器的元素

  // c1容器1

  // c2容器2

  

**示例：**

```c++
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i);
		v2.push_back(i+100);
	}

	cout << "交换前： " << endl;
	for_each(v1.begin(), v1.end(), myPrint());
	cout << endl;
	for_each(v2.begin(), v2.end(), myPrint());
	cout << endl;

	cout << "交换后： " << endl;
	swap(v1, v2);
	for_each(v1.begin(), v1.end(), myPrint());
	cout << endl;
	for_each(v2.begin(), v2.end(), myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

**总结：**swap交换容器时，注意交换的容器要同种类型



### 5.6 常用集合算法

**学习目标：**

- 掌握常用的集合算法



**算法简介：**

- `set_intersection`          // 求两个容器的交集

- `set_union`                       // 求两个容器的并集

- `set_difference `              // 求两个容器的差集

  



#### 5.6.1 set_intersection

**功能描述：**

* 求两个容器的交集



**函数原型：**

- `set_intersection(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);  `

  // 求两个集合的交集

  // **注意:两个集合必须是有序序列**

  // beg1 容器1开始迭代器
  // end1 容器1结束迭代器
  // beg2 容器2开始迭代器
  // end2 容器2结束迭代器
  // dest 目标容器开始迭代器



**示例：**

```C++
#include <vector>
#include <algorithm>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 10; i++)
    {
		v1.push_back(i);
		v2.push_back(i+5);
	}

	vector<int> vTarget;
	//取两个里面较小的值给目标容器开辟空间
	vTarget.resize(min(v1.size(), v2.size()));

	//返回记录目标容器的最后一个元素的迭代器地址
	vector<int>::iterator itEnd = 
        set_intersection(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());		//求完交集后，返回的是最后一个交集元素的迭代器地址，所以用一个itEnd来接受一下，不然用for_each遍历输出的时候会把vTarget容器里所有值输出来，若v1和v2的交集不满10个，则后面全部输出0

	for_each(vTarget.begin(), itEnd, myPrint());	//输出5 6 7 8 9	
        //这里结束迭代器地址用itEnd，若用vTarget.end() 则会输出5 6 7 8 9 0 0 0 0 0 
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

**总结：** 

* 求交集的两个集合必须的有序序列
* 目标容器开辟空间需要从**两个容器中取小值**
* set_intersection返回值既是交集中最后一个元素的位置













#### 5.6.2 set_union

**功能描述：**

* 求两个集合的并集



**函数原型：**

- `set_union(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);  `

  // 求两个集合的并集

  // **注意:两个集合必须是有序序列**

  // beg1 容器1开始迭代器
  // end1 容器1结束迭代器
  // beg2 容器2开始迭代器
  // end2 容器2结束迭代器
  // dest 目标容器开始迭代器

  

**示例：**

```C++
#include <vector>
#include <algorithm>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i);
		v2.push_back(i+5);
	}

	vector<int> vTarget;
	//取两个容器的和给目标容器开辟空间
	vTarget.resize(v1.size() + v2.size());

	//返回目标容器的最后一个元素的迭代器地址
	vector<int>::iterator itEnd = 
        set_union(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());

	for_each(vTarget.begin(), itEnd, myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

**总结：** 

- 求并集的两个集合必须的有序序列
- 目标容器开辟空间需要**两个容器相加**
- set_union返回值既是并集中最后一个元素的位置








#### 5.6.3  set_difference

**功能描述：**

* 求两个集合的差集



**函数原型：**

- `set_difference(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);  `

  // 求两个集合的差集

  // **注意:两个集合必须是有序序列**

  // beg1 容器1开始迭代器
  // end1 容器1结束迭代器
  // beg2 容器2开始迭代器
  // end2 容器2结束迭代器
  // dest 目标容器开始迭代器

  **差集分前后，是相对的**。例如：v1: 1,2,3。 v2: 3,4,5。     v1和v2的差集是1,2 。     v2和v1的差集是4,5

**示例：**

```C++
#include <vector>
#include <algorithm>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i);
		v2.push_back(i+5);
	}

	vector<int> vTarget;
	//取两个里面较大的值给目标容器开辟空间
	vTarget.resize( max(v1.size() , v2.size()));

	//返回目标容器的最后一个元素的迭代器地址
	cout << "v1与v2的差集为： " << endl;
	vector<int>::iterator itEnd = 
        set_difference(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());
	for_each(vTarget.begin(), itEnd, myPrint());
	cout << endl;


	cout << "v2与v1的差集为： " << endl;
	itEnd = set_difference(v2.begin(), v2.end(), v1.begin(), v1.end(), vTarget.begin());
	for_each(vTarget.begin(), itEnd, myPrint());
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

**总结：** 

- 求差集的两个集合必须的有序序列
- 目标容器开辟空间需要从**两个容器取较大值**
- set_difference返回值既是差集中最后一个元素的位置





# 职工管理系统

需求：

职工管理系统可以用来管理公司内所有员工的信息

主要利用C++来实现一个基于多态的职工管理系统



公司中职工分为三类：普通员工、经理、老板，显示信息时，需要显示职工编号、职工姓名、职工岗位、以及职责

普通员工职责：完成经理交给的任务

经理职责：完成老板交给的任务，并下发任务给员工

老板职责：管理公司所有事务



管理系统中需要实现的功能如下：

* 退出管理程序：退出当前管理系统

- 增加职工信息：实现批量添加职工功能，将信息录入到文件中，职工信息为：职工编号、姓名、部门编号
- 显示职工信息：显示公司内部所有职工的信息
- 删除离职职工：按照编号删除指定的职工
- 修改职工信息：按照编号修改职工个人信息
- 查找职工信息：按照职工的编号或者职工的姓名进行查找相关的人员信息
- 按照编号排序：按照职工编号，进行排序，排序规则由用户指定
- 清空所有文档：清空文件中记录的所有职工信息 （清空前需要再次确认，防止误删）



## 1、创建管理类

​		管理类负责的内容如下：

- 对用户的沟通菜单界面
- 对职工增删改查的操作
- 文件的读写交互

### 1.1 创建文件

在头文件和源文件的文件夹下分别创建workerManager.h 和workerManager.cpp文件

workerManager.h

```c++
#pragma once				//防止头文件重复包含
#include<iostream>
using namespace std;
class WorkerManager
{
public:
	WorkerManager();
	~WorkerManager();
};
```

workerManager.cpp

```c++
#include"workerManager.h"

WorkerManager::WorkerManager()
{

}

WorkerManager::~WorkerManager()
{

}
```



## 2、菜单功能

功能描述：与用户沟通界面

### 2.1 添加成员函数

在管理类workerManager.h中添加成员函数`void Show_Menu();`

在workerManager.cpp中实现

```c++
void WorkerManager::Show_Menu()
{
	cout << "********************************************" << endl;
	cout << "*********  欢迎使用职工管理系统！ **********" << endl;
	cout << "*************  0.退出管理程序  *************" << endl;
	cout << "*************  1.增加职工信息  *************" << endl;
	cout << "*************  2.显示职工信息  *************" << endl;
	cout << "*************  3.删除离职职工  *************" << endl;
	cout << "*************  4.修改职工信息  *************" << endl;
	cout << "*************  5.查找职工信息  *************" << endl;
	cout << "*************  6.按照编号排序  *************" << endl;
	cout << "*************  7.清空所有文档  *************" << endl;
	cout << "********************************************" << endl;
	cout << endl;
}
```



## 3、退出功能

### 3.1 提供功能接口

在main函数中提供分支选择，提供每个功能接口

```c++
int main()
{
	WorkerManager wm;
	int choice = 0;
	while (1)
	{
		wm.Show_Menu();
		cout << "请输入您选择的功能：" << endl;
		cin >> choice;
		switch (choice)
		{
		case 0:					//退出系统	
			break;
		case 1:					//添加职工
			break;
		case 2:					//显示职工
			break;
		case 3:					//删除职工
			break;
		case 4:					//修改职工
			break;
		case 5:					//查找职工
			break;
		case 6:					//排序职工
			break;
		case 7:					//清空文件
			break;
		default:
			system("cls");
			break;
		}
	}
	system("pause");
	return 0;
}
```

### 3.2 实现退出功能

在workerManager.h中提供退出系统的成员函数`void ExitSystem();`

在workerManager.cpp中实现

```c++
void WorkerManager::ExitSystem()
{
	cout << "欢迎下次使用！" << endl;
	system("pause");
	exit(0);
}
```



## 4、创建职工类

### 4.1 创建职工抽象类

职工的分类为：普通员工、经理、老板

将三种职工抽象到一个类（worker）中,利用多态管理不同职工种类

职工的属性为：职工编号、职工姓名、职工所在部门编号

职工的行为为：岗位职责信息描述，获取岗位名称



头文件文件夹下  创建文件worker.h  文件并且添加如下代码：

```c++
#include<iostream>
using namespace std;
#include<string>

class Worker
{
public:
	int m_Id;
	string m_Name;
	int m_DeptId;

	virtual void showInfo() = 0;			//显示个人信息	
	virtual string getDeptName() = 0;		//获取岗位名称
};
```



### 4.2 创建普通员工类

普通员工类**继承**职工抽象类，并重写父类中纯虚函数



创建employee.h文件

```c++
#include<iostream>
using namespace std;
#include<string>
#include"worker.h"

class Employee :public Worker	//继承Worker类，需要重写虚函数
{
public:
	Employee(int id,string name,int dId);					//构造函数
	void showInfo();			//在.h文件中声明就好，在.cpp中实现
	string getDeptName();
};
```



创建employee.cpp文件

```c++
#include"employee.h"

Employee::Employee(int id,string name,int dId)
{
	this->m_Id = id;
	this->m_Name = name;
	this->m_DeptId = dId;
}

//显示个人信息
void Employee::showInfo()
{
	cout << "职工编号：" << this->m_Id 
		 << "\t职工姓名：" << this->m_Name 
		 << "\t岗位：" << this->getDeptName()		//调用获取岗位函数
		 << "\t岗位职责：完成经理交给的任务" << endl;
}

//获取职工岗位名称
string Employee::getDeptName()
{
	return string("员工");		//将员工强转为string类型
}
```



### 4.3 创建经理类

经理类也**继承**职工抽象类，并重写父类中纯虚函数



创建manager.h文件

```c++
#pragma once
#include<iostream>
using namespace std;
#include"worker.h"

class Manager :public Worker
{
public:
	Manager(int id, string name, int dId);
	virtual void showInfo();
	virtual string getDeptName();
};
```



创建manager.cpp文件

```c++
#pragma once
#include"manager.h"

Manager::Manager(int id, string name, int dId)
{
	this->m_Id = id;
	this->m_Name = name;
	this->m_DeptId = dId;
}
void Manager::showInfo()
{
	cout << "职工编号：" << this->m_Id
		<< "\t职工姓名：" << this->m_Name
		<< "\t岗位：" << this->getDeptName()		//调用获取岗位函数
		<< "\t岗位职责：完成老板交给的任务，并且下发任务给员工" << endl;
}
string Manager::getDeptName()
{
	return string("经理");
}
```



### 4.4 创建老板类

老板类也**继承**职工抽象类，并重写父类中纯虚函数



创建boss.h文件

```c++
#pragma once
#include<iostream>
using namespace std;
#include<string>
#include"worker.h"

class Boss :public Worker	//继承Worker类，需要重写虚函数
{
public:
	Boss(int id, string name, int dId);					//构造函数
	void showInfo();			//在.h文件中声明就好，在.cpp中实现
	string getDeptName();
};
```



创建boss.cpp文件

```c++
#include"boss.h"

Boss::Boss(int id, string name, int dId)
{
	this->m_Id = id;
	this->m_Name = name;
	this->m_DeptId = dId;
}

//显示个人信息
void Boss::showInfo()
{
	cout << "职工编号：" << this->m_Id
		 << "\t职工姓名：" << this->m_Name
		 << "\t岗位：" << this->getDeptName()		//调用获取岗位函数
		 << "\t岗位职责：发任务给经理" << endl;
}

//获取职工岗位名称
string Boss::getDeptName()
{
	return string("老板");		//将员工强转为string类型
}
```



## 5、添加职工

功能描述：批量添加职工，并且保存到文件中

### 5.1 功能分析

分析：

用户在批量创建时，可能会创建不同种类的职工

如果想将所有不同种类的员工都放入到一个数组中，可以将所有员工的指针维护到一个数组里

如果想在程序中维护这个不定长度的数组，可以将数组创建到堆区，并利用Worker **的指针维护

### 5.2 功能实现

在WokerManager.h头文件中添加成员属性 代码：

```C++
	//记录文件中的人数个数
	int m_EmpNum;

	//员工数组的指针
	Worker ** m_EmpArray;
```



在WorkerManager构造函数中初始化属性

```C++
WorkerManager::WorkerManager()
{
	//初始化人数
	this->m_EmpNum = 0;

	//初始化数组指针
	this->m_EmpArray = NULL;
}
```



在workerManager.h中添加成员函数

```C++
	//增加职工
	void Add_Emp();
```



workerManager.cpp中实现该函数

```c++
void WorkerManager::Add_Emp()
{
	cout << "请输入添加职工数量：" << endl;
	int addNum = 0;		//保存用户的输入数量
	cin >> addNum;
	if (addNum > 0)
	{
							//添加
		int newSize = this->m_EmpNum + addNum;	//计算新空间大小
		Worker** newSpace = new Worker * [newSize];		//开辟新空间
		if (this->m_EmpArray != NULL)
		{
			for (int i = 0; i < this->m_EmpNum; i++)
			{
				newSpace[i] = this->m_EmpArray[i];		//将原来空间下的数据拷贝到新空间
			}
		}
		for (int i = 0; i < addNum; i++)
		{
			int id;
			string name;
			int dSelect;
			cout << "请输入第" << i + 1 << "个新职工编号：" << endl;
			cin >> id;
			cout << "请输入第 " << i + 1 << " 个新职工姓名：" << endl;
			cin >> name;
			cout << "请选择该职工的岗位：" << endl;
			cout << "1、普通职工" << endl;
			cout << "2、经理" << endl;
			cout << "3、老板" << endl;
			cin >> dSelect;
			Worker* worker = NULL;
			switch (dSelect)
			{
			case 1: //普通员工
				worker = new Employee(id, name, 1);
				break;
			case 2: //经理
				worker = new Manager(id, name, 2);
				break;
			case 3:  //老板
				worker = new Boss(id, name, 3);
				break;
			default:
				break;
			}
			newSpace[this->m_EmpNum + i] = worker;		//将创建职工职责，保存到数组中

		}
		delete[] this->m_EmpArray;			//释放原有空间

		this->m_EmpArray = newSpace;		//更改新空间的指向

		this->m_EmpNum = newSize;			//更新职工的个数

		cout << "成功添加" << addNum << "名新职工！" << endl;
	}
	else
	{
		cout << "输入有误" << endl;
	}
	system("pause");
	system("cls");
}
```



在WorkerManager.cpp的析构函数中，释放堆区数据

```C++
WorkerManager::~WorkerManager()
{
	if (this->m_EmpArray != NULL)
	{
		delete[] this->m_EmpArray;
        this->m_EmpArray = NULL;
	}
}

```



## 6、文件交互 --- 写文件

功能描述：对文件进行读写

​	在上一个添加功能中，我们只是将所有的数据添加到了内存中，一旦程序结束就无法保存了

​	因此文件管理类中需要一个与文件进行交互的功能，对于文件进行读写操作

### 6.1 设定文件路径

首先我们将文件路径，在workerManager.h中添加宏常量,并且包含头文件 fstream

```C++
#include <fstream>
#define  FILENAME "empFile.txt"
```



### 6.2 成员函数声明

在workerManager.h中类里添加成员函数 `void save()`

```C++
//保存文件
void save();
```



### 6.3 保存文件功能实现

```C++
void WorkerManager::save()
{
	ofstream ofs;
	ofs.open(FILENAME, ios::out);		//用输出的方式打开文件 --写文件
	for (int i = 0; i < this->m_EmpNum; i++)	//将每个人的数据写入文件中
	{
		ofs << this->m_EmpArray[i]->m_Id << " "
			<< this->m_EmpArray[i]->m_Name << " "
			<< this->m_EmpArray[i]->m_DeptId << endl;
	}
	ofs.close();
}
```



## 7、文件交互 --- 读文件

功能描述：将文件中的内容读取到程序中

虽然我们实现了添加职工后保存到文件的操作，但是每次开始运行程序，并没有将文件中数据读取到程序中

而我们的程序功能中还有清空文件的需求

因此构造函数初始化数据的情况分为三种



1. 第一次使用，文件未创建
2. 文件存在，但是数据被用户清空
3. 文件存在，并且保存职工的所有数据

### 7.1 文件未创建

在workerManager.h中添加新的成员属性 m_FileIsEmpty标志文件是否为空

```C++
	//标志文件是否为空
	bool m_FileIsEmpty;
```



修改WorkerManager.cpp中构造函数代码

```C++
WorkerManager::WorkerManager()
{
	ifstream ifs;
	ifs.open(FILENAME, ios::in);

	//文件不存在情况
	if (!ifs.is_open())
	{
		this->m_EmpNum = 0;  //初始化人数
		this->m_FileIsEmpty = true; //初始化文件为空标志
		this->m_EmpArray = NULL; //初始化数组
		ifs.close(); //关闭文件
		return;			//文件不存在，直接return，不进行其他操作
	}
}
```



### 7.2 文件存在且数据为空

在workerManager.cpp中的构造函数追加代码：

```C++
	//当文件存在且数据为空
	char ch;			//定义一个字符，不赋值，则该字符只有一个结尾标志eof
	ifs >> ch;			//ifs右移去读这一个字符，读走ch的eof结尾标
	if (ifs.eof())		//eof()函数--判断文件里的数据是否为空，为空返回1
	{
		//数据为空和文件不存在的操作相同
		this->m_EmpNum = 0;
		this->m_EmpArray = NULL;
		this->m_FileIsEmpty = true;
		ifs.close();
		return;
	}
```



我们发现文件不存在或者为空清空 m_FileIsEmpty 判断文件是否为空的标志都为真，那何时为假？

成功添加职工后，应该更改文件不为空的标志

在`void WorkerManager::Add_Emp() `成员函数中添加：

```C++
		//更新职工不为空标志
		this->m_FileIsEmpty = false;
```





### 7.3 文件存在且保存职工数据

#### 7.3.1 获取记录的职工人数

在workerManager.h中添加成员函数 ` int get_EmpNum();`

```C++
	//统计人数
	int get_EmpNum();
```



workerManager.cpp中实现

```c++
int WorkerManager::get_EmpNum()
{
	ifstream ifs;
	ifs.open(FILENAME, ios::in);
	int id;
	string name;
	int dId;
	int num = 0;
	while (ifs >> id && ifs >> name && ifs >> dId)	//右移运算符去读文件里的职工信息
	{
		num++;		//统计人数
	}
	return num;
}
```

在workerManager.cpp构造函数中继续追加代码：

```C++
	int num = this->get_EmpNum();		//获取文件中职工的数量
	this->m_EmpNum = num;		//更新职工数量
```



#### 7.3.2 初始化数组

根据职工的数据以及职工数据，初始化workerManager中的Worker ** m_EmpArray 指针



在WorkerManager.h中添加成员函数  `void init_Emp();`

```C++
	//初始化员工
	void init_Emp();
```



在WorkerManager.cpp中实现

```c++
void WorkerManager::init_Emp()
{
	ifstream ifs;
	ifs.open(FILENAME, ios::in);
	int id;
	string name;
	int dId;
	int index = 0;
	while (ifs >> id && ifs >> name && ifs >> dId)	//读文件中的职工信息
	{
		Worker* worker = NULL;
        //根据不同的部门Id创建不同对象
		if (dId == 1)		//普通员工
		{
			worker = new Employee(id, name, dId);
		}
		else if (dId == 2)		 //经理
		{
			worker = new Manager(id, name, dId);
		}
		else		//老板
		{
			worker = new Boss(id, name, dId);
		}
		this->m_EmpArray[index] = worker;	//将文件中的信息写进m_EmpArray中
		index++;
	}
}
```



在workerManager.cpp构造函数中追加代码

```C++
	this->m_EmpArray = new Worker * [this->m_EmpNum];		//开辟职工空间
	this->init_Emp();			//进行文件读入操作
```



## 8、显示职工

功能描述：显示当前所有职工信息

#### 8.1 显示职工函数声明

在workerManager.h中添加成员函数  `void Show_Emp();`

```C++
	//显示职工
	void Show_Emp();
```



#### 8.2 显示职工函数实现

在workerManager.cpp中实现成员函数 `void Show_Emp();`

```c++
void WorkerManager::Show_Emp()
{
	if (this->m_FileIsEmpty)
	{
		cout << "文件为空或数据为空！" << endl;
	}
	else
	{
		for (int i = 0; i < m_EmpNum; i++)
		{
			this->m_EmpArray[i]->showInfo();	//利用多态调用程序接口，每个职工对应的showInfo()函数不一样
		}
	}
	system("pause");
	system("cls");
}
```



## 9、删除职工

功能描述：按照职工的编号进行删除职工操作



#### 9.1 删除职工函数声明

在workerManager.h中添加成员函数  `void Del_Emp();`

```C++
	//删除职工
	void Del_Emp();
```



#### 9.2 职工是否存在函数声明

很多功能都需要用到根据职工是否存在来进行操作如：删除职工、修改职工、查找职工

因此添加该公告函数，以便后续调用

在workerManager.h中添加成员函数  `int IsExist(int id);`

```C++
	//按照职工编号判断职工是否存在,若存在返回职工在数组中位置，不存在返回-1
	int IsExist(int id);
```



#### 9.3 职工是否存在函数实现

在workerManager.cpp中实现成员函数 `int IsExist(int id);`

```c++
int WorkerManager::IsExist(int id)
{
	int index = -1;

	for (int i = 0; i < this->m_EmpNum; i++)
	{
		if (this->m_EmpArray[i]->m_Id == id)
		{
			index = i;

			break;
		}
	}

	return index;
}
```



#### 9.4 删除职工函数实现

在workerManager.cpp中实现成员函数 ` void Del_Emp();`

```C++
void WorkerManager::Del_Emp()
{
	if (this->m_FileIsEmpty)
	{
		cout << "文件不存在或记录为空！" << endl;
	}
	else
	{
		//按职工编号删除
		cout << "请输入想要删除的职工号：" << endl;
		int id = 0;
		cin >> id;

		int index = this->IsExist(id);

		if (index != -1)  //说明index上位置数据需要删除
		{
			for (int i = index; i < this->m_EmpNum - 1; i++)
			{
				this->m_EmpArray[i] = this->m_EmpArray[i + 1];	//采用向前覆盖的方式
			}
			this->m_EmpNum--;

			this->save(); //删除后数据同步到文件中
			cout << "删除成功！" << endl;
		}
		else
		{
			cout << "删除失败，未找到该职工" << endl;
		}
	}
	
	system("pause");
	system("cls");
}
```



## 10、修改职工

功能描述：能够按照职工的编号对职工信息进行修改并保存

#### 10.1 修改职工函数声明

在workerManager.h中添加成员函数  `void Mod_Emp();`

```C++
	//修改职工
	void Mod_Emp();
```



#### 10.2 修改职工函数实现

在workerManager.cpp中实现成员函数 ` void Mod_Emp();`

```C++
void WorkerManager::Mod_Emp()
{
	if (this->m_FileIsEmpty)
	{
		cout << "文件不存在或记录为空！" << endl;
	}
	else
	{
		cout << "请输入修改职工的编号：" << endl;
		int id;
		cin >> id;

		int ret = this->IsExist(id);
		if (ret != -1)
		{
			//查找到编号的职工

			delete this->m_EmpArray[ret];	//先释放调要修改的职工

			int newId = 0;
			string newName = "";
			int dSelect = 0;

			cout << "查到： " << id << "号职工，请输入新职工号： " << endl;
			cin >> newId;

			cout << "请输入新姓名： " << endl;
			cin >> newName;

			cout << "请输入岗位： " << endl;
			cout << "1、普通职工" << endl;
			cout << "2、经理" << endl;
			cout << "3、老板" << endl;
			cin >> dSelect;

			Worker* worker = NULL;
			switch (dSelect)	//重新给员工赋职位
			{
			case1:
				worker = new Employee(newId, newName, dSelect);
				break;
			case2:
				worker = new Manager(newId, newName, dSelect);
				break;
			case 3:
				worker = new Boss(newId, newName, dSelect);
				break;
			default:
				break;
			}

			//更改数据 到数组中
			this->m_EmpArray[ret] = worker;

			cout << "修改成功！" << endl;

			//保存到文件中
			this->save();
		}
		else
		{
			cout << "修改失败，查无此人" << endl;
		}
	}

	//按任意键 清屏
	system("pause");
	system("cls");
}
```



## 11、查找职工

功能描述：提供两种查找职工方式，一种按照职工编号，一种按照职工姓名

#### 11.1 查找职工函数声明

在workerManager.h中添加成员函数  `void Find_Emp();`

```c++
	//查找职工
	void Find_Emp();
```



#### 11.2 查找职工函数实现

在workerManager.cpp中实现成员函数 ` void Find_Emp();`

```C++
void WorkerManager::Find_Emp()
{
	if (this->m_FileIsEmpty)
	{
		cout << "文件不存在或记录为空！" << endl;
	}
	else
	{
		cout << "请输入查找的方式：" << endl;
		cout << "1、按职工编号查找" << endl;
		cout << "2、按姓名查找" << endl;

		int select = 0;
		cin >> select;


		if (select == 1) //按职工号查找
		{
			int id;
			cout << "请输入查找的职工编号：" << endl;
			cin >> id;

			int ret = IsExist(id);
			if (ret != -1)
			{
				cout << "查找成功！该职工信息如下：" << endl;
				this->m_EmpArray[ret]->showInfo();
			}
			else
			{
				cout << "查找失败，查无此人" << endl;
			}
		}
		else if (select == 2) //按姓名查找
		{
			string name;
			cout << "请输入查找的姓名：" << endl;
			cin >> name;

			bool flag = false;  //查找的标志
			for (int i = 0; i < m_EmpNum; i++)
			{
				if (m_EmpArray[i]->m_Name == name)
				{
					cout << "查找成功,职工编号为："
						<< m_EmpArray[i]->m_Id
						<< " 号的信息如下：" << endl;
					this->m_EmpArray[i]->showInfo();
					flag = true;		//将标志更新为true
				}
			}
			if (flag == false)
			{
				//查无此人
				cout << "查找失败，查无此人" << endl;
			}
		}
		else
		{
			cout << "输入选项有误" << endl;
		}
	}
	system("pause");
	system("cls");
}
```



## 14、排序

功能描述：按照职工编号进行排序，排序的顺序由用户指定

#### 12.1 排序函数声明

在workerManager.h中添加成员函数  `void Sort_Emp();`

```C++
	//排序职工
	void Sort_Emp();
```



#### 12.2 排序函数实现

在workerManager.cpp中实现成员函数 ` void Sort_Emp();`

```C++
void WorkerManager::Sort_Emp()
{
	if (this->m_FileIsEmpty)
	{
		cout << "文件不存在或记录为空！" << endl;
		system("pause");
		system("cls");
	}
	else
	{
		cout << "请选择排序方式： " << endl;
		cout << "1、按职工号进行升序" << endl;
		cout << "2、按职工号进行降序" << endl;

		int select = 0;
		cin >> select;


		for (int i = 0; i < m_EmpNum; i++)		//选择排序
		{
			int minOrMax = i;
			for (int j = i + 1; j < m_EmpNum; j++)
			{
				if (select == 1) //升序
				{
					if (m_EmpArray[minOrMax]->m_Id > m_EmpArray[j]->m_Id)
					{
						minOrMax = j;
					}
				}
				else  //降序
				{
					if (m_EmpArray[minOrMax]->m_Id < m_EmpArray[j]->m_Id)
					{
						minOrMax = j;
					}
				}
			}

			if (i != minOrMax)
			{
				Worker * temp = m_EmpArray[i];
				m_EmpArray[i] = m_EmpArray[minOrMax];
				m_EmpArray[minOrMax] = temp;
			}

		}

		cout << "排序成功,排序后结果为：" << endl;
		this->save();
		this->Show_Emp();
	}

}
```



## 13、清空文件

功能描述：将文件中记录数据清空



#### 13.1 清空函数声明

在workerManager.h中添加成员函数  `void Clean_File();`

```C++
	//清空文件
	void Clean_File();
```





#### 13.2 清空函数实现

在workerManager.cpp中实现员函数 ` void Clean_File();`

```C++
//清空文件
void WorkerManager::Clean_File()
{
	cout << "确认清空？" << endl;
	cout << "1、确认" << endl;
	cout << "2、返回" << endl;

	int select = 0;
	cin >> select;

	if (select == 1)
	{
		//打开模式 ios::trunc 如果存在删除文件并重新创建
		ofstream ofs(FILENAME, ios::trunc);
		ofs.close();

		if (this->m_EmpArray != NULL)
		{
            for (int i = 0; i < this->m_EmpNum; i++)
			{
				if (this->m_EmpArray[i] != NULL)
				{
					delete this->m_EmpArray[i];		//先释放数组中的对象
				}
			}
			this->m_EmpNum = 0;					//更新职工数量为0
			delete[] this->m_EmpArray;			//再释放整个数组
			this->m_EmpArray = NULL;
			this->m_FileIsEmpty = true;			//更新文件为空
		}
		cout << "清空成功！" << endl;
	}

	system("pause");
	system("cls");
}
```



**析构代码也重新更新一下：**

```c++
WorkerManager::~WorkerManager()
{
	if (this->m_EmpArray != NULL)
	{
		for (int i = 0; i < this->m_EmpNum; i++)
		{
			if (this->m_EmpArray[i] != NULL)
			{
				delete this->m_EmpArray[i];		//先释放数组中的对象
			}
		}
		delete[] this->m_EmpArray;				//再释放整个数组
        this->m_EmpArray = NULL;
    }
}
```





