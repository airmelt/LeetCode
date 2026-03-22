# 1825 Finding MK Average 求出 MK 平均值

__Description:__

You are given two integers, `m` and `k`, and a stream of integers. You are tasked to implement a data structure that calculates the __MKAverage__ for the stream.

The MKAverage can be calculated using these steps:

Implement the `MKAverage` class:

- `MKAverage(int m, int k)` Initializes the __MKAverage__ object with an empty stream and the two integers `m` and `k`.
- `void addElement(int num)` Inserts a new element `num` into the stream.
- `int calculateMKAverage()` Calculates and returns the __MKAverage__ for the current stream __rounded down to the nearest integer__.

__Example:__

Example 1:

```text
Input
["MKAverage", "addElement", "addElement", "calculateMKAverage", "addElement", "calculateMKAverage", "addElement", "addElement", "addElement", "calculateMKAverage"]
[[3, 1], [3], [1], [], [10], [], [5], [5], [5], []]
Output
[null, null, null, -1, null, 3, null, null, null, 5]
```

Explanation

```Java
MKAverage obj = new MKAverage(3, 1); 
obj.addElement(3);    // current elements are [3]
obj.addElement(1);    // current elements are [3,1]
obj.calculateMKAverage(); // return -1, because m = 3 and only 2 elements exist.
obj.addElement(10);    // current elements are [3,1,10]
obj.calculateMKAverage(); // The last 3 elements are [3,1,10].
             // After removing smallest and largest 1 element the container will be [3].
             // The average of [3] equals 3/1 = 3, return 3
obj.addElement(5);    // current elements are [3,1,10,5]
obj.addElement(5);    // current elements are [3,1,10,5,5]
obj.addElement(5);    // current elements are [3,1,10,5,5,5]
obj.calculateMKAverage(); // The last 3 elements are [5,5,5].
             // After removing smallest and largest 1 element the container will be [5].
             // The average of [5] equals 5/1 = 5, return 5
```

__Constraints:__

- `3 <= m <= 10 ^ 5`
- `1 <= k*2 < m`
- `1 <= num <= 10 ^ 5`
- At most `10 ^ 5` calls will be made to `addElement` and `calculateMKAverage`.

__题目描述:__

给你两个整数 `m` 和 `k` ，以及数据流形式的若干整数。你需要实现一个数据结构，计算这个数据流的 _MK 平均值_ 。

MK 平均值 按照如下步骤计算：

请你实现 `MKAverage` 类:

- `MKAverage(int m, int k)` 用一个空的数据流和两个整数 `m` 和 `k` 初始化 __MKAverage__ 对象。
- `void addElement(int num)` 往数据流中插入一个新的元素 `num` 。
- `int calculateMKAverage()` 对当前的数据流计算并返回 __MK 平均数__ ，结果需 __向下取整到最近的整数__ 。

__示例:__

示例 1：

```text
输入：
["MKAverage", "addElement", "addElement", "calculateMKAverage", "addElement", "calculateMKAverage", "addElement", "addElement", "addElement", "calculateMKAverage"]
[[3, 1], [3], [1], [], [10], [], [5], [5], [5], []]
输出：
[null, null, null, -1, null, 3, null, null, null, 5]
```

解释：

```Java
MKAverage obj = new MKAverage(3, 1); 
obj.addElement(3);        // 当前元素为 [3]
obj.addElement(1);        // 当前元素为 [3,1]
obj.calculateMKAverage(); // 返回 -1 ，因为 m = 3 ，但数据流中只有 2 个元素
obj.addElement(10);       // 当前元素为 [3,1,10]
obj.calculateMKAverage(); // 最后 3 个元素为 [3,1,10]
                          // 删除最小以及最大的 1 个元素后，容器为 [3]
                          // [3] 的平均值等于 3/1 = 3 ，故返回 3
obj.addElement(5);        // 当前元素为 [3,1,10,5]
obj.addElement(5);        // 当前元素为 [3,1,10,5,5]
obj.addElement(5);        // 当前元素为 [3,1,10,5,5,5]
obj.calculateMKAverage(); // 最后 3 个元素为 [5,5,5]
                          // 删除最小以及最大的 1 个元素后，容器为 [5]
                          // [5] 的平均值等于 5/1 = 5 ，故返回 5
```

__提示：__

- `3 <= m <= 10 ^ 5`
- `1 <= k*2 < m`
- `1 <= num <= 10 ^ 5`
- `addElement` 与 `calculateMKAverage` 总操作次数不超过 `10 ^ 5` 次。

__思路:__

```text
有序哈希表
类似流的中位数
维护两个大小为 k 的有序哈希表 low, high 和另一个有序哈希表 mid
low 记录最小的 k 个值
high 记录最大的 k 个值
再维护一个大小为 m 的双端队列
同时用两个变量 size1 和 size3 分别记录两个大小为 k 的有序哈希表中元素的个数
用一个变量 s 记录 mid 中元素的和
如果 low 为空或者 num <= low 的最后一个元素, 将 num 插入 low 中, size1++
否则如果 high 为空或者 num >= high 的第一个元素, 将 num 插入 high 中, size3++
否则将 num 插入 mid 中, s += num
将 num 加入队列
如果队列大小超过 m, 将队首元素 x 弹出
并且如果 x 在 low 中, 将 low[x]--, 如果 low[x] == 0, 将 low 中 x 删除, size1--
否则如果 x 在 high 中, 将 high[x]--, 如果 high[x] == 0, 将 high 中 x 删除, size3--
否则将 mid[x]--, 如果 mid[x] == 0, 将 mid 中 x 删除, s -= x
如果 size1 > k, 将 low 中最后一个元素 x 弹出, size1--, 将 x 插入 mid 中, s += x
如果 size3 > k, 将 high 中第一个元素 x 弹出, size3--, 将 x 插入 mid 中, s += x
如果 size1 < k, 且 mid 不为空, 将 mid 中第一个元素 x 弹出, size1++, 将 x 插入 low 中, s -= x
如果 size3 < k, 且 mid 不为空, 将 mid 中最后一个元素 x 弹出, size3++, 将 x 插入 high 中, s -= x
计算平均值时比较队列的长度和 m 的关系返回 -1 或者 s / (m - (k << 1))
addElement() 函数时间复杂度为 O(logM), calculateMKAverage() 函数时间复杂度为 O(1), 空间复杂度为 O(M)
```

__代码:__

__C++__:

```C++
class MKAverage 
{
public:
    MKAverage(int _m, int _k) 
    {
        m = _m;
        k = _k;
    }
    
    void addElement(int num) 
    {
        if (q.size() == m)
        {
            int x = q.front();
            q.pop();
            if (!--mp[x]) mp.erase(x);
        }
        q.push(num);
        ++mp[num];
    }
    
    int calculateMKAverage() 
    {
        if (q.size() < m) return -1;
        int cur = 0, s = 0;
        for (const auto& [key, val] : mp)
        {
            if (cur + val <= k)
            {
                cur += val;
                continue;
            }
            if (cur >= m - k) break;
            s += key * (min(m - k, cur + val) - max(cur, k));
            cur += val;
        }
        return s / (m - (k << 1));
    }
private:
    int m, k;
    queue<int> q;
    map<int, int> mp;
};

/**
 * Your MKAverage object will be instantiated and called as such:
 * MKAverage* obj = new MKAverage(m, k);
 * obj->addElement(num);
 * int param_2 = obj->calculateMKAverage();
 */
```

__Java__:

```Java
class MKAverage {

    private int m, k, size1, size3;
    private long s;
    private Deque<Integer> q = new ArrayDeque<>();
    private TreeMap<Integer, Integer> low = new TreeMap<>(), mid = new TreeMap<>(), high = new TreeMap<>();

    public MKAverage(int m, int k) {
        this.m = m;
        this.k = k;
    }

    public void addElement(int num) {
        if (low.isEmpty() || num <= low.lastKey()) {
            low.merge(num, 1, Integer::sum);
            ++size1;
        } else if (high.isEmpty() || num >= high.firstKey()) {
            high.merge(num, 1, Integer::sum);
            ++size3;
        } else {
            mid.merge(num, 1, Integer::sum);
            s += num;
        }
        q.offer(num);
        if (q.size() > m) {
            int x = q.poll();
            if (low.containsKey(x)) {
                if (low.merge(x, -1, Integer::sum) == 0) low.remove(x);
                --size1;
            } else if (high.containsKey(x)) {
                if (high.merge(x, -1, Integer::sum) == 0) high.remove(x);
                --size3;
            } else {
                if (mid.merge(x, -1, Integer::sum) == 0) mid.remove(x);
                s -= x;
            }
        }
        while (size1-- > k) {
            int x = low.lastKey();
            if (low.merge(x, -1, Integer::sum) == 0) low.remove(x);
            mid.merge(x, 1, Integer::sum);
            s += x;
        }
        while (size3-- > k) {
            int x = high.firstKey();
            if (high.merge(x, -1, Integer::sum) == 0) high.remove(x);
            mid.merge(x, 1, Integer::sum);
            s += x;
        }
        while (++size1 < k && !mid.isEmpty()) {
            int x = mid.firstKey();
            if (mid.merge(x, -1, Integer::sum) == 0) mid.remove(x);
            s -= x;
            low.merge(x, 1, Integer::sum);
        }
        while (++size3 < k && !mid.isEmpty()) {
            int x = mid.lastKey();
            if (mid.merge(x, -1, Integer::sum) == 0) mid.remove(x);
            s -= x;
            high.merge(x, 1, Integer::sum);
        }
    }

    public int calculateMKAverage() {
        return q.size() < m ? -1 : (int) (s / (q.size() - (k << 1)));
    }
}

/**
 * Your MKAverage object will be instantiated and called as such:
 * MKAverage obj = new MKAverage(m, k);
 * obj.addElement(num);
 * int param_2 = obj.calculateMKAverage();
 */
```

__Python__:

```Python
from sortedcontainers import SortedList

class MKAverage:

    def __init__(self, m: int, k: int):
        self.m = m
        self.k = k
        self.s = None
        self.q = deque()
        self.sl = SortedList()


    def addElement(self, num: int) -> None:
        n = len(self.q)
        if n == self.m:
            x = self.q.popleft()
            if x in self.sl:
                idx = self.sl.bisect_left(x)
                if idx <= self.k - 1:
                    self.s -= self.sl[self.k]
                    self.s += self.sl[n - self.k]
                elif self.k <= idx <= n - self.k - 1:
                    self.s -= x
                    self.s += self.sl[n - self.k]
                self.sl.pop(idx)
        self.q.append(num)
        if n == self.m:
            idx = self.sl.bisect_left(num)
            if idx <= self.k - 1:
                self.s += self.sl[self.k - 1]
                self.s -= self.sl[n - self.k - 1]
            elif self.k <= idx <= n - self.k - 1:
                self.s += num
                self.s -= self.sl[n - self.k - 1] 
        self.sl.add(num)
        if n == self.m - 1:
            self.s = sum(self.sl[self.k:self.m - self.k])


    def calculateMKAverage(self) -> int:
        return -1 if len(self.q) < self.m else self.s // (self.m - (self.k << 1))



# Your MKAverage object will be instantiated and called as such:
# obj = MKAverage(m, k)
# obj.addElement(num)
# param_2 = obj.calculateMKAverage()
```
