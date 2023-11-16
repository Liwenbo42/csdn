### 快排：

```
#include<iostream>
using namespace std;

void print(int a[]){
    for(int i = 0; i <= 9; i++){
        cout << a[i] << "  ";
    }
    cout << endl;
}

void kuai(int a[], int l, int r){
    if(l >= r ) return;
    int ll = l, rr = r;
    int n = a[l];

    while(ll < rr){
        while(ll < rr && a[rr] > n){
            rr--;
        }
        if(ll < rr) a[ll++] = a[rr];
        while(ll < rr && a[ll] < n){
            ll++;
        }
        if(ll < rr) a[rr--] = a[ll];
    }
    a[ll] = n;

    kuai(a, l, ll);
    kuai(a, ll + 1, r);
}

int main(){
    int x;
    int a[10] = {2, 6, 4, 8, 9, 1, 3, 5, 7};
    int l = a[0], r = a[9];
    print(a);
    kuai(a, 0, 9);
    print(a);
    cout << endl;
    return 0;
}
```



### 简单Sort：

```
#include<iostream>
#include<algorithm>
#include<functional>
using namespace std;

#define BEGINS(x) namespace x{
#define ENDS(x)}

BEGINS(zhu)

bool cmp(int a, int b){
    return a > b;
}

bool cmp1(int a, int b){
    return a < b;
}
void input(int *first, int *last, const char *g){
    cout << g << endl;
    for(int *i = first; i < last; i++){
        cout << *i  << " ";
    }
    cout << endl;
}

void you_sort(int *first, int *last, function<bool(int, int)> cmp1){
    if(first >= last) return ;
    int *l = first, *r = last - 1;
    int z = *first;
    while(l < r){
        while(l < r && cmp1(z, *r)) {
            --r;
        }
        if(l < r) *l = *r;
        while(l < r && cmp1(*l, z)){
            l++;
        }
        if(l < r) *r = *l;
    }
    *l = z;

    you_sort(first, r, cmp1);
    you_sort(r + 1, last, cmp1);

}
int main(){
    int x, a[100];
    cin >> x;
    for(int i = 0; i < x; i++){
        cin >> a[i];
    }
    sort(a, a + x, cmp);
    input(a, a + x, "cmp");
    you_sort(a, a + x, cmp1);
    input(a, a + x, "cmp1");
    return 0;
}
ENDS(zhu)

int main(){
    zhu::main();
    return 0;
}
```





### Sort优化2.0：

```
#include<iostream>
#include<functional>
using namespace std;

#define BEGINS(x) namespace x{
#define ENDS(x)}

BEGINS(zhu)

bool cmp(int a, int b){
    return a < b;
}

void input(int *first, int *last, const char *g){
    cout << g << endl;
    while(first != last){
        cout << *first << " " ;
        ++first;
    }
    cout << endl;
}

void _you_sort(int *first, int *last, function<bool(int , int)> cmp = less<int>()){
    while(first < last){
        int *l = first, *r = last - 1;
        int v = *first;
        do{
            while(cmp(*l, v)) ++l;
            while(cmp(v, *r)) --r;
            if(l <= r){
                swap(*l, *r);
                ++l;
                --r;
            }
        }while(l <= r);
        _you_sort(l, last, cmp);
        last = r + 1;
    }
    return;
}

int main(){
    int a[100],x;
    cin >> x;
    for(int i = 0; i < x; i++){
        cin >> a[i];
    }
    _you_sort(a, a + x, cmp);
    input(a, a + x, "cmp :");
    return 0;
}
ENDS(zhu)

int main(){
    zhu::main();

    return 0;
}
```





### Sort优化3.0：

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





### Sort优化4.0：

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

