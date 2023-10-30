# 1862 Sum of Floored Pairs 向下取整数对和

__Description:__

Given an integer array `nums`, return the sum of `floor(nums[i] / nums[j])` for all pairs of indices `0 <= i, j < nums.length` in the array. Since the answer may be too large, return it __modulo__ `10 ^ 9 + 7`.

The `floor()` function returns the integer part of the division.

__Example:__

Example 1:

```text
Input: nums = [2,5,9]
Output: 10
Explanation:
floor(2 / 5) = floor(2 / 9) = floor(5 / 9) = 0
floor(2 / 2) = floor(5 / 5) = floor(9 / 9) = 1
floor(5 / 2) = 2
floor(9 / 2) = 4
floor(9 / 5) = 1
We calculate the floor of the division for every pair of indices in the array then sum them up.
```

Example 2:

```text
Input: nums = [7,7,7,7,7,7,7]
Output: 49
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个整数数组 `nums` ，请你返回所有下标对 `0 <= i, j < nums.length` 的 `floor(nums[i] / nums[j])` 结果之和。由于答案可能会很大，请你返回答案对 `10 ^ 9 + 7` __取余__ 的结果。

函数 `floor()` 返回输入数字的整数部分。

__示例:__

示例 1：

```text
输入：nums = [2,5,9]
输出：10
解释：
floor(2 / 5) = floor(2 / 9) = floor(5 / 9) = 0
floor(2 / 2) = floor(5 / 5) = floor(9 / 9) = 1
floor(5 / 2) = 2
floor(9 / 2) = 4
floor(9 / 5) = 1
我们计算每一个数对商向下取整的结果并求和得到 10 。
```

示例 2：

```text
输入：nums = [7,7,7,7,7,7,7]
输出：49
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__思路:__

```text
前缀和
暴力解法是遍历数组的元素对 (x, y) 并求出向下取整的和, 时间复杂度为 O(N ^ 2), 肯定会超时
不妨遍历 y 和 d = floor(x / y)
则 d * y <= x < (d + 1) * y
设数组的最大值为 m, MOD = 1e9 + 7
先统计 nums 数组中每个元素的出现次数
并求出出现次数的前缀和 pre
对于 nums 数组中每一个独特的元素 y
分别计算 d = 1, 2, ..., m / y + 1 可以对应多少个 x
上限不能超过数组的最大值, 所以为 min(m, (d + 1) * y - 1)
下限为 d * y
在前缀和中应该为 pre[min(m, (d + 1) * y - 1)] - pre[d * y]
这样就计算出了 d * y <= x < (d + 1) * y 中有多少个 x 满足 x / y = d
y 出现次数为 pre[y] - pre[y - 1]
再乘上 d
即 (d, y) 对于结果的贡献为 (pre[min(m, (d + 1) * y - 1)] - pre[d * y]) * d * (pre[y] - pre[y - 1])
将所有的 (d, y) 累计到结果中, 并对 MOD 取模即可
时间复杂度为 O(N + MlogM), 空间复杂度为 O(M), 其中 M 为 nums 数组中的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int sumOfFlooredPairs(vector<int>& nums) 
    {
        int m = *max_element(nums.begin(), nums.end()), result = 0, MOD = 1e9 + 7;
        vector<int> pre(m + 1);
        unordered_set<int> s(nums.begin(), nums.end());
        for (const auto& num : nums) ++pre[num];
        for (int i = 0; i < m; i++) pre[i + 1] += pre[i];
        for (const auto& num : s) for (int d = 1; d <= m / num; d++) result = (result + (long)(pre[min((d + 1) * num - 1, m)] - pre[d * num - 1]) * d * (pre[num] - pre[num - 1])) % MOD;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int sumOfFlooredPairs(int[] nums) {
        int m = 0;
        for (int num : nums) m = Math.max(m, num);
        int result = 0, MOD = 1_000_000_007, pre[] = new int[m + 1];
        for (int num : nums) ++pre[num];
        for (int i = 0; i < m; i++) pre[i + 1] += pre[i];
        for (int num : Arrays.stream(nums).boxed().collect(Collectors.toSet())) {
            for (int d = 1; d <= m / num; d++) {
                long cur = (long)(pre[Math.min((d + 1) * num - 1, m)] - pre[d * num - 1]) * d * (pre[num] - pre[num - 1]);
                result = (result + (int)(cur % MOD)) % MOD;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def sumOfFlooredPairs(self, nums: List[int]) -> int:
        pre, result, MOD = [0] * ((m := max(nums)) + 1), 0, 10 ** 9 + 7
        for num in nums:
            pre[num] += 1
        for i in range(m):
            pre[i + 1] += pre[i]
        for num in set(nums):
            for d in range(1, m // num + 1):
                result += (pre[min((d + 1) * num - 1, m)] - pre[d * num - 1]) * d * (pre[num] - pre[num - 1])
        return result % MOD
```
