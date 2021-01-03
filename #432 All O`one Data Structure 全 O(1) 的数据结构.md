# 432 All O`one Data Structure 全 O(1) 的数据结构

__Description__:
Implement a data structure supporting the following operations:

Inc(Key) - Inserts a new key with value 1. Or increments an existing key by 1. Key is guaranteed to be a non-empty string.
Dec(Key) - If Key's value is 1, remove it from the data structure. Otherwise decrements an existing key by 1. If the key does not exist, this function does nothing. Key is guaranteed to be a non-empty string.
GetMaxKey() - Returns one of the keys with maximal value. If no element exists, return an empty string "".
GetMinKey() - Returns one of the keys with minimal value. If no element exists, return an empty string "".

__Challenge:__ Perform all these in O(1) time complexity.

__题目描述__:
请你实现一个数据结构支持以下操作：

Inc(key) - 插入一个新的值为 1 的 key。或者使一个存在的 key 增加一，保证 key 不为空字符串。
Dec(key) - 如果这个 key 的值是 1，那么把他从数据结构中移除掉。否则使一个存在的 key 值减一。如果这个 key 不存在，这个函数不做任何事情。key 保证不为空字符串。
GetMaxKey() - 返回 key 中值最大的任意一个。如果没有元素存在，返回一个空字符串"" 。
GetMinKey() - 返回 key 中值最小的任意一个。如果没有元素存在，返回一个空字符串""。

__挑战：__

你能够以 O(1) 的时间复杂度实现所有操作吗？

__思路__:

使用两个哈希表, 一张表记录 key对应的数量, 另一张表记录值对应的 key
使用有序的双向链表, 这样获得最大最小值只需要获取 head节点/ tail节点
时间复杂度O(1), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class AllOne 
{
public:
    /** Initialize your data structure here. */
    AllOne() 
    {
        head -> next = tail;
        tail -> pre = head;
    }
    
    /** Inserts a new key <Key> with value 1. Or increments an existing key by 1. */
    void inc(string key) 
    {
        if (key_map.count(key))
        {
            int val = key_map[key]++;
            Node *node = value_map[val];
            node -> keys.erase(key);
            Node *pre_node = node -> pre;
            if (pre_node == head or pre_node -> val > val + 1)
            {
                Node *cur = new Node(val + 1);
                cur -> keys.insert(key);
                cur -> next = node;
                cur -> pre = pre_node;
                pre_node -> next = cur;
                node -> pre = cur;
                value_map[val + 1] = cur;
                pre_node = cur;
            }
            else pre_node -> keys.insert(key);
            if (node -> keys.empty())
            {
                pre_node -> next = node -> next;
                node -> next -> pre = pre_node;
                value_map.erase(val);
            }
        }
        else
        {
            key_map[key] = 1;
            if (value_map.count(1)) (value_map[1]) -> keys.insert(key);
            else
            {
                Node *cur = new Node(1);
                cur -> keys.insert(key);
                cur -> next = tail;
                cur -> pre = tail -> pre;
                tail -> pre -> next = cur;
                tail -> pre = cur;
                value_map[1] = cur;
            }
        }
    }
    
    /** Decrements an existing key by 1. If Key's value is 1, remove it from the data structure. */
    void dec(string key) 
    {
        if (key_map.count(key))
        {
            int val = key_map[key];
            Node *node = value_map[val];
            node -> keys.erase(key);
            if (val == 1) key_map.erase(key);
            else
            {
                --key_map[key];
                Node *next_node = node -> next;
                if (next_node == tail || next_node -> val < val - 1)
                {
                    Node *cur = new Node(val - 1);
                    cur -> keys.insert(key);
                    cur -> pre = node;
                    cur -> next = next_node;
                    node -> next = cur;
                    next_node -> pre = cur;
                    value_map[val - 1] = cur;
                }
                else next_node -> keys.insert(key);
            }
            if (node -> keys.empty()) {
                node -> pre -> next = node -> next;
                node -> next -> pre = node -> pre;
                value_map.erase(val);
            }
        }
    }
    
    /** Returns one of the keys with maximal value. */
    string getMaxKey() 
    {
        return head -> next == tail ? "" : *head -> next -> keys.begin();
    }
    
    /** Returns one of the keys with Minimal value. */
    string getMinKey() 
    {
        return tail -> pre == head ? "" : *tail -> pre -> keys.begin();
    }
private:
    class Node 
    {
    public:
        int val;
        Node *pre, *next;
        unordered_set<string> keys;
        Node(int v) { val = v; }
    };
    unordered_map<string, int> key_map;
    unordered_map<int, Node*> value_map;
    Node *head = new Node(-1), *tail = new Node(-1);
};

/**
 * Your AllOne object will be instantiated and called as such:
 * AllOne* obj = new AllOne();
 * obj->inc(key);
 * obj->dec(key);
 * string param_3 = obj->getMaxKey();
 * string param_4 = obj->getMinKey();
 */
```

__Java__:

```Java
public class AllOne {

    private Map<String, Integer> keyMap;
    private Map<Integer, DLinkedNode> valueMap;
    private DLinkedNode head;
    private DLinkedNode tail;

    /** Initialize your data structure here. */
    public AllOne() {
        keyMap = new HashMap<>();
        valueMap = new HashMap<>();
        head = new DLinkedNode(0);
        tail = new DLinkedNode(0);
        head.next = tail;
        tail.pre = head;
    }

    /** Inserts a new key <Key> with value 1. Or increments an existing key by 1. */
    public void inc(String key) {
        if (keyMap.containsKey(key)) {
            int val = keyMap.get(key);
            keyMap.put(key, val + 1);
            DLinkedNode node = valueMap.get(val);
            node.keys.remove(key);
            DLinkedNode preNode = node.pre;
            if (preNode == head || preNode.val > val + 1) {
                DLinkedNode cur = new DLinkedNode(val + 1);
                cur.keys.add(key);
                cur.next = node;
                cur.pre = preNode;
                preNode.next = cur;
                node.pre = cur;
                valueMap.put(val + 1, cur);
                preNode = cur;
            } else preNode.keys.add(key);
            if (node.keys.size() == 0) {
                preNode.next = node.next;
                node.next.pre = preNode;
                valueMap.remove(val);
            }
        } else {
            keyMap.put(key, 1);
            DLinkedNode node = valueMap.get(1);
            if (node == null) {
                DLinkedNode cur = new DLinkedNode(1);
                cur.keys.add(key);
                cur.next = tail;
                cur.pre = tail.pre;
                tail.pre.next = cur;
                tail.pre = cur;
                valueMap.put(1, cur);
            } else {
                node.keys.add(key);
            }
        }
    }

    /** Decrements an existing key by 1. If Key's value is 1, remove it from the data structure. */
    public void dec(String key) {
        if (keyMap.containsKey(key)) {
            int val = keyMap.get(key);
            DLinkedNode node = valueMap.get(val);
            node.keys.remove(key);
            if (val == 1) keyMap.remove(key);
            else {
                keyMap.put(key, val - 1);
                DLinkedNode nextNode = node.next;
                if (nextNode == tail || nextNode.val < val - 1) {
                    DLinkedNode cur = new DLinkedNode(val - 1);
                    cur.keys.add(key);
                    cur.pre = node;
                    cur.next = nextNode;
                    node.next = cur;
                    nextNode.pre = cur;
                    valueMap.put(val - 1, cur);
                } else nextNode.keys.add(key);
            }
            if (node.keys.size() == 0) {
                node.pre.next = node.next;
                node.next.pre = node.pre;
                valueMap.remove(val);
            }
        }
    }

    /** Returns one of the keys with maximal value. */
    public String getMaxKey() {
        if (head.next == tail) return "";
        else return head.next.keys.iterator().next();
    }

    /** Returns one of the keys with Minimal value. */
    public String getMinKey() {
        if (tail.pre == head)  return "";
        else return tail.pre.keys.iterator().next();1
    }

    private class DLinkedNode {
        int val;
        Set<String> keys;
        DLinkedNode pre, next;
        public DLinkedNode(int val) {
            this.val = val;
            this.keys = new HashSet<>();
        }
    }
}

/**
 * Your AllOne object will be instantiated and called as such:
 * AllOne obj = new AllOne();
 * obj.inc(key);
 * obj.dec(key);
 * String param_3 = obj.getMaxKey();
 * String param_4 = obj.getMinKey();
 */
```

__Python__:

```Python
class AllOne:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.d = collections.defaultdict(int)
        

    def inc(self, key: str) -> None:
        """
        Inserts a new key <Key> with value 1. Or increments an existing key by 1.
        """
        self.d[key] += 1
        

    def dec(self, key: str) -> None:
        """
        Decrements an existing key by 1. If Key's value is 1, remove it from the data structure.
        """
        if key in self.d:
            if self.d[key] == 1:
                self.d.pop(key)
            else:
                self.d[key] -= 1

    def getMaxKey(self) -> str:
        """
        Returns one of the keys with maximal value.
        """
        return max(self.d.items(), key=lambda x: x[1], default=[""])[0]
        

    def getMinKey(self) -> str:
        """
        Returns one of the keys with Minimal value.
        """
        return min(self.d.items(), key=lambda x: x[1], default=[""])[0]



# Your AllOne object will be instantiated and called as such:
# obj = AllOne()
# obj.inc(key)
# obj.dec(key)
# param_3 = obj.getMaxKey()
# param_4 = obj.getMinKey()
```
