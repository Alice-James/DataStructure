# 单链表

```c++
#include <iostream>
using namespace std;

//定义单链表结点
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

//定义具有头结点的单链表
template <class T> class lnkList{
    public:
        Link<T> *head, *tail;//链表的头、尾指针
        Link<T> *setPos(const int p);//返回线性表指向第p个元素的指针值

        lnkList();//构造函数
        ~lnkList();//析构函数
        bool isEmpty();//判断链表是否为空
        void clear();//将链表存储的内容清除，成为空表
        int length();//返回链表当前长度
        bool append(const T value);//在链尾添加一个结点，其数据为value
        bool insert(const int i, const T value);//在位置i上插入一个结点，其数据为value
        bool remove(const int i);//删除位置i上的元素
        bool getValue(const int i, T& value);//用value返回位置i的数据
        bool getPos(int& i, const T value);//查找数据为value的结点，用i返回第一次出现的位置
        void displayList();//打印链表
};

//单链表构造函数
template <class T>
lnkList<T>::lnkList() {
    head = tail = new Link<T>;//采用链表结点Link的构造函数来初始化head和tail
};

//单链表析构函数
template <class T>
lnkList<T>::~lnkList() {
    Link<T> *temp;

    //从头到尾删除结点
    while(head != NULL) {
        temp = head;
        head = head->next;
        delete temp;
    }
}

//判断链表是否为空
template <class T>
bool lnkList<T>::isEmpty() {
    return head->next == NULL;
}

//将链表清空
template <class T>
void lnkList<T>::clear() {    
    Link<T> *temp;//利用temo来删除结点
    Link<T> *p;//利用p来指向待删除结点

    //从第一个结点开始删除
    p = head->next;
    while(p != NULL) {
        temp = p;
        p = p->next;
        delete temp;
    }
    head->next = NULL;
    tail = head;//更新尾指针
}

//返回链表当前长度
template <class T>
int lnkList<T>::length() {
    int count = 0;

    Link<T> *p = new Link<T>(head->next);
    while(p->next != NULL) {
        p = p->next;
        count++;
    }
    
    return count;
}

//在链尾添加一个结点
template <class T>
bool lnkList<T>::append(const T value) {
    Link<T> *p;
    p = new Link<T>(value, NULL);
    tail->next = p;
    tail = p;//更新尾指针

    return true;
}

//寻找链表的第i个结点
//当链表实际长度小于给定的i时，返回NULL；当i为-1时，返回指向头结点的指针
template <class T>
Link<T> * lnkList<T>::setPos(int i) {
    int count = 0;
    if(i == -1)
        return head;

    Link<T> *p = new Link<T>(head->next);
    while(p != NULL && count <= i) {
        p = p->next;
        count++;
    }
    
    return p;
}

//插入单链表的第i个结点
template <class T>
bool lnkList<T>::insert(const int i, const T value) {
    Link<T> *p, *q;//p用来定位，q作为插入的新结点

    if((p = setPos(i - 1)) == NULL) {
        cout << "Illegal insertion" << endl;
        return false;
    }
    q = new Link<T>(value, p->next);
    p->next = q;

    if(p == tail)//如果插入点在链尾，更新尾指针
        tail = q;

    return true;
}

//删除单链表的第i个结点
template <class T>
bool lnkList<T>::remove(const int i) {
    Link<T> *p, *q;//p用来定位，q作为待删结点
    Link<T> *temp = head->next;

    if(i == 0) {//删除第0个结点
        head->next = head->next->next;
        delete temp;
        return true;
    }

    if((p = setPos(i)) == NULL || p == tail) {//待删结点不存在
        cout << "Illegal removal" << endl;
    }

    q = p->next;
    if(q == tail) {//若待删结点为链尾，更新尾指针
        tail = p;
        p->next = NULL;
        delete q;
    }
    else if(q != NULL) {
        p->next = q->next;
        delete q;
    }

    return true;
}

//用value返回位置i的数据
template <class T>
bool lnkList<T>::getValue(const int i, T& value) {
    Link<T> *p;
    if((p = setPos(i - 1)) == NULL || p == tail)//待读取结点不存在
        return false;
    else {
        value = p->next->data;
        return true;
    }
}

//查找数据为value的结点，用i返回第一次出现的位置
template <class T>
bool lnkList<T>::getPos(int& i, const T value) {
    i = -1;
    Link<T> *p = new Link<T>(head->next);
    while(p != NULL && p->data != value) {
        p = p->next;
        i++;
    }
    if(p == NULL)
        return false;
    else 
        return true;
}

//打印链表
template <class T>
void lnkList<T>::displayList() {
    Link<T> *p = new Link<T>(head->next);
    p = p->next;
    int i = 0;

    if(isEmpty())
        cout << "The list is empty!" << endl;
    else 
        while(p != NULL) {
            cout << "LinkList[" << i << "] = " << p->data << endl;
            p = p->next;
            i++;
        }
}
```

# 作业题

```c++
#include "SingleLinkList1.h"
#include <iostream>
using namespace std;

//第一次作业

//第1题：将两个非递减的有序链表合为一个非递减的有序链表。
//（要求利用原来两个链表的存储空间，不另外占用其他空间，表中不允许有重复数据）
template <class T>
void merge1(lnkList<T>& list1, lnkList<T>& list2);

//第2题：将两个非递减的有序链表合为一个非递增的有序链表。
//（要求利用原来两个链表的存储空间，不另外占用其他空间，表中不允许有重复数据）
template <class T>
void merge2(lnkList<T>& list1, lnkList<T>& list2);

//第3题：通过一趟遍历确定单链表中值最大的结点。
template <class T>
Link<T> *maxLink(lnkList<T> list);

//第4题：通过遍历一趟，将链表中所有结点的链接方向逆转（使用原表存储空间）
template <class T>
void reverse(lnkList<T>& list);

//第5题：删除递增有序链表中值大于min(i)且小于max(k)的所有元素
//（i和k是给定的两个参数，其值可以和表中的元素相同，也可以不同）
template <class T>
bool deleteBetween(lnkList<T>& list, T i, T k);

//第1题
template <class T>
void merge1(lnkList<T>& list1, lnkList<T>& list2) {
    list1.tail->next = list2.head->next;//将list1与list2相连
    list1.tail = list2.tail;
    delete list2.head;

    int totalLength = list1.length();
    Link<T> *p = list1.head;
    Link<T> *temp1, *temp2;
    //采用冒泡排序算法
    for(int i = 0; i < totalLength; i++) {
        while(p->next->next != NULL) {
            //交换相邻结点
            if(p->next->data > p->next->next->data) {
                temp1 = p->next;
                temp2 = p->next->next->next;
                p->next = p->next->next;
                p->next->next = temp1;
                p->next->next->next = temp2;
            }
            //删除重复结点
            else if(p->next->data == p->next->next->data) {
                temp1 = p->next->next;
                p->next->next = p->next->next->next;
                delete temp1;
                totalLength--;
            }
            else
                p = p->next;
        }
        p = list1.head;
    }
}

//第2题
template <class T>
void merge2(lnkList<T>& list1, lnkList<T>& list2) {
    list1.tail->next = list2.head->next;//将list1与list2相连
    list1.tail = list2.tail;
    delete list2.head;

    int totalLength = list1.length();
    Link<T> *p = list1.head;
    Link<T> *temp1, *temp2;
    //采用冒泡排序算法
    for(int i = 0; i < totalLength; i++) {
        while(p->next->next != NULL) {
            //交换相邻结点
            if(p->next->data < p->next->next->data) {
                temp1 = p->next;
                temp2 = p->next->next->next;
                p->next = p->next->next;
                p->next->next = temp1;
                p->next->next->next = temp2;
            }
            //删除重复结点
            else if(p->next->data == p->next->next->data) {
                temp1 = p->next->next;
                p->next->next = p->next->next->next;
                delete temp1;
                totalLength--;
            }
            else
                p = p->next;
        }
        p = list1.head;
    }
}

//第3题
template <class T>
Link<T> *maxLink(lnkList<T> list) {
    Link<T> *dest = list.head->next, *current = list.head->next;

    if(dest == NULL)
        return NULL;

    while(current != NULL) {
        if(dest->data < current->data)
            dest = current;
        current = current->next;
    }

    return dest;
}

//第4题
template <class T>
void reverse(lnkList<T>& list) {
    Link<T> *temp1 = list.head, *temp2 = NULL, *firstLink = list.head->next;

    if(temp1->next == NULL || temp1->next->next == NULL)//空链表或只有一个结点
        return;

    while(temp1->next != NULL) {//在最后1个结点跳出循环
        temp2 = temp1->next;

        temp1->next = list.head->next;
        list.head->next = temp1;

        temp1 = temp2;//步进
    }

    //反转最后一个指针
    temp1->next = list.head->next;
    list.head->next = temp1;

    list.tail = firstLink;//更新尾指针
    list.tail->next = NULL;
}

//第5题：删除递增有序链表中值大于min(i)且小于max(k)的所有元素
//（i和k是给定的两个参数，其值可以和表中的元素相同，也可以不同）
template <class T>
bool deleteBetween(lnkList<T>& list, T i, T k) {
    if(i >= k)
        return true;
    
    Link<T> *current = list.head, *temp = NULL;

    while(current->next != NULL) {
        if(i < current->next->data && current->next->data < k) {
            temp = current->next;
            current->next = current->next->next;
            delete temp;
        }
        else
            current = current->next;
    }
        return true;
}
```
