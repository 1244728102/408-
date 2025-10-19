#  						C++ 教程知识点总结

## 第一阶段：C++ 基础语法 

### 1. 第一个 C 程序：HelloWorld

**核心代码**：

```cpp
#include <iostream>
using namespace std;

int main() {
    // 输出Hello World
    cout << "Hello World" << endl;
    return 0;
}
```

**说明**：

- `#include <iostream>`：引入输入输出流头文件（用于`cout`）；
- `using namespace std`：使用标准命名空间（避免重复写`std::cout`）；
- `main()`：程序入口函数，返回值为`int`；
- `cout << "内容"`：标准输出，`endl`表示换行并刷新缓冲区。

### 2. 程序的注释

用于解释代码，不参与编译，分两种类型：

#### （1）单行注释

```cpp
// 这是单行注释，只能写一行
int a = 10; // 行尾也可加单行注释
```

#### （2）多行注释

```cpp
/* 
这是多行注释
可以跨多行
*/
int b = 20;
```

### 3. 变量

#### （1）定义语法

```cpp
数据类型 变量名 = 初始值; // 推荐初始化，避免随机值
```

#### （2）常用数据类型及示例

| 数据类型 | 说明                 | 占用内存（32 位系统） | 示例代码                 |
| -------- | -------------------- | --------------------- | ------------------------ |
| `int`    | 整型（整数）         | 4 字节                | `int age = 18;`          |
| `char`   | 字符型（单个字符）   | 1 字节                | `char ch = 'A';`         |
| `float`  | 单精度浮点型（小数） | 4 字节                | `float score = 95.5f;`   |
| `double` | 双精度浮点型（小数） | 8 字节                | `double pi = 3.1415926;` |
| `bool`   | 布尔型（真 / 假）    | 1 字节                | `bool isPass = true;`    |
| `string` | 字符串型（C++ 特有） | 动态分配              | `string name = "张三";`  |

#### （3）`sizeof`关键字

用于计算数据类型 / 变量占用的内存大小（单位：字节）：

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    cout << "int占用内存：" << sizeof(int) << endl;       // 输出4
    cout << "char占用内存：" << sizeof(char) << endl;     // 输出1
    cout << "double占用内存：" << sizeof(double) << endl; // 输出8
    
    string str = "test";
    cout << "string占用内存：" << sizeof(str) << endl;    // 输出4（32位）/8（64位）
    return 0;
}
```

### 4. 常量

用于存储不可修改的值，分两种类型：

#### （1）宏常量（`#define`定义）

**语法**：`#define 常量名 常量值`（无分号，通常定义在文件最上方）

```cpp
#include <iostream>
#define DAY 7 // 宏常量：一周7天
using namespace std;

int main() {
    cout << "一周有" << DAY << "天" << endl; // 输出7
    // DAY = 8; // 错误：宏常量不可修改
    return 0;
}
```

#### （2）`const`修饰的常量

**语法**：`const 数据类型 常量名 = 常量值`（有分号，定义时必须初始化）

```cpp
#include <iostream>
using namespace std;

int main() {
    const int MONTH = 12; // const常量：一年12个月
    cout << "一年有" << MONTH << "个月" << endl; // 输出12
    // MONTH = 13; // 错误：const常量不可修改
    return 0;
}
```

### 5. 关键字与标识符命名规则

#### （1）常用关键字

`asm`、`auto`、`bool`、`break`、`case`、`catch`、`char`、`class`、`const`、`continue`、`default`、`delete`、`do`、`double`、`else`、`enum`、`extern`、`float`、`for`、`goto`、`if`、`int`、`long`、`new`、`operator`、`private`、`protected`、`public`、`register`、`return`、`short`、`signed`、`sizeof`、`static`、`struct`、`switch`、`template`、`this`、`throw`、`try`、`typedef`、`union`、`unsigned`、`using`、`virtual`、`void`、`volatile`、`while`

#### （2）标识符命名规则

1. 由**字母、数字、下划线**组成；
2. 不能以**数字开头**；
3. 不能与**关键字重名**；
4. 区分**大小写**（如`age`和`Age`是两个不同标识符）。

**示例**：

- 合法：`student_name`、`Score`、`num1`；
- 非法：`123abc`（数字开头）、`int`（关键字）、`student name`（含空格）。

### 6. 运算符

#### （1）算术运算符

用于数值计算，包括：`+`（加）、`-`（减）、`*`（乘）、`/`（除）、`%`（取模）、`++`（自增）、`--`（自减）。

**示例代码**：

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10, b = 3;
    
    // 加减乘除
    cout << "a + b = " << a + b << endl; // 13
    cout << "a - b = " << a - b << endl; // 7
    cout << "a * b = " << a * b << endl; // 30
    cout << "a / b = " << a / b << endl; // 3（整数除法，舍弃小数）
    cout << "a % b = " << a % b << endl; // 1（取余数，仅用于整数）
    
    // 自增（前置：先+1再使用；后置：先使用再+1）
    int c = a++; // c=10，a=11
    int d = ++a; // d=12，a=12
    cout << "c = " << c << ", d = " << d << endl; // 10, 12
    
    // 自减（同理）
    int e = b--; // e=3，b=2
    int f = --b; // f=1，b=1
    cout << "e = " << e << ", f = " << f << endl; // 3, 1
    
    return 0;
}
```

#### （2）赋值运算符

用于赋值，包括：`=`（直接赋值）、`+=`（加后赋值）、`-=`（减后赋值）、`*=`（乘后赋值）、`/=`（除后赋值）、`%=`（取模后赋值）。

**示例代码**：

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10;
    a += 5; // 等价于a = a + 5 → a=15
    cout << "a += 5 后：" << a << endl;
    
    a -= 3; // 等价于a = a - 3 → a=12
    cout << "a -= 3 后：" << a << endl;
    
    a *= 2; // 等价于a = a * 2 → a=24
    cout << "a *= 2 后：" << a << endl;
    
    a /= 4; // 等价于a = a / 4 → a=6
    cout << "a /= 4 后：" << a << endl;
    
    a %= 2; // 等价于a = a % 2 → a=0
    cout << "a %= 2 后：" << a << endl;
    
    return 0;
}
```

#### （3）比较运算符

用于比较两个值，返回`bool`类型（`true`/`false`），包括：`==`（等于）、`!=`（不等于）、`>`（大于）、`<`（小于）、`>=`（大于等于）、`<=`（小于等于）。

**示例代码**：

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10, b = 20;
    cout << "a == b ? " << (a == b) << endl; // 0（false）
    cout << "a != b ? " << (a != b) << endl; // 1（true）
    cout << "a > b ? " << (a > b) << endl;   // 0（false）
    cout << "a < b ? " << (a < b) << endl;   // 1（true）
    cout << "a >= b ? " << (a >= b) << endl; // 0（false）
    cout << "a <= b ? " << (a <= b) << endl; // 1（true）
    return 0;
}
```

#### （4）逻辑运算符

用于逻辑判断，返回`bool`类型，包括：`!`（非）、`&&`（与）、`||`（或）。

| 运算符 | 规则（以 a、b 为例）                   | 示例（a=10, b=0） |                             |      |      |      |
| ------ | -------------------------------------- | ----------------- | --------------------------- | ---- | ---- | ---- |
| `!`    | `!a`：a 为 true 则返回 false，反之则真 | `!a=0`，`!b=1`    |                             |      |      |      |
| `&&`   | 全 true 则 true，否则 false            | `a&&b=0`          |                             |      |      |      |
| `  | | `                 | 有 true 则 true，否则 false | `a   |      | b=1` |||||

**示例代码**：



```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10, b = 0;
    cout << "!a = " << !a << endl;     // 0
    cout << "!b = " << !b << endl;     // 1
    cout << "a && b = " << (a && b) << endl; // 0
    cout << "a || b = " << (a || b) << endl; // 1
    return 0;
}
```

### 7. 程序流程结构

#### （1）选择结构

##### ① 单行`if`语句

**语法**：`if (条件) 语句;`（条件为 true 则执行语句）

```cpp
#include <iostream>
using namespace std;

int main() {
    int score = 90;
    if (score >= 60)
        cout << "成绩合格" << endl; // 条件成立，执行此句
    return 0;
}
```

##### ② 多行`if`语句

**语法**：`if (条件) { 语句块; }`（语句块需用`{}`包裹）

```cpp
#include <iostream>
using namespace std;

int main() {
    int score = 55;
    if (score >= 60) {
        cout << "成绩合格" << endl;
        cout << "可以参加下一阶段学习" << endl;
    }
    return 0; // 条件不成立，不执行语句块
}
```

##### ③ 多条件`if-else if-else`语句

**语法**：

```cpp
if (条件1) { 语句块1; }
else if (条件2) { 语句块2; }
...
else { 语句块n; }
```

**案例：成绩分级**：

```cpp
#include <iostream>
using namespace std;

int main() {
    int score;
    cout << "请输入成绩：";
    cin >> score;
    
    if (score >= 90 && score <= 100) {
        cout << "优秀" << endl;
    } else if (score >= 80 && score < 90) {
        cout << "良好" << endl;
    } else if (score >= 60 && score < 80) {
        cout << "及格" << endl;
    } else if (score >= 0 && score < 60) {
        cout << "不及格" << endl;
    } else {
        cout << "成绩输入错误" << endl;
    }
    return 0;
}
```

##### ④ 嵌套`if`语句

**案例：三只小猪称体重**：

```cpp
#include <iostream>
using namespace std;

int main() {
    int pig1, pig2, pig3;
    cout << "请输入三只小猪的体重：";
    cin >> pig1 >> pig2 >> pig3;
    
    // 找出最重的小猪
    if (pig1 > pig2) {
        if (pig1 > pig3) {
            cout << "最重的小猪体重是：" << pig1 << endl;
        } else {
            cout << "最重的小猪体重是：" << pig3 << endl;
        }
    } else {
        if (pig2 > pig3) {
            cout << "最重的小猪体重是：" << pig2 << endl;
        } else {
            cout << "最重的小猪体重是：" << pig3 << endl;
        }
    }
    return 0;
}
```

##### ⑤ 三目运算符

**语法**：`条件 ? 表达式1 : 表达式2`（条件为 true 返回表达式 1，否则返回表达式 2）

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10, b = 20;
    int max = (a > b) ? a : b; // 找出a和b中的最大值
    cout << "最大值是：" << max << endl; // 20
    return 0;
}
```

##### ⑥ `switch`语句

**语法**：

```cpp
switch (表达式) {
    case 常量1: 语句块1; break;
    case 常量2: 语句块2; break;
    ...
    default: 语句块n; break;
}
```

**说明**：`表达式`必须是整型 / 字符型 / 枚举型；`break`用于跳出`switch`，否则会 “穿透” 执行后续`case`。

**示例代码**：

```cpp
#include <iostream>
using namespace std;

int main() {
    char grade = 'B';
    switch (grade) {
        case 'A':
            cout << "优秀" << endl;
            break;
        case 'B':
            cout << "良好" << endl;
            break;
        case 'C':
            cout << "及格" << endl;
            break;
        case 'D':
            cout << "不及格" << endl;
            break;
        default:
            cout << "成绩等级输入错误" << endl;
            break;
    }
    return 0;
}
```

#### （2）循环结构

##### ① `while`循环

**语法**：`while (条件) { 循环体; }`（条件为 true 则重复执行循环体）

**案例：猜数字游戏**：

```cpp
#include <iostream>
#include <ctime> // 用于time()函数
using namespace std;

int main() {
    // 生成1-100的随机数
    srand((unsigned int)time(NULL)); // 设置随机数种子（避免每次随机数相同）
    int randomNum = rand() % 100 + 1;
    int guessNum;
    
    while (true) {
        cout << "请猜一个1-100之间的数字：";
        cin >> guessNum;
        
        if (guessNum > randomNum) {
            cout << "猜大了，再试试！" << endl;
        } else if (guessNum < randomNum) {
            cout << "猜小了，再试试！" << endl;
        } else {
            cout << "恭喜你，猜对了！" << endl;
            break; // 猜对后跳出循环
        }
    }
    return 0;
}
```

##### ② `do-while`循环

**语法**：`do { 循环体; } while (条件);`（先执行 1 次循环体，再判断条件）

**案例：水仙花数**：

```cpp
#include <iostream>
using namespace std;

int main() {
    // 找出100-999之间的水仙花数（个位^3 + 十位^3 + 百位^3 = 本身）
    int num = 100;
    do {
        int ge = num % 10;     // 个位
        int shi = num / 10 % 10;// 十位
        int bai = num / 100;    // 百位
        
        if (ge*ge*ge + shi*shi*shi + bai*bai*bai == num) {
            cout << num << " 是水仙花数" << endl;
        }
        num++;
    } while (num <= 999);
    return 0;
}
```

##### ③ `for`循环

**语法**：`for (初始化表达式; 条件表达式; 更新表达式) { 循环体; }`

**案例：敲桌子**：

```cpp
#include <iostream>
using namespace std;

int main() {
    // 1-100中，含7或7的倍数的数字，输出“敲桌子”，否则输出数字
    for (int i = 1; i <= 100; i++) {
        if (i % 7 == 0 || i % 10 == 7 || i / 10 == 7) {
            cout << "敲桌子 " << endl;
        } else {
            cout << i << " ";
        }
    }
    return 0;
}
```

##### ④ 嵌套循环

**案例：乘法口诀表**：

```cpp
#include <iostream>
using namespace std;

int main() {
    // 输出9x9乘法口诀表
    for (int i = 1; i <= 9; i++) { // 行数：1-9
        for (int j = 1; j <= i; j++) { // 列数：1-当前行数
            cout << j << "x" << i << "=" << j*i << " ";
        }
        cout << endl; // 换行
    }
    return 0;
}
```

#### （3）跳转语句

##### ① `break`语句

用于跳出**当前循环**或**switch 语句**（不影响外层循环）：

```cpp
#include <iostream>
using namespace std;

int main() {
    for (int i = 1; i <= 10; i++) {
        if (i == 5) {
            break; // 当i=5时，跳出for循环
        }
        cout << i << " ";
    }
    // 输出：1 2 3 4
    return 0;
}
```

##### ② `continue`语句

用于跳过**当前循环的剩余部分**，直接进入下一次循环：

```cpp
#include <iostream>
using namespace std;

int main() {
    for (int i = 1; i <= 10; i++) {
        if (i == 5) {
            continue; // 跳过i=5的输出，直接进入下一次循环
        }
        cout << i << " ";
    }
    // 输出：1 2 3 4 6 7 8 9 10
    return 0;
}
```

##### ③ `goto`语句

用于跳转到指定标签（不推荐频繁使用，易导致代码混乱）：

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "1" << endl;
    goto FLAG; // 跳转到FLAG标签
    cout << "2" << endl;
    cout << "3" << endl;
    
FLAG: // 标签（格式：标签名:）
    cout << "4" << endl;
    // 输出：1 4
    return 0;
}
```

### 8. 数组

#### （1）一维数组

##### ① 定义方式（三种）

```cpp
#include <iostream>
using namespace std;

int main() {
    // 方式1：指定数组大小，不初始化（元素为随机值）
    int arr1[5];
    arr1[0] = 10; // 给第1个元素赋值（数组下标从0开始）
    cout << "arr1[0] = " << arr1[0] << endl; // 10
    
    // 方式2：指定数组大小，部分初始化（未初始化元素为0）
    int arr2[5] = {1, 2, 3};
    cout << "arr2[3] = " << arr2[3] << endl; // 0
    
    // 方式3：不指定数组大小，全初始化（大小由元素个数决定）
    int arr3[] = {1, 2, 3, 4, 5};
    cout << "arr3的大小：" << sizeof(arr3) / sizeof(arr3[0]) << endl; // 5
    return 0;
}
```

##### ② 数组名的用途

1. 计算数组总大小：`sizeof(数组名)`；
2. 获取数组首元素地址：`数组名`（或`&数组名[0]`）；
3. 遍历数组（结合`sizeof`计算长度）。

**示例代码**：

```cpp
#include <iostream>
using namespace std;

int main() {
    int arr[] = {10, 20, 30, 40, 50};
    
    // 1. 计算数组总大小
    cout << "数组总大小：" << sizeof(arr) << endl; // 20（5个int，每个4字节）
    
    // 2. 计算数组元素个数
    int arrLen = sizeof(arr) / sizeof(arr[0]);
    cout << "数组元素个数：" << arrLen << endl; // 5
    
    // 3. 获取数组首地址
    cout << "数组首地址：" << arr << endl;       // 0x...（地址值）
    cout << "首元素地址：" << &arr[0] << endl;   // 与数组首地址相同
    
    // 4. 遍历数组
    for (int i = 0; i < arrLen; i++) {
        cout << arr[i] << " "; // 10 20 30 40 50
    }
    return 0;
}
```

##### ③ 案例 1：五只小猪称体重

```cpp
#include <iostream>
using namespace std;

int main() {
    int pigs[] = {300, 350, 200, 400, 320};
    int max = pigs[0]; // 假设第1只小猪最重
    
    // 遍历数组找最大值
    for (int i = 1; i < 5; i++) {
        if (pigs[i] > max) {
            max = pigs[i];
        }
    }
    cout << "最重的小猪体重是：" << max << endl; // 400
    return 0;
}
```

##### ④ 案例 2：元素逆置

```cpp
#include <iostream>
using namespace std;

int main() {
    int arr[] = {1, 2, 3, 4, 5};
    int arrLen = sizeof(arr) / sizeof(arr[0]);
    int start = 0; // 起始下标
    int end = arrLen - 1; // 结束下标
    
    // 交换首尾元素，直到中间
    while (start < end) {
        int temp = arr[start];
        arr[start] = arr[end];
        arr[end] = temp;
        
        start++;
        end--;
    }
    
    // 输出逆置后的数组
    cout << "逆置后的数组：";
    for (int i = 0; i < arrLen; i++) {
        cout << arr[i] << " "; // 5 4 3 2 1
    }
    return 0;
}
```

##### ⑤ 案例 3：冒泡排序

```cpp
#include <iostream>
using namespace std;

int main() {
    int arr[] = {4, 2, 8, 0, 5, 7, 1, 3, 9};
    int arrLen = sizeof(arr) / sizeof(arr[0]);
    
    // 冒泡排序：每轮将最大元素“冒泡”到末尾
    for (int i = 0; i < arrLen - 1; i++) { // 轮数：n-1轮（n为元素个数）
        for (int j = 0; j < arrLen - 1 - i; j++) { // 每轮比较次数：n-1-i次
            if (arr[j] > arr[j+1]) {
                // 交换元素
                int temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
        }
    }
    
    // 输出排序后的数组
    cout << "排序后的数组：";
    for (int i = 0; i < arrLen; i++) {
        cout << arr[i] << " "; // 0 1 2 3 4 5 7 8 9
    }
    return 0;
}
```

#### （2）二维数组

##### ① 定义方式（四种）

```cpp
#include <iostream>
using namespace std;

int main() {
    // 方式1：指定行和列，全初始化
    int arr1[2][3] = {{1,2,3}, {4,5,6}};
    
    // 方式2：指定行和列，部分初始化（未初始化元素为0）
    int arr2[2][3] = {1,2,3,4};
    
    // 方式3：指定行，不指定列（列数由元素个数决定）
    int arr3[2][] = {{1,2}, {3,4,5}}; // 错误：列数不能省略
    
    // 方式4：不指定行，指定列（行数由元素个数决定）
    int arr4[][3] = {1,2,3,4,5,6};
    
    // 遍历二维数组
    for (int i = 0; i < 2; i++) { // 行
        for (int j = 0; j < 3; j++) { // 列
            cout << arr1[i][j] << " ";
        }
        cout << endl;
    }
    // 输出：
    // 1 2 3 
    // 4 5 6 
    return 0;
}
```

##### ② 数组名的用途

1. 计算二维数组总大小：`sizeof(数组名)`；
2. 计算一行大小：`sizeof(数组名[行号])`；
3. 计算一列大小：`sizeof(数组名[0][0])`；
4. 计算行数：`sizeof(数组名) / sizeof(数组名[0])`；
5. 计算列数：`sizeof(数组名[0]) / sizeof(数组名[0][0])`。

**示例代码**：

```cpp
#include <iostream>
using namespace std;

int main() {
    int arr[2][3] = {{1,2,3}, {4,5,6}};
    
    cout << "数组总大小：" << sizeof(arr) << endl; // 24（2行3列，每个int4字节）
    cout << "一行大小：" << sizeof(arr[0]) << endl; // 12（3列）
    cout << "一列大小：" << sizeof(arr[0][0]) << endl; // 4
    cout << "行数：" << sizeof(arr) / sizeof(arr[0]) << endl; // 2
    cout << "列数：" << sizeof(arr[0]) / sizeof(arr[0][0]) << endl; // 3
    
    return 0;
}
```

#####  ③ 案例：考试成绩统计

```cpp
#include <iostream>
using namespace std;

int main() {
    // 3个学生，2门课程的成绩
    int scores[3][2] = {{90, 85}, {88, 92}, {78, 80}};
    string names[] = {"张三", "李四", "王五"};
    
    // 统计每个学生的总分
    for (int i = 0; i < 3; i++) {
        int sum = 0;
        for (int j = 0; j < 2; j++) {
            sum += scores[i][j];
        }
        cout << names[i] << "的总分：" << sum << endl;
    }
    // 输出：
    // 张三的总分：175
    // 李四的总分：180
    // 王五的总分：158
    return 0;
}
```

### 9. 函数

#### （1）函数定义与调用

##### ① 定义语法

```cpp
返回值类型 函数名(参数列表) {
    函数体;
    return 返回值; // 若返回值类型为void，可省略return
}
```

##### ② 调用语法

`函数名(参数列表);`（参数个数、类型需与定义匹配）

**示例代码**：

```cpp
#include <iostream>
using namespace std;

// 定义：计算两个int的和
int add(int a, int b) {
    int sum = a + b;
    return sum; // 返回结果
}

// 定义：无返回值，输出问候语
void sayHello(string name) {
    cout << "Hello, " << name << "!" << endl;
    // 无return
}

int main() {
    // 调用add函数
    int result = add(10, 20);
    cout << "10 + 20 = " << result << endl; // 30
    
    // 调用sayHello函数
    sayHello("张三"); // Hello, 张三!
    
    return 0;
}
```

#### （2）函数参数传递

##### ① 值传递

- 特点：将实参的值**拷贝**给形参，形参修改不影响实参；
- 示例代码：

```cpp
#include <iostream>
using namespace std;

// 交换两个数（值传递，无法实现真正交换）
void swap(int x, int y) {
    int temp = x;
    x = y;
    y = temp;
    cout << "swap内：x=" << x << ", y=" << y << endl; // 20, 10
}

int main() {
    int a = 10, b = 20;
    swap(a, b);
    cout << "main内：a=" << a << ", b=" << b << endl; // 10, 20（实参未变）
    return 0;
}
```

#### （3）函数常见样式

1. **无参无返**：`void func() { ... }`；
2. **无参有返**：`int func() { return 10; }`；
3. **有参无返**：`void func(int a) { ... }`；
4. **有参有返**：`int func(int a, int b) { return a + b; }`。

#### （4）函数声明

- 作用：告诉编译器函数的名称、返回值类型、参数列表（无需实现）；
- 语法：`返回值类型 函数名(参数列表);`（分号结尾）；
- 位置：通常放在`main`函数上方或头文件中。

**示例代码**：

```cpp
#include <iostream>
using namespace std;

// 函数声明（先声明，后实现）
int max(int a, int b);

int main() {
    int result = max(15, 25);
    cout << "最大值是：" << result << endl; // 25
    return 0;
}

// 函数实现（在main之后）
int max(int a, int b) {
    return (a > b) ? a : b;
}
```

#### （5）函数分文件编写

**步骤**（视频标准流程）：

1. 创建`.h`头文件（声明函数）；
2. 创建`.cpp`源文件（实现函数，需包含头文件）；
3. 主文件包含头文件，调用函数。

**示例**：

- ① `swap.h`（头文件）：

```cpp
#ifndef SWAP_H // 防止头文件重复包含
#define SWAP_H

void swap(int &a, int &b); // 声明函数（引用传递，后续讲解）

#endif
```

- ② `swap.cpp`（源文件）：

```cpp
#include "swap.h"
#include <iostream>
using namespace std;

// 实现函数
void swap(int &a, int &b) {
    int temp = a;
    a = b;
    b = temp;
}
```

- ③ `main.cpp`（主文件）：

```cpp
#include <iostream>
#include "swap.h" // 包含头文件
using namespace std;

int main() {
    int a = 10, b = 20;
    swap(a, b);
    cout << "a=" << a << ", b=" << b << endl; // 20, 10（成功交换）
    return 0;
}
```

### 10. 指针

#### （1）指针定义与使用

- 定义：`数据类型 *指针变量名;`（`*`表示指针）；
- 核心：指针存储的是**变量的地址**，通过`*指针变量名`可访问地址对应的值。

**示例代码**：

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10;
    int *p; // 定义int类型指针p
    p = &a; // 将a的地址赋值给p（&是取地址符）
    
    cout << "a的值：" << a << endl;       // 10
    cout << "a的地址：" << &a << endl;    // 0x...（地址值）
    cout << "p存储的地址：" << p << endl; // 与a的地址相同
    cout << "p指向的值：" << *p << endl;  // 10（*p访问指针指向的值）
    
    // 通过指针修改a的值
    *p = 20;
    cout << "修改后a的值：" << a << endl; // 20
    return 0;
}
```

#### （2）指针所占内存空间

- 32 位操作系统：所有指针都占**4 字节**；
- 64 位操作系统：所有指针都占**8 字节**。

**示例代码**：

```cpp
#include <iostream>
using namespace std;

int main() {
    int *p1;
    char *p2;
    double *p3;
    
    cout << "int* 占用内存：" << sizeof(p1) << endl;   // 4（32位）/8（64位）
    cout << "char* 占用内存：" << sizeof(p2) << endl;  // 4（32位）/8（64位）
    cout << "double* 占用内存：" << sizeof(p3) << endl;// 4（32位）/8（64位）
    return 0;
}
```

#### （3）空指针与野指针

##### ① 空指针

- 定义：指向`NULL`（值为 0）的指针，用于初始化指针（避免野指针）；
- 注意：空指针指向的地址（0-255）是系统占用的，不可访问。

**示例代码**：

```cpp
#include <iostream>
using namespace std;

int main() {
    int *p = NULL; // 空指针
    // *p = 10; // 错误：空指针不可访问
    cout << "空指针p的地址：" << p << endl; // 0
    return 0;
}
```

##### ② 野指针

- 定义：指向非法地址的指针（未初始化或指向已释放的内存）；
- 危害：访问野指针可能导致程序崩溃；
- 避免：指针定义时初始化（空指针或合法地址）。

**错误示例**：

```cpp
#include <iostream>
using namespace std;

int main() {
    int *p; // 野指针（未初始化，指向随机地址）
    // *p = 10; // 危险：可能崩溃
    return 0;
}
```

#### （4）`const`修饰指针

三种场景，区别在于 “指针指向的值” 和 “指针本身” 是否可修改：

| 类型                 | 指针指向的值是否可改 | 指针本身是否可改 | 示例代码                      |
| -------------------- | -------------------- | ---------------- | ----------------------------- |
| `const int *p`       | 不可改               | 可改             | `*p=20;`（错），`p=&b;`（对） |
| `int *const p`       | 可改                 | 不可改           | `*p=20;`（对），`p=&b;`（错） |
| `const int *const p` | 不可改               | 不可改           | 两者都错                      |

**示例代码**：

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10, b = 20;
    
    // 1. const修饰指针指向的值（*p不可改）
    const int *p1 = &a;
    // *p1 = 20; // 错误
    p1 = &b; // 正确
    
    // 2. const修饰指针本身（p不可改）
    int *const p2 = &a;
    *p2 = 20; // 正确
    // p2 = &b; // 错误
    
    // 3. const修饰两者（都不可改）
    const int *const p3 = &a;
    // *p3 = 20; // 错误
    // p3 = &b; // 错误
    
    return 0;
}
```

#### （5）指针与数组

- 数组名本质是**数组首元素的地址**；
- 指针指向数组首地址后，可通过`*(p+i)`访问第`i`个元素（等价于`arr[i]`）。

**示例代码**：

```cpp
#include <iostream>
using namespace std;

int main() {
    int arr[] = {10, 20, 30, 40, 50};
    int *p = arr; // p指向数组首元素地址（等价于p=&arr[0]）
    
    // 遍历数组（两种方式等价）
    cout << "通过指针访问：";
    for (int i = 0; i < 5; i++) {
        cout << *(p + i) << " "; // 10 20 30 40 50
    }
    
    cout << "\n通过数组名访问：";
    for (int i = 0; i < 5; i++) {
        cout << arr[i] << " "; // 10 20 30 40 50
    }
    
    return 0;
}
```

#### （6）指针与函数

- 作用：通过指针传递参数，实现**修改实参的值**（解决值传递的局限性）；
- 案例：指针配合数组和函数实现排序（视频示例）。

**示例代码**：

```cpp
#include <iostream>
using namespace std;

// 函数：通过指针给数组排序（冒泡排序）
void bubbleSort(int *arr, int len) {
    for (int i = 0; i < len - 1; i++) {
        for (int j = 0; j < len - 1 - i; j++) {
            if (*(arr + j) > *(arr + j + 1)) {
                int temp = *(arr + j);
                *(arr + j) = *(arr + j + 1);
                *(arr + j + 1) = temp;
            }
        }
    }
}

// 函数：通过指针遍历数组
void printArray(int *arr, int len) {
    for (int i = 0; i < len; i++) {
        cout << *(arr + i) << " ";
    }
    cout << endl;
}

int main() {
    int arr[] = {4, 2, 8, 0, 5};
    int len = sizeof(arr) / sizeof(arr[0]);
    
    cout << "排序前：";
    printArray(arr, len); // 4 2 8 0 5
    
    bubbleSort(arr, len);
    
    cout << "排序后：";
    printArray(arr, len); // 0 2 4 5 8
    
    return 0;
}
```

### 11. 结构体

#### （1）结构体定义与使用

- 定义：`struct 结构体名 { 成员列表; };`（用于封装不同类型的数据）；
- 使用：三种方式（定义变量时加`struct`、省略`struct`、用`typedef`重命名）。

**示例代码**：

```cpp
#include <iostream>
#include <string>
using namespace std;

// 定义结构体：学生
struct Student {
    string name; // 姓名
    int age;     // 年龄
    int score;   // 成绩
};

// 方式3：用typedef重命名结构体
typedef struct Teacher {
    string name;
    int id;
} Teacher;

int main() {
    // 方式1：定义变量时加struct（C语言风格，C++可省略）
    struct Student s1;
    s1.name = "张三";
    s1.age = 18;
    s1.score = 95;
    
    // 方式2：定义变量时省略struct（C++风格）
    Student s2 = {"李四", 19, 92};
    
    // 方式3：用typedef重命名后的结构体
    Teacher t1 = {"王老师", 1001};
    
    // 输出
    cout << "s1：" << s1.name << ", " << s1.age << "岁, " << s1.score << "分" << endl;
    cout << "s2：" << s2.name << ", " << s2.age << "岁, " << s2.score << "分" << endl;
    cout << "t1：" << t1.name << ", 工号：" << t1.id << endl;
    
    return 0;
}
```

#### （2）结构体数组

- 定义：`struct 结构体名 数组名[数组大小] = { {成员1}, {成员2}, ... };`；
- 示例代码：

```cpp
#include <iostream>

#include <string>
using namespace std;

struct Student {
    string name;
    int score;
};

int main() {
    // 定义结构体数组并初始化
    Student arr[] = {
        {"张三", 90},
        {"李四", 85},
        {"王五", 95}
    };
    int len = sizeof(arr) / sizeof(arr[0]);
    
    // 遍历数组并修改成绩
    for (int i = 0; i < len; i++) {
        arr[i].score += 5; // 每人加5分
        cout << arr[i].name << "的新成绩：" << arr[i].score << endl;
    }
    // 输出：
    // 张三的新成绩：95
    // 李四的新成绩：90
    // 王五的新成绩：100
    return 0;
}
```

#### （3）结构体指针

- 定义：`struct 结构体名 *指针变量名;`；
- 访问成员：`指针变量名->成员名`（等价于`(*指针变量名).成员名`）。

**示例代码**：

```cpp
#include <iostream>
#include <string>
using namespace std;

struct Student {
    string name;
    int age;
};

int main() {
    Student s = {"张三", 18};
    Student *p = &s; // 结构体指针指向s的地址
    
    // 访问成员（两种方式等价）
    cout << "姓名：" << p->name << ", 年龄：" << p->age << endl; // 张三, 18
    cout << "姓名：" << (*p).name << ", 年龄：" << (*p).age << endl; // 张三, 18
    
    return 0;
}
```

#### （4）结构体嵌套结构体

- 场景：结构体的成员是另一个结构体；
- 示例代码：

```cpp
#include <iostream>
#include <string>
using namespace std;

// 定义结构体：生日
struct Birthday {
    int year;
    int month;
    int day;
};

// 定义结构体：学生（嵌套Birthday）
struct Student {
    string name;
    Birthday birth; // 成员是Birthday结构体
};

int main() {
    Student s;
    s.name = "张三";
    s.birth.year = 2005;
    s.birth.month = 9;
    s.birth.day = 10;
    
    cout << s.name << "的生日：" << s.birth.year << "年" << s.birth.month << "月" << s.birth.day << "日" << endl;
    // 输出：张三的生日：2005年9月10日
    return 0;
}
```

#### （5）结构体做函数参数

两种传递方式：值传递（实参不被修改）、地址传递（实参可被修改）。

**示例代码**：

```cpp
#include <iostream>
#include <string>
using namespace std;

struct Student {
    string name;
    int score;
};

// 1. 值传递（修改形参不影响实参）
void modify1(Student s) {
    s.score = 100;
    cout << "modify1内成绩：" << s.score << endl; // 100
}

// 2. 地址传递（修改形参影响实参）
void modify2(Student *s) {
    s->score = 100;
    cout << "modify2内成绩：" << s->score << endl; // 100
}

int main() {
    Student s = {"张三", 90};
    
    modify1(s);
    cout << "main内成绩（值传递后）：" << s.score << endl; // 90（未变）
    
    modify2(&s);
    cout << "main内成绩（地址传递后）：" << s.score << endl; // 100（已变）
    
    return 0;
}
```

#### （6）`const`修饰结构体指针

- 作用：防止函数内部修改结构体指针指向的值；
- 语法：`void func(const struct 结构体名 *p) { ... }`。

**示例代码**：

```cpp
#include <iostream>
#include <string>
using namespace std;

struct Student {
    string name;
    int score;
};

// const修饰结构体指针，防止修改指向的值
void print(const Student *s) {
    // s->score = 95; // 错误：const禁止修改
    cout << s->name << "的成绩：" << s->score << endl; // 正确：仅读取
}

int main() {
    Student s = {"张三", 90};
    print(&s); // 张三的成绩：90
    return 0;
}
```

### 12. 案例：通讯录管理系统

#### 1. 系统功能概述

通讯录管理系统是 C++ 基础阶段的核心案例，用于巩固**结构体、数组、函数、分支 / 循环逻辑**等基础知识点。系统支持以下核心功能：

- 菜单展示（功能入口）
- 退出系统（安全退出确认）
- 添加联系人（存储姓名、性别、年龄、电话、地址）
- 显示联系人（遍历展示所有存储的联系人信息）
- 删除联系人（按姓名查找并删除，支持后续元素前移）
- 查找联系人（按姓名精准查找并展示信息）
- 修改联系人（按姓名查找并更新信息）
- 清空通讯录（重置联系人数量，间接清空数据）

#### 2. 项目创建与环境准备

##### 2.1 知识点

- 使用 C++ 控制台项目（Windows 下推荐 Dev-C++/VS，Linux 下用 GCC 编译）
- 依赖标准输入输出库（`iostream`）和字符串库（`string`）
- 命名空间统一使用`std`（避免前缀冗余）

##### 2.2 基础代码框架

cpp







```cpp
// 包含必要头文件
#include <iostream>
#include <string>
using namespace std;  // 统一使用std命名空间

// 后续结构体、函数、主函数均在此框架下编写
int main() {
    // 主逻辑代码
    return 0;
}
```

#### 3. 核心结构体设计

##### 3.1 知识点

- **联系人结构体（`struct Person`）**：存储单个联系人的具体信息，包含 5 个字段（姓名、性别、年龄、电话、地址）。
- **通讯录结构体（`struct Addressbooks`）**：管理联系人集合，包含两个核心成员（联系人数组、当前联系人数量）。
- 宏定义（`#define MAX`）：限制通讯录最大存储容量（视频中设为 1000）。

##### 3.2 结构体代码实现

cpp







```cpp
// 1. 宏定义：通讯录最大存储容量（视频中固定为1000）
#define MAX 1000

// 2. 联系人结构体：存储单个联系人信息
struct Person {
    string m_Name;    // 姓名
    string m_Sex;     // 性别（视频中用字符串，如"男"/"女"）
    int m_Age;        // 年龄（整型）
    string m_Phone;   // 电话（字符串，支持区号/长号）
    string m_Addr;    // 地址（字符串，支持详细地址）
};

// 3. 通讯录结构体：管理所有联系人
struct Addressbooks {
    struct Person m_PersonArray[MAX];  // 联系人数组（存储MAX个联系人）
    int m_Size;                        // 当前联系人数量（初始为0）
};
```

#### 4. 菜单功能实现

##### 4.1 知识点

- 无返回值函数（`void`）：单独定义`showMenu()`函数，封装菜单打印逻辑。
- 固定格式输出：使用`cout`打印功能选项，视频中菜单包含 6 个功能 + 1 个退出选项。
- 函数调用：在`main`函数中循环调用菜单，作为功能入口。

##### 4.2 菜单代码实现

cpp







```cpp
// 菜单函数：打印功能选项
void showMenu() {
    cout << "-------------------------" << endl;
    cout << "-----  通讯录管理系统  -----" << endl;
    cout << "-----  1. 添加联系人  -----" << endl;
    cout << "-----  2. 显示联系人  -----" << endl;
    cout << "-----  3. 删除联系人  -----" << endl;
    cout << "-----  4. 查找联系人  -----" << endl;
    cout << "-----  5. 修改联系人  -----" << endl;
    cout << "-----  6. 清空通讯录  -----" << endl;
    cout << "-----  0. 退出通讯录  -----" << endl;
    cout << "-------------------------" << endl;
}

// 主函数中调用示例（后续整合到主逻辑）
int main() {
    Addressbooks abs;  // 创建通讯录对象
    abs.m_Size = 0;    // 初始化联系人数量为0

    int select = 0;    // 存储用户选择的功能编号
    while (true) {     // 循环显示菜单（直到选择0退出）
        showMenu();    // 调用菜单函数
        cin >> select; // 接收用户输入
        // 后续用switch-case处理功能选择
    }
    return 0;
}
```

#### 5. 退出功能实现

##### 5.1 知识点

- 分支逻辑（`switch-case`）：在`main`函数中接收用户选择（`select`），`case 0`对应退出功能。
- 确认逻辑：通过`char`类型接收用户确认（`y/n`），避免误操作。
- 循环终止：选择 “确认退出” 后，用`break`跳出`while(true)`循环，程序结束。

##### 5.2 退出功能代码实现（整合到主函数）

cpp







```cpp
int main() {
    Addressbooks abs;
    abs.m_Size = 0;
    int select = 0;

    while (true) {
        showMenu();
        cin >> select;

        switch (select) {
            case 0:  // 退出功能
                cout << "确定要退出通讯录吗？(y/n)" << endl;
                char confirm;
                cin >> confirm;
                if (confirm == 'y' || confirm == 'Y') {
                    cout << "欢迎下次使用！" << endl;
                    system("pause");  // 暂停显示（视频中保留此句，便于查看）
                    return 0;         // 直接结束程序
                }
                break;  // 若选择n，返回菜单

            // 其他case（后续功能）暂留空，后续逐步实现
            default:
                cout << "输入错误，请重新选择！" << endl;
                break;
        }
    }
    return 0;
}
```

#### 6. 添加联系人功能

##### 6.1 知识点

- 函数参数：使用**引用传递（`Addressbooks &abs`）**，直接修改原通讯录对象（避免值传递拷贝开销）。
- 边界判断：先检查通讯录是否已满（`abs.m_Size == MAX`），满则提示无法添加。
- 数据存储：通过`cin`接收用户输入，存入通讯录数组的当前位置（`abs.m_PersonArray[abs.m_Size]`），最后更新联系人数量（`abs.m_Size++`）。

##### 6.2 添加联系人代码实现

cpp







```cpp
// 添加联系人函数（引用传递通讯录对象）
void addPerson(Addressbooks &abs) {
    // 1. 判断通讯录是否已满
    if (abs.m_Size == MAX) {
        cout << "通讯录已满（最大容量1000），无法添加新联系人！" << endl;
        return;  // 直接返回，终止函数
    }

    // 2. 接收用户输入的联系人信息
    string name;
    cout << "请输入联系人姓名：" << endl;
    cin >> name;
    abs.m_PersonArray[abs.m_Size].m_Name = name;

    string sex;
    cout << "请输入联系人性别（男/女）：" << endl;
    cin >> sex;
    abs.m_PersonArray[abs.m_Size].m_Sex = sex;

    int age;
    cout << "请输入联系人年龄：" << endl;
    cin >> age;
    abs.m_PersonArray[abs.m_Size].m_Age = age;

    string phone;
    cout << "请输入联系人电话：" << endl;
    cin >> phone;
    abs.m_PersonArray[abs.m_Size].m_Phone = phone;

    string addr;
    cout << "请输入联系人地址：" << endl;
    cin >> addr;
    abs.m_PersonArray[abs.m_Size].m_Addr = addr;

    // 3. 更新联系人数量（当前位置存储完成后+1）
    abs.m_Size++;
    cout << "添加联系人成功！当前通讯录共有 " << abs.m_Size << " 个联系人" << endl;
}

// 主函数中添加case 1（调用添加函数）
switch (select) {
    case 0:  // 退出功能（不变）
        // ... 省略原有代码 ...
        break;
    case 1:  // 添加联系人
        addPerson(abs);
        system("pause");  // 暂停查看结果（视频中保留）
        system("cls");    // 清屏（视频中添加，便于下次显示菜单）
        break;
    // 其他case...
}
```

#### 7. 显示联系人功能

##### 7.1 知识点

- 空判断：先检查当前联系人数量（`abs.m_Size == 0`），为空则提示 “无联系人”。
- 循环遍历：用`for`循环遍历通讯录数组（从 0 到`abs.m_Size-1`），依次打印每个联系人的 5 个字段。
- 格式对齐：视频中用固定字符串分隔字段，确保输出清晰。

##### 7.2 显示联系人代码实现

cpp







```cpp
// 显示联系人函数
void showPerson(Addressbooks &abs) {
    // 1. 判断通讯录是否为空
    if (abs.m_Size == 0) {
        cout << "当前通讯录无联系人，请先添加！" << endl;
        return;
    }

    // 2. 遍历数组，打印所有联系人信息
    cout << "------------------------ 联系人列表 ------------------------" << endl;
    for (int i = 0; i < abs.m_Size; i++) {
        cout << "姓名：" << abs.m_PersonArray[i].m_Name 
             << " | 性别：" << abs.m_PersonArray[i].m_Sex 
             << " | 年龄：" << abs.m_PersonArray[i].m_Age 
             << " | 电话：" << abs.m_PersonArray[i].m_Phone 
             << " | 地址：" << abs.m_PersonArray[i].m_Addr << endl;
    }
    cout << "------------------------------------------------------------" << endl;
    cout << "共显示 " << abs.m_Size << " 个联系人" << endl;
}

// 主函数中添加case 2（调用显示函数）
switch (select) {
    // ... 省略case 0、case 1 ...
    case 2:  // 显示联系人
        showPerson(abs);
        system("pause");
        system("cls");
        break;
    // 其他case...
}
```

#### 8. 删除联系人功能（含检测联系人存在函数）

##### 8.1 知识点

- 辅助函数（`isExist`）：单独封装 “按姓名查找联系人” 逻辑，返回索引（存在则返回对应下标，不存在返回 - 1），供删除、查找、修改功能复用。
- 元素移动：删除联系人时，找到目标索引后，用`for`循环将后续联系人向前移动一位（覆盖被删除元素），最后更新联系人数量（`abs.m_Size--`）。

##### 8.2 代码实现（分两步：辅助函数 + 删除函数）

###### 8.2.1 辅助函数：检测联系人是否存在

cpp







```cpp
// 检测联系人是否存在（返回索引：存在返回i，不存在返回-1）
int isExist(Addressbooks &abs, string name) {
    for (int i = 0; i < abs.m_Size; i++) {
        // 若找到姓名匹配的联系人，返回当前索引
        if (abs.m_PersonArray[i].m_Name == name) {
            return i;
        }
    }
    // 遍历结束未找到，返回-1
    return -1;
}
```

###### 8.2.2 删除联系人函数

cpp







```cpp
// 删除联系人函数
void deletePerson(Addressbooks &abs) {
    // 1. 接收要删除的联系人姓名
    string name;
    cout << "请输入要删除的联系人姓名：" << endl;
    cin >> name;

    // 2. 调用辅助函数，判断是否存在
    int index = isExist(abs, name);
    if (index == -1) {
        cout << "未找到姓名为 \"" << name << "\" 的联系人，删除失败！" << endl;
        return;
    }

    // 3. 存在则删除：后续元素向前移动一位
    for (int i = index; i < abs.m_Size - 1; i++) {
        // 后一个元素覆盖前一个元素
        abs.m_PersonArray[i] = abs.m_PersonArray[i + 1];
    }

    // 4. 更新联系人数量（总数-1）
    abs.m_Size--;
    cout << "删除联系人 \"" << name << "\" 成功！当前通讯录剩余 " << abs.m_Size << " 个联系人" << endl;
}

// 主函数中添加case 3（调用删除函数）
switch (select) {
    // ... 省略case 0、1、2 ...
    case 3:  // 删除联系人
        deletePerson(abs);
        system("pause");
        system("cls");
        break;
    // 其他case...
}
```

#### 9. 查找联系人功能

##### 9.1 知识点

- 复用辅助函数：直接调用`isExist`函数查找联系人，避免重复代码。
- 精准查找：视频中按 “姓名完全匹配” 查找，找到后打印该联系人的所有信息，未找到则提示。

##### 9.2 查找联系人代码实现

cpp







```cpp
// 查找联系人函数
void findPerson(Addressbooks &abs) {
    // 1. 接收要查找的联系人姓名
    string name;
    cout << "请输入要查找的联系人姓名：" << endl;
    cin >> name;

    // 2. 调用辅助函数判断是否存在
    int index = isExist(abs, name);
    if (index == -1) {
        cout << "未找到姓名为 \"" << name << "\" 的联系人！" << endl;
        return;
    }

    // 3. 存在则打印该联系人信息
    cout << "------------------------ 查找结果 ------------------------" << endl;
    cout << "姓名：" << abs.m_PersonArray[index].m_Name 
         << " | 性别：" << abs.m_PersonArray[index].m_Sex 
         << " | 年龄：" << abs.m_PersonArray[index].m_Age 
         << " | 电话：" << abs.m_PersonArray[index].m_Phone 
         << " | 地址：" << abs.m_PersonArray[index].m_Addr << endl;
    cout << "------------------------------------------------------------" << endl;
}

// 主函数中添加case 4（调用查找函数）
switch (select) {
    // ... 省略case 0-3 ...
    case 4:  // 查找联系人
        findPerson(abs);
        system("pause");
        system("cls");
        break;
    // 其他case...
}
```

#### 10. 修改联系人功能

##### 10.1 知识点

- 复用辅助函数：通过`isExist`找到目标联系人索引，确保修改的是存在的联系人。
- 信息覆盖：找到索引后，重新接收用户输入的新信息，覆盖原联系人的字段（视频中支持修改所有字段）。

##### 10.2 修改联系人代码实现

cpp







```cpp
// 修改联系人函数
void modifyPerson(Addressbooks &abs) {
    // 1. 接收要修改的联系人姓名
    string name;
    cout << "请输入要修改的联系人姓名：" << endl;
    cin >> name;

    // 2. 调用辅助函数判断是否存在
    int index = isExist(abs, name);
    if (index == -1) {
        cout << "未找到姓名为 \"" << name << "\" 的联系人，修改失败！" << endl;
        return;
    }

    // 3. 存在则重新输入新信息（覆盖原数据）
    string newName;
    cout << "请输入新姓名：" << endl;
    cin >> newName;
    abs.m_PersonArray[index].m_Name = newName;

    string newSex;
    cout << "请输入新性别（男/女）：" << endl;
    cin >> newSex;
    abs.m_PersonArray[index].m_Sex = newSex;

    int newAge;
    cout << "请输入新年龄：" << endl;
    cin >> newAge;
    abs.m_PersonArray[index].m_Age = newAge;

    string newPhone;
    cout << "请输入新电话：" << endl;
    cin >> newPhone;
    abs.m_PersonArray[index].m_Phone = newPhone;

    string newAddr;
    cout << "请输入新地址：" << endl;
    cin >> newAddr;
    abs.m_PersonArray[index].m_Addr = newAddr;

    cout << "修改联系人 \"" << newName << "\" 成功！" << endl;
}

// 主函数中添加case 5（调用修改函数）
switch (select) {
    // ... 省略case 0-4 ...
    case 5:  // 修改联系人
        modifyPerson(abs);
        system("pause");
        system("cls");
        break;
    // 其他case...
}
```

#### 11. 清空通讯录功能

##### 11.1 知识点

- 简化逻辑：无需逐个删除数组元素，只需将 “当前联系人数量（`abs.m_Size`）” 设为 0 即可。因为后续功能（显示、删除等）均以`m_Size`作为判断依据，`m_Size=0`即视为 “无联系人”。
- 确认逻辑：视频中添加 “确认清空” 步骤，避免误操作。

##### 11.2 清空通讯录代码实现

cpp







```cpp
// 清空通讯录函数
void cleanPerson(Addressbooks &abs) {
    // 1. 确认清空（避免误操作）
    cout << "确定要清空所有联系人吗？(y/n)" << endl;
    char confirm;
    cin >> confirm;
    if (confirm != 'y' && confirm != 'Y') {
        cout << "已取消清空操作！" << endl;
        return;
    }

    // 2. 清空逻辑：将联系人数量设为0（核心）
    abs.m_Size = 0;
    cout << "通讯录已清空！当前联系人数量为：" << abs.m_Size << endl;
}

// 主函数中添加case 6（调用清空函数）
switch (select) {
    // ... 省略case 0-5 ...
    case 6:  // 清空通讯录
        cleanPerson(abs);
        system("pause");
        system("cls");
        break;
    // default（不变）...
}
```

#### 12. 完整代码整合

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

// 1. 宏定义：通讯录最大容量
#define MAX 1000

// 2. 联系人结构体
struct Person {
    string m_Name;    // 姓名
    string m_Sex;     // 性别
    int m_Age;        // 年龄
    string m_Phone;   // 电话
    string m_Addr;    // 地址
};

// 3. 通讯录结构体
struct Addressbooks {
    struct Person m_PersonArray[MAX];  // 联系人数组
    int m_Size;                        // 当前联系人数量
};

// 4. 菜单函数
void showMenu() {
    cout << "-------------------------" << endl;
    cout << "-----  通讯录管理系统  -----" << endl;
    cout << "-----  1. 添加联系人  -----" << endl;
    cout << "-----  2. 显示联系人  -----" << endl;
    cout << "-----  3. 删除联系人  -----" << endl;
    cout << "-----  4. 查找联系人  -----" << endl;
    cout << "-----  5. 修改联系人  -----" << endl;
    cout << "-----  6. 清空通讯录  -----" << endl;
    cout << "-----  0. 退出通讯录  -----" << endl;
    cout << "-------------------------" << endl;
}

// 5. 辅助函数：检测联系人是否存在（返回索引）
int isExist(Addressbooks &abs, string name) {
    for (int i = 0; i < abs.m_Size; i++) {
        if (abs.m_PersonArray[i].m_Name == name) {
            return i;
        }
    }
    return -1;
}

// 6. 添加联系人函数
void addPerson(Addressbooks &abs) {
    if (abs.m_Size == MAX) {
        cout << "通讯录已满（最大容量1000），无法添加新联系人！" << endl;
        return;
    }

    string name;
    cout << "请输入联系人姓名：" << endl;
    cin >> name;
    abs.m_PersonArray[abs.m_Size].m_Name = name;

    string sex;
    cout << "请输入联系人性别（男/女）：" << endl;
    cin >> sex;
    abs.m_PersonArray[abs.m_Size].m_Sex = sex;

    int age;
    cout << "请输入联系人年龄：" << endl;
    cin >> age;
    abs.m_PersonArray[abs.m_Size].m_Age = age;

    string phone;
    cout << "请输入联系人电话：" << endl;
    cin >> phone;
    abs.m_PersonArray[abs.m_Size].m_Phone = phone;

    string addr;
    cout << "请输入联系人地址：" << endl;
    cin >> addr;
    abs.m_PersonArray[abs.m_Size].m_Addr = addr;

    abs.m_Size++;
    cout << "添加联系人成功！当前通讯录共有 " << abs.m_Size << " 个联系人" << endl;
}

// 7. 显示联系人函数
void showPerson(Addressbooks &abs) {
    if (abs.m_Size == 0) {
        cout << "当前通讯录无联系人，请先添加！" << endl;
        return;
    }

    cout << "------------------------ 联系人列表 ------------------------" << endl;
    for (int i = 0; i < abs.m_Size; i++) {
        cout << "姓名：" << abs.m_PersonArray[i].m_Name 
             << " | 性别：" << abs.m_PersonArray[i].m_Sex 
             << " | 年龄：" << abs.m_PersonArray[i].m_Age 
             << " | 电话：" << abs.m_PersonArray[i].m_Phone 
             << " | 地址：" << abs.m_PersonArray[i].m_Addr << endl;
    }
    cout << "------------------------------------------------------------" << endl;
    cout << "共显示 " << abs.m_Size << " 个联系人" << endl;
}

// 8. 删除联系人函数
void deletePerson(Addressbooks &abs) {
    string name;
    cout << "请输入要删除的联系人姓名：" << endl;
    cin >> name;

    int index = isExist(abs, name);
    if (index == -1) {
        cout << "未找到姓名为 \"" << name << "\" 的联系人，删除失败！" << endl;
        return;
    }

    for (int i = index; i < abs.m_Size - 1; i++) {
        abs.m_PersonArray[i] = abs.m_PersonArray[i + 1];
    }

    abs.m_Size--;
    cout << "删除联系人 \"" << name << "\" 成功！当前通讯录剩余 " << abs.m_Size << " 个联系人" << endl;
}

// 9. 查找联系人函数
void findPerson(Addressbooks &abs) {
    string name;
    cout << "请输入要查找的联系人姓名：" << endl;
    cin >> name;

    int index = isExist(abs, name);
    if (index == -1) {
        cout << "未找到姓名为 \"" << name << "\" 的联系人！" << endl;
        return;
    }

    cout << "------------------------ 查找结果 ------------------------" << endl;
    cout << "姓名：" << abs.m_PersonArray[index].m_Name 
         << " | 性别：" << abs.m_PersonArray[index].m_Sex 
         << " | 年龄：" << abs.m_PersonArray[index].m_Age 
         << " | 电话：" << abs.m_PersonArray[index].m_Phone 
         << " | 地址：" << abs.m_PersonArray[index].m_Addr << endl;
    cout << "------------------------------------------------------------" << endl;
}

// 10. 修改联系人函数
void modifyPerson(Addressbooks &abs) {
    string name;
    cout << "请输入要修改的联系人姓名：" << endl;
    cin >> name;

    int index = isExist(abs, name);
    if (index == -1) {
        cout << "未找到姓名为 \"" << name << "\" 的联系人，修改失败！" << endl;
        return;
    }

    string newName;
    cout << "请输入新姓名：" << endl;
    cin >> newName;
    abs.m_PersonArray[index].m_Name = newName;

    string newSex;
    cout << "请输入新性别（男/女）：" << endl;
    cin >> newSex;
    abs.m_PersonArray[index].m_Sex = newSex;

    int newAge;
    cout << "请输入新年龄：" << endl;
    cin >> newAge;
    abs.m_PersonArray[index].m_Age = newAge;

    string newPhone;
    cout << "请输入新电话：" << endl;
    cin >> newPhone;
    abs.m_PersonArray[index].m_Phone = newPhone;

    string newAddr;
    cout << "请输入新地址：" << endl;
    cin >> newAddr;
    abs.m_PersonArray[index].m_Addr = newAddr;

    cout << "修改联系人 \"" << newName << "\" 成功！" << endl;
}

// 11. 清空通讯录函数
void cleanPerson(Addressbooks &abs) {
    cout << "确定要清空所有联系人吗？(y/n)" << endl;
    char confirm;
    cin >> confirm;
    if (confirm != 'y' && confirm != 'Y') {
        cout << "已取消清空操作！" << endl;
        return;
    }

    abs.m_Size = 0;
    cout << "通讯录已清空！当前联系人数量为：" << abs.m_Size << endl;
}

// 12. 主函数（程序入口）
int main() {
    Addressbooks abs;    // 创建通讯录对象
    abs.m_Size = 0;      // 初始化联系人数量为0
    int select = 0;      // 存储用户选择的功能编号

    while (true) {
        showMenu();      // 显示菜单
        cin >> select;   // 接收用户输入

        // 功能分支处理
        switch (select) {
            case 0:  // 退出系统
                cout << "确定要退出通讯录吗？(y/n)" << endl;
                char confirm;
                cin >> confirm;
                if (confirm == 'y' || confirm == 'Y') {
                    cout << "欢迎下次使用！" << endl;
                    system("pause");
                    return 0;
                }
                break;

            case 1:  // 添加联系人
                addPerson(abs);
                system("pause");
                system("cls");
                break;

            case 2:  // 显示联系人
                showPerson(abs);
                system("pause");
                system("cls");
                break;

            case 3:  // 删除联系人
                deletePerson(abs);
                system("pause");
                system("cls");
                break;

            case 4:  // 查找联系人
                findPerson(abs);
                system("pause");
                system("cls");
                break;

            case 5:  // 修改联系人
                modifyPerson(abs);
                system("pause");
                system("cls");
                break;

            case 6:  // 清空通讯录
                cleanPerson(abs);
                system("pause");
                system("cls");
                break;

            default:  // 输入错误
                cout << "输入错误，请重新选择！" << endl;
                system("pause");
                system("cls");
                break;
        }
    }

    return 0;
}
```

编辑分享





生成一个通讯录管理系统的代码示例。

如何优化通讯录管理系统的代码？

除了 C++，还有哪些语言可以实现通讯录管理系统？

对象是类的 “实例”，创建后通过`.`访问成员（变量 / 函数）：

cpp





```cpp
// 延续上面的Person类定义
int main() {
    // 方式1：栈区创建对象（常用）
    Person p1;
    // 访问成员变量
    p1.name = "张三";
    p1.age = 18;
    // 访问成员函数
    p1.showInfo(); // 输出：姓名：张三，年龄：18

    // 方式2：堆区创建对象（用new，需手动释放）
    Person *p2 = new Person;
    // 访问成员变量（指针用“->”）
    p2->name = "李四";
    p2->age = 19;
    // 访问成员函数
    p2->setInfo("李四", 20); // 修改年龄为20
    p2->showInfo(); // 输出：姓名：李四，年龄：20

    // 释放堆区内存（避免内存泄漏）
    delete p2;
    p2 = NULL; // 防止野指针

    return 0;
}
```



## 第二阶段：C++ 核心特性（面向对象）

### 一、程序的内存模型（视频重点：分区与生命周期）

C++ 程序运行时，内存会被划分为**4 个核心区域**，不同区域存储的数据有不同的 “生命周期” 和 “访问规则”，理解内存模型是避免内存泄漏、野指针等问题的关键。

#### 1. 内存分区详解（视频标准划分）

内存分为**代码区、全局区、栈区、堆区**，各分区的功能、存储内容、生命周期完全不同，具体对比如下：

| 内存分区   | 存储内容                                                     | 生命周期                                                     | 分配 / 释放方式                           | 关键特点                                                     |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ----------------------------------------- | ------------------------------------------------------------ |
| **代码区** | 程序的二进制指令（如`main`函数、自定义函数的执行代码）       | 程序启动时分配，程序结束时释放                               | 操作系统自动管理                          | 只读（防止意外修改指令）、共享（多个进程可共享同一代码）     |
| **全局区** | 1. 全局变量（定义在函数外的变量）2. 静态变量（`static`修饰的变量，无论全局 / 局部）3. 字符串常量（如`"hello world"`）4. 全局常量（`const`修饰的全局变量） | 程序启动时分配，程序结束时释放                               | 操作系统自动管理                          | 变量初始化默认值为 0（全局未初始化变量也在全局区，值为 0）   |
| **栈区**   | 1. 局部变量（定义在函数内的变量）2. 函数参数（如`void func(int a)`中的`a`）3. 函数返回值（临时存储） | 进入作用域（如函数调用）时分配，离开作用域（如函数返回）时自动释放 | 操作系统自动管理（栈帧机制，先进后出）    | 分配速度快，但空间有限（默认栈大小几 MB），不允许手动释放    |
| **堆区**   | 程序员手动申请的动态内存（如`new`创建的变量 / 数组）         | 手动分配（`new`）时创建，手动释放（`delete`）时销毁；若未释放，程序结束后由操作系统回收 | 程序员手动管理（`new`分配，`delete`释放） | 空间大（可利用系统大部分内存），分配速度慢，必须手动释放（否则内存泄漏） |

#### 2. 各分区代码验证（视频核心案例）

通过打印变量地址和生命周期，直观理解各分区特点：

cpp



```cpp
#include <iostream>
using namespace std;

// 1. 全局区数据（全局变量、静态变量、字符串常量、全局常量）
// 全局变量（函数外定义）
int g_a = 10;
int g_b;  // 全局未初始化变量（默认值0，仍在全局区）

// 全局静态变量
static int g_static_a = 20;
static int g_static_b;  // 全局静态未初始化变量（默认0）

// 全局常量
const int g_const_a = 30;

int main() {
    // 2. 全局区数据验证（打印地址：全局区地址范围相近）
    cout << "=== 全局区数据地址 ===" << endl;
    cout << "全局变量g_a地址：" << (long long)&g_a << endl;       // 全局区
    cout << "全局变量g_b地址：" << (long long)&g_b << endl;       // 全局区
    cout << "全局静态变量g_static_a地址：" << (long long)&g_static_a << endl;  // 全局区
    cout << "全局静态变量g_static_b地址：" << (long long)&g_static_b << endl;  // 全局区
    cout << "全局常量g_const_a地址：" << (long long)&g_const_a << endl;       // 全局区
    cout << "字符串常量地址：" << (long long)"hello world" << endl;            // 全局区

    // 3. 栈区数据（局部变量、函数参数）
    cout << "\n=== 栈区数据地址 ===" << endl;
    int s_a = 100;  // 局部变量（栈区）
    int s_b;        // 局部未初始化变量（栈区，值随机）
    static int s_static_a = 200;  // 局部静态变量（全局区，非栈区！）
    const int s_const_a = 300;    // 局部常量（栈区，非全局区！）

    cout << "局部变量s_a地址：" << (long long)&s_a << endl;         // 栈区（地址与全局区差异大）
    cout << "局部变量s_b地址：" << (long long)&s_b << endl;         // 栈区
    cout << "局部静态变量s_static_a地址：" << (long long)&s_static_a << endl;  // 全局区（静态变量无论局部/全局都在全局区）
    cout << "局部常量s_const_a地址：" << (long long)&s_const_a << endl;       // 栈区（局部常量在栈区）

    // 4. 堆区数据（new手动分配）
    cout << "\n=== 堆区数据地址 ===" << endl;
    int* h_a = new int(1000);    // 堆区int变量
    int* h_arr = new int[5];     // 堆区int数组（5个元素）

    cout << "堆区变量h_a地址（指针本身在栈区，指向的内容在堆区）：" << (long long)h_a << endl;  // 堆区地址
    cout << "堆区数组h_arr地址：" << (long long)h_arr << endl;  // 堆区地址（与栈区、全局区地址均不同）

    // 手动释放堆区内存（视频重点：不释放会内存泄漏）
    delete h_a;
    h_a = NULL;  // 置空，避免野指针
    delete[] h_arr;  // 数组释放需加[]
    h_arr = NULL;

    return 0;
}
```

**视频关键结论**：

- 静态变量（`static`）无论定义在全局还是局部，都在**全局区**；
- 常量分 “全局常量”（全局区）和 “局部常量”（栈区）；
- 堆区地址与栈区、全局区差异最大，栈区地址通常比全局区小（不同系统可能有差异，但分区界限明显）。

### 二、引用（`&`，视频核心：“别名” 与本质）

引用是变量的**别名**，语法上是 “简化的指针”，但本质是 “指针常量”（`T* const`），必须初始化且不能更改指向，核心作用是 “避免拷贝、简化指针操作”。

#### 1. 引用的基础定义与使用（视频标准语法）

##### （1）语法格式

cpp







```cpp
// 定义引用：类型& 引用名 = 目标变量;（必须初始化，且“&”不是取地址，是引用符号）
int a = 10;
int& ref_a = a;  // ref_a是a的别名，操作ref_a等价于操作a
```

##### （2）核心特性（视频重点强调）

1. **必须初始化**：定义引用时必须绑定一个已存在的变量（不能绑定字面量，如`int& ref = 10;`错误，除非是`const`引用）；
2. **指向唯一**：引用一旦绑定变量，不能再绑定其他变量（本质是指针常量，指向不可改）；
3. **操作等价**：对引用的读写操作，完全等价于对目标变量的读写（引用本身不占内存空间，`sizeof(ref)`等于目标变量的大小）。

##### （3）基础代码示例

cpp







```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10;
    // 1. 定义引用（必须初始化）
    int& ref_a = a;  // 正确：绑定变量a
    // int& ref_b;    // 错误：引用未初始化

    // 2. 操作引用等价于操作变量
    cout << "a = " << a << ", ref_a = " << ref_a << endl;  // 10, 10
    ref_a = 20;  // 修改引用，等价于修改a
    cout << "a = " << a << ", ref_a = " << ref_a << endl;  // 20, 20

    // 3. 引用不能重新绑定其他变量
    int b = 30;
    ref_a = b;  // 错误理解：不是重新绑定b，而是将b的值赋给a（等价于a = b）
    cout << "a = " << a << ", b = " << b << ", ref_a = " << ref_a << endl;  // 30, 30, 30

    // 4. 引用的大小：与目标变量大小一致（证明引用不占额外空间）
    cout << "sizeof(a) = " << sizeof(a) << ", sizeof(ref_a) = " << sizeof(ref_a) << endl;  // 4, 4（int占4字节）

    return 0;
}
```

#### 2. 引用的常见使用场景（视频重点案例）

引用最常用的场景是**函数参数**和**函数返回值**，核心优势是 “避免拷贝开销”（尤其对大型对象，如结构体、类）。

##### （1）引用作为函数参数（替代指针，简化代码）

**问题场景**：用函数交换两个变量的值，传统指针方式代码繁琐，引用方式更简洁。

cpp







```cpp
#include <iostream>
using namespace std;

// 方式1：指针作为参数（传统方式，需解引用，代码繁琐）
void swap_by_ptr(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

// 方式2：引用作为参数（视频推荐，无需解引用，代码简洁）
void swap_by_ref(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;  // 直接操作引用，等价于操作实参
}

int main() {
    int x = 10, y = 20;

    // 指针方式交换
    swap_by_ptr(&x, &y);
    cout << "指针交换后：x = " << x << ", y = " << y << endl;  // 20, 10

    // 引用方式交换（无需传地址，直接传变量）
    swap_by_ref(x, y);
    cout << "引用交换后：x = " << x << ", y = " << y << endl;  // 10, 20（交换回来）

    return 0;
}
```

**视频关键优势**：

- 引用作为参数时，实参直接传递（无需传地址`&`），代码更直观；
- 函数内部无需解引用（`*`），避免指针操作错误（如野指针、空指针）。

##### （2）引用作为函数返回值（避免拷贝，注意生命周期）

**核心规则**：函数返回引用时，返回的变量必须是 “全局变量、静态变量或堆区变量”（不能返回局部变量的引用，因为局部变量在函数返回后被栈区释放，引用会变成野引用）。

cpp







```cpp
#include <iostream>
using namespace std;

// 情况1：返回全局变量的引用（合法，全局变量在全局区，生命周期长）
int g_num = 100;
int& return_global_ref() {
    return g_num;  // 返回全局变量g_num的引用
}

// 情况2：返回静态局部变量的引用（合法，静态变量在全局区）
int& return_static_ref() {
    static int s_num = 200;  // 静态局部变量，在全局区
    return s_num;
}

// 情况3：返回局部变量的引用（非法！局部变量在栈区，函数返回后释放）
int& return_local_ref() {
    int l_num = 300;  // 局部变量，栈区
    return l_num;     // 错误：返回后l_num已释放，引用无效
}

int main() {
    // 合法场景1：操作全局变量的引用
    int& ref1 = return_global_ref();
    ref1 = 150;  // 修改引用，等价于修改g_num
    cout << "g_num = " << g_num << endl;  // 150

    // 合法场景2：操作静态局部变量的引用
    int& ref2 = return_static_ref();
    ref2 = 250;  // 修改引用，等价于修改s_num
    cout << "s_num = " << return_static_ref() << endl;  // 250

    // 非法场景3：操作局部变量的引用（编译可能通过，但运行结果随机）
    int& ref3 = return_local_ref();
    cout << "ref3 = " << ref3 << endl;  // 随机值（栈区内存已被覆盖）

    return 0;
}
```

#### 3. 常引用（`const`引用，视频重点：只读保护）

常引用是`const`修饰的引用，语法：`const 类型& 引用名 = 目标变量;`，核心作用是 “保护目标变量不被修改”，且可以绑定**字面量**或**临时变量**。

##### （1）核心特性

1. 常引用只能读取目标变量，不能修改（`ref = 10;`错误）；
2. 常引用可以绑定字面量（如`const int& ref = 5;`合法），普通引用不行；
3. 常引用可以接收不同类型的临时变量（如`double`转`int`的临时值）。

##### （2）代码示例

cpp







```cpp
#include <iostream>
using namespace std;

int main() {
    // 1. 常引用绑定普通变量（保护变量不被修改）
    int a = 10;
    const int& ref_a = a;  // 常引用
    cout << "ref_a = " << ref_a << endl;  // 10（可读取）
    // ref_a = 20;  // 错误：常引用不能修改目标变量
    a = 20;  // 允许：变量本身可以修改，常引用只是限制引用的操作
    cout << "ref_a = " << ref_a << endl;  // 20（变量修改后，引用值同步变化）

    // 2. 常引用绑定字面量（普通引用不行，常引用可以）
    // int& ref_lit = 100;  // 错误：普通引用不能绑定字面量
    const int& ref_lit = 100;  // 正确：常引用可以绑定字面量（编译器会创建临时变量）
    cout << "ref_lit = " << ref_lit << endl;  // 100

    // 3. 常引用接收不同类型的临时变量
    double d = 3.14;
    // int& ref_d = d;  // 错误：double转int会产生临时变量，普通引用不能绑定
    const int& ref_d = d;  // 正确：常引用可以绑定临时变量（临时变量值为3）
    cout << "ref_d = " << ref_d << endl;  // 3（临时变量的值，d本身还是3.14）
    cout << "d = " << d << endl;          // 3.14（原变量未修改）

    return 0;
}
```

**视频常见用途**：函数参数用常引用，既避免拷贝，又保护实参不被修改（如传递大型类对象时）：

cpp







```cpp
// 常引用作为函数参数（保护实参不被修改）
void show_num(const int& num) {
    // num = 100;  // 错误：常引用不能修改
    cout << "num = " << num << endl;  // 只能读取
}
```

#### 4. 引用与指针的区别（视频对比总结）

很多初学者混淆引用和指针，两者核心区别如下：

| 对比维度 | 引用（Reference）                             | 指针（Pointer）                              |
| -------- | --------------------------------------------- | -------------------------------------------- |
| 定义语法 | `类型& 引用名 = 变量;`（必须初始化）          | `类型* 指针名;`（可初始化，也可后续赋值）    |
| 指向性   | 一旦绑定变量，不能更改指向（指向唯一）        | 可以随时更改指向（如`p = &b;`）              |
| 空值     | 不能指向空（`NULL`），必须绑定有效变量        | 可以指向空（`int* p = NULL;`），需避免野指针 |
| 内存空间 | 不占额外内存（`sizeof(ref)`等于目标变量大小） | 占内存（32 位系统 4 字节，64 位 8 字节）     |
| 操作方式 | 直接操作（如`ref = 10;`），无需解引用         | 需解引用（`*p = 10;`），否则操作的是指针地址 |
| 安全性   | 更安全（无空引用，避免指针操作错误）          | 风险高（可能出现野指针、空指针访问）         |

### 三、函数高级（重载、默认参数、函数指针、内联函数）

函数高级特性是 C++ 对 C 函数的扩展，核心目的是 “提升代码灵活性和效率”。

#### 1. 函数重载（Function Overloading，视频核心：同名不同参）

函数重载是指在**同一作用域**内，定义多个 “函数名相同但参数列表不同” 的函数，编译器会根据 “实参的个数、类型、顺序” 自动匹配对应的函数。

##### （1）函数重载的条件（视频强制要求）

必须同时满足以下 3 个条件，否则不构成重载：

1. 函数名**完全相同**；
2. 参数列表**不同**（至少满足以下一种）：
	- 参数个数不同（如`func(int a)`和`func(int a, int b)`）；
	- 参数类型不同（如`func(int a)`和`func(double a)`）；
	- 参数顺序不同（如`func(int a, double b)`和`func(double a, int b)`）；
3. 函数返回值**不同不构成重载**（如`int func()`和`double func()`不构成重载，调用时无法区分）。

##### （2）代码示例（合法与非法重载）

cpp



```cpp
#include <iostream>
using namespace std;

// 1. 合法重载：参数个数不同
void print_num(int a) {
    cout << "int类型：" << a << endl;
}
void print_num(int a, int b) {
    cout << "两个int类型：" << a << ", " << b << endl;
}

// 2. 合法重载：参数类型不同
void print_num(double a) {
    cout << "double类型：" << a << endl;
}

// 3. 合法重载：参数顺序不同
void print_order(int a, double b) {
    cout << "int在前，double在后：" << a << ", " << b << endl;
}
void print_order(double a, int b) {
    cout << "double在前，int在后：" << a << ", " << b << endl;
}

// 4. 非法：返回值不同，不构成重载（编译报错：函数重复定义）
// int add(int a, int b) { return a + b; }
// double add(int a, int b) { return (double)(a + b); }

int main() {
    // 调用重载函数：编译器自动匹配参数
    print_num(10);          // 匹配void print_num(int a)
    print_num(10, 20);      // 匹配void print_num(int a, int b)
    print_num(3.14);        // 匹配void print_num(double a)
    print_order(5, 3.14);   // 匹配void print_order(int a, double b)
    print_order(3.14, 5);   // 匹配void print_order(double a, int b)

    return 0;
}
```

##### （3）函数重载的注意事项（视频易错点）

- **作用域问题**：不同作用域的函数不构成重载（如全局函数和类的成员函数，即使同名同参也不算）；
- **默认参数冲突**：若函数有默认参数，可能导致重载匹配歧义（如`func(int a)`和`func(int a, int b=10)`，调用`func(5)`时无法确定匹配哪个）；
- **引用与 const 引用**：`func(int& a)`和`func(const int& a)`构成重载（参数类型不同：普通引用 vs 常引用）。

#### 2. 函数的默认参数（Default Parameters，视频核心：简化调用）

默认参数是指函数定义时，给参数指定 “默认值”，调用函数时若未传递该参数，编译器会自动使用默认值。

##### （1）核心规则（视频重点强调）

1. **从右往左连续指定**：默认参数必须从最右边的参数开始，依次向左指定，不能跳过中间参数（如`func(int a, int b=10, int c=20)`合法，`func(int a=10, int b, int c=20)`非法）；
2. **不能重复定义**：默认参数不能在 “函数声明” 和 “函数实现” 中重复指定（要么在声明中指定，要么在实现中指定，推荐在声明中指定）；
3. **与重载的冲突**：若默认参数导致重载匹配歧义，编译器会报错（如上述重载注意事项中的案例）。

##### （2）代码示例

cpp







```cpp
#include <iostream>
using namespace std;

// 1. 函数声明：指定默认参数（推荐在声明中指定）
void show_info(string name, int age = 18, string gender = "未知");

// 2. 函数实现：不能重复指定默认参数（否则编译报错）
void show_info(string name, int age, string gender) {
    cout << "姓名：" << name << "，年龄：" << age << "，性别：" << gender << endl;
}

// 3. 错误示例：默认参数不是从右往左连续（编译报错）
// void func(int a=10, int b, int c=20) { }

int main() {
    // 调用函数：可传递部分参数（未传递的用默认值）
    show_info("张三");                  // 只传name，age=18，gender="未知"
    show_info("李四", 20);             // 传name和age，gender="未知"
    show_info("王五", 22, "男");       // 传所有参数，不用默认值

    return 0;
}
```

#### 3. 函数指针（Pointer to Function，视频核心：指向函数的指针）

函数指针是 “指向函数的指针变量”，存储的是函数的**入口地址**，核心用途是 “回调函数”（如排序算法中的比较函数、事件响应函数）。

##### （1）语法格式

cpp







```cpp
// 函数指针定义：返回值类型 (*指针名)(参数列表);
// 注意：*指针名必须加括号，否则变成“返回指针的函数”
int add(int a, int b) { return a + b; }
int (*p_add)(int a, int b) = add;  // 定义函数指针p_add，指向add函数
```

##### （2）核心用法（视频案例：回调函数）

以 “自定义排序” 为例，用函数指针传递不同的比较规则，实现不同的排序逻辑：

cpp







```cpp
#include <iostream>
using namespace std;

// 1. 比较函数1：升序（a < b返回true）
bool compare_asc(int a, int b) {
    return a < b;
}

// 2. 比较函数2：降序（a > b返回true）
bool compare_desc(int a, int b) {
    return a > b;
}

// 3. 排序函数：用函数指针接收比较规则（回调函数的核心）
void sort_arr(int arr[], int size, bool (*compare)(int a, int b)) {
    // 冒泡排序
    for (int i = 0; i < size - 1; i++) {
        for (int j = 0; j < size - 1 - i; j++) {
            // 调用函数指针指向的比较函数，决定是否交换
            if (compare(arr[j], arr[j + 1]) == false) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

// 4. 打印数组
void print_arr(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;
}

int main() {
    int arr[] = {3, 1, 4, 1, 5, 9};
    int size = sizeof(arr) / sizeof(arr[0]);

    // 调用排序函数：传递“升序比较函数”的地址
    cout << "升序排序：";
    sort_arr(arr, size, compare_asc);  // 函数名即函数地址
    print_arr(arr, size);  // 1 1 3 4 5 9

    // 调用排序函数：传递“降序比较函数”的地址
    cout << "降序排序：";
    sort_arr(arr, size, compare_desc);
    print_arr(arr, size);  // 9 5 4 3 1 1

    return 0;
}
```

#### 4. 内联函数（Inline Function，视频核心：优化性能）

内联函数是用`inline`关键字修饰的函数，编译器会将内联函数的 “代码体” 直接插入到函数调用的位置，**避免函数调用的开销**（如栈帧创建、参数传递、返回值处理）。

##### （1）核心特性（视频重点）

1. **替代宏函数**：内联函数有类型检查，比宏函数更安全（宏函数无类型检查，容易出错）；
2. **代码展开**：编译器对 inline 函数的处理是 “建议性” 的（不是强制），若函数体过大（如含循环、递归），编译器可能忽略 inline 关键字，按普通函数处理；
3. **定义位置**：内联函数的定义通常放在头文件中（若放在.cpp 文件中，其他文件包含头文件时无法找到定义，导致链接错误）。

##### （2）代码示例（内联函数 vs 宏函数）

cpp







```cpp
#include <iostream>
using namespace std;

// 1. 内联函数（有类型检查，安全）
inline int add_inline(int a, int b) {
    return a + b;  // 函数体小，适合内联
}

// 2. 宏函数（无类型检查，不安全）
#define ADD_MACRO(a, b) ((a) + (b))  // 宏定义需加括号，避免优先级问题

int main() {
    // 调用内联函数（编译器会将add_inline(3,4)替换为3+4）
    int res1 = add_inline(3, 4);
    cout << "内联函数结果：" << res1 << endl;  // 7

    // 调用宏函数（直接替换文本，无类型检查）
    int res2 = ADD_MACRO(3, 4);
    cout << "宏函数结果：" << res2 << endl;    // 7

    // 宏函数的风险：无类型检查（如传递不同类型）
    double res3 = ADD_MACRO(3.14, 5.28);  // 编译通过，但宏本身无类型
    cout << "宏函数（不同类型）结果：" << res3 << endl;  // 8.42

    return 0;
}
```

##### （3）内联函数的注意事项（视频易错点）

- 函数体不能过大：若函数含循环、递归、switch 等复杂结构，编译器不会将其作为内联函数（内联的目的是优化小函数的调用开销，大函数展开后会导致代码膨胀）；
- 避免内联虚函数：虚函数需要动态绑定（运行时确定调用哪个函数），内联是静态展开（编译时确定），两者冲突，虚函数加 inline 通常无效；
- 类的成员函数：类内定义的成员函数（如`class A { void func() { ... } };`）会被编译器自动视为内联函数（无需加 inline 关键字）。

编辑分享







### 四. 类和对象基础（视频标准定义与使用）

#### 1.类的对象

##### （1）类的本质与结构

类是**抽象的数据类型模板**，封装了 “属性（成员变量）” 和 “行为（成员函数）”，语法严格遵循`class`关键字定义，访问权限需显式声明（默认`private`）。

**视频核心代码（类的定义与实现）**：

```cpp
#include <iostream>
#include <string>
using namespace std;

// 类的定义：首字母大写（规范），包含访问权限、成员变量、成员函数
class Person {
    // 1. 访问权限：public（类内/类外均可访问）
public:
    // 成员变量（属性）
    string name;  // 姓名
    int age;      // 年龄

    // 2. 成员函数（行为）- 类内声明并实现
    void showPersonInfo() {
        cout << "姓名：" << name << "，年龄：" << age << endl;
    }

    // 成员函数（行为）- 类内声明，类外实现（视频重点强调“类名::作用域”）
    void setPersonInfo(string n, int a);

    // 访问权限：private（仅类内可访问，类外不可见）
private:
    string idCard;  // 身份证号（私有属性，仅类内函数可操作）

    // 访问权限：protected（类内可访问，子类可访问，类外不可见）
protected:
    string phone;   // 手机号（保护属性，后续继承用）

    // 类内函数可访问所有权限成员（视频示例）
    void setIdCard(string id) {
        idCard = id;  // private成员可被类内函数修改
    }
};

// 类外实现成员函数：必须加“类名::”指定作用域（视频重点，漏写会报错）
void Person::setPersonInfo(string n, int a) {
    name = n;
    age = a;
    // 类外实现的成员函数，仍可访问private/protected成员
    phone = "13800138000";  // protected成员赋值
}
```

##### （2）对象的创建与成员访问

对象是类的**实例化产物**，创建方式分 “栈区创建” 和 “堆区创建”，成员访问通过`.`（栈区对象）或`->`（堆区指针）。

**视频核心代码（对象使用）**：

```cpp
int main() {
    // 方式1：栈区创建对象（常用，自动释放内存）
    Person p1;
    // 访问public成员变量
    p1.name = "张三";
    p1.age = 18;
    // 访问public成员函数
    p1.showPersonInfo();  // 输出：姓名：张三，年龄：18
    p1.setPersonInfo("张三", 19);  // 修改属性
    p1.showPersonInfo();  // 输出：姓名：张三，年龄：19

    // 方式2：堆区创建对象（new关键字，需手动释放）
    Person *p2 = new Person;  // 堆区分配内存，返回指针
    // 堆区对象访问成员：指针用“->”
    p2->name = "李四";
    p2->age = 20;
    p2->showPersonInfo();  // 输出：姓名：李四，年龄：20
    // 释放堆区内存（视频强调：避免内存泄漏，释放后置空）
    delete p2;
    p2 = NULL;  // 防止野指针

    // 错误示例（视频演示）：类外访问private/protected成员
    // p1.idCard = "110101200001011234";  // 报错：private不可访问
    // p1.phone = "13900139000";          // 报错：protected不可访问

    return 0;
}
```

##### （3）访问权限的核心区别（视频表格对应）

| 访问权限    | 类内访问 | 类外访问 | 子类访问（后续继承） | 视频关键说明             |
| ----------- | -------- | -------- | -------------------- | ------------------------ |
| `public`    | ✅        | ✅        | ✅                    | 对外接口，暴露可访问成员 |
| `private`   | ✅        | ❌        | ❌                    | 内部数据，仅类内操作     |
| `protected` | ✅        | ❌        | ✅                    | 父子类共享，类外隐藏     |

#### 2. 构造函数与析构函数

##### （1）构造函数：对象初始化的 “自动工具”

- **视频定义**：对象创建时**自动调用**，用于初始化成员变量（解决 “成员变量默认随机值” 问题），无返回值（连`void`都不写），函数名与类名**完全一致**，支持重载。
- **默认规则**：若未手动定义，编译器自动生成 “默认无参构造函数”（空实现）；若定义了任意构造函数，编译器不再生成默认构造。

**视频核心代码（三种构造函数）**：

```cpp
#include <iostream>
#include <string>
using namespace std;

class Person {
public:
    string name;
    int age;

    // 1. 无参构造函数（默认构造）- 视频强调“无参数，无返回值”
    Person() {
        cout << "Person无参构造函数调用" << endl;
        name = "未知姓名";
        age = 0;
    }

    // 2. 有参构造函数（重载）- 视频示例：初始化参数赋值
    Person(string n, int a) {
        cout << "Person有参构造函数调用" << endl;
        name = n;
        age = a;
    }

    // 3. 拷贝构造函数（视频重点：参数必须是“const 类名&”，避免递归调用）
    Person(const Person &p) {
        cout << "Person拷贝构造函数调用" << endl;
        // 将传入对象p的成员赋值给当前对象
        name = p.name;
        age = p.age;
    }

    void showInfo() {
        cout << "姓名：" << name << "，年龄：" << age << endl;
    }
};

// 视频演示：构造函数的调用场景
int main() {
    // 场景1：调用无参构造（注意：不能写Person p1(); 会被识别为函数声明）
    Person p1;
    p1.showInfo();  // 输出：姓名：未知姓名，年龄：0

    // 场景2：调用有参构造（三种写法，视频均演示）
    Person p2("张三", 18);          // 写法1：括号法（推荐）
    Person p3 = Person("李四", 19); // 写法2：显式构造
    Person p4 = {"王五", 20};       // 写法3：列表初始化（C++11后支持）
    p2.showInfo();  // 姓名：张三，年龄：18

    // 场景3：调用拷贝构造（三种触发场景，视频重点）
    Person p5 = p2;          // 场景1：用已有对象初始化新对象
    Person p6(p3);           // 场景2：括号法拷贝
    Person p7 = Person(p4);  // 场景3：显式拷贝
    p5.showInfo();  // 姓名：张三，年龄：18（拷贝p2的值）

    return 0;
}
```

##### （2）析构函数：对象销毁的 “资源清理工具”

- **视频定义**：对象销毁时**自动调用**，用于释放 “堆区内存、文件句柄” 等资源，无返回值、无参数（**不可重载**），函数名固定为`~类名`。
- **核心逻辑**：栈区对象 “出作用域时自动调用”，堆区对象 “`delete`时手动触发”，析构顺序与构造顺序**完全相反**。

**视频核心代码（堆区内存释放）**：

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

class Person {
public:
    // 堆区成员变量（必须用析构释放，否则内存泄漏）
    string *pName;  // 姓名指针（指向堆区字符串）
    int age;

    // 构造函数：在堆区分配内存
    Person(string n, int a) {
        cout << "有参构造函数调用" << endl;
        pName = new string(n);  // 堆区创建字符串
        age = a;
    }

    // 析构函数：释放堆区内存（视频重点：必须手动写）
    ~Person() {
        cout << "析构函数调用" << endl;
        // 防止野指针：先判断是否为空，再释放
        if (pName != NULL) {
            delete pName;  // 释放堆区内存
            pName = NULL;  // 置空，避免野指针
        }
    }

    void showInfo() {
        cout << "姓名：" << *pName << "，年龄：" << age << endl;
    }
};

int main() {
    // 1. 栈区对象：main函数结束时自动调用析构
    Person p1("张三", 18);
    p1.showInfo();  // 姓名：张三，年龄：18

    // 2. 堆区对象：必须用delete触发析构（视频强调：漏写会内存泄漏）
    Person *p2 = new Person("李四", 19);
    p2->showInfo();  // 姓名：李四，年龄：19
    delete p2;  // 手动释放堆区对象，触发析构
    p2 = NULL;  // 置空

    return 0;
}
// 视频演示输出顺序（关键）：
// 有参构造函数调用（p1）
// 姓名：张三，年龄：18
// 有参构造函数调用（p2）
// 姓名：李四，年龄：19
// 析构函数调用（p2，delete触发）
// 析构函数调用（p1，main结束栈区销毁）
```

#### 3. 静态成员

静态成员属于**类本身**，而非单个对象，所有对象共享同一静态成员，内存存储在 “全局区”，生命周期与程序一致。

##### （1）静态成员变量

- **视频核心规则**：
	1. 必须在**类外初始化**（初始化时不加`static`，需加 “类名::”）；
	2. 访问方式：`类名::静态变量名`（推荐，不依赖对象）或`对象.静态变量名`；
	3. 静态变量不占对象内存（`sizeof(对象)`不包含静态变量）。

**视频核心代码（统计对象数量）**：

cpp







```cpp
#include <iostream>
using namespace std;

class Person {
public:
    // 静态成员变量：统计Person类创建的对象总数
    static int objCount;
    // 普通成员变量
    int age;

    // 构造函数：每创建一个对象，静态变量+1
    Person(int a) {
        age = a;
        objCount++;  // 所有对象共享objCount，创建时自增
    }
};

// 静态成员变量：类外初始化（视频重点：必须写，否则链接错误）
int Person::objCount = 0;

int main() {
    // 访问静态变量：方式1：类名::变量名（不依赖对象）
    cout << "初始对象数：" << Person::objCount << endl;  // 0

    // 创建对象，触发构造函数，objCount自增
    Person p1(18);
    cout << "p1创建后对象数：" << Person::objCount << endl;  // 1
    // 访问静态变量：方式2：对象.变量名（依赖对象）
    cout << "p1的objCount：" << p1.objCount << endl;  // 1

    Person p2(19);
    cout << "p2创建后对象数：" << Person::objCount << endl;  // 2
    cout << "p2的objCount：" << p2.objCount << endl;  // 2

    // 静态变量共享：修改一个对象的静态变量，所有对象共享变化
    p1.objCount = 10;
    cout << "p2的objCount（修改后）：" << p2.objCount << endl;  // 10

    // 验证：静态变量不占对象内存
    cout << "对象p1的大小：" << sizeof(p1) << endl;  // 4（仅age的大小，int占4字节）

    return 0;
}
```

##### （2）静态成员函数

- **视频核心规则**：
	1. 无`this`指针（无法访问普通成员变量 / 函数，只能访问静态成员）；
	2. 访问方式：`类名::静态函数名()`（推荐）或`对象.静态函数名()`；
	3. 静态函数不能用`const`修饰（无`this`指针，无法保证成员不被修改）。

**视频核心代码（静态函数操作静态变量）**：

cpp







```cpp
#include <iostream>
using namespace std;

class Person {
public:
    static int objCount;  // 静态成员变量
    int age;              // 普通成员变量

    // 静态成员函数（视频强调：只能访问静态成员）
    static void showObjCount() {
        cout << "当前对象数：" << objCount << endl;
        // cout << age;  // 错误：静态函数不能访问普通成员变量
    }

    // 普通成员函数：可访问静态成员和普通成员
    void showAll() {
        cout << "年龄：" << age << "，对象数：" << objCount << endl;
    }

    // 构造函数：创建对象时objCount自增
    Person(int a) {
        age = a;
        objCount++;
    }
};

// 静态成员变量初始化
int Person::objCount = 0;

int main() {
    // 访问静态函数：方式1：类名::函数名（不依赖对象）
    Person::showObjCount();  // 0

    Person p1(18);
    // 访问静态函数：方式2：对象.函数名
    p1.showObjCount();  // 1
    // 普通成员函数访问静态成员
    p1.showAll();  // 年龄：18，对象数：1

    Person p2(19);
    Person::showObjCount();  // 2
    p2.showAll();  // 年龄：19，对象数：2

    return 0;
}
```

#### 4. 友元

友元是**特殊授权机制**，允许类外的 “函数 / 类” 访问当前类的`private`/`protected`成员，仅用于特定场景（如运算符重载、外部工具函数），不推荐滥用。

##### （1）友元函数

**视频定义**：在类内用`friend`声明的全局函数 / 其他类的成员函数，可直接访问类的私有成员。

**视频核心代码（全局友元函数访问私有成员）**：

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

class Person {
    // 关键：声明全局函数showPersonDetail为友元（无访问权限限制）
    friend void showPersonDetail(const Person &p);

private:
    // 私有成员（类外默认不可访问）
    string name = "张三";
    int age = 18;
    string phone = "13800138000";
};

// 全局函数：友元身份，可直接访问Person的private成员
void showPersonDetail(const Person &p) {
    cout << "友元函数访问私有成员：" << endl;
    cout << "姓名：" << p.name << endl;    // 私有成员
    cout << "年龄：" << p.age << endl;     // 私有成员
    cout << "电话：" << p.phone << endl;   // 私有成员
}

int main() {
    Person p;
    showPersonDetail(p);  // 成功访问私有成员，输出正确信息
    return 0;
}
// 视频输出：
// 友元函数访问私有成员：
// 姓名：张三
// 年龄：18
// 电话：13800138000
```

##### （2）友元类

**视频定义**：在类内用`friend class 类名`声明的类，该类的所有成员函数均可访问当前类的私有成员。

**视频核心代码（友元类访问私有成员）**：

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

// 前置声明：告诉编译器Teacher类存在（后续会定义）
class Teacher;

class Student {
    // 声明Teacher类为友元类：Teacher的所有成员函数可访问Student的私有成员
    friend class Teacher;

private:
    string secret = "我上课偷偷看C++教程";  // 私有成员
};

// 友元类Teacher
class Teacher {
public:
    // Teacher的成员函数：可直接访问Student的private成员
    void checkStudentSecret(Student &s) {
        cout << "老师查看学生秘密：" << s.secret << endl;
    }
};

int main() {
    Student s;
    Teacher t;
    t.checkStudentSecret(s);  // 友元类访问私有成员，输出正确信息
    return 0;
}
// 视频输出：
// 老师查看学生秘密：我上课偷偷看C++教程
```

#### 5. 继承（`inheritance`，重点：代码复用）

继承是**子类获取父类成员的机制**，实现 “代码复用”，子类可新增成员或重写父类成员，继承方式决定父类成员在子类中的权限。

##### （1）继承的基础语法

**语法**：`class 子类名 : 继承方式 父类名 { 子类新增成员 };`，继承方式分`public`（常用）、`protected`、`private`（默认）。

**视频核心代码（public 继承案例）**：

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

// 父类（基类）：Person
class Person {
public:
    // 父类成员变量
    string name;
    int age;

    // 父类成员函数
    void setBaseInfo(string n, int a) {
        name = n;
        age = a;
    }

    void eat() {
        cout << name << "在吃午饭" << endl;
    }
};

// 子类（派生类）：Student，public继承Person（视频重点：public继承权限）
class Student : public Person {
public:
    // 子类新增成员变量
    int studentID;  // 学号

    // 子类新增成员函数
    void setStudentInfo(string n, int a, int id) {
        // 子类可直接访问父类的public成员
        setBaseInfo(n, a);  // 调用父类函数
        studentID = id;      // 子类自有成员赋值
    }

    void study() {
        // 子类可直接访问父类的public成员变量
        cout << name << "（学号：" << studentID << "）在学习C++" << endl;
    }
};

int main() {
    Student s;
    // 1. 调用子类函数，间接使用父类成员
    s.setStudentInfo("张三", 18, 2024001);
    s.study();  // 输出：张三（学号：2024001）在学习C++

    // 2. 直接调用父类成员函数
    s.eat();  // 输出：张三在吃午饭

    // 3. 直接访问父类成员变量
    cout << "学生姓名：" << s.name << "，年龄：" << s.age << endl;  // 张三，18

    return 0;
}
```

##### （2）继承中的权限传递

仅`public`继承的权限传递规则：

| 父类成员权限 | 子类中成员权限  | 子类外能否访问（通过子类对象） | 视频说明                 |
| ------------ | --------------- | ------------------------------ | ------------------------ |
| `public`     | 保持`public`    | ✅                              | 对外暴露的接口           |
| `protected`  | 保持`protected` | ❌                              | 仅子类内部可访问         |
| `private`    | 不可访问        | ❌                              | 父类私有，子类也无法访问 |

**视频验证代码**：

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

class Person {
public:
    string pubName = "公有姓名";    // public成员
protected:
    string proPhone = "13800138000";// protected成员
private:
    string priAddr = "北京";        // private成员
};

// public继承
class Student : public Person {
public:
    void showInherit() {
        // 子类内可访问父类public和protected成员
        cout << "父类public：" << pubName << endl;    // 正常
        cout << "父类protected：" << proPhone << endl;// 正常
        // cout << priAddr;  // 错误：父类private成员子类不可访问
    }
};

int main() {
    Student s;
    // 子类外可访问父类public成员（通过子类对象）
    cout << "子类外访问父类public：" << s.pubName << endl;  // 正常
    // cout << s.proPhone;  // 错误：子类外不可访问父类protected成员

    s.showInherit();  // 输出父类public和protected成员
    return 0;
}
```

##### （3）子类构造与析构顺序（视频核心逻辑）

- **构造顺序**：先调用**父类构造函数**，再调用**子类构造函数**（确保父类成员先初始化）；
- **析构顺序**：先调用**子类析构函数**，再调用**父类析构函数**（确保子类资源先释放）。

**视频验证代码**：

cpp







```cpp
#include <iostream>
using namespace std;

// 父类
class Person {
public:
    Person() {
        cout << "Person父类构造函数调用" << endl;
    }

    ~Person() {
        cout << "Person父类析构函数调用" << endl;
    }
};

// 子类
class Student : public Person {
public:
    Student() {
        cout << "Student子类构造函数调用" << endl;
    }

    ~Student() {
        cout << "Student子类析构函数调用" << endl;
    }
};

int main() {
    Student s;  // 创建子类对象，触发构造/析构
    // 视频输出顺序（固定）：
    // Person父类构造函数调用
    // Student子类构造函数调用
    // Student子类析构函数调用
    // Person父类析构函数调用
    return 0;
}
```

#### 6. 多态（`polymorphism`，核心：动态绑定）

多态是**同一行为在不同对象上的不同表现**，核心依赖 “虚函数” 和 “父类指针 / 引用”，实现 “运行时动态绑定”。

##### （1）多态的两个前

1. 存在**继承关系**（子类继承父类）；
2. 子类**重写**父类的 “虚函数”（父类函数加`virtual`，子类函数与父类 “函数名、参数、返回值” 完全一致）。

##### （2）虚函数与多态实现

**视频核心代码（父类指针指向子类对象）**：

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

// 父类：Animal（含虚函数）
class Animal {
public:
    // 虚函数：父类加virtual，允许子类重写
    virtual void speak() {
        cout << "动物在叫" << endl;
    }
};

// 子类1：Dog（重写父类虚函数）
class Dog : public Animal {
public:
    // 重写虚函数：函数名、参数、返回值与父类完全一致（可加override显式声明）
    void speak() override {
        cout << "小狗汪汪叫" << endl;
    }
};

// 子类2：Cat（重写父类虚函数）
class Cat : public Animal {
public:
    void speak() override {
        cout << "小猫喵喵叫" << endl;
    }
};

// 视频关键函数：接收父类引用，实现多态调用
void makeAnimalSpeak(Animal &animal) {
    animal.speak();  // 运行时动态绑定：调用子类重写的函数
}

int main() {
    Dog d;
    Cat c;

    // 1. 父类引用指向子类对象，多态调用
    makeAnimalSpeak(d);  // 输出：小狗汪汪叫
    makeAnimalSpeak(c);  // 输出：小猫喵喵叫

    // 2. 父类指针指向子类对象，多态调用
    Animal *p1 = new Dog;
    p1->speak();  // 输出：小狗汪汪叫
    delete p1;
    p1 = NULL;

    Animal *p2 = new Cat;
    p2->speak();  // 输出：小猫喵喵叫
    delete p2;
    p2 = NULL;

    return 0;
}
```

##### （3）纯虚函数与抽象类（重点：强制重写）

- **纯虚函数**：父类只声明不实现的虚函数，语法：`virtual 返回值类型 函数名(参数) = 0;`；
- **抽象类**：含纯虚函数的类，**不能实例化对象**，只能作为父类被继承；
- **强制规则**：子类必须重写父类所有纯虚函数，否则子类也是抽象类（无法实例化）。

**视频核心代码（抽象类与子类实现）**：

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

// 抽象类：Shape（含纯虚函数）
class Shape {
public:
    // 纯虚函数1：计算面积（无实现）
    virtual double calculateArea() = 0;
    // 纯虚函数2：显示形状信息（无实现）
    virtual void showShape() = 0;
};

// 子类1：Circle（重写所有纯虚函数，可实例化）
class Circle : public Shape {
private:
    double radius;  // 半径
public:
    // 构造函数
    Circle(double r) {
        radius = r;
    }

    // 重写纯虚函数1：计算圆面积（πr²）
    double calculateArea() override {
        return 3.14 * radius * radius;
    }

    // 重写纯虚函数2：显示圆信息
    void showShape() override {
        cout << "形状：圆形，半径：" << radius << endl;
    }
};

// 子类2：Rectangle（重写所有纯虚函数，可实例化）
class Rectangle : public Shape {
private:
    double width;   // 宽
    double height;  // 高
public:
    Rectangle(double w, double h) {
        width = w;
        height = h;
    }

    double calculateArea() override {
        return width * height;
    }

    void showShape() override {
        cout << "形状：矩形，宽：" << width << "，高：" << height << endl;
    }
};

// 多态函数：接收抽象类引用（视频重点：抽象类可作为函数参数类型）
void showShapeArea(Shape &shape) {
    shape.showShape();
    cout << "面积：" << shape.calculateArea() << endl;
}

int main() {
    // Shape s;  // 错误：抽象类不能实例化对象

    Circle c(5);    // 子类实例化（已重写纯虚函数）
    Rectangle r(4, 6);  // 子类实例化

    // 多态调用
    showShapeArea(c);
    // 输出：
    // 形状：圆形，半径：5
    // 面积：78.5

    showShapeArea(r);
    // 输出：
    // 形状：矩形，宽：4，高：6
    // 面积：24

    return 0;
}
```

#### 7、类的特殊成员函数（补充：深拷贝与浅拷贝）

之前讲解拷贝构造函数时，未深入区分 “浅拷贝” 与 “深拷贝”—— 这是 C++ 中避免内存泄漏和重复释放的核心考点，尤其当类中包含**堆区成员变量**时，浅拷贝会导致严重问题。

##### 1. 浅拷贝（默认拷贝构造的问题）

###### （1）定义

浅拷贝是指**仅复制成员变量的 “值”**，若成员变量是指针（指向堆区内存），则仅复制指针的地址（而非堆区内容），导致两个对象的指针指向**同一块堆区内存**。

###### （2）浅拷贝的风险（视频核心案例：重复释放内存）

默认拷贝构造函数（编译器自动生成）采用浅拷贝，当类含堆区指针时，会出现 “对象销毁时重复释放同一块内存” 的错误。

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

class Person {
public:
    string* name;  // 堆区指针成员（存储姓名）
    int age;

    // 有参构造：在堆区分配内存
    Person(string n, int a) {
        cout << "有参构造调用" << endl;
        name = new string(n);  // 堆区创建字符串
        age = a;
    }

    // 编译器自动生成的“浅拷贝构造函数”（默认）
    // Person(const Person& p) {
    //     name = p.name;  // 仅复制指针地址，未复制堆区内容
    //     age = p.age;
    // }

    // 析构函数：释放堆区内存
    ~Person() {
        cout << "析构函数调用：释放name内存" << endl;
        if (name != NULL) {
            delete name;  // 释放堆区内存
            name = NULL;
        }
    }
};

int main() {
    Person p1("张三", 18);
    Person p2 = p1;  // 调用默认浅拷贝构造函数

    // 问题：p1和p2的name指针指向同一块堆区内存
    cout << "p1.name地址：" << (long long)p1.name << endl;
    cout << "p2.name地址：" << (long long)p2.name << endl;  // 与p1相同

    // 程序结束时，p2先析构：释放name指向的堆区内存
    // 随后p1析构：再次释放已被释放的内存 → 程序崩溃（重复释放）
    return 0;
}
```

**视频关键错误原因**：

两个对象的`name`指针指向同一块堆区内存，第一个对象析构时已释放该内存，第二个对象析构时再次释放 “已无效的内存”，触发操作系统内存保护机制，导致程序崩溃。

##### 2. 深拷贝（解决浅拷贝问题）

###### （1）定义

深拷贝是指**不仅复制成员变量的值，还为堆区成员变量重新分配内存，并复制堆区内容**，确保两个对象的堆区成员指向**不同的内存空间**，彻底独立。

##### （2）深拷贝的实现（手动重写拷贝构造函数）

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

class Person {
public:
    string* name;
    int age;

    // 有参构造
    Person(string n, int a) {
        cout << "有参构造调用" << endl;
        name = new string(n);
        age = a;
    }

    // 手动实现“深拷贝构造函数”（核心：为堆区成员重新分配内存）
    Person(const Person& p) {
        cout << "深拷贝构造函数调用" << endl;
        age = p.age;  // 复制普通成员
        // 关键：为当前对象的name重新分配堆区内存，并复制p.name的内容
        name = new string(*(p.name));  // 复制堆区字符串内容，而非指针地址
    }

    // 析构函数
    ~Person() {
        cout << "析构函数调用：释放name内存" << endl;
        if (name != NULL) {
            delete name;
            name = NULL;
        }
    }
};

int main() {
    Person p1("张三", 18);
    Person p2 = p1;  // 调用手动实现的深拷贝构造函数

    // 验证：p1和p2的name指针指向不同堆区内存
    cout << "p1.name地址：" << (long long)p1.name << endl;
    cout << "p2.name地址：" << (long long)p2.name << endl;  // 地址不同

    // 程序结束时，p2和p1分别释放各自的堆区内存 → 无重复释放问题
    return 0;
}
```

**视频核心结论**：

当类中包含堆区成员变量（如指针、动态数组）时，**必须手动实现深拷贝构造函数**，否则默认浅拷贝会导致内存错误。

##### 3. 赋值运算符重载的深拷贝（补充）

除了拷贝构造，赋值运算符（`=`）默认也采用浅拷贝，需手动重载实现深拷贝：

cpp







```cpp
// 赋值运算符重载（深拷贝）
Person& operator=(const Person& p) {
    cout << "赋值运算符重载（深拷贝）" << endl;
    // 1. 先释放当前对象的堆区内存（避免内存泄漏）
    if (name != NULL) {
        delete name;
        name = NULL;
    }
    // 2. 深拷贝：重新分配内存并复制内容
    age = p.age;
    name = new string(*(p.name));
    // 3. 返回自身引用（支持链式赋值，如p3 = p2 = p1）
    return *this;
}

```

#### 8. 类和对象特性

##### （1）初始化列表

- **作用**：在构造函数中直接初始化成员变量，语法更简洁，尤其适合常量成员（`const`修饰）和引用成员（必须初始化）。
- **语法**：`构造函数(参数) : 成员变量1(值1), 成员变量2(值2), ... {}`
- **视频核心代码**：

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

class Person {
public:
    // 成员变量（包含const和引用，必须初始化）
    const int id;  // 常量成员，必须初始化且不能修改
    string& name;  // 引用成员，必须初始化且绑定后不能改
    int age;

    // 初始化列表初始化成员（视频重点：const和引用必须用此方式）
    Person(int i, string& n, int a) : id(i), name(n), age(a) {
        // 此处无需再赋值，初始化已完成
        cout << "构造函数调用，id=" << id << endl;
    }
};

int main() {
    string name = "张三";
    // 初始化列表传递参数
    Person p(1001, name, 18);
    cout << "id=" << p.id << ", name=" << p.name << ", age=" << p.age << endl;
    // p.id = 1002;  // 错误：const成员不能修改
    return 0;
}
// 视频输出：
// 构造函数调用，id=1001
// id=1001, name=张三, age=18
```

##### （2）类对象作为类成员

- **场景**：一个类的成员是另一个类的对象（如`Phone`类对象作为`Person`类成员）。
- **核心规则**：构造顺序：先构造**成员对象**，再构造**自身**；析构顺序：先析构**自身**，再析构**成员对象**（与构造相反）。
- **视频核心代码**：

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

// 成员类：Phone
class Phone {
public:
    string brand;

    // Phone构造函数
    Phone(string b) : brand(b) {
        cout << "Phone构造函数调用" << endl;
    }

    // Phone析构函数
    ~Phone() {
        cout << "Phone析构函数调用" << endl;
    }
};

// 主类：Person，包含Phone对象作为成员
class Person {
public:
    string name;
    Phone phone;  // 类对象作为成员

    // 构造Person：先初始化成员对象phone（必须在初始化列表）
    Person(string n, string b) : name(n), phone(b) {
        cout << "Person构造函数调用" << endl;
    }

    // 析构Person
    ~Person() {
        cout << "Person析构函数调用" << endl;
    }
};

int main() {
    Person p("张三", "苹果");
    cout << p.name << "的手机品牌：" << p.phone.brand << endl;
    return 0;
}
// 视频输出顺序（关键）：
// Phone构造函数调用（先构造成员对象）
// Person构造函数调用（再构造自身）
// 张三的手机品牌：苹果
// Person析构函数调用（先析构自身）
// Phone析构函数调用（再析构成员对象）
```

##### （3）this 指针的用途

- **本质**：隐含的指针参数，指向当前对象（每个非静态成员函数都有`this`指针，无需显式声明）。
- **核心用途**：
	1. 解决成员变量与函数参数同名问题；
	2. 从成员函数中返回当前对象（实现链式调用）。
- **视频核心代码**：

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

class Person {
public:
    int age;

    // 用途1：解决参数与成员变量同名
    Person(int age) {
        this->age = age;  // this->age指向成员变量，age指向参数
    }

    // 用途2：返回当前对象（实现链式调用）
    Person& PersonAddAge(Person& p) {
        this->age += p.age;
        return *this;  // 返回当前对象的引用
    }
};

int main() {
    // 用途1测试
    Person p1(10);
    cout << "p1.age = " << p1.age << endl;  // 10

    // 用途2测试：链式调用
    Person p2(5);
    p1.PersonAddAge(p2).PersonAddAge(p2).PersonAddAge(p2);  // 多次调用
    cout << "p1.age = " << p1.age << endl;  // 10+5*3=25

    return 0;
}
```

##### （4）const 修饰成员函数

- **语法**：`返回值类型 函数名(参数) const;`
- **核心规则**：
	1. const 成员函数**不能修改成员变量**（只读权限）；
	2. const 成员函数**不能调用非 const 成员函数**（非 const 函数可能修改成员变量）；
	3. const 对象**只能调用 const 成员函数**（保证对象不被修改）。
- **视频核心代码**：

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

class Person {
public:
    string name;
    int age;

    Person(string n, int a) : name(n), age(a) {}

    // const成员函数：不能修改成员变量
    void showInfo() const {
        // name = "李四";  // 错误：const函数不能修改成员变量
        cout << "姓名：" << name << "，年龄：" << age << endl;
    }

    // 非const成员函数：可修改成员变量
    void setAge(int a) {
        age = a;
    }
};

int main() {
    // const对象：只能调用const成员函数
    const Person p1("张三", 18);
    p1.showInfo();  // 正确：const对象调用const函数
    // p1.setAge(20);  // 错误：const对象不能调用非const函数

    // 非const对象：可调用任意成员函数
    Person p2("李四", 19);
    p2.showInfo();  // 正确
    p2.setAge(20);  // 正确
    p2.showInfo();  // 姓名：李四，年龄：20

    return 0;
}
```

#### 9. 继承进阶

##### （1）同名成员处理

- **规则**：子类与父类成员同名时，子类成员会**隐藏**父类成员（需用`父类名::`作用域分辨符访问父类成员）。
- **视频核心代码**：

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

// 父类
class Person {
public:
    string name = "父类姓名";

    void showName() {
        cout << "父类showName：" << name << endl;
    }
};

// 子类：同名成员隐藏父类成员
class Student : public Person {
public:
    string name = "子类姓名";  // 隐藏父类name

    void showName() {  // 隐藏父类showName
        cout << "子类showName：" << name << endl;
    }
};

int main() {
    Student s;
    // 访问子类成员（默认）
    cout << "s.name = " << s.name << endl;  // 子类姓名
    s.showName();  // 子类showName：子类姓名

    // 访问父类成员（需加作用域）
    cout << "s.Person::name = " << s.Person::name << endl;  // 父类姓名
    s.Person::showName();  // 父类showName：父类姓名

    return 0;
}
```

##### （2）同名静态成员处理

- **规则**：与普通同名成员一致，子类静态成员隐藏父类静态成员，访问父类静态成员需加`父类名::`。
- **视频核心代码**：

cpp







```cpp
#include <iostream>
using namespace std;

// 父类
class Person {
public:
    static int count;  // 父类静态成员

    static void showCount() {
        cout << "父类count：" << count << endl;
    }
};
int Person::count = 10;  // 父类静态成员初始化

// 子类
class Student : public Person {
public:
    static int count;  // 子类静态成员（隐藏父类count）

    static void showCount() {
        cout << "子类count：" << count << endl;
    }
};
int Student::count = 20;  // 子类静态成员初始化

int main() {
    // 访问子类静态成员（两种方式）
    cout << "Student::count = " << Student::count << endl;  // 20
    Student s;
    cout << "s.count = " << s.count << endl;  // 20
    Student::showCount();  // 子类count：20

    // 访问父类静态成员（需加父类作用域）
    cout << "Person::count = " << Person::count << endl;  // 10
    cout << "s.Person::count = " << s.Person::count << endl;  // 10
    s.Person::showCount();  // 父类count：10

    return 0;
}
```

##### （3）菱形继承问题与解决

- **菱形继承结构**：子类 D 继承 B 和 C，B 和 C 均继承 A（导致 D 包含两份 A 的成员，数据冗余 + 二义性）。
- **解决方案**：虚继承（`virtual`关键字），使 B 和 C 共享 A 的成员（A 成为 “虚基类”）。
- **视频核心代码**：

cpp







```cpp
#include <iostream>
using namespace std;

// 虚基类：A
class A {
public:
    int a;
    A(int num) : a(num) {
        cout << "A构造函数" << endl;
    }
};

// 中间类B：虚继承A（virtual加在继承列表）
class B : virtual public A {
public:
    B(int num) : A(num) {
        cout << "B构造函数" << endl;
    }
};

// 中间类C：虚继承A
class C : virtual public A {
public:
    C(int num) : A(num) {
        cout << "C构造函数" << endl;
    }
};

// 最终子类D：继承B和C，需直接初始化虚基类A
class D : public B, public C {
public:
    // 虚继承时，D必须直接初始化A（B和C的A初始化被忽略）
    D(int numA, int numB, int numC) : A(numA), B(numB), C(numC) {
        cout << "D构造函数" << endl;
    }
};

int main() {
    D d(100, 200, 300);
    // 二义性解决：D仅含一份A的成员
    cout << "d.a = " << d.a << endl;  // 100（由D直接初始化）
    // 也可通过中间类访问（均指向同一份A成员）
    cout << "d.B::a = " << d.B::a << endl;  // 100
    cout << "d.C::a = " << d.C::a << endl;  // 100

    return 0;
}
// 视频输出顺序：
// A构造函数（D直接初始化）
// B构造函数
// C构造函数
// D构造函数
```



#### 10、运算符重载（C++ 核心特性：自定义运算符行为）

运算符重载允许程序员为**自定义类型（如类、结构体）** 重新定义运算符的行为（如`+`、`-`、`<<`、`=`），使自定义类型能像内置类型（int、double）一样使用运算符，提升代码可读性。

##### 1. 运算符重载的语法规则

###### （1）基础语法

cpp







```cpp
// 方式1：成员函数重载（this指针指向左操作数）
返回值类型 operator运算符(参数列表) {
    // 运算符逻辑
}

// 方式2：友元函数重载（无this指针，需显式传递两个操作数）
friend 返回值类型 operator运算符(参数列表) {
    // 运算符逻辑
}
```

###### （2）核心限制（重点强调）

- 不能重载的运算符：`.`（成员访问）、`.*`（成员指针访问）、`::`（作用域）、`sizeof`（大小计算）、`?:`（三目运算符）；
- 运算符的优先级和结合性不能改变；
- 操作数个数不能改变（如`+`必须有两个操作数）。

##### 2. 常见运算符重载案例

###### （1）重载加法运算符（`+`）：实现两个对象的 “相加”

需求：定义`Person`类，重载`+`，实现 “两个 Person 对象的年龄相加，姓名拼接”。

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

class Person {
public:
    string name;
    int age;

    Person(string n, int a) : name(n), age(a) {}  // 初始化列表构造

    // 方式1：成员函数重载+（左操作数是this，右操作数是p）
    Person operator+(const Person& p) const {
        // 返回新对象：姓名拼接，年龄相加
        return Person(name + p.name, age + p.age);
    }

    // 重载+：支持对象与int类型相加（如p1 + 5）
    Person operator+(int num) const {
        return Person(name, age + num);
    }
};

// 方式2：友元函数重载+：支持int类型与对象相加（如5 + p1，成员函数无法实现）
friend Person operator+(int num, const Person& p) {
    return Person(p.name, p.age + num);
}

// 重载输出运算符<<（必须用友元，因为左操作数是cout（ostream类型），非Person对象）
friend ostream& operator<<(ostream& cout, const Person& p) {
    cout << "姓名：" << p.name << "，年龄：" << p.age;
    return cout;  // 返回cout，支持链式输出（如cout << p1 << p2）
}

int main() {
    Person p1("张三", 18);
    Person p2("李四", 20);

    // 调用成员函数重载的+：p1.operator+(p2)
    Person p3 = p1 + p2;
    cout << "p1 + p2 = " << p3 << endl;  // 姓名：张三李四，年龄：38

    // 调用对象与int相加的+：p1.operator+(5)
    Person p4 = p1 + 5;
    cout << "p1 + 5 = " << p4 << endl;  // 姓名：张三，年龄：23

    // 调用友元函数重载的+：operator+(5, p1)
    Person p5 = 5 + p1;
    cout << "5 + p1 = " << p5 << endl;  // 姓名：张三，年龄：23

    return 0;
}
```

**视频关键说明**：

- 成员函数重载`+`时，左操作数是`this`（调用对象），右操作数是参数；
- 若需支持 “内置类型 + 自定义类型”（如`5 + p1`），必须用**友元函数重载**（因为左操作数不是自定义类型，无法调用成员函数）；
- 重载`<<`时，左操作数是`cout`（`ostream`类型），必须用友元函数，且返回`ostream&`以支持链式输出（如`cout << p1 << " " << p2`）。

###### （2）重载自增运算符（`++`）：区分前置 ++ 与后置 ++

自增运算符分 “前置”（`++p`）和 “后置”（`p++`），需通过**参数占位符**区分（后置 ++ 加一个`int`类型的占位参数，无实际意义）。

cpp







```cpp
#include <iostream>
using namespace std;

class MyInt {
public:
    int num;

    MyInt(int n) : num(n) {}

    // 1. 重载前置++（++p）：先自增，再返回自身（返回引用）
    MyInt& operator++() {
        num++;          // 先自增
        return *this;   // 返回自身引用（支持连续前置++，如++(++p)）
    }

    // 2. 重载后置++（p++）：先返回原对象，再自增（返回值，加int占位符）
    MyInt operator++(int) {  // int是占位参数，用于区分前置/后置
        MyInt temp = *this;  // 先保存原对象
        num++;               // 再自增
        return temp;         // 返回原对象（不支持连续后置++，如(p++)++）
    }

    // 重载<<
    friend ostream& operator<<(ostream& cout, const MyInt& mi) {
        cout << mi.num;
        return cout;
    }
};

int main() {
    MyInt p(10);

    // 前置++：调用operator++()
    cout << "前置++：" << ++p << endl;  // 11（先自增，再输出）

    // 后置++：调用operator++(int)
    cout << "后置++：" << p++ << endl;  // 11（先输出原对象，再自增）
    cout << "后置++后：" << p << endl;  // 12（自增后的结果）

    return 0;
}
```

#### 11、继承的进阶问题（菱形继承与虚继承）

之前讲解的是 “单继承”（一个子类继承一个父类），但实际开发中可能遇到 “多继承”（一个子类继承多个父类），其中 “菱形继承”（钻石继承）是多继承的典型问题，需用**虚继承**解决。

##### 1. 菱形继承的问题（数据冗余与二义性）

###### （1）菱形继承结构

plaintext

```plaintext
      基类A
     /     \
    B       C  （B和C都继承A）
     \     /
      子类D  （D继承B和C）
```

###### （2）核心问题

- **数据冗余**：D 的对象中会包含两份 A 的成员（一份来自 B，一份来自 C）；
- **二义性**：访问 A 的成员时，编译器无法确定访问哪一份（来自 B 还是 C）。

cpp



```cpp
#include <iostream>
using namespace std;

// 顶层基类A
class A {
public:
    int a;
    A(int num) : a(num) {}
};

// 子类B继承A
class B : public A {
public:
    B(int num) : A(num) {}
};

// 子类C继承A
class C : public A {
public:
    C(int num) : A(num) {}
};

// 子类D继承B和C（菱形继承）
class D : public B, public C {
public:
    // 构造D时，需初始化B和C，间接初始化A两次（导致两份A的成员）
    D(int num1, int num2) : B(num1), C(num2) {}
};

int main() {
    D d(10, 20);

    // 问题1：数据冗余：d包含两份A的a（B::a=10，C::a=20）
    cout << "B::a = " << d.B::a << endl;  // 10
    cout << "C::a = " << d.C::a << endl;  // 20

    // 问题2：二义性：直接访问d.a会报错（编译器无法确定访问B::a还是C::a）
    // cout << d.a << endl;  // 错误：'a' is ambiguous

    return 0;
}
```

##### 2. 虚继承（解决菱形继承问题）

###### （1）核心原理

通过`virtual`关键字将 “中间父类（B、C）对顶层基类（A）的继承” 声明为**虚继承**，使顶层基类 A 成为 “虚基类”。此时，子类 D 的对象中**仅包含一份 A 的成员**，所有中间父类共享这份成员，从而解决数据冗余和二义性。

###### （2）虚继承的语法

cpp







```cpp
#include <iostream>
using namespace std;

// 顶层基类A
class A {
public:
    int a;
    A(int num) : a(num) {
        cout << "A的构造函数：a = " << a << endl;
    }
};

// 中间父类B：虚继承A（virtual关键字加在继承列表中）
class B : virtual public A {  // 虚继承
public:
    B(int num) : A(num) {}  // 虚继承时，中间父类的构造不再初始化A（由最终子类初始化）
};

// 中间父类C：虚继承A
class C : virtual public A {  // 虚继承
public:
    C(int num) : A(num) {}
};

// 最终子类D：继承B和C，需直接初始化虚基类A（关键）
class D : public B, public C {
public:
    // 虚继承时，最终子类必须直接初始化虚基类A（中间父类的A初始化被忽略）
    D(int numA, int numB, int numC) : A(numA), B(numB), C(numC) {}
};

int main() {
    D d(100, 20, 30);  // A的构造由D直接初始化，B和C的A初始化被忽略

    // 解决二义性：直接访问d.a（仅一份A的成员）
    cout << "d.a = " << d.a << endl;  // 100（不再冗余）

    // 也可通过中间父类访问（但指向同一份成员）
    cout << "d.B::a = " << d.B::a << endl;  // 100
    cout << "d.C::a = " << d.C::a << endl;  // 100

    return 0;
}
```

**视频关键结论**：

- 虚继承的`virtual`关键字需加在 “中间父类（B、C）” 的继承列表中，而非顶层基类（A）；
- 最终子类（D）必须**直接初始化虚基类（A）**，中间父类对虚基类的初始化会被编译器忽略；
- 虚继承通过 “虚基类表”（存储虚基类成员的偏移量）实现共享，避免数据冗余。





## 第三阶段：C++ 高级特性与实战

### 1. 模板（`template`，视频核心：通用代码）

模板是**泛型编程的基础**，实现 “一份代码适配多种数据类型”，避免为`int`、`double`、`string`等写重复代码，分 “函数模板” 和 “类模板”。

#### （1）函数模板（视频重点：通用函数）

**语法**：`template <typename T>`（`T`为通用类型占位符，可替换为任意数据类型），函数参数 / 返回值用`T`表示 “通用类型”。

**视频核心代码（通用交换函数）**：

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

// 函数模板：通用交换函数（适配int、double、string等）
template <typename T>  // 声明模板参数T（T是通用类型）
void mySwap(T &a, T &b) {
    T temp = a;  // 通用类型变量
    a = b;
    b = temp;
}

int main() {
    // 场景1：交换int类型
    int a = 10, b = 20;
    cout << "交换前：a=" << a << ", b=" << b << endl;  // 10,20
    mySwap(a, b);  // 自动推导T为int
    cout << "交换后：a=" << a << ", b=" << b << endl;  // 20,10

    // 场景2：交换double类型
    double c = 3.14, d = 6.28;
    cout << "交换前：c=" << c << ", d=" << d << endl;  // 3.14,6.28
    mySwap(c, d);  // 自动推导T为double
    cout << "交换后：c=" << c << ", d=" << d << endl;  // 6.28,3.14

    // 场景3：交换string类型
    string e = "hello", f = "world";
    cout << "交换前：e=" << e << ", f=" << f << endl;  // hello,world
    mySwap(e, f);  // 自动推导T为string
    cout << "交换后：e=" << e << ", f=" << f << endl;  // world,hello

    return 0;
}
```

#### （2）类模板（视频重点：通用类）

**语法**：`template <typename T>`，类的成员变量 / 成员函数可用`T`表示 “通用类型”，类模板的成员函数需在类内声明、类外实现（需加模板声明）。

**视频核心代码（通用数组类）**：

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

// 类模板：通用数组类（适配int、string等类型）
template <typename T>
class MyArray {
private:
    T *arr;     // 通用类型指针（指向堆区数组）
    int size;   // 数组大小
public:
    // 构造函数：初始化堆区数组
    MyArray(int s) {
        size = s;
        arr = new T[size];  // 动态创建通用类型数组
    }

    // 析构函数：释放堆区内存（视频重点：防止内存泄漏）
    ~MyArray() {
        if (arr != NULL) {
            delete[] arr;  // 释放数组内存
            arr = NULL;    // 置空，避免野指针
        }
    }

    // 成员函数1：给指定索引赋值
    void setValue(int index, T value) {
        if (index >= 0 && index < size) {
            arr[index] = value;
        } else {
            cout << "索引越界！" << endl;
        }
    }

    // 成员函数2：获取指定索引的值
    T getValue(int index) {
        if (index >= 0 && index < size) {
            return arr[index];
        } else {
            cout << "索引越界！" << endl;
            // 返回默认值（不同类型默认值不同，此处简化）
            return T();
        }
    }

    // 成员函数3：获取数组大小
    int getSize() {
        return size;
    }
};

int main() {
    // 场景1：创建int类型数组
    MyArray<int> intArr(3);  // 显式指定T为int
    intArr.setValue(0, 10);
    intArr.setValue(1, 20);
    intArr.setValue(2, 30);
    cout << "int数组元素：";
    for (int i = 0; i < intArr.getSize(); i++) {
        cout << intArr.getValue(i) << " ";  // 10 20 30
    }
    cout << endl;

    // 场景2：创建string类型数组
    MyArray<string> strArr(2);  // 显式指定T为string
    strArr.setValue(0, "张三");
    strArr.setValue(1, "李四");
    cout << "string数组元素：";
    for (int i = 0; i < strArr.getSize(); i++) {
        cout << strArr.getValue(i) << " ";  // 张三 李四
    }
    cout << endl;

    return 0;
}
```

### 2. 异常处理

异常处理用于**捕获运行时错误**（如除零、索引越界），避免程序直接崩溃，核心是 “抛出异常（`throw`）- 捕获异常（`try-catch`）- 处理异常” 流程。

#### （1）异常处理基础语法

cpp







```cpp
try {
    // 可能抛出异常的代码块
    if (错误条件) {
        throw 异常值;  // 抛出异常（异常值可是int、string、自定义类型）
    }
} catch (异常类型1 变量名) {
    // 处理类型1的异常
} catch (异常类型2 变量名) {
    // 处理类型2的异常
} catch (...) {
    // 处理所有未匹配的异常（默认捕获，避免遗漏）
}
```

#### （2）核心代码

cpp







```cpp
#include <iostream>
#include <string>
using namespace std;

// 场景1：除法运算，抛出“除零异常”（string类型）
double divide(double a, double b) {
    if (b == 0) {
        // 抛出string类型异常（描述错误信息）
        throw string("错误：除数不能为0！");
    }
    return a / b;
}

// 场景2：数组访问，抛出“索引越界异常”（int类型）
int getArrayElem(int arr[], int size, int index) {
    if (index < 0 || index >= size) {
        // 抛出int类型异常（异常值为越界的索引）
        throw index;
    }
    return arr[index];
}

int main() {
    // 处理“除零异常”
    try {
        double result = divide(10, 0);  // 触发异常
        cout << "除法结果：" << result << endl;  // 不会执行
    } catch (string errMsg) {  // 捕获string类型异常
        cout << "捕获到异常：" << errMsg << endl;  // 输出：错误：除数不能为0！
    }

    // 处理“索引越界异常”
    int arr[] = {10, 20, 30};
    int size = 3;
    try {
        int value = getArrayElem(arr, size, 5);  // 索引5越界，触发异常
        cout << "数组元素：" << value << endl;  // 不会执行
    } catch (int errIndex) {  // 捕获int类型异常
        cout << "捕获到异常：索引" << errIndex << "越界，数组大小为" << size << endl;
    } catch (...) {  // 默认捕获：处理所有未匹配的异常
        cout << "捕获到未知类型异常！" << endl;
    }

    return 0;
}
```

###  3.模板进阶

#### （1）模板的局限性

- **问题**：模板无法处理所有数据类型（如自定义类型无法直接用`==`比较）。
- **解决方案**：模板具体化（为特定类型提供专属实现）。
- **视频核心代码**：

cpp



```cpp
#include <iostream>
#include <string>
using namespace std;

// 通用模板：比较两个值是否相等
template <typename T>
bool myCompare(T a, T b) {
    return a == b;
}

// 模板具体化：为string类型提供专属实现（视频重点）
template <>
bool myCompare<string>(string a, string b) {
    return a.compare(b) == 0;  // string用compare()比较
}

int main() {
    // 测试int类型（通用模板）
    int a = 10, b = 20;
    cout << "int比较：" << myCompare(a, b) << endl;  // 0（false）

    // 测试string类型（具体化模板）
    string s1 = "hello", s2 = "hello";
    cout << "string比较：" << myCompare(s1, s2) << endl;  // 1（true）

    return 0;
}
```

#### （2）类模板分文件编写

- **问题**：类模板的声明（.h）与实现（.cpp）分离时，链接器无法找到实现（模板编译特性导致）。
- **解决方案**：将声明和实现写在同一文件，后缀改为`.hpp`（约定俗成）。
- **视频核心代码（Person.hpp）**：

cpp



```cpp
// Person.hpp（声明+实现）
#include <iostream>
#include <string>
using namespace std;

// 类模板声明
template <typename T>
class Person {
public:
    T id;
    string name;

    Person(T i, string n) : id(i), name(n) {}
    void showInfo();  // 成员函数声明
};

// 类模板成员函数实现（需加模板声明）
template <typename T>
void Person<T>::showInfo() {
    cout << "ID：" << id << "，姓名：" << name << endl;
}
```

- **调用代码（main.cpp）**：

cpp





```cpp
#include "Person.hpp"  // 包含.hpp文件
using namespace std;

int main() {
    // 测试int类型ID
    Person<int> p1(1001, "张三");
    p1.showInfo();  // ID：1001，姓名：张三

    // 测试string类型ID
    Person<string> p2("2024001", "李四");
    p2.showInfo();  // ID：2024001，姓名：李四

    return 0;
}
```

#### （3）类模板友元

- **分类**：
	1. 友元函数在类内声明并实现（直接成为模板友元）；
	2. 友元函数在类外实现（需提前声明模板）。
- **视频核心代码**：

cpp



```cpp
#include <iostream>
#include <string>
using namespace std;

// 提前声明类模板（为类外友元函数做准备）
template <typename T>
class Person;

// 提前声明友元函数模板（类外实现用）
template <typename T>
void showPerson(Person<T>& p);

// 类模板
template <typename T>
class Person {
    // 1. 类内实现的友元函数（直接友元）
    friend void showPersonInner(Person<T>& p) {
        cout << "类内友元：ID=" << p.id << "，姓名=" << p.name << endl;
    }

    // 2. 类外实现的友元函数（需指定模板参数）
    friend void showPerson<>(Person<T>& p);  // <>表示模板函数

private:
    T id;
    string name;

public:
    Person(T i, string n) : id(i), name(n) {}
};

// 类外实现友元函数
template <typename T>
void showPerson(Person<T>& p) {
    cout << "类外友元：ID=" << p.id << "，姓名=" << p.name << endl;
}

int main() {
    Person<int> p(1001, "张三");
    showPersonInner(p);  // 类内友元：ID=1001，姓名=张三
    showPerson(p);       // 类外友元：ID=1001，姓名=张三

    return 0;
}
```



#### （4）vector 容器核心特点（视频总结）

- 动态扩容：大小可自动增长，无需手动管理内存；
- 随机访问：支持`[]`直接访问指定索引元素（效率高）；
- 尾部操作高效：`push_back()`（尾部插入）、`pop_back()`（尾部删除）效率 O (1)；
- 需包含头文件：`#include <vector>`。

#### （5）vector 常用接口与迭代器



```cpp
#include <iostream>
#include <vector>   // 包含vector头文件
#include <algorithm>  // 包含sort算法头文件（视频强调：sort需此头文件）
using namespace std;

int main() {
    // 1. 创建vector容器（存储int类型）
    vector<int> v;

    // 2. 尾部插入元素（push_back，视频重点）
    v.push_back(30);
    v.push_back(10);
    v.push_back(20);
    cout << "插入后元素个数：" << v.size() << endl;  // 3（size()获取元素个数）

    // 3. 遍历方式1：迭代器遍历（视频重点：迭代器=容器的“指针”）
    cout << "迭代器遍历（未排序）：";
    // 定义迭代器：vector<int>::iterator
    vector<int>::iterator it;
    // begin()：指向首元素；end()：指向尾元素的下一个位置（不指向有效元素）
    for (it = v.begin(); it != v.end(); it++) {
        cout << *it << " ";  // *it访问迭代器指向的元素（输出：30 10 20）
    }
    cout << endl;

    // 4. 排序：使用STL的sort算法（视频重点：sort(首迭代器, 尾迭代器)）
    sort(v.begin(), v.end());  // 默认升序排序

    // 5. 遍历方式2：[]访问（随机访问，仅vector支持）
    cout << "[]遍历（排序后）：";
    for (int i = 0; i < v.size(); i++) {
        cout << v[i] << " ";  // 输出：10 20 30
    }
    cout << endl;

    // 6. 尾部删除元素（pop_back，视频重点）
    v.pop_back();  // 删除最后一个元素（30）
    cout << "删除后元素个数：" << v.size() << endl;  // 2
    cout << "删除后最后一个元素：" << v.back() << endl;  // 20（back()获取尾元素）

    // 7. 清空容器（clear）
    v.clear();
    cout << "清空后元素个数：" << v.size() << endl;  // 0

    return 0;
}
```

####  （6）vector 存储自定义类型（结构体，视频案例）

cpp







```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

// 自定义结构体：Person
struct Person {
    string name;
    int age;

    // 构造函数：初始化结构体（视频重点：方便插入vector）
    Person(string n, int a) {
        name = n;
        age = a;
    }
};

int main() {
    // 创建vector，存储Person类型
    vector<Person> v;

    // 插入自定义类型元素（直接用构造函数创建临时对象）
    v.push_back(Person("张三", 18));
    v.push_back(Person("李四", 19));
    v.push_back(Person("王五", 20));

    // 迭代器遍历：访问自定义类型成员（用->）
    cout << "vector中的Person：" << endl;
    vector<Person>::iterator it;
    for (it = v.begin(); it != v.end(); it++) {
        // it是指向Person对象的迭代器，用->访问成员
        cout << "姓名：" << it->name << "，年龄：" << it->age << endl;
    }

    return 0;
}
// 视频输出：
// vector中的Person：
// 姓名：张三，年龄：18
// 姓名：李四，年龄：19
// 姓名：王五，年龄：20
```

### 4.stL 容器扩展

#### （1）string 容器（视频第 23-30 集）

- **核心接口**：构造、赋值、拼接、查找替换、比较、存取、插入删除、子串。
- **视频核心代码**：

cpp





```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    // 1. 构造函数
    string s1;                  // 空构造
    string s2("hello");         // 字符串构造
    string s3(5, 'a');          // 重复字符构造（5个'a'）
    string s4(s2);              // 拷贝构造
    cout << "s2=" << s2 << ", s3=" << s3 << ", s4=" << s4 << endl;  // hello, aaaaa, hello

    // 2. 赋值操作
    string s5;
    s5 = "world";               // 字符串赋值
    s5.assign("cpp", 2);        // 取前2个字符赋值（"cp"）
    cout << "s5=" << s5 << endl;  // cp

    // 3. 字符串拼接
    s2 += " world";             // +=拼接
    s2.append("!!!");           // append拼接
    cout << "s2=" << s2 << endl;  // hello world!!!

    // 4. 查找与替换
    int pos = s2.find("world");  // 查找子串位置（6）
    s2.replace(pos, 5, "cpp");   // 从pos开始，替换5个字符为"cpp"
    cout << "s2替换后=" << s2 << endl;  // hello cpp!!!

    // 5. 字符存取
    for (int i = 0; i < s2.size(); i++) {
        cout << s2[i] << " ";    // []存取
    }
    cout << endl;

    // 6. 插入与删除
    s2.insert(5, " my");        // 第5位插入" my"
    s2.erase(10, 3);            // 第10位开始，删除3个字符
    cout << "s2插入删除后=" << s2 << endl;  // hello my cpp

    // 7. 子串获取
    string sub = s2.substr(6, 2);  // 第6位开始，取2个字符
    cout << "子串=" << sub << endl;  // my

    return 0;
}
```

#### （2）deque 容器（视频第 38-43 集）

- **特点**：双端数组，支持两端插入删除（效率 O (1)），支持随机访问。
- **核心接口**：构造、赋值、大小、插入删除、存取、排序。
- **视频核心代码**：

cpp





```cpp
#include <iostream>
#include <deque>
#include <algorithm>  // sort用
using namespace std;

void printDeque(const deque<int>& d) {
    for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

int main() {
    // 1. 构造
    deque<int> d1;
    deque<int> d2(3, 10);  // 3个10
    deque<int> d3(d2.begin(), d2.end());
    printDeque(d2);  // 10 10 10

    // 2. 赋值
    d1.assign(d3.begin(), d3.end());
    printDeque(d1);  // 10 10 10

    // 3. 插入删除（两端）
    d1.push_back(20);   // 尾插
    d1.push_front(5);   // 头插
    printDeque(d1);     // 5 10 10 10 20
    d1.pop_back();      // 尾删
    d1.pop_front();     // 头删
    printDeque(d1);     // 10 10 10

    // 4. 随机存取
    cout << "d1[1] = " << d1[1] << endl;  // 10
    cout << "d1.at(2) = " << d1.at(2) << endl;  // 10

    // 5. 排序（STL的sort算法）
    deque<int> d4 = {3, 1, 4, 2};
    sort(d4.begin(), d4.end());  // 升序
    printDeque(d4);  // 1 2 3 4

    return 0;
}
```

#### （3）stack 容器（视频第 45-46 集）

- **特点**：栈，先进后出（LIFO），仅能访问栈顶元素。
- **核心接口**：`push()`（栈顶插入）、`pop()`（栈顶删除）、`top()`（获取栈顶）、`empty()`（判空）、`size()`（大小）。
- **视频核心代码**：

cpp







```cpp
#include <iostream>
#include <stack>
using namespace std;

int main() {
    stack<int> s;

    // 入栈
    s.push(10);
    s.push(20);
    s.push(30);

    // 栈大小
    cout << "栈大小：" << s.size() << endl;  // 3

    // 出栈（需判断非空）
    while (!s.empty()) {
        cout << "栈顶元素：" << s.top() << endl;  // 30, 20, 10
        s.pop();  // 删除栈顶
    }

    cout << "栈空后大小：" << s.size() << endl;  // 0

    return 0;
}
```

#### （4）queue 容器（视频第 47-48 集）

- **特点**：队列，先进先出（FIFO），仅能访问队头和队尾。
- **核心接口**：`push()`（队尾插入）、`pop()`（队头删除）、`front()`（队头）、`back()`（队尾）、`empty()`、`size()`。
- **视频核心代码**：

cpp







```cpp
#include <iostream>
#include <queue>
using namespace std;

int main() {
    queue<int> q;

    // 入队
    q.push(10);
    q.push(20);
    q.push(30);

    // 队列大小
    cout << "队列大小：" << q.size() << endl;  // 3

    // 出队（需判断非空）
    while (!q.empty()) {
        cout << "队头：" << q.front() << "，队尾：" << q.back() << endl;
        // 第一次：队头10，队尾30；第二次：队头20，队尾30
        q.pop();  // 删除队头
    }

    cout << "队空后大小：" << q.size() << endl;  // 0

    return 0;
}
```

#### （5）list 容器（视频第 49-56 集）

- **特点**：双向链表，任意位置插入删除效率 O (1)，不支持随机访问。
- **核心接口**：构造、赋值、大小、插入删除、反转、排序（`list::sort()`）。
- **视频核心代码**：

cpp







```cpp
#include <iostream>
#include <list>
using namespace std;

void printList(const list<int>& l) {
    for (list<int>::const_iterator it = l.begin(); it != l.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

int main() {
    // 1. 构造
    list<int> l1 = {3, 1, 4, 2};
    printList(l1);  // 3 1 4 2

    // 2. 插入删除
    list<int>::iterator it = l1.begin();
    it++;  // 指向1
    l1.insert(it, 10);  // 插入10
    printList(l1);      // 3 10 1 4 2
    l1.erase(it);       // 删除1
    printList(l1);      // 3 10 4 2

    // 3. 反转
    l1.reverse();
    printList(l1);  // 2 4 10 3

    // 4. 排序（list专属sort，不支持STL的sort）
    l1.sort();  // 升序
    printList(l1);  // 2 3 4 10

    return 0;
}
```

#### （6）set 容器（视频第 57-64 集）

- **特点**：红黑树实现，元素唯一且自动升序排序（`multiset`允许重复）。
- **核心接口**：构造、赋值、插入删除、查找（`find()`）、统计（`count()`）、排序规则。
- **视频核心代码**：

cpp







```cpp
#include <iostream>
#include <set>
using namespace std;

// 自定义排序规则：降序（仿函数）
class MyCompare {
public:
    bool operator()(int a, int b) {
        return a > b;
    }
};

int main() {
    // 1. 默认排序（升序，元素唯一）
    set<int> s1;
    s1.insert(30);
    s1.insert(10);
    s1.insert(20);
    s1.insert(30);  // 重复元素不插入
    for (set<int>::iterator it = s1.begin(); it != s1.end(); it++) {
        cout << *it << " ";  // 10 20 30
    }
    cout << endl;

    // 2. 自定义排序（降序）
    set<int, MyCompare> s2;
    s2.insert(30);
    s2.insert(10);
    s2.insert(20);
    for (set<int, MyCompare>::iterator it = s2.begin(); it != s2.end(); it++) {
        cout << *it << " ";  // 30 20 10
    }
    cout << endl;

    // 3. 查找与统计
    set<int>::iterator pos = s1.find(20);
    if (pos != s1.end()) {
        cout << "找到：" << *pos << endl;  // 20
    }
    cout << "30的个数：" << s1.count(30) << endl;  // 1（set唯一）

    return 0;
}
```

#### （7）map 容器（视频第 65-69 集）

- **特点**：红黑树实现，键值对（key-value），key 唯一且自动升序排序。
- **核心接口**：构造、赋值、插入删除、查找（`find()`）、统计（`count()`）、排序。
- **视频核心代码**：

cpp







```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;

// 自定义排序：key降序
class MyCompare {
public:
    bool operator()(int a, int b) {
        return a > b;
    }
};

int main() {
    // 1. 默认排序（key升序）
    map<int, string> m1;
    // 插入方式
    m1.insert(pair<int, string>(1, "张三"));
    m1.insert(make_pair(2, "李四"));
    m1[3] = "王五";  // 简洁方式
    // 遍历
    for (map<int, string>::iterator it = m1.begin(); it != m1.end(); it++) {
        cout << "key=" << it->first << ", value=" << it->second << endl;
    }
    // 输出：key=1(张三), key=2(李四), key=3(王五)

    // 2. 自定义排序（key降序）
    map<int, string, MyCompare> m2;
    m2.insert(make_pair(1, "张三"));
    m2.insert(make_pair(2, "李四"));
    for (map<int, string, MyCompare>::iterator it = m2.begin(); it != m2.end(); it++) {
        cout << "key=" << it->first << ", value=" << it->second << endl;
    }
    // 输出：key=2(李四), key=1(张三)

    // 3. 查找与统计
    map<int, string>::iterator pos = m1.find(2);
    if (pos != m1.end()) {
        cout << "找到key=2：" << pos->second << endl;  // 李四
    }
    cout << "key=3的个数：" << m1.count(3) << endl;  // 1（map key唯一）

    return 0;
}
```

### 5.STL 函数对象与谓词（视频第 71-76 集）

#### （1）函数对象（视频第 71 集）

- **定义**：重载`()`运算符的类 / 结构体（又称 “仿函数”），可像函数一样调用。
- **视频核心代码**：

cpp







```cpp
#include <iostream>
using namespace std;

// 函数对象类
class MyAdd {
public:
    int operator()(int a, int b) {
        return a + b;
    }
};

int main() {
    MyAdd add;
    // 像函数一样调用
    int result = add(10, 20);
    cout << "10+20=" << result << endl;  // 30

    // 匿名函数对象（直接使用，无需命名）
    cout << "30+40=" << MyAdd()(30, 40) << endl;  // 70

    return 0;
}
```

#### （2）谓词（视频第 72-73 集）

- **定义**：返回`bool`类型的函数对象，分为：
	- 一元谓词：参数个数为 1；
	- 二元谓词：参数个数为 2。
- **视频核心代码**：

cpp







```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

// 1. 一元谓词：判断是否大于5
class Greater5 {
public:
    bool operator()(int num) {
        return num > 5;
    }
};

// 2. 二元谓词：降序排序
class MyCompare {
public:
    bool operator()(int a, int b) {
        return a > b;
    }
};

int main() {
    vector<int> v = {3, 7, 2, 9, 5};

    // 一元谓词：查找第一个大于5的元素
    vector<int>::iterator it = find_if(v.begin(), v.end(), Greater5());
    if (it != v.end()) {
        cout << "第一个大于5的元素：" << *it << endl;  // 7
    }

    // 二元谓词：降序排序
    sort(v.begin(), v.end(), MyCompare());
    for (int num : v) {
        cout << num << " ";  // 9 7 5 3 2
    }

    return 0;
}
```

#### （3）内建函数对象（视频第 74-76 集）

- **定义**：STL 提供的预定义函数对象，需包含头文件`<functional>`。
- **分类**：
	- 算术仿函数：`plus<T>`（加）、`minus<T>`（减）；
	- 关系仿函数：`greater<T>`（大于）、`less<T>`（小于）；
	- 逻辑仿函数：`logical_and<T>`（与）、`logical_or<T>`（或）。
- **视频核心代码**：

cpp







```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <functional>  // 内建函数对象头文件
using namespace std;

int main() {
    // 1. 算术仿函数：plus（加）
    plus<int> p;
    cout << "10+20=" << p(10, 20) << endl;  // 30

    // 2. 关系仿函数：greater（大于，用于排序）
    vector<int> v = {3, 1, 4, 2};
    sort(v.begin(), v.end(), greater<int>());  // 降序
    for (int num : v) {
        cout << num << " ";  // 4 3 2 1
    }
    cout << endl;

    // 3. 逻辑仿函数：logical_and（与）
    vector<bool> v1 = {true, true, false};
    vector<bool> v2 = {true, false, false};
    vector<bool> v3(3);
    transform(v1.begin(), v1.end(), v2.begin(), v3.begin(), logical_and<bool>());
    for (bool b : v3) {
        cout << boolalpha << b << " ";  // true false false
    }

    return 0;
}
```

### 6.STL 常用算法（视频第 77-97 集）

#### （1）遍历算法（for_each、transform）

cpp







```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

// for_each回调函数
void print(int num) {
    cout << num << " ";
}

// transform回调函数（将a+b存入c）
int add(int a, int b) {
    return a + b;
}

int main() {
    vector<int> v = {1, 2, 3, 4, 5};
    // for_each：遍历并执行函数
    for_each(v.begin(), v.end(), print);  // 1 2 3 4 5
    cout << endl;

    // transform：将两个容器元素运算后存入第三个容器
    vector<int> v1 = {1, 2, 3};
    vector<int> v2 = {4, 5, 6};
    vector<int> v3(3);
    transform(v1.begin(), v1.end(), v2.begin(), v3.begin(), add);
    for_each(v3.begin(), v3.end(), print);  // 5 7 9

    return 0;
}
```

#### （2）查找算法（find、find_if、count、count_if）

cpp







```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
using namespace std;

// 查找自定义类型（Person）
class Person {
public:
    string name;
    int age;
    Person(string n, int a) : name(n), age(a) {}
    // 重载==（find需要）
    bool operator==(const Person& p) {
        return this->name == p.name && this->age == p.age;
    }
};

// 谓词：年龄大于18
class AgeGreater18 {
public:
    bool operator()(const Person& p) {
        return p.age > 18;
    }
};

int main() {
    // 1. find：查找内置类型
    vector<int> v = {10, 20, 30};
    vector<int>::iterator it = find(v.begin(), v.end(), 20);
    cout << "找到：" << *it << endl;  // 20

    // 2. find_if：查找自定义类型（谓词）
    vector<Person> vp;
    vp.push_back(Person("张三", 18));
    vp.push_back(Person("李四", 20));
    vector<Person>::iterator itp = find_if(vp.begin(), vp.end(), AgeGreater18());
    cout << "年龄>18：" << itp->name << endl;  // 李四

    // 3. count：统计内置类型
    cout << "20的个数：" << count(v.begin(), v.end(), 20) << endl;  // 1

    // 4. count_if：统计自定义类型
    cout << "年龄>18的人数：" << count_if(vp.begin(), vp.end(), AgeGreater18()) << endl;  // 1

    return 0;
}
```

#### （3）排序算法（sort、random_shuffle、merge、reverse）

cpp







```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <ctime>
using namespace std;

int main() {
    // 1. sort：排序（升序/降序）
    vector<int> v = {3, 1, 4, 2};
    sort(v.begin(), v.end());  // 升序
    for (int num : v) cout << num << " ";  // 1 2 3 4
    cout << endl;
    sort(v.begin(), v.end(), greater<int>());  // 降序
    for (int num : v) cout << num << " ";  // 4 3 2 1
    cout << endl;

    // 2. random_shuffle：随机打乱
    srand((unsigned int)time(NULL));  // 随机种子
    random_shuffle(v.begin(), v.end());
    for (int num : v) cout << num << " ";  // 随机顺序
    cout << endl;

    // 3. merge：合并两个有序容器（需提前开辟空间）
    vector<int> v1 = {1, 3, 5};
    vector<int> v2 = {2, 4, 6};
    vector<int> v3(v1.size() + v2.size());
    merge(v1.begin(), v1.end(), v2.begin(), v2.end(), v3.begin());
    for (int num : v3) cout << num << " ";  // 1 2 3 4 5 6
    cout << endl;

    // 4. reverse：反转
    reverse(v3.begin(), v3.end());
    for (int num : v3) cout << num << " ";  // 6 5 4 3 2 1

    return 0;
}
```

#### （4）拷贝替换算法（copy、replace、replace_if、swap）

cpp







```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    // 1. copy：拷贝
    vector<int> v1 = {1, 2, 3, 4, 5};
    vector<int> v2(5);
    copy(v1.begin(), v1.end(), v2.begin());
    for (int num : v2) cout << num << " ";  // 1 2 3 4 5
    cout << endl;

    // 2. replace：替换指定值
    replace(v2.begin(), v2.end(), 3, 30);
    for (int num : v2) cout << num << " ";  // 1 2 30 4 5
    cout << endl;

    // 3. replace_if：按条件替换（大于5的替换为100）
    replace_if(v2.begin(), v2.end(), [](int num){return num>5;}, 100);
    for (int num : v2) cout << num << " ";  // 1 2 100 4 5
    cout << endl;

    // 4. swap：交换两个容器
    swap(v1, v2);
    for (int num : v1) cout << num << " ";  // 1 2 100 4 5

    return 0;
}
```

### 7.文件操作（视频第 61-64 集）

#### （1）文本文件（读 / 写）

cpp







```cpp
#include <iostream>
#include <fstream>  // 文件操作头文件
#include <string>
using namespace std;

// 1. 文本文件写
void writeText() {
    ofstream ofs;
    ofs.open("test.txt", ios::out);  // 打开文件，写模式
    // 或直接初始化：ofstream ofs("test.txt", ios::out);

    // 写内容
    ofs << "姓名：张三" << endl;
    ofs << "年龄：18" << endl;

    // 关闭文件
    ofs.close();
}

// 2. 文本文件读
void readText() {
    ifstream ifs;
    ifs.open("test.txt", ios::in);  // 打开文件，读模式
    if (!ifs.is_open()) {  // 判断是否打开成功
        cout << "文件打开失败！" << endl;
        return;
    }

    // 读方式1：char数组
    char buf[1024] = {0};
    while (ifs >> buf) {  // 空格/换行分隔
        cout << buf << endl;
    }

    // 读方式2：getline
    // string buf;
    // while (getline(ifs, buf)) {
    //     cout << buf << endl;
    // }

    ifs.close();
}

int main() {
    writeText();
    readText();
    return 0;
}
```

#### （2）二进制文件（读 / 写）

cpp







```cpp
#include <iostream>
#include <fstream>
#include <string>
using namespace std;

// 自定义类型
class Person {
public:
    char name[64];  // 用char数组，避免string的指针问题
    int age;
};

// 1. 二进制文件写
void writeBinary() {
    ofstream ofs("person.dat", ios::out | ios::binary);  // 二进制写模式
    Person p = {"张三", 18};

    // 写：&p取地址，sizeof(p)字节数
    ofs.write((const char*)&p, sizeof(p));

    ofs.close();
}

// 2. 二进制文件读
void readBinary() {
    ifstream ifs("person.dat", ios::in | ios::binary);  // 二进制读模式
    if (!ifs.is_open()) {
        cout << "文件打开失败！" << endl;
        return;
    }

    Person p;
    // 读：&p存数据地址，sizeof(p)字节数
    ifs.read((char*)&p, sizeof(p));
    cout << "姓名：" << p.name << "，年龄：" << p.age << endl;  // 张三，18

    ifs.close();
}

int main() {
    writeBinary();
    readBinary();
    return 0;
}
```

### 8.实战案例（视频第 44、70 集）

#### （1）评委打分（视频第 44 集）

- **需求**：10 个评委打分，去掉最高 / 最低分，求平均分。
- **视频核心代码**：

cpp







```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <ctime>
using namespace std;

int main() {
    srand((unsigned int)time(NULL));
    vector<int> scores;

    // 1. 生成10个评委打分（0-100）
    for (int i = 0; i < 10; i++) {
        int score = rand() % 101;
        scores.push_back(score);
        cout << score << " ";
    }
    cout << endl;

    // 2. 排序（升序）
    sort(scores.begin(), scores.end());

    // 3. 去掉最高/最低分（删除首尾）
    scores.erase(scores.begin());
    scores.erase(scores.end() - 1);

    // 4. 计算平均分
    int sum = 0;
    for (int s : scores) {
        sum += s;
    }
    double avg = sum / (double)scores.size();

    cout << "平均分：" << avg << endl;
    return 0;
}
```

#### （2）员工分组（视频第 70 集）

- **需求**：将员工按部门分组（部门编号 1-3），用 multimap 存储。
- **视频核心代码**：

cpp







```cpp
#include <iostream>
#include <vector>
#include <map>
#include <string>
#include <ctime>
using namespace std;

// 员工类
class Employee {
public:
    string name;
    int deptId;  // 部门编号：1-3
};

// 创建员工
void createEmployees(vector<Employee>& emps) {
    string names[] = {"张三", "李四", "王五", "赵六", "孙七"};
    srand((unsigned int)time(NULL));
    for (int i = 0; i < 5; i++) {
        Employee e;
        e.name = names[i];
        e.deptId = rand() % 3 + 1;  // 1-3
        emps.push_back(e);
    }
}

// 员工分组
void groupEmployees(vector<Employee>& emps, multimap<int, Employee>& deptEmps) {
    for (Employee e : emps) {
        // multimap允许key重复，插入部门-员工对
        deptEmps.insert(make_pair(e.deptId, e));
    }
}

// 打印分组
void printGroups(multimap<int, Employee>& deptEmps) {
    // 遍历每个部门
    for (int dept = 1; dept <= 3; dept++) {
        cout << "部门" << dept << "：";
        // 查找部门对应的员工范围
        auto range = deptEmps.equal_range(dept);
        for (auto it = range.first; it != range.second; it++) {
            cout << it->second.name << " ";
        }
        cout << endl;
    }
}

int main() {
    vector<Employee> emps;
    multimap<int, Employee> deptEmps;

    createEmployees(emps);
    groupEmployees(emps, deptEmps);
    printGroups(deptEmps);

    return 0;
}
```

### 9.智能指针（现代 C++：解决内存泄漏）

智能指针是`C++11`引入的**模板类**，通过 “RAII（资源获取即初始化）” 机制，自动管理堆区内存（无需手动`delete`），彻底避免野指针和内存泄漏。

#### 1. 智能指针的核心原理

智能指针本质是 “封装了普通指针的类”，在**析构函数中自动调用`delete`**，当智能指针对象出作用域时，析构函数触发，自动释放指向的堆区内存。

#### 2. 四种智能指针（视频重点：unique_ptr、shared_ptr）

##### （1）auto_ptr（C++98，已弃用，不推荐）

- 特点：独占所有权，但转移所有权时存在风险（原指针会变成空指针，访问时崩溃）；
- 替代方案：`unique_ptr`（更安全的独占智能指针）。

cpp







```cpp
#include <iostream>
#include <memory>  // 智能指针头文件
using namespace std;

int main() {
    auto_ptr<int> p1(new int(10));
    auto_ptr<int> p2 = p1;  // 所有权转移：p1失去所有权，变为空指针

    // cout << *p1 << endl;  // 错误：p1已空，访问崩溃
    cout << *p2 << endl;  // 10（p2获得所有权）

    return 0;
}
```

##### （2）unique_ptr（独占型智能指针，推荐）

- 核心特点：**独占所有权**（同一时间只能有一个`unique_ptr`指向堆区内存），禁止拷贝和赋值（编译时报错，避免`auto_ptr`的风险）；
- 转移所有权：需用`std::move()`显式转移，转移后原指针失效。

cpp







```cpp
#include <iostream>
#include <memory>
using namespace std;

int main() {
    // 1. 创建unique_ptr（独占指向堆区int=10）
    unique_ptr<int> p1(new int(10));
    cout << "p1 = " << *p1 << endl;  // 10

    // 2. 禁止拷贝和赋值（编译报错）
    // unique_ptr<int> p2 = p1;  // 错误：拷贝禁止
    // p2 = p1;                  // 错误：赋值禁止

    // 3. 显式转移所有权（std::move()）
    unique_ptr<int> p2 = move(p1);  // p1失去所有权，变为空指针
    cout << "p2 = " << *p2 << endl;  // 10
    // cout << *p1 << endl;        // 错误：p1已空

    // 4. 自动释放：p2出作用域时，析构函数自动delete
    return 0;
}
```

##### （3）shared_ptr（共享型智能指针，推荐）

- 核心特点：**共享所有权**（多个`shared_ptr`可指向同一块堆区内存），通过 “引用计数” 管理内存；
- 引用计数：每增加一个`shared_ptr`指向内存，计数 + 1；每销毁一个`shared_ptr`，计数 - 1；当计数为 0 时，自动释放内存。

cpp







```cpp
#include <iostream>
#include <memory>
using namespace std;

void printCount(const shared_ptr<int>& p, const string& name) {
    cout << name << "：值=" << *p << "，引用计数=" << p.use_count() << endl;
}

int main() {
    // 1. 创建shared_ptr，引用计数=1
    shared_ptr<int> p1(new int(10));
    printCount(p1, "p1");  // p1：值=10，引用计数=1

    // 2. 共享所有权：引用计数+1（变为2）
    shared_ptr<int> p2 = p1;
    printCount(p2, "p2");  // p2：值=10，引用计数=2
    printCount(p1, "p1");  // p1：值=10，引用计数=2

    // 3. 共享所有权：引用计数+1（变为3）
    shared_ptr<int> p3(p1);
    printCount(p3, "p3");  // p3：值=10，引用计数=3

    // 4. 销毁p2：引用计数-1（变为2）
    p2.reset();  // reset()：释放所有权，引用计数-1
    if (p2 == nullptr) {
        cout << "p2已空" << endl;
    }
    printCount(p1, "p1");  // p1：值=10，引用计数=2

    // 5. 自动释放：main结束时，p1、p3销毁，引用计数变为0，内存释放
    return 0;
}
```

**视频注意事项**：

- 避免`shared_ptr`的 “循环引用”（如 A 和 B 的`shared_ptr`互相指向对方），会导致引用计数无法归零，内存泄漏；
- 解决循环引用：用`weak_ptr`（弱引用智能指针，不增加引用计数）。

##### （4）weak_ptr（弱引用智能指针，辅助 shared_ptr）

- 核心特点：**不拥有所有权**，指向`shared_ptr`管理的内存，但不增加引用计数；
- 用途：解决`shared_ptr`的循环引用问题，需通过`lock()`获取`shared_ptr`后才能访问内存。

cpp







```cpp
#include <iostream>
#include <memory>
using namespace std;

// 循环引用场景：A和B互相指向对方
class B;  // 前置声明
class A {
public:
    shared_ptr<B> b_ptr;  // A的shared_ptr指向B
    ~A() { cout << "A析构" << endl; }
};
class B {
public:
    weak_ptr<A> a_ptr;    // B的weak_ptr指向A（不增加引用计数）
    ~B() { cout << "B析构" << endl; }
};

int main() {
    shared_ptr<A> a(new A());
    shared_ptr<B> b(new B());

    // 互相指向：a的b_ptr指向b（引用计数+1），b的a_ptr指向a（不增加引用计数）
    a->b_ptr = b;
    b->a_ptr = a;

    // 引用计数：a的计数=1，b的计数=2（a->b_ptr指向b，所以b的计数+1）
    cout << "a引用计数：" << a.use_count() << endl;  // 1
    cout << "b引用计数：" << b.use_count() << endl;  // 2

    // main结束时：a销毁（计数=0，A析构）→ b的计数=1（a->b_ptr销毁）→ b销毁（计数=0，B析构）
    // 无内存泄漏（因B用weak_ptr指向A，避免循环引用）
    return 0;
}
```

### 10、总结（C++ 核心知识脉络）

1. **基础语法**：变量、函数、数组、指针、结构体；
2. **面向对象**：类与对象、构造 / 析构、访问权限、继承、多态（虚函数）、深拷贝；
3. **高级特性**：引用、函数重载、运算符重载、模板、异常处理、虚继承；
4. **STL 容器**：vector（动态数组）、list（双向链表）、map（键值对）；
5. **内存管理**：内存分区、智能指针（unique_ptr、shared_ptr）。