## 二：指针进阶



> 码字不易，对你有帮助 **点赞/转发/关注** 支持一下作者 
>
> 微信搜公众号：**不会编程的程序圆**
>
> **看更多干货，获取第一时间更新**



## 思维导图

***

![](https://img-blog.csdnimg.cn/20200306012414663.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTU0MDEw,size_16,color_FFFFFF,t_70)

## 目录

***

[TOC]



@[toc]
### 前言

***

#### 指针的概念

>1. 指针就是个变量，用来存放地址，地址唯一标识一块内存空间。
>2. 指针的大小是固定的4/8个字节（32位平台/64位平台）。
>3. 指针是有类型，指针的类型决定了指针的+-整数的步长，指针解引用操作的时候的权限。



### 1、字符指针

#### 字符串的 数组 与 指针 表示的区别

请看下面这段代码，猜测会输出什么：

**1-1.c**

```c
#include<stdio.h>

int main(void) {

	char str1[] = "Hello";
	char str2[] = "Hello";

	const char* str3 = "Hello";
	const char* str4 = "Hello";

	//查看str1与str2 ，str3与str4 的地址是否相同

	if (str1 == str2)
		printf("str1 == str2\n");

	if (str3 == str4)
		printf("str3 == str4\n");

	return 0;
}
```

输出：

```c
str3 == str4
```

我们不着急解释原因，我们再来看一下下面这个例子：

**1-2.c**

```c
#include<stdio.h>

int main(void){
    char str1[] = "hello";
	str1[0] = 'a';
	printf("%s\n", str1);
  
    return 0;
}
```

**1-3.c**

```c
#include<stdio.h>

int main(void){
  	char* str2 = "hello";
	str2[0] = 'a';
	printf("%s\n", str2);
  
    return 0;
}
```

**试着思考：**
1-2.c 和 1-3.c 输出的结果一样吗？
为什么 1-3.c 程序会直接崩溃呢？

这是因为字符串字面量 "hello" 存储在 **常量区** ，该区域内的常量是**只读**的，不能被修改。

为了更加清楚的了解上面三个程序的原理，不妨看看下图：

![](https://img-blog.csdnimg.cn/20200306012441968.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTU0MDEw,size_16,color_FFFFFF,t_70)

<br>

###  2、指针数组

```c
int* arr1[10]; //整形指针的数组
char *arr2[4]; //一级字符指针的数组
char **arr3[5];//二级字符指针的数组
```

<br>

### 3、数组指针

#### Ⅰ：定义

数组指针是**指针**。

```c
int *p1[10];//这是一个数组，里面放的都是 int型的指针
int (*p2)[10];//这是一个指针，指向一个大小为 10个 int 的数组
```

**`[]`的优先级高于 `*`，所以 `()`不能省略**



#### Ⅱ：数组名 与 &数组名

**3-1.c**

```c
#include <stdio.h>

int main(void){
    
int arr[10] = { 0 };
	printf("arr = %p\n", arr);
	printf("&arr= %p\n", &arr);
    
	printf("arr+1 = %p\n", arr+1);
	printf("&arr+1= %p\n", &arr+1);
    
	return 0;
}
```

输出：

```c
arr = 012FFE48
&arr= 012FFE48
arr+1 = 012FFE4C
&arr+1= 012FFE70
```



实际上： **&arr 表示的是数组的地址**，而不是数组首元素的地址。
数组的地址+1，跳过整个数组的大小，所以 &arr+1 相对于 &arr 的差值是40.  



#### Ⅲ：数组指针的使用

**3-2.c**

```c
#include<stdio.h>

void printArr1(int arr[][4], int row, int col) {

	int i, j;
	printf("\nint arr[][4]\n");
	for (i = 0; i < row; i++) {
		for (j = 0; j < col; j++)
			printf("%d	", arr[i][j]);
		printf("\n");
	}

}

void printArr2(int (*arr)[4], int row, int col) {
	
	int i, j;
	printf("\n int (*arr)[]\n");
	for (i = 0; i < row; i++) {
		for (j = 0; j < col; j++)
			printf("%d	", arr[i][j]);
		printf("\n");
	}

}

int main(void) {

	int arr[3][4] = { 0 };

	printArr1(arr, 3, 4);
	printArr2(arr, 3, 4);

	return 0;
}
```

输出：

```c
int arr[][4]
0       0       0       0
0       0       0       0
0       0       0       0

 int (*arr)[]
0       0       0       0
0       0       0       0
0       0       0       0
```



### 4、数组参数 & 指针参数

#### Ⅰ：一维数组传参

**4-1.c**

我们先来看一下 main 函数

```c
#include<stdio.h>

int main(void) {

	int arr[10] = { 0 };
	int* arr2[20] = { 0 };

	test1(arr);
	test2(arr);
	test3(arr);

	test4(arr2);
	test5(arr2);

	return 0;
}
```

请判断以下五个函数的参数写法是否正确：

```c
void test1(int arr[]) {

}

void test2(int arr[10]) {

}

void test3(int* arr) {

}

void test4(int* arr[20]) {

}

void test5(int** arr) {

}
```

这五种写法都是可以的。



#### Ⅱ：二维数组传参

**4-2.c**

```c
#include<stdio.h>

void test(int arr[3][5])
{}
void test(int arr[][])// error
{}
void test(int arr[][5])
{}
void test(int *arr) // error
{}
void test(int* arr[5])
{}
void test(int (*arr)[5])
{}
void test(int **arr) // error
{}
int main(void)
{
	int arr[3][5] = {0};
	test(arr)
    
	return 0;
}
```

**二维数组传参，只有第一个 [] 内的数字可以省略，其他的都不能省略**



### 5、函数指针

#### Ⅰ：定义

**5-1.c**

```c
#include<stdio.h>

int main(void) {

	printf("%p\n", main);
	printf("%p\n", &main);//输出函数地址 用不用 & 都可以

	return 0;
}
```

输出：

```c
008A129E
008A129E
```

输出的两个地址是 main 函数的地址。



如何理解函数指针？

> 当你写完并保存一个 .c 文件后，这个文件是存储在计算机硬盘中的。编译 c文件后生成的 .exe(可执行文件)也是在硬盘上。
>
> 当你双击 .exe 文件时，操作系统就会把这个文件加载到**内存中**，并创建一个对应的 进程。

对于函数指针来说，最大的用处就是可以直接调用函数。



如何保存函数地址呢？
**5-2.c**
```c
void test()
{
printf("hehe\n");
}

void (*pfun1)();

```



#### Ⅱ：函数指针数组

我们要将函数的地址存放到一个数组中去，这个数组就叫 函数指针数组。

> 定义方法：`int (*parr[10])();`
>
> parr 先与 [] 结合，说明 parr 是数组，数组的内容是 int (*)() 类型的函数指针。



##### 应用：转移表

**5-3.c** 计算器
```c
#include<stdio.h>

int add(int x, int y) {
	return (x + y);
}

int sub(int x, int y) {
	return (x - y);
}

int mul(int x, int y) {
	return (x * y);
}

int div(int x, int y) {
	return (x / y);
}



int main(void) {

	int x, y;
	int input = 1;
	int (*p[5])(int, int) = { 0, add, sub, mul, div };

	while (input) {

		printf("**********************\n");
		printf("         1: ADD      \n");
		printf("         2: SUB      \n");
		printf("         3: MUL      \n");
		printf("         4: DIV      \n");
		printf("         0: EXIT     \n");
		printf("**********************\n");

		printf("Enter a choice: ");
		scanf("%d", &input);
		
		if (input == 0)
			break;
		else if (input < 1 || input > 4) {
			printf("wrong input!\n");
		}
		else {
			printf("Enter two numbers: ");
			scanf("%d %d", &x, &y);
			printf("result = %d\n", (*p[input])(x, y));
		}

	}

	return 0;
}
```

##### 应用：回调函数



### 6.指针和数组笔试题

环境：**32 位机器**

#### 第一组

```c
int a[] = {1,2,3,4};
printf("%d\n",sizeof(a));
printf("%d\n",sizeof(a+0));
printf("%d\n",sizeof(*a));
printf("%d\n",sizeof(a+1));
printf("%d\n",sizeof(a[1]));
printf("%d\n",sizeof(&a));
printf("%d\n",sizeof(*&a));
printf("%d\n",sizeof(&a+1));
printf("%d\n",sizeof(&a[0]));
printf("%d\n",sizeof(&a[0]+1));
```

答案：

```c
printf("%d\n",sizeof(a));// 16
printf("%d\n",sizeof(a+0));// 4 (a + 0 这个操作使得编译器将 a 看为指针)
printf("%d\n",sizeof(*a));// 4
printf("%d\n",sizeof(a+1));// 4
printf("%d\n",sizeof(a[1]));// 4
printf("%d\n",sizeof(&a));// 4
printf("%d\n",sizeof(*&a));// 16 (&a 是数组指针。再次用 * 解引用，是从这个地址开始取 int(*)[4] 类型对应的字节数)
printf("%d\n",sizeof(&a+1));// 4 (&a 得到的是 int(*)[4] 类型的指针，只要是指针大小就是 4)
printf("%d\n",sizeof(&a[0]));// 4
printf("%d\n",sizeof(&a[0]+1));// 4
```

#### 第二组

```c
char arr[] = {'a','b','c','d','e','f'};
printf("%d\n", sizeof(arr));
printf("%d\n", sizeof(arr+0));
printf("%d\n", sizeof(*arr));
printf("%d\n", sizeof(arr[1]));
printf("%d\n", sizeof(&arr));
printf("%d\n", sizeof(&arr+1));
printf("%d\n", sizeof(&arr[0]+1));

printf("%d\n", strlen(arr));
printf("%d\n", strlen(arr+0));
printf("%d\n", strlen(*arr));
printf("%d\n", strlen(arr[1]));
printf("%d\n", strlen(&arr));
printf("%d\n", strlen(&arr+1));
printf("%d\n", strlen(&arr[0]+1));
```

答案：

```c
printf("%d\n", sizeof(arr));// 6
printf("%d\n", sizeof(arr+0));// 4
printf("%d\n", sizeof(*arr));// 1
printf("%d\n", sizeof(arr[1]));// 1
printf("%d\n", sizeof(&arr));// 4 (char (*)[6] 类型的指针)
printf("%d\n", sizeof(&arr+1));// 4
printf("%d\n", sizeof(&arr[0]+1));//4

printf("%d\n", strlen(arr));// 未定义 (arr 字符数组没有 '\0'，有可能会出现一个随机值，程序也有可能会崩溃。)
printf("%d\n", strlen(arr+0));// 未定义
printf("%d\n", strlen(*arr)); // 错误的参数类型 (strlen 要的是 char* 类型，但是 *arr 是 char类型。*arr 是字符 a，也就是 97，编译器有可能将 97 当成一个 16 进制的地址。所以，这样的代码一定是不对的)
printf("%d\n", strlen(arr[1]));//同上
printf("%d\n", strlen(&arr));// 未定义
printf("%d\n", strlen(&arr+1)); // 未定义
printf("%d\n", strlen(&arr[0]+1));// 未定义
```

#### 第三组

```c
char arr[] = "abcdef";
printf("%d\n", sizeof(arr));
printf("%d\n", sizeof(arr+0));
printf("%d\n", sizeof(*arr));
printf("%d\n", sizeof(arr[1]));
printf("%d\n", sizeof(&arr));
printf("%d\n", sizeof(&arr+1));
printf("%d\n", sizeof(&arr[0]+1));

printf("%d\n", strlen(arr));
printf("%d\n", strlen(arr+0));
printf("%d\n", strlen(*arr));
printf("%d\n", strlen(arr[1]));
printf("%d\n", strlen(&arr));
printf("%d\n", strlen(&arr+1));
printf("%d\n", strlen(&arr[0]+1));
```

答案：

```c
char arr[] = "abcdef";
printf("%d\n", sizeof(arr));//7
printf("%d\n", sizeof(arr+0));//7
printf("%d\n", sizeof(*arr));//1
printf("%d\n", sizeof(arr[1]));//1
printf("%d\n", sizeof(&arr));//4 (char (*)[7])
printf("%d\n", sizeof(&arr+1));//4 (char (*)[7])
printf("%d\n", sizeof(&arr[0]+1));//4 (char*)

printf("%d\n", strlen(arr));// 6
printf("%d\n", strlen(arr+0));// 6
printf("%d\n", strlen(*arr));// 错误的参数类型
printf("%d\n", strlen(arr[1]));// 同上
printf("%d\n", strlen(&arr));// 6 (&arr 的类型是 char (*)[7] 与 char* 类型不一致，但是 &arr 与 arr 是相同的，所以恰巧能得出正确结果，但是这是错误的写法。)
printf("%d\n", strlen(&arr+1)); // 未定义 (&arr + 1,跳过了整个数组，访问数组后面的空间，非法内存访问)
printf("%d\n", strlen(&arr[0]+1)); // 5 (&arr[0] -> char* ,加以跳过一个数组元素)
```

#### 第四组

```c
char *p = "abcdef";
printf("%d\n", sizeof(p));
printf("%d\n", sizeof(p+1));
printf("%d\n", sizeof(*p));
printf("%d\n", sizeof(p[0]));
printf("%d\n", sizeof(&p));
printf("%d\n", sizeof(&p+1));
printf("%d\n", sizeof(&p[0]+1));

printf("%d\n", strlen(p));
printf("%d\n", strlen(p+1));
printf("%d\n", strlen(*p));
printf("%d\n", strlen(p[0]));
printf("%d\n", strlen(&p));
printf("%d\n", strlen(&p+1));
printf("%d\n", strlen(&p[0]+1));
```

答案：

```c
char *p = "abcdef";
printf("%d\n", sizeof(p));// 4
printf("%d\n", sizeof(p+1));// 4
printf("%d\n", sizeof(*p));// 1
printf("%d\n", sizeof(p[0]));// 1
printf("%d\n", sizeof(&p));// 4 (char**)
printf("%d\n", sizeof(&p+1));// 4 (char**)
printf("%d\n", sizeof(&p[0]+1));// 4 

printf("%d\n", strlen(p));// 6
printf("%d\n", strlen(p+1));// 5
printf("%d\n", strlen(*p));// 错误的参数类型 
printf("%d\n", strlen(p[0]));// 错误的参数类型
printf("%d\n", strlen(&p));// 同上 (&p 的类型是 char**，将char** 强转成的 char* 并不是一个字符串) 
printf("%d\n", strlen(&p+1));// 未定义
printf("%d\n", strlen(&p[0]+1));// 5 (对于 &p[0] 来说，p 先与 [] 结合)
```



> 指针为什么也可以用 `[]`运算符？
>
> 对于指针 int* p = "abc";
>
> `p[1]` 等价于 `*(p + 1)`
>
> 这是因为数组很多时候可以隐式转换成指针。



重点注意：`printf("%d\n", strlen(&p));`

`&p`的类型是 `char**`,但是C语言会将其隐式类型转换成 `char*`，但是 strlen 访问的是地址p的内存空间，那这其实是未定义行为。



#### 第五组

```c
int a[3][4] = {0};
printf("%d\n",sizeof(a));
printf("%d\n",sizeof(a[0][0]));
printf("%d\n",sizeof(a[0]));
printf("%d\n",sizeof(a[0]+1));
printf("%d\n",sizeof(*(a[0]+1)));
printf("%d\n",sizeof(a+1));
printf("%d\n",sizeof(*(a+1)));
printf("%d\n",sizeof(&a[0]+1));
printf("%d\n",sizeof(*(&a[0]+1)));
printf("%d\n",sizeof(*a));
printf("%d\n",sizeof(a[3]));
```

答案：

```c
int a[3][4] = {0};
//所谓二维数组本质是一维数组。里面的每个元素又是一个一维数组。
//本例是一个长度为 3 的一维数组，每个元素又是长度为 4 的一维数组。(VS 中可以用调试来测试)
printf("%d\n",sizeof(a));// 48
printf("%d\n",sizeof(a[0][0]));// 4
printf("%d\n",sizeof(a[0]));// 16 (a[0] 的类型是 int[4])
printf("%d\n",sizeof(a[0]+1));// 4 (a[0]->int[4]相当于一个一维数组，a[0] + 1 隐式转换为指针 int*)
printf("%d\n",sizeof(*(a[0]+1)));// 4 (a[0] + 1 -> a[0][1])
printf("%d\n",sizeof(a+1));// 4
printf("%d\n",sizeof(*(a+1)));// 4
printf("%d\n",sizeof(&a[0]+1));// 4 (a[0] -> int[4],&a[0] -> int (*)[4],再加1还是数组指针)
printf("%d\n",sizeof(*(&a[0]+1)));// 16 (int (*)[4] 解引用变为 int[4])
printf("%d\n",sizeof(*a));// 16 (*a -> *(a + 0) -> a[0])
printf("%d\n",sizeof(a[3]));// 16
```

重点注意：

`printf("%d\n",sizeof(a[0]+1))`

`printf("%d\n",sizeof(&a[0]+1))`

a[0] 与 &a[0] 的差异比较：

```c
	int a[3][4] = {
		{1, 2, 3, 4},
		{5, 6, 7, 8},
		{5, 10, 11, 12},
	};

	printf("%d\n", *(a[0] + 1));// 2
	printf("%d\n", **(&a[0] + 1));//5
```



`printf("%d\n",sizeof(*(&a[0]+1)));`

我们来一步一步分析：

`a[0] -> int[4] ; &a[0] -> int (\*)[4] ; &a[0] + 1 -> int (\*)[4] ; *(&a[0] + 1) -> int[4]`



`printf("%d\n",sizeof(a[3]))`

`sizeof`是一个运算符，并不是函数。它在预编译时期替换。而我们说的“数组下标访问越界”前提条件是 **内存**访问越界，这个时期是程序运行时。a[3] 就是 int[4] 类型，所以就是 16。哪怕你写 a[100]都可以。

`printf("%d\n", 16)`是程序运行时执行的语句。

#### 关于 const

```c
int num;
const int* p = &num;
int const* p = &num;// 这样的写法不科学，int* 应该当成一个整体，不过它的含义与上面的相同。
int* const p = &num;
```

对于第一种写法，*p 是不能改变的；对于第三种写法，地址 p 是不能被改变的。



### 7. 指针笔试题

#### Ⅰ

```c
int main(void)
{
int a[5] = { 1, 2, 3, 4, 5 };
int *ptr = (int *)(&a + 1);
printf( "%d,%d", *(a + 1), *(ptr - 1));
return 0;
}
```



`a + 1`：a 隐式转换成 指针，指向 首地址后移 4 个字节。（a 隐式转换后是 int* 类型，它指向的 int 大小是 4 个字节，所以后移 4 个字节）

`&a` 的类型是 `int(*)[5]` ,所以 `&a + 1` 后移 int[5] 的长度



所以最后输出的是：2，5

#### Ⅱ

```c
//由于还没学习结构体，这里告知结构体的大小是20个字节
struct Test
{
int Num;
char *pcName;
short sDate;
char cha[2];
short sBa[4];
}*p;
//假设p 的值为0x100000。 如下表表达式的值分别为多少？
int main(void)
{
	printf("%p\n", p + 0x1);
	printf("%p\n", (unsigned long)p + 0x1);
	printf("%p\n", (unsigned int*)p + 0x1);
	
    return 0;
}
```



`p + 0x1`: p 加十六进制的 1，p 所指向的结构体大小是 20，所以 p 会增加 20 。但是注意 `%p` 输出的是 16 进制的地址，所以输出的是 0x100014

`(unsigned long)p + 0x1`: p 被强转成了一个数，所以输出的就是 0x100001

`(unsigned int*)p + 0x1`: p 被强转成了一个 int* 类型的指针，所以输出的是 0x100004

#### Ⅲ

```c
int main(void)
{
	int a[4] = { 1, 2, 3, 4 };
	int *ptr1 = (int *)(&a + 1);
	int *ptr2 = (int *)((int)a + 1);
	printf( "%x,%x", ptr1[-1], *ptr2);
	
    return 0;
}
```



`ptr1[-1]`: 前面我们说过，这个操作相当于 `*(ptr1 - 1)`

`(int)a + 1`: 是将 a 先强转为 int 然后再加 1，所以 a 仅仅增加了 1 个字节

![](https://img-blog.csdnimg.cn/20200306012537936.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTU0MDEw,size_16,color_FFFFFF,t_70)

#### Ⅳ

```c
#include <stdio.h>
int main(void)
{
	int a[3][2] = { (0, 1), (2, 3), (4, 5) };
	int *p;
	p = a[0];	
	printf( "%d", p[0]);
	
    return 0;
}
```



p[0] -> a[0] [0] ，所以输出的是 0 吗？

并不是，注意看 a[3] [2]大括号内的内容，里面是圆括号而不是大括号，这是**逗号表达式**。

所以，a[0] [0] == 1 



#### Ⅴ

```c
int main(void){
	int a[5][5];
	int(*p)[4];
	p = a;
	printf( "%p,%d\n", &p[4][2] - &a[4][2], &p[4][2] - &a[4][2]);
	
    return 0;
}
```



指针（同类型）相减的意义是**两个指针之间间隔的元素个数**

`&p[4][2]` -> 数组中的第 19 个元素（4 * 4 + 3）

`&a[4][2]` -> 数组中的第 23 个元素  (4 * 5 + 3)



答案：FFFFFFFC,-4



#### Ⅵ

```c
int main(void)
{
	int aa[2][5] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
	int *ptr1 = (int *)(&aa + 1);
	int *ptr2 = (int *)(*(aa + 1));
	printf( "%d,%d", *(ptr1 - 1), *(ptr2 - 1));
    
	return 0;
}

```

`&aa` 的类型是 `int(*)[2][5]`，所以 `&aa + 1` 指向的是整个数组后面的内存 。所以 `*(ptr1 - 1)` 的值是 10

`aa`  aa + 1 让 aa 隐式转换为 `int(*)[5]` ,所以 `aa + 1` 指向的是元素 6 所在的地址。所以 `*(ptr2 - 1)` 的值是 5



#### Ⅶ

```c
#include <stdio.h>
int main(void)
{
	char *a[] = {"work","at","alibaba"};
	char**pa = a;
	pa++;
	printf("%s\n", *pa);
	
    return 0;
} 
```
<br>
![](https://img-blog.csdnimg.cn/20200306012601867.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTU0MDEw,size_16,color_FFFFFF,t_70)


#### Ⅷ

```c
int main(void)
{
	char *c[] = {"ENTER","NEW","POINT","FIRST"};
	char** cp[] = {c+3,c+2,c+1,c};
	char***cpp = cp;
	printf("%s\n", **++cpp);// ++cpp 会改变 cpp 的值
	printf("%s\n", *--*++cpp+3);//
	printf("%s\n", *cpp[-2]+3);//-2 并没有改变 cpp
	printf("%s\n", cpp[-1][-1]+1);
	
    return 0;
}
```

<br>

![](https://img-blog.csdnimg.cn/20200306012612708.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTU0MDEw,size_16,color_FFFFFF,t_70)



单目运算符从右向左依次运算。

```c
char* p = "ENTER";
printf("%s", p + 3);// 输出 ER，p + 3 增加 3 个字节，因为 p 指向的类型是 char 大小是 1 个字节。
```







***

以上就是本次的内容。

如果文章有错误欢迎指正和补充，感谢！

最后，如果你还有什么问题或者想知道到的，可以在评论区告诉我呦，我可以在后面的文章加上你们的真知灼见​​。

**关注我**，看更多干货！

我是程序圆，我们下次再见。