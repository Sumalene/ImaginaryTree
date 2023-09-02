---
{"dg-publish":true,"permalink":"/text/","title":"数据结构"}
---

## 线性表  
[百度]<a href="http://www.baidu.com" target="_blank">百度</a>  
  
```cpp  
//顺序表  
class SeqList {  
private:  
    E *data;  //存储数据的数组  
    int maxSize;  //最大容量  
    int last;  //当前长度  
public:  
    //初始化  
    SeqList(int sz) {        maxSize = sz; //设置最大容量  
        data = new E[maxSize]; //动态分配数组空间  
        last = -1; //空表  
    }    //判空  
    bool isEmpty() const {        return last == -1;    }    //判满  
    bool isFull() const {        return last == maxSize - 1;    }    //插入  
    void insert(int i, E x) {        if (isFull()) {            cout << "上溢" << endl;  
            return;        }        if (i < 0 || i > last + 1) {            cout << "位置非法" << endl;  
            return;        }        for (int j = last; j >= i; j--)            data[j + 1] = data[j];  
        data[i] = x;        last++;    }    //删除  
    void remove(int i) {        if (isEmpty()) {            cout << "下溢" << endl;  
            return;        }        if (i < 0 || i > last) {            cout << "位置非法" << endl;  
            return;        }        for (int j = i; j < last; j++)            data[j] = data[j + 1];        last--;    }    //获取长度  
    int length() const {        return last + 1;    }    //按位置获取元素  
    E get(int i) {        if (i < 0 || i > last) {            cout << "位置非法" << endl;  
            return -1;        }        return data[i];    }    //按值查找位置  
    int locate(E x) {        int low = 0, high = last, mid;        while (low <= high) {            mid = (low + high) / 2;            if (data[mid] == x)                return mid;            else if (data[mid] > x)                high = mid - 1;            else                low = mid + 1;        }        return -1;    }    //测试:输出元素  
    void print() {        for (int i = 0; i <= last; i++)            cout << data[i] << " ";        cout << endl;    }    //析构函数  
    ~SeqList() {        delete[] data; //释放数组空间  
    }};  
```  
  
```cpp  
//单链表  
class LNode {  
public:  
    E data{}; //数据域  
    LNode *next; //指针域,后继  
    LNode() { //构造函数  
        next = nullptr;    }    LNode(E e) { //构造函数  
        data = e;        next = nullptr;    }};  
  
class LinkList {  
public:  
    LNode *head; //头结点  
    LinkList() { //构造函数  
        head = new LNode();    }    ~LinkList() { //析构函数  
        delete head;    }    //插入结点  
    bool InsertNode(int i, E e) const { //参数:插入位置,插入元素  
        if (i < 1) return false; //i值不合法  
        LNode *p; //定义一个指针p  
        int j = 0; //计数器  
        p = head; //p指向头结点  
        while (p != nullptr && j < i - 1) { //循环找到第i-1个结点  
            p = p->next;            j++;        }        if (p == nullptr) return false; //i值不合法,合法性取决于i-1的位置是否存在  
        LNode *s = new LNode(e); //生成新结点  
        s->next = p->next; //将新结点插入到第i-1个结点之后  
        p->next = s;        return true;    }    //删除结点  
    bool DeleteNode(int i, E &e) const {        if (i < 1) return false; //i值不合法  
        LNode *p; //定义一个指针p  
        int j = 0; //计数器  
        p = head; //p指向头结点  
        while (p != nullptr && j < i - 1) { //循环找到第i-1个结点  
            p = p->next;            j++;        }        if (p == nullptr) return false; //i值不合法  
        if (p->next == nullptr) return false; //第i-1个结点之后已无其他结点  
        LNode *q = p->next; //令q指向被删除结点  
        e = q->data; //用e返回被删除结点的值  
        p->next = q->next; //将*q结点从链中“断开”  
        delete q; //释放结点的存储空间  
        return true;    }    //查找结点  
    [[nodiscard]] LNode *GetNode(int i) const { //返回值为结点指针  
        if (i < 0) return nullptr; //i值不合法  
        LNode *p; //定义一个指针p  
        int j = 0; //计数器  
        p = head; //p指向头结点  
        while (p != nullptr && j < i) { //循环找到第i个结点  
            p = p->next;            j++;        }        return p;    }    //按值查找  
    [[nodiscard]] LNode *LocateNode(E e) const { //返回值为结点指针  
        LNode *p = head->next; //p指向首结点  
        while (p != nullptr && p->data != e) { //循环查找  
            p = p->next;        }        return p;    }    //打印链表  
    void PrintList() const {        LNode *p = head->next; //p指向首结点  
        while (p != nullptr) { //p所指结点存在  
            cout << p->data << " "; //输出p所指结点的值  
            p = p->next; //p指向下一个结点  
        }        cout << endl;    }};  
```  
  
```cpp