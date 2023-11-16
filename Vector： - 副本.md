Vector：

```
/*************************************************************************
        > File Name: Vector.cpp
        > Author:
        > Mail:
        > Created Time: Sun Nov  5 17:40:52 2023
 ************************************************************************/

#include <iostream>
#include <cassert>
#include <string>
using namespace std;

#define BEGINS(x) namespace x{
#define ENDS(x)}

BEGINS(zhu)
template <typename T>
class Vector {
public:
    typedef T value_type;
    typedef T* iterator;

    Vector() : _start(nullptr), _finish(nullptr), _end(nullptr) {}

    size_t size() const {
        return _finish - _start;
    }

    size_t capacity() const {
        return _end - _start;
    }

    void push_back(const value_type &x) {
        if (capacity() == 0 || capacity() == size()) {
            reserve((capacity() == 0) ? 1 : capacity() * 2);
        }
        *_finish = x;
        ++_finish;
    }

    void pop_back() {
        if (!empty()) {
            --_finish;
            cout << "弹出元素: " << *_finish << endl;
        } else {
            cout << "容器为空，无法弹出元素." << endl;
        }
    }

    void insert(const value_type &x, const size_t &pos) {
        if (pos > size()) {
            cout << "越界: 无法在指定位置插入元素." << endl;
            return;
        }
        if (capacity() == 0) {
            reserve(1);
            *_finish = x;
            ++_finish;
        } else if (size() == capacity()) {
            size_t size1 = size();
            size_t cap = capacity();
            value_type* tmp = new value_type[2 * capacity()]();
            for (size_t i = 0; i < pos; i++) {
                tmp[i] = _start[i];
            }
            tmp[pos] = x;
            for (size_t i = pos; i < size1; i++) {
                tmp[i + 1] = _start[i];
            }
            delete[] _start;
            _start = tmp;
            _finish = _start + size1 + 1;
            _end = _start + 2 * cap;
        } else {
            for (int i = size(); i > pos; i--) {
                _start[i] = _start[i - 1];
            }
            _start[pos] = x;
            _finish = _start + size() + 1;
        }
    }

    void erase(const size_t &pos) {
        if (pos >= 0 && pos < size()) {
            iterator begin = _start + pos;
            while (begin != _finish) {
                *begin = *(begin + 1);
                ++begin;
            }
            --_finish;
            --_end;
        } else {
            cout << "越界: 无法删除指定位置的元素." << endl;
        }
    }

    void Print() {
        if (empty()) {
            cout << "容器为空." << endl;
            return;
        }
        cout << "容器元素: ";
        for (size_t i = 0; i < size(); i++) {
            cout << _start[i] << "  ";
        }
        cout << endl;
    }

    void reserve(size_t newCapacity) {
        value_type* tmp = new value_type[newCapacity]();
        size_t size1 = size();
        if (_start) {
            for (size_t i = 0; i < size1; i++) {
                tmp[i] = _start[i];
            }
        }
        delete[] _start;
        _start = tmp;
        _finish = _start + size1;
        _end = _start + newCapacity;
    }

    bool empty() const {
        return _start == _finish;
    }

private:
    iterator _start;
    iterator _finish;
    iterator _end;
};
ENDS(zhu)

BEGINS(test1)
using namespace zhu;

int main() {
    Vector<int> ve;
    cout << "按1压入数据" << endl;
    cout << "按2插入数据" << endl;
    cout << "按3弹出数据" << endl;
    cout << "按4删除数据" << endl;
    cout << "按5查看现有数据" << endl;
    cout << "按111结束" << endl;
    int a, x, y;
    while (true) {
        cin >> a;
        if (a == 111) break;

        switch (a) {
            case 1:
                cout << "请输入要压入的元素: ";
                cin >> x;
                ve.push_back(x);
                break;
            case 2:
                cout << "请输入要插入的元素和位置 (元素 位置): ";
                cin >> x >> y;
                ve.insert(x, y);
                break;
            case 3:
                ve.pop_back();
                break;
            case 4:
                cout << "请输入要删除的位置: ";
                cin >> x;
                ve.erase(x);
                break;
            case 5:
                ve.Print();
                break;
        }
    }
    return 0;
}
ENDS(test1)

BEGINS(test2)
using namespace zhu;

int main() {
    Vector<int> ve;
    for (int i = 0; i < 10; i++) {
        ve.push_back(i);
    }
    ve.insert(111, 2);
    for (int i = 0; i < 10; i++) {
        cout << "弹出元素: " << i << "  ";
        ve.pop_back();
        cout << endl;
    }
    return 0;
}
ENDS(test2)

BEGINS(test3)
using namespace zhu;

int main() {
  Vector<string> ve;
    cout << "按1压入数据" << endl;
    cout << "按2插入数据" << endl;
    cout << "按3弹出数据" << endl;
    cout << "按4删除数据" << endl;
    cout << "按5查看现有数据" << endl;
    cout << "按111结束" << endl;
    int a, y;
    string x;
    while (true) {
        cin >> a;
        if (a == 111) break;
        switch (a) {
            case 1:
                cout << "请输入要压入的元素: ";
                cin >> x;
                ve.push_back(x);
                break;
            case 2:
                cout << "请输入要插入的元素和位置 (元素 位置): ";
                cin >> x >> y;
                ve.insert(x, y);
                break;
            case 3:
                ve.pop_back();
                break;
            case 4:
                cout << "请输入要删除的位置: ";
                cin >> y;
                ve.erase(y);
                break;
            case 5:
                ve.Print();
                break;
        }
    }
    return 0;
}
ENDS(test3)


int main() {
//  test1::main();
//	test2::mian();
    test3::main();
    return 0;
}
```

