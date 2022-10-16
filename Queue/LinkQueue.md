# 链式队列

```c++
#include <iostream>
using namespace std;

//定义链式队列结点
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

//定义链式队列
template <class T> class LinkQueue {
    private:
        int size;//队列中当前元素的个数
        Link<T> *front;//表示队头的指针
        Link<T> *rear;//表示队尾的指针
    public:
        LinkQueue() {//构造函数
            size = 0;
            front = rear = NULL;
        }
        ~LinkQueue() {//析构函数
            while(front != NULL) {
                rear = front;
                front = front->next;
                delete rear;
            }
            rear = NULL;
            size = 0;
        }
        bool enQueue(const T item) {//item入队，插入队尾
            if(rear == NULL) {
                front = rear = new Link<T>(item, NULL);
            }
            else {
                rear->next = new Link<T>(item, NULL);
                rear = rear->next;
            }
            size++;
            
            return true;
        }
        bool deQueue(T& item) {//返回队头元素并删除
            Link<T> *temp;
            if(size == 0) {
                cout << "Empty!!!" << endl;
                return false;
            }
            item = front->data;
            temp = front;
            front = front->next;
            delete temp;
            if(front == NULL) 
                rear = NULL;
            size--;
            
            return true;
        }
        bool getFront(T& item) {//返回队头元素，但不删除
            if(size == 0) {
                cout << "Empty!!!" <<endl;
                return false;
            }
            item = front->data;

            return true;
        }
        bool isEmpty() {//返回队列是否为空
            return size == 0;
        }
        int length() {//返回队列长度
            return size;
        }
};
```

# 作业题

```c++
#include "LinkQueue.h"
#include <iostream>
#include <cstring>
using namespace std;

//第2次作业
//判断依次读入的一个以#为结束符的字母序列，是否形如“序列a&序列b”模式的字符序列。
//（其中序列a和序列b中都不含字符'&'，并且序列b是序列a的逆序列）
//队列实现

template <class T>
bool isReversal(string& str, LinkQueue<T> strQueue);

template <class T>
bool isReversal(string& str, LinkQueue<T> strQueue) {
    int strLength = str.size();

    if(strLength % 2 == 0)
        return false;
    
    for(int i = 0; i < strLength; i++) {//将字符串a设为队列
        if(str[i] == '&')
            break;
        strQueue.enQueue(str[i]);
    }
    if(strQueue.length() != (strLength - 1) / 2)
        return false;

    T temp;
    for(int i = strLength - 1; i >= 0; i--) {//将字符串b逆序与a比较
        if(strQueue.isEmpty())
            break;

        strQueue.getFront(temp);
        if(temp == str[i])
            strQueue.deQueue(temp);
        else
            return false;
    }
    
    return true;
}
```

