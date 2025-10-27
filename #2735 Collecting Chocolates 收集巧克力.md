# 2735 Collecting Chocolates 收集巧克力

__Description:__

You are given a __0-indexed__ integer array `nums` of size `n` representing the cost of collecting different chocolates. The cost of collecting the chocolate at the index `i` is `nums[i]`. Each chocolate is of a different type, and initially, the chocolate at the index `i` is of `i ^ th` type.

In one operation, you can do the following with an incurred __cost__ of `x`:

- Simultaneously change the chocolate of `i ^ th` type to `((i + 1) mod n) ^ th` type for all chocolates.

Return the minimum cost to collect chocolates of all types, given that you can perform as many operations as you would like.

__Example:__

Example 1:

```text
Input: nums = [20,1,15], x = 5
Output: 13
Explanation: Initially, the chocolate types are [0,1,2]. We will buy the 1st type of chocolate at a cost of 1.
Now, we will perform the operation at a cost of 5, and the types of chocolates will become [1,2,0]. We will buy the 2nd type of chocolate at a cost of 1.
Now, we will again perform the operation at a cost of 5, and the chocolate types will become [2,0,1]. We will buy the 0th type of chocolate at a cost of 1. 
Thus, the total cost will become (1 + 5 + 1 + 5 + 1) = 13. We can prove that this is optimal.
```

Example 2:

```text
Input: nums = [1,2,3], x = 4
Output: 6
Explanation: We will collect all three types of chocolates at their own price without performing any operations. Therefore, the total cost is 1 + 2 + 3 = 6.
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= x <= 10 ^ 9`

__题目描述:__

给你一个长度为 `n`、下标从 __0__ 开始的整数数组 `nums`， `nums[i]` 表示收集位于下标 `i` 处的巧克力成本。每个巧克力都对应一个不同的类型，最初，位于下标 `i` 的巧克力就对应第 `i` 个类型。

在一步操作中，你可以用成本 `x` 执行下述行为:

- 同时修改所有巧克力的类型，将巧克力的类型 `i ^ th` 修改为类型 `((i + 1) mod n) ^ th`。

假设你可以执行任意次操作，请返回收集所有类型巧克力所需的最小成本。

__示例:__

示例 1：

```text
输入：nums = [20,1,15], x = 5
输出：13
解释：最开始，巧克力的类型分别是 [0,1,2] 。我们可以用成本 1 购买第 1 个类型的巧克力。
接着，我们用成本 5 执行一次操作，巧克力的类型变更为 [1,2,0] 。我们可以用成本 1 购买第 2 个类型的巧克力。
然后，我们用成本 5 执行一次操作，巧克力的类型变更为 [2,0,1] 。我们可以用成本 1 购买第 0 个类型的巧克力。
因此，收集所有类型的巧克力需要的总成本是 (1 + 5 + 1 + 5 + 1) = 13 。可以证明这是一种最优方案。
```

示例 2：

```text
输入：nums = [1,2,3], x = 4
输出：6
解释：我们将会按最初的成本收集全部三个类型的巧克力，而不需执行任何操作。因此，收集所有类型的巧克力需要的总成本是 1 + 2 + 3 = 6 。
```

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= x <= 10 ^ 9`

__思路:__

```text
枚举
枚举修改次数
如例 1 最多也就修改 x 次
初始化一个从 0 到 (n - 1)x 的数组表示修改 x 次的代价 cost
对于 i, 要么直接取, 要么等取完模再取
nums[i] = min(nums[i], nums[(i + 1) % n])
如果操作 k 次, 那就是取 min(nums[i:i + k]), 这里可以将 nums 视为环形数组
每次取区间 [i, n + i]
将这段区间上的最小值加到 cost[j - i] 上
最后取 cost 的最小值
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minCost(vector<int>& nums, int x) 
    {
        vector<long long> result(nums.size());
        for (int i = 0, n = nums.size(); i < n; i++) for (int j = i, cur = nums[j]; j < n + i; j++) result[j - i] += (cur = min(cur, nums[j % n]));
        for (int i = 0, n = nums.size(); i < n; i++) result[i] += (long long)i * x;
        return *min_element(result.begin(), result.end());
    }
};
```

__Java__:

```Java
class Solution {
    public long minCost(int[] nums, int x) {
        long result[] = new long[nums.length];
        for (int i = 0, n = nums.length; i < n; i++) for (int j = i, cur = nums[j]; j < n + i; j++) result[j - i] += (cur = Math.min(cur, nums[j % n]));
        for (int i = 0, n = nums.length; i < n; i++) result[i] += (long)i * x;
        return Arrays.stream(result).min().getAsLong();
    }
}
```

__Python__:

```Python
class Solution:
    def minCost(self, nums: List[int], x: int) -> int:
        result = list(range(0, (n := len(nums)) * x, x))
        for i, v in enumerate(nums):
            for j in range(i, n + i):
                result[j - i] += (v := min(v, nums[j % n]))
        return min(result)
```
