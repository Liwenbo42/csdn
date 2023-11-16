# C++ 栈数据结构的实现与用法

栈（Stack）是一种常用的数据结构，它基于"后进先出"（Last In, First Out）原则，常用于解决各种问题，如表达式求值、函数调用栈、括号匹配等。在本文中，我们将介绍两种不同的栈数据结构的实现，分别是基于数组的栈和基于链表的栈。我们将分别讨论它们的代码实现和用法示例。

## 基于数组的栈

首先，让我们看看基于数组的栈的实现。以下是相关代码的详细解释以及用法示例。

### 数据结构定义

```
typedef struct {
	int Data[Maxsize];
	int topIdx;
} StackNode;
```

这段代码定义了一个栈的结构体，其中包括一个整数数组 `Data` 用于存储栈元素，以及 `topIdx` 用于跟踪栈顶元素的索引。

### 初始化栈

```
void InitStack(StackNode &L){
	L.topIdx = -1;
}
```

`InitStack` 函数用于初始化栈，将 `topIdx` 设置为-1，表示栈为空。

### 入栈

```
void Push(StackNode &L, int a){
	int shu;
	while(a-- && L.topIdx < Maxsize){
		scanf("%d", &shu);
		L.Data[++L.topIdx] = shu;
	}
	if(L.topIdx == Maxsize - 1){
		printf("栈满"); 
	}
}
```

`Push` 函数用于入栈，它允许一次性将多个元素入栈，并会检查栈是否已满。

### 出栈

```
void Pop(StackNode &L){
	if(pan(L)){
		printf("栈空"); 
	}
	int x = L.Data[L.topIdx--];
	printf("出栈元素是：%d",x);
}
```

`Pop` 函数用于出栈，它会检查栈是否为空，然后弹出栈顶元素。

### 获取栈顶元素

```
void Gettop(StackNode L){
	if(pan(L)){
		printf("栈空"); 
	}
	int x = L.Data[L.topIdx];
	printf("栈顶元素是：%d",x);
}
```

`Gettop` 函数用于获取栈顶元素。

### 示例代码

```
int main(){
	StackNode L;
	InitStack(L);
	int a;
	scanf("%d",&a);
	printf("初始化输入位数和数");
	Push(L,a);
	PrintStack(L);
	Gettop(L);
	PrintStack(L);
	Pop(L);
	PrintStack(L);
	ClearStack(L);
	PrintStack(L);
	return 0; 	
}
```

在示例代码中，我们演示了如何初始化栈、入栈、出栈、获取栈顶元素以及清空栈的操作。

### 顺序栈完整代码：

```
#include<bits/stdc++.h>
#include<stdio.h>

using namespace std;
#define Maxsize 11

typedef struct {
	int Data[Maxsize];
	int topIdx;
}StackNode;

void InitStack(StackNode &L){
	L.topIdx = -1;
}

bool pan(StackNode L){
	if(L.topIdx == -1)
	return true;
	else 
	return false;
}

void Push(StackNode &L, int a){
	int shu;
	while(a-- && L.topIdx < Maxsize){
		scanf("%d", &shu);
		L.Data[++L.topIdx] = shu;
//		L.topIdx++;
	}
	if(L.topIdx == Maxsize - 1){
		printf("栈满"); 
	}
}

void Pop(StackNode &L){
	if(pan(L)){
		printf("栈空"); 
	}
	int x = L.Data[L.topIdx--];
	printf("出栈元素是：%d",x);
}

void Gettop(StackNode L){
	if(pan(L)){
		printf("栈空"); 
	}
	int x = L.Data[L.topIdx];
	printf("栈顶元素是：%d",x);
}

void PrintStack(StackNode L) {
    if (pan(L)) {
        printf("栈空");
        return;
    }
    
    printf("栈内元素为: ");
    for (int i = 0; i <= L.topIdx; i++) {
        printf("%d ", L.Data[i]);
    }
    printf("\n");
}

void ClearStack(StackNode &L) {
    L.topIdx = -1; // 将栈顶指针重置为-1，表示栈为空
}
 
int main(){
	StackNode L;
	InitStack(L);
	int a;
	scanf("%d",&a);
	printf("初始化输入位数和数");
	Push(L,a);
	PrintStack(L);
	Gettop(L);
	PrintStack(L);
	Pop(L);
	PrintStack(L);
	ClearStack(L);
	PrintStack(L);
	return 0; 	
} 
```





## 基于链表的栈

接下来，让我们探讨基于链表的栈的实现。以下是相关代码的详细解释以及用法示例。

### 数据结构定义

```
#define MAXSIZE 100
typedef int SElemType;

typedef struct StackNode
{
	SElemType data;
	struct StackNode *next;
} StackNode, *LinkStack;
```

这段代码定义了链栈的数据结构，每个栈元素包括一个数据域 `data` 和一个指向下一个元素的指针 `next`。

### 初始化栈

```
void InitStack(LinkStack &S)
{
	S = NULL;	// 构造一个空栈，栈顶指针置空即可
}
```

`InitStack` 函数用于初始化链栈，将栈顶指针 `S` 置空。

### 入栈

```
int Push(LinkStack &S, SElemType e)
{
	StackNode *p = new StackNode;
	p->data = e;
	p->next = S;
	S = p;
	return 1;
}
```

`Push` 函数用于入栈，将新元素作为链表的头节点，并更新栈顶指针。

### 出栈

```
int Pop(LinkStack &S, SElemType &e)
{
	if (S == NULL) return 0;    // 栈空
	StackNode *p = S;
	e = S->data;
	S = S->next;
	delete p;
	return 1;
}
```

`Pop` 函数用于出栈，它首先检查栈是否为空，然后弹出栈顶元素，释放相关内存。

### 获取栈顶元素

```
SElemType GetTop(LinkStack S)
{
	if (S != NULL)	 // 非栈空时返回
		return S->data;
}
```

`GetTop` 函数用于获取栈顶元素，如果栈非空，它返回栈顶元素的值。

### 示例代码

```
int main()
{
	LinkStack S;
	int e;
	InitStack(S);
	printf("请输入一个要入栈的元素（-1表示结束）：");
	scanf("%d", &e);
	while (e != -1)
	{
		Push(S, e);
		printf("请输入一个要入栈的元素（-1表示结束）：");
		scanf("%d", &e);
	}
	printStack(S);
	printf("出栈测试：");
	Pop(S, e);
	printStack(S);
	printf("取栈顶元素测试：");
	e = GetTop(S);
	printf("取出的栈顶元素为：%d\n", e);
}
```

在示例代码中，我们首先初始化链栈，然后通过循环输入要入栈的元素，最后测试出栈和获取栈顶元素的功能。

### 链栈完整代码：

```
#include<stdio.h>
#include<stdlib.h>

#define MAXSIZE 100
typedef int SElemType;

//链栈的存储结构
typedef struct StackNode
{
	SElemType data;
	struct StackNode *next;
}StackNode,*LinkStack;

//初始化
void InitStack(LinkStack &S)
{
	S = NULL;	//构造一个空栈，栈顶指针置空即可
}

//入栈
int Push(LinkStack &S, SElemType e)
{
	//链栈不需要判断栈满

	StackNode *p = new StackNode;
//	LinkStack p = (LinkStack)malloc(sizeof(StackNode));一样一个是c++一个是c对应的是delete和free 
	p->data = e;
	p->next = S;
	S = p;
	return 1;
}

//出栈
int Pop(LinkStack &S, SElemType &e)
{
	if (S==NULL) return 0;    //栈空

	StackNode *p = S;
	e = S->data;
	S = S->next;
	delete p;
	return 1;
}

//取栈顶元素
SElemType GetTop(LinkStack S)
{
	if (S!=NULL)	 //非栈空时返回
		return S->data;   
}

//遍历输出栈
void printStack(LinkStack S)
{
	printf("栈顶->");
	StackNode *p = S;
	while (p!=NULL)
	{
		printf("%d ",p->data);
		p = p->next;
	}
	printf("\n");
}

int main()
{
	LinkStack S;
	int e;
	InitStack(S);
	printf("请输入一个要入栈的元素（-1表示结束）：");
	scanf("%d",&e);
	while (e!=-1)
	{
		Push(S,e);
		printf("请输入一个要入栈的元素（-1表示结束）：");
		scanf("%d", &e);
	}
	printStack(S);
	printf("出栈测试：");
	Pop(S, e);
	printStack(S);

	printf("取栈顶元素测试：");
	e=GetTop(S);
	printf("取出的栈顶元素为：%d\n",e);
}

```



## 总结

本文介绍了两种不同的栈数据结构的实现方法，分别是基于数组和基于链表的栈。每种栈都有其适用的场景，根据具体需求可以选择合适的实现方式。希望本文能够帮助你更好地理解栈数据结构及其应用。