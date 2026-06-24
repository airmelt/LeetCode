# 3075 Maximize Happiness of Selected Children 幸福值最大化的选择方案

__Description:__

You are given an array `happiness` of length `n`, and a __positive__ integer `k`.

There are `n` children standing in a queue, where the `i ^ th` child has __happiness value__ `happiness[i]`. You want to select `k` children from these `n` children in `k` turns.

In each turn, when you select a child, the __happiness value__ of all the children that have __not__ been selected till now decreases by `1`. Note that the happiness value __cannot__ become negative and gets decremented __only__ if it is positive.

Return _the __maximum__ sum of the happiness values of the selected children you can achieve by selecting_ `k` _children_.

__Example:__

Example 1:

```text
Input: happiness = [1,2,3], k = 2
Output: 4
Explanation: We can pick 2 children in the following way:
```

- Pick the child with the happiness value == 3. The happiness value of the remaining children becomes [0,1].
- Pick the child with the happiness value == 1. The happiness value of the remaining child becomes [0]. Note that the happiness value cannot become less than 0.

The sum of the happiness values of the selected children is 3 + 1 = 4.

Example 2:

```text
Input: happiness = [1,1,1,1], k = 2
Output: 1
Explanation: We can pick 2 children in the following way:
```

- Pick any child with the happiness value == 1. The happiness value of the remaining children becomes [0,0,0].
- Pick the child with the happiness value == 0. The happiness value of the remaining child becomes [0,0].

The sum of the happiness values of the selected children is 1 + 0 = 1.

Example 3:

```text
Input: happiness = [2,3,4,5], k = 1
Output: 5
Explanation: We can pick 1 child in the following way:
```

- Pick the child with the happiness value == 5. The happiness value of the remaining children becomes [1,2,3].

The sum of the happiness values of the selected children is 5.

__Constraints:__

- `1 <= n == happiness.length <= 2 * 10 ^ 5`
- `1 <= happiness[i] <= 10 ^ 8`
- `1 <= k <= n`

__题目描述:__

给你一个长度为 `n` 的数组 `happiness` ，以及一个 __正整数__ `k` 。

`n` 个孩子站成一队，其中第 `i` 个孩子的 __幸福值__ 是 `happiness[i]` 。你计划组织 `k` 轮筛选从这 `n` 个孩子中选出 `k` 个孩子。

在每一轮选择一个孩子时，所有 __尚未__ 被选中的孩子的 __幸福值__ 将减少 `1` 。注意，幸福值 __不能__ 变成负数，且只有在它是正数的情况下才会减少。

选择 `k` 个孩子，并使你选中的孩子幸福值之和最大，返回你能够得到的 __最大值__ 。

__示例:__

示例 1：

```text
输入：happiness = [1,2,3], k = 2
输出：4
解释：按以下方式选择 2 个孩子：
```

- 选择幸福值为 3 的孩子。剩余孩子的幸福值变为 [0,1] 。
- 选择幸福值为 1 的孩子。剩余孩子的幸福值变为 [0] 。注意幸福值不能小于 0 。

所选孩子的幸福值之和为 3 + 1 = 4 。

示例 2：

```text
输入：happiness = [1,1,1,1], k = 2
输出：1
解释：按以下方式选择 2 个孩子：
```

- 选择幸福值为 1 的任意一个孩子。剩余孩子的幸福值变为 [0,0,0] 。
- 选择幸福值为 0 的孩子。剩余孩子的幸福值变为 [0,0] 。

所选孩子的幸福值之和为 1 + 0 = 1 。

示例 3：

```text
输入：happiness = [2,3,4,5], k = 1
输出：5
解释：按以下方式选择 1 个孩子：
```

- 选择幸福值为 5 的孩子。剩余孩子的幸福值变为 [1,2,3] 。

所选孩子的幸福值之和为 5 。

__提示：__

- `1 <= n == happiness.length <= 2 * 10 ^ 5`
- `1 <= happiness[i] <= 10 ^ 8`
- `1 <= k <= n`

__思路:__

```text
贪心
从大到小选择前 k 个孩子
如果幸福值不够减
可以提前结束
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maximumHappinessSum(vector<int>& happiness, int k) 
    {
        sort(happiness.begin(), happiness.end());
        long long result = 0LL;
        for (int n = happiness.size(), i = n - 1; i >= n - k and happiness[i] > n - 1 - i; i--) result += happiness[i] - (n - 1 - i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long maximumHappinessSum(int[] happiness, int k) {
        Arrays.sort(happiness);
        long result = 0L;
        for (int n = happiness.length, i = n - 1; i >= n - k && happiness[i] > n - 1 - i; i--) result += happiness[i] - (n - 1 - i);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumHappinessSum(self, happiness: List[int], k: int) -> int:
        return sum(max(x - i, 0) for i, x in enumerate(sorted(happiness, reverse=True)[:k]))
```
