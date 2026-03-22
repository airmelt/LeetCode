# 381 Insert Delete GetRandom O(1) - Duplicates allowed O(1) 时间插入、删除和获取随机元素 - 允许重复

__Description__:
Design a data structure that supports all following operations in average O(1) time.

__Note:__
Duplicate elements are allowed.

insert(val): Inserts an item val to the collection.
remove(val): Removes an item val from the collection if present.
getRandom: Returns a random element from current collection of elements. The probability of each element being returned is linearly related to the number of same value the collection contains.

__Example:__

```Java
// Init an empty collection.
RandomizedCollection collection = new RandomizedCollection();

// Inserts 1 to the collection. Returns true as the collection did not contain 1.
collection.insert(1);

// Inserts another 1 to the collection. Returns false as the collection contained 1. Collection now contains [1,1].
collection.insert(1);

// Inserts 2 to the collection, returns true. Collection now contains [1,1,2].
collection.insert(2);

// getRandom should return 1 with the probability 2/3, and returns 2 with the probability 1/3.
collection.getRandom();

// Removes 1 from the collection, returns true. Collection now contains [1,2].
collection.remove(1);

// getRandom should return 1 and 2 both equally likely.
collection.getRandom();
```

__题目描述__:
设计一个支持在平均 时间复杂度 O(1) 下， 执行以下操作的数据结构。

__注意:__
允许出现重复元素。

insert(val)：向集合中插入元素 val。
remove(val)：当 val 存在时，从集合中移除一个 val。
getRandom：从现有集合中随机获取一个元素。每个元素被返回的概率应该与其在集合中的数量呈线性相关。

__示例 :__

```Java
// 初始化一个空的集合。
RandomizedCollection collection = new RandomizedCollection();

// 向集合中插入 1 。返回 true 表示集合不包含 1 。
collection.insert(1);

// 向集合中插入另一个 1 。返回 false 表示集合包含 1 。集合现在包含 [1,1] 。
collection.insert(1);

// 向集合中插入 2 ，返回 true 。集合现在包含 [1,1,2] 。
collection.insert(2);

// getRandom 应当有 2/3 的概率返回 1 ，1/3 的概率返回 2 。
collection.getRandom();

// 从集合中删除 1 ，返回 true 。集合现在包含 [1,2] 。
collection.remove(1);

// getRandom 应有相同概率返回 1 和 2 。
collection.getRandom();
```

__思路__:

使用 map记录值和下标, 将下标存入一个 set中
使用 vector(列表)记录值
插入时存入 map和插在列表的结尾, 返回 map中对应的 set的长度是否为 1
移除时如果查找 map中没有, 返回 false, 否则将列表的最后一个元素和待删除的元素交换, 更新 map对应的下标, 移除 map和列表的最后一个元素, 返回true
获取一个随机数, 范围为列表的长度值
insert()时间复杂度O(1), remove()时间复杂度O(1), getRandom()时间复杂度O(1), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class RandomizedCollection 
{
private:
    vector<int> v;
    unordered_map<int, unordered_set<int>> m;
public:
    /** Initialize your data structure here. */
    RandomizedCollection() {}
    
    /** Inserts a value to the collection. Returns true if the collection did not already contain the specified element. */
    bool insert(int val) 
    {
        m[val].emplace(v.size());
        v.emplace_back(val);
        return m[val].size() == 1;
    }
    
    /** Removes a value from the collection. Returns true if the collection contained the specified element. */
    bool remove(int val) 
    {
        if (!m.count(val)) return false;
        int last = v.size() - 1, i = *m[val].begin();
        v[i] = v.back();
        m[val].erase(i);
        m[v[i]].erase(last);
        v.pop_back();
        if (i < last) m[v[i]].emplace(i);
        if (m[val].empty()) m.erase(val);
        return true;
    }
    
    /** Get a random element from the collection. */
    int getRandom() 
    {
        return v[rand() % v.size()];
    }
};

/**
 * Your RandomizedCollection object will be instantiated and called as such:
 * RandomizedCollection* obj = new RandomizedCollection();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */
```

__Java__:

```Java
class RandomizedCollection {
    private Map<Integer, Set<Integer>> map;
    private List<Integer> list;
    
    /** Initialize your data structure here. */
    public RandomizedCollection() {
        map = new HashMap<>();
        list = new ArrayList<>();
    }
    
    /** Inserts a value to the collection. Returns true if the collection did not already contain the specified element. */
    public boolean insert(int val) {
        Set<Integer> set = map.getOrDefault(val, new HashSet<Integer>());
        set.add(list.size());
        map.put(val, set);
        list.add(val);
        return set.size() == 1;
    }
    
    /** Removes a value from the collection. Returns true if the collection contained the specified element. */
    public boolean remove(int val) {
        if (!map.containsKey(val)) return false;
        Iterator<Integer> it = map.get(val).iterator();  
        int i = it.next(), last = list.get(list.size() - 1);
        list.set(i, last);
        map.get(val).remove(i);
        map.get(last).remove(list.size() - 1);
        if (i < list.size() - 1) map.get(last).add(i);
        if (map.get(val).isEmpty()) map.remove(val);
        list.remove(list.size() - 1);
        return true;
    }
    
    /** Get a random element from the collection. */
    public int getRandom() {
        return list.get((int)(Math.random() * list.size()));
    }
}

/**
 * Your RandomizedCollection object will be instantiated and called as such:
 * RandomizedCollection obj = new RandomizedCollection();
 * boolean param_1 = obj.insert(val);
 * boolean param_2 = obj.remove(val);
 * int param_3 = obj.getRandom();
 */
```

__Python__:

```Python
class RandomizedCollection:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.hash = {}
        self.stack = []
        

    def insert(self, val: int) -> bool:
        """
        Inserts a value to the collection. Returns true if the collection did not already contain the specified element.
        """
        self.hash[val] = self.hash.get(val, set()) | {len(self.stack)}
        self.stack.append(val)
        return len(self.hash[val]) == 1
        

    def remove(self, val: int) -> bool:
        """
        Removes a value from the collection. Returns true if the collection contained the specified element.
        """
        if val not in self.hash:
            return False
        i, last = self.hash[val].pop(), len(self.stack) - 1
        if not self.hash[val]:
            del self.hash[val]
        if i != last:
            self.stack[i] = self.stack[last]
            self.hash[self.stack[i]].remove(last)
            self.hash[self.stack[i]].add(i)
        self.stack.pop()
        return True


    def getRandom(self) -> int:
        """
        Get a random element from the collection.
        """
        return self.stack[random.randint(0, len(self.stack) - 1)]


# Your RandomizedCollection object will be instantiated and called as such:
# obj = RandomizedCollection()
# param_1 = obj.insert(val)
# param_2 = obj.remove(val)
# param_3 = obj.getRandom()
```
