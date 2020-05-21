#include<iostream>
using namespace std;
template<typename T>class List;  
template<typename T>class Node {
	T info;                                     //数据域
	Node<T>* link; //指针域
public:
	Node();                           //生成头结点的构造函数
	Node(const T& data);               //生成一般结点的构造函数
	void InsertAfter(Node<T>* P);        //在当前结点后插入一个结点
	Node<T>* RemoveAfter();           //删除当前结点的后继结点，返回该结点备用
	friend class List<T>;
};
template <typename T> Node<T>::Node() { link = NULL; }
template <typename T> Node<T>::Node(const T& data) {
	info = data;
	link = NULL;
}
template<typename T>void Node<T>::InsertAfter(Node<T>* p) {
	p->link = link;
	link = p;
}
template<typename T>Node<T>* Node<T>::RemoveAfter() {
	Node<T>* tempP = link;
	if (link == NULL) tempP = NULL;                 //已在链尾,后面无结点
	else link = tempP->link;
	return tempP;
}
//再定义链表类
template<typename T>class List {
	Node<T>* head, * tail;                         //链表头指针和尾指针
public:
	List();                               //构造函数，生成头结点(空链表)
	~List();                              //析构函数
	void MakeEmpty();                    //清空一个链表，只余表头结点
	Node<T>* Find(T data);           //搜索数据域与data相同的结点，返回该结点的地址
	Node<T>* search(Node<T>*);
	int Length();                         //计算单链表长度
	void print();                      //打印链表的数据域
	Node<T>* insertFront(T p);          //可用来向前生成链表，在表头插入一个结点
	Node<T>* insertRear(T p);           //可用来向后生成链表，在表尾添加一个结点
	Node<T>* CreatNode(T data);           //创建一个结点(孤立结点)
	Node<T>* deleteNode(Node<T>* p);     //删除指定结点
};
template<typename T>List<T>::List() {
	head = tail = new Node<T>();
}
template<typename T>List<T>::~List() {
	MakeEmpty();
	delete head;
}
template<typename T>void List<T>::MakeEmpty() {
	Node<T>* tempP;
	while (head->link != NULL) {
		tempP = head->link;
		head->link = tempP->link;  //把头结点后的第一个节点从链中脱离
		delete tempP;            //删除(释放)脱离下来的结点
	}
	tail = head;           //表头指针与表尾指针均指向表头结点，表示空链
}
template<typename T> Node<T>* List<T>::Find(T data) {
	Node<T>* tempP = head->link;
	while (tempP != NULL && tempP->info != data) tempP = tempP->link;
	return tempP;        //搜索成功返回该结点地址，不成功返回NULL
}
template<typename T> Node<T>* List<T>::search(Node<T>* keynode)
{
	Node<T>* temp = head->link;
	while (temp != NULL && temp->info != keynode->info) { temp = temp->link; }
	return temp;
}
template<typename T>int List<T>::Length() {
	Node<T>* tempP = head->link;
	int count = 0;
	while (tempP != NULL) {
		tempP = tempP->link;
		count++;
	}
	return count;
}
template<typename T>void List<T>::print() {
	Node<T>* tempP = head->link;
	while (tempP != NULL) {
		cout << tempP->info << '\t';
		tempP = tempP->link;
	}
	cout << endl;
}
template<typename T>Node<T>* List<T>::insertFront(T data) {    //链头插入
	Node<T>* p = new Node<T>(data);
	p->link = head->link;
	head->link = p;
	if (tail = head) tail = p;
	return head;
}
template<typename T>Node<T>* List<T>::insertRear(T data) {    //链尾插入
	Node<T>* p = new Node<T>(data);
	p->link = tail->link;
	tail->link = p;
	tail = p;
	return head;
}
template<typename T>Node<T>* List<T>::CreatNode(T data) {//建立新节点
	Node<T>* tempP = new Node<T>(data);
	return tempP;
}
template<typename T>Node<T>* List<T>::deleteNode(Node<T>* p) {
	Node<T>* tempP = head;
	while (tempP->link != NULL && tempP->link != p) tempP = tempP->link;
	if (tempP->link == tail) tail = tempP;
	return tempP->RemoveAfter(); //本函数所用方法可省一个工作指针，与InsertOrder比较
}
template<class T> class List;
template<class T> class Node;
int main() {
    List<int> l1, l2;     //创建2个空的整型链表
    int c;
    Node<int>* head1,* head2;
    cout << "输入数据" << endl;
    for (int i = 0; i < 8; i++) {
        cin >> c;
        head1 = l1.insertFront(c);  //插在链头，倒向生成链表，头指针head1    
        head2 = l2.insertRear(c);  //插在链尾，正向生成链表，头指针head2
    }
    l1.print();  cout << endl;
    l2.print();  cout << endl;
    int key;                        //待查找关键字
    cin >> key;
    Node<int>* keyNode, * p;
    keyNode = new Node<int>(key);   //构造关键字节点   
    p = l2.search(keyNode);
    if (p)  l2.deleteNode(p);       //删除链表中第一次出现的关键字节点
    else cout << "no such keyword "<< endl;
    l2.print();
    return 0;
}
