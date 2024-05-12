# 2102 Sequentially Ordinal Rank Tracker 序列顺序查询

__Description:__

A scenic location is represented by its `name` and attractiveness `score`, where `name` is a __unique__ string among all locations and `score` is an integer. Locations can be ranked from the best to the worst. The __higher__ the score, the better the location. If the scores of two locations are equal, then the location with the __lexicographically smaller__ name is better.

You are building a system that tracks the ranking of locations with the system initially starting with no locations. It supports:

- __Adding__ scenic locations, __one at a time__.
- __Querying__ the `i ^ th` __best__ location of __all locations already added__, where `i` is the number of times the system has been queried (including the current query).
- For example, when the system is queried for the `4 ^ th` time, it returns the `4 ^ th` best location of all locations already added.

Note that the test data are generated so that at any time, the number of queries does not exceed the number of locations added to the system.

Implement the `SORTracker` class:

- `SORTracker()` Initializes the tracker system.
- `void add(string name, int score)` Adds a scenic location with `name` and `score` to the system.
- `string get()` Queries and returns the `i ^ th` best location, where `i` is the number of times this method has been invoked (including this invocation).

__Example:__

Example 1:

```text
Input
["SORTracker", "add", "add", "get", "add", "get", "add", "get", "add", "get", "add", "get", "get"]
[[], ["bradford", 2], ["branford", 3], [], ["alps", 2], [], ["orland", 2], [], ["orlando", 3], [], ["alpine", 2], [], []]
Output
[null, null, null, "branford", null, "alps", null, "bradford", null, "bradford", null, "bradford", "orland"]

Explanation
```

```Java
SORTracker tracker = new SORTracker(); // Initialize the tracker system.
tracker.add("bradford", 2); // Add location with name="bradford" and score=2 to the system.
tracker.add("branford", 3); // Add location with name="branford" and score=3 to the system.
tracker.get();              // The sorted locations, from best to worst, are: branford, bradford.
                            // Note that branford precedes bradford due to its higher score (3 > 2).
                            // This is the 1st time get() is called, so return the best location: "branford".
tracker.add("alps", 2);     // Add location with name="alps" and score=2 to the system.
tracker.get();              // Sorted locations: branford, alps, bradford.
                            // Note that alps precedes bradford even though they have the same score (2).
                            // This is because "alps" is lexicographically smaller than "bradford".
                            // Return the 2nd best location "alps", as it is the 2nd time get() is called.
tracker.add("orland", 2);   // Add location with name="orland" and score=2 to the system.
tracker.get();              // Sorted locations: branford, alps, bradford, orland.
                            // Return "bradford", as it is the 3rd time get() is called.
tracker.add("orlando", 3);  // Add location with name="orlando" and score=3 to the system.
tracker.get();              // Sorted locations: branford, orlando, alps, bradford, orland.
                            // Return "bradford".
tracker.add("alpine", 2);   // Add location with name="alpine" and score=2 to the system.
tracker.get();              // Sorted locations: branford, orlando, alpine, alps, bradford, orland.
                            // Return "bradford".
tracker.get();              // Sorted locations: branford, orlando, alpine, alps, bradford, orland.
                            // Return "orland".
```

__Constraints:__

- `name` consists of lowercase English letters, and is unique among all locations.
- `1 <= name.length <= 10`
- `1 <= score <= 10 ^ 5`
- At any time, the number of calls to `get` does not exceed the number of calls to `add`.
- At most `4 * 10 ^ 4` calls __in total__ will be made to `add` and `get`.

__题目描述:__

一个观光景点由它的名字 `name` 和景点评分 `score` 组成，其中 `name` 是所有观光景点中 __唯一__ 的字符串， `score` 是一个整数。景点按照最好到最坏排序。景点评分 __越高__ ，这个景点越好。如果有两个景点的评分一样，那么 __字典序较小__ 的景点更好。

你需要搭建一个系统，查询景点的排名。初始时系统里没有任何景点。这个系统支持：

- __添加__ 景点，每次添加 __一个__ 景点。
- __查询__ 已经添加景点中第 `i` __好__ 的景点，其中 `i` 是系统目前位置查询的次数（包括当前这一次）。
- 比方说，如果系统正在进行第 `4` 次查询，那么需要返回所有已经添加景点中第 `4` 好的。

注意，测试数据保证 任意查询时刻 ，查询次数都 不超过 系统中景点的数目。

请你实现 `SORTracker` 类:

- `SORTracker()` 初始化系统。
- `void add(string name, int score)` 向系统中添加一个名为 `name` 评分为 `score` 的景点。
- `string get()` 查询第 `i` 好的景点，其中 `i` 是目前系统查询的次数（包括当前这次查询）。

__示例:__

示例：

```text
输入：
["SORTracker", "add", "add", "get", "add", "get", "add", "get", "add", "get", "add", "get", "get"]
[[], ["bradford", 2], ["branford", 3], [], ["alps", 2], [], ["orland", 2], [], ["orlando", 3], [], ["alpine", 2], [], []]
输出：
[null, null, null, "branford", null, "alps", null, "bradford", null, "bradford", null, "bradford", "orland"]

解释：
```

```Java
SORTracker tracker = new SORTracker(); // 初始化系统
tracker.add("bradford", 2); // 添加 name="bradford" 且 score=2 的景点。
tracker.add("branford", 3); // 添加 name="branford" 且 score=3 的景点。
tracker.get();              // 从好带坏的景点为：branford ，bradford 。
                            // 注意到 branford 比 bradford 好，因为它的 评分更高 (3 > 2) 。
                            // 这是第 1 次调用 get() ，所以返回最好的景点："branford" 。
tracker.add("alps", 2);     // 添加 name="alps" 且 score=2 的景点。
tracker.get();              // 从好到坏的景点为：branford, alps, bradford 。
                            // 注意 alps 比 bradford 好，虽然它们评分相同，都为 2 。
                            // 这是因为 "alps" 字典序 比 "bradford" 小。
                            // 返回第 2 好的地点 "alps" ，因为当前为第 2 次调用 get() 。
tracker.add("orland", 2);   // 添加 name="orland" 且 score=2 的景点。
tracker.get();              // 从好到坏的景点为：branford, alps, bradford, orland 。
                            // 返回 "bradford" ，因为当前为第 3 次调用 get() 。
tracker.add("orlando", 3);  // 添加 name="orlando" 且 score=3 的景点。
tracker.get();              // 从好到坏的景点为：branford, orlando, alps, bradford, orland 。
                            // 返回 "bradford".
tracker.add("alpine", 2);   // 添加 name="alpine" 且 score=2 的景点。
tracker.get();              // 从好到坏的景点为：branford, orlando, alpine, alps, bradford, orland 。
                            // 返回 "bradford" 。
tracker.get();              // 从好到坏的景点为：branford, orlando, alpine, alps, bradford, orland 。
                            // 返回 "orland" 。
```

__提示：__

- `name` 只包含小写英文字母，且每个景点名字互不相同。
- `1 <= name.length <= 10`
- `1 <= score <= 10 ^ 5`
- 任意时刻，调用 `get` 的次数都不超过调用 `add` 的次数。
- __总共__ 调用 `add` 和 `get` 不超过 `4 * 10 ^ 4`

__思路:__

```text
1. 优先队列
使用两个优先队列
每次加入队列 1
并将队列 1 最大的移动到队列 2
每次查询到时候返回队列 2 的最小值, 并将其移动到队列 1
两个函数的时间复杂度都为 O(logN), 空间复杂度为 O(N)
2. 二分查找/有序集合
使用有序集合
每次加入有序集合
并将当前指针移动到下一个位置
每次查询返回当前指针的值
两个函数的时间复杂度都为 O(logN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class SORTracker 
{
public:
    SORTracker() 
    {
        s.emplace(0, "");
        cur = s.begin();
    }

    void add(string name, int score) 
    {
        pair<int, string> p = {-score, name};
        s.emplace(p);
        if (p < *cur) --cur;
    }

    string get() 
    {
        return cur++->second;
    }
private:
    set<pair<int, string>> s;
    set<pair<int, string>>::iterator cur;
};

/**
 * Your SORTracker object will be instantiated and called as such:
 * SORTracker* obj = new SORTracker();
 * obj->add(name,score);
 * string param_2 = obj->get();
 */
```

__Java__:

```Java
class SORTracker {
    private TreeSet<String> set1, set2;
    private Map<String, Integer> scores = new HashMap<>();

    public SORTracker() {
        set1 = new TreeSet<>((a, b) -> scores.get(a).compareTo(scores.get(b)) == 0 ? a.compareTo(b) : -scores.get(a).compareTo(scores.get(b)));
        set2 = new TreeSet<>((a, b) -> scores.get(a).compareTo(scores.get(b)) == 0 ? a.compareTo(b) : -scores.get(a).compareTo(scores.get(b)));
    }
    
    public void add(String name, int score) {
        scores.put(name,score);
        set1.add(name);
        set2.add(set1.last());
        set1.remove(set1.last());
    }
    
    public String get() {
        String result = set2.first();
        set1.add(set2.first());
        set2.remove(set2.first());
        return result;
    }
}

/**
 * Your SORTracker object will be instantiated and called as such:
 * SORTracker obj = new SORTracker();
 * obj.add(name,score);
 * String param_2 = obj.get();
 */
```

__Python__:

```Python
class SORTracker:

    def __init__(self):
        self.i = -1
        self.s = []

    def add(self, name: str, score: int) -> None:
        insort(self.s, (-score, name))


    def get(self) -> str:
        self.i += 1
        return self.s[self.i][1]


# Your SORTracker object will be instantiated and called as such:
# obj = SORTracker()
# obj.add(name,score)
# param_2 = obj.get()
```
