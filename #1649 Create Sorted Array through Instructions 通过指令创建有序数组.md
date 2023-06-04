# 1649 Create Sorted Array through Instructions 通过指令创建有序数组

__Description:__

Given an integer array `instructions`, you are asked to create a sorted array from the elements in `instructions`. You start with an empty container `nums`. For each element from __left to right__ in `instructions`, insert it into `nums`. The __cost__ of each insertion is the _minimum_ of the following:

- The number of elements currently in `nums` that are __strictly less than__ `instructions[i]`.
- The number of elements currently in `nums` that are __strictly greater than__ `instructions[i]`.

For example, if inserting element `3` into `nums = [1,2,3,5]`, the __cost__ of insertion is `min(2, 1)` (elements `1` and `2` are less than `3`, element `5` is greater than `3`) and `nums` will become `[1,2,3,3,5]`.

Return _the __total cost__ to insert all elements from_ `instructions` _into_ `nums`. Since the answer may be large, return it __modulo__ `10 ^ 9 + 7`

__Example:__

Example 1:

```text
Input: instructions = [1,5,6,2]
Output: 1
Explanation: Begin with nums = [].
Insert 1 with cost min(0, 0) = 0, now nums = [1].
Insert 5 with cost min(1, 0) = 0, now nums = [1,5].
Insert 6 with cost min(2, 0) = 0, now nums = [1,5,6].
Insert 2 with cost min(1, 2) = 1, now nums = [1,2,5,6].
The total cost is 0 + 0 + 0 + 1 = 1.
```

Example 2:

```text
Input: instructions = [1,2,3,6,5,4]
Output: 3
Explanation: Begin with nums = [].
Insert 1 with cost min(0, 0) = 0, now nums = [1].
Insert 2 with cost min(1, 0) = 0, now nums = [1,2].
Insert 3 with cost min(2, 0) = 0, now nums = [1,2,3].
Insert 6 with cost min(3, 0) = 0, now nums = [1,2,3,6].
Insert 5 with cost min(3, 1) = 1, now nums = [1,2,3,5,6].
Insert 4 with cost min(3, 2) = 2, now nums = [1,2,3,4,5,6].
The total cost is 0 + 0 + 0 + 0 + 1 + 2 = 3.
```

Example 3:

```text
Input: instructions = [1,3,3,3,2,4,2,1,2]
Output: 4
Explanation: Begin with nums = [].
Insert 1 with cost min(0, 0) = 0, now nums = [1].
Insert 3 with cost min(1, 0) = 0, now nums = [1,3].
Insert 3 with cost min(1, 0) = 0, now nums = [1,3,3].
Insert 3 with cost min(1, 0) = 0, now nums = [1,3,3,3].
Insert 2 with cost min(1, 3) = 1, now nums = [1,2,3,3,3].
Insert 4 with cost min(5, 0) = 0, now nums = [1,2,3,3,3,4].
Insert 2 with cost min(1, 4) = 1, now nums = [1,2,2,3,3,3,4].
Insert 1 with cost min(0, 6) = 0, now nums = [1,1,2,2,3,3,3,4].
Insert 2 with cost min(2, 4) = 2, now nums = [1,1,2,2,2,3,3,3,4].
The total cost is 0 + 0 + 0 + 0 + 1 + 0 + 1 + 0 + 2 = 4.
```

__Constraints:__

- `1 <= instructions.length <= 10 ^ 5`
- `1 <= instructions[i] <= 10 ^ 5`

__题目描述:__

给你一个整数数组 `instructions` ，你需要根据 `instructions` 中的元素创建一个有序数组。一开始你有一个空的数组 `nums` ，你需要 __从左到右__ 遍历 `instructions` 中的元素，将它们依次插入 `nums` 数组中。每一次插入操作的 __代价__ 是以下两者的 __较小值__ :

- `nums` 中 __严格小于__  `instructions[i]` 的数字数目。
- `nums` 中 __严格大于__  `instructions[i]` 的数字数目。

比方说，如果要将 `3` 插入到 `nums = [1,2,3,5]` ，那么插入操作的 __代价__ 为 `min(2, 1)` (元素 `1` 和  `2` 小于 `3` ，元素 `5` 大于 `3`)，插入后 `nums` 变成 `[1,2,3,3,5]` 。

请你返回将 `instructions` 中所有元素依次插入 `nums` 后的 __总最小代价__ 。由于答案会很大，请将它对 `10 ^ 9 + 7` _取余_ 后返回。

__示例:__

示例 1：

```text
输入：instructions = [1,5,6,2]
输出：1
解释：一开始 nums = [] 。
插入 1 ，代价为 min(0, 0) = 0 ，现在 nums = [1] 。
插入 5 ，代价为 min(1, 0) = 0 ，现在 nums = [1,5] 。
插入 6 ，代价为 min(2, 0) = 0 ，现在 nums = [1,5,6] 。
插入 2 ，代价为 min(1, 2) = 1 ，现在 nums = [1,2,5,6] 。
总代价为 0 + 0 + 0 + 1 = 1 。
```

示例 2:

```text
输入：instructions = [1,2,3,6,5,4]
输出：3
解释：一开始 nums = [] 。
插入 1 ，代价为 min(0, 0) = 0 ，现在 nums = [1] 。
插入 2 ，代价为 min(1, 0) = 0 ，现在 nums = [1,2] 。
插入 3 ，代价为 min(2, 0) = 0 ，现在 nums = [1,2,3] 。
插入 6 ，代价为 min(3, 0) = 0 ，现在 nums = [1,2,3,6] 。
插入 5 ，代价为 min(3, 1) = 1 ，现在 nums = [1,2,3,5,6] 。
插入 4 ，代价为 min(3, 2) = 2 ，现在 nums = [1,2,3,4,5,6] 。
总代价为 0 + 0 + 0 + 0 + 1 + 2 = 3 。
```

示例 3：

```text
输入：instructions = [1,3,3,3,2,4,2,1,2]
输出：4
解释：一开始 nums = [] 。
插入 1 ，代价为 min(0, 0) = 0 ，现在 nums = [1] 。
插入 3 ，代价为 min(1, 0) = 0 ，现在 nums = [1,3] 。
插入 3 ，代价为 min(1, 0) = 0 ，现在 nums = [1,3,3] 。
插入 3 ，代价为 min(1, 0) = 0 ，现在 nums = [1,3,3,3] 。
插入 2 ，代价为 min(1, 3) = 1 ，现在 nums = [1,2,3,3,3] 。
插入 4 ，代价为 min(5, 0) = 0 ，现在 nums = [1,2,3,3,3,4] 。
插入 2 ，代价为 min(1, 4) = 1 ，现在 nums = [1,2,2,3,3,3,4] 。
插入 1 ，代价为 min(0, 6) = 0 ，现在 nums = [1,1,2,2,3,3,3,4] 。
插入 2 ，代价为 min(2, 4) = 2 ，现在 nums = [1,1,2,2,2,3,3,3,4] 。
总代价为 0 + 0 + 0 + 0 + 1 + 0 + 1 + 0 + 2 = 4 。
```

__提示：__

- `1 <= instructions.length <= 10 ^ 5`
- `1 <= instructions[i] <= 10 ^ 5`

__思路:__

```text
树状数组
每次插入的时候只需要记录比当前数字小的数字的个数以及比当前数字大的数字的个数即可
查询比当前元素小的即查询 [1, instructions[i] - 1] 的元素个数
查询比当前元素大的即查询 [instructions[i] + 1, max(instructions)] 的元素个数
将 [1, instructions[i] - 1] 的元素个数与 [instructions[i] + 1, max(instructions)] 的元素个数取最小值即可
再将 instructions[i] 插入树状数组中
时间复杂度为 O(NlogM), 空间复杂度为 O(M), 其中 M 为 instructions 中的最大值
```

__代码:__

__C++__:

```C++
class BIT 
{
public:
    BIT(int _n): n(_n), tree(_n + 1) {}

    static constexpr int lowbit(int x) 
    {
        return x & (-x);
    }
    
    void update(int x) 
    {
        while (x <= n) 
        {
            ++tree[x];
            x += lowbit(x);
        }
    }

    int query(int x) const 
    {
        int result = 0;
        while (x) 
        {
            result += tree[x];
            x -= lowbit(x);
        }
        return result;
    }
private:
    int n;
    vector<int> tree;
};

class Solution 
{
public:
    int createSortedArray(vector<int>& instructions) 
    {
        BIT bit(*max_element(instructions.begin(), instructions.end()));
        long long result = 0, MOD = 1e9 + 7;
        for (int i = 0, n = instructions.size(); i < n; i++) 
        {
            int x = instructions[i], smaller = bit.query(x - 1), larger = i - bit.query(x);
            result += min(smaller, larger);
            bit.update(x);
        }
        return result % MOD;
    }
};
```

__Java__:

```Java
class BIT {
    private int n;
    private int[] tree;
    
    public BIT(int n) {
        this.n = n;
        this.tree = new int[n + 1];
    }

    private int lowbit(int x) {
        return x & (-x);
    }
    
    public void update(int x) {
        while (x <= n) 
        {
            ++tree[x];
            x += lowbit(x);
        }
    }

    public int query(int x) {
        int result = 0;
        while (x != 0) {
            result += tree[x];
            x -= lowbit(x);
        }
        return result;
    }
}

class Solution {
    public int createSortedArray(int[] instructions) {
        int result = 0, MOD = 1_000_000_007, maxValue = 0;
        for (int x : instructions) maxValue = Math.max(maxValue, x);
        BIT bit = new BIT(maxValue);
        for (int i = 0, n = instructions.length; i < n; i++) 
        {
            int x = instructions[i], smaller = bit.query(x - 1), larger = i - bit.query(x);
            result = (result + Math.min(smaller, larger)) % MOD;
            bit.update(x);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def createSortedArray(self, instructions: List[int]) -> int:
        result, ordered, MOD = 0, [], 10 ** 9 + 7
        for x in instructions:
            result += min(smaller := bisect.bisect_left(ordered, x), len(ordered) - bisect.bisect_right(ordered, x))
            ordered[smaller:smaller] = [x]
        return result % MOD
```
