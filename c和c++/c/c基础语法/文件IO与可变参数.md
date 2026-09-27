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
#### `fscanf()`是按空白字符分割读取
重复调用会依次读到下一个字段（空格、制表符、换行都算分隔）
```c
#include <stdio.h>

int main(){
	FILE *fp=fopen("data.txt","r");
	if(fp==NULL){return -1;}
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
	FILE *fp=fopen("data.txt","r");
	if(fp==NULL){return -1;}
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

# 可变参数
- 需要 `<stdarg.h>` 头文件
- 
基础可变参数示例 `（stdarg.h）`

```c
#include <stdio.h>
#include <stdarg.h>

// ... 代表可变参数；前面必须至少有一个固定参数（count）
int sum(int count, ...)
{
    va_list ap;         // 1. 定义参数迭代器
    va_start(ap, count);// 2. 初始化，从count后面开始取参数

    int res = 0;
    for(int i = 0; i < count; i++)
    {
        int val = va_arg(ap, int); // 3. 取出一个int参数
        res += val;
    }
    va_end(ap);         // 4. 清理
    return res;
}

int main(void)
{
    printf("%d\n", sum(3, 10,20,30)); // count=3，后面3个数字
    return 0;
}
```
## 关键规则（C 可变参数的坑）

1. **必须至少有 1 个固定参数**，`...` 只能放最后面
    
    ```
    void func(...); // ❌ 非法，不能没有固定参数
    ```
    
2. `va_arg` **必须指定类型**，而且 C 不会自动知道传了多少个参数！
    - 所以一般要靠：第一个参数传数量（例子里的`count`），或者用标记值（比如 `-1` 代表结束）
3. **没有类型安全！** 你传 `double`，却用 `va_arg(ap, int)` 读取 → 直接乱码、崩溃。Python *args 会保留类型，C 不会检查。
4. **没有关键字参数**：C 完全不存在 `**kwargs`，没有键值对。
## 基本方法

```c
va_list ap;         // ① 定义一个游标，还没初始化
va_start(ap, count);// ② 把游标ap定位到【固定参数count的后面】，也就是第一个可变参数位置
va_arg(ap, int);    // ③ 读取当前位置的int，并且游标自动往后移动，指向下一个参数
va_end(ap);         // ④ 清理游标ap
```