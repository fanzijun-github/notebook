
> [!NOTE] 说明  
> 仅供参考，没考到别怪我

---

## 一、期末考试结构与复习策略

### 考试形式

| 题型 | 分值 | 考察内容 | 复习重点 |
| :--- | :---: | :--- | :--- |
| **选择题** | 20分 | 基础概念辨析、语法细节、运算符优先级等 | 熟读教材章节小结，掌握基本定义 |
| **程序分析题** | 50分 | 程序填空、执行结果问答、程序改错/改写 | 精做所有例题与上机作业，理解控制流与函数调用 |
| **程序设计题** | 30分 | 综合应用（如数组处理、字符串操作、简单算法） | 动手编写并调试典型题目，掌握函数封装思想 |

> **考试要点**：课本基础知识、教材例题、上机作业。

---

## 二、概述：Linux环境下程序编译与文件后缀名

在 Linux 环境下，C 程序从源码到可执行文件需经历**预处理 → 编译 → 汇编 → 链接**四个阶段。不同阶段生成不同类型的文件。

### 文件类型与后缀名

| 后缀名 | 含义 | 示例 |
| :--- | :--- | :--- |
| `.c` | C语言的源代码 | `genlib.c` |
| `.h` | C语言的头文件，声明了常量、宏、系统全局变量和函数原型等。通过 `#include` 引入 | `simpio.h` |
| `.o` | 编译过程中生成的目标文件 | `strlib.o` |
| `.a` | 静态库（把多个 `.o` 文件打包） | `libcs.a` |
| （无后缀） | （Linux下）一般是你编译好的可执行文件，通过 `./<文件名>` 来执行 | `test` |

### 教学库列表（作者提供）

| 库名 | 作用 |
| :--- | :--- |
| `exception` | 异常处理 |
| `genlib` | 核心库，定义了 `bool`、`string`、`stream` 类型等 |
| `graphics` | 图形函数库 |
| `random` | 随机数库 |
| `simpio` | 输入数据读取库 |
| `strlib` | 简单字符串库 |

#### 图形库 `graphics.h` 函数表

| 函数 | 参数 | 返回值 | 功能说明 |
| :--- | :--- | :--- | :--- |
| `InitGraphics` | 无 | `void` | 创建图形窗口，必须最先调用 |
| `MovePen` | `double x, y` | `void` | 将画笔移动到绝对坐标 (x, y)，不绘图 |
| `DrawLine` | `double dx, dy` | `void` | 从当前点绘制一条相对位移为 (dx, dy) 的直线 |
| `DrawArc` | `double r, start, sweep` | `void` | 从当前点开始绘制圆弧：<br>• `r`：半径（英寸）<br>• `start`：起始角度（°，从3点钟逆时针）<br>• `sweep`：扫掠角度（°，正=逆时针，负=顺时针） |
| `GetWindowWidth` | 无 | `double` | 返回图形窗口宽度（英寸） |
| `GetWindowHeight` | 无 | `double` | 返回图形窗口高度（英寸） |
| `GetCurrentX` | 无 | `double` | 返回画笔当前 x 坐标（英寸） |
| `GetCurrentY` | 无 | `double` | 返回画笔当前 y 坐标（英寸） |

---

#### 随机数库 `random.h` 函数表

| 函数 | 参数 | 返回值 | 功能说明 | 示例 |
| :--- | :--- | :--- | :--- | :--- |
| `Randomize` | 无 | `void` | 用当前时间等不可预测值初始化随机种子 | `Randomize();` |
| `RandomInteger` | `int low, high` | `int` | 返回 `[low, high]` 内的随机整数（含端点） | `die = RandomInteger(1, 6);` |
| `RandomReal` | `double low, high` | `double` | 返回 `[low, high)` 内的随机实数（左闭右开） | `x = RandomReal(0.0, 1.0);` |
| `Random Chance` | `double p` | `bool` | 以概率 `p` 返回 `TRUE`（`0 ≤ p ≤ 1`） | ```c if (RandomChance(0.5)) printf("Heads\n"); ``` |

---

#### 输入输出库 `simpio.h` 函数表

| 函数 | 参数 | 返回值 | 功能说明 | 输入示例 | 是否重试 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `GetInteger` | 无 | `int` | 从 `stdin` 读取一个整数 | `"42"` → 42<br>`"-7 "` → -7<br>`"12.3"` → 无效（重试） | ✅ 是 |
| `GetLong` | 无 | `long` | 从 `stdin` 读取一个长整型 | `"1234567890"` → 对应 long 值 | ✅ 是 |
| `GetReal` | 无 | `double` | 从 `stdin` 读取一个浮点数 | `"3.1415"` → 3.1415<br>`"-2e5"` → -200000.0 | ✅ 是 |
| `GetLine` | 无 | `string` | 从 `stdin` 读取一整行（不含换行符） | `"Hello, world!"` → `"Hello, world!"` | ❌ 否（总是成功，即使空行） |
| `ReadLine` | `FILE *infile` | `string` 或 `NULL` | 从文件读取一行（不含换行符）；若到文件末尾，返回 `NULL` | 同上 | ❌ 否 |

---

#### 字符串库 `strlib.h` 函数表

##### 第一类：基本操作

| 函数 | 功能 | 示例 |
| :--- | :--- | :--- |
| `Concat(s1, s2)` | 拼接字符串 | `"Hi" + "!"` → `"Hi!"` |
| `IthChar(s, i)` | 安全获取第 `i` 个字符 | `IthChar("abc", 1)` → `'b'` |
| `SubString(s, p1, p2)` | 提取子串（含边界修正） | `SubString("hello", 1, 3)` → `"ell"` |
| `CharToString(ch)` | 字符转字符串 | `'X'` → `"X"` |
| `StringLength(s)` | 获取长度 | `"abc"` → `3` |
| `CopyString(s)` | 深拷贝字符串 | 用于跨库兼容 |

##### 第二类：比较

| 函数 | 功能 |
| :--- | :--- |
| `StringEqual(s1, s2)` | 判断两字符串是否完全相等（区分大小写） |
| `StringCompare(s1, s2)` | 字典序比较（类似 `strcmp`） |

##### 第三类：搜索

| 函数 | 功能 |
| :--- | :--- |
| `FindChar(ch, text, start)` | 从 `start` 开始找字符 `ch` |
| `FindString(str, text, start)` | 从 `start` 开始找子串 `str` |

##### 第四类：大小写转换

| 函数 | 功能 |
| :--- | :--- |
| `ConvertToLowerCase(s)` | 全转小写 |
| `ConvertToUpperCase(s)` | 全转大写 |

##### 第五类：数字 ↔ 字符串

| 函数 | 功能 | 注意 |
| :--- | :--- | :--- |
| `IntegerToString(n)` | 整数 → 字符串 | 支持负数 |
| `StringToInteger(s)` | 字符串 → 整数 | 非法输入会报错 |
| `RealToString(d)` | 浮点数 → 字符串 | 使用 `%G` 格式 |
| `StringToReal(s)` | 字符串 → 浮点数 | 非法输入会报错 |


### 常用编译命令

| 功能 | 命令示例 | 说明 |
| :--- | :--- | :--- |
| 编译生成目标文件 | `gcc -c *.c` | 将所有 `.c` 文件编译为 `.o` 文件 |
| 打包静态链接库 | `ar -rc libcs.a *.o` | 将当前目录下所有 `.o` 文件打包成 `libcs.a` |
| 编译自己写的代码（包含静态链接库） | `gcc mycode.c libcs.a -o mycode` | 链接主程序与教学库 |
| 运行自己的程序 | `./mycode` | 执行可执行文件 |

> **注**：`*` 是通配符，代表任意数量（包括零个）的任意字符。

---

## 三、通过例子学习：Hello World 程序结构

一个最简单的 C 程序包含以下要素：

```c
#include <stdio.h>     // 1.a. 库包含

// 1.b. 注释：用于解释代码，编译器忽略

int main() {           // 1.c. 主程序：程序入口点
    printf("Hello, World!\n"); // 1.d. Printf：标准输出函数
    return 0;
}
```

- **1.a. 库包含**：使用 `#include` 引入头文件，以使用其中声明的函数（如 `printf`）。
- **1.b. 注释**：`//` 单行注释；`/* ... */` 多行注释。
- **1.c. 主程序**：`main` 函数是程序唯一入口。
- **1.d. Printf**：格式化输出函数，需包含 `<stdio.h>`。

---

## 四、数据类型

C 语言基本数据类型包括：

- `int`：整型
- `double`：双精度浮点型
- `char`：字符型
- `bool`：布尔型（需 `#include "genlib.h"`）

---

## 五、表达式

### 3.a. 常量
- 整型常量：`123`, `-45`
- 浮点常量：`3.14`, `-0.001`
- 字符常量：`'A'`, `'\n'`
- 字符串常量：`"Hello"`
- 符号常量：`#define PI 3.14159`

### 3.b. 变量
- 声明：`类型 变量名;`
- 初始化：`int x = 10;`

### 3.c. 赋值
- 简单赋值：`x = 5;`
- 复合赋值：`x += 1;` 等价于 `x = x + 1;`

### 3.d. 运算符
- 算术运算符：`+ - * / %`
- 关系运算符：`< <= > >= == !=`
- 逻辑运算符：`&& || !`
- 位运算符（略，大纲未强调）

### 3.e. 类型转换
- 自动转换（隐式）
- 强制转换（显式）：`(int) 3.14`

---

## 六、问题求解：复合赋值

复合赋值是简化赋值操作的语法糖：
- `a += b` ⇨ `a = a + b`
- `a *= b` ⇨ `a = a * b`
- 等等。

---

## 七、控制语句

### 2.a. `for` 循环
```c
for (初始化; 条件; 更新) {
    // 循环体
}
```

### 2.b. `while` 循环
```c
while (条件) {
    // 循环体
}
```

### 2.c. `if-else` 语句
```c
if (条件) {
    // 真分支
} else {
    // 假分支
}
```

### 补充：`switch` 语句
```c
switch (表达式) {
    case 常量1:
        // ...
        break;
    default:
        // ...
}
```

### `for` 与 `while` 的相互转换
任何 `for` 循环均可等价转换为 `while` 循环，反之亦然（需手动管理循环变量）。

---

## 八、函数

### 1. 函数声明
```c
返回类型 函数名(参数列表);
// 例：int max(int a, int b);
```

### 2. `return` 语句
- 用于从函数返回值并终止执行。
- `void` 函数可省略 `return;` 或仅写 `return;`。

### 3. 调用函数
```c
result = max(5, 10);
```

### 4. 变量

#### 4.a. 全局变量
- 定义在所有函数之外。
- 作用域：从定义处到文件结束。
- 生存期：整个程序运行期间。

#### 4.b. 局部变量
- 定义在函数内部。
- 作用域：仅在该函数内。
- 生存期：函数调用开始至结束。

#### 4.c. 变量的作用域
- 决定了变量在程序中的可见范围。
- 遵循“就近原则”：局部变量屏蔽同名全局变量。

---

## 九、接口：随机数库的定义和使用

- 使用 `#include "random.h"`
- 核心函数：
  - `Randomize()`：初始化随机数种子（通常在 `main` 开头调用一次）
  - `RandomInteger(low, high)`：生成 `[low, high]` 之间的随机整数
  - `RandomReal(low, high)`：生成 `[low, high)` 之间的随机实数

示例：
 ```c
#include "random.h"
int main() {
    Randomize();
    int dice = RandomInteger(1, 6);
    return 0;
}
```

---

## 十、字符串与字符

### 理解字符和字符串
- **字符**：`char` 类型，用单引号 `'A'` 表示。
- **字符串**：以 `\0` 结尾的 `char` 数组，用双引号 `"Hello"` 表示。

### 1. 枚举类型

#### 1.a. 定义
```c
enum Color { RED, GREEN, BLUE };
```

#### 1.b. 操作
- 默认值：`RED=0`, `GREEN=1`, `BLUE=2`
- 可显式赋值：`enum Status { OFF=0, ON=1 };`
- 使用：`enum Color c = RED;`

### 2. 字符类型

#### 2.a. `char`
- 存储单个字符，本质是整数（ASCII码）。

#### 2.b. ASCII码
- 字符与整数一一对应，如 `'A' == 65`。

#### 2.c. 转义符
- `\n` 换行，`\t` 制表，`\\` 反斜杠，`\'` 单引号，`\"` 双引号。

#### 2.d. 字符运算
- 可进行算术运算：`'A' + 1 == 'B'`

#### 2.e. `ctype.h` 库
- 提供字符判断函数：
  - `isalpha(c)`：是否字母
  - `isdigit(c)`：是否数字
  - `isupper(c)` / `islower(c)`：大小写判断
  - `toupper(c)` / `tolower(c)`：大小写转换

### 3. `strlib.h` 库（教学库）

该库将字符串抽象为 `string` 类型，提供安全、易用的接口。

#### 3.a. 创建一个字符串
- 字符串常量直接赋值：`string s = "Hello";`

#### 3.b. 获取长度
- `int len = StringLength(s);`

#### 3.c. 从字符串里选择字符
- `char ch = IthChar(s, i);` // 带边界检查

#### 3.d. 连接两个字符串
- `string result = Concat(s1, s2);`

#### 3.e. 字符转换成字符串
- `string s = CharToString('A');`

#### 3.f. 抽取字符串一部分
- `string sub = SubString(s, start, finish);` // 闭区间 `[start, finish]`

#### 3.g. 比较两个字符串
- `bool eq = StringEqual(s1, s2);`
- `int cmp = StringCompare(s1, s2);` // <0, =0, >0

#### 3.h. 在字符串里搜索
- `int pos = FindChar(ch, text, start);`
- `int pos = FindString(substr, text, start);`

#### 3.i. 大小写转换
- `string lower = ConvertToLowerCase(s);`
- `string upper = ConvertToUpperCase(s);`

#### 3.j. 数值转换
- `string s = IntegerToString(123);`
- `int n = StringToInteger("123");`
- `string s = RealToString(3.14);`
- `double d = StringToReal("3.14");`

> **警告**：`StringToInteger` 和 `StringToReal` 在输入非法时会报错（异常处理）。

---

## 十一、数组

### 一维数组的定义、使用、参数传递

#### 1. 数组的声明
```c
int scores[10]; // 声明含10个整数的数组
```

#### 2. 通过下标选择数组元素
- 下标从 `0` 开始：`scores[0]` 是第一个元素。
- 最大下标为 `size - 1`。

#### 3. `sizeof()` 运算符
- `sizeof(scores)` 返回整个数组字节数。
- `sizeof(scores) / sizeof(scores[0])` 计算元素个数（仅在数组作用域内有效）。

#### 4. 数组的传参

##### 4.a. 传参机制
- 数组作为参数时，**传递的是首元素地址（指针）**。
- 函数无法获知原数组大小，**必须额外传入长度参数**。
- 函数内对数组的修改会**直接影响原数组**。

 示例：
 ```c
void printArray(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        printf("%d ", arr[i]);
    }
}
```

---

## 十二、指针

### 1. 声明指针变量
```c
int *p; // p 是一个指向 int 的指针
```

### 2. `&` 和 `*` 运算符
- `&x`：取变量 `x` 的地址
- `*p`：访问指针 `p` 所指向的值（解引用）

### 3. 引用传参
- 通过传递地址，函数可修改主调函数中的变量。
- 示例：`swap(&a, &b);`

### 4. 指针和数组

#### 4.a. 指针运算
- `p + i`：指向 `p` 后第 `i` 个同类型元素的地址
- `*(p + i)` 等价于 `p[i]`

#### 4.b. 指针与数组的关系
- **数组名是常量指针**：`arr == &arr[0]`
- `arr[i]` 等价于 `*(arr + i)`

> **注**：`scanf()` 和动态内存分配（`malloc`/`free`）不考。

---

## 十三、再论字符串

### 1. 字符数组

#### 1.a. 初始化
```c
char s1[] = "Hello";      // 自动加 '\0'
char s2[6] = {'H','e','l','l','o','\0'};
```

#### 1.b. 字符指针
```c
char *p = "Hello"; // p 指向字符串常量（不可修改）
```

#### 1.c. 输出函数
- `printf("%s", s);`
- `fputs(s, stdout);`

#### 1.d. 输入函数
- `scanf("%s", s);` // 遇空白停止，**有溢出风险**
- `fgets(s, size, stdin);` // 安全，推荐（但会读入换行符）

> **注**：`gets()` 已废弃，因无缓冲区保护。

### 2. `string.h` 库（标准库）

#### 2.a. `strcpy`
- `strcpy(dest, src);` // 复制字符串（危险）

#### 2.b. `strncpy`
- `strncpy(dest, src, n);` // 复制最多 n 字节

#### 2.c. `strlen`
- `size_t len = strlen(s);` // 返回长度（不含 `\0`）

#### 2.d. `strcat`
- `strcat(dest, src);` // 追加字符串

#### 2.e. `strcmp`
- `int cmp = strcmp(s1, s2);` // 字典序比较

### 3. 字符串惯用法

#### 3.a. 搜索字符串空字符惯用法
```c
for (i = 0; s[i] != '\0'; i++) {
    // 遍历字符串
}
```

#### 3.b. 复制字符串惯用法
```c
for (i = 0; (dest[i] = src[i]) != '\0'; i++);
```

### 4. 字符串数组（二维字符数组）
- 用于存储多个字符串。
- 声明：`char names[10][50];` // 10个字符串，每个最长49字符+`\0`
- 使用：`strcpy(names[0], "Alice");`

---

## 十四、记录（结构体）

### 记录的概念
- **记录（Record）**：将多个相关数据项组合成一个逻辑单元。
- 在 C 中通过 `struct` 实现。

### 创建记录的数据库
- 定义结构体类型：
  ```c
  struct Student {
      string name;
      int id;
      double gpa;
  };
  ```
- 声明结构体变量：
  ```c
  struct Student s1;
  s1.id = 12345;
  s1.name = "Alice";
  ```
- 数组 of 结构体：
  ```c
  struct Student class[30];
  class[0].gpa = 3.8;
  ```

> 结构体是构建更复杂数据结构（如链表）的基础。
