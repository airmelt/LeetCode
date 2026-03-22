# 705 Design HashSet 设计哈希集合

__Description__:
Design a HashSet without using any built-in hash table libraries.

To be specific, your design should include these functions:

add(value): Insert a value into the HashSet.
contains(value) : Return whether the value exists in the HashSet or not.
remove(value): Remove a value in the HashSet. If the value does not exist in the HashSet, do nothing.

__Example:__

```Java
MyHashSet hashSet = new MyHashSet();
hashSet.add(1);         
hashSet.add(2);         
hashSet.contains(1);    // returns true
hashSet.contains(3);    // returns false (not found)
hashSet.add(2);          
hashSet.contains(2);    // returns true
hashSet.remove(2);          
hashSet.contains(2);    // returns false (already removed)
```

__Note:__

All values will be in the range of [0, 1000000].
The number of operations will be in the range of [1, 10000].
Please do not use the built-in HashSet library.

__题目描述__:
不使用任何内建的哈希表库设计一个哈希集合

具体地说，你的设计应该包含以下的功能

add(value)：向哈希集合中插入一个值。
contains(value) ：返回哈希集合中是否存在这个值。
remove(value)：将给定值从哈希集合中删除。如果哈希集合中没有这个值，什么也不做。

__示例 :__

```Java
MyHashSet hashSet = new MyHashSet();
hashSet.add(1);         
hashSet.add(2);         
hashSet.contains(1);    // 返回 true
hashSet.contains(3);    // 返回 false (未找到)
hashSet.add(2);          
hashSet.contains(2);    // 返回 true
hashSet.remove(2);          
hashSet.contains(2);    // 返回  false (已经被删除)
```

__注意：__

所有的值都在 [1, 1000000]的范围内。
操作的总数目在[1, 10000]范围内。
不要使用内建的哈希集合库。

__思路__:

哈希函数使用取余, 用拉链法解决冲突
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
struct Node
{
    int val;
    Node *next;
    Node(int val): val(val), next(nullptr){}
};
const int len = 13;
class MyHashSet 
{
public:
    vector<Node*> arr;
    /** Initialize your data structure here. */
    MyHashSet() 
    {
        arr = vector<Node*>(len, new Node(-1));
    }
    
    void add(int key) 
    {
        Node* temp = arr[key % len];
        if (temp -> val != -1) 
        {
            while (temp) 
            {
                if (temp -> val == key) return;
                if (!temp -> next) 
                {
                    Node *node = new Node(key);
                    temp -> next = node;
                    return;
                }
                temp = temp -> next;
            }
        } 
        else temp -> val = key;
    }
    
    void remove(int key) 
    {
        Node* temp = arr[key % len];
        if (temp -> val != -1) 
        {
            while (temp) 
            {
                if (temp -> val == key) 
                {
                    temp -> val = -1;
                    return;
                }
                temp = temp -> next;
            }
        }
    }
    
    /** Returns true if this set contains the specified element */
    bool contains(int key) 
    {
        Node* temp = arr[key % len];
            while (temp) 
            {
                if (temp -> val == key) return true;
                temp = temp -> next;
            }
        return false;
    }
};


/**
 * Your MyHashSet object will be instantiated and called as such:
 * MyHashSet* obj = new MyHashSet();
 * obj->add(key);
 * obj->remove(key);
 * bool param_3 = obj->contains(key);
 */
```

__Java__:

```Java
class MyHashSet {
    private List<LinkedList<Integer>> list;
    private int size = 13;

    /**
     * Initialize your data structure here.
     */
    public MyHashSet() {
        list = new ArrayList<>();
        for (int i = 0; i < size; i++) {
            list.add(new LinkedList<>());
        }
    }

    public void add(int key) {
        if (!contains(key)) list.get(key % size).add(key);
    }

    public void remove(int key) {
        if (contains(key)) list.get(key % size).removeFirstOccurrence(key);
    }

    /**
     * Returns true if this set contains the specified element
     */
    public boolean contains(int key) {
        LinkedList<Integer> bucket = list.get(key % size);
        for (int num : bucket) if (num == key) return true;
        return false;
    }
}


/**
 * Your MyHashSet object will be instantiated and called as such:
 * MyHashSet obj = new MyHashSet();
 * obj.add(key);
 * obj.remove(key);
 * boolean param_3 = obj.contains(key);
 */
```

__Python__:

```Python
class Node:
    
    def __init__(self, val, next):
        self.val = val
        self.next = next

class MyHashSet:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.size = 13
        self.h = [Node(None, None) for _ in range(self.size)]

    def add(self, key: int) -> None:
        p = self.h[key % self.size]
        node = p.next
        while node:
            if node.val == key:
                break
            p = node
            node = node.next
        else:
            p.next = Node(key, None)

    def remove(self, key: int) -> None:
        p = self.h[key % self.size]
        node = p.next
        while node:
            if node.val == key:
                p.next = node.next
                break
            p = node
            node = node.next

    def contains(self, key: int) -> bool:
        """
        Returns true if this set contains the specified element
        """
        node = self.h[key % self.size]
        while node:
            if node.val == key:
                return True
            node = node.next
        return False 


# Your MyHashSet object will be instantiated and called as such:
# obj = MyHashSet()
# obj.add(key)
# obj.remove(key)
# param_3 = obj.contains(key)
```
