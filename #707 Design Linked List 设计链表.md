# 707 Design Linked List 设计链表

__Description__:
Design your implementation of the linked list. You can choose to use the singly linked list or the doubly linked list. A node in a singly linked list should have two attributes: val and next. val is the value of the current node, and next is a pointer/reference to the next node. If you want to use the doubly linked list, you will need one more attribute prev to indicate the previous node in the linked list. Assume all nodes in the linked list are 0-indexed.

Implement these functions in your linked list class:

get(index) : Get the value of the index-th node in the linked list. If the index is invalid, return -1.
addAtHead(val) : Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
addAtTail(val) : Append a node of value val to the last element of the linked list.
addAtIndex(index, val) : Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted.
deleteAtIndex(index) : Delete the index-th node in the linked list, if the index is valid.

__Example:__

```Java
MyLinkedList linkedList = new MyLinkedList();
linkedList.addAtHead(1);
linkedList.addAtTail(3);
linkedList.addAtIndex(1, 2);  // linked list becomes 1->2->3
linkedList.get(1);            // returns 2
linkedList.deleteAtIndex(1);  // now the linked list is 1->3
linkedList.get(1);            // returns 3
```

__Note:__

All values will be in the range of [1, 1000].
The number of operations will be in the range of [1, 1000].
Please do not use the built-in LinkedList library.

__题目描述__:
设计链表的实现。您可以选择使用单链表或双链表。单链表中的节点应该具有两个属性：val 和 next。val 是当前节点的值，next 是指向下一个节点的指针/引用。如果要使用双向链表，则还需要一个属性 prev 以指示链表中的上一个节点。假设链表中的所有节点都是 0-index 的。

在链表类中实现这些功能：

get(index)：获取链表中第 index 个节点的值。如果索引无效，则返回-1。
addAtHead(val)：在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点。
addAtTail(val)：将值为 val 的节点追加到链表的最后一个元素。
addAtIndex(index,val)：在链表中的第 index 个节点之前添加值为 val  的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。
deleteAtIndex(index)：如果索引 index 有效，则删除链表中的第 index 个节点。

__示例 :__

```Java
MyLinkedList linkedList = new MyLinkedList();
linkedList.addAtHead(1);
linkedList.addAtTail(3);
linkedList.addAtIndex(1,2);   //链表变为1-> 2-> 3
linkedList.get(1);            //返回2
linkedList.deleteAtIndex(1);  //现在链表是1-> 3
linkedList.get(1);            //返回3
```

__提示：__

所有val值都在 [1, 1000] 之内。
操作次数将在  [1, 1000] 之内。
请不要使用内置的 LinkedList 库。

__思路__:
可以用单链表也可以用双链表解决
注意不要断链即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Node 
{
public:
    int val;
    Node* next;
    Node* pre;
    Node(int val, Node* next, Node* pre): val(val), next(next), pre(pre) {}
};

class MyLinkedList 
{
public:
    /** Initialize your data structure here. */
    MyLinkedList() 
    {
        root = nullptr;
        tail = nullptr;
        len = 0;
    }
    
    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    int get(int index) 
    {
        int temp = 0;
        Node* cur = root;
        while (cur) 
        {
            if (temp == index) return cur -> val;
            cur = cur -> next;
            temp++;
        }
        return -1;
    }
    
    /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
    void addAtHead(int val) 
    {
        if (root) 
        {
            Node* node = new Node(val, root, nullptr);
            root -> pre = node;
            root = node;
        } 
        else 
        {
            root = new Node(val, nullptr, nullptr);
            tail = root;
        }
        len++;
    }
    
    /** Append a node of value val to the last element of the linked list. */
    void addAtTail(int val) 
    {
        if (tail) 
        {
            Node* node = new Node(val, nullptr, tail);
            tail -> next = node;
            tail = node;
        } 
        else 
        {
            tail = new Node(val, nullptr, nullptr);
            root = tail;
        }
        len++;
    }
    
    /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
    void addAtIndex(int index, int val) 
    {
        if (index <= 0) 
        {
            addAtHead(val);
            return;
        }
        if (index == len) 
        {
            addAtTail(val);
            return;
        }
        int temp = 0;
        Node* pre = nullptr;
        Node* cur = root;
        while (cur) 
        {
            if (temp == index) 
            {
                Node* node = new Node(val, cur, pre);
                if (pre) pre -> next = node;
                cur -> pre = node;
                ++len;
                return;
            }
            pre = cur;
            cur = cur -> next;
            ++temp;
        }
    }
    
    /** Delete the index-th node in the linked list, if the index is valid. */
    void deleteAtIndex(int index) 
    { 
        if (auto node = getNode(index)) 
        {
            if (node == root) 
            {
                root = root -> next;
                if (root) root -> pre = nullptr;
                node -> next = nullptr;
            }
            if (node == tail) 
            {
                tail = tail -> pre;
                if (tail) tail -> next = nullptr;
                node -> pre = nullptr;
            }
            if (node -> next) node -> next -> pre = node -> pre;
            if (node -> pre) node -> pre -> next = node -> next;
            delete node;
            --len;
        }
    }
private:
    Node* root;
    Node* tail;
    int len;
    Node* getNode(int index) 
    {
        if (index >= len or index < 0) return nullptr;
        Node* node;
        int i;
        if (len / 2 >= index) 
        {
            i = index;
            node = root;
            while (i-- > 0) node = node -> next;
        } 
        else 
        {
            i = len - index - 1;
            node = tail;
            while (i-- > 0) node = node -> pre;
        }
        return node;
    }
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```

__Java__:

```Java
class MyLinkedList {
    private class Node{
        int val;
        Node next;
        Node(int val){
            this.val = val;
            this.next = null;
        }
    }
    private Node head, tail;
    private int size;

    /** Initialize your data structure here. */
    public MyLinkedList() {
        Node p= new Node(0);
        this.head = p;
        this.tail = p;
        this.size = 0;
    }
    
    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    public int get(int index) {
        if (index < 0 || index >= this.size) return -1;
        else {
            Node cur = this.head;
            for (int i = 0 ; i < index; i++) cur= cur.next;
            return cur.val;
        }
    }
    
    /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
    public void addAtHead(int val) {
        if (this.size == 0) this.head.val = val;
        else {
            Node tmp = new Node(val);
            tmp.next = this.head;
            this.head = tmp;
        }
        this.size++;
    }
    
    /** Append a node of value val to the last element of the linked list. */
    public void addAtTail(int val) {
        if (this.size == 0) this.tail.val = val;
        else {
            Node tmp = new Node(val);
            this.tail.next = tmp;
            this.tail = tmp;
        }
        this.size++;
    }
    
    /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
    public void addAtIndex(int index, int val) {
        if (index > this.size) return;
        else if (index <= 0) addAtHead(val);
        else if (index == this.size) addAtTail(val);
        else {
            Node tmp = new Node(val);
            Node cur = this.head;
            for (int i = 0; i < index - 1; i++) cur = cur.next;
            tmp.next = cur.next;
            cur.next = tmp;
            this.size++;
        }
    }
    
    /** Delete the index-th node in the linked list, if the index is valid. */
    public void deleteAtIndex(int index) {
        if (index >= 0 && index < this.size) {
            if (index == 0) this.head = this.head.next;
            else {
                Node cur = this.head;
                for (int i = 0; i < index - 1; i++) cur = cur.next;
                cur.next = cur.next.next;
                if (index == this.size - 1) tail=cur;
            }
            this.size--;
        }
    }
}

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
```

__Python__:

```Python
class Node(object):
    def __init__(self, x):
        self.val = x
        self.next = None

class MyLinkedList:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.head = Node(0)

    def get(self, index: int) -> int:
        """
        Get the value of the index-th node in the linked list. If the index is invalid, return -1.
        """
        if index < 0: 
            return -1
        node = self.head
        for _ in range(index + 1):
            if node.next:
                node = node.next
            else: 
                return -1
        return node.val
    
    def addAtHead(self, val: int) -> None:
        """
        Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
        """
        new = Node(val)
        new.next = self.head.next
        self.head.next = new
        

    def addAtTail(self, val: int) -> None:
        """
        Append a node of value val to the last element of the linked list.
        """
        node = self.head
        while node.next:
            node = node.next
        node.next = Node(val)

    def addAtIndex(self, index: int, val: int) -> None:
        """
        Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted.
        """
        node = self.head
        for i in range(index):
            if not node:
                return 
            node = node.next
        if not node:
            node = Node(val)
        else:
            new = Node(val)
            new.next = node.next
            node.next = new

    def deleteAtIndex(self, index: int) -> None:
        """
        Delete the index-th node in the linked list, if the index is valid.
        """
        if index < 0: return
        node = self.head
        for _ in range(index):
            node = node.next
        if node and node.next:
            node.next = node.next.next
```
