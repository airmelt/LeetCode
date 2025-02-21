# 2447 Number of Subarrays With GCD Equal to K 最大公因数等于 K 的子数组数目

__Description:__

Given an integer array `nums` and an integer `k`, return _the number of __subarrays__ of_ `nums` _where the greatest common divisor of the subarray's elements is_ `k`.

A subarray is a contiguous non-empty sequence of elements within an array.

The greatest common divisor of an array is the largest integer that evenly divides all the array elements.

__Example:__

Example 1:

```text
Input: nums = [9,3,1,2,6,3], k = 3
Output: 4
Explanation: The subarrays of nums where 3 is the greatest common divisor of all the subarray's elements are:
```

- [9,3,1,2,6,3]
- [9,3,1,2,6,3]
- [9,3,1,2,6,3]
- [9,3,1,2,6,3]

Example 2:

```text
Input: nums = [4], k = 7
Output: 0
Explanation: There are no subarrays of nums where 7 is the greatest common divisor of all the subarray's elements.
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i], k <= 10 ^ 9`

__题目描述:__

给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 `nums` 的子数组中元素的最大公因数等于 `k` 的子数组数目。

子数组 是数组中一个连续的非空序列。

数组的最大公因数 是能整除数组中所有元素的最大整数。

__示例:__

示例 1：

```text
输入：nums = [9,3,1,2,6,3], k = 3
输出：4
解释：nums 的子数组中，以 3 作为最大公因数的子数组如下：
```

- [9,3,1,2,6,3]
- [9,3,1,2,6,3]
- [9,3,1,2,6,3]
- [9,3,1,2,6,3]

示例 2：

```text
输入：nums = [4], k = 7
输出：0
解释：不存在以 7 作为最大公因数的子数组。
```

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i], k <= 10 ^ 9`

__思路:__

```text
1. 暴力法
遍历所有子数组
计算到目前为止的最大公因数
如果等于 k, 结果加一
如果小于 k, 直接跳出
时间复杂度为 O(N ^ 2 + NlogM), 空间复杂度为 O(1), 其中 M 为 nums 中的最大值
2. 质因子计数
以 nums[i] 为右端点记录 i 的下标
计算 nums[i] 的因子
如果出现相同的因子, 将它们的计数合并
找到第一个等于 k 的因子的下标减去前一个下标就是刚好等于 k 的子数组的长度
时间复杂度为 O(NlogM), 空间复杂度为 O(logM), 其中 M 为 nums 中的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int subarrayGCD(vector<int>& nums, int k) 
    {
        int result = 0, n = nums.size();
        vector<pair<int, int>> v({{-1, 0}});
        for (int i = 0; i < n; i++) 
        {
            for (int m = v.size(), j = 1; j < v.size(); j++) v[j].second = gcd(v[j].second, nums[i]);
            v.push_back({i, nums[i]});
            for (int m = v.size(), j = m - 2; j > 0; j--) if (v[j].second == v[j + 1].second) v[j].first = v[j + 1].first;
            v.erase(unique(v.begin(), v.end()), v.end());
            for (int m = v.size(), j = 1; j < m; j++) if (v[j].second == k) result += v[j].first - v[j - 1].first;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int subarrayGCD(int[] nums, int k) {
        int result = (int)Arrays.stream(nums).filter(x -> x == k).count(), n = nums.length, x = nums[0];
        for (int i = 0; i < n; x = i < n - 1 ? nums[++i] : i++) for (int j = i + 1; j < n && x >= k; j++) result += (x = gcd(x, nums[j])) == k ? 1 : 0;
        return result;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
class Solution:
    def subarrayGCD(self, nums: List[int], k: int) -> int:
        return sum(Counter(accumulate(nums[i:], gcd))[k] for i in range(len(nums)))
```
