# 指针
## 普通指针
### 普通指针是存放变量地址的变量
```c
#include <stdio.h>
int main(){
	int *x;/*定义一个x指针*/
	int a=123;
	x=&a;/*将a变量取址传给x指针*/
	printf("a变量的值：%d\na变量的地址%p",*x,x);
	return 0;
}
```
- `*` 解引用,将指针变量中的地址转变为变量原值
- `&` 取址符，将后面的变量取址
### 指针可以作为函数的参数
```c
#include <stdio.h>
void func(int *p){
	printf("参数的值：%d\n参数的地址：%p",*p,p)
}
int main(){
	int a=12;
	func(&a);
	return 0;
}
```
### char 类型指针变量
定义`char *str;`
`str 是char 类型指针变量。

- 所以`str`存放的是地址
赋值`str="ok";`
`"ok"`存放在程序**只读常量内存段**，`str = "ok";` 让指针`str`指向 `"ok"` 的首字符`'o'`
内存：
```plaintext
只读段： 'o' 'k' '\0'
 str ───────────> ↑
```
- 可以对str解引用
- `*str`默认是第一个字符,等价与`*(str+0)`
- `*(str+x)`  第(x+1)个字符
例：
```c
char *str;
str="ok";
printf("%c",*str)//输出：o
printf("%c",*(str+1))//输出：k
```
- char 类型指针变量本质是存着地址，可以用`%p`来显示地址
```c
char *str;
str="hello word";
printf("%p",str);//输出str的地址，默认是str+0
printf("%s",str)//输出值
```
## 函数指针
### 函数指针是存函数地址的变量
```c
#include <stdio.h>
void (*cd)(int);/*定义一个函数指针*/
void func1(int a){
	printf("%d",a)
}
int main(){
	cd=func1;/*将函数func1的地址赋值给cd*/
	cd(12);/*接收参数，执行对应地址的函数*/
	return 0;
}
```
- 定义指针函数应与要接收的函数的返回值和参数定义一致
### 回调函数
- 函数指针也可做函数的参数，函数指针和函数结合，可形成回调函数
```c
#include <stdio.h>
// 这就是回调函数，由别人来调用
void my_callback(int data)
{
    printf("回调被执行，收到数据：%d\n", data);
}

// 库函数，接收一个函数指针
void lib_do_work(void (*cb)(int))
{
    printf("库正在干活...\n");
    // 条件满足，调用你传进来的回调
    cb(666);
}

int main()
{
    // 把你的函数地址传给库，不是在这里调用my_callback()
    lib_do_work(my_callback);
    return 0;
}
```
## 结构体指针
### 把一堆不同类型的变量打包，合成一个整体，就叫结构体。
```c
#include <stdio.h>
#include <string.h>
//定义结构体
struct Person {
	char name[20];
	int age;
	int id;
	void (*cd) (int);
}
void func1 (int a){
	printf("a:%d",a)
}
int main(){
	struct Person p1;  //创建一个结构体p1
	strcpy(p1.name,"tang")
	p1.age=19;         //定义age为19
	p1.id=123; 
	p1.cd=func1;
	p1.cd(4)//执行func1,传递参数4        
	printf("p1的id是：%d",p1.id)//p1.id查看
	return 0;
}
```
- 访问成员：变量.成员名
### 结构体指针是存结构体地址的变量

- `p->id` 等价于 `(*p).id`， `->`就是语法糖，简化书写
### `typedef`  别名写法
`typedef struct x{...}别名`

```c
#include <stdio.h>

//定义结构体
typedef struct Student {
    int id;
    void* data;
} student2;//student2别名

int main(void)
{
    // 普通结构体变量
    student2 s;//struct Student s;的别名定义
    s.id = 100;
    s.data = (void*)666;
    // 访问成员：变量.成员名      . 点

    // 结构体指针，存s的地址
    student2 *p=&s//struct Student *p = &s;的别名写法

    // 指针访问结构体成员：指针->成员名    ->箭头
    printf("%d\n", p->id);
    printf("%p\n", p->data);

    // 等价的长写法（几乎没人这么写）
    printf("%d\n", (*p).id);
    // *p：把指针解引用，拿到结构体本体，再用 . 访问成员
    return 0;
}
```

# 联合体
**联合体（共用体）**：多个成员**共用同一块内存空间**，同一时刻只能使用其中一个成员；内存大小等于**最大成员的字节数**。

> 和结构体`struct`对比：struct 是成员内存累加；union 是成员重叠共享内存。

## 1. 语法

运行

```c
union 联合体名 {
    类型1 成员1;
    类型2 成员2;
    ...
};
```

示例：

运行

```c
#include <stdio.h>

union Data {
    int     i;    // 4字节
    float   f;    // 4字节
    char    c;    // 1字节
};

int main(void) {
    union Data d;
    printf("联合体大小：%zu\n", sizeof(d)); // 最大成员4字节 → 输出4

    d.i = 100;
    printf("d.i = %d\n", d.i);

    d.f = 3.14f;   // 覆盖这块内存！i的值直接失效
    printf("d.f = %f\n", d.f);
    // 此时再读 d.i 是乱码，因为内存已经被float改写
    return 0;
}
```

> ⚠️关键点：**赋值一个成员，会覆盖其他成员的数据，不要同时读多个成员**。

# printf /scanf 格式说明符大全

`%`开头的叫**格式占位符（format specifier）**，`printf`输出、`scanf`输入都用这套。

## printf 输出常用

表格

| 占位符    | 含义                                          | 类型                          |
| ------ | ------------------------------------------- | --------------------------- |
| `%d`   | 十进制有符号整数                                    | `int`                       |
| `%i`   | 十进制有符号整数                                    | `int`（和 % d 几乎一样，scanf 有区别） |
| `%u`   | 十进制无符号整数                                    | `unsigned int`              |
| `%x`   | 小写十六进制                                      | `unsigned int`              |
| `%X`   | 大写十六进制                                      | `unsigned int`              |
| `%o`   | 八进制                                         | `unsigned int`              |
| `%f`   | 浮点数（小数）                                     | `float / double`            |
| `%lf`  | printf 里等价 % f；**scanf 才必须用 % lf 读 double** | `double`                    |
| `%c`   | 单个字符                                        | `char`                      |
| `%s`   | 字符串（遇到`\0`停止）                               | `char *`                    |
| `%p`   | 打印指针 / 内存地址                                 | 指针类型                        |
| `%%`   | 输出一个普通的 `%`                                 | 无参数                         |
| `%zd`  | 打印`size_t`（sizeof 返回的类型）                    | `size_t`                    |
| `%lld` | long long 长整型                               | `long long`                 |
| `%llu` | unsigned long long                          | `unsigned long long`        |
## 修饰符（写在 % 和字母中间）


运行

```c
%5d      //占5个字符宽度，右对齐
%-5d     //左对齐
%.2f     //保留2位小数
%05d     //宽度5，不足补0
%19s     //scanf最多读19字符
```

例子：


运行

```c
printf("%05d\n", 12);   //输出 00012
printf("%.2f\n",3.1415);//输出3.14
```

### 注意：

```c
// ✅可以：定义时初始化
char str[20] = "abcd";

// ❌不可以：运行时赋值
char str[20];
str = "abcd";
```

#### 1、`char str[20] = "abcd";` 这是**初始化**，不是赋值

`=`在这里不是普通赋值运算符，是**数组初始化语法**，发生在**创建数组的一瞬间**。

编译器行为：

1. 在内存开辟 20 字节连续数组空间。
2. 把字符串字面量 `"abcd"` 的字符 `a b c d \0`，拷贝到这块数组内存里面。
3. 剩下没用到的数组元素自动填 0。
 等价手动展开（编译器内部相当于帮你做了这个）

运行

```c
char str[20];
str[0] = 'a';
str[1] = 'b';
str[2] = 'c';
str[3] = 'd';
str[4] = '\0';
//剩下元素全部置0
```

只是编译器帮你写好了，不用你手动一个个写。

> 这是 C 语言专门给数组初始化的特殊语法，只允许写在变量定义那一行。

#### 2、`str = "abcd";` 这是运行时赋值，为什么报错

- `str` 是**数组名**，数组名是**地址常量**，不是变量。常量不能放在等号左边被修改。
- `"abcd"` 是字符串字面量，返回一个 `char*` 指针（指向只读常量区）。
- 你写 `str = "abcd"`，意图是把这个指针地址赋值给数组名。
- 但数组名没有存储空间，不能接收赋值，编译器直接报错：`assignment to expression with array type`。

> 类比：


运行

```c
int a;
3 = a; //❌报错，字面常量3不能放在等号左边
```

数组名`str`就类似这种常量，不能被赋值。

#### 简单来说 `str [0]` 等价与 `*(str+0)`，`str+0` 是地址，`*(str+0)='A"`对它解引用，访问这块内存，赋值字符`'A'`,`str`不是变量，是地址常量，常量不能做赋值运算
# C 内存分区（程序跑起来之后，内存分为 4 大块）

1. **栈 stack**
2. **堆 heap**
3. **全局 / 静态存储区（分初始化、未初始化）**
4. **常量字符串只读段（代码段里面的只读区域）**

> 我们写代码最常打交道：栈、堆；另外两块存放全局变量、字符串字面量。

## 1、栈 stack

**存：局部变量、函数形参、函数返回地址。**

运行

```c
void func(){
    int a;              //栈
    struct dict d1;     //栈
    char buf[20];       //栈数组
    int *p;             //指针变量p本身，存在栈；p指向的内容不一定在栈
}
```

- 分配释放：**自动管理**。进入函数自动分配；函数执行结束，栈直接回收，不用手动 free。
- 大小很小，Windows 默认 1MB 左右，Linux 一般 8MB。
- 栈空间很小，**不要定义巨大数组**，比如 `char big[1024*1024*10]` → 栈溢出程序直接崩溃。
- 速度极快。

> 注意：指针变量`int *p`本身在栈；但`p`存的地址，可以指向堆、全局区。

## 2、堆 heap

**存：malloc /calloc/realloc 手动申请的内存。**

运行

```c
int *p = malloc(sizeof(int)); //p变量本身在栈；申请出来的内存在堆
```

- 分配释放：**完全手动**，`malloc`申请，`free`释放；不会自动回收。忘记 free →内存泄漏。
- 空间很大，几十 GB 都可以，适合大块数据、动态结构体、libuv malloc 出来的句柄。
- 速度比栈慢一点。

`realloc` 动态扩容

`realloc`：重新调整堆内存块大小，可以扩大内存。
逻辑：先分配一小块内存，存不下的时候，`realloc`扩大容量。

运行

```c
ptr = realloc(old_ptr, new_size);
```
- `iod_ptr` 旧的内存地址，`nwe_size` 扩容后的内存大小
- 有可能原地扩容；也会找一块新内存，把旧内容拷贝过去，旧内存自动释放。
- ⚠️不要直接写 `str = realloc(str,cap);`，分配失败返回 NULL，会把原来 `str` 直接弄丢内存泄漏；要用临时变量接收。
```c
ptr=realloc(old_ptr,new_size);
if(ptr!=NULL){free(str);return 1;}
str=ptr;
```

> 重点区分：


运行

```c
int *p = malloc(4);
// p：指针变量，存于【栈】
// *p：p指向的那块内存，存于【堆】
```
- `malloc()`的返回值是`void*`赋值给结构体指针时，可以做做显示转换，也可以不做（C 语言规则：**`void*` 可以隐式自动转换成任意别的指针类型，不需要手动强转**。）
## 3、全局静态区（data /bss）

存放**全局变量、static 静态变量**，不在栈也不在堆。


运行

```c
int g_a = 100;   //全局初始化 → data段
static int s_b;  //static未初始化 → bss段，程序启动自动置0

int main(){
    static int c = 1; //函数内static，也在全局静态区，函数退出不会销毁
    return 0;
}
```

生命周期：**整个程序运行全程存在**，程序退出操作系统才回收。

`static`局部变量，函数结束不会销毁，下一次调用函数值还保留。

## 4、只读常量段 (.rodata)

存放字符串字面量、const 常量。


运行

```c
char *str = "hello";
//"hello"字符串本体放在只读段，不能修改 *str='x'会崩溃
//str这个指针变量本身在栈上
```

这块内存**只读，禁止写入**。

---

# 完整小例子，每个变量在哪

运行

```c
#include <stdio.h>
#include <stdlib.h>

int global_x = 10;          //全局初始化 → data

int main(void)
{
    int a = 1;              //栈
    int *p = malloc(4);     //p变量在栈；malloc出来4字节 →堆
    *p = 99;

    static int sta = 20;    //static静态变量 →全局静态区

    char *s = "abcd";       //s指针在栈；"abcd"字符串本体在只读段

    free(p);
    return 0;
}
```

## 表格总结

表格

|分区|存放内容|生命周期|谁管理|
|---|---|---|---|
|栈 stack|局部变量、数组、函数参数|函数调用期间，函数结束自动销毁|系统自动|
|堆 heap|malloc 申请内存|直到调用 free；程序退出 OS 回收|程序员手动 malloc/free|
|全局静态区|全局变量、static 变量|整个程序运行期间|程序启动就分配|
|只读段 rodata|"字符串字面量"、const 常量|整个程序运行期间|只读，不能写|

## 高频踩坑

1. 栈不要开超大数组，会栈溢出。大内存用 malloc 堆。
2. `"abc"`字符串在只读段，只能读不能改；想要修改，拷贝到栈数组或者堆内存。
3. static 局部变量不在栈，函数结束不会丢值。
4. 指针变量本身位置，和它指向的内存位置，**完全是两回事**。

> 补充：二进制机器指令（你的代码编译后的机器码）放在**代码段 (.text)**，只存 CPU 指令，不存程序数据。
# 常量变量（命名常量）
常数简介:
在编程中，**常量**是指在程序执行期间不能改变的值
`const 变量=值`
```c
const int a = 1;
```
##  const 只读修饰符

> `const`：这个变量的值不允许被修改，编译期检查。

运行

```c
const int x = 10;
x = 20; // ❌编译报错，不能写，只读
```

### 注意：const 不是真正常量（重点坑）


运行

```c
const int a = 10;
int arr[a]; // C语言标准不允许，a是const变量，不是编译字面量
```

`const int a` 是**只读变量**，不是宏`#define`编译期常量。

### const 修饰指针两种写法

运行

```c
const char *p;  // *p不能改，p本身可以改
char * const p;  // p本身不能改，*p可以改
```
# static 修饰符
## ① 修饰局部变量（函数内部）

- **生命周期变长：不在栈，放到全局静态区，函数退出不销毁**
- **作用域不变：依旧只能本函数内访问**
- **初始化只执行第一次**
- ✅**值可以修改！！**（static≠只读，可以`a++`）


运行

```c
void test(void){
    static int a = 1;
    a++; // ✅可以修改！static不阻止写
    printf("%d",a);
}
```
### static修饰局部变量,当重复调用函数，重新执行到`static int a = 1;`时，不会重新初始化，只有第一次执行
```c
#include <stdio.h>
void func1(){
	static int a = 1;
	a++;
	printf("%d",a)
}
int main(){
	func1();//输出2
	func1();//输出3（没有重新初始化）
	return 0;
}
```

## ② 修饰全局变量（写在函数外面）


运行

```c
static int g = 100;
```

- 生命周期：整个程序运行。
- **作用域限制：仅仅当前 .c 文件内部可见，别的 c 文件 extern 拿不到**。
    
    不加 static 的全局变量，整个项目所有 c 文件都可以访问。

## ③ 修饰函数


运行

```c
static void func(void){}
```

这个函数**仅本`.c`文件内部可用**，别的源文件不能调用。



# 组合使用 static const


运行

```c
// 本文件内，全局，生命周期全程，只读，不能修改
static const int MAX = 1024;
```

- static：仅限当前 c 文件使用
- const：值禁止修改

## 直观对比表格

表格

| 关键字      | 作用                        | 能不能修改值 |
| -------- | ------------------------- | ------ |
| `const`  | 变量变成只读；控制读写权限             | ❌不能修改  |
| `static` | 局部：延长生命周期；全局 / 函数：限制文件作用域 | ✅可以修改  |

# unsigned 无符号修饰符
`unsigned` 用来修饰整数类型，表示**去掉符号位，只能存非负数（≥0），不能存负数**。

C 里面默认的 `int`、`short`、`long` 都是**有符号 signed**（可以存正数、负数）。

## 1. 对比 int vs unsigned int

- `int`（有符号）：最高位是符号位。0 代表正数，1 代表负数。
    
    32 位 int 范围：`‑2147483648 ~ 2147483647`
- `unsigned int`（无符号）：全部比特都用来存数值，**没有负数**
    
    32 位 unsigned int 范围：`0 ~ 4294967295`

> 同样 4 字节内存，无符号把原本符号位也拿来存数字，**正数上限扩大一倍**。

简写：

运行

```c
unsigned int   // 无符号int
unsigned short // 无符号短整型
unsigned char  // 无符号字符，非常常用！
unsigned long
```

## 2. 重点坑：无符号数不能小于 0

运行

```c
#include <stdio.h>
int main(void)
{
    unsigned int a = 0;
    a = a - 1;   // 0减1，不会变成‑1！
    printf("%u\n", a); // 输出 4294967295（溢出回绕）
    return 0;
}
```

> ⚠️ 无符号数减法下溢，会发生**回绕（模 2^n）**，不会报错，是隐藏 bug 源头。
# 强制类型转换
语法通用格式：

运行

```c
(目标类型) 表达式
```

`(类型)` 这叫**强制转换运算符**。

> 注意：这只是把**值的解释方式改变**，**不会改变内存里面二进制比特本身**。

运行

```c
(struct dict*)&dict1;//dict是结构体变量名
```

这是**强制类型转换表达式**，它产生一个**临时的结构体指针值**，但是：**它没有变量名！**

- ### 拆解每一部分

1. `&dict1`：取结构体变量`dict1`的地址，类型是 `struct dict *`
2. `(struct dict *)`：C 强制类型转换运算符，把后面的地址，强行转成 `struct dict*` 指针类型

> 整体：得到一个**临时指针值**，这个东西不是变量，没有名字，没有内存存储，只是一个运算结果。 

 可以用临时的结构体指针访问结构体
```c
((struct dict*)&dict1)->id;
```
## 1、基础数值类型转换

### 整型之间


运行

```c
int a = 100;
short b = (short)a;     //int → short

unsigned int c = (unsigned int)-1; //负数转无符号
```

### 浮点 ↔ 整数

c

运行

```c
double x = 3.99;
int i = (int)x;   //double转int，直接截断小数，i=3
```

> ⚠️强制转换不会四舍五入，直接砍掉小数部分。

## 2、指针类强制转换（你现在重点用的，libuv 高频）

### ① 普通指针互转


运行

```c
int num = 10;
void *v = &num;

int *p = (int *)v;  // void* → int*
```

### ② 结构体指针互转（libuv handle‑data 套路）


运行

```c
struct MyData { int cnt; };
struct MyData d;

void *temp = &d;
struct MyData *p = (struct MyData *) temp;
// 就像你看到的 (struct dict *)&dict1
```

libuv 经典代码：


运行

```c
// handle->data 是 void*，强转回自己的结构体指针
MyObj *obj = (MyObj *) handle->data;
```

### ③ 指针转整数（很少用）

把地址当成数字看，调试用


运行

```c
#include <stdint.h>
int a = 10;
uint64_t addr = (uint64_t)&a;
```

> ⚠️危险：不同指针类型乱强转，很容易内存错位崩溃。

## 3、函数指针强制转换（回调会见到）

不同签名的函数指针互相强转，写回调的时候偶尔见到。


运行

```c
void f(int x){}

//把函数地址强制转成别的函数指针类型
typedef void(*cb_t)(void);
cb_t fun = (cb_t)f;
```

> ⚠️不建议随便乱转函数指针，调用会直接崩溃，libuv 一般不需要你手动做这个。

---

# 重要：**隐式转换（不用写括号，自动转）**

和强制转换区分开：

- **显式强制转换**：你手动写`(类型)xxx`，告诉编译器我就要这么转。
- **隐式转换**：编译器偷偷自动帮你转，你代码看不见。

### 隐式转换典型场景

1. `void*`可以**自动隐式转换**成任意指针，不需要写`(int*)`

c

运行

```c
void *v = &a;
int *p = v; // ✅不需要强制转换，void*自动适配
```

> 反过来：别的指针 → void*，也是自动。

2. 数值赋值自动隐式转换

c

运行

```c
int a = 3.14; // double隐式转int，a=3，编译器警告
```

## 容易踩坑点

1. 强制转换**不改内存二进制，只改变编译器如何解读这块内存**。

c

运行

```c
float f = 1.0f;
int i = *(int*)&f;
//不是把1.0变成整数1；而是把float的4字节二进制，直接当成int解读
```

2. 结构体指针乱强转，内存对齐不对直接崩溃。
3. `void*`转出其他指针：C 允许隐式；C++**必须手动写强制转换**。
# 文件读写
## `fprintf`
属于 C 标准库 `stdio.h`，**文件格式化输出函数**。

`printf` 输出到屏幕；`fprintf` 输出到指定文件流（可以是磁盘文件、也可以输出到屏幕）。

### 函数原型

运行

```c
int fprintf(FILE *stream, const char *format, ...);
```

- `stream`：文件指针 `FILE*`，往哪个流输出
- `format`：格式字符串，和 printf 一模一样 `"%d %s\n"`
- 返回值：成功返回打印的字符数；出错返回负数。

### 对比 printf


运行

```c
printf("hello %d\n", 123);
//等价于
fprintf(stdout, "hello %d\n", 123);
```

- `stdout` → 标准输出（控制台屏幕）
- `stderr` → 标准错误输出（报错信息）


运行

```c
fprintf(stderr, "出错了！\n"); //打印错误信息
```

### 示例 1：写入磁盘文件


运行

```c
#include <stdio.h>

int main(void)
{
    FILE *fp = fopen("log.txt", "w"); //w模式：创建/清空文件,文件路径可写绝对路径
    if(fp == NULL) return -1;

    int a = 666;
    fprintf(fp, "数字 = %d\n", a); //写入文件log.txt

    fclose(fp); //必须关闭文件
    return 0;
}
```

### 三种输出分清

1. `printf(fmt,...)` → 输出到 `stdout` 屏幕
2. `fprintf(stream, fmt,...)` → 输出到你指定的 FILE 流（文件 /stdout/stderr）
3. `sprintf(buf, fmt,...)` → 输出字符串写到内存字符数组缓冲区（⚠️容易缓冲区溢出，推荐`snprintf`）

### 小坑

- 使用`fprintf`写文件，用完**一定要`fclose(fp)`**，否则内容可能留在缓冲区，没真正写入磁盘。
- `fopen`有可能打开失败，一定要判断返回值是否为`NULL`。

## `fscanf`
`fscanf` 是**从文件流 `FILE*` 读取格式化数据**，和 `scanf` 是一对。
头文件同样：`#include <stdio.h>`

### 原型

运行

```c
int fscanf(FILE *stream, const char *format, ...);
```

- `stream`：FILE* 文件指针，从哪个文件读
- `format` 格式串，语法和 `scanf` 一样 `%d %s %f`
- 返回值：成功匹配到的变量个数；读到文件末尾返回 `EOF`(-1)

### 对比


运行

```c
scanf("%d", &a);          // 从键盘 stdin 读取
fscanf(stdin, "%d", &a);  // 和上面等价，stdin就是键盘输入流
```

### 简单示例：读文件

文件 `data.txt` 内容：

```plaintext
100 hello
```


运行

```c
#include <stdio.h>

int main(void)
{
    FILE *fp = fopen("data.txt", "r"); // r 只读打开
    if(fp == NULL) return -1;

    int num;
    char buf[32];
    // 读取一个整数，一个字符串
    int ret = fscanf(fp, "%d %s", &num, buf);

    printf("num=%d, str=%s\n", num, buf);

    fclose(fp);
    return 0;
}
```
#### `fscanf()`是行读
重复调用会换行
```c
#include <stdio.h>

int main(){
	FLIE *fp=fopen("data.txt","r");
	if(fp==NULL){return -1};
	char a[20],b[20];
	fscanf(fp,"%s",a);
	fscanf(fp,"%s",b);
	printf("第一行:%s\n第二行:%s",a,b);
	fclose(fp);
	return 0;
}
```
##### 也可以在`fscanf()`的第二个参数加`\n`
```c
#include <stdio.h>

int main(){
	FLIE *fp=fopen("data.txt","r");
	if(fp==NULL){return -1};
	char a[20],b[20];
	fscanf(fp,"%s\n%s",a,b);
	printf("第一行:%s\n第二行:%s",a,b);
	fclose(fp);
	return 0;
}
```
## 函数一览，方便记忆

表格

| 函数                 | 作用                    |
| ------------------ | --------------------- |
| `printf`           | 输出到屏幕 stdout          |
| `fprintf(fp, ...)` | 输出到指定 FILE 流（文件 / 屏幕） |
| `scanf`            | 从键盘 stdin 读取          |
| `fscanf(fp, ...)`  | 从指定 FILE 流读取          |
| `sprintf/snprintf` | 格式化输出到内存字符数组          |
| `sscanf`           | **从内存字符串读取，不是文件！**    |
# size_t类型
size_t  是一个无符号整数类型（unsigned integer type），专门用来表示对象的大小或内存中的字节数。

它并不是一个基础数据类型，而是定义在  `<stddef.h>` 、 `<stdio.h>` 、 `<stdlib.h> `等标准头文件中的一个类型别名（`typedef`）。


>为什么需要  size_t ？

C 语言标准并没有规定  int  或  long  具体占多少个字节，这取决于编译器和操作系统（例如在 32 位系统上  long  是 4 字节，而在 64 位系统上通常是 8 字节）。

为了让代码具有跨平台可移植性，C 标准引入了  size_t 。编译器会根据当前平台的寻址能力，自动将  size_t  定义为足够大的无符号整数类型（在 64 位系统上通常是  unsigned long  或  unsigned long long ），确保它能装下该平台最大可能的内存块大小。

3. 使用时的注意事项 ⚠️

它是无符号的： size_t  永远 `size_t≥ 0`。如果你将一个负数赋给  size_t ，它会发生无符号整数回绕（变成极大的正数），这常常是导致内存越界或死循环的 Bug 根源。

格式化输出：在  `printf`  或  `scanf `中打印/读取  `size_t ` 时，必须使用  `%zu `  格式说明符（C99 引入），而不是`  %d ` 或 ` %lu` ，否则在不同平台上可能会出现警告或输出错误。

```c
size_t len = strlen("hello");
printf("Length: %zu\n", len); // 正确
```

与有符号整数混用：尽量避免将  `size_t`  与  `int`  或  `ssize_t ` 进行直接的比较或算术运算，这会触发隐式类型转换，可能导致意料之外的逻辑错误。如果确实需要表示“可能为负”的大小（例如某些系统调用的返回值），可以使用  `ssize_t` 。

# `ssize_t`类型
带符号的整数类型。
在 `POSIX` 标准中，通常约定返回  -1  表示操作失败。因为  size_t  无法表示负数，所以引入了  `ssize_t` 。它的取值范围至少是  `[-1, SSIZE_MAX]`  。

| 特性         | size_t               | ssize_t                    |
| ---------- | -------------------- | -------------------------- |
| 符号性        | 无符号 (Unsigned)       | 有符号 (Signed)               |
| 典型用途       | 内存大小、数组长度、函数参数       | 系统调用的返回值（如 read/write）     |
| 能否表示错误     | 不能（永远 >= 0）          | 能（返回 -1 表示错误）              |
| printf 格式化 | %zu                  | %zd                        |
| 标准归属       | C 标准库 ( <stddef.h> ) | POSIX 标准 ( <sys/types.h> ) |

# EOF
在 C 语言中， EOF  是 "End Of File" 的缩写，它是一个宏定义，用于表示文件结束标志或输入流结束标志 。

需要特别强调的是， EOF  并不是一个真实存在于文件中的字符，而是由文件读取函数在尝试读取但发现已经没有更多数据时，返回的一个特殊信号值 。

以下是关于  EOF  的核心知识点：


1. 数据类型与值

整型常量： EOF  不是字符类型（ char ），而是一个整型常量（ int ） 。

通常为 -1：在大多数系统和编译器中， EOF  的值被定义为  -1  。由于普通的 ASCII 字符码值都是非负数，使用  -1  可以确保它不会与任何有效的字符发生冲突 。


2. 常见的使用场景

 EOF  通常配合文件读取或标准输入函数使用，用来控制循环的结束：

读取文件：使用 ` fgetc() ` 或  `fscanf()`  读取文件时，如果读到文件末尾，函数会返回  EOF  

标准输入：使用 ` getchar()`  或  `scanf()`  从键盘读取输入时，可以通过手动触发 EOF 来结束输入（在 Linux/Mac 中按  Ctrl+D ，在 Windows 中按  Ctrl+Z ） 。

3. 使用时的关键注意事项 ⚠️

必须使用  int  类型存储：因为 ` EOF`  的值是  -1 ，如果你用  char  类型的变量去接收  fgetc()  的返回值，可能会导致判断失效（例如在某些平台上  char  是无符号的， -1  会被截断为  255 ，从而无法与  EOF  匹配） 。

它既代表结束，也代表错误：当读取函数返回  EOF  时，有两种可能：一是真的读到了文件末尾，二是读取过程中发生了错误。如果只判断  != EOF ，可能会漏掉错误处理。

配合  `feof()`  和 ` ferror()`  使用：为了准确区分是“文件结束”还是“读取错误”，在循环结束后，通常需要调用  feof(fp) （检查是否到文件尾）或  ferror(fp) （检查是否出错）来进行二次确认 。

简单来说， `EOF`  是 C 语言中用来告诉你“数据已经读完了”的一个整数信号，在处理文件 I/O 时必不可少。

# `getchar()`逐字符读取
- 默认从标准输入流`(stdin)`读取数据，通常是键盘输入
- 标准调用方法不需要参数

## 返回值
- 正常读取到字符：返回该字符的ASCII码值（作为一个int类型的整数）
- 遇到文件结束或读取错误时：返回`EOF`（通常是`-1`）
## `getchar()`每次只取一位字符
```c
#include <stdio.h>
int main(void){
	int ch;
	ch=getchar();
	prinft("%c",ch);
	
	return 0;
}
```
只调用了一次，无论输入多少字符，都只取第一位，重复调用依次往后取
```c
for(int a=0;a<5;a++){
	int ch;
	ch=getchar();
	printf("%s",ch);
	
}
```
取前5个字符

# 前后自增
核心一句话：

- **`i++`：先用 i 原来的值，运算结束之后 i 再加 1**（后置）
- **`++i`：i 先加 1，再拿新的值参与运算**（前置）

> 只有**在表达式里面参与赋值、下标、函数参数**的时候才有区别；
> 
> 如果单独一行 `i++;` 或者 `++i;`，两者完全一模一样。

## 1. 单独写一行，无区别

运行

```c
int i = 1;
i++;
//等价
++i;
//结果i都等于2
```

## 2. 放到表达式里面，差别就出来

### ① 后置 `i++`

运行

```c
int a, i = 1;
a = i++;
//等价拆开
a = i;   //先把旧值给a
i = i + 1;
```

结果：`a=1`，`i=2`

### ② 前置 `++i`

运行

```c
int a, i =1;
a = ++i;
//等价拆开
i = i +1; //先自增
a = i;
```

结果：`a=2`，`i=2`

# 指针数组和数组指针
在 C 语言中， [] （数组）的优先级高于  * （指针）。因此，谁先和变量名结合，谁就是这个变量的“真身”。

 

1. 指针数组 (Array of Pointers)

代码写法： ` int *arr[5];` 
结合顺序：  []  先和  arr  结合，说明  arr  首先是一个数组；然后  `*`  表示数组里的元素是指针。

本质：它是一个数组，里面装了 5 个指向  int  类型的指针。

内存布局：在内存中连续存放着 5 个指针变量（在 64 位系统上，这个数组总共占 5 × 8 = 40 字节）。

常见用途：用来存储多个字符串，或者指向多个一维数组。

```c
char *strs[3];
strs[0] = "Hello";
strs[1] = "World";
strs[2] = "C";
```

 

2. 数组指针 (Pointer to an Array)

代码写法：  `int (*ptr)[5];` 
结合顺序： 括号  ()  强行让  *  先和  ptr  结合，说明  ptr  首先是一个指针；然后  [5]  表示这个指针指向的是一个包含 5 个  int  元素的数组。

本质：它是一个单独的指针，专门用来指向一个“长度为 5 的整型数组”。

内存布局：它本身只占一个指针的大小（64 位系统上占 8 字节），它指向的那块内存才存放着 5 个整数。

常见用途：用来遍历二维数组（按行遍历），或者作为函数参数传递二维数组。

```c
int arr[5] = {1, 2, 3, 4, 5};
int (*ptr)[5] = &arr; // ptr 指向整个 arr 数组
printf("%d\n", (*ptr)[2]); // 输出: 3
```

# 三目运算符

`?1:0`
- 如果前面的条件为真，整个表达式的结果就是1。
- 如果前面的条件为假，整个表达式的结果就是0。
```c
int a=10;
(a==10)?1:0;返回1
```

# 大端和小端
## 核心：大小端，字节存放顺序

4 字节整数 `0x01020304`，逻辑上的字节顺序（逻辑高位→低位）：

```plaintext
逻辑字节： [01]  [02]  [03]  [04]
高字节                      低字节
```
- `0x01020304`是个16进制数字，两个数字占一个字节(`8bit`)，8个数字占用4字节，04是16进制的个位和十位，也是最低字节
### 1. 小端模式（我们电脑 x86、x64 都是小端）

**低字节存低地址**

内存地址从低到高：

```plaintext
地址低 →  04    03    02    01 ←地址高
```

联合体例子：

运行

```c
union Endian {
    unsigned int num;
    unsigned char bytes[4];
};
u.num = 0x01020304;
```

小端机器：

- `bytes[0] = 0x04`（最低地址存最低字节）
- `bytes[1] = 0x03`
- `bytes[2] = 0x02`
- `bytes[3] = 0x01`

### 2. 大端模式（网络字节序、部分单片机）

**高字节存低地址**

内存地址从低到高：


```plaintext
地址低 → 01   02   03   04 ←地址高
```


- `bytes[0] = 0x01`（最低地址存最高字节）
- `bytes[1] = 0x02`
- `bytes[2] = 0x03`
- `bytes[3] = 0x04`
### 3. 为什么会存在这两种模式？

- **大端**：更符合人类的直观逻辑（网络传输中普遍采用，因此也叫**网络字节序**），且在进行数值比较时（如判断正负）只需读取第一个字节即可。
    
- **小端**：更符合CPU的运算逻辑。CPU进行加法运算时通常从低位开始算起（因为进位是从低位往高位传递），小端存储可以让CPU直接从低地址取数开始计算，效率更高。**我们常见的 x86 架构（Intel/AMD）PC 都是小端模式**。
    

---

### 4. 在C语言编程中，为什么必须关注它？

如果你只写纯逻辑运算（如 `int a = b + c;`），字节序对你完全透明。但在以下场景中，**字节序错误会导致严重的数据错乱（乱码、数值巨大错误）**：

- **网络编程（Socket）**：发送端是小端，接收端是大端，必须使用 `htonl()`、`ntohl()`、`htons()`、`ntohs()` 等函数进行转换。
    
- **二进制文件读写**：将结构体直接写入文件，如果换到另一台字节序不同的机器上读取，数据会完全反了。
    
- **类型强转（指针截断）**：通过 `char*` 指针读取 `int` 变量的第一个字节时，在大端和小端下得到的结果完全不同。
    
- **联合体（Union）**：利用 `union` 查看内存时，必须清楚当前机器的字节序。
