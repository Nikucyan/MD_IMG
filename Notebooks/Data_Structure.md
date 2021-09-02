# Data Structure  数据结构

耿国华教授

---

# 1. Introduction 绪论



## Defininition  定义

### Basic Definitions

**Data 数据**：你输入计算机能被处理的符号集合

源程序（C） - （*C 编译程序* ）- 目标程序（obj） - （*C 链接程序* ）- 可执行程序（exe）

**Data Element 数据元素**：组成数据的基本单位（可以由若干数据项 / 单个数据项组合而成）

**Data Object 数据对象**：性质相同的数据元素集合（子集）

**Data Structure 数据结构**：有关系的数据元素的集合，数据元素之间是有结构的归集的

​	树形 / 图 / ……

**Data Type 数据类型**：高级语言间已经实现的数据结构

​	定义域 / 操作集（允许的运算）

​	整型 int （定义域：-32768 / 32767；操作集：+-*/%）（原子型）/ 指针（原子型）

### Abstract Data Type  抽象数据类型 (ADT)

定义数据对象、对象之间各元素的关系、处理数据的操作

特点：数据抽象、信息隐蔽

- 定义：需要包含的信息，根据功能确定界面服务，使用者的操作（独立于实现）
- 物理实现：封装进模块内，使用者只有界面中的服务访问数据

（实例：图书馆借书系统的查阅 / 借还系统）



## Content  内容

### Logic Structure  逻辑结构

数据元素之间的逻辑关系描述，Data_structure = (D, R)，D - 数据元素的有限集，R - D 上关系的有限集

- 集合结构：同属于一个集合无其他关系
- 线性结构：一对一的线性关系（如线性表）
- 树型结构：存在一对多的层次关系（如树）
- 网状结构：存在复杂关系

### Storage Structure  存储结构

逻辑结构（物理结构）在计算机中的实现、存储映像，包含元素的表示和关系的表示

- 顺序：地址连续的空间（逻辑关系可以通过连续空间表示）
- 非顺序：空间可以是任意配置的（逻辑关系需要依靠指针维持）

### Operations  运算集合

插入 / 更新 / 修改 / 查找 / ……



**数据结构的研究内容**：按照一定逻辑关系组织起来的一批数据，按照一定的映像方式存储在计算机中，并在其上定义运算集合



## Method  方法

### Algorithm  算法

规则有限集，为解决特定问题而规定的一系列操作

**特性**：有限性、确定性、输入、输出、可行性



## Tools  工具

算法描述：自然语言 / 框图 / 高级程序语言（细节）

类语言：接近高级语言，撇除语言细节限制（伪语言）



## Performance Evaluation  性能评价

- **时间 T**（影响因素：系统 / 机器 / ……）

  语句执行次数（仅与程序设计 / 算法步骤有关）和执行一次的时间的乘积

  问题规模 N：多项式的项数 / 矩阵阶数 / 顶点数 / ……

  语句频度频度：语句执行的次数（n / n^2^ / ......）

  算法的时间复杂度：$T(n) = O(f(n))$ 

  ​	O(1) - 常量阶；O(n) - 线性阶；O(n^2^)；O(2^n^)；O(log~2~n) ……

  ​	如：$T(n) = 2n^3+2n^2 +n \Rightarrow O(n^3)$ （常数与低次项被忽略）

  最坏时间复杂度：分析最坏情况以估算算法执行时间上界

- **空间 S** - 单元（变量）



## DS with C Lang  数据结构与 C 语言

- Program = Algorithm + Data structure
- Program Structure = Control structure + Data structure

### 结构化程序设计

使程序具有合理结构以保证程序的正确性（用静态的结构保证动态执行）

**基本控制结构：顺序、选择、重复**（单出单入）

​	限制 GOTO 语句（无条件转义语句 - 多入多出 ~ 程序正确性难以阅读和保证）

### 用 C 语言实现 ADT

用结构体组合 int / float 等类型 - 用 typedef 为该类型 / 指针起名

​	typedef：`typedef struct card Card` (新用户定义类型名 "Card"，是类型 “struct card” 的别名)

​	变量 - 空间；类型 - 类别（是类型符号，和 int 等同理）

再用子函数实现各个操作

### 算法描述方法 - 函数

宏定义 `#define ...` （全程序有效）

子函数定义 `type_n fun_n {...}` 函数表1 （仅与函数体中有效）

主函数 `main() {...}` 

（先定义再使用）

**参数传递**问题：变量（只能带出不能带进）/ 地址（指针，可以带进 + 带出）

**函数结果带出的方式**：全程量（是隐含的接口）、函数返回值、传址参数

全局变量方式（隐式传递）、数组方式（多相同类型的值）、结构体方式（多类型不同值）、指针方式（显式传递）



# 2. Linear List  线性表



## Linear List  线性表

### Definition  定义

**定义**：n 个同类元素的有限序列，存在唯一头元素和尾元素，中间其他元素都只有唯一前驱和后继元素

![Linear List](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210719224239274.png) 

**特点**：同一性、有穷性、有序性

### ADT  抽象数据类型定义

`ADT LinearList{}`

- `InitList(L)`	将 L 初始化为空表
- `DestroyList(L)`	将 L 销毁
- `ClearList(L)`	将 L 置为空表
- `EmptyList(L)` 	如果 Ｌ 为空则返回真，否则为假
- `ListLenght(L)`	如果 L 为空则返回 0，否则返回元素个数
- `Localte(L, e)`	如果 L 中存在元素 e，则将 “当前指针” 指向 e 元素所在位置，并返回真，否则则返回假
- `GetData(L, i)`	返回 L 中第 i 个元素的值
- `InsList(L, i, e)`	在 L 的第 i 位插入心的数据元素 e，L 长度 + 1
- `DelList(L, i, &e)`	删除 L 的第 i 个数据元素，并用指针 e 返回其值，L 长度 - 1

## Sequenatial  list  顺序存储

使用的是 c 语言中的数组形式

**优点**：方便查找

**缺点**：插入 / 删除都不方便，所有后续元素都需要移动（性能分析 / 开销），事先需要确定表大小，是静态分配

- 插入时移动次数期望：insert seq $\mathrm{EXP} = \sum_{i=1} ^{n+1} P_i C_i = \frac{1}{n} (n + (n-1) + (n-2) + ... +1) =\frac{n}{2}$​​ （接近一半元素需要移动）
- 删除时移动次数期望：delete seq $\mathrm{EXP} = \sum^n_{i=1} = q_iC_i = \frac{1}{n} ((n-1) + (n-2)+ ... +1 ) = \frac{n-1}{2}$

### 合并线性表（demo）

`LA = [1, 3, 5]` & `LB = [2, 4, 6]`（即两个值有序的线性表）合并为 `LC` 并使其按顺序排列元素：在 `LA` 和 `LB` 中各设置指针 `i`, `j`，`LC` 中 `k`，将 `LC` 作为 `LA` 的扩充

``` c
void merge(SeqList *LA, SeqList *LB, SeqList *LC)
{
    int i, j, k, l;
    i = 0; j = 0; k = 0;
    // while LA and LB both not complete
    while(i <= LA -> last && j <= LB -> last)	// LA->last == (*LA).last, 
        if(LA -> elem[i] <= LB -> elem[j])		// call 'last' from the struct that 'LA' points to 
        {
            LC -> elem[k] = LA -> elem[i];
            i++; k++;
        }
    	else {
            LC -> elem[k] = LB -> elem[j];
            j++; k++;
        }
    while(i <= LA -> last)	// if List LA longer than LB, put the rest elements in LA into LC directly
    {
        LC -> elem[k] = LA -> elem[i];
        i++; k++;
    }
    while(j <= LB -> last) {
        LC -> elem[k] = LB -> elem[j];
        j++; k++;
    }
    LC -> last = LA -> last + LB -> last + 1;
}
```

## Linked List  链式存储

使用 c 语言中的指针

链表：采用链式存储结构的线性表，可以是任意配置地址不连续的

- 实现方式：动态 / 静态
- 链接方式：单链表 / 循环链表 / 双链表

### Singly Linked List  单链表

又称线性链表（只有一个指针域）

节点结构：数据域 + 指针域

``` c
typedef struct Node		// 存储结构描述
{   ElemType data;		// 定义了数据
    struct Node *next; 	// next：具有这种类型的结构的形式（定义了指向这种类型的指针）
} Node, *LinkList
```

**头指针** H：指向链表头结点的指针（在第一个元素结点（首元节点 A）前设置了头结点，无数据 - 简化运算）

（表示方式：带头结点的单链表 H） 

<img src="https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210728161358756.png" alt="Linked List" style="zoom:50%;" /> 

F 后无后继：使用空

指针：如：从 H 指向 A 的数据：H -> next -> data；从 H 指向 B：H -> next -> next -> data

#### 基本运算

##### 单链表长度（demo）

即求表中的元素个数（第 n 个，但由于为动态结构，是任意配置的单元，关系仅有指针维持，非显式表示）

只能从第一个往后沿着表数

由于这个原因，不能使用 `for i = 1, i <= n` 的循环

`linklength(L)`：L 其实就是这个链表的头指针

**Demo Code**:

```c 
int Listlength(LinkList L) 	//求带头结点单链表 L 的长度
{
    Node *p;
    p = L -> next;	// 首元节点的地址（指针）
    j = 0; 		// 用来存放其长度
    while(p != NULL) 	//表没处理完
    {
        p = p -> next;	// p 为当前位置指示器，如果 p 到了最后一项，-> next 会超过链表，p == NULL
        j++;
    }
    return j;
}	// 如果是空表，p = L -> next == NULL，直接结束，j == 0
```

##### 建立单链表

###### 尾插入（demo）

动态申请 - 组合的结构，是表尾指示器（把尾指针和新申请的 s 连接）指向新的地址

``` c
Linklist CreateFromTail(LinkList L) {
    LinkList L; Node *r *s;
    int flag = 1;	// 设置一个标志，初始值为 1，当输入 "$"，flag 为 0，结束（0 结束，1 未结束）
    r = L;	 // *r 指针时终动态指向链表的当前表尾，以便尾插入，初值指向头结点
    while(flag) {
        c = getchar(); 	// 读取一个 c
        if(c != '$') {	// 不等于特殊的结束符号 "$"
            s = (Node*)malloc(sizeof(Node));	// 申请新节点，地址为 s，规格：data + pointer 
            s -> data = c; r -> next = s;		// s 的数据写入的是刚读的 c；把新申请的 s 作为上一个 r -> next
            r = s;		// 更新 r 作为新的表尾，准备开始下一个循环
        }
        else {
            flag = 0;	// 读取到 "$" 字符，标志结束
            r -> next = NULL;	//把最后一个结点的 next 链域置为空，表示链表结束
        }
    }	// while
}
```

###### 头插入（demo）

最先输入 a~n~ 再插入 a~n-1~ 于其前方……（时终插入于头结点 L 之后）

``` c
Linklist CreateFromTail(LinkList L) {
    LinkList L; Node *s; char c;
    int flag = 1;	
    while(flag) {
        c = getchar();
        if(c != '$') {
            s = (Node*)malloc(sizeof(Node));
            s -> data = c; 	// 先申请了 s 结点，并给（第一次 an）赋值
            s -> next = L -> next; 	// 新的 s -> next 和头结点 L 的 -> next 指的一样（即上一次循环产生的结点）
            // （第一次 L -> next 是空则 s -> next 也是空）
            L -> next = s;	// 更新 L 的头结点的位置，指向本次产生的新 s
        }
        else flag = 0;
    }
```

##### 单链表查找

链表特点：只能顺链查找

###### 按序号找（demo）

思路：查找表 L	-	顺链找（设置当前位置的指针 p）  -	计数器（j = 0）  -	走一个点让计数值 + 1

``` c
Node *Get(LinkList L, int i)
{ 	// 查找带头结点的单链表 L 中第 i 个结点，如果找到 (1 <= i <= n) 则返回该节点的存储位置，否则 NULL
    int j; Node *p;	// 顺链找 & 计数器 j
    p = L; j = 0; 	// 从头结点开始扫描
    while ((p -> next != NULL) && (j < i))	{ 	// 并列条件
        p = p -> next; 	// 扫描下一个结点
        j++;	// 计数器 +1
    }
    if (i == j)
        return p;	// 找到了第 i 个结点
    else return NULL;	// 找不到，i <= 0 或者 i > n
}
```

###### 按内容找（demo）

找结点值等于需要的值的结点

``` c
Node *Locate(LinkList L, ElemType key)
{ 	// Find the node whose data equal to node 'key', if found return location p, else return NULL
    Node *p;
    p = L -> next;	// compare from the first node in the list
    while (p != NULL)	// not finish
        if(p -> data != key)
            p = p -> next; 
    	else break;		// find node 'key', break the loop
	return p;
}	// if not found finally, return NULL
```

##### 单链表的插入 / 删除

改链 / 挂链操作：（插入位置：i 前，i-1 后）`s -> next = p -> next` & `p -> next = s -> next`

```  c
void InsList(LinkList L, int i, ElemType e)
{
    Node *pre, *s; 	// 'pre' for node before insertion; 's' for the new node
    int k;
    pre = L; k = 0;
    while(pre != NULL && k < i-1)	// (this step find the i-1 -th elem)
    {	// insert before i-th elem, find i-1 location and point 'pre' to this node 
        pre = pre -> next;
        k = k + 1;
    }
    if(!pre)
    {	// if the now 'pre' location is null, means code finish but not reach i-th, this i is not valid
        printf("Not valid!");
        return ERROR;
    }
    s = (Node*)malloc(sizeof(Node));	// Generate new node and point s to it
    s -> data = e;	
    s -> next = pre -> next;
    pre -> next = s;	// re-point 'pre' to 's'
    return OK;
}
```

删除操作逻辑相似，删除 a~i~ (r) 前也需要先找 pre（a~i-1~）并将其指向 a~i~ 的 `-> next` ，后续将删除的元素保存到指针变量 *e 中

(In code: `r = p -> next;` `p -> next = p -> next -> next;` (or `p = r -> next`) `*e = r -> data`)

对边界情况也适用

挂链结束后，将被删除的结点所占用的空间释放：`free(r);`

#### 求两个集合的差

集合 A、B 使用链表表示，集合的差即为属于 A 而不属于 B 的元素（A-B）

思路：对于 A 的所有元素在 B 中查找，存在则在 A 表中删除（A 表的条件删除 + B 表的查找）

``` c
void Difference(LinkList LA, LinkList LB)
{
    Node *pre, *p, *q, *r;
    pre = LA; p = LA -> next;
    while (p != NULL) {	 // A list not finished		// Location: p for LA, q for LB
        q = LB -> next;	 // test all B
        
        // B not finished && The selected elem in A != ... in B
        while (q != NULL && q -> data != p -> data)	 
            q = q -> next;	// (not found equal) find next q in B (skip)
        
        // if found in B and not finished (end while loop above and ...)
        if (q != NULL)	
        {	// actually equivalent to deletion for the corresponding elem in A (LA)
            r = p;
            pre -> next = p -> next;
            p = p -> next;
            free(r);
        }
        
        else {	// not found in B and B finished, remains in A
            pre = p;
            p = p -> next; }	
    }
}
```

### Circular Linked List  循环链表

属于单链表，表尾指向表头（首尾相接），即最后一个指针 -> NULL 改为 -> 头结点 / 第一个结点

判定当前结点是否为表尾需要改变（原来是 P = NULL）：if P -> next = L

#### 循环链表的合并

合并 LA、LB 并把 LA 尾指向 LB 头，LB 尾指向 LA

![Cirucular List Combination Original](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210811110308440.png) ![Circular List Combination 1](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210811110747878.png) <img src="https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210811111550332.png" alt="Circular List Combination 2" style="zoom:60%;" />

``` c
LinkList merge_1 (LinkList LA, LinkList LB)
{
    Node *p, *q; p = LA; q = LB;
    while (p -> next != LA)
        p = p -> next;
    while (q -> next != LB)
        q = q -> next;		// place p and q at the end node at LA and LB
    q -> next = LA;			// link the end node of LB to the head node of LA
    p -> next = LB -> next;	 // link the end node of LA to the first node (b1) not LB
    free(LB);
    return(LA);
}
```

OR

```c
LinkList merge_2(LinkList RA, LinkList RB)
{
    Node *p; 
    p = RA -> next;		// Actually p is now the head node of RA
    // Place RA as the node of a_n (end node pointer); RA -> next point to b1
    RA -> next = RB -> next -> next;  // RA -> next: head node of RA; RB -> next -> next: b1 
    free (RB -> next);
    RB -> next = p; 	// link b_n -> next = head node of RA
    return RB
}
```

### Doubly Linked List  双链表

或**双向链表**：指针的节点结构（指针域）可以有两个指针，一个向前一个向后（可以做成双向循环链表）

如果经常需要前后移动的链表用双向列表更方便，但是占用多了一个指针位置，数据存储密度降低

#### 插入运算

![Insertion in Doubly Linked List](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210811112937839.png) 

``` c
void DlinkIns(DoubleList L, int i, ElemType c)	
{	// the inserted data is c, at position i in the double linked list
    DNode *s, *p;
    // ... need to check if position i valid; if valid, point *p -> this position ...
    s = (DNode*)malloc(sizeof(DNode));
    if (s)  {	// s: the new node; p: the current node at i (after to be i+1)
        s -> data = c;
        // MAIN Algorithm
        s -> prior = p -> prior;	// s' prior points to (i-1) node (containing a)
        p -> prior -> next = s;		// p prior node (i-1)'s next point to new node s
        s -> next = p;
        p -> prior = s;
        return TRUE;
    }
    else return FALSE;
}
```

#### 删除结点

``` c
int DLinkDel (DoubleList L, int i, ElemType *e)
{
    DNode *p; 
    // check i valid .... and point p to i position ...
    *e = p -> data;
    p -> prior -> next = p -> next;
    p -> next -> prior = p -> prior; 	// point the p node's prior and next ...
    free (p);
    return TRUE;
}
```

### * Static Linked List  静态链表

在没有指针类型的语言中用数组的方式模拟实现的链表（数组是事先申请的，没有动态申请 / 删除的操作）

申请较大的结构数组，包含：数据 Data（ElemType） + 指针 Cursor（int）

``` c
typedef struct{
    ElemType Data;
    int cursor;
} Component,
StaticList[Maxsize];
```

![Static Linked List Array](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210811114256306.png) 

#### Initialization 初始化

``` c
void initial (StaticList space, int *av)	// *av 即为指示器（指针）
{
    int k;
    space[0] -> cursor = -1; 	// 作为首结点，-1：空表
    for (k = 1; k < Maxsize -1; k++)
        space[k] -> cursor = k+1;
    space[Maxsize - 1] = -1;	// 当跳出循环 将最后的结点也设为 -1
    *av = 1;
}
```

形成一个备用的静态链表

#### 申请结点

``` c
int getnode(StaticList space, int *av)
{
    int i; i = *av;
    *av = space[*av].cur;	// 相当于申请得到了 *av 的结点，系统中减少了一个结点，*av 获得的是相应 ”结点“ 的 cursor
    return i;	// i 即为这个 *av 所指向的地址
}
```

对于系统而言是在备用链表中减少了一个结点（模拟了申请结点的过程）

#### 结点回收

``` c
void freenode(StaticList space, int *av, int k)
{
    space[k].cursor = *av;	// 结点序号是 k
    *av = k;	// 备用列表头指针 *av（相当于头插入了 k 在备用链表 *av 前）
}
```

对系统而言在备用列表中增加了一个结点

## First Order Polynomials  一元多项式的表示与运算

$P_n(x) = P_0 + P_1x + P_2x^2 + \cdot \cdot \cdot + P_nx^n$

- **顺序表**：

  - 只存储各项的系数，存储位置下标对应其指数项 ~ 适于非零系数多的多项式
  - 系数和指数都存入顺序表 ~ 适用于零系数多的

- **单链表**：[ 系数 | 指数 | 指向下一结点 ]

  ``` c
  typedef struct Polynode {
      int coef; int exp; Polynode *next;
  }	Polynode, *Polylist;
  ```

  动态结构，更适合于元素动态变化（更合适）

### 建立多项式链表

需要按指数升幂排列，尾插法

``` c
Polylist polycreate() {
    Polynode *head, *rear, *s;
    int c, e;
    h = (Polynode*)malloc(sizeof(Polynode));	// create the head node of the polynomial
    rear = head; 	// rear always points to the end of the single linked list
    scanf("%d, %d, &c, &e");	// input the coefficient and the power of the polynomial
    while (c != 0)  {   // if c = 0, the input of the polynomial is finished
        s = (Polynode*)malloc(sizeof(Polynode));
        s -> coef = c;
        s -> exp = e;
        rear -> next = s;	// 尾插入
        rear = s;	// 尾指针移到新结点
        scanf("%d, %d, &c, &e"); 
    }
    rear -> next = NULL; 	// end (-> next = NULL)
    return (head);
}
```

### 合并两个多项式单链表

- 指数不同：直接复制
  - `p -> exp < q -> exp`：p 所指为 “和多项式” 中一项，指针 p 后移
  - `p -> exp > q -> exp`：q 所指为 “和多项式” 中一项，结点 q 需插入至结点 p 之前，q 在原来链表后移
- 指数相同：需要相加；
  - 和不为零，修改 p 的系数域，释放 q 结点
  - 和为 0，需要从 LA 删除 p 结点，同时释放 p、q 结点

``` c
void polyadd(Polylist polya, Polylist polyb) {
    Polynode *p, *q, *tail, *temp;
    int sum;
    p = polya -> next; q = polyb -> next;	// now proceeding position
    tail = polya;	// list tail indicator (end of the expected list)
    while (p != NULL && q != NULL) {
        if (p -> exp < q -> exp) {	
            tail -> next = p; tail = p;	p = p -> next;
        }
        else if (p -> exp == q -> exp) {
            sum = p -> coef + q -> coef;	// sum up coefficients
            if (sum != 0) {		// check if sum = 0: yes -> free both; no -> add one to another
                p -> coef = sum;	// change the coefficient to the sum of the 2 coefficients
                tail -> next = p; tail = p;
                p = p -> next;
                temp = q; q = q -> next; free(temp);
            }
            else {	// delete both p and q (coeff. sum = 0)
                temp = p; p = p -> next; free(temp);
                temp = q; q = q -> next; free(temp);
            } 
        }
        else {
            tail -> next = q; tail = q;
            q = q -> next;
        }     
    }
    if (p != NULL)	// break the while loop, check which list is still not finished
        tail -> next = p;	// link all rest to the tail
    else 
        tail -> next = q;
}
```

e.g.	`polya`: $7 + 3x + 9x^8 + 5x ^{17}$;	`polyb`: $8x + 22x ^7 -9x^8$	Result: $7 + 11x + 22x^7 + 5x^{17}$

![Polynomial A](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210811155412818.png) 

![Polynomial B](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210811155425381.png) 

## Comparison of Linear and Linked Lists  比较

### 考虑方式

#### 基于空间考虑

- 顺序表：存储空间**静态分配**，必须在**执行前**明确规定其**规模**
- 静态链表：初始存储池也是**静态分配**，但当同时存在多个结点类型一致的链表时可以共享存储空间
- 动态链表：**动态分配**，只要内存空间有空闲就不会溢出

**存储密度** (Storage Density)：顺序表：一一对应，存储密度大（1）；链表：密度小于 1

#### 基于时间考虑

- 顺序表：随机存取，$O(1)$，适合多查找，少插入和删除的场景
- 链表：只需要修改指针就可以插入和删除（如果插入和删除主要在表两端，适合采用尾指针表示的单循环链表）

#### 基于语言考虑

考虑指针 -> 静态链表

### 链式存储方式比较

|                          | 找首元素结点        | 找尾元素结点         | 找 P 结点前驱结点                            |
| ------------------------ | ------------------- | -------------------- | -------------------------------------------- |
| 带头结点单链表 L         | `L -> next`, $O(1)$ | 一重循环，$O(n)$     | 顺 P 结点的 next 域无法找到 P 的前驱         |
| 带头结点循环单链表 L     | `L -> next`, $O(1)$ | 一重循环，$O(n)$     | 顺 P 结点的 next 域可以找到 P 的前驱，$O(n)$ |
| 带尾指针循环单链表 R     | `R -> next`, $O(1)$ | `R`, $O(1)$          | 顺 P 结点的 next 域可以找到 P 的前驱，$O(n)$ |
| 带头结点双向循环单链表 L | `L -> next`, $O(1)$ | `L -> prior`，$O(1)$ | `P -> prior`, $O(1)$                         |

## Example Problems  例题

1. L 中数据元素为 int 型，设计算法分为左右两部分：左（前）均为奇数，右（后）均为偶数。要求时间 $O(n)$；空间 $O(1)$

   *（时间 $O(n)$：即为需要单循环链表，不管怎么找都只需要找一圈）*

   ``` c
   AdjustSqlist (SeqList *L)
   {
       int i = 0, j = L -> last; 	// i, j 分别在线性表两端
       while(i < j) {
           while(L -> elem[i] % 2 != 0)	// 奇数，i++，遇到非奇数则跳出 while loop
               i++;
           while(L -> elem[j] % 2 == 0)	// 偶数，j--
               j--;
           if(i < j) {		// 两变量交换内容（引入第三变量）
               t = L -> elem[i];
               L -> elem[i] = L -> elem[j];
               L -> elem[j] = t;
           }
       }
   }
   ```

2. 单链表的就地逆置（不申请额外空间）a~1~ -> a~n~ => a~n~ -> a~1~

   单链表的头插入法与指针保留

   ``` c
   void ReverseList (LinkList L) {
       p = L -> next;	// 记录原表第一个元素结点位置 p
       L -> next = NULL;	// 将原来的头结点的 next 设置为 NULL，即设置结果表为空表
       while (p != NULL) {	// 讲原表结点摘下并放入新表
           q = p -> next; 	// 保留指针技术
           p -> next = L -> next; L -> next = p;	// 头插入法，挂入结果链
           p = q;	// 把刚刚保留的指针还回去
       }
   }
   ```

3. 二进制加一问题（线性链表）

   建链表（尾插入）-> 二进制 +1 运算原则（找最后一个 0，将其变为 1，其后所有结点值赋为 0；如链表中无 0，则各位都是 1，头插入新结点，后续都改为 0

   找最后的 0：需要顺链走，每次遇到 0，都需要保留，知道 `-> next = NULL`

   ```c
   void BinAdd (LinkList l) {
       Node *q, *r, *temp, *s;
       q = l -> next;
       r = l;
       while (q != NULL) {	// 找最后一个值域为 0 的结点
           if (q -> data == 0)
               r = q;	// 对于找到的每个 0，保留指针 r，先赋上首元结点
           q = q -> next;
       }
       if (r != 1)	// 有 0 结点
           r -> data = 1;	// 把该结点值域设为 1
       else {		// 无 0 结点，需要申请新结点
           temp = r -> next;	// 此时 r 还在头结点 = l；temp = l -> next 即首元结点
           s = (Node*)malloc(sizeof(Node));
           s -> data = 1;
           s -> next = temp;
           r -> next = s;
           r = s;
       }
       r = r -> next;
       while (r != NULL) {	// 用 r 指针把后续所有结点变为 0
           r -> data = 0;
           r = r -> next;
       }
   }
   ```

   

# 3. Stack & Queue  栈和队列

**限定的线性表**：限制线性表插入和删除等运算的位置（仅允许在端点的位置操作）

## Stack  栈

### Definition  栈的定义 

- **栈的定义**：仅允许插入 / 删除运算在线性表的一端。允许进行运算的一端（表尾 a~n~）：**栈顶（Top）**；不允许运算的一端：**栈底（Bottom）**；栈顶位置动态变化，需要设置栈顶指示器。当栈没有元素时为**空栈**。

- 常见运算：**进栈 / 入栈（表尾插入）**；**出栈 / 退栈（表尾删除）**

- 特性：**后进先出（LIFO / Last in Fisrt out）**

  ![image-20210826165912977](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210826165912977.png) 

### ADT  抽象数据类型定义

- 数据元素：任意数据类型，但必须属于**同一个数据对象**
- 关系：栈中元素为**线性关系**
- 基本操作：`InitStack(S)`; `ClearStack(S)`; `IsEmpty(S)`; `IsFull(S)`; `Push(S, x)`; `Pop(S, x)`; `GetTop(S, x)`

### 表示和实现

基本存储结构：顺序（顺序栈） & 链式（链栈）

#### Sequential Stack  顺序栈

用一组连续的存储单元（数组）依次存放自栈底至栈顶的元素（开辟长度）；设一个位置指针 top（栈顶指针）动态指示使用长度；top = -1 为空栈

``` c
#define TRUE 1
#define FALSE 0
#define Stack_Size 50	// 开辟长度（尺寸） m
typedef struct {
    StactElementType elem[Stack_Size];	// 定义为栈中元素类型即可
    int top;	// top 指示器（实际使用情况）
}	SeqStack
```

##### Basic Operations 基本操作

- **初始化**：

  ``` c
  void InitStack(SeqStack *S) { // generate empty stack S
      S -> top = -1;
  } 
  ```

- **判断空栈**：

  ``` c
  int IsEmpty(SeqStack *S) {	// if empty return TRUE
      return (S -> top == -1? TRUE:FALSE);
  }
  ```

- **判断满栈**：

  ``` c
  int IsFull(SeqStack *S) {
      return (S -> top == Stack_Size? TRUE:FALSE)
  }
  ```

- **Push  进栈**

   ![image-20210826170638322](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210826170638322.png) 

  ```c
  int Push(SeqStack *S, StackElementType x) {	// insert an elem with value x on the top of stack S
      if (S -> top == Stack_Size)
          return (FALSE);	// Stack is full (Prevent overflow)
      S -> top ++;
      S -> elem[S -> top] = x;
      return (TRUE);
  }
  ```

- **Pop  出栈**

  检查是否为空栈，避免下溢

  ``` c
  int Pop(SeqStack *S, StackElementType *x) {	// pop the top element out and store in some x pointed space
      if (S -> top == -1) // empty
          return (FALSE);
      else {
          *x = S -> elem[S->top];
          S -> top --;	// modify the top pointer
          return (TRUE);
      }
  }
  ```


#### Double-ended Sequential Stack  两栈共享（双端栈）

两栈申请一个共享的一维数组空间 S[M] 并将栈底分别放在数组两端，分别为 0 和 M-1（栈底位置不变，栈顶位置动态变化，只要公用区间还有空余都可以用）

``` c
#define M 100
typedef struct 
{
    StackElementType Stack[M];
    StackElementType top[2];
    // top[0] and top[1] are the 2 top indicators
} DqStack;
```

![image-20210902172521397](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210902172521397.png) 

##### Basic Operations  基本操作

- **Initializaion 初始化**

  ``` c
  void InitStack(DqStack *S)
  {
      S -> top[0] = -1;
      S -> top[1] = M;
  }
  ```

- **Push  进栈**

  需要先**判断满栈（上溢）**（两栈栈顶相邻）

  ``` c
  int Push(DqStack *S, StackElementType x, int i){	// i is the stack num
      if (S->top[0] + 1 == S->top[1])	// Full Stack
          returen(FALSE);
      switch(i) {
          case 0: S -> top[0]++; S -> Stack[S->top[0]] = x; break;
          case 1: S -> top[1]--; S -> Stack[S->top[1]] = x; break;
          default: return(FALSE);
      }
      return(TRUE);    
  }
  ```

- **Pop  出栈**

  需要**判别是否产生下溢**（分别判别 0 和 1 的栈）

  ``` c
  int Pop(DqStack *S, StackElementType *x, int i) {
      switch(i) {
          case 0: if(S->top[0] == -1) return(FALSE);
              *x = S->Stack[S->top[0]]; S->top[0]--; break;
          case 1: if(S->top[1] == M) return(FALSE);
              *x = S->Stack[S->top[1]]; S->top[1]++; break;
          default: return(FALSE);
      }
  	return(TRUE);
  }
  ```
  
  



