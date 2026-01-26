# 2862 Maximum Element-Sum of a Complete Subset of Indices 完全子集的最大元素和

__Description:__

You are given a __1-indexed__ array `nums`. Your task is to select a __complete subset__ from `nums` where every pair of selected indices multiplied is a `perfect square`. i. e. if you select `ai` and `aj`, `i * j` must be a perfect square.

Return the sum of the complete subset with the maximum sum.

__Example:__

Example 1:

```text
Input: nums = [8,7,3,5,7,2,4,9]

Output: 16

Explanation:

We select elements at indices 2 and 8 and 2 * 8 is a perfect square.
```

Example 2:

```text
Input: nums = [8,10,3,8,1,13,7,9,4]

Output: 20

Explanation:

We select elements at indices 1, 4, and 9. 1 * 4, 1 * 9, 4 * 9 are perfect squares.
```

__Constraints:__

- `1 <= n == nums.length <= 10 ^ 4`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __1__ 开始、由 `n` 个整数组成的数组。你需要从 `nums` 选择一个 __完全集__，其中每对元素下标的乘积都是一个 `完全平方数`，例如选择 `ai` 和 `aj` ， `i * j` 一定是完全平方数。

返回 完全子集 所能取到的 最大元素和 。

__示例:__

示例 1：

```text
输入：nums = [8,7,3,5,7,2,4,9]

输出：16

解释：

我们选择下标为 2 和 8 的元素，并且 2 * 8 是一个完全平方数。
```

示例 2：

```text
输入：nums = [8,10,3,8,1,13,7,9,4]

输出：20

解释：

我们选择下标为 1, 4, 9 的元素。 1 * 4, 1 * 9, 4 * 9 是完全平方数。
```

__提示：__

- `1 <= n == nums.length <= 10 ^ 4`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
模拟
注意到数组元素的范围为正数
为了获取子集和的最大值
需要把能构成完全子集的所有元素全部求和
对于 i 范围为 [1, n] 两边闭区间
为了构成完全子集
要找到每一个 j, j 的范围为 [1, sqrt(n / i)] 闭区间
那么每一个 i * j * j 都可以构成完全子集
求出最大和即可
时间复杂度为 O(N), 空间复杂度为 O(1), 对于求 N 以内的完全平方数需要 N ^ 1 / 2, 每一次循环为 1 / X ^ 1 / 2, X 的范围即 [1, n], 求和即 N ^ 1 / 2, 嵌套循环总时间复杂度为 N
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maximumSum(vector<int>& nums) 
    {
        long long result = 0LL;
        for (int i = 1, n = nums.size(); i <= n; i++) 
        {
            long long cur = 0LL;
            for (int j = 1, up = n / i; j * j <= up; j++) cur += nums[i * j * j - 1];
            result = max(result, cur);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long maximumSum(List<Integer> nums) {
        long result = 0L;
        for (int i = 1, n = nums.size(); i <= n; i++) {
            long cur = 0L;
            for (int j = 1, up = n / i; j * j <= up; j++) cur += nums.get(i * j * j - 1);
            result = Math.max(result, cur);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumSum(self, nums: List[int]) -> int:
        return max((sum(nums[i * j * j - 1] for j in range(1, isqrt(len(nums) // i) + 1)) for i in range(1, len(nums) + 1)))
```
