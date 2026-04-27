# 2276 Count Integers in Intervals 统计区间中的整数数目

__Description:__

Given an empty set of intervals, implement a data structure that can:

- __Add__ an interval to the set of intervals.
- __Count__ the number of integers that are present in __at least one__ interval.

Implement the `CountIntervals` class:

- `CountIntervals()` Initializes the object with an empty set of intervals.
- `void add(int left, int right)` Adds the interval `[left, right]` to the set of intervals.
- `int count()` Returns the number of integers that are present in __at least one__ interval.

__Note__ that an interval `[left, right]` denotes all the integers `x` where `left <= x <= right`.

__Example:__

Example 1:

```text
Input
["CountIntervals", "add", "add", "count", "add", "count"]
[[], [2, 3], [7, 10], [], [5, 8], []]
Output
[null, null, null, 6, null, 8]

Explanation
```

```Java
CountIntervals countIntervals = new CountIntervals(); // initialize the object with an empty set of intervals. 
countIntervals.add(2, 3);  // add [2, 3] to the set of intervals.
countIntervals.add(7, 10); // add [7, 10] to the set of intervals.
countIntervals.count();    // return 6
                           // the integers 2 and 3 are present in the interval [2, 3].
                           // the integers 7, 8, 9, and 10 are present in the interval [7, 10].
countIntervals.add(5, 8);  // add [5, 8] to the set of intervals.
countIntervals.count();    // return 8
                           // the integers 2 and 3 are present in the interval [2, 3].
                           // the integers 5 and 6 are present in the interval [5, 8].
                           // the integers 7 and 8 are present in the intervals [5, 8] and [7, 10].
                           // the integers 9 and 10 are present in the interval [7, 10].
```

__Constraints:__

- `1 <= left <= right <= 10 ^ 9`
- At most `10 ^ 5` calls __in total__ will be made to `add` and `count`.
- At least __one__ call will be made to `count`.

__题目描述:__

给你区间的 空 集，请你设计并实现满足要求的数据结构：

- __新增:__添加一个区间到这个区间集合中。
- __统计:__计算出现在 __至少一个__ 区间中的整数个数。

实现 `CountIntervals` 类:

- `CountIntervals()` 使用区间的空集初始化对象
- `void add(int left, int right)` 添加区间 `[left, right]` 到区间集合之中。
- `int count()` 返回出现在 __至少一个__ 区间中的整数个数。

__注意:__区间 `[left, right]` 表示满足 `left <= x <= right` 的所有整数 `x` 。

__示例:__

示例 1：

```text
输入
["CountIntervals", "add", "add", "count", "add", "count"]
[[], [2, 3], [7, 10], [], [5, 8], []]
输出
[null, null, null, 6, null, 8]

解释
```

```Java
CountIntervals countIntervals = new CountIntervals(); // 用一个区间空集初始化对象
countIntervals.add(2, 3);  // 将 [2, 3] 添加到区间集合中
countIntervals.add(7, 10); // 将 [7, 10] 添加到区间集合中
countIntervals.count();    // 返回 6
                           // 整数 2 和 3 出现在区间 [2, 3] 中
                           // 整数 7、8、9、10 出现在区间 [7, 10] 中
countIntervals.add(5, 8);  // 将 [5, 8] 添加到区间集合中
countIntervals.count();    // 返回 8
                           // 整数 2 和 3 出现在区间 [2, 3] 中
                           // 整数 5 和 6 出现在区间 [5, 8] 中
                           // 整数 7 和 8 出现在区间 [5, 8] 和区间 [7, 10] 中
                           // 整数 9 和 10 出现在区间 [7, 10] 中
```

__提示：__

- `1 <= left <= right <= 10 ^ 9`
- 最多调用  `add` 和 `count` 方法 __总计__ `10 ^ 5` 次
- 调用 `count` 方法至少一次

__思路:__

```text
有序列表
用平衡树维护区间
每次 add 区间时, 找到被覆盖的区间并删除
然后将删除的区间与 [left, right] 合并成一个大区间并插入平衡树
使用平衡树的 key 存储区间的右端点, value 存储区间的左端点
每次查询的时候查询 key >= left 的区间
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class CountIntervals 
{
public:
    CountIntervals() {}
    
    void add(int left, int right) 
    {
        int l = left, r = right;
        auto it = st.lower_bound(make_pair(left - 1, 0x3f3f3f3f));
        while (it != st.end() and it -> second <= right)
        {
            l = min(l, it -> second);
            r = max(r, it -> first);
            result -= it -> first - it -> second + 1;
            st.erase(it++);
        }
        result += r - l + 1;
        st.insert(make_pair(r, l));
    }
    
    int count() 
    {
        return result;
    }
private:
    int result = 0;
    set<pair<int, int>> st;
};

/**
 * Your CountIntervals object will be instantiated and called as such:
 * CountIntervals* obj = new CountIntervals();
 * obj->add(left,right);
 * int param_2 = obj->count();
 */
```

__Java__:

```Java
class CountIntervals {
    private TreeMap<Integer, Integer> map = new TreeMap<>();
    private int result;

    public CountIntervals() {}

    public void add(int left, int right) {
        for (var e = map.ceilingEntry(left); e != null && e.getValue() <= right; e = map.ceilingEntry(left)) {
            int l = e.getValue(), r = e.getKey();
            left = Math.min(left, l);
            right = Math.max(right, r);
            result -= r - l + 1;
            map.remove(r);
        }
        result += right - left + 1;
        map.put(right, left);
    }

    public int count() { 
        return result; 
    }
}

/**
 * Your CountIntervals object will be instantiated and called as such:
 * CountIntervals obj = new CountIntervals();
 * obj.add(left,right);
 * int param_2 = obj.count();
 */
```

__Python__:

```Python
from sortedcontainers import SortedDict
class CountIntervals:

    def __init__(self):
        self.d = SortedDict()
        self.result = 0


    def add(self, left: int, right: int) -> None:
        it = self.d.bisect_left(left)
        while it < len(self.d) and self.d.values()[it] <= right:
            r, l = self.d.items()[it]
            left, right = min(left, l), max(right, r)
            self.result -= r - l + 1
            self.d.popitem(it)
        self.result += right - left + 1
        self.d[right] = left


    def count(self) -> int:
        return self.result



# Your CountIntervals object will be instantiated and called as such:
# obj = CountIntervals()
# obj.add(left,right)
# param_2 = obj.count()
```
