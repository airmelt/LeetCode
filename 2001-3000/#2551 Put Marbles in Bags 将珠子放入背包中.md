# 2551 Put Marbles in Bags 将珠子放入背包中

__Description:__

You have `k` bags. You are given a __0-indexed__ integer array `weights` where `weights[i]` is the weight of the `i ^ th` marble. You are also given the integer `k.`

Divide the marbles into the `k` bags according to the following rules:

- No bag is empty.
- If the `i ^ th` marble and `j ^ th` marble are in a bag, then all marbles with an index between the `i ^ th` and `j ^ th` indices should also be in that same bag.
- If a bag consists of all the marbles with an index from `i` to `j` inclusively, then the cost of the bag is `weights[i] + weights[j]`.

The __score__ after distributing the marbles is the sum of the costs of all the `k` bags.

Return the difference between the maximum and minimum scores among marble distributions.

__Example:__

Example 1:

```text
Input: weights = [1,3,5,1], k = 2
Output: 4
Explanation: 
The distribution [1],[3,5,1] results in the minimal score of (1+1) + (3+1) = 6. 
The distribution [1,3],[5,1], results in the maximal score of (1+3) + (5+1) = 10. 
Thus, we return their difference 10 - 6 = 4.
```

Example 2:

```text
Input: weights = [1, 3], k = 2
Output: 0
Explanation: The only distribution possible is [1],[3]. 
Since both the maximal and minimal score are the same, we return 0.
```

__Constraints:__

- `1 <= k <= weights.length <= 10 ^ 5`
- `1 <= weights[i] <= 10 ^ 9`

__题目描述:__

你有 `k` 个背包。给你一个下标从 __0__ 开始的整数数组 `weights` ，其中 `weights[i]` 是第 `i` 个珠子的重量。同时给你整数 `k` 。

请你按照如下规则将所有的珠子放进 `k` 个背包。

- 没有背包是空的。
- 如果第 `i` 个珠子和第 `j` 个珠子在同一个背包里，那么下标在 `i` 到 `j` 之间的所有珠子都必须在这同一个背包中。
- 如果一个背包有下标从 `i` 到 `j` 的所有珠子，那么这个背包的价格是 `weights[i] + weights[j]` 。

一个珠子分配方案的 __分数__ 是所有 `k` 个背包的价格之和。

请你返回所有分配方案中，最大分数 与 最小分数 的 差值 为多少。

__示例:__

示例 1：

```text
输入：weights = [1,3,5,1], k = 2
输出：4
解释：
分配方案 [1],[3,5,1] 得到最小得分 (1+1) + (3+1) = 6 。
分配方案 [1,3],[5,1] 得到最大得分 (1+3) + (5+1) = 10 。
所以差值为 10 - 6 = 4 。
```

示例 2：

```text
输入：weights = [1, 3], k = 2
输出：0
解释：唯一的分配方案为 [1],[3] 。
最大最小得分相等，所以返回 0 。
```

__提示：__

- `1 <= k <= weights.length <= 10 ^ 5`
- `1 <= weights[i] <= 10 ^ 9`

__思路:__

```text
贪心
weights[0] 和 weights[-1] 一定在最小值和最大值中
相当于把数组分成 k 个连续子数组, 分数即数组两端的和
把 weights[i] + weights[i + 1] 排序
求最大的 k - 1 个和最小的 k - 1 个即可
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long putMarbles(vector<int>& weights, int k) 
    {
        for (int i = 0, n = weights.size(); i < n - 1; i++) weights[i] += weights[i + 1];
        sort(weights.begin(), weights.end() - 1);
        long long result = 0LL;
        for (int i = 0; i < k - 1; i++) result += weights[weights.size() - 2 - i] - weights[i];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long putMarbles(int[] weights, int k) {
        for (int i = 0, n = weights.length; i < n - 1; i++) weights[i] += weights[i + 1];
        Arrays.sort(weights, 0, weights.length - 1);
        long result = 0L;
        for (int i = 0; i < k - 1; i++) result += weights[weights.length - 2 - i] - weights[i];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def putMarbles(self, weights: List[int], k: int) -> int:
        return sum(weights[len(weights) - k + 1:]) - sum(weights[:k - 1]) if k > 1 and (weights := sorted(x + y for x, y in pairwise(weights))) else 0
```
