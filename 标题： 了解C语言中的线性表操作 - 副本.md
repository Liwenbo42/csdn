# C 语言实现动态数组：手写顺序表

在编程中，动态数组是一种非常有用的数据结构，它允许我们在运行时动态添加和删除元素。本篇博客将介绍如何使用 C 语言来实现一个基本的动态数组，也称为顺序表。我们将一步一步介绍代码实现，包括初始化、插入、删除、修改和添加元素等基本操作，具有以下常见操作：

- 初始化：创建一个新的线性表并初始化。
- 插入：将新元素插入到指定位置。
- 删除：删除指定位置的元素。
- 修改：修改指定位置的元素。
- 添加：将新元素添加到线性表的末尾。
- 查看：所有元素。

## 顺序表定义

首先，我们定义一个结构体 `SeqList` 来表示顺序表。这个结构体包括了一个整数数组 `data`，表示存储数据的指针，`length` 表示当前数组中元素的数量，以及 `maxSize` 表示数组的最大容量。

```
struct SeqList {
    int* data;      // 存储数据的指针
    int length;     // 当前数组中元素的数量
    int maxSize;    // 数组的最大容量
};
```

## 初始化顺序表

初始化顺序表的函数 `init`，我们使用 `malloc` 分配内存并将顺序表初始化为初始大小。

```
void init(struct SeqList* list) {
    // 分配内存并初始化数组
    list->data = (int*)malloc(INIT_SIZE * sizeof(int));
    list->length = 0;
    list->maxSize = INIT_SIZE;

    // 填充数组，从 0 到 INIT_SIZE-1
    for (int i = 0; i < INIT_SIZE; i++) {
        list->data[i] = i;
        list->length++;
    }
}
```

## 扩展顺序表大小

如果需要，我们使用 `expand` 函数扩展顺序表的容量。

```
void expand(struct SeqList* list) {
    int* temp = list->data;

    // 分配更大的内存
    list->data = (int*)malloc((list->maxSize + INIT_SIZE) * sizeof(int));

    // 复制数据到新数组
    for (int i = 0; i < list->length; i++) {
        list->data[i] = temp[i];
    }

    // 更新最大容量
    list->maxSize = list->maxSize + INIT_SIZE;

    // 释放旧内存
    free(temp);
}
```

## 插入元素

我们实现了 `insert` 函数，用于在指定位置插入一个元素。

```
void insert(struct SeqList* list, int pos, int val) {
    if (list->maxSize == list->length) {
        expand(list);
    }

    // 从后往前移动元素以腾出位置
    for (int i = list->length; i >= pos - 1; i--) {
        list->data[i + 1] = list->data[i];
    }

    // 插入新值
    list->data[pos - 1] = val;
    list->length++;
}
```

## 删除元素

使用 `remove` 函数可以删除指定位置的元素，并返回被删除的元素值。

```
void remove(struct SeqList* list, int pos, int* removedVal) {
    *removedVal = list->data[pos - 1];

    // 从删除位置开始，将后续元素向前移动一个位置
    for (int i = pos - 1; i < list->length; i++) {
        list->data[i] = list->data[i + 1];
    }

    // 更新数组长度
    list->length--;
}
```

## 修改元素

`modify` 函数允许修改指定位置的元素值。

```
void modify(struct SeqList* list, int pos, int newVal) {
    int prevVal;
    prevVal = list->data[pos - 1];

    // 替换值
    list->data[pos - 1] = newVal;

    // 打印替换前的值
    printf("前值 %d\n", prevVal);
}
```

## 添加元素

使用 `append` 函数可以在顺序表末尾添加一个元素。

```
void append(struct SeqList* list, int val) {
    if (list->length == list->maxSize) {
        int* temp = list->data;

        // 分配更大的内存
        list->data = (int*)malloc((list->maxSize + INIT_SIZE) * sizeof(int));

        // 复制数据到新数组
        for (int i = 0; i < list->length; i++) {
            list->data[i] = temp[i];
        }

        // 更新最大容量
        list->maxSize = list->maxSize + INIT_SIZE;

        // 释放旧内存
        free(temp);
    }

    // 在末尾添加新值
    list->data[list->length] = val;

    // 打印添加的值
    printf("添加值：%d\n", val);
    list->length++;
}
```

## 查看顺序表

最后，我们实现了 `view` 函数，用于查看当前顺序表的内容和状态。

```
void view(struct SeqList* list) {
    printf("当前数据：\n");
    for (int i = 0; i < list->length; i++) {
        printf("%d\n", list->data[i]);
    }
    printf("有 %d 个数\n", list->length);
    printf("空间有 %d\n", list->maxSize);
}
```



完整的示例程序

以下是一个完整的示例程序，展示如何使用上述操作来管理线性表：

```
#include <stdio.h>
#include <stdlib.h>

struct SeqList {
    int* data;
    int length;
    int maxSize;
};

const int INIT_SIZE = 10;
const int EXPAND_FACTOR = 2;

void init(struct SeqList* list) {
    list->data = (int*)malloc(INIT_SIZE * sizeof(int));
    list->length = 0;
    list->maxSize = INIT_SIZE;
    for (int i = 0; i < INIT_SIZE; i++) {
        list->data[i] = i;
        list->length++;
    }
}

void expand(struct SeqList* list) {
    int* temp = list->data;
    list->data = (int*)malloc((list->maxSize + INIT_SIZE) * sizeof(int));
    for (int i = 0; i < list->length; i++) {
        list->data[i] = temp[i];
    }
    list->maxSize = list->maxSize + INIT_SIZE;
    free(temp);
}

void insert(struct SeqList* list, int pos, int val) {
    if (list->maxSize == list->length) {
        expand(list);
    }
    for (int i = list->length; i >= pos - 1; i--) {
        list->data[i + 1] = list->data[i];
    }
    list->data[pos - 1] = val;
    list->length++;
}

void remove(struct SeqList* list, int pos, int* removedVal) {
    *removedVal = list->data[pos - 1];
    for (int i = pos - 1; i < list->length; i++) {
        list->data[i] = list->data[i + 1];
    }
    list->length--;
}

void modify(struct SeqList* list, int pos, int newVal) {
    int prevVal;
    prevVal = list->data[pos - 1];
    list->data[pos - 1] = newVal;
    printf("前值 %d\n", prevVal);
}

void append(struct SeqList* list, int val) {
    if (list->length == list->maxSize) {
        int* temp = list->data;
        list->data = (int*)malloc((list->maxSize + INIT_SIZE) * sizeof(int));
        for (int i = 0; i < list->length; i++) {
            list->data[i] = temp[i];
        }
        list->maxSize = list->maxSize + INIT_SIZE;
        free(temp);
    }

    list->data[list->length] = val;
    printf("添加值：%d\n", val);
    list->length++;
}

void view(struct SeqList* list) {
    printf("当前数据：\n");
    for (int i = 0; i < list->length; i++) {
        printf("%d\n", list->data[i]);
    }
    printf("有 %d 个数\n", list->length);
    printf("空间有 %d\n", list->maxSize);
}

int main() {
    struct SeqList list;
    init(&list);
    view(&list);

    printf("输入插入位置和数值: ");
    int pos, val;
    scanf("%d %d", &pos, &val);
    insert(&list, pos, val);
    view(&list);

    printf("输入删除位置: ");
    int posToRemove, removedVal = 0;
    scanf("%d", &posToRemove);
    remove(&list, posToRemove, &removedVal);
    printf("删除的数值是 %d\n", removedVal);
    view(&list);

    printf("输入修改位置和数值: ");
    int posToModify, newVal;
    scanf("%d %d", &posToModify, &newVal);
    modify(&list, posToModify, newVal);
    view(&list);

    printf("输入添加的数值: ");
    int valToAdd;
    scanf("%d", &valToAdd);
    append(&list, valToAdd);
    view(&list);
}

```



通过本文，我们了解了C语言中线性表的基本操作，包括初始化、插入、删除、修改和扩容。这些操作是数据结构和算法中的基础，可以应用于各种编程问题。未来，您可以进一步探索高级的线性表实现和更多数据结构操作。

结语

在本文中，我们详细介绍了C语言中线性表操作的实现，提供了完整的代码示例和解释。希望本文对您理解线性表以及C语言中的数据结构操作有所帮助。请随时下载示例代码并进行尝试，以加深理解。