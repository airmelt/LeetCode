# 2279 Maximum Bags With Full Capacity of Rocks 装满石头的背包的最大数量

__Description:__

You have `n` bags numbered from `0` to `n - 1`. You are given two __0-indexed__ integer arrays `capacity` and `rocks`. The `i ^ th` bag can hold a maximum of `capacity[i]` rocks and currently contains `rocks[i]` rocks. You are also given an integer `additionalRocks`, the number of additional rocks you can place in __any__ of the bags.

Return the maximum number of bags that could have full capacity after placing the additional rocks in some bags.

__Example:__

Example 1:

```text
Input: capacity = [2,3,4,5], rocks = [1,2,4,4], additionalRocks = 2
Output: 3
Explanation:
Place 1 rock in bag 0 and 1 rock in bag 1.
The number of rocks in each bag are now [2,3,4,4].
Bags 0, 1, and 2 have full capacity.
There are 3 bags at full capacity, so we return 3.
It can be shown that it is not possible to have more than 3 bags at full capacity.
Note that there may be other ways of placing the rocks that result in an answer of 3.
```

Example 2:

```text
Input: capacity = [10,2,2], rocks = [2,2,0], additionalRocks = 100
Output: 3
Explanation:
Place 8 rocks in bag 0 and 2 rocks in bag 2.
The number of rocks in each bag are now [10,2,2].
Bags 0, 1, and 2 have full capacity.
There are 3 bags at full capacity, so we return 3.
It can be shown that it is not possible to have more than 3 bags at full capacity.
Note that we did not use all of the additional rocks.
```

__Constraints:__

- `n == capacity.length == rocks.length`
- `1 <= n <= 5 * 10 ^ 4`
- `1 <= capacity[i] <= 10 ^ 9`
- `0 <= rocks[i] <= capacity[i]`
- `1 <= additionalRocks <= 10 ^ 9`

__题目描述:__

现有编号从 `0` 到 `n - 1` 的 `n` 个背包。给你两个下标从 __0__ 开始的整数数组 `capacity` 和 `rocks` 。第 `i` 个背包最大可以装 `capacity[i]` 块石头，当前已经装了 `rocks[i]` 块石头。另给你一个整数 `additionalRocks` ，表示你可以放置的额外石头数量，石头可以往任意背包中放置。

请你将额外的石头放入一些背包中，并返回放置后装满石头的背包的 最大 数量。

__示例:__

示例 1：

```text
输入：capacity = [2,3,4,5], rocks = [1,2,4,4], additionalRocks = 2
输出：3
解释：
1 块石头放入背包 0 ，1 块石头放入背包 1 。
每个背包中的石头总数是 [2,3,4,4] 。
背包 0 、背包 1 和 背包 2 都装满石头。
总计 3 个背包装满石头，所以返回 3 。
可以证明不存在超过 3 个背包装满石头的情况。
注意，可能存在其他放置石头的方案同样能够得到 3 这个结果。
```

示例 2：

```text
输入：capacity = [10,2,2], rocks = [2,2,0], additionalRocks = 100
输出：3
解释：
8 块石头放入背包 0 ，2 块石头放入背包 2 。
每个背包中的石头总数是 [10,2,2] 。
背包 0 、背包 1 和背包 2 都装满石头。
总计 3 个背包装满石头，所以返回 3 。
可以证明不存在超过 3 个背包装满石头的情况。
注意，不必用完所有的额外石头。
```

__提示：__

- `n == capacity.length == rocks.length`
- `1 <= n <= 5 * 10 ^ 4`
- `1 <= capacity[i] <= 10 ^ 9`
- `0 <= rocks[i] <= capacity[i]`
- `1 <= additionalRocks <= 10 ^ 9`

__思路:__

```text
排序 ➕ 贪心
将 capacity[i] - rocks[i] 排序为 diff
然后从小到大遍历, 将 additionalRocks 减去 diff[i], 如果 additionalRocks 不小于 0, 则 result++, 否则结束
或者使用二分查找 accumulate(diff) 中第一个大于 additionalRocks 的位置
时间复杂度为 O(NlogN), 空间复杂度为 O(N), 主要是排序的时间复杂度
```

__代码:__

__C++__:

```C++
class Solution {
    public int maximumBags(int[] capacity, int[] rocks, int additionalRocks) {
        int n = rocks.length, diff[] = new int[n], result = 0;
        for (int i = 0; i < n; i++) diff[i] = capacity[i] - rocks[i];
        Arrays.sort(diff);
        for (int i = 0; i < n && additionalRocks > -1; i++) if ((additionalRocks = additionalRocks - diff[i]) > -1) ++result;
        return result;
    }
}
```

__Java__:

```Java
class Solution {
    public int maximumBags(int[] capacity, int[] rocks, int additionalRocks) {
        int n = rocks.length, diff[] = new int[n], result = 0;
        for (int i = 0; i < n; i++) diff[i] = capacity[i] - rocks[i];
        Arrays.sort(diff);
        for (int i = 0; i < n && additionalRocks > -1; i++) if ((additionalRocks = additionalRocks - diff[i]) > -1) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumBags(self, capacity: List[int], rocks: List[int], additionalRocks: int) -> int:
        return bisect_left(list(accumulate(sorted(c - r for c, r in zip(capacity, rocks)))), additionalRocks + 1)
```
