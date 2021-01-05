# 706 Design HashMap 设计哈希映射

__Description__:
Design a HashMap without using any built-in hash table libraries.

To be specific, your design should include these functions:

put(key, value) : Insert a (key, value) pair into the HashMap. If the value already exists in the HashMap, update the value.
get(key): Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key.
remove(key) : Remove the mapping for the value key if this map contains the mapping for the key.

__Example:__

```Java
MyHashMap hashMap = new MyHashMap();
hashMap.put(1, 1);          
hashMap.put(2, 2);         
hashMap.get(1);            // returns 1
hashMap.get(3);            // returns -1 (not found)
hashMap.put(2, 1);          // update the existing value
hashMap.get(2);            // returns 1 
hashMap.remove(2);          // remove the mapping for 2
hashMap.get(2);            // returns -1 (not found) 
```

__Note:__

All keys and values will be in the range of [0, 1000000].
The number of operations will be in the range of [1, 10000].
Please do not use the built-in HashMap library.

__题目描述__:
不使用任何内建的哈希表库设计一个哈希映射

具体地说，你的设计应该包含以下的功能

put(key, value)：向哈希映射中插入(键,值)的数值对。如果键对应的值已经存在，更新这个值。
get(key)：返回给定的键所对应的值，如果映射中不包含这个键，返回-1。
remove(key)：如果映射中存在这个键，删除这个数值对。

__示例 :__

```Java
MyHashMap hashMap = new MyHashMap();
hashMap.put(1, 1);          
hashMap.put(2, 2);         
hashMap.get(1);            // 返回 1
hashMap.get(3);            // 返回 -1 (未找到)
hashMap.put(2, 1);         // 更新已有的值
hashMap.get(2);            // 返回 1 
hashMap.remove(2);         // 删除键为2的数据
hashMap.get(2);            // 返回 -1 (未找到) 
```

__注意：__

所有的值都在 [1, 1000000]的范围内。
操作的总数目在[1, 10000]范围内。
不要使用内建的哈希库。

__思路__:

哈希函数使用取余, 用拉链法解决冲突
Java代码参考源码实现
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
struct Node
{
    int nkey;
    int nval;
    Node* next;
    Node(int key, int val): nkey(key), nval(val), next(nullptr){}
};
const int len = 13;
class MyHashMap 
{
public:
    vector <Node*> arr;
    /** Initialize your data structure here. */
    MyHashMap() 
    {
        arr = vector<Node*> (len, new Node(-1, -1));
    }
    
    /** value will always be non-negative. */
    void put(int key, int value) 
    {
        Node* p = arr[key % len];
        Node* node;
        while (p) 
        {
            if (p -> nkey == key) 
            {
                p -> nval = value;
                return;
            }
            node = p;
            p = p -> next;
        }
        p = new Node(key,value);
        node -> next = p;
    }
    
    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) 
    {
        Node* node = arr[key % len];
        while (node) 
        {
            if (node -> nkey == key) return node -> nval;
            node = node -> next;
        }
        return -1;
    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) 
    {
        Node* node = arr[key % len];
        while (node) 
        {
            if (node -> nkey == key) node -> nval = -1;
            node = node -> next;
        }
    }
};

/**
 * Your MyHashMap object will be instantiated and called as such:
 * MyHashMap* obj = new MyHashMap();
 * obj->put(key,value);
 * int param_2 = obj->get(key);
 * obj->remove(key);
 */
```

__Java__:

```Java
class MyHashMap {
    private static final int DEFAULT_CAPACITY = 16;
    private static final float DEFAULT_LOAD_FACTOR = 0.75f;
    private int size;
    private int capacity;
    private float factor; 
    private int threshold; 

    private Node[] table;

    @SuppressWarnings("unchecked")
    public MyHashMap(int capacity, float factor) {
        if (capacity <= 0) {
            capacity = DEFAULT_CAPACITY;
        }
        if (factor <= 0) {
            factor = DEFAULT_LOAD_FACTOR;
        }

        int minimumCapacity = 1;
        capacity -= 1;
        while (capacity != 0) {
            capacity >>= 1;
            minimumCapacity <<= 1;
        }
        capacity = minimumCapacity;

        this.factor = factor;
        this.capacity = capacity;
        this.threshold = (int) (capacity * factor);

        this.table = new Node[this.capacity];
    }

    public MyHashMap() {
        this(DEFAULT_CAPACITY, DEFAULT_LOAD_FACTOR);
    }

    private class Node {
        private int key;
        private int value;
        private Node next;

        Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }


    private int hash(Object key) {
        if (key == null) {
            return 0;
        }
        int hashCode = key.hashCode();
        return hashCode ^ (hashCode >>> 16);
    }

    public void put(int key, int value) {
        ensureCapacity();

        int pos = getPos(key, table);
        if (table[pos] == null) { 
            Node node = new Node(key, value);
            table[pos] = node;
            size++;
        } else {
            Node head = table[pos]; 
            Node tmp = head;
            while (tmp != null) {
                if (key == tmp.key) {
                    tmp.value = value; 
                    break;
                }
                tmp = tmp.next;
            }
            if (tmp == null) { 
                Node node = new Node(key, value); 
                node.next = head;
                table[pos] = node;
                size++;
            }
        }
    }

    @SuppressWarnings("unchecked")
    private void ensureCapacity() {
        if (size >= threshold) { 

            capacity <<= 1;
            threshold = (int) (capacity * factor);

            Node[] newTable = new Node[capacity];
            for (Node node : table) {
                Node tmp = node; 
                while (tmp != null) { 
                    int pos = getPos(tmp.key, newTable); 

                    Node next = tmp.next;

                    tmp.next = newTable[pos];
                    newTable[pos] = tmp;

                    tmp = next;
                }
            }
            table = newTable;
        }
    }

    private int getPos(Object key, Node[] table) {
        return hash(key) & (table.length - 1);
    }

    public int get(int key) {
        int pos = getPos(key, table);
        if (table[pos] == null) { 
            return -1;
        } else { 
            Node head = table[pos]; 
            while (head != null) {
                if (key == head.key) {
                    return head.value;
                }
                head = head.next;
            }
            return -1;
        }
    }

    public void remove(int key) {
        int pos = getPos(key, table);

        if (table[pos] == null) { 
            return;
        } else {
            Node head = table[pos]; 
            Node preNode = null;
            while (head != null) {
                if (key == head.key) {
                    break;
                }
                preNode = head;
                head = head.next;
            }
            if (head != null) { 
                Node next = head.next;
                head.next = null;
                if (preNode == null) { 
                    table[pos] = next;
                } else {
                    preNode.next = next;
                }
                size--;
            }
        }
    }

    public int size() {
        return size;
    }

}

/**
 * Your MyHashMap object will be instantiated and called as such:
 * MyHashMap obj = new MyHashMap();
 * obj.put(key,value);
 * int param_2 = obj.get(key);
 * obj.remove(key);
 */
```

__Python__:

```Python
class Node:
    
    def __init__(self, key=None, val=None, next=None):
        self.key = key
        self.val = val
        self.next = next

class MyHashMap:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.size = 1000
        self.h = [Node() for _ in range(self.size)]

    def put(self, key: int, value: int) -> None:
        """
        value will always be non-negative.
        """
        p = self.h[key % self.size]
        c = p.next
        while c:
            if c.key == key:
                c.val = value
                break
            p = c
            c = c.next
        else:
            p.next = Node(key, value)

    def get(self, key: int) -> int:
        """
        Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key
        """
        c = self.h[key % self.size]
        while c:
            if c.key == key:
                return c.val
            c = c.next
        return -1

    def remove(self, key: int) -> None:
        """
        Removes the mapping of the specified value key if this map contains a mapping for the key
        """
        p = self.h[key % self.size]
        c = p.next
        while c:
            if c.key == key:
                p.next = c.next
                break
            p = c
            c = c.next


# Your MyHashMap object will be instantiated and called as such:
# obj = MyHashMap()
# obj.put(key,value)
# param_2 = obj.get(key)
# obj.remove(key)
```
