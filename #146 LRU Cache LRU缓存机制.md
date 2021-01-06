# 146 LRU Cache LRU缓存机制

__Description__:
Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: get and put.

get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
put(key, value) - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

The cache is initialized with a positive capacity.

__Follow up:__
Could you do both operations in O(1) time complexity?

__Example:__

```Java
LRUCache cache = new LRUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```

__题目描述__:
运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制。它应该支持以下操作： 获取数据 get 和 写入数据 put 。

获取数据 get(key) - 如果关键字 (key) 存在于缓存中，则获取关键字的值（总是正数），否则返回 -1。
写入数据 put(key, value) - 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字/值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

__进阶：__

你是否可以在 O(1) 时间复杂度内完成这两种操作？

__示例 :__

```Java
LRUCache cache = new LRUCache( 2 /* 缓存容量 */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // 返回  1
cache.put(3, 3);    // 该操作会使得关键字 2 作废
cache.get(2);       // 返回 -1 (未找到)
cache.put(4, 4);    // 该操作会使得关键字 1 作废
cache.get(1);       // 返回 -1 (未找到)
cache.get(3);       // 返回  3
cache.get(4);       // 返回  4
```

__思路__:

采用双链表和哈希表结合的方式
哈希表查找时间为 O(1)
get函数实现思路: key不存在直接返回 -1; 否则将 key对应的 value移动到双链表开头
put函数实现思路: key已经存在, 将旧节点删除, 放入新节点; 若 cache已经满了, 删除双链表结尾, 删除 map中对应的 key, 将新节点插入到 cache, 对应的 key插入到 map
时间复杂度O(1), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class LRUCache 
{
private:
    list<pair<int, int>> l;
    unordered_map<int, list<pair<int, int>>::iterator> m;
    int cap;
public:
    LRUCache (int capacity) 
    {
        cap = capacity;
    }
    
    int get(int key) 
    {
        if (m.find(key) != m.end())
        {
            int result = (*m[key]).second;
            l.erase(m[key]);
            l.push_front(make_pair(key, result));
            m[key] = l.begin();
            return result;
        }
        return -1;
    }
    
    void put(int key, int value) 
    {
        if (m.find(key) != m.end())
        {
            l.erase(m[key]);
            l.push_front(make_pair(key, value));
            m[key] = l.begin();
        }
        else
        {
            if (l.size() == cap)
            {
                m.erase(l.back().first);
                l.pop_back();
            }
            l.push_front(make_pair(key, value));
            m[key] = l.begin();
        }
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

__Java__:

```Java
class LRUCache extends LinkedHashMap<Integer, Integer>{
    private int capacity;
    
    public LRUCache(int capacity) {
        super(capacity, 0.75F, true);
        this.capacity = capacity;
    }

    public int get(int key) {
        return super.getOrDefault(key, -1);
    }

    public void put(int key, int value) {
        super.put(key, value);
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
        return size() > capacity; 
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

__Python__:

```Python
class LRUCache(dict):
    def __init__(self, capacity: int):
        self.c = capacity

    def get(self, key: int) -> int:
        if key in self:
            self[key] = self.pop(key)
            return self[key]    
        return -1    

    def put(self, key: int, value: int) -> None:
        key in self and self.pop(key) 
        self[key] = value
        len(self) > self.c and self.pop(next(iter(self)))

# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```
