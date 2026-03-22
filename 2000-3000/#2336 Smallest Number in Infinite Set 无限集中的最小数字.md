# 2336 Smallest Number in Infinite Set 无限集中的最小数字

__Description:__

You have a set which contains all positive integers `[1, 2, 3, 4, 5, ...]`.

Implement the `SmallestInfiniteSet` class:

- `SmallestInfiniteSet()` Initializes the __SmallestInfiniteSet__ object to contain __all__ positive integers.
- `int popSmallest()` __Removes__ and returns the smallest integer contained in the infinite set.
- `void addBack(int num)` __Adds__ a positive integer `num` back into the infinite set, if it is __not__ already in the infinite set.

__Example:__

Example 1:

```text
Input
["SmallestInfiniteSet", "addBack", "popSmallest", "popSmallest", "popSmallest", "addBack", "popSmallest", "popSmallest", "popSmallest"]
[[], [2], [], [], [], [1], [], [], []]
Output
[null, null, 1, 2, 3, null, 1, 4, 5]

Explanation
```

```Java
SmallestInfiniteSet smallestInfiniteSet = new SmallestInfiniteSet();
smallestInfiniteSet.addBack(2);    // 2 is already in the set, so no change is made.
smallestInfiniteSet.popSmallest(); // return 1, since 1 is the smallest number, and remove it from the set.
smallestInfiniteSet.popSmallest(); // return 2, and remove it from the set.
smallestInfiniteSet.popSmallest(); // return 3, and remove it from the set.
smallestInfiniteSet.addBack(1);    // 1 is added back to the set.
smallestInfiniteSet.popSmallest(); // return 1, since 1 was added back to the set and
                                   // is the smallest number, and remove it from the set.
smallestInfiniteSet.popSmallest(); // return 4, and remove it from the set.
smallestInfiniteSet.popSmallest(); // return 5, and remove it from the set.
```

__Constraints:__

- `1 <= num <= 1000`
- At most `1000` calls will be made __in total__ to `popSmallest` and `addBack`.

__题目描述:__

现有一个包含所有正整数的集合 `[1, 2, 3, 4, 5, ...]` 。

实现 `SmallestInfiniteSet` 类:

- `SmallestInfiniteSet()` 初始化 __SmallestInfiniteSet__ 对象以包含 __所有__ 正整数。
- `int popSmallest()` __移除__ 并返回该无限集中的最小整数。
- `void addBack(int num)` 如果正整数 `num` __不__ 存在于无限集中，则将一个 `num` __添加__ 到该无限集中。

__示例:__

示例：

```text
输入
["SmallestInfiniteSet", "addBack", "popSmallest", "popSmallest", "popSmallest", "addBack", "popSmallest", "popSmallest", "popSmallest"]
[[], [2], [], [], [], [1], [], [], []]
输出
[null, null, 1, 2, 3, null, 1, 4, 5]

解释
```

```Java
SmallestInfiniteSet smallestInfiniteSet = new SmallestInfiniteSet();
smallestInfiniteSet.addBack(2);    // 2 已经在集合中，所以不做任何变更。
smallestInfiniteSet.popSmallest(); // 返回 1 ，因为 1 是最小的整数，并将其从集合中移除。
smallestInfiniteSet.popSmallest(); // 返回 2 ，并将其从集合中移除。
smallestInfiniteSet.popSmallest(); // 返回 3 ，并将其从集合中移除。
smallestInfiniteSet.addBack(1);    // 将 1 添加到该集合中。
smallestInfiniteSet.popSmallest(); // 返回 1 ，因为 1 在上一步中被添加到集合中，
                                   // 且 1 是最小的整数，并将其从集合中移除。
smallestInfiniteSet.popSmallest(); // 返回 4 ，并将其从集合中移除。
smallestInfiniteSet.popSmallest(); // 返回 5 ，并将其从集合中移除。
```

__提示：__

- `1 <= num <= 1000`
- 最多调用 `popSmallest` 和 `addBack` 方法 __共计__ `1000` 次

__思路:__

```text
有序集合(小根堆)
由于 popSmallest 操作最多调用 1000 次
因此可以使用有序集合预先存储 1 ~ 1000 的所有正整数
popSmallest 操作直接返回有序集合的第一个元素
addBack 操作直接将元素插入有序集合
预处理时间复杂度为 O(C), 空间复杂度为 O(C), 这里 C = 1000
popSmallest() 时间复杂度为 O(logC)
addBack() 时间复杂度为 O(logC)
```

__代码:__

__C++__:

```C++
class SmallestInfiniteSet 
{
public:
    SmallestInfiniteSet() 
    {
        for (int i = 1; i < 1001; ++i) s.insert(i);
    }
    
    int popSmallest() 
    {
        int result = *s.begin();
        s.erase(s.begin());
        return result;
    }
    
    void addBack(int num) 
    {
        s.insert(num);
    }
private:
    set<int> s;
};

/**
 * Your SmallestInfiniteSet object will be instantiated and called as such:
 * SmallestInfiniteSet* obj = new SmallestInfiniteSet();
 * int param_1 = obj->popSmallest();
 * obj->addBack(num);
 */
```

__Java__:

```Java
class SmallestInfiniteSet {
    private static TreeSet<Integer> ts = new TreeSet<Integer>();

    public SmallestInfiniteSet() {
        for (int i = 1; i < 1001; i++) ts.add(i);
    }
    
    public int popSmallest() {
        return ts.pollFirst();
    }
    
    public void addBack(int num) {
        ts.add(num);
    }
}

/**
 * Your SmallestInfiniteSet object will be instantiated and called as such:
 * SmallestInfiniteSet obj = new SmallestInfiniteSet();
 * int param_1 = obj.popSmallest();
 * obj.addBack(num);
 */
```

__Python__:

```Python
from sortedcontainers import SortedSet
class SmallestInfiniteSet:

    def __init__(self):
        self.data = SortedSet(range(1, 1001))


    def popSmallest(self) -> int:
        return self.data.pop(0)


    def addBack(self, num: int) -> None:
        self.data.add(num)



# Your SmallestInfiniteSet object will be instantiated and called as such:
# obj = SmallestInfiniteSet()
# param_1 = obj.popSmallest()
# obj.addBack(num)
```
