### C++排序算法详解与实例 - 快速排序篇

在本文中，我们将深入探讨C++中的快速排序算法，并提供一个简单的实例来演示其原理和实现。快速排序是一种高效的排序算法，通过分治法将问题拆分为小问题，然后递归地解决这些小问题。

#### 快速排序简介

快速排序是一种基于比较的排序算法，其基本思想是选择一个基准元素，将数组分为两个子数组，其中一个子数组的所有元素小于基准元素，另一个子数组的所有元素大于基准元素。然后，对这两个子数组递归地进行排序。

```
#include<iostream>
using namespace std;

// 打印数组
void printArray(int array[]) {
    for(int i = 0; i <= 9; i++) {
        cout << array[i] << "  ";
    }
    cout << endl;
}

// 快速排序核心算法，通过递归将数组分为较小的子数组，并排序。
void quickSort(int array[], int left, int right) {
    if(left >= right) return;

    int low = left, high = right;
    int pivot = array[left];

    while(low < high) {
        // 从右向左找第一个小于基准值的元素
        while(low < high && array[high] > pivot) {
            high--;
        }
        if(low < high) array[low++] = array[high];

        // 从左向右找第一个大于基准值的元素
        while(low < high && array[low] < pivot) {
            low++;
        }
        if(low < high) array[high--] = array[low];
    }
    
    // 基准值归位
    array[low] = pivot;

    // 递归调用，分别对左右两部分进行排序
    quickSort(array, left, low);
    quickSort(array, low + 1, right);
}

int main() {
    int x;
    int array[10] = {2, 6, 4, 8, 9, 1, 3, 5, 7};
    int left = 0, right = 9;
    
    // 打印原始数组
    cout << "Original Array: ";
    printArray(array);

    // 调用快速排序算法
    quickSort(array, left, right);

    // 打印排序后数组
    cout << "Sorted Array: ";
    printArray(array);

    return 0;
}

```



下面我们基于快速排序写一个简单sort

### 简单Sort：

自定义排序允许我们定义排序的规则，而不仅仅是按照默认的升序或降序排列。在C++中，我们可以使用函数指针或函数对象来定义自定义比较函数

```
#include<iostream>
#include<algorithm>
#include<functional>
using namespace std;

#define BEGIN_NAMESPACE(x) namespace x {
#define END_NAMESPACE(x) }

BEGIN_NAMESPACE(zhu)

// 比较函数，用于降序排序
bool descendingCompare(int a, int b) {
    return a > b;
}

// 比较函数，用于升序排序
bool ascendingCompare(int a, int b) {
    return a < b;
}

// 显示数组元素
void displayArray(int *first, int *last, const char *label) {
    cout << label << endl;
    for(int *i = first; i < last; i++) {
        cout << *i << " ";
    }
    cout << endl;
}

// 自定义排序算法
void customSort(int *first, int *last, function<bool(int, int)> compareFunction) {
    if(first >= last) return;
    int *low = first, *high = last - 1;
    int pivot = *first;

    while(low < high) {
        // 从右向左找第一个小于基准值的元素
        while(low < high && compareFunction(pivot, *high)) {
            --high;
        }
        if(low < high) *low = *high;

        // 从左向右找第一个大于基准值的元素
        while(low < high && compareFunction(*low, pivot)) {
            low++;
        }
        if(low < high) *high = *low;
    }

    // 基准值归位
    *low = pivot;

    // 递归调用，分别对左右两部分进行排序
    customSort(first, low, compareFunction);
    customSort(low + 1, last, compareFunction);
}

int main() {
    int x, a[100];
    cin >> x;
    for(int i = 0; i < x; i++) {
        cin >> a[i];
    }

    // 使用STL的sort函数进行降序排序
    sort(a, a + x, descendingCompare);
    displayArray(a, a + x, "Descending Order");

    // 使用自定义排序算法进行升序排序
    customSort(a, a + x, ascendingCompare);
    displayArray(a, a + x, "Ascending Order");

    return 0;
}

END_NAMESPACE(zhu)

int main() {
    // 调用zhu命名空间下的main函数
    zhu::main();
    return 0;
}
```

通过这个实例，我们展示了如何使用自定义比较函数来控制排序的方式，从而更灵活地满足不同的排序需求。

#### 结论

自定义排序是在C++中非常有用的功能，它使得我们能够按照自己的规则对数据进行排序。通过灵活运用这一特性，我们可以更好地适应不同的业务场景和排序需求。

在实际写作中，你可以进一步扩展每个部分，添加更多的细节和实例，以确保读者对算法和代码的理解更加深入。