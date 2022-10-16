# 链式栈

```c++
#include <iostream>
using namespace std;

//定义链式栈结点
template <class T> class Link {
    public:
        T data;//用于保存结点元素的内容
        Link<T> *next;//指向后继结点的指针

        Link(const T info, Link<T> *nextValue = NULL) {//具有两个参数的Link构造函数
            data = info;
            next = nextValue;
        }
        Link(Link<T> *nextValue = NULL) {//具有一个参数的Link构造函数
            next = nextValue;
        }
};

//定义链式栈
template <class T> class LinkStack{//栈的链式存储
    private:
        Link<T> *top;//指向栈顶的指针
        int size;//存放元素的个数
    public:
        LinkStack() {//构造函数
            top = NULL;
            size = 0;
        }
        ~LinkStack() {//析构函数
            clear();
        }
        void clear() {//清空栈内容
            while(top != NULL) {
                Link<T> *temp = top;
                top = top->next;
                delete temp;
            }
            size = 0;
        }
        bool push(const T item) {//入栈操作的链式实现
            Link<T> *temp = new Link<T>(item, top);
            top = temp;
            size++;
            return true;
        }
        bool pop(T& item) {//出栈操作的链式实现
            Link<T> *temp;
            if(size == 0) {
                cout << "Empty!!!" << endl;
                return false;
            }
            item = top->data;
            temp = top->next;
            delete top;
            top = temp;
            size--;
            return true;
        }
        bool stackTop(T& item) {//返回栈顶内容，但不弹出
            if(size == 0) {
                cout << "Empty!!!" << endl;
                return false;
            }
            item = top->data;
            return true;
        }
        bool isEmpty() {//返回栈是否为空
            return size == 0;
        }
};
```

# 作业2——栈实现

```c++
#include "LinkStack.h"
#include <iostream>
#include <cstring>
using namespace std;

//第2次作业
//判断依次读入的一个以#为结束符的字母序列，是否形如“序列a&序列b”模式的字符序列。
//（其中序列a和序列b中都不含字符'&'，并且序列b是序列a的逆序列）
//栈实现

template <class T>
bool isReversal(string& str, LinkStack<T> strStack);

template <class T>
bool isReversal(string& str, LinkStack<T> strStack) {//借助链式栈strStack来帮助运行字符串str的isreversal函数（模板函数参数表要包含T?）
    int length = str.size();
    
    int i = 0;
    for( ; i < length; i++) {
        if(str[i] == '&')
            break;
        strStack.push(str[i]);
    }

    if(i == length)//没有'&'字符
        return false;

    char temp;
    for(i = i + 1; i < length; i++) {
        if(strStack.isEmpty())//b字符串比a字符串长
            return false;

        strStack.stackTop(temp);
        if(temp == str[i])
            strStack.pop(temp);
        else//字符不匹配
            return false;
    }

    if(i == length && strStack.isEmpty())
        return true;
    else//a字符串比b字符串长
        return false;
}
```

