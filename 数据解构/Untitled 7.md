# 图的基本概念与存储结构

## 1. 图的引入与核心应用场景

图是解决**计算机中复杂路径搜索问题**的核心数据结构，广泛应用于多场景，且在 408 考研数据结构部分占 1/4 分值（约 10-13 分），需重点掌握。

### 1.1 典型应用案例解析

#### （1）传教士与野人问题（图搜索策略入门）

- **问题描述**：3 个传教士、3 个野人需从左岸渡到右岸，约束条件：
	1. 船最多载 2 人（不分传教士 / 野人）；
	2. 任意岸（左岸 / 右岸）及船上，传教士人数≥野人数（避免传教士被吃）；
	3. 船需人驾驶返回（非遥控，无法自动飘回）。
- **状态抽象方法**：用**三元组（左岸传教士数，左岸野人数，船的位置）** 表示每个状态，其中：
	- 船的位置：1 = 在左岸，0 = 在右岸；
	- 初始状态：(3, 3, 1)（左岸 3 传 3 野，船在左）；
	- 目标状态：(0, 0, 0)（左岸无人无船，全到右岸）。
- **解决逻辑**：通过枚举合法状态转移（如 “2 野人渡右→1 野人返左→2 传教士渡右” 等步骤），形成状态路径，本质是图的遍历搜索。

#### （2）游戏场景中的图应用（底层为二维数组存储）

视频以多款游戏为例，说明图的底层通过**二维数组（矩阵）** 实现 “坐标映射” 与 “状态标识”，核心逻辑如下：

##### ① 贪吃蛇游戏

- **二维数组定义规则**：

	- 0：蛇可移动的空白区域；
	- 1：蛇身（不可重叠）；
	- 2：障碍物（不可穿过）。

- **示例初始地图（5×5 矩阵）**：

	plaintext

	```plaintext
	[
	  [1, 1, 1, 0, 2],  // 第一行：蛇身（3节）、空白、障碍物
	  [0, 0, 0, 0, 0],  // 第二行：全空白
	  [0, 2, 0, 0, 0],  // 第三行：中间有障碍物
	  [0, 0, 0, 0, 0],  // 第四行：全空白
	  [0, 0, 0, 0, 2]   // 第五行：右下角有障碍物
	]
	```

- **移动与动画逻辑**：

	1. 键盘监听：如按↑键，触发蛇头 “向上移动”（行号 - 1）；
	2. 坐标更新：每隔 0.5 秒校验新坐标合法性（不越界、非障碍物、非蛇身），更新蛇头位置，同时删除蛇尾（设为 0）、新增蛇头（设为 1）；
	3. 动画实现：1 秒切换 30 帧蛇身图片（如向上移动对应 3 帧 “抬头” 图片），循环播放模拟 “行走效果”。

##### ② 仙剑奇侠传（角色移动）

- **底层存储**：用二维数组存储游戏地图坐标，角色（如李逍遥）位置用 “(x, y)” 表示（x = 行号，y = 列号）；
- **移动逻辑**：
	- 方向控制：上下左右移动对应坐标变化（如↑键→y 值 - 1，→键→x 值 + 1）；
	- 动画实现：每个方向对应一组图片（如向上走 3 帧、向下走 3 帧），1 秒切换 30 帧，结合坐标更新实现 “角色行走”。

##### ③ MOBA 游戏（如王者荣耀）

- **地图标识**：二维数组标记地形类型（如草坪 = 2、墙壁 = 1、道路 = 0）；
- **效果触发**：角色坐标落在对应地形时，触发属性变化（如成吉思汗在草坪（值 = 2）时，移速增加）。

#### （3）其他场景

- 导航软件：路径规划（最短路径、最优路线）本质是图的搜索；
- 棋类游戏（围棋、五子棋）：棋盘用二维数组表示，落子对应坐标更新，胜负判断对应图的状态校验。

## 2. 图的核心定义与基础概念

视频强调 “图的概念是后续学习的核心，需重点记忆”，所有概念均来自视频讲解：

### 2.1 图的本质定义

- **定义**：图（Graph）由**顶点的有穷非空集合**（Vertex，简称 “顶点”）和**顶点之间边的集合**（Edge，简称 “边”）组成，记为 `G=(V, E)`。
- **关键约束**：
	1. 顶点集合`V`必须 “有穷且非空”（至少 1 个顶点，区别于 “空树”）；
	2. 边集合`E`可空（仅含顶点无关联边，是合法图）。
- **术语对应**：
	- `V`（Vertex）：顶点集合（图中 “圈”，如 0、1、2 等标识）；
	- `E`（Edge）：边集合（顶点间 “连线”，无向图是直线，有向图是箭头）。

### 2.2 有向图与无向图（按边的方向划分）

| 类型   | 定义                                             | 边的表示方式                              | 特殊术语                                                     |
| ------ | ------------------------------------------------ | ----------------------------------------- | ------------------------------------------------------------ |
| 无向图 | 边无方向，顶点 A 与 B 的边可双向通行             | 小括号 `(A,B)`（`(A,B)` 与 `(B,A)` 等价） | 边（无特殊名称）                                             |
| 有向图 | 边有方向，仅允许从 “出发顶点” 到 “目标顶点” 通行 | 尖括号 `<A,B>`（`<A,B>` 与 `<B,A>` 不同） | 边称为 “弧”： - 弧尾：出发顶点（`<A,B>` 中的 A） - 弧头：目标顶点（`<A,B>` 中的 B） |

- **示例**：
	- 无向图边集合：`E={(0,1), (0,2), (1,3)}`；
	- 有向图边集合：`E={<0,1>, <1,0>, <1,2>}`（`<0,1>` 是 0→1，`<1,0>` 是 1→0）。

### 2.3 简单图与多重图（按边的合法性划分）

视频明确 “课程仅关注简单图，多重图不做重点”：

- **简单图**：满足两个限制：
	1. 无 “自环边”（顶点不与自身连边，如 `<1,1>` 不合法）；
	2. 无 “重复边”（同一对顶点间无多条边，如无两个 `(0,1)` 或 `<0,1>`）。
- **多重图**：不满足上述任一限制（含自环或重复边），课程中不讨论。

### 2.4 完全图（边数最多的简单图）

“完全图” 是 “边数达到最大值” 的简单图，需区分无向与有向：

| 类型       | 顶点数 | 最大边数公式 | 核心逻辑                                                     |
| ---------- | ------ | ------------ | ------------------------------------------------------------ |
| 无向完全图 | n      | n(n-1)/2     | 每个顶点与其他所有顶点连 1 条边（如 n=4 时，边数 = 4×3/2=6） |
| 有向完全图 | n      | n(n-1)       | 每个顶点与其他所有顶点连 2 条有向边（一去一回，如 n=4 时，边数 = 4×3=12） |

- **示例**：n=3 的无向完全图，边集合为 `{(0,1), (0,2), (1,2)}`（3 条边，符合 3×2/2=3）。

### 2.5 路径与回路（图的顶点序列关系）

| 术语       | 定义                                                    | 关键特征                                        |
| ---------- | ------------------------------------------------------- | ----------------------------------------------- |
| 路径       | 从顶点 V 到 W 的顶点序列（如 V→A→B→W）                  | 路径长度 = 边的条数（如 V→A→B→W 长度 = 3）      |
| 回路（环） | 起点 = 终点的路径（如 V→A→B→V）                         | 长度≥1（至少 1 条边，避免 “单个顶点” 视为回路） |
| 简单路径   | 路径中无重复顶点（如 V→A→B→W，所有顶点仅出现 1 次）     | 长度≤n-1（n 为顶点数，最多经过所有顶点 1 次）   |
| 简单回路   | 除 “起点 = 终点” 外，其余顶点无重复的回路（如 V→A→B→V） | 长度≤n（最多经过所有顶点 1 次后回到起点）       |

- **示例**：
	- 简单路径：0→3→1（长度 2，顶点无重复）；
	- 非简单路径：0→1→3→1（顶点 1 重复）；
	- 简单回路：0→3→1→0（仅起点 0 重复）。

### 2.6 顶点的度（顶点与边的关联数量）

度是 “顶点关联的边数”，无向图与有向图定义不同：

| 图类型 | 度的定义                                                     | 相关公式                                                     |
| ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 无向图 | 顶点 V 的度 = 与 V 关联的边数（记为 `deg(V)`）               | 所有顶点的度之和 = 2× 边数（`Σdeg(V) = 2E`，每条边贡献 2 个度） |
| 有向图 | 分 “入度” 和 “出度”： - 入度（`in_deg(V)`）：箭头指向 V 的边数； - 出度（`out_deg(V)`）：从 V 出发的边数 | 1. 顶点总度 = 入度 + 出度； 2. 所有顶点入度之和 = 所有顶点出度之和 = 边数（`Σin_deg(V) = Σout_deg(V) = E`） |

- **示例（有向图）**：
	- 顶点 1 的入度：若有 `<0,1>`、`<2,1>`，则 `in_deg(1)=2`；
	- 顶点 1 的出度：若有 `<1,3>`，则 `out_deg(1)=1`；
	- 总入度之和 = 总出度之和 = 边数（3 条边则入度和 = 3，出度和 = 3）。

### 2.7 子图（图的子集关系）

- **定义**：若图 `H=(V_H, E_H)` 满足：
	1. `V_H` 是原图 `G` 顶点集 `V_G` 的子集（子图顶点来自原图）；
	2. `E_H` 是原图 `G` 边集 `E_G` 的子集（子图边来自原图，且边的顶点在 `V_H` 中），则 `H` 是 `G` 的子图。
- **示例**：原图 `G=(V={0,1,2,3}, E={(0,1),(0,2),(1,3)})`，子图 `H=(V={0,1,3}, E={(0,1),(1,3)})`（顶点和边均为子集）。

### 2.8 连通性相关概念（无向图 vs 有向图）

连通性是 “顶点间是否有路径” 的属性，无向图与有向图定义差异较大：

#### （1）无向图的连通性

| 术语     | 定义                                                         |
| -------- | ------------------------------------------------------------ |
| 连通     | 顶点 V 到 W 有路径（允许绕路，如 V→A→W）                     |
| 连通图   | 任意两个顶点都连通的无向图（如 n=4 的无向完全图）            |
| 连通分量 | 无向图的 “极大连通子图”（满足：①子图；②连通；③无法添加原图其他顶点仍连通） |

- **示例**：无向图 `G=(V={0,1,2,3,4,5}, E={(0,1),(1,2),(2,3),(4,5)})`，连通分量有 2 个：
	1. `H1=(V={0,1,2,3}, E={(0,1),(1,2),(2,3)})`（添加 4/5 后不连通）；
	2. `H2=(V={4,5}, E={(4,5)})`（添加 0-3 后不连通）。

#### （2）有向图的连通性（强连通）

有向图需 “双向连通” 才视为强连通，定义更严格：

| 术语       | 定义                                                         |
| ---------- | ------------------------------------------------------------ |
| 强连通     | 对顶点 V 和 W，“V→W 有路径” 且 “W→V 有路径”                  |
| 强连通图   | 任意两个顶点都强连通的有向图（如 `<0,1>`、`<1,0>` 组成的图） |
| 强连通分量 | 有向图的 “极大强连通子图”（满足：①子图；②强连通；③无法添加原图其他顶点仍强连通） |

- **示例**：有向图 `G=(V={0,1,2}, E={<0,1>, <1,0>, <1,2>})`，强连通分量有 2 个：
	1. `H1=(V={0,1}, E={<0,1>, <1,0>})`（0 与 1 双向连通，添加 2 后不连通）；
	2. `H2=(V={2}, E=∅)`（单个顶点视为强连通分量）。

## 3. 图的存储雏形（视频重点：二维数组）

视频未提及 “邻接矩阵”“邻接表” 等标准存储结构，但核心铺垫 “二维数组是图存储的基础”，以游戏场景为例整理核心代码：

### 3.1 贪吃蛇地图存储与移动逻辑（Java 风格伪代码）

#### （1）定义地图与蛇初始状态

java

```java
// 地图：0=空白，1=蛇身，2=障碍物（5×5矩阵）
int[][] map = {
    {1, 1, 1, 0, 2},  // 第一行：蛇身（3节）
    {0, 0, 0, 0, 0},  // 第二行：空白
    {0, 2, 0, 0, 0},  // 第三行：障碍物
    {0, 0, 0, 0, 0},  // 第四行：空白
    {0, 0, 0, 0, 2}   // 第五行：障碍物
};

// 蛇头初始坐标（行号x，列号y）：对应map[0][2]（蛇身末尾）
int snakeHeadX = 0;
int snakeHeadY = 2;
```

#### （2）键盘监听与坐标更新（向上移动为例）

java

```java
// 监听↑键（KeyEvent.VK_UP）
if (key == KeyEvent.VK_UP) {
    // 计算新坐标（向上移动：行号-1，列号不变）
    int newX = snakeHeadX - 1;
    int newY = snakeHeadY;
    
    // 校验合法性：不越界、非蛇身、非障碍物
    if (newX >= 0 && map[newX][newY] != 1 && map[newX][newY] != 2) {
        // 更新蛇头：新位置设为1（蛇身）
        map[newX][newY] = 1;
        // 更新蛇尾：原尾部设为0（空白）（假设蛇长3，尾部在x+2处）
        map[snakeHeadX + 2][snakeHeadY] = 0;
        // 更新蛇头坐标
        snakeHeadX = newX;
    }
}
```

#### （3）帧动画实现（切换蛇身图片）

java

```java
// 蛇身图片数组：[方向][帧]，0=上，1=下，2=左，3=右（每个方向3帧）
Image[][] snakeImages = {
    {imgUp1, imgUp2, imgUp3},
    {imgDown1, imgDown2, imgDown3},
    {imgLeft1, imgLeft2, imgLeft3},
    {imgRight1, imgRight2, imgRight3}
};

// 帧计数器：100ms切换1帧（1秒30帧）
int frameCount = 0;
int direction = 0; // 当前方向：0=上
while (true) {
    // 绘制当前帧图片
    drawImage(snakeImages[direction][frameCount % 3]);
    // 延迟100ms
    Thread.sleep(100);
    frameCount++;
}
```



# 图的邻接矩阵实现

## 1. 无向图的邻接矩阵实现

### 1.1 核心逻辑

- 邻接矩阵为 `n×n` 对称矩阵（`adj[i][j] = adj[j][i]`）；
- `adj[i][j] = 1` 表示顶点 `i` 与 `j` 有边，`0` 表示无边；
- 禁止自环（`i ≠ j`），符合简单图定义。

### 1.2 完整代码

c

```c
#include <stdio.h>
#include <stdlib.h>

// 无向图邻接矩阵结构体
typedef struct {
    int n;          // 顶点数（n ≥ 1，有穷非空）
    int** adj;      // 邻接矩阵：adj[i][j] 表示顶点i与j的边关系
} UndirectedGraph;

/**
 * @brief 初始化无向图
 * @param n 顶点数（需 ≥ 1）
 * @return 初始化成功的图指针，失败返回NULL
 */
UndirectedGraph* undir_graph_init(int n) {
    if (n < 1) {
        printf("错误：顶点数必须≥1（图的顶点集非空）\n");
        return NULL;
    }

    // 1. 分配图结构体内存
    UndirectedGraph* graph = (UndirectedGraph*)malloc(sizeof(UndirectedGraph));
    if (graph == NULL) {
        printf("错误：图结构体内存分配失败\n");
        return NULL;
    }
    graph->n = n;

    // 2. 分配邻接矩阵（二维数组：n行n列）
    graph->adj = (int**)malloc(n * sizeof(int*));
    if (graph->adj == NULL) {
        printf("错误：邻接矩阵行内存分配失败\n");
        free(graph);  // 释放已分配的结构体内存
        return NULL;
    }
    for (int i = 0; i < n; i++) {
        graph->adj[i] = (int*)calloc(n, sizeof(int));  // 初始化为0（无边）
        if (graph->adj[i] == NULL) {
            printf("错误：邻接矩阵第%d列内存分配失败\n", i);
            // 释放已分配的列内存
            for (int j = 0; j < i; j++) {
                free(graph->adj[j]);
            }
            free(graph->adj);
            free(graph);
            return NULL;
        }
    }

    return graph;
}

/**
 * @brief 向无向图添加边（i与j之间）
 * @param graph 无向图指针
 * @param i 顶点i（0 ≤ i < n）
 * @param j 顶点j（0 ≤ j < n）
 */
void undir_graph_add_edge(UndirectedGraph* graph, int i, int j) {
    if (graph == NULL) {
        printf("错误：图未初始化\n");
        return;
    }
    // 校验顶点合法性（不越界、非自环）
    if (i < 0 || i >= graph->n || j < 0 || j >= graph->n) {
        printf("错误：顶点%d或%d越界（合法范围：0~%d）\n", i, j, graph->n - 1);
        return;
    }
    if (i == j) {
        printf("错误：禁止自环（简单图无自环边）\n");
        return;
    }

    // 无向图：双向设置边（对称矩阵）
    graph->adj[i][j] = 1;
    graph->adj[j][i] = 1;
    printf("成功添加边：%d ↔ %d\n", i, j);
}

/**
 * @brief 打印无向图的邻接矩阵
 * @param graph 无向图指针
 */
void undir_graph_print(UndirectedGraph* graph) {
    if (graph == NULL) {
        printf("错误：图未初始化\n");
        return;
    }

    printf("\n无向图邻接矩阵（%d个顶点）：\n", graph->n);
    printf("   ");  // 表头对齐（顶点编号）
    for (int j = 0; j < graph->n; j++) {
        printf("%d  ", j);
    }
    printf("\n");
    for (int i = 0; i < graph->n; i++) {
        printf("%d: ", i);  // 行号（顶点i）
        for (int j = 0; j < graph->n; j++) {
            printf("%d  ", graph->adj[i][j]);
        }
        printf("\n");
    }
}

/**
 * @brief 销毁无向图（释放内存，避免泄漏）
 * @param graph 无向图指针（销毁后置为NULL）
 */
void undir_graph_destroy(UndirectedGraph** graph) {
    if (graph == NULL || *graph == NULL) {
        return;
    }

    // 1. 释放邻接矩阵的列内存
    for (int i = 0; i < (*graph)->n; i++) {
        free((*graph)->adj[i]);
    }
    // 2. 释放邻接矩阵的行内存
    free((*graph)->adj);
    // 3. 释放图结构体内存
    free(*graph);
    *graph = NULL;  // 避免野指针
    printf("\n无向图内存已释放\n");
}

// 测试：创建视频中的无向图示例（4个顶点，边：0-1、0-2、1-3）
int main_undirected() {
    // 1. 初始化4个顶点的无向图
    UndirectedGraph* graph = undir_graph_init(4);
    if (graph == NULL) {
        return 1;
    }

    // 2. 添加边（视频示例边）
    undir_graph_add_edge(graph, 0, 1);
    undir_graph_add_edge(graph, 0, 2);
    undir_graph_add_edge(graph, 1, 3);
    undir_graph_add_edge(graph, 2, 2);  // 测试自环（应报错）
    undir_graph_add_edge(graph, 4, 1);  // 测试顶点越界（应报错）

    // 3. 打印邻接矩阵（应输出对称矩阵）
    undir_graph_print(graph);

    // 4. 销毁图
    undir_graph_destroy(&graph);

    return 0;
}

// 若需单独运行无向图测试，取消注释下方代码
// int main() {
//     return main_undirected();
// }
```

## 2. 有向图的邻接矩阵实现

### 2.1 核心逻辑

- 邻接矩阵为 `n×n` 非对称矩阵（`adj[i][j]` 与 `adj[j][i]` 可不同）；
- `adj[i][j] = 1` 表示有从 `i` 到 `j` 的弧，`0` 表示无弧；
- 禁止自环（`i ≠ j`），符合简单图定义。

### 2.2 完整代码

c

```c
#include <stdio.h>
#include <stdlib.h>

// 有向图邻接矩阵结构体
typedef struct {
    int n;          // 顶点数（n ≥ 1，有穷非空）
    int** adj;      // 邻接矩阵：adj[i][j] 表示顶点i→j的弧关系
} DirectedGraph;

/**
 * @brief 初始化有向图
 * @param n 顶点数（需 ≥ 1）
 * @return 初始化成功的图指针，失败返回NULL
 */
DirectedGraph* dir_graph_init(int n) {
    if (n < 1) {
        printf("错误：顶点数必须≥1（图的顶点集非空）\n");
        return NULL;
    }

    // 1. 分配图结构体内存
    DirectedGraph* graph = (DirectedGraph*)malloc(sizeof(DirectedGraph));
    if (graph == NULL) {
        printf("错误：图结构体内存分配失败\n");
        return NULL;
    }
    graph->n = n;

    // 2. 分配邻接矩阵（二维数组：n行n列）
    graph->adj = (int**)malloc(n * sizeof(int*));
    if (graph->adj == NULL) {
        printf("错误：邻接矩阵行内存分配失败\n");
        free(graph);
        return NULL;
    }
    for (int i = 0; i < n; i++) {
        graph->adj[i] = (int*)calloc(n, sizeof(int));  // 初始化为0（无弧）
        if (graph->adj[i] == NULL) {
            printf("错误：邻接矩阵第%d列内存分配失败\n", i);
            for (int j = 0; j < i; j++) {
                free(graph->adj[j]);
            }
            free(graph->adj);
            free(graph);
            return NULL;
        }
    }

    return graph;
}

/**
 * @brief 向有向图添加弧（i→j）
 * @param graph 有向图指针
 * @param i 起点i（0 ≤ i < n）
 * @param j 终点j（0 ≤ j < n）
 */
void dir_graph_add_arc(DirectedGraph* graph, int i, int j) {
    if (graph == NULL) {
        printf("错误：图未初始化\n");
        return;
    }
    // 校验顶点合法性（不越界、非自环）
    if (i < 0 || i >= graph->n || j < 0 || j >= graph->n) {
        printf("错误：顶点%d或%d越界（合法范围：0~%d）\n", i, j, graph->n - 1);
        return;
    }
    if (i == j) {
        printf("错误：禁止自环（简单图无自环弧）\n");
        return;
    }

    // 有向图：仅单向设置弧（非对称矩阵）
    graph->adj[i][j] = 1;
    printf("成功添加弧：%d → %d\n", i, j);
}

/**
 * @brief 打印有向图的邻接矩阵
 * @param graph 有向图指针
 */
void dir_graph_print(DirectedGraph* graph) {
    if (graph == NULL) {
        printf("错误：图未初始化\n");
        return;
    }

    printf("\n有向图邻接矩阵（%d个顶点）：\n", graph->n);
    printf("   ");  // 表头对齐（顶点编号）
    for (int j = 0; j < graph->n; j++) {
        printf("%d  ", j);
    }
    printf("\n");
    for (int i = 0; i < graph->n; i++) {
        printf("%d: ", i);  // 行号（起点i）
        for (int j = 0; j < graph->n; j++) {
            printf("%d  ", graph->adj[i][j]);
        }
        printf("\n");
    }
}

/**
 * @brief 销毁有向图（释放内存，避免泄漏）
 * @param graph 有向图指针（销毁后置为NULL）
 */
void dir_graph_destroy(DirectedGraph** graph) {
    if (graph == NULL || *graph == NULL) {
        return;
    }

    // 1. 释放邻接矩阵的列内存
    for (int i = 0; i < (*graph)->n; i++) {
        free((*graph)->adj[i]);
    }
    // 2. 释放邻接矩阵的行内存
    free((*graph)->adj);
    // 3. 释放图结构体内存
    free(*graph);
    *graph = NULL;
    printf("\n有向图内存已释放\n");
}

// 测试：创建视频中的有向图示例（3个顶点，弧：0→1、1→0、1→2）
int main_directed() {
    // 1. 初始化3个顶点的有向图
    DirectedGraph* graph = dir_graph_init(3);
    if (graph == NULL) {
        return 1;
    }

    // 2. 添加弧（视频示例弧）
    dir_graph_add_arc(graph, 0, 1);
    dir_graph_add_arc(graph, 1, 0);
    dir_graph_add_arc(graph, 1, 2);
    dir_graph_add_arc(graph, 2, 2);  // 测试自环（应报错）
    dir_graph_add_arc(graph, 3, 0);  // 测试顶点越界（应报错）

    // 3. 打印邻接矩阵（应输出非对称矩阵）
    dir_graph_print(graph);

    // 4. 销毁图
    dir_graph_destroy(&graph);

    return 0;
}

// 运行有向图测试（若需运行无向图，注释此段并启用无向图main）
int main() {
    return main_directed();
}
```

## 3. 代码说明

1. **顶点集约束**：严格遵循 “顶点集非空”（`n ≥ 1`），初始化时校验，不符合则报错；
2. **简单图规则**：添加边 / 弧时禁止自环（`i ≠ j`），符合视频中 “简单图无自环” 的定义；
3. **邻接矩阵特性**：
	- 无向图：添加边后自动保持矩阵对称（`adj[i][j] = adj[j][i]`）；
	- 有向图：添加弧仅单向设置（`adj[i][j] = 1`），矩阵非对称；
4. **内存安全**：每个初始化函数对应销毁函数，避免内存泄漏，符合 C 语言编程规范；
5. **测试用例**：测试代码与视频中的示例完全一致（无向图 4 顶点、有向图 3 顶点），可直接运行验证结果。



# 图的邻接表实现

## 1. 邻接表核心结构

邻接表由两部分组成：

- **顶点数组**：存储所有顶点，每个顶点对应一个 “链表头指针”；
- **邻接链表**：每个顶点的链表存储其 “邻接顶点”（无向图中是直接相连的顶点，有向图中是弧的终点）。

### 1.1 通用节点结构（邻接链表的节点）

c

```c
#include <stdio.h>
#include <stdlib.h>

// 邻接链表的节点：存储邻接顶点的索引和下一个节点指针
typedef struct EdgeNode {
    int adj_vex;          // 邻接顶点的索引（对应顶点数组的下标）
    struct EdgeNode* next;// 指向下一个邻接节点的指针
} EdgeNode;

// 顶点结构：存储顶点信息（此处简化为仅存索引，实际可扩展存顶点数据）
typedef struct VexNode {
    EdgeNode* first_edge; // 指向当前顶点的邻接链表头节点
} VexNode;
```

## 2. 无向图的邻接表实现

### 2.1 核心逻辑

- 无向图的边是双向的，添加边`i-j`时，需在**i 的邻接链表中添加 j**，同时在**j 的邻接链表中添加 i**；
- 顶点的度 = 其邻接链表的长度（链表节点数）。

### 2.2 完整代码

c

```c
// 无向图邻接表结构体
typedef struct {
    VexNode* vexs; // 顶点数组（存储所有顶点）
    int n;         // 顶点数（n ≥ 1，有穷非空）
    int e;         // 边数（初始为0，添加边时自增）
} UndirGraphAdjList;

/**
 * @brief 初始化无向图邻接表
 * @param n 顶点数（需 ≥ 1）
 * @return 初始化成功的图指针，失败返回NULL
 */
UndirGraphAdjList* undir_adjlist_init(int n) {
    if (n < 1) {
        printf("错误：顶点数必须≥1（图的顶点集非空）\n");
        return NULL;
    }

    // 1. 分配图结构体内存
    UndirGraphAdjList* graph = (UndirGraphAdjList*)malloc(sizeof(UndirGraphAdjList));
    if (graph == NULL) {
        printf("错误：图结构体内存分配失败\n");
        return NULL;
    }
    graph->n = n;
    graph->e = 0; // 初始边数为0

    // 2. 分配顶点数组，初始化每个顶点的邻接链表头指针为NULL（无边）
    graph->vexs = (VexNode*)calloc(n, sizeof(VexNode));
    if (graph->vexs == NULL) {
        printf("错误：顶点数组内存分配失败\n");
        free(graph);
        return NULL;
    }
    for (int i = 0; i < n; i++) {
        graph->vexs[i].first_edge = NULL;
    }

    return graph;
}

/**
 * @brief 向无向图邻接表添加边（i与j之间）
 * @param graph 无向图指针
 * @param i 顶点i（0 ≤ i < n）
 * @param j 顶点j（0 ≤ j < n）
 */
void undir_adjlist_add_edge(UndirGraphAdjList* graph, int i, int j) {
    if (graph == NULL) {
        printf("错误：图未初始化\n");
        return;
    }
    // 校验顶点合法性（不越界、非自环、边已存在）
    if (i < 0 || i >= graph->n || j < 0 || j >= graph->n) {
        printf("错误：顶点%d或%d越界（合法范围：0~%d）\n", i, j, graph->n - 1);
        return;
    }
    if (i == j) {
        printf("错误：禁止自环（简单图无自环边）\n");
        return;
    }

    // --------------- 步骤1：在i的邻接链表中添加j ---------------
    EdgeNode* node1 = (EdgeNode*)malloc(sizeof(EdgeNode));
    if (node1 == NULL) {
        printf("错误：邻接节点内存分配失败\n");
        return;
    }
    node1->adj_vex = j;
    node1->next = graph->vexs[i].first_edge; // 头插法（效率高）
    graph->vexs[i].first_edge = node1;

    // --------------- 步骤2：在j的邻接链表中添加i（无向图双向性） ---------------
    EdgeNode* node2 = (EdgeNode*)malloc(sizeof(EdgeNode));
    if (node2 == NULL) {
        printf("错误：邻接节点内存分配失败\n");
        free(node1); // 释放已分配的node1，避免内存泄漏
        return;
    }
    node2->adj_vex = i;
    node2->next = graph->vexs[j].first_edge;
    graph->vexs[j].first_edge = node2;

    graph->e++; // 边数自增
    printf("成功添加边：%d ↔ %d（当前边数：%d）\n", i, j, graph->e);
}

/**
 * @brief 计算无向图顶点的度（邻接链表长度）
 * @param graph 无向图指针
 * @param vex 顶点索引（0 ≤ vex < n）
 * @return 顶点的度，失败返回-1
 */
int undir_adjlist_get_degree(UndirGraphAdjList* graph, int vex) {
    if (graph == NULL || vex < 0 || vex >= graph->n) {
        printf("错误：参数非法（图未初始化或顶点越界）\n");
        return -1;
    }

    int degree = 0;
    EdgeNode* p = graph->vexs[vex].first_edge;
    while (p != NULL) {
        degree++;
        p = p->next;
    }
    return degree;
}

/**
 * @brief 打印无向图邻接表
 * @param graph 无向图指针
 */
void undir_adjlist_print(UndirGraphAdjList* graph) {
    if (graph == NULL) {
        printf("错误：图未初始化\n");
        return;
    }

    printf("\n无向图邻接表（%d个顶点，%d条边）：\n", graph->n, graph->e);
    for (int i = 0; i < graph->n; i++) {
        printf("顶点%d（度：%d）：", i, undir_adjlist_get_degree(graph, i));
        EdgeNode* p = graph->vexs[i].first_edge;
        if (p == NULL) {
            printf("无邻接顶点\n");
            continue;
        }
        // 遍历邻接链表
        while (p != NULL) {
            printf("%d → ", p->adj_vex);
            p = p->next;
        }
        printf("NULL\n");
    }
}

/**
 * @brief 销毁无向图邻接表（释放内存，避免泄漏）
 * @param graph 无向图指针（销毁后置为NULL）
 */
void undir_adjlist_destroy(UndirGraphAdjList** graph) {
    if (graph == NULL || *graph == NULL) {
        return;
    }

    // 1. 释放每个顶点的邻接链表节点
    for (int i = 0; i < (*graph)->n; i++) {
        EdgeNode* p = (*graph)->vexs[i].first_edge;
        while (p != NULL) {
            EdgeNode* temp = p;
            p = p->next;
            free(temp); // 释放当前节点
        }
    }
    // 2. 释放顶点数组
    free((*graph)->vexs);
    // 3. 释放图结构体内存
    free(*graph);
    *graph = NULL;
    printf("\n无向图邻接表内存已释放\n");
}

// 测试：创建视频中的无向图示例（4个顶点，边：0-1、0-2、1-3）
int main_undir_adjlist() {
    // 1. 初始化4个顶点的无向图邻接表
    UndirGraphAdjList* graph = undir_adjlist_init(4);
    if (graph == NULL) {
        return 1;
    }

    // 2. 添加边（视频示例边）
    undir_adjlist_add_edge(graph, 0, 1);
    undir_adjlist_add_edge(graph, 0, 2);
    undir_adjlist_add_edge(graph, 1, 3);
    undir_adjlist_add_edge(graph, 3, 3); // 测试自环（应报错）
    undir_adjlist_add_edge(graph, 4, 0); // 测试顶点越界（应报错）

    // 3. 打印邻接表（验证邻接关系和度）
    undir_adjlist_print(graph);

    // 4. 销毁图
    undir_adjlist_destroy(&graph);

    return 0;
}

// 若需单独运行无向图邻接表测试，取消注释下方代码
// int main() {
//     return main_undir_adjlist();
// }
```

## 3. 有向图的邻接表实现

### 3.1 核心逻辑

- 有向图的弧是单向的，添加弧`i→j`时，仅需在**i 的邻接链表中添加 j**（无需在 j 的链表中添加 i）；
- 顶点的出度 = 其邻接链表的长度；顶点的入度 = 所有邻接链表中包含该顶点的节点数（需额外遍历统计）。

### 3.2 完整代码

c

```c
// 有向图邻接表结构体
typedef struct {
    VexNode* vexs; // 顶点数组
    int n;         // 顶点数（n ≥ 1）
    int e;         // 弧数（初始为0，添加弧时自增）
} DirGraphAdjList;

/**
 * @brief 初始化有向图邻接表
 * @param n 顶点数（需 ≥ 1）
 * @return 初始化成功的图指针，失败返回NULL
 */
DirGraphAdjList* dir_adjlist_init(int n) {
    if (n < 1) {
        printf("错误：顶点数必须≥1（图的顶点集非空）\n");
        return NULL;
    }

    // 1. 分配图结构体内存
    DirGraphAdjList* graph = (DirGraphAdjList*)malloc(sizeof(DirGraphAdjList));
    if (graph == NULL) {
        printf("错误：图结构体内存分配失败\n");
        return NULL;
    }
    graph->n = n;
    graph->e = 0; // 初始弧数为0

    // 2. 分配顶点数组，初始化邻接链表头指针为NULL
    graph->vexs = (VexNode*)calloc(n, sizeof(VexNode));
    if (graph->vexs == NULL) {
        printf("错误：顶点数组内存分配失败\n");
        free(graph);
        return NULL;
    }
    for (int i = 0; i < n; i++) {
        graph->vexs[i].first_edge = NULL;
    }

    return graph;
}

/**
 * @brief 向有向图邻接表添加弧（i→j）
 * @param graph 有向图指针
 * @param i 起点i（0 ≤ i < n）
 * @param j 终点j（0 ≤ j < n）
 */
void dir_adjlist_add_arc(DirGraphAdjList* graph, int i, int j) {
    if (graph == NULL) {
        printf("错误：图未初始化\n");
        return;
    }
    // 校验顶点合法性（不越界、非自环）
    if (i < 0 || i >= graph->n || j < 0 || j >= graph->n) {
        printf("错误：顶点%d或%d越界（合法范围：0~%d）\n", i, j, graph->n - 1);
        return;
    }
    if (i == j) {
        printf("错误：禁止自环（简单图无自环弧）\n");
        return;
    }

    // 仅在i的邻接链表中添加j（有向图单向性）
    EdgeNode* node = (EdgeNode*)malloc(sizeof(EdgeNode));
    if (node == NULL) {
        printf("错误：邻接节点内存分配失败\n");
        return;
    }
    node->adj_vex = j;
    node->next = graph->vexs[i].first_edge; // 头插法
    graph->vexs[i].first_edge = node;

    graph->e++; // 弧数自增
    printf("成功添加弧：%d → %d（当前弧数：%d）\n", i, j, graph->e);
}

/**
 * @brief 计算有向图顶点的出度（邻接链表长度）和入度（遍历统计）
 * @param graph 有向图指针
 * @param vex 顶点索引
 * @param out_degree 输出参数：顶点的出度
 * @param in_degree 输出参数：顶点的入度
 */
void dir_adjlist_get_degree(DirGraphAdjList* graph, int vex, int* out_degree, int* in_degree) {
    if (graph == NULL || vex < 0 || vex >= graph->n || out_degree == NULL || in_degree == NULL) {
        printf("错误：参数非法\n");
        *out_degree = -1;
        *in_degree = -1;
        return;
    }

    // 1. 计算出度（邻接链表长度）
    *out_degree = 0;
    EdgeNode* p = graph->vexs[vex].first_edge;
    while (p != NULL) {
        (*out_degree)++;
        p = p->next;
    }

    // 2. 计算入度（遍历所有顶点的邻接链表，统计包含vex的节点数）
    *in_degree = 0;
    for (int i = 0; i < graph->n; i++) {
        p = graph->vexs[i].first_edge;
        while (p != NULL) {
            if (p->adj_vex == vex) {
                (*in_degree)++;
            }
            p = p->next;
        }
    }
}

/**
 * @brief 打印有向图邻接表
 * @param graph 有向图指针
 */
void dir_adjlist_print(DirGraphAdjList* graph) {
    if (graph == NULL) {
        printf("错误：图未初始化\n");
        return;
    }

    printf("\n有向图邻接表（%d个顶点，%d条弧）：\n", graph->n, graph->e);
    for (int i = 0; i < graph->n; i++) {
        int out_deg, in_deg;
        dir_adjlist_get_degree(graph, i, &out_deg, &in_deg);
        printf("顶点%d（出度：%d，入度：%d）：", i, out_deg, in_deg);
        
        EdgeNode* p = graph->vexs[i].first_edge;
        if (p == NULL) {
            printf("无出弧\n");
            continue;
        }
        // 遍历邻接链表（出弧）
        while (p != NULL) {
            printf("%d → ", p->adj_vex);
            p = p->next;
        }
        printf("NULL\n");
    }
}

/**
 * @brief 销毁有向图邻接表（释放内存）
 * @param graph 有向图指针（销毁后置为NULL）
 */
void dir_adjlist_destroy(DirGraphAdjList** graph) {
    if (graph == NULL || *graph == NULL) {
        return;
    }

    // 1. 释放每个顶点的邻接链表节点
    for (int i = 0; i < (*graph)->n; i++) {
        EdgeNode* p = (*graph)->vexs[i].first_edge;
        while (p != NULL) {
            EdgeNode* temp = p;
            p = p->next;
            free(temp);
        }
    }
    // 2. 释放顶点数组
    free((*graph)->vexs);
    // 3. 释放图结构体内存
    free(*graph);
    *graph = NULL;
    printf("\n有向图邻接表内存已释放\n");
}

// 测试：创建视频中的有向图示例（3个顶点，弧：0→1、1→0、1→2）
int main_dir_adjlist() {
    // 1. 初始化3个顶点的有向图邻接表
    DirGraphAdjList* graph = dir_adjlist_init(3);
    if (graph == NULL) {
        return 1;
    }

    // 2. 添加弧（视频示例弧）
    dir_adjlist_add_arc(graph, 0, 1);
    dir_adjlist_add_arc(graph, 1, 0);
    dir_adjlist_add_arc(graph, 1, 2);
    dir_adjlist_add_arc(graph, 2, 2); // 测试自环（应报错）
    dir_adjlist_add_arc(graph, 3, 1); // 测试顶点越界（应报错）

    // 3. 打印邻接表（验证出度、入度和弧关系）
    dir_adjlist_print(graph);

    // 4. 销毁图
    dir_adjlist_destroy(&graph);

    return 0;
}

// 运行有向图邻接表测试（若需运行无向图，注释此段并启用无向图main）
int main() {
    return main_dir_adjlist();
}
```

## 4. 邻接表与邻接矩阵对比

| 对比维度    | 邻接表（Adjacency List）                                     | 邻接矩阵（Adjacency Matrix）                                 |
| ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 空间复杂度  | O (n+E)（适合稀疏图，E 远小于 n²）                           | O (n²)（适合稠密图，E 接近 n²）                              |
| 查询边 / 弧 | O (degree (v))（需遍历 v 的邻接链表）                        | O (1)（直接访问 adj [i][j]）                                 |
| 添加边 / 弧 | O (1)（头插法，无需遍历）                                    | O (1)（直接赋值 adj [i][j]）                                 |
| 计算顶点度  | 无向图：O (1)（链表长度）； 有向图入度：O (n+E)（遍历所有链表） | 无向图：O (n)（遍历行）； 有向图：O (n)（入度遍历列，出度遍历行） |
| 适用场景    | 稀疏图（如社交网络、路由表）                                 | 稠密图（如城市交通网、全连接网络）                           |

## 5. 代码说明

1. **结构一致性**：邻接表的 “顶点数组 + 链表” 结构完全遵循视频定义，无向图双向添加边、有向图单向添加弧，符合视频中 “简单图” 的约束；
2. **功能完整性**：包含初始化、添加边 / 弧、计算度、打印、销毁等全流程函数，解决视频中提到的 “图存储需兼顾效率与内存” 问题；
3. **测试匹配性**：测试用例与之前邻接矩阵的示例完全一致（无向图 4 顶点、有向图 3 顶点），便于对比两种存储方式的差异；
4. **内存安全**：每个动态分配的内存（顶点数组、链表节点）均在销毁函数中释放，避免内存泄漏，符合 C 语言编程规范。



#  十字链表的核心定位与结构

十字链表的核心是 “用一个弧节点存储一条有向弧，同时关联弧的起点（尾顶点）和终点（头顶点）”，形成 “出边链表 + 入边链表” 交叉的结构（形似 “十字”）。分为两类节点：**顶点节点**和**弧节点**。

## 1. 1顶点节点（VexNode）—— 边链表的 “表头”

每个顶点对应一个顶点节点，存储顶点信息及 “出边 / 入边链表” 的入口指针，所有顶点节点构成**顶点数组**（顺序存储，便于通过下标快速定位）。

| 字段名     | 数据类型               | 作用说明（结合示例）                                         |
| ---------- | ---------------------- | ------------------------------------------------------------ |
| `data`     | 自定义类型（如 int）   | 顶点数据（如顶点编号 V0、V1，示例中简化为顶点下标 0、1、2）  |
| `firstout` | 弧节点指针（ArcNode*） | 出边表头：指向以当前顶点为**尾顶点**（弧的起点）的第一条弧（出边链表的头） |
| `firstin`  | 弧节点指针（ArcNode*） | 入边表头：指向以当前顶点为**头顶点**（弧的终点）的第一条弧（入边链表的头） |

**示例**：顶点 1（V1）的`firstout`指向弧 V1→V2 的弧节点，`firstin`指向弧 V0→V1 的弧节点。

## 1.2弧节点（ArcNode）—— 存储有向弧的 “核心”

每条有向弧（如 V0→V1）对应一个弧节点，通过指针串联同起点的出边和同终点的入边。

| 字段名    | 数据类型     | 作用说明（结合示例）                                         |
| --------- | ------------ | ------------------------------------------------------------ |
| `tailvex` | int          | 弧的尾顶点下标（弧的起点，如弧 V0→V1 的`tailvex=0`）         |
| `headvex` | int          | 弧的头顶点下标（弧的终点，如弧 V0→V1 的`headvex=1`）         |
| `tlink`   | ArcNode*     | 出边指针：指向**同一尾顶点**的下一条弧（如 V0→V1 的`tlink`指向 V0→V2 的弧节点） |
| `hlink`   | ArcNode*     | 入边指针：指向**同一头顶点**的下一条弧（如 V0→V1 的`hlink`指向 V2→V1 的弧节点） |
| `info`    | 指针（可选） | 弧的附加信息（如权重、长度，示例中简化省略）                 |

**核心逻辑**：一个弧节点同时属于两个链表 —— 通过`tlink`加入 “尾顶点的出边链表”，通过`hlink`加入 “头顶点的入边链表”。



##  二、示例构建：用具体有向图理解十字链表

为了直观理解，我们构建一个**4 顶点有向图**，边集为：`E = {V0→V1, V0→V2, V1→V2, V2→V1, V2→V3}`，顶点编号 0（V0）、1（V1）、2（V2）、3（V3），结构如下：

plaintext

```plaintext
V0 → V1 ← V2
|    ↑    |
↓    |    ↓
V2 → V2    V3
（注：V1→V2 和 V2→V1 是两条反向弧）
```

## 步骤 1：定义顶点数组与弧节点

首先创建 4 个顶点节点（构成顶点数组`vex[4]`），5 个弧节点（对应 5 条边，记为 A~E）：

- 弧 A：V0→V1（tailvex=0，headvex=1）
- 弧 B：V0→V2（tailvex=0，headvex=2）
- 弧 C：V1→V2（tailvex=1，headvex=2）
- 弧 D：V2→V1（tailvex=2，headvex=1）
- 弧 E：V2→V3（tailvex=2，headvex=3）

## 步骤 2：填充弧节点的指针（关键关联）

按 “入边 / 出边链表头插法”（视频常用方式，O (1) 插入）关联指针：

| 弧节点 | tailvex | headvex | tlink（同尾顶点下一条出边）  | hlink（同头顶点下一条入边）    | 对应边 |
| ------ | ------- | ------- | ---------------------------- | ------------------------------ | ------ |
| A      | 0       | 1       | B（V0 的下一条出边是 V0→V2） | D（V1 的下一条入边是 V2→V1）   | V0→V1  |
| B      | 0       | 2       | NULL（V0 无更多出边）        | C（V2 的下一条入边是 V1→V2）   | V0→V2  |
| C      | 1       | 2       | NULL（V1 无更多出边）        | NULL（V2 的入边链表到 C 结束） | V1→V2  |
| D      | 2       | 1       | E（V2 的下一条出边是 V2→V3） | NULL（V1 的入边链表到 D 结束） | V2→V1  |
| E      | 2       | 3       | NULL（V2 无更多出边）        | NULL（V3 无更多入边）          | V2→V3  |

## 步骤 3：填充顶点数组的表头指针

每个顶点的`firstin`和`firstout`指向对应边链表的头节点：

| 顶点下标 | data | firstout（出边表头）         | firstin（入边表头）          | 对应顶点 |
| -------- | ---- | ---------------------------- | ---------------------------- | -------- |
| 0        | V0   | A（V0 的第一条出边是 V0→V1） | NULL（V0 无入边）            | V0       |
| 1        | V1   | C（V1 的第一条出边是 V1→V2） | A（V1 的第一条入边是 V0→V1） | V1       |
| 2        | V2   | D（V2 的第一条出边是 V2→V1） | B（V2 的第一条入边是 V0→V2） | V2       |
| 3        | V3   | NULL（V3 无出边）            | E（V3 的第一条入边是 V2→V3） | V3       |

## 步骤 4：可视化十字链表结构

plaintext

```plaintext
顶点数组：
vex[0]（V0）：firstout→A（V0→V1） → tlink→B（V0→V2）→NULL
              firstin→NULL
              
vex[1]（V1）：firstout→C（V1→V2）→NULL
              firstin→A（V0→V1） → hlink→D（V2→V1）→NULL
              
vex[2]（V2）：firstout→D（V2→V1） → tlink→E（V2→V3）→NULL
              firstin→B（V0→V2） → hlink→C（V1→V2）→NULL
              
vex[3]（V3）：firstout→NULL
              firstin→E（V2→V3）→NULL
```

**关键观察**：

- V0 的出边链表：A→B（通过`tlink`串联），直接通过`vex[0].firstout`访问；
- V1 的入边链表：A→D（通过`hlink`串联），直接通过`vex[1].firstin`访问；
- 弧 A（V0→V1）同时在 V0 的出边链表和 V1 的入边链表中，无需重复存储。



## 三、十字链表的核心操作（结合示例步骤）

视频中重点讲解 “初始化”“添加弧”“遍历入边 / 出边”，以下结合上述示例说明。

## 3.1 初始化

**目标**：创建顶点数组，初始化表头指针为 NULL。
**步骤**：

1. 输入顶点数 n（示例中 n=4），分配顶点数组`vex[n]`的内存；
2. 对每个顶点`vex[i]`：`data=i`（顶点编号），`firstin=NULL`，`firstout=NULL`；
3. 弧节点暂未创建，初始为空。

## 3.2添加弧（以添加 “V2→V1” 为例，即弧 D）

**核心逻辑**：新弧需同时插入 “尾顶点的出边链表” 和 “头顶点的入边链表”（头插法）。
**步骤**：

1. 分配弧节点 D，设置`tailvex=2`（V2）、`headvex=1`（V1）、`tlink=NULL`、`hlink=NULL`；
2. **插入 V2 的出边链表**（尾顶点是 2）：
	- 新弧的`tlink` = `vex[2].firstout`（此时`vex[2].firstout`为 NULL）；
	- 更新`vex[2].firstout` = 弧 D（V2 的出边表头变为 D）；
3. **插入 V1 的入边链表**（头顶点是 1）：
	- 新弧的`hlink` = `vex[1].firstin`（此时`vex[1].firstin`是弧 A）；
	- 更新`vex[1].firstin` = 弧 D（V1 的入边表头变为 D？不，示例中 V1 的入边表头是 A，因为 A 先添加，头插法后添加的 D 会成为新表头？哦，示例中 A 先添加，D 后添加，所以步骤应为：
		- 弧 D 的`hlink` = `vex[1].firstin`（即 A）；
		- `vex[1].firstin` = D，此时 V1 的入边链表是 D→A，与示例中 “V1 的入边链表 A→D” 不符，说明示例用的是尾插法？这里需统一：视频中常用头插法，若要示例中 A→D 的顺序，需用尾插法，核心是 “指针关联逻辑”，而非插入顺序）；
4. 添加完成，弧 D 同时属于 V2 的出边链表和 V1 的入边链表。



## 3. 3遍历操作（示例：遍历 V2 的出边和 V1 的入边）

### （1）遍历 V2 的出边（所有以 V2 为起点的弧）

**目标**：输出 V2 的所有出边（V2→V1、V2→V3）。
**步骤**：

1. 从`vex[2].firstout`获取出边表头（示例中是弧 D）；
2. 循环遍历：
	- 当前弧节点 D：输出 “V2→V1”（`tailvex=2`→`headvex=1`）；
	- 移动到下一条出边：`p = p->tlink`（D 的`tlink`是弧 E）；
	- 当前弧节点 E：输出 “V2→V3”；
	- `p = p->tlink`（E 的`tlink`是 NULL），循环结束。
		**结果**：V2→V1，V2→V3。

### （2）遍历 V1 的入边（所有以 V1 为终点的弧）

**目标**：输出 V1 的所有入边（V0→V1、V2→V1）。
**步骤**：

1. 从`vex[1].firstin`获取入边表头（示例中是弧 A）；

2. 循环遍历：

	- 当前弧节点 A：输出 “V0→V1”（`tailvex=0`→`headvex=1`）；
	- 移动到下一条入边：`p = p->hlink`（A 的`hlink`是弧 D）；
	- 当前弧节点 D：输出 “V2→V1”；
	- `p = p->hlink`（D 的`hlink`是 NULL），循环结束。
		**结果**：V0→V1，V2→V1。

	

##  4.1十字链表的核心优势（对比普通邻接表）

以 “统计 V1 的入度” 为例（V1 的入边有 2 条：V0→V1、V2→V1），对比效率：

| 对比维度             | 普通邻接表（有向图）                                         | 十字链表（有向图）                                           |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 统计 V1 入度步骤     | 1. 遍历所有顶点（0、1、2、3）的出边链表； 2. 逐个判断弧的终点是否为 1； 3. 计数（共 2 次匹配） | 1. 从`vex[1].firstin`出发，遍历入边链表； 2. 每遍历一个弧节点，计数 + 1； 3. 遍历结束，计数 = 2 |
| 时间复杂度           | O (n+E)（n=4，E=5，需遍历 9 个节点）                         | O (k)（k = 入边数，仅需遍历 2 个节点）                       |
| 边操作（删除 V0→V1） | 需找到 V0 出边链表中的 “V0→V1” 节点，无入边关联，易遗漏      | 仅需找到弧 A，同时处理 V0 出边链表和 V1 入边链表的指针，无遗漏 |
| 空间复杂度           | O (n+E)（存储 5 条弧，无重复）                               | O (n+E)（同普通邻接表，仅多 2 个指针 / 弧）                  |

## 5.1应用场景

十字链表仅适用于**有向图**，核心场景均依赖 “高效的入边访问”：

1. **拓扑排序**：需频繁找 “入度为 0 的顶点”，通过十字链表的入边链表可快速统计入度，无需遍历全图；
2. **关键路径**：需同时处理 “前驱活动”（入边）和 “后继活动”（出边），十字链表可高效获取两类边；
3. **有向图的强连通分量分析**：需遍历顶点的入边和出边，避免普通邻接表的低效遍历。

##  5.1总结

十字链表的核心是 “用双向指针关联入边 / 出边，实现边的高效双向访问”，结合示例可清晰看到：

1. 结构上：顶点数组 + 弧节点，通过`tlink`/`hlink`形成十字关联；
2. 操作上：添加弧需维护两个链表，遍历入边 / 出边仅需从表头出发；
3. 优势上：入边处理效率远超普通邻接表，是有向图复杂操作的 “基础结构”。



#  邻接多重表基本定义与核心定位

邻接多重表（Adjacency Multilist）本质是**无向图的 “边中心” 存储结构**：

- 无向图中一条边（Vi, Vj）在邻接表中会被存储两次（Vi 的邻接表存 Vj，Vj 的邻接表存 Vi），导致边操作（如删除、标记）需处理两个结点，效率低且易出错。
- 邻接多重表通过**一条边仅存储一个边结点**，并通过指针关联边的两个顶点，实现 “边的唯一存储” 和 “顶点与边的高效关联”。

邻接多重表由两类结点构成：**顶点结点**和**边结点**，结构如下：

### 1. 顶点结点（存储顶点信息）

每个顶点对应一个顶点结点，所有顶点结点构成一个**数组**（便于通过顶点下标快速访问）。

| 字段名      | 数据类型                     | 作用说明                                                     |
| ----------- | ---------------------------- | ------------------------------------------------------------ |
| `data`      | 顶点数据类型（如 int、char） | 存储顶点本身的信息（如顶点编号、名称）                       |
| `firstedge` | 边结点指针                   | 指向与当前顶点**关联的第一条边结点**（作为顶点访问边的入口） |

**示例**：顶点 V1 的`firstedge`指向边（V1,V2）对应的边结点。

### 2. 边结点（存储边信息）

每条无向边对应一个边结点，是邻接多重表的核心，通过指针串联同顶点的所有边。

| 字段名  | 数据类型             | 作用说明                                                     |
| ------- | -------------------- | ------------------------------------------------------------ |
| `mark`  | 布尔型 / 整型（0/1） | 标记边是否被访问过（用于遍历，避免重复处理，0 = 未访问，1 = 已访问） |
| `ivex`  | 整型                 | 边的两个顶点在 “顶点数组” 中的下标（如 Vi 的下标为 i）       |
| `jvex`  | 整型                 | 边的另一个顶点在 “顶点数组” 中的下标（如 Vj 的下标为 j）     |
| `ilink` | 边结点指针           | 指向与**ivex（Vi）关联的下一条边结点**（同顶点 Vi 的其他边） |
| `jlink` | 边结点指针           | 指向与**jvex（Vj）关联的下一条边结点**（同顶点 Vj 的其他边） |
| `info`  | 指针（可选）         | 指向边的附加信息（如边的权重、长度，无附加信息时可省略）     |

**核心逻辑**：一条边（Vi,Vj）的边结点，通过`ilink`串联 Vi 的所有边，通过`jlink`串联 Vj 的所有边，实现 “一个边结点关联两个顶点的边链”。

## 三、核心特点（对比邻接表）

| 对比维度     | 邻接表（无向图）                        | 邻接多重表（无向图）             |
| ------------ | --------------------------------------- | -------------------------------- |
| 边的存储次数 | 1 条边存 2 个边结点（Vi 和 Vj 各 1 个） | 1 条边存 1 个边结点（唯一存储）  |
| 边操作效率   | 删除 / 标记边需找 2 个结点，易遗漏      | 仅操作 1 个结点，效率高、无遗漏  |
| 空间复杂度   | O (n+2e)（n = 顶点数，e = 边数）        | O (n+e)（更节省空间）            |
| 适用场景     | 无向图边操作少，或有向图                | 无向图边操作频繁（如遍历、删除） |

## 四、实例解析（以无向图为例）

### 步骤 1：确定无向图的顶点与边

假设无向图 G 有 4 个顶点（V0, V1, V2, V3）和 4 条边，边的集合为：
`E = {(V0,V1), (V0,V2), (V1,V2), (V2,V3)}`
图的结构如下：

plaintext

```plaintext
V0 ─── V1
│      │
│      │
V2 ─── V3
```

### 步骤 2：构建邻接多重表的顶点数组

顶点数组大小为 4（对应 4 个顶点），每个顶点的`firstedge`指向其关联的第一条边结点：

| 顶点下标 | data | firstedge（指向的边结点） |
| -------- | ---- | ------------------------- |
| 0        | V0   | 边 e0（V0,V1）的边结点    |
| 1        | V1   | 边 e0（V0,V1）的边结点    |
| 2        | V2   | 边 e1（V0,V2）的边结点    |
| 3        | V3   | 边 e3（V2,V3）的边结点    |

### 步骤 3：构建边结点及指针链接

4 条边对应 4 个边结点（e0~e3），各边结点的字段值及指针关联如下表（`info`省略，`mark`初始为 0）：

| 边结点 | mark | ivex | jvex | ilink（指向 ivex 的下一条边） | jlink（指向 jvex 的下一条边） | 对应实际边 |
| ------ | ---- | ---- | ---- | ----------------------------- | ----------------------------- | ---------- |
| e0     | 0    | 0    | 1    | e1（V0 的下一条边：V0,V2）    | e2（V1 的下一条边：V1,V2）    | (V0,V1)    |
| e1     | 0    | 0    | 2    | NULL（V0 无更多边）           | e2（V2 的下一条边：V1,V2）    | (V0,V2)    |
| e2     | 0    | 1    | 2    | NULL（V1 无更多边）           | e3（V2 的下一条边：V2,V3）    | (V1,V2)    |
| e3     | 0    | 2    | 3    | NULL（V2 无更多边）           | NULL（V3 无更多边）           | (V2,V3)    |

### 步骤 4：可视化邻接多重表结构

plaintext

```plaintext
顶点数组：
[V0] → firstedge → e0（ivex=0,jvex=1）
                    ↓ ilink        ↓ jlink
                    e1（V0,V2）    e2（V1,V2）
                     ↓ ilink        ↓ ilink    ↓ jlink
                     NULL          NULL       e3（V2,V3）
                                              ↓ ilink    ↓ jlink
                                              NULL      NULL
[V1] → firstedge → e0（同上面的e0）
[V2] → firstedge → e1（同上面的e1）
[V3] → firstedge → e3（同上面的e3）
```

**关键理解**：

- 顶点 V0 的边链：e0（V0,V1）→ e1（V0,V2）（通过 e0 的`ilink`关联）；
- 顶点 V2 的边链：e1（V0,V2）→ e2（V1,V2）→ e3（V2,V3）（通过 e1 的`jlink`→e2，e2 的`jlink`→e3）；
- 一条边（如 e0）同时属于 V0 和 V1 的边链，无需重复存储。

## 五、邻接多重表的基本操作（结合示例）

以 “边遍历” 和 “创建邻接多重表” 为例，说明核心操作逻辑。

### 1. 边遍历（避免重复访问）

目标：遍历无向图的所有边，且每条边仅访问 1 次（依赖`mark`标记）。
**遍历步骤**：

1. 初始化所有边结点的`mark=0`；
2. 遍历顶点数组（从 V0 到 V3）；
3. 对每个顶点 Vi，通过`firstedge`找到其关联的第一条边结点 e；
4. 若 e 的`mark=0`：
	- 访问边 e（如输出边 (Vi, Vj)）；
	- 标记 e 的`mark=1`（避免重复访问）；
	- 通过 e 的`ilink`（若当前顶点是 ivex）或`jlink`（若当前顶点是 jvex），访问 Vi 的下一条边；
5. 重复步骤 3-4，直到所有顶点的边链遍历完毕。

**示例遍历过程**：

- 遍历 V0：`firstedge→e0（mark=0）`→访问 e0（V0,V1）→mark=1→e0.ilink→e1（mark=0）→访问 e1（V0,V2）→mark=1→e1.ilink→NULL；
- 遍历 V1：`firstedge→e0（mark=1，跳过）`→e0.jlink→e2（mark=0）→访问 e2（V1,V2）→mark=1→e2.ilink→NULL；
- 遍历 V2：`firstedge→e1（mark=1，跳过）`→e1.jlink→e2（mark=1，跳过）→e2.jlink→e3（mark=0）→访问 e3（V2,V3）→mark=1→e3.ilink→NULL；
- 遍历 V3：`firstedge→e3（mark=1，跳过）`；
- 最终遍历结果：(V0,V1) → (V0,V2) → (V1,V2) → (V2,V3)（无重复）。

### 2. 创建邻接多重表（核心步骤）

输入：顶点数 n、边数 e，以及每条边的两个顶点（如 (V0,V1)）。
**创建步骤**：

1. 初始化顶点数组：创建 n 个顶点结点，`data`赋值为顶点编号，`firstedge=NULL`；
2. 循环处理每条边（共 e 条）：
	a. 输入边的两个顶点下标 i 和 j（如 V0 的下标 0，V1 的下标 1）；
	b. 分配一个边结点 e，初始化：`mark=0`，`ivex=i`，`jvex=j`，`ilink=NULL`，`jlink=NULL`；
	c. 将 e 插入到 Vi 的边链头部：e.ilink = 顶点 i 的 firstedge → 顶点 i 的 firstedge = e；
	d. 将 e 插入到 Vj 的边链头部：e.jlink = 顶点 j 的 firstedge → 顶点 j 的 firstedge = e；
3. 所有边处理完毕，邻接多重表创建完成。

**示例创建 e0（V0,V1）**：

- 顶点 0 的`firstedge`原本为 NULL → e0.ilink = NULL → 顶点 0.firstedge = e0；
- 顶点 1 的`firstedge`原本为 NULL → e0.jlink = NULL → 顶点 1.firstedge = e0；
- 后续创建 e1（V0,V2）时，e1.ilink = 顶点 0.firstedge（即 e0）→ 顶点 0.firstedge = e1，实现 V0 的边链 “e1→e0”。

## 六、应用场景

邻接多重表仅适用于**无向图**，尤其适合以下场景：

1. 需频繁操作边的场景（如边的删除、标记、权重修改），避免邻接表的重复操作；
2. 无向图的遍历（如边遍历、连通分量分析），确保每条边仅处理一次；
3. 稀疏无向图（边数 e 远小于 n²），空间优势更明显（O (n+e) vs 邻接矩阵 O (n²)）。

## 总结

邻接多重表是无向图的 “优化存储方案”，通过 “边的唯一存储” 和 “双向指针关联”，解决了邻接表的冗余问题，核心价值在于**高效的边操作和空间节省**。理解其顶点结点与边结点的结构、指针关联逻辑，是掌握该结构的关键（结合实例可快速突破）。



# 图算法核心知识点总结

## 一、图的基础表示 —— 邻接矩阵

视频中采用**邻接矩阵**存储无向图，核心是用结构体封装顶点、边的信息，以及辅助数组记录顶点访问状态。

### 1. 核心定义

- **顶点（Vertex）**：用字符型（`char`）表示，如视频中的 `A, B, C, ..., I`。
- **边（Edge）**：用整型（`int`）表示，无向图中 `arc[i][j] = 1` 表示顶点 `i` 与 `j` 相连，`0` 表示不相连（初始化时全为 0）。
- **邻接矩阵结构体**：包含顶点数组、边的二维数组、顶点数、边数。
- **访问标记数组（visited）**：整型数组，`visited[i] = 1` 表示顶点 `i` 已访问，`0` 表示未访问（避免重复遍历）。

### 2. 数据结构代码实现

c

```c
#include <stdio.h>
#include <stdlib.h>

// 重定义数据类型
typedef char VertexType;  // 顶点数据类型（字符型）
typedef int EdgeType;     // 边数据类型（整型）
#define MAXSIZE 100       // 最大顶点数（视频中实际用9个顶点：A-I）

// 邻接矩阵结构体（图的存储）
typedef struct {
    VertexType vertex[MAXSIZE];  // 存储顶点的数组
    EdgeType arc[MAXSIZE][MAXSIZE];  // 存储边的二维数组（邻接矩阵）
    int vertexNum;  // 图的顶点总数
    int edgeNum;    // 图的边总数
} MatGraph;

// 访问标记数组（全局或作为参数传入，视频中用全局）
int visited[MAXSIZE];
```

### 3. 图的创建（createGraph 函数）

视频中创建的图包含 **9 个顶点（A-I）、15 条边**，步骤：

1. 初始化顶点数组（存入 `A, B, C, ..., I`）；
2. 初始化邻接矩阵（所有元素设为 0）；
3. 赋值有边的位置（`arc[i][j] = 1`），并利用无向图对称性赋值 `arc[j][i] = 1`。

#### 代码实现

c

```c
// 创建无向图（基于视频中的图：A-I顶点，15条边）
void createGraph(MatGraph *G) {
    // 1. 初始化顶点数和边数（视频中9个顶点，15条边）
    G->vertexNum = 9;
    G->edgeNum = 15;

    // 2. 初始化顶点数组（下标0对应A，1对应B，...，8对应I）
    G->vertex[0] = 'A';
    G->vertex[1] = 'B';
    G->vertex[2] = 'C';
    G->vertex[3] = 'D';
    G->vertex[4] = 'E';
    G->vertex[5] = 'F';
    G->vertex[6] = 'G';
    G->vertex[7] = 'H';
    G->vertex[8] = 'I';

    // 3. 初始化邻接矩阵（所有边先设为0）
    for (int i = 0; i < G->vertexNum; i++) {
        for (int j = 0; j < G->vertexNum; j++) {
            G->arc[i][j] = 0;
        }
    }

    // 4. 赋值有边的位置（视频中明确的边：i-j对应顶点下标）
    // 边列表（无向图，对称）：0-1(A-B), 0-5(A-F), 1-2(B-C), 1-6(B-G), 1-8(B-I),
    // 2-3(C-D), 2-8(C-I), 3-4(D-E), 3-6(D-G), 3-7(D-H), 3-8(D-I),
    // 4-5(E-F), 4-7(E-H), 5-6(F-G), 6-7(G-H)
    G->arc[0][1] = 1; G->arc[1][0] = 1;
    G->arc[0][5] = 1; G->arc[5][0] = 1;
    G->arc[1][2] = 1; G->arc[2][1] = 1;
    G->arc[1][6] = 1; G->arc[6][1] = 1;
    G->arc[1][8] = 1; G->arc[8][1] = 1;
    G->arc[2][3] = 1; G->arc[3][2] = 1;
    G->arc[2][8] = 1; G->arc[8][2] = 1;
    G->arc[3][4] = 1; G->arc[4][3] = 1;
    G->arc[3][6] = 1; G->arc[6][3] = 1;
    G->arc[3][7] = 1; G->arc[7][3] = 1;
    G->arc[3][8] = 1; G->arc[8][3] = 1;
    G->arc[4][5] = 1; G->arc[5][4] = 1;
    G->arc[4][7] = 1; G->arc[7][4] = 1;
    G->arc[5][6] = 1; G->arc[6][5] = 1;
    G->arc[6][7] = 1; G->arc[7][6] = 1;
    
   
}
```

### 4. 示例：邻接矩阵片段（A-F 部分）

| 顶点下标 | 0(A) | 1(B) | 2(C) | 3(D) | 4(E) | 5(F) | ...  |
| -------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0(A)     | 0    | 1    | 0    | 0    | 0    | 1    | ...  |
| 1(B)     | 1    | 0    | 1    | 0    | 0    | 0    | ...  |
| 2(C)     | 0    | 1    | 0    | 1    | 0    | 0    | ...  |
| 5(F)     | 1    | 0    | 0    | 0    | 1    | 0    | ...  |

## 二、深度优先搜索（DFS）

视频中 DFS 是图的核心遍历算法，**类似树的前序遍历**，核心思想是 “先深后广”—— 从起始顶点出发，优先遍历当前顶点的邻接顶点，直到无法深入再回溯。

### 1. 核心思想

1. 访问起始顶点，标记为 “已访问”（`visited[i] = 1`）；
2. 遍历当前顶点的所有邻接顶点，若未访问，则递归调用 DFS 遍历该邻接顶点；
3. 回溯：当当前顶点的所有邻接顶点均已访问，返回上一层。

### 2. 代码实现（递归版）

c

```c
// 深度优先搜索（DFS）：从顶点i开始遍历
void DFS(MatGraph G, int i) {
    // 1. 访问当前顶点（输出），标记为已访问
    printf("%c ", G.vertex[i]);
    visited[i] = 1;

    // 2. 遍历当前顶点的所有邻接顶点
    for (int j = 0; j < G.vertexNum; j++) {
        // 若顶点j与i相连（arc[i][j]=1）且未访问（visited[j]=0），递归DFS
        if (G.arc[i][j] == 1 && visited[j] == 0) {
            DFS(G, j);
        }
    }
}

// 主函数调用示例（视频中从A顶点（下标0）开始）
int main() {
    MatGraph G;
    createGraph(&G);

    // 初始化访问标记数组（全为0，未访问）
    for (int i = 0; i < G.vertexNum; i++) {
        visited[i] = 0;
    }

    // 从顶点A（下标0）开始DFS遍历
    dfs(G, 0);
    return 0;
}
```

### 3. 示例：DFS 遍历过程（视频中的图）

- **起始顶点**：A（下标 0）
- **遍历顺序**：A → B → C → D → E → F → G → H → I（视频中基于 “优先左邻接” 的逻辑）
- **visited 数组变化**：
	1. 访问 A：`visited[0] = 1`；
	2. 递归访问 A 的邻接 B（下标 1）：`visited[1] = 1`；
	3. 递归访问 B 的邻接 C（下标 2）：`visited[2] = 1`；
	4. 递归访问 C 的邻接 D（下标 3）：`visited[3] = 1`；
	5. 递归访问 D 的邻接 E（下标 4）：`visited[4] = 1`；
	6. 递归访问 E 的邻接 F（下标 5）：`visited[5] = 1`；
	7. 递归访问 F 的邻接 G（下标 6）：`visited[6] = 1`；
	8. 递归访问 G 的邻接 H（下标 7）：`visited[7] = 1`；
	9. H 无未访问邻接顶点，回溯，最终遍历 I（下标 8）：`visited[8] = 1`。

### 4. 应用场景（视频提及）

- 游戏中的 “目标寻址”（如迷宫寻路，优先深入一条路径）。

## 三、广度优先搜索（BFS）

视频中 BFS 是图的另一种核心遍历算法，**类似树的层序遍历**，核心思想是 “先广后深”—— 从起始顶点出发，先遍历当前顶点的所有邻接顶点（同一层），再依次遍历邻接顶点的邻接顶点。

### 1. 核心思想

1. 用**队列**（视频中用数组模拟）存储待遍历的顶点；
2. 访问起始顶点，标记为 “已访问”，入队；
3. 出队一个顶点，遍历其所有邻接顶点，若未访问，则标记、入队；
4. 重复步骤 3，直到队列为空。

### 2. 代码实现（数组模拟队列）

c

```c
// 广度优先搜索（BFS）：从顶点i开始遍历
void BFS(MatGraph G, ) 
{
    // 数组模拟队列：Q存储顶点下标，front=队头，rear=队尾（指向队尾下一个位置）
    int q[MAXSIZE], front = 0, rear = 0;

    // 1. 访问起始顶点，标记为已访问，入队
    int i 
    printf("%c ", G.vertex[i]);
    visited[i] = 1;
    Q[rear] = i;  // 入队
       rear++;
    // 2. 队列不为空时，循环出队、入队
    while (front != rear) {
        // 出队
        i = queue[front];
        front ++;
        // 遍历当前顶点的所有邻接顶点
        for (int j = 0; j < G.vertexNum; j++) {
            // 若顶点j与current相连且未访问，标记、入队
            if (G.arcc[i][j] == 1 && visited[j] == 0) {
                visited[j] = 1;
                printf("%c\n"),G.vertex[j]
                Q[rear] = j;  // 入队
                rear++
            }
        }
    }
}

// 主函数调用示例（从A顶点（下标0）开始）
int main() {
     Mat_Graph G;
 create_graph(&G);

 // 初始化访问标记数组（全为0）
 for (int i = 0; i < G.vertex_num; i++) {
     visited[i] = 0;
 }

 // 从顶点A（下标0）开始BFS遍历

 BFS(G);

    return 0;
}
```

### 3. 示例：BFS 遍历过程（视频中的图）

- **起始顶点**：A（下标 0）
- **队列操作与遍历顺序**：
	1. 访问 A，标记，入队：`Q = [0]`，输出：A；
	2. 出队 0（A），遍历邻接 1（B）、5（F）：标记 B、F，入队：`Q = [1,5]`，输出：B F；
	3. 出队 1（B），遍历邻接 2（C）、6（G）、8（I）：标记 C、G、I，入队：`Q = [5,2,6,8]`，输出：C G I；
	4. 出队 5（F），遍历邻接 4（E）：标记 E，入队：`Q = [2,6,8,4]`，输出：E；
	5. 出队 2（C），遍历邻接 3（D）：标记 D，入队：`Q = [6,8,4,3]`，输出：D；
	6. 出队 6（G），遍历邻接 7（H）：标记 H，入队：`Q = [8,4,3,7]`，输出：H；
	7. 后续出队 8（I）、4（E）、3（D）、7（H），无未访问邻接顶点，队列空，遍历结束。
- **最终遍历顺序**：A → B → F → C → G → I → E → D → H（视频中层序逻辑）。

### 4. 应用场景（视频提及）

- 游戏中的 “最短路径搜索”（如网格地图中找最近目标，层序遍历保证最短）。





## 四、最小生成树（MST）与普里姆算法（Prime）

## 一、最小生成树基础概念

### 1. 定义

在**带权连通图**中，找到包含所有顶点的**极小连通子图**，满足：

- 包含图中所有 *n* 个顶点；
- 仅用 *n*−1 条边连接所有顶点；
- 所有边的**总权值最小**。

### 2. 核心特性

- **无环性**：添加任意一条非 MST 的边，会形成环路（冗余边，增加总权值）；
- **连通性**：MST 中任意两顶点间有且仅有一条路径；
- **唯一性**：若所有边权值唯一，MST 结构唯一；若存在相同权值边，可能有多个 MST，但总权值相同。

### 3. 应用场景

- 基础设施建设（如村庄修路、城市铺电缆）：顶点 = 地点，边权 = 建设成本，需最小成本连通所有地点；
- 网络拓扑设计：顶点 = 网络节点，边权 = 传输成本，需最小成本构建连通网络。

## 二、普里姆（Prim）算法

### 1. 算法原理（贪心策略）

从**一个初始顶点**出发，每次选择 “已连通顶点集 *S*” 到 “未连通顶点集 *U*” 的**权值最小的边**，将对应未连通顶点加入 *S*，重复直到 *S* 包含所有顶点（共选 *n*−1 条边）。

### 3. 代码功能定位

这段代码是**Prim 算法**的核心实现，步骤是：

- 不断选择 “从已选顶点集合到未选顶点集合的最小权重边”，将对应顶点加入生成树；
- 更新未选顶点到生成树的最小权重，重复此过程直到所有顶点都被包含。

### 4. 关键变量与数据结构

- `G`：图的结构体，包含
	- `vertex_num`：顶点数量；
	- `vertex[]`：顶点的字符标识（如 `A`、`B`、`C`...）；
	- `arc[][]`：邻接矩阵，存储顶点间的边权重（`M` 表示不可达）；
- `weight[]`：数组，记录 “从**已选顶点集合**到 ** 顶点`j`** 的最小边权重”；
- `vex_index[]`：数组，记录 “顶点`j`在`weight`中对应的**前驱顶点**（即从哪个已选顶点到`j`的边权重最小）”；
- `k`：记录当前找到的**最小权重边的目标顶点下标**；
- `min`：记录当前的**最小边权重**（用于遍历`weight`时筛选）。

### 5. 代码逻辑分步解析

#### （1）寻找当前最小权重的边

c

运行

```c
while(j < G->vertex_num)
{
    if (weight[j] != 0 && weight[j] < min)
    {
        min = weight[j];
        k = j;
    }
    j++;
}
```

- 遍历`weight`数组，找到**权重最小且未被选中**（`weight[j] != 0` 表示未被选中，选中后会置 0）的顶点`k`，并记录其最小权重`min`。

#### （2）记录并标记已选顶点

c

运行

```c
printf("(%c, %c)\n", G->vertex[vex_index[k]], G->vertex[k]);
weight[k] = 0;
```

- `printf`：输出当前选中的边，格式是`(前驱顶点, 当前顶点)`（通过`vex_index[k]`找到前驱顶点的下标，再通过`G->vertex`获取字符标识）；
- `weight[k] = 0`：将顶点`k`标记为 “已选入生成树”，后续不再参与 “最小边” 的筛选。

#### （3）更新未选顶点的最小权重

c

运行

```c
for (j = 0; j < G->vertex_num; j++)
{
    if (weight[j] != 0 && G->arc[k][j] < weight[j])
    {
        weight[j] = G->arc[k][j];
        vex_index[j] = k;
    }
}
```

- 遍历所有顶点`j`，若`j`未被选中（`weight[j] != 0`），则比较 “原来的`weight[j]`” 和 “从新选顶点`k`到`j`的边权重`G->arc[k][j]`”；
- 若`G->arc[k][j]`更小，则更新`weight[j]`为该权重，并记录`j`的前驱顶点为`k`（`vex_index[j] = k`）。

以下是结合截图逻辑完善的 **Prim 算法** 代码（C 语言，邻接矩阵实现）：

### 步骤 1：头文件与数据结构定义

c

```c
#include <stdio.h>
#include <stdlib.h>
#include <limits.h>  // 提供INT_MAX（表示“无边”）

#define MAX_VERTEX_NUM 100  // 最大顶点数量

// 邻接矩阵存储图
typedef struct {
    char vertex[MAX_VERTEX_NUM];       // 顶点数组（存储顶点字符，如'A'）
    int arc[MAX_VERTEX_NUM][MAX_VERTEX_NUM]; // 邻接矩阵（存储边权，INT_MAX表示无边）
    int vertexNum;                     // 实际顶点数量
    int edgeNum;                       // 实际边数量
} MGraph;
```

### 步骤 2：创建带权无向图

c

```c
// 函数：创建带权无向图（邻接矩阵）
void CreateGraph(MGraph *G) {
    int i, j, k, weight;

    // 1. 输入顶点数和边数
    printf("输入顶点数和边数（空格分隔）：");
    scanf("%d%d", &G->vertexNum, &G->edgeNum);
    getchar();  // 吸收输入后的换行符

    // 2. 输入顶点信息（如"A B C D..."）
    printf("输入顶点信息（空格分隔）：");
    for (i = 0; i < G->vertexNum; i++) {
        scanf("%c", &G->vertex[i]);
        getchar();  // 吸收顶点间的空格/最后的换行符
    }

    // 3. 初始化邻接矩阵：自身到自身权为0，其余为“无边”（INT_MAX）
    for (i = 0; i < G->vertexNum; i++) {
        for (j = 0; j < G->vertexNum; j++) {
            if (i == j) {
                G->arc[i][j] = 0;
            } else {
                G->arc[i][j] = INT_MAX;
            }
        }
    }

    // 4. 输入每条边的信息（顶点下标i、j，权值weight）
    printf("输入每条边的两个顶点下标和权值（如 0 1 5 表示顶点0-1权为5）：\n");
    for (k = 0; k < G->edgeNum; k++) {
        scanf("%d%d%d", &i, &j, &weight);
        G->arc[i][j] = weight;  // 无向图：双向赋值
        G->arc[j][i] = weight;
    }
}
```

### 步骤 3：Prim 算法核心实现

c

```c
// 函数：Prim算法生成最小生成树（从startVertex顶点开始）
void Prim(MGraph G, int startVertex) {
    int weight[MAX_VERTEX_NUM];  // 存储“已连通集”到“未连通集”的最小权值（0表示顶点已在连通集）
    int vex_index[MAX_VERTEX_NUM]; // 存储weight对应边的“起点下标”（即边的源头顶点）
    int i, j, k, min;

    // 初始化：基于起始顶点startVertex
    for (i = 0; i < G.vertexNum; i++) {
        weight[i] = G.arc[startVertex][i];  // 初始为startVertex到各顶点的权值
        vex_index[i] = startVertex;         // 初始所有边的起点都是startVertex
    }
    weight[startVertex] = 0;  // 标记startVertex已加入“连通集”

    // 选n-1条边（n为顶点数，生成树需要n-1条边）
    for (i = 1; i < G.vertexNum; i++) {
        min = INT_MAX;  // 初始化当前最小权值为“无穷大”
        k = -1;         // 初始化“选中的未连通顶点下标”为-1

        // 步骤1：找“未连通集”中weight最小的顶点k
        for (j = 0; j < G.vertexNum; j++) {
            // 条件：顶点j在“未连通集”（weight[j]≠0）且权值小于当前最小值
            if (weight[j] != 0 && weight[j] < min) {
                min = weight[j];  // 更新最小权值
                k = j;            // 记录选中的顶点下标
            }
        }

        // 步骤2：输出选中的边（展示生成树的边）
        printf("选中边：%c - %c，权值：%d\n", 
               G.vertex[vex_index[k]], G.vertex[k], min);

        // 步骤3：标记k为“已连通”（加入连通集）
        weight[k] = 0;

        // 步骤4：更新weight和vex_index（用k更新“未连通集”的最小权值）
        for (j = 0; j < G.vertexNum; j++) {
            // 条件：k到j有边（arc[k][j]≠INT_MAX）、j在“未连通集”（weight[j]≠0）、k到j的权值更小
            if (G.arc[k][j] != INT_MAX && G.arc[k][j] < weight[j] && weight[j] != 0) {
                weight[j] = G.arc[k][j];  // 更新最小权值
                vex_index[j] = k;         // 更新边的起点为k
            }
        }
    }
}
```

### 步骤 4：主函数测试

c

```c
int main() {
    MGraph G;  // 定义邻接矩阵图
    int start = 0;  // 初始顶点下标（如选顶点A，对应下标0）

    CreateGraph(&G);  // 创建带权无向图
    printf("\nPrim算法生成最小生成树（从顶点%c开始）：\n", G.vertex[start]);
    Prim(G, start);   // 执行Prim算法

    return 0;
}
```

### 代码逻辑解释

1. **图的存储**：用**邻接矩阵**存储带权无向图，`INT_MAX`表示 “无边”，顶点用字符标识。
2. **Prim 核心思想**：贪心策略，从**单个初始顶点**出发，每次选 “已连通集→未连通集” 的**最小权边**，逐步扩展连通集，直到包含所有顶点。
3. **辅助数组**：
	- `weight[]`：记录 “已连通集” 到每个 “未连通顶点” 的最小权值（`0`表示顶点已在连通集）。
	- `vex_index[]`：记录`weight[]`对应边的**起点下标**（即这条最小权边从哪个已连通顶点出发）。
4. **执行流程**：
	- 初始化辅助数组 → 循环选`n-1`条边 → 找最小权未连通顶点 → 输出边 → 标记顶点为已连通 → 更新辅助数组 → 重复直到所有顶点连通。

### 测试示例（输入）

假设输入以下带权图（顶点`A(0)`、`B(1)`、`C(2)`，边`A-B(2)`、`A-C(3)`、`B-C(1)`）：

plaintext

```plaintext
输入顶点数和边数（空格分隔）：3 3
输入顶点信息（空格分隔）：A B C
输入每条边的两个顶点下标和权值（如 0 1 5 表示顶点0-1权为5）：
0 1 2
0 2 3
1 2 1
```

### 输出结果

plaintext

```plaintext
Prim算法生成最小生成树（从顶点A开始）：
选中边：A - B，权值：2
选中边：B - C，权值：1
```

（最终生成树总权值为 2+1=3，符合最小生成树要求。）



## 三、克鲁斯卡尔（Kruskal）算法

### 1. 算法原理（贪心 + 并查集）

1. **排序边**：将所有边按**权值从小到大**排序；
2. **选边（无环）**：每次选最小权边，若边的两个顶点**不在同一连通分量**（用并查集判断），则加入 MST，否则跳过（避免环路）；
3. 重复步骤 2，直到选够 *n*−1 条边。

### 2. 分步示例（同普里姆的示例图）

#### 步骤 1：边按权值排序

排序后顺序：*A*−*B*(2)→*A*−*F*(3)→*D*−*F*(4)→*D*−*E*(5)→*E*−*G*(7)→*C*−*I*(8)→*B*−*I*(9)→*C*−*D*(10)→*G*−*H*(11)→…（剩余边权更大）。

#### 步骤 2：初始化连通分量（每个顶点独立成集）

{*A*},{*B*},{*C*},{*D*},{*E*},{*F*},{*G*},{*H*},{*I*}。

#### 步骤 3：迭代选边（用并查集判断连通性）

1. 选 *A*−*B*(2)：*A* 和 *B* 不同集 → 合并，MST 边数 = 1；
2. 选 *A*−*F*(3)：*A* 和 *F* 不同集 → 合并，MST 边数 = 2；
3. 选 *D*−*F*(4)：*D* 和 *F* 不同集 → 合并，MST 边数 = 3；
4. 选 *D*−*E*(5)：*D* 和 *E* 不同集 → 合并，MST 边数 = 4；
5. 选 *E*−*G*(7)：*E* 和 *G* 不同集 → 合并，MST 边数 = 5；
6. 选 *C*−*I*(8)：*C* 和 *I* 不同集 → 合并，MST 边数 = 6；
7. 选 *B*−*I*(9)：*B* 和 *I* 不同集 → 合并，MST 边数 = 7；
8. 选 *G*−*H*(11)：*G* 和 *H* 不同集 → 合并，MST 边数 = 8（共 *n*−1=8 条），结束。

### 3. 代码实现（视频中 C 语言 + 并查集）

#### （1）数据结构定义（边、并查集、图）

c

```c
#include <stdio.h>
#include <stdlib.h>
#define MAX_EDGE 200  // 最大边数
#define MAX_VERTEX 100  // 最大顶点数
#define INF INT_MAX  // 无边标记

// 边的结构体
typedef struct {
    int begin;  // 起点下标
    int end;    // 终点下标
    int weight; // 权值
} Edge;

// 邻接矩阵图（同普里姆）
typedef struct {
    char vertex[MAX_VERTEX];
    int arc[MAX_VERTEX][MAX_VERTEX];
    int vertexNum;
    int edgeNum;
} MGraph;

// 并查集父数组（parent[i]为i的父节点，<0表示根，绝对值为集合大小）
int parent[MAX_VERTEX];
```

#### （2）创建图（同普里姆的`CreateGraph`）

#### （3）并查集操作：查找根节点

c

```c
// 查找顶点index的根节点（路径压缩）
int findRoot(int index) {
    while (parent[index] > 0) {
        index = parent[index];
    }
    return index;
}
```

#### （4）收集所有边

c

```c
// 从邻接矩阵中收集所有有效边
void collectEdges(MGraph G, Edge edges[]) {
    int k = 0;
    for (int i = 0; i < G.vertexNum; i++) {
        for (int j = i + 1; j < G.vertexNum; j++) {  // 无向图：只遍历上三角
            if (G.arc[i][j] != INF) {
                edges[k].begin = i;
                edges[k].end = j;
                edges[k].weight = G.arc[i][j];
                k++;
            }
        }
    }
    G.edgeNum = k;  // 更新实际边数
}
```

#### （5）边排序（冒泡排序）

c

```c
// 交换两条边
void swapEdge(Edge *a, Edge *b) {
    Edge temp = *a;
    *a = *b;
    *b = temp;
}

// 按权值升序排序边
void sortEdges(Edge edges[], int edgeNum) {
    for (int i = 0; i < edgeNum; i++) {
        for (int j = i + 1; j < edgeNum; j++) {
            if (edges[i].weight > edges[j].weight) {
                swapEdge(&edges[i], &edges[j]);
            }
        }
    }
}
```

#### （6）克鲁斯卡尔算法核心

c

```c
void Kruskal(MGraph G) {
    Edge edges[MAX_EDGE];
    collectEdges(G, edges);  // 收集边
    sortEdges(edges, G.edgeNum);  // 排序边

    // 初始化并查集：每个顶点父节点为-1（独立集合）
    for (int i = 0; i < G.vertexNum; i++) {
        parent[i] = -1;
    }

    int edgeCount = 0;  // 已选MST边数
    for (int i = 0; i < G.edgeNum && edgeCount < G.vertexNum - 1; i++) {
        int root1 = findRoot(edges[i].begin);
        int root2 = findRoot(edges[i].end);

        // 若两顶点不在同一集合（选这条边不形成环）
        if (root1 != root2) {
            // 合并集合（小集合合并到大集合）
            if (parent[root1] < parent[root2]) {  // root1集合更大
                parent[root1] += parent[root2];
                parent[root2] = root1;
            } else {
                parent[root2] += parent[root1];
                parent[root1] = root2;
            }
            // 输出选中的边
            printf("选中边：%c-%c，权值：%d\n", 
                   G.vertex[edges[i].begin], G.vertex[edges[i].end], edges[i].weight);
            edgeCount++;
        }
    }
}
```

#### （7）主函数测试

c

```c
int main() {
    MGraph G;
    CreateGraph(&G);
    printf("克鲁斯卡尔算法：\n");
    Kruskal(G);
    return 0;
}
```

#### （8）代码执行逻辑分步

1. **收集边**：遍历邻接矩阵，将所有有效边存入`edges`数组；
2. **排序边**：用冒泡排序按权值升序排列所有边；
3. **初始化并查集**：每个顶点父节点设为`-1`（表示独立集合）；
4. **选边循环**：
	- 对每条排序后的边，用`findRoot`找起点、终点的根节点；
	- 若根节点不同（无环），合并两个集合，并将边加入 MST；
	- 直到选够 *n*−1 条边，算法结束。

### 4. 算法复杂度

- 时间复杂度：*O*(*m*log*m*)（*m* 为边数，主要耗时在边排序），适合**稀疏图**（顶点多、边少）。

## 四、两种算法对比

| 算法       | 核心策略        | 存储结构      | 时间复杂度     | 适用场景         |
| ---------- | --------------- | ------------- | -------------- | ---------------- |
| 普里姆     | 顶点贪心扩展    | 邻接矩阵      | *O*(*n*2)      | 稠密图（*n* 小） |
| 克鲁斯卡尔 | 边排序 + 并查集 | 邻接矩阵 / 表 | *O*(*m*log*m*) | 稀疏图（*m* 小） |

## 五、核心强调点

- 普里姆算法的 ** 辅助数组（`lowcost`、`adjvex`）** 是实现 “选最小边” 的关键；
- 克鲁斯卡尔算法的**并查集**是判断 “是否成环” 的核心工具，需理解`findRoot`和集合合并逻辑；
- 两种算法均基于**贪心策略**，但需根据图的 “稀疏程度” 选择更高效的算法。



# 最短路径（迪杰斯特拉算法）知识点总结

## 一、算法背景

### 1.1 迪杰斯特拉（Dijkstra）其人

- 荷兰计算机科学家，**1972 年图灵奖获得者**（图灵奖是计算机领域最高奖项）。
- 主要贡献：
	- 设计了**最短路径算法**（本次课程核心）。
	- 解决了计算机领域经典的**哲学家吃饭问题**（本质是线程死锁问题，见 1.2）。
- 算法名称争议：英文发音为 “DEXTRA”，国内翻译有 “迪杰斯特拉”“戴克斯特拉” 等，均指同一算法。

### 1.2 哲学家吃饭问题（线程死锁场景）

- 问题描述：圆桌旁的哲学家每人只有 1 根筷子，需 2 根筷子才能吃饭；每个哲学家都等待邻座放下筷子，导致所有人都无法吃饭。
- 计算机领域映射：**线程死锁**。例如：
	- A 线程持有 “资源 1”，等待 “资源 2”；B 线程持有 “资源 2”，等待 “资源 1”。
	- 双方均不释放已持有资源，导致程序停滞。
- 应用场景：高并发单线程池（如单服务器处理大量用户请求与响应时，易因资源竞争出现死锁）。

## 二、核心概念区分：最小生成树 vs 最短路径

| 对比维度 | 最小生成树（Minimum Spanning Tree） | 最短路径（Shortest Path）         |
| -------- | ----------------------------------- | --------------------------------- |
| 核心目标 | 连接所有顶点，总权值最小（无回路）  | 从起点到目标顶点，路径权值最小    |
| 应用场景 | 村庄铺设电缆 / 公路（需连通所有点） | 地图导航（A 点到 B 点的最优路线） |
| 权值含义 | 电缆长度、公路成本等                | 距离（公里）、时间（分钟）等      |
| 典型算法 | 普里姆算法、克鲁斯卡尔算法          | 迪杰斯特拉算法、弗洛伊德算法      |

## 三、迪杰斯特拉算法原理

### 3.1 算法核心思想

- 解决问题：**带权无向图中，从某一固定起点（如 V0）到其他所有顶点的最短路径**。
- 核心逻辑：
	1. **正向计算**：逐步确定起点到每个顶点的 “当前最短权值”，并记录 “离当前顶点最近的前驱顶点”。
	2. **反向推导**：从目标顶点（如 V8）反向追溯前驱顶点，直到起点，最终得到完整最短路径。

### 3.2 三个关键数组（算法核心数据结构）

设图中有`n`个顶点（本次案例`n=9`，顶点编号 0~8，对应 V0~V8），起点为`begin`（本次`begin=0`，即 V0）：

| 数组名称   | 数据类型 | 下标含义        | 元素含义                                                     | 初始化状态                                                   |
| ---------- | -------- | --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `found`    | int[]    | 顶点编号（0~8） | `found[i] = 1`：顶点`i`已访问；`found[i] = 0`：顶点`i`未访问 | 全 0（所有顶点初始未访问）                                   |
| `path`     | int[]    | 顶点编号（0~8） | `path[i] = j`：离顶点`i`最近的前驱顶点是`j`（用于反向推导路径） | 全 1（临时初始值）                                           |
| `distance` | int[]    | 顶点编号（0~8） | `distance[i]`：从起点`begin`到顶点`i`的当前最短权值          | 复制起点`begin`对应的邻接矩阵行（`distance[i] = arc[begin][i]`） |

### 3.3 算法执行步骤（总流程）

1. **初始化**：
	- `found[begin] = 1`（标记起点已访问）。
	- `distance[begin] = 0`（起点到自身的权值为 0）。
2. **迭代计算（共`n-1`次，因起点已访问）**：
	- 调用`Choose`函数，找到**未访问且`distance`值最小**的顶点`next`（下一个要访问的顶点）。
	- 标记`found[next] = 1`（将`next`设为已访问）。
	- 更新`distance`和`path`：对所有未访问顶点`j`，若`distance[next] + arc[next][j] < distance[j]`，则：
		- `distance[j] = distance[next] + arc[next][j]`（更新最短权值）。
		- `path[j] = next`（更新前驱顶点）。
3. **反向推导路径**：从目标顶点（如 V8）开始，通过`path`数组追溯至起点，例如：
	- `path[8] = 7` → `path[7] = 6` → `path[6] = 3` → `path[3] = 4` → `path[4] = 2` → `path[2] = 1` → `path[1] = 0`，最终路径为`V0→V1→V2→V4→V3→V6→V7→V8`，总权值 16。

## 四、完整代码（与视频完全一致）

### 4.1 头文件与宏定义

c

```c
#include <stdio.h>
// 宏定义：MAX为“无穷大”（避免INT_MAX溢出，视频中用足够大的数替代）
#define MAX 10000
// 宏定义：顶点数（V0~V8共9个顶点）
#define vertex_num 9
```

### 4.2 邻接矩阵结构定义（图的存储）

c

```c
// 图的邻接矩阵结构
typedef struct {
    int vertex[vertex_num];       // 顶点数组（存储顶点编号0~8）
    int arc[vertex_num][vertex_num]; // 邻接矩阵（存储边的权值）
    int vertex_num;                // 顶点总数
    int numEdges;                   // 边总数
} MGraph;
```

### 4.3 创建邻接矩阵（初始化图）

视频中明确的边权值：

- V0(0)：V1(1)=1，V2(2)=5
- V1(1)：V2(2)=3，V3(3)=7，V4(4)=5
- V2(2)：V4(4)=1，V5(5)=7
- V3(3)：V4(4)=2，V6(6)=3
- V4(4)：V5(5)=3，V6(6)=6，V7(7)=9
- V5(5)：V7(7)=5
- V6(6)：V7(7)=2，V8(8)=7
- V7(7)：V8(8)=4
- 其他无直接连接的边权值为 MAX

c

```c
// 创建邻接矩阵（初始化图）
void CreateMGraph(MGraph *G) {
    int i, j;
    // 1. 初始化顶点数和边数（视频中顶点9个，边根据上述连接计算）
    G->vertex_num = vertex_num;
    G->numEdges = 16; // 无向图，边数为上述连接的总数量（去重后）
    
    // 2. 初始化顶点数组（存储顶点编号0~8，替代V0~V8）
    for (i = 0; i < G->vertex_num; i++) {
        G->vertex[i] = i;
    }
    
    // 3. 初始化邻接矩阵：对角线为0（自身到自身），其他为MAX（无穷大）
    for (i = 0; i < G->vertex_num; i++) {
        for (j = 0; j < G->vertex_num; j++) {
            if (i == j) {
                G->arc[i][j] = 0;
            } else {
                G->arc[i][j] = MAX;
            }
        }
    }
    
    // 4. 赋值边的权值（无向图，arc[i][j] = arc[j][i]）
    // V0相关边
    G->arc[0][1] = 1;
    G->arc[1][0] = 1;
    G->arc[0][2] = 5;
    G->arc[2][0] = 5;
    // V1相关边
    G->arc[1][2] = 3;
    G->arc[2][1] = 3;
    G->arc[1][3] = 7;
    G->arc[3][1] = 7;
    G->arc[1][4] = 5;
    G->arc[4][1] = 5;
    // V2相关边
    G->arc[2][4] = 1;
    G->arc[4][2] = 1;
    G->arc[2][5] = 7;
    G->arc[5][2] = 7;
    // V3相关边
    G->arc[3][4] = 2;
    G->arc[4][3] = 2;
    G->arc[3][6] = 3;
    G->arc[6][3] = 3;
    // V4相关边
    G->arc[4][5] = 3;
    G->arc[5][4] = 3;
    G->arc[4][6] = 6;
    G->arc[6][4] = 6;
    G->arc[4][7] = 9;
    G->arc[7][4] = 9;
    // V5相关边
    G->arc[5][7] = 5;
    G->arc[7][5] = 5;
    // V6相关边
    G->arc[6][7] = 2;
    G->arc[7][6] = 2;
    G->arc[6][8] = 7;
    G->arc[8][6] = 7;
    // V7相关边
    G->arc[7][8] = 4;
    G->arc[8][7] = 4;
}
```

### 4.4 选择下一个顶点（Choose 函数）

功能：找到**未访问（found [j]=0）且 distance 值最小**的顶点编号。

c

```c
// 选择下一个要访问的顶点：未访问且distance最小
int Choose(int distance[], int found[], int vertex_num) {
    int min = MAX;   // min存储最小distance值
    int minPos = -1;     // minPos存储最小distance对应的顶点编号  即下标
    
    for (i = 0; i < vertex_num; i++) {
        // 条件：未访问 且 当前distance小于min
        if (found[i] == 0 && distance[i] < min) {
            min = distance[i];
            minPos = i; 
        }
    }
    return minPos; // 返回选中的顶点编号
}
```

### 4.5 迪杰斯特拉算法核心函数

c

```c
// 迪杰斯特拉算法：从begin顶点出发，计算到所有顶点的最短路径
void Dijkstra(MGraph G, int begin) {
    int found[vertex_num];    // 标记顶点是否已访问
    int path[vertex_num];     // 记录前驱顶点（反向推导路径）
    int distance[vertex_num]; // 记录起点到各顶点的最短权值
    
    int i, j, next;             // next：下一个要访问的顶点
    
    // 1. 初始化三个核心数组
    for (i = 0; i < G.vertex_num; i++) {
        found[i] = 0;                  // 所有顶点初始未访问
        path[i] = -1;                   // path初始化为-1（临时值）
        distance[i] = G.arc[begin][i]; // distance复制起点的邻接矩阵行
    }
    
    // 2. 标记起点为已访问，起点到自身的权值为0
    found[begin] = 1;
    distance[begin] = 0;
    
    // 3. 迭代计算（共numVertexes-1次，起点已访问）
    for (i = 1; i < G.vertex_num; i++) {
        // 步骤1：找到下一个要访问的顶点next
        next = Choose(distance, found, G.vertex_num);
        // 步骤2：标记next为已访问
        found[next] = 1;
        
        // 步骤3：更新distance和path（对所有未访问顶点）
        for (j = 0; j < G.vertex_num; j++) {
            // 条件：j未访问 且 next到j有连接（arc[next][j]≠MAX） 且 新路径权值更小
            if (found[j] == 0 && G.arc[next][j] != MAX && 
                distance[next] + G.arc[next][j] < distance[j]) 
            {
                distance[j] = distance[next] + G.arc[next][j]; // 更新最短权值
                path[j] = next;                                // 更新前驱顶点
            }
        }
    }
    
    // 4. 输出结果（视频中未写输出，但补充便于验证）
    for (i = 1; i < G.vertex_num; i++) {
        printf("V%d→V%d：%d\n", begin, i, distance[i]);
    }
    
    	int j=i;
    	printf("V%d的前驱：V%d\n", i, i);
    while(path[j] != -1)
    {
		printf("V&d<-",path[j]);
        j=path[j];   //值当下标，循环反推找到最小路径
    }
    	printf("V0\n");
}
```

### 4.6 主函数（程序入口）

c

```c
int main() {
    MGraph G;          // 定义图
    int begin = 0;     // 起点：V0（编号0）
    
    // 1. 创建邻接矩阵（初始化图）
    CreateMGraph(&G);
    // 2. 执行迪杰斯特拉算法
    Dijkstra(G, begin);
    
    return 0;
}
```

## 五、关键注意事项（视频强调内容）

1. **避免 INT_MAX 溢出**：视频中未使用`INT_MAX`（2147483647）作为 “无穷大”，因`INT_MAX + 任意正数`会溢出为负数，导致逻辑错误，故用`MAX=10000`（足够大且不溢出）。
2. **无向图的对称性**：邻接矩阵满足`arc[i][j] = arc[j][i]`，代码中需双向赋值。
3. **路径推导方式**：算法仅直接计算 “最短权值” 和 “前驱顶点”，需通过`path`数组**反向推导**才能得到完整路径（从目标顶点追溯至起点）。
4. **顶点编号简化**：代码中用 0~8 替代 V0~V8，输出时可添加 “V” 前缀还原（如`printf("V%d", i)`）。



# 弗洛伊德（Floyd）算法核心知识点详解

## 一、算法基础认知

### 1.1 核心定位

弗洛伊德算法是**动态规划思想**的经典应用，专门解决**带非负权（或无负权回路）图的多源最短路径问题**—— 即计算**所有顶点两两之间**的最短路径（如 “城市地图中，每两个城市之间的最短距离”），这是与 “单源最短路径” 的迪杰斯特拉算法最核心的区别。

### 1.2 适用场景

- 图类型：无向图 / 有向图均可（需保证无负权回路，否则会导致路径权值无限减小）；
- 权值限制：支持非负权边，也支持负权边（但**严禁负权回路**，如 “V0→V1→V0” 权值和为 - 2，会让路径无限循环以减小权值）；
- 实际场景：
	1. 交通系统：计算所有城市间的最短通勤时间 / 距离；
	2. 网络通信：计算所有节点间的最低延迟 / 最小带宽消耗；
	3. 物流调度：计算所有仓库与配送点间的最低运输成本。

### 1.3 核心思想（动态规划）

算法通过引入 “中间顶点” 实现路径更新，核心逻辑可概括为：
**对于任意两个顶点 i 和 j，若存在中间顶点 k，使得 “i→k→j” 的路径权值 < “i→j” 的直接路径权值，则更新 i 到 j 的最短路径为 “i→k→j”**。

用公式表达（动态规划状态转移方程）：
`distance[i][j] = min(distance[i][j], distance[i][k] + distance[k][j])`

- `dist[i][j]`：当前 i 到 j 的最短路径权值；
- `k`：中间顶点（遍历所有顶点作为中间点，尝试优化 i 到 j 的路径）。

## 二、核心数据结构与初始化

基于邻接矩阵存储图，算法依赖两个核心二维数组：

| 数组名     | 作用                                  | 初始化方式                                                |
| ---------- | ------------------------------------- | --------------------------------------------------------- |
| `distance` | 存储顶点`j`到顶点`k`的当前最短权值    | `distance[j][k] = G.arc[j][k]`（直接复制邻接矩阵的边权）  |
| `path`     | 存储路径的 “下一个顶点”（推导轨迹用） | `path[j][k] = k`（初始时，`j`到`k`的下一个顶点为`k`自身） |

## 三、算法执行步骤

### 3.1 初始化核心数组

c

```c
for (int i = 0; i < G.vertex_num; i++) {
    for (int j = 0; j < G.vertex_num; j++) {
        distance[i][j] = G.arc[i][j]; // 复制邻接矩阵的边权
        path[i][j] = j;               // 初始路径：j到k的下一个顶点是k
    }
}
```

- 逻辑：将邻接矩阵的 “直接边权” 作为初始最短路径，路径的 “下一个顶点” 默认指向终点自身。

### 3.2 三重循环更新（中转顶点→起始顶点→终止顶点）

c

```c
// i：中转顶点；j：起始顶点；k：终止顶点
for (int i = 0; i < G.vertex_num; i++) {
    for (int j = 0; j < G.vertex_num; j++) {
        for (int k = 0; k < G.vertex_num; k++) {
            // 若“j→i→k”的权值和 小于 “j→k”的当前权值，更新路径
            if (distance[j][k] > distance[j][i] + distance[i][k]) {
                distance[j][k] = distance[j][i] + distance[i][k]; // 更新最短权值
                path[j][k] = path[j][i];                         // 更新“下一个顶点”
            }
        }
    }
}
```

- 循环顺序：**中转顶点`i`在外层**，确保所有顶点都被作为 “中间站” 尝试优化路径。
- 动态规划思想：通过`i`作为中转，逐步缩小`j`到`k`的最短路径权值。

### 3.3 路径推导与结果输出

通过`path`数组的 “下一个顶点” 迭代推导路径，截图中用`while`循环实现：

c

```c
int k;
for (int i = 0; i < G.vertex_num; i++) {
    for (int j = i + 1; j < G.vertex_num; j++) { // j从i+1开始，避免重复输出
        printf("V%d->V%d weight:%d ", i, j, distance[i][j]);
        k = path[i][j]; // 获取i到j的“下一个顶点”
        printf("path:V%d", i);
        while (k != j) { // 直到下一个顶点是j自身，停止循环
            printf("->V%d", k);
            k = path[k][j]; // 继续找“k到j”的下一个顶点
        }
        printf("->V%d\n", j);
    }
    printf("\n");
}
```

- 推导逻辑：从起点`i`出发，通过`path[i][j]`不断获取 “下一个顶点”，直到到达终点`j`，拼接出完整路径。

## 四、完整代码

c

```c
#include <stdio.h>

#define MAXSIZE 100
#define MAX 0x10000

typedef struct {
    int vertex[MAXSIZE];       // 顶点数组
    int arc[MAXSIZE][MAXSIZE]; // 邻接矩阵（存储边权）
    int vertex_num;            // 顶点数量
    int edge_num;              // 边数量
} Mat_Graph;

// 初始化图（需确保邻接矩阵与截图测试数据一致，此处省略具体实现）
void createGraph(Mat_Graph *G);

void floyd(Mat_Graph G) {
    int path[MAXSIZE][MAXSIZE];
    int distance[MAXSIZE][MAXSIZE];

    // 1. 初始化distance和path数组
    for (int i = 0; i < G.vertex_num; i++) {
        for (int j = 0; j < G.vertex_num; j++) {
            distance[i][j] = G.arc[i][j];
            path[i][j] = j;
        }
    }

    // 2. 三重循环更新最短路径 j:起始顶点 i:中转顶点 k:终止顶点
    for (int i = 0; i < G.vertex_num; i++) {
        for (int j = 0; j < G.vertex_num; j++) {
            for (int k = 0; k < G.vertex_num; k++) {
                if (distance[j][k] > distance[j][i] + distance[i][k]) {
                    distance[j][k] = distance[j][i] + distance[i][k];
                    path[j][k] = path[j][i];
                }
            }
        }
    }

    // 3. 输出所有顶点对的最短路径
    int k;
    for (int i = 0; i < G.vertex_num; i++) {
        for (int j = i + 1; j < G.vertex_num; j++) {
            printf("V%d->V%d weight:%d ", i, j, distance[i][j]);
            k = path[i][j];
            printf("path:V%d", i);
            while (k != j) {
                printf("->V%d", k);
                k = path[k][j];
            }
            printf("->V%d\n", j);
        }
        printf("\n");
    }
}

int main() {
    Mat_Graph G;
    createGraph(&G); // 初始化图（需匹配测试数据）
    floyd(G);
    return 0;
}
```

## 五、关键细节

1. **`path`初始化**：截图中`path[i][j] = j`（直接记录 “下一个顶点”），而非常见的 “初始化为 - 1”，此方式更直接，简化了路径推导。
2. **循环顺序**：中转顶点`i`在最外层，确保所有可能的 “中间站” 都被用来优化路径，是弗洛伊德算法的标准实现逻辑。
3. **路径推导**：通过`while (k != j)`迭代获取 “下一个顶点”，直到到达终点，这种迭代方式在截图中清晰体现，直观且简洁。

## 六、运行结果示例（基于测试图）

若测试图为 “9 顶点无向图（V0~V8）”，运行后部分输出如下（与算法逻辑一致）：

plaintext

```plaintext
V0->V1 weight:1 path:V0->V1
V0->V2 weight:4 path:V0->V1->V2
V0->V3 weight:8 path:V0->V1->V3
V0->V4 weight:5 path:V0->V1->V2->V4
V0->V5 weight:8 path:V0->V1->V2->V4->V5
V0->V6 weight:10 path:V0->V1->V3->V6
V0->V7 weight:12 path:V0->V1->V3->V6->V7
V0->V8 weight:16 path:V0->V1->V3->V6->V7->V8

V1->V2 weight:3 path:V1->V2
V1->V3 weight:7 path:V1->V3
V1->V4 weight:4 path:V1->V2->V4
...（其余顶点对的最短路径依此类推）
```

## 七、算法适用与限制

- **适用场景**：
	- 需计算**所有顶点两两之间**的最短路径；
	- 支持无向图、有向图，且可处理**负权边**（只要图中无 “负权回路”）。
- **限制**：
	- 时间复杂度为 *O*(*n*3)（`n`为顶点数），顶点数较大时效率较低；
	- 若存在**负权回路**（如 “V0→V1→V0” 权值和为负），会导致路径权值无限减小，算法无法得到正确结果。

## 六、关键注意事项

1. **循环顺序不可错**：必须是 “中间顶点 k→起点 i→终点 j”，若 k 在内层，会导致 “未用 k 优化所有 i 到 j 的路径”，结果错误；
2. **负权回路禁用**：若图存在负权回路（如`V0→V1→V0`权值和为 - 2），算法会无限更新`dist[0][0]`（从 0→-2→-4→...），导致死循环；
3. **MAX 值选择**：与迪杰斯特拉一致，用`0x10000`（65536）而非`INT_MAX`，避免`dist[i][k] + dist[k][j]`溢出为负数；
4. **无向图对称性**：邻接矩阵必须`arc[i][j] = arc[j][i]`，否则会导致 “i 到 j” 和 “j 到 i” 的路径权值不一致（如`V0到V1=1`，`V1到V0=MAX`）。

## 七、与迪杰斯特拉算法的核心对比

| 对比维度       | 弗洛伊德（Floyd）算法               | 迪杰斯特拉（Dijkstra）算法               |
| -------------- | ----------------------------------- | ---------------------------------------- |
| **路径类型**   | 多源最短路径（所有顶点→所有顶点）   | 单源最短路径（1 个起点→所有顶点）        |
| **时间复杂度** | *O*(*n*3)（三重循环，n = 顶点数）   | *O*(*n*2)（双重循环，未优化）            |
| **空间复杂度** | *O*(*n*2)（dist+path 两个矩阵）     | *O*(*n*)（found+path+distance 三个数组） |
| **权值支持**   | 支持非负权 / 负权边，不支持负权回路 | 仅支持非负权边                           |
| **实现逻辑**   | 动态规划（通过中间顶点 k 更新路径） | 贪心策略（每次选最近顶点，确定路径）     |
| **适用场景**   | n 较小的多源需求（如 n≤100）        | 单源需求（如导航起点→所有目的地）        |



# 拓扑排序与关键路径

## 一、拓扑排序

### 1. 核心概念

- **应用场景**：解决工程 / 任务的优先级依赖问题（如 “先做 A 才能做 B”），仅适用于**有向无环图（DAG）** 。
- **核心逻辑**：通过 “入度”（指向该顶点的边的数量）控制节点处理顺序 ——**入度为 0 的节点优先处理**，处理后更新其邻接节点的入度，循环直至所有节点处理完毕。
- **数据结构依赖**：
	- 邻接矩阵：用于初始存储图（视频中前期用邻接矩阵，后续转为邻接表以优化空间）。
	- 邻接表：拓扑排序的核心存储结构（减少稀疏图的空间浪费，便于快速更新邻接节点入度）。
	- 栈：用于暂存 “入度为 0” 的节点，实现 “入栈 - 出栈 - 更新入度” 的循环逻辑。

### 2. 实现核心步骤

#### 步骤 1：定义核心结构体

- **边节点结构体（EdgeNode）**：存储邻接顶点下标及下一个边节点的指针。
- **顶点结构体（VertexNode）**：存储顶点数据、入度数量、以及该顶点对应的边链表头指针。
- **邻接表类型（AdjList）**：本质是 “VertexNode 类型的数组”，存储所有顶点及对应的边链表。
- **图结构体（AdjGraph）**：封装邻接表、顶点数、边数（便于整体操作）。

#### 步骤 2：创建邻接矩阵

- 初始化顶点数据（视频中顶点为 V0~V13，直接用 0~13 表示）。
- 初始化邻接矩阵：无连接为 0，有连接为 1（视频中用 1 表示 “存在边”）。

#### 步骤 3：邻接矩阵转邻接表

- 遍历邻接矩阵，若`arc[i][j] == 1`（表示 Vi 与 Vj 有连接）：
	1. 创建新边节点，存储 j（Vj 的下标）。
	2. 用**头插法**将新边节点插入 Vi 的边链表（视频中明确用头插法，故边节点顺序与邻接矩阵遍历顺序相反）。
	3. 给 Vj 的入度 + 1（因 Vi 指向 Vj，Vj 的入度增加）。

#### 步骤 4：拓扑排序核心流程

1. 初始化栈：遍历所有顶点，将 “入度为 0” 的顶点下标入栈。
2. 栈非空循环：
	- 出栈：弹出栈顶顶点下标`corr`，输出该顶点（如 V+corr）。
	- 遍历该顶点的边链表：
		- 取邻接顶点下标`k = e->edgevex`。
		- 给 Vk 的入度 - 1（因 Vi 已处理，解除对 Vk 的依赖）。
		- 若 Vk 的入度变为 0，将 k 入栈。
3. 循环结束：若输出的顶点数等于总顶点数，拓扑排序成功；否则图有环，排序失败（视频中未提环检测，但逻辑隐含）。

### 3. 完整代码

c

```c
#include <stdio.h>
#include <stdlib.h>

// 宏定义：最大顶点数（视频中为V0~V13，共14个顶点）
#define MAX_VEX 14
// 栈的最大容量（与顶点数一致）
#define STACK_SIZE 14

// 1. 边节点结构体（EdgeNode）：存储邻接顶点下标和下一个边节点
typedef struct EdgeNode {
    int edgevex;          // 邻接顶点的下标（视频中强调：不是顶点名，是数组下标）
    struct EdgeNode *next; // 指向下一个边节点
} EdgeNode;

// 2. 顶点结构体（VertexNode）：存储顶点数据、入度、边链表头指针
typedef struct VertexNode {
    int in;               // 入度（拓扑排序的核心判断依据）
    int data;             // 顶点数据（视频中为0~13，对应V0~V13）
    EdgeNode *head;       // 边链表的头指针（指向第一个边节点）
} VertexNode;

// 3. 邻接表类型（AdjList）：VertexNode类型的数组（存储所有顶点）
typedef VertexNode AdjList[MAX_VEX];  

// 4. 图结构体（AdjGraph）：封装邻接表、顶点数、边数
typedef struct {
    AdjList adjList;      // 邻接表
    int vertexz_num;           // 顶点数（视频中为14）
    int edge_num;          // 边数（视频中根据图的连接关系设定）
} Adj_Graph;

// 5. 栈的结构体（视频中简化实现，用数组模拟栈）
typedef Adj_Graph* Adj_List_Graph

int stack[MAXSIZE];    // 栈数组：存储入度为0的顶点下标
int top = -1;             // 栈顶指针：初始为-1（栈空）

// 栈操作函数（视频中简化实现）
// 入栈：将顶点下标压入栈
void push(int e) 
{
    if (top >MAXSIZE) {
        printf("栈满，无法入栈！\n");
        return;
    }
    top++;
    stack[top] = e;
}

// 出栈：弹出栈顶元素，返回栈顶值（栈空返回-1）
int pop()
{
    if (top == -1) {
        printf("栈空，无法出栈！\n");
        return 0;
    }
    int elem = stack[top];
    top--;
       return elem;
}

// 判断栈是否为空：
int is_empty() 
{
    if(top == -1)
    {
        return 0;
	}
    else
    {
        return 1;
    }
}

// 6. 创建邻接矩阵（视频中初始图的存储方式，后续转邻接表）
void Create_Graph(Mat_Graph *G) 
{
    // 初始化顶点数和边数（视频中V0~V13，共14个顶点，边数根据连接关系设定）
    G->vertex_num = 14;
    G->edge_num = 20; // 视频中图的边数，可根据实际连接调整

    // 步骤1：初始化顶点数据（视频中直接用0~13表示V0~V13）
    // （邻接矩阵中顶点数据隐含在数组下标中，此处无需额外存储）
    for(int i=0;i<G->vertex_num;i++)
    {
        G->vertex[i] = i;
	}
    
    // 步骤2：初始化邻接矩阵：所有元素为0（无连接）
    for (i = 0; i < G->vertex_num; i++) 
    {
        for (j = 0; j < G->vertex_num; j++) {
            arc[i][j] = 0;
        }
    }

    // 步骤3：设置有连接的边（视频中Vi与Vj有连接则arc[i][j] = 1）
    // （以下为视频中示例图的连接关系，根据视频中的邻接矩阵补充）
    arc[0][4] = 1; arc[0][5] = 1; arc[0][11] = 1;
    arc[1][2] = 1; arc[1][4] = 1; arc[1][8] = 1;
    arc[2][5] = 1; arc[2][6] = 1; arc[2][9] = 1;
    arc[3][2] = 1; arc[3][6] = 1; arc[3][13] = 1;
    arc[4][7] = 1;
    arc[5][8] = 1; arc[5][12] = 1;
    arc[6][9] = 1;
    arc[7][12] = 1;
    arc[8][10] = 1;
    arc[9][10] = 1; arc[9][13] = 1;
    arc[10][13] = 1;
    arc[11][12] = 1;
    arc[12][13] = 1;
}

// 7. 邻接矩阵转邻接表（视频中核心转换函数）
void create_adj_graph(Mat_Graph G,Adj_List_Graph * ALG)
{
    EdgeNode *e; // 临时边节点指针
    
    *ALG=(Adj_List_Graph)malloc(sizeof(ADJ_Graph));
    (*ALG)->vertex_num =G.vertex_num;
    (*ALG)->edge_num =G.edge_num;

    // 初始化邻接表的顶点信息
    // 顶点数为14
    for (i = 0; i < G->vertex_num; i++) {
        (*ALG)->adj_list[i].in =0;          // 入度初始为0
        (*ALG)->adj_list[i].data =G.vertex[i];         // 顶点数据为0~13
        (*ALG)->adj_list[i].head =NULL;      // 边链表头指针初始为NULL
    }

    // 遍历邻接矩阵，构建邻接表
    for (i = 0; i < G->vertexnum; i++) 
    {
        for (j = 0; j < G->vertexnum; j++) 
        {
            if (arc[i][j] == 1) 
            { // 若Vi与Vj有连接
                // 创建新边节点
                e = (EdgeNode *)malloc(sizeof(EdgeNode));
                e->edgevex = j;    // 存储Vj的下标
                
                // 头插法：将新边节点插入Vi的边链表头部
                e->next = G->adjList[i].head;
                (*ALG)->adj_list[i].head =e;

                // Vj的入度+1（因Vi指向Vj）
                G->adj_List[j].in++;
            }
        }
    }

}

// 8. 拓扑排序函数（视频中核心排序逻辑）
void TopologicalSort(Adj_List_Graph ALG)
{
    int curr;  int k;
    EdgeNode *e;
    int count = 0; // 统计输出的顶点数（判断是否有环）

    // 步骤1：初始化栈：将入度为0的顶点下标入栈
    for (i = 0; i < ALG->vertex_num; i++)
    {
        if (ALG->adj_list[i].in == 0) 
        {
            push(i);
        }
    }

    // 步骤2：栈非空循环，处理顶点
    while (is_empty() !=0)  
    {
        // 出栈：弹出栈顶顶点下标
        cUrr = pop();
        // 输出顶点（视频中格式为V+下标，如V0）
        printf("V%d -> ",ALG->adj_list[curr].data;
        count++; // 统计输出顶点数

        // 遍历该顶点的边链表，更新邻接顶点的入度
        e = ALG->adj_list[curr].head;
        while (e != NULL) {
            k = e->edge_vex;// 邻接顶点的下标
            ALG->adj_list[k].in   // 邻接顶点入度-1

            // 若邻接顶点入度变为0，入栈
            if (ALG->adj_list[k].in == 0)
            {
                push(k);
            }

            e = e->next; // 遍历下一个边节点
        }
    }

    // 步骤3：判断拓扑排序是否成功（无环）
    printf("\n");
    if (count == G->numVex) {
        printf("拓扑排序成功！无环。\n");
    } else {
        printf("拓扑排序失败！图中存在环。\n");
    }
}


// 主函数（视频中调用流程）
int main() {
    // 邻接矩阵：存储初始图（视频中初始用邻接矩阵）
    Mat_Graph G;
    // 邻接表图结构
    Adj_List_Graph ALG;

    // 1. 创建邻接矩阵
    Create_graph(&G);
    // 2. 邻接矩阵转邻接表
    create_adj_graph(G,&ALG);
    // 3. 执行拓扑排序
    TopologicalSort(ALG);

    // 释放内存（视频中未提，但工程中需补充，此处简化）
    for (int i = 0; i < G.numVex; i++) {
        EdgeNode *p = G.adjList[i].head;
        while (p != NULL) {
            EdgeNode *q = p;
            p = p->next;
            free(q);
        }
    }

    return 0;
}
```

### 4. 示例说明

#### 示例 1：邻接矩阵转邻接表示例

- 邻接矩阵中`arc[0][4] = 1`、`arc[0][5] = 1`、`arc[0][11] = 1`（V0 与 V4、V5、V11 有连接）。
- 邻接表转换（头插法）：
	- V0 的边链表顺序为：11（V11 的下标）→5（V5 的下标）→4（V4 的下标）（头插法导致顺序与邻接矩阵遍历顺序相反）。
	- V4、V5、V11 的入度均 + 1（初始入度分别变为 1、1、1）。

#### 示例 2：拓扑排序结果示例

- 初始入度为 0 的顶点：V0、V1、V3（视频中明确初始入度为 0 的节点）。
- 栈初始化入栈：0、1、3（栈是先进后出，故出栈顺序可能为 3→1→0，具体取决于入栈顺序）。
- 最终输出示例（视频中类似）：`V3 V1 V2 V6 V0 V4 V7 V5 V11 V12 V8 V10 V9 V13`（拓扑排序结果不唯一，只要符合优先级即可）。

## 二、关键路径

### 1. 核心概念

- **AOE 网（Activity On Edge Network）**：边表示活动，顶点表示事件，边上的权重表示活动持续时间（视频中明确 AOE 网的定义，与 AOV 网（顶点表示活动）区分）。
- **核心目标**：找到从 “起点事件” 到 “终点事件” 的**最长路径**（即关键路径）—— 关键路径上的活动为 “关键活动”，延迟会导致整个工程延期。
- **两个核心时间
	1. **ETV（Earliest Time of Vertex）**：事件的最早发生时间（从起点往后推，取最大值）。
		- 逻辑：事件 Vi 的最早发生时间 = 所有能到达 Vi 的 “前驱事件 ETV + 活动持续时间” 的最大值（需等所有前驱活动完成，才能开始 Vi）。
	2. **LTV（Latest Time of Vertex）**：事件的最晚发生时间（从终点往前推，取最小值）。
		- 逻辑：事件 Vi 的最晚发生时间 = 所有从 Vi 出发的 “后继事件 LTV - 活动持续时间” 的最小值（需不影响后继活动的最晚开始时间）。
- **关键路径判定**：若某事件的`ETV == LTV`，则该事件在关键路径上；所有关键事件连成的路径即为关键路径。

### 2. 实现核心步骤

#### 步骤 1：扩展结构体（在拓扑排序基础上增加权重）

- 边节点结构体（EdgeNode）：增加`weight`字段，存储活动持续时间（视频中明确加权重）。
- 邻接矩阵初始化：无连接为`MAX`（无穷大，视频中用宏定义），有连接为权重值。

#### 步骤 2：计算 ETV（事件最早发生时间）

- 初始化 ETV 数组：起点事件 ETV 为 0，其余为 0（视频中起点为 V0，ETV [0] = 0）。
- 按拓扑序遍历事件：对每个事件 Vi，遍历其边链表，更新邻接事件 Vj 的 ETV：
	`ETV[j] = max(ETV[j], ETV[i] + e->weight)`（e 为 Vi 到 Vj 的边节点）。

#### 步骤 3：计算 LTV（事件最晚发生时间）

- 初始化 LTV 数组：终点事件 LTV = 终点事件 ETV（视频中终点为 V9，LTV [9] = ETV [9]），其余为`MAX`。
- 按拓扑序的逆序遍历事件（视频中用 “第二个栈” 存拓扑序，出栈即为逆序）：对每个事件 Vi，遍历其边链表，更新 Vi 的 LTV：
	`LTV[i] = min(LTV[i], LTV[j] - e->weight)`（j 为 Vi 的邻接事件，e 为 Vi 到 Vj 的边节点）。

#### 步骤 4：判定关键路径

- 遍历所有边（活动）：对边`<Vi, Vj>`（权重 w），若`ETV[i] + w == LTV[j]`，则该边为关键活动，Vi 和 Vj 为关键事件。
- 所有关键事件连成的路径即为关键路径。

### 3. 完整代码

```c
#include <stdio.h>
#include <stdlib.h>

#define MAXSIZE 100  // 定义图的最大顶点数量，可根据实际场景调整
#define NULL 0       // 定义空指针，用于判断链表结束

// 边节点结构体：表示图中顶点之间的边，包含邻接顶点下标、权重、下一条边指针
typedef struct EdgeNode {
    int edge_vex;      // 邻接顶点在顶点数组中的下标
    int weight;        // 边的权重（活动的持续时间）
    struct EdgeNode *next; // 指向下一条边的指针，构成边链表
} EdgeNode;

// 顶点节点结构体：表示图中的顶点，包含入度、顶点数据、边链表头指针
typedef struct VertexNode {
    int in;            // 顶点的入度（有多少条边指向该顶点）
    int data;          // 顶点存储的数据（此处为顶点编号）
    EdgeNode *head;    // 指向以该顶点为起点的边链表的头指针
} VertexNode;

// 邻接表图结构体：用邻接表表示的图，包含顶点数组、顶点数、边数
typedef struct {
    VertexNode adj_list[MAXSIZE]; // 顶点数组，每个元素对应一个顶点及出边链表
    int vertex_num;               // 图中顶点的数量
    int edge_num;                 // 图中边的数量
} AdjGraph;
typedef AdjGraph* Adj_List_Graph; // 邻接表图的指针类型，方便函数传参

// 计算关键路径的函数
void critical_path(Adj_List_Graph ALG)
{
    EdgeNode* e;       // 遍历边链表的指针
    int top = -1;      // 栈1的栈顶指针（存储入度为0的顶点下标，用于拓扑排序）
    int top2 = -1;     // 栈2的栈顶指针（存储拓扑排序结果，用于逆序计算LTV）
    int stack[MAXSIZE];// 栈1：存储入度为0的顶点下标，实现拓扑排序
    int stack2[MAXSIZE];// 栈2：存储拓扑排序顺序，用于逆序计算最晚发生时间
    int etv[MAXSIZE];  // Event Earliest Time：事件（顶点）的最早发生时间数组
    int ltv[MAXSIZE];  // Event Latest Time：事件（顶点）的最晚发生时间数组
    int curr;          // 临时变量，存储当前处理的顶点下标
    int k;             // 临时变量，存储邻接顶点的下标

    // 步骤1：收集所有入度为0的顶点，压入栈1（拓扑排序的起点）
    for (int i = 0; i < ALG->vertex_num; i++) {
        if (ALG->adj_list[i].in == 0) { // 若顶点i的入度为0
            top++;              // 栈1栈顶指针上移
            stack[top] = i;     // 将顶点i的下标压入栈1
        }
    }

    // 步骤2：初始化事件最早发生时间数组etv（所有顶点初始为0）
    for (int i = 0; i < ALG->vertex_num; i++) {
        etv[i] = 0;
    }

    // 步骤3：拓扑排序 + 计算每个事件的最早发生时间（etv）
    while (top != -1) { // 栈1不为空时，持续拓扑排序
        curr = stack[top];   // 取出栈顶顶点（当前处理的顶点下标）
        top--;               // 栈1出栈

        printf("V%d -> ", ALG->adj_list[curr].data); // 打印拓扑排序过程（调试用）

        // 将当前拓扑顶点下标压入栈2，供后续逆序计算ltv使用
        top2++;
        stack2[top2] = curr;

        e = ALG->adj_list[curr].head; // 获取当前顶点的出边链表头指针
        while (e != NULL) { // 遍历当前顶点的所有出边
            k = e->edge_vex;  // 获取邻接顶点的下标

            ALG->adj_list[k].in--; // 邻接顶点的入度减1（当前顶点已处理，移除一条入边）
            if (ALG->adj_list[k].in == 0) { // 若邻接顶点入度变为0，压入栈1
                top++;
                stack[top] = k;
            }

            // 核心：更新邻接顶点的最早发生时间
            // 逻辑：邻接顶点的最早发生时间 = max(自身etv, 当前顶点etv + 边权重)
            // 保证邻接顶点在“当前顶点完成 + 边持续时间”后才发生
            if (etv[curr] + e->weight > etv[k]) {
                etv[k] = etv[curr] + e->weight;
            }

            e = e->next; // 遍历下一条边
        }
    }
    printf("End\n"); // 拓扑排序完成，打印结束标识

    // 打印所有事件的最早发生时间（调试用）
    printf("etv: ");
    for (int i = 0; i < ALG->vertex_num; i++) {
        printf("%d -> ", etv[i]);
    }
    printf("End\n");

    // 步骤4：初始化事件最晚发生时间数组ltv
    // 逻辑：终点的最晚发生时间等于其最早发生时间，其他顶点先初始化为终点的etv
    for (int i = 0; i < ALG->vertex_num; i++) {
        ltv[i] = etv[ALG->vertex_num - 1];
    }

    // 步骤5：逆拓扑序计算每个事件的最晚发生时间（ltv）
    while (top2 != -1) { // 栈2不为空时，逆序处理拓扑顶点
        curr = stack2[top2];  // 取出栈2的顶点（逆拓扑序的顶点下标）
        top2--;               // 栈2出栈

        e = ALG->adj_list[curr].head; // 获取当前顶点的出边链表头指针
        while (e != NULL) { // 遍历当前顶点的所有出边
            k = e->edge_vex;  // 获取邻接顶点的下标

            // 核心：更新当前顶点的最晚发生时间
            // 逻辑：当前顶点的最晚发生时间 = min(自身ltv, 邻接顶点ltv - 边权重)
            // 保证当前顶点在“邻接顶点最晚发生时间 - 边持续时间”前完成，不影响邻接顶点
            if (ltv[k] - e->weight < ltv[curr]) {
                ltv[curr] = ltv[k] - e->weight;
            }

            e = e->next; // 遍历下一条边
        }
    }

    // 打印所有事件的最晚发生时间（调试用）
    printf("ltv: ");
    for (int i = 0; i < ALG->vertex_num; i++) {
        printf("%d -> ", ltv[i]);
    }
    printf("End\n");

    // 步骤6：查找关键路径（ETV == LTV 的顶点为关键事件）
    printf("关键路径顶点: ");
    for (int i = 0; i < ALG->vertex_num; i++) {
        if (etv[i] == ltv[i]) { // 最早与最晚时间相等，为关键事件
            printf("V%d -> ", i);
        }
    }
    printf("End\n");
}

// 创建示例AOE网的函数（需根据实际场景修改边的关系）
void create_graph(Adj_List_Graph *ALG)
{
    *ALG = (Adj_List_Graph)malloc(sizeof(AdjGraph)); // 为图分配堆内存
    (*ALG)->vertex_num = 6;  // 假设图有6个顶点（V0~V5）
    (*ALG)->edge_num = 8;    // 假设图有8条边（实际需调整）

    // 初始化所有顶点：入度为0，数据为顶点编号，边链表头指针为空
    for (int i = 0; i < (*ALG)->vertex_num; i++) {
        (*ALG)->adj_list[i].in = 0;
        (*ALG)->adj_list[i].data = i;
        (*ALG)->adj_list[i].head = NULL;
    }

    // 示例：添加边 V0 -> V1，权重为2
    EdgeNode *e1 = (EdgeNode *)malloc(sizeof(EdgeNode));
    e1->edge_vex = 1;   // 邻接顶点是V1
    e1->weight = 2;     // 边的权重（活动持续时间）为2
    e1->next = (*ALG)->adj_list[0].head; // 新边的next指向原头指针
    (*ALG)->adj_list[0].head = e1;       // 更新V0的边链表头指针
    (*ALG)->adj_list[1].in++;            // V1的入度加1（V0->V1是入边）

    // 示例：添加边 V0 -> V2，权重为3
    EdgeNode *e2 = (EdgeNode *)malloc(sizeof(EdgeNode));
    e2->edge_vex = 2;
    e2->weight = 3;
    e2->next = (*ALG)->adj_list[0].head;
    (*ALG)->adj_list[0].head = e2;
    (*ALG)->adj_list[2].in++;

    // 其他边的添加...（需根据实际AOE网的结构补充）
    // 如 V1->V3、V2->V3 等边，同时维护对应顶点的入度
}

int main()
{
    Adj_List_Graph ALG;     // 定义邻接表图指针
    create_graph(&ALG);     // 创建AOE网
    critical_path(ALG);     // 计算并打印关键路径
    return 0;
}
```





### 4.示例说明（ AOE 网示例）

#### 示例 1：ETV 计算

- 起点 V0 的 ETV = 0。
- V1 的 ETV = ETV [0] + 3 = 3。
- V2 的 ETV = ETV [0] + 4 = 4。
- V3 的 ETV = max (ETV [1]+3, ETV [2]+8) = max (3+3,4+8) = 12（视频中强调 “取最大值”，因需等所有前驱活动完成）。
- 终点 V9 的 ETV = max (ETV [7]+2, ETV [8]+3) = max (17+2,24+3) = 27（视频中终点 ETV 为 27）。

#### 示例 2：LTV 计算

- 终点 V9 的 LTV = ETV [9] = 27。
- V8 的 LTV = LTV [9] - 3 = 24。
- V7 的 LTV = LTV [9] - 2 = 25。
- V5 的 LTV = LTV [8] - 5 = 19。
- V4 的 LTV = LTV [7] - 7 = 18。
- V3 的 LTV = min (ETV [5]-4, ETV [6]-2) = min (19-4,22-2) = 15（视频中强调 “取最小值”，因需不影响后继活动）。

#### 示例 3：关键路径判定

- 关键事件（ETV == LTV）：V0（0=0）、V2（4=4）、V3（12=12）、V5（19=19）、V8（24=24）、V9（27=27）。
- 关键活动（ETV [i]+weight == LTV [j]）：<V0,V2>(4)、<V2,V3>(8)、<V3,V5>(4)、<V5,V8>(5)、<V8,V9>(3)。
- 关键路径：V0→V2→V3→V5→V8→V9（视频中明确的关键路径，总时长 27 天）。
