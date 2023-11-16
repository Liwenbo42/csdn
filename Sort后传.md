# sort的简单实现

## 引言

排序算法是计算机科学中的一个基本问题，有许多不同的实现方法。在本文中，我们将介绍一个基于快速排序和插入排序结合的排序算法的实现一个简单sort。这个算法结合了快速排序的高效性和插入排序的简单性，适用于大部分数据集。

## 算法实现

首先，我们定义了一个命名空间 `zhu`，并在其中实现了比较函数 `cmp`、一个比较类 `CMP` 以及输入输出函数 `input`。接下来，我们实现了快速排序 `_quick_sort` 和插入排序 `insertion_sort`，最后，我们将两者结合在 `Sort` 函数中。

```
include<iostream>
#include<functional>
using namespace std;

#define BEGINS(x) namespace x{
#define ENDS(x)}

BEGINS(zhu)

// 比较函数
bool cmp(int a, int b){
    return a > b;
}	

// 比较类
class CMP{
public:
    bool cmp(int a, int b){
        return a < b;
    }
};

// 输入输出函数
void input(int *first, int *last, const char *g){
    while(first != last){
        cout << *first << " ";
        ++ first;
    }
    cout << endl;
}

// 常量
const int length = 16;
//快排
void _quick_sort(int *first, int *last, function<bool(int, int)> cmp = less<int>()){
    while(last - first > length){
        int *l = first, *r = last - 1;
        int v = *first;
        do{
        while(cmp(v, *l)) ++l;
        while(cmp(*r, v)) --r;
        if(l <= r) swap(*(l++), *(r--));
        }while(l <= r);
         _quick_sort(l, last, cmp);
          last = r + 1;
    }
    return;
}

// 插入排序
void inserion_sort(int *first, int *last, function<bool(int, int)> cmp = less<int>()){
    int *l = first;
    for(int *i = first + 1; i < last; i++){
        if(cmp(*i, *(i - 1))) l = i;
    }
    while(l != first){
        swap(*l, *(l - 1));
        l--;
    }
    for(int *i = first + 2; i < last; i++){
        int *j = i;
        while(cmp(*j, *(j - 1))){
            swap(*j, *(j - 1));
            j--;
        }
    }
    return;
}

// 结合排序
void Sort(int *first, int *last, function<bool(int, int)> cmp = less<int>()){
    _quick_sort(first, last, cmp);
    insertion_sort(first, last, cmp);
}

ENDS(zhu)
```

## 主函数

在 `main` 函数中，我们演示了如何使用我们的排序算法对数组进行排序。用户输入数组的长度和元素，然后调用 `Sort` 函数进行排序，并最终输出结果。

```
int main(){
    int a[100], n;
    cin >> n;
    for(int i = 1; i < n; i++){
        cin >> a[i];
    }
    zhu::Sort(a, a + n, zhu::cmp);
    zhu::input(a, a + n, "cmp:");
    return 0;
}
```

## 完整代码

让我们通过一个简单的例子来演示我们的排序算法。

```
#include<iostream>
#include<functional>
using namespace std;

#define BEGINS(x) namespace x{
#define ENDS(x)}

BEGINS(zhu)

bool cmp(int a, int b){
    return a > b;
}

class CMP{
    public:
    bool cmp(int a, int b){
        return a < b;
    }

};

void input(int *first, int *last,const char *g){
    while(first != last){
        cout << *first << " " ;
        ++ first;
    }
    cout << endl;
}

const int length = 16;
void _quick_sort(int *first, int *last, function<bool(int, int)> cmp = less<int>()){
    while(last - first > length){
        int *l = first, *r = last - 1;
        int v = *first;
        do{
        while(cmp(v, *l)) ++l;
        while(cmp(*r, v)) --r;
        if(l <= r) swap(*(l++), *(r--));
        }while(l <= r);
         _quick_sort(l, last, cmp);
          last = r + 1;
    }
    return;
}

void inserion_sort(int *first, int *last, function<bool(int, int)> cmp = less<int>()){
    int *l = first;
    for(int *i = first + 1; i < last; i++){
        if(cmp(*i, *(i - 1))) l = i;
    }
    while(l != first){
        swap(*l, *(l - 1));
        l--;
    }
    for(int *i = first + 2; i < last; i++){
        int *j = i;
        while(cmp(*j, *(j - 1))){
            swap(*j, *(j - 1));
            j--;
        }
    }
    return;
}
void Sort(int *first, int *last, function<bool(int, int)> cmp = less<int>()){
    _quick_sort(first, last, cmp);
    inserion_sort(first, last, cmp);
    return;

}

int main(){
    int a[100], n;
    cin >> n;
    for(int i = 1; i < n; i++){
        cin >> a[i];
    }
    Sort(a, a + n, cmp);
    input(a, a + n, "cmp:");
    return 0;
}

ENDS(zhu)

int main(){
    zhu::main();
    return 0;
}
```



通过上文，我们深入了解了基于快速排序和插入排序结合的排序算法，并实现了相应的sort代码。这种结合排序的方法在一些情况下可能比单独使用快速排序或插入排序更加高效。读者可以通过阅读代码和运行示例来更好地理解这一排序算法的实现和应用



## Sort优化



当我们编写涉及迭代器的算法时，为了增强代码的灵活性和可扩展性，我们通常会定义一些自定义迭代器。在你提供的代码中，`RandomIter` 类就是一个模拟迭代器的示例

```
class RandomIter{
    public:
    RandomIter(int *ptr) : ptr(ptr){}
    int operator-(const RandomIter &iter) {return ptr - iter.ptr;}
    int &operator*(){return *ptr;}
    RandomIter &operator++(){ ptr++; return *this;}
    RandomIter &operator--(){ ptr--; return *this;}
    RandomIter operator+(int x){return RandomIter(ptr + x);}
    RandomIter operator-(int x){return RandomIter(ptr - x);}
    RandomIter &operator++(int){ptr++; return *this;}
    RandomIter operator--(int){return RandomIter(ptr + 1);}
    bool operator<(const RandomIter &iter) const {
        return ptr < iter.ptr;
    }
    bool operator<=(const RandomIter &iter) const {
        return !(iter < *this);
    }
    bool operator==(const RandomIter &iter) const{
        return !(iter < *this) && !(*this < iter);
    }
    bool operator!=(const RandomIter &iter) const {
        return !(iter == *this);
    }
    private:
    int *ptr;
};
```





## 功能解析

- `BEGINS` 和 `ENDS` 宏用于定义命名空间，本例中命名空间为 `zhu`。
- `RandomIter` 类模拟了迭代器的基本功能，用于表示排序算法中的迭代器。
- `CMP` 类定义了一个仿函数，可以用于比较元素。
- `_quick_sort` 函数实现了快速排序算法，而 `insertion_sort` 函数实现了插入排序算法。
- `sort` 函数是一个接口函数，允许用户选择使用不同的比较函数。
- 主函数中演示了如何使用不同的比较函数进行排序。

#### 完整代码：

```
#include<iostream>
#include<functional>
using namespace std;

#define BEGINS(x) namespace x{
#define ENDS(x)}

BEGINS(zhu)
bool cmp(int a, int b){
    return a > b;
}

class CMP{
    public:
    bool operator()(int a, int b){
        return a < b;
    }
};

void output(int *first, int * last,const char * g){
    cout << g << endl;
    while(first != last){
        cout << *(first++) << " ";
    }
    cout << endl;
}

const int threshold = 16;


class RandomIter{
    public:
    RandomIter(int *ptr) : ptr(ptr){}
    int operator-(const RandomIter &iter) {return ptr - iter.ptr;}
    int &operator*(){return *ptr;}
    RandomIter &operator++(){ ptr++; return *this;}
    RandomIter &operator--(){ ptr--; return *this;}
    RandomIter operator+(int x){return RandomIter(ptr + x);}
    RandomIter operator-(int x){return RandomIter(ptr - x);}
    RandomIter &operator++(int){ptr++; return *this;}
    RandomIter operator--(int){return RandomIter(ptr + 1);}
    bool operator<(const RandomIter &iter) const {
        return ptr < iter.ptr;
    }
    bool operator<=(const RandomIter &iter) const {
        return !(iter < *this);
    }
    bool operator==(const RandomIter &iter) const{
        return !(iter < *this) && !(*this < iter);
    }
    bool operator!=(const RandomIter &iter) const {
        return !(iter == *this);
    }
    private:
    int *ptr;
};
void _quick_sort(RandomIter first, RandomIter last, function<bool(int, int)> cmp = less<int>()){
    while(last - first > 16){
        RandomIter l = first, r = last;
        int v = *first;
        do{
            while(cmp(v, *l)); ++l;
            while(cmp(*r, v)); --r;
            if(l <= r) swap(*(l++), *(r--));
        }while(l <= r);
        _quick_sort(l, last, cmp);
        last = r + 1;
    }
    return;
}

void inserion_sort(RandomIter first, RandomIter last, function<bool(int, int)> cmp =less<int>()){
    RandomIter ind = first;
    for(RandomIter i = first + 1; i < last; ++i){
        if(cmp(*i, *ind)) swap(*i, *ind);
    }

    while(first != ind){
        swap(*ind, *(ind - 1));
        --ind;
    }

    for(RandomIter i = first + 2; i < last; ++i){
        RandomIter j = i;
        while(cmp(*j, *(j - 1))){
            swap(*j, *(j - 1));
            --j;
        }
    }

    return;
}

void sort(RandomIter first,  RandomIter last, function<bool(int, int)> cmp = less<int>()){
    _quick_sort(first, last, cmp);
    inserion_sort(first, last, cmp);
    return;
}
int main(){
    int a[100], x;
    cin >> x;
    for(int i = 0; i < x; i++){
        cin >> a[i];
    }
    sort(a, a + x, cmp);
    output(a, a + x, "cmp:");
    CMP cmp1;
    sort(a, a+ x, cmp1);
    output(a, a + x, "cmp1:");
    return 0;
}
ENDS(zhu)

int main(){
    zhu::main();
    return 0;
}
```



## 结论

这个排序算法示例展示了C++中灵活且可扩展的排序实现。通过自定义比较函数，用户可以根据实际需求灵活选择排序策略，使得代码更具通用性。

希望这个简单的示例对你理解C++中的排序算法以及自定义比较函数有所帮助。如果有任何疑问或建议，请留言让我知道！