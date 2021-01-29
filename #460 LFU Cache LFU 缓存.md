# 460 LFU Cache LFU 缓存

__Description__:
Design and implement a data structure for a Least Frequently Used (LFU) cache.

Implement the LFUCache class:

LFUCache(int capacity) Initializes the object with the capacity of the data structure.
int get(int key) Gets the value of the key if the key exists in the cache. Otherwise, returns -1.
void put(int key, int value) Update the value of the key if present, or inserts the key if not already present. When the cache reaches its capacity, it should invalidate and remove the least frequently used key before inserting a new item. For this problem, when there is a tie (i.e., two or more keys with the same frequency), the least recently used key would be invalidated.
To determine the least frequently used key, a use counter is maintained for each key in the cache. The key with the smallest use counter is the least frequently used key.

When a key is first inserted into the cache, its use counter is set to 1 (due to the put operation). The use counter for a key in the cache is incremented either a get or put operation is called on it.

__Example:__

Example 1:

Input
["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, 3, null, -1, 3, 4]

Explanation

```Java
// cnt(x) = the use counter for key x
// cache=[] will show the last used order for tiebreakers (leftmost element is  most recent)
LFUCache lfu = new LFUCache(2);
lfu.put(1, 1);   // cache=[1,_], cnt(1)=1
lfu.put(2, 2);   // cache=[2,1], cnt(2)=1, cnt(1)=1
lfu.get(1);      // return 1
                 // cache=[1,2], cnt(2)=1, cnt(1)=2
lfu.put(3, 3);   // 2 is the LFU key because cnt(2)=1 is the smallest, invalidate 2.
                 // cache=[3,1], cnt(3)=1, cnt(1)=2
lfu.get(2);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,1], cnt(3)=2, cnt(1)=2
lfu.put(4, 4);   // Both 1 and 3 have the same cnt, but 1 is LRU, invalidate 1.
                 // cache=[4,3], cnt(4)=1, cnt(3)=2
lfu.get(1);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,4], cnt(4)=1, cnt(3)=3
lfu.get(4);      // return 4
                 // cache=[3,4], cnt(4)=2, cnt(3)=3
```

__Constraints:__

0 <= capacity, key, value <= 10^4
At most 105 calls will be made to get and put.

__Follow up:__ Could you do both operations in O(1) time complexity?

__题目描述__:
请你为 最不经常使用（LFU）缓存算法设计并实现数据结构。

实现 LFUCache 类：

LFUCache(int capacity) - 用数据结构的容量 capacity 初始化对象
int get(int key) - 如果键存在于缓存中，则获取键的值，否则返回 -1。
void put(int key, int value) - 如果键已存在，则变更其值；如果键不存在，请插入键值对。当缓存达到其容量时，则应该在插入新项之前，使最不经常使用的项无效。在此问题中，当存在平局（即两个或更多个键具有相同使用频率）时，应该去除 最久未使用 的键。
注意「项的使用次数」就是自插入该项以来对其调用 get 和 put 函数的次数之和。使用次数会在对应项被移除后置为 0 。

为了确定最不常使用的键，可以为缓存中的每个键维护一个 使用计数器 。使用计数最小的键是最久未使用的键。

当一个键首次插入到缓存中时，它的使用计数器被设置为 1 (由于 put 操作)。对缓存中的键执行 get 或 put 操作，使用计数器的值将会递增。

__示例 :__

示例：

输入：
["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]
输出：
[null, null, null, 1, null, -1, 3, null, -1, 3, 4]

解释：

```Java
// cnt(x) = 键 x 的使用计数
// cache=[] 将显示最后一次使用的顺序（最左边的元素是最近的）
LFUCache lFUCache = new LFUCache(2);
lFUCache.put(1, 1);   // cache=[1,_], cnt(1)=1
lFUCache.put(2, 2);   // cache=[2,1], cnt(2)=1, cnt(1)=1
lFUCache.get(1);      // 返回 1
                      // cache=[1,2], cnt(2)=1, cnt(1)=2
lFUCache.put(3, 3);   // 去除键 2 ，因为 cnt(2)=1 ，使用计数最小
                      // cache=[3,1], cnt(3)=1, cnt(1)=2
lFUCache.get(2);      // 返回 -1（未找到）
lFUCache.get(3);      // 返回 3
                      // cache=[3,1], cnt(3)=2, cnt(1)=2
lFUCache.put(4, 4);   // 去除键 1 ，1 和 3 的 cnt 相同，但 1 最久未使用
                      // cache=[4,3], cnt(4)=1, cnt(3)=2
lFUCache.get(1);      // 返回 -1（未找到）
lFUCache.get(3);      // 返回 3
                      // cache=[3,4], cnt(4)=1, cnt(3)=3
lFUCache.get(4);      // 返回 4
                      // cache=[3,4], cnt(4)=2, cnt(3)=3
```

__提示：__

0 <= capacity, key, value <= 10^4
最多调用 105 次 get 和 put 方法

__进阶：__ 你可以为这两种操作设计时间复杂度为 O(1) 的实现吗？

__思路__:

用两个双向链表记录频率最大的数据和频率最小的数据
cache中存储数据和对应的节点 Node
Node记录值, 频率和前后的节点的信息
时间复杂度O(1), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class LFUCache 
{
private:
    struct Node 
    {
        int key, value, freq;
        Node *pre, *post;
        Node() : key(-1), value(-1), freq(0), pre(NULL), post(NULL) {};
        Node(int k, int v, int f) : key(k), value(v), freq(f), pre(NULL), post(NULL) {};
    };

    struct DLinkedList 
    {
        Node *head, *tail;
        int size;
        DLinkedList() 
        {
            head = new Node();
            tail = new Node();
            head -> post = tail;
            tail -> pre = head;
            size = 0;
        }
        void remove(Node *node) 
        {
            node -> post -> pre = node -> pre;
            node -> pre -> post = node -> post;
            --size;
        }

        Node *pop_tail() 
        {
            auto node = tail -> pre;
            remove(node);
            return node;
        }

        void add_front(Node *node) 
        {
            node -> pre = head;
            node -> post = head -> post;
            node -> pre -> post = node;
            node -> post -> pre = node;
            ++size;
        }

        ~DLinkedList() 
        {
            size = 0;
            delete head;
            delete tail;
        }
    };

    int capacity, size;
    map<int, DLinkedList> freq_map;
    map<int, Node*> cache;
    
    void freq_add(Node *node) 
    {
        freq_map[node -> freq].remove(node);
        if (!(freq_map[node -> freq].size)) freq_map.erase(node -> freq);
        ++node -> freq;
        freq_map[node -> freq].add_front(node);
    }
    
    void add(Node *node) 
    {
        cache[node -> key] = node;
        freq_map[node -> freq].add_front(node);
        ++size;
    }
    
    void pop() 
    {
        if (freq_map.empty()) return;
        auto it = freq_map.begin();
        auto node = it -> second.pop_tail();
        if (!it -> second.size) freq_map.erase(it);
        cache.erase(node -> key);
        delete node;
        --size;
    }
public:    
    LFUCache(int capacity) 
    {
        this -> capacity = capacity;
        this -> size = 0;
    }
    
    int get(int key) 
    {
        if (!cache.count(key)) return -1;
        auto node = cache[key];
        freq_add(node);
        return node -> value;
    }
    
    void put(int key, int value) 
    {
        if (!capacity) return;
        if (!cache.count(key)) 
        {
            if (size == capacity) pop();
            auto node = new Node(key, value, 1);
            add(node);
        } 
        else 
        {
            auto node = cache[key];
            node -> value = value;
            freq_add(node);
        }
    }
};

/**
  *Your LFUCache object will be instantiated and called as such:
  *LFUCache *obj = new LFUCache(capacity);
  *int param_1 = obj->get(key);
  *obj->put(key,value);
 */
```

__Java__:

```Java
class LFUCache {

    private Map<Integer, Node> cache;
    DLinkedList first, last;
    int size, capacity;
    
    public LFUCache(int capacity) {
        cache = new HashMap<>(capacity);
        first = new DLinkedList();
        last = new DLinkedList();
        first.post = last;
        last.pre = first;
        this.capacity = capacity;
    }
    
    public int get(int key) {
        Node node = cache.get(key);
        if (node == null) return -1;
        freqAdd(node);
        return node.value;
    }
    
    public void put(int key, int value) {
        if (capacity == 0) return;
        Node node = cache.get(key);
        if (node != null) {
            node.value = value;
            freqAdd(node);
        } else {
            if (size == capacity) {
                cache.remove(last.pre.tail.pre.key);
                last.removeNode(last.pre.tail.pre);
                --size;
                if (last.pre.head.post == last.pre.tail) removeDLinkedList(last.pre);
            }
            Node cur = new Node(key, value);
            cache.put(key, cur);
            if (last.pre.freq != 1) {
                DLinkedList dLinkedList = new DLinkedList(1);
                addDLinkedList(dLinkedList, last.pre);
                dLinkedList.addNode(cur);
            } else last.pre.addNode(cur);
            ++size;
        }
    }
    
    private void freqAdd(Node node) {
        DLinkedList dLinkedList = node.dLinkedList;
        DLinkedList preLinkedList = dLinkedList.pre;
        dLinkedList.removeNode(node);
        if (dLinkedList.head.post == dLinkedList.tail) removeDLinkedList(dLinkedList);
        ++node.freq;
        if (preLinkedList.freq != node.freq) {
            DLinkedList newDLinkedList = new DLinkedList(node.freq);
            addDLinkedList(newDLinkedList, preLinkedList);
            newDLinkedList.addNode(node);
        } else preLinkedList.addNode(node);
    }
    
    private void addDLinkedList(DLinkedList newDLinkedList, DLinkedList preLinkedList) {
        newDLinkedList.post = preLinkedList.post;
        newDLinkedList.post.pre = newDLinkedList;
        newDLinkedList.pre = preLinkedList;
        preLinkedList.post = newDLinkedList; 
    }
    
    private void removeDLinkedList(DLinkedList dLinkedList) {
        dLinkedList.pre.post = dLinkedList.post;
        dLinkedList.post.pre = dLinkedList.pre;
    }
}

class Node {
    int key, value, freq = 1;
    Node pre, post;
    DLinkedList dLinkedList;
    
    public Node() {}
    
    public Node(int key, int value) {
        this.key = key;
        this.value = value;
    }
}

class DLinkedList {
    int freq;
    DLinkedList pre, post;
    Node head, tail;
    
    public DLinkedList() {
        head = new Node();
        tail = new Node();
        head.post = tail;
        tail.pre = head;
    }
    
    public DLinkedList(int freq) {
        head = new Node();
        tail = new Node();
        head.post = tail;
        tail.pre = head;
        this.freq = freq;
    }
    
    public void removeNode(Node node) {
        node.pre.post = node.post;
        node.post.pre = node.pre;
    }
    
    public void addNode(Node node) {
        node.post = head.post;
        head.post.pre = node;
        head.post = node;
        node.pre = head;
        node.dLinkedList = this;
    }
}

/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache obj = new LFUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

__Python__:

```Python
class LFUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.count = 0
        self.keys = {}
        

    def get(self, key: int) -> int:
        self.count += 1
        if key in self.keys:
            self.keys[key][1] += 1
            self.keys[key][2] = self.count
            return self.keys[key][0]
        return -1
        

    def put(self, key: int, value: int) -> None:
        self.count += 1
        if not self.capacity:
            return
        if key not in self.keys:
            if len(self.keys) >= self.capacity:
                min_freq, min_time, least_key = self.keys[min(self.keys, key=lambda x:  self.keys[x][1])][1], float('inf'), 0
                min_keys = [k for k in self.keys if self.keys[k][1] == min_freq]
                for k in min_keys:
                    if self.keys[k][2] < min_time:
                        min_time = self.keys[k][2]
                        least_key = k
                del self.keys[least_key]
            self.keys[key] = [value, 1, self.count]
        else:
            self.keys[key][0], self.keys[key][1], self.keys[key][2] = value, self.keys[key][1] + 1, self.count


# Your LFUCache object will be instantiated and called as such:
# obj = LFUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```
