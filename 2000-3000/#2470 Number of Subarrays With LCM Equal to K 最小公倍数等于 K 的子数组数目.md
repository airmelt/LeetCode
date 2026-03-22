# 2470 Number of Subarrays With LCM Equal to K 最小公倍数等于 K 的子数组数目

__Description:__

Given an integer array `nums` and an integer `k`, return _the number of __subarrays__ of_ `nums` _where the least common multiple of the subarray's elements is_ `k`.

A subarray is a contiguous non-empty sequence of elements within an array.

The least common multiple of an array is the smallest positive integer that is divisible by all the array elements.

__Example:__

Example 1:

```text
Input: nums = [3,6,2,7,1], k = 6
Output: 4
Explanation: The subarrays of nums where 6 is the least common multiple of all the subarray's elements are:
```

- [3,__6__,2,7,1]
- [__3,6__,2,7,1]
- [__3,6,2__,7,1]
- [3,__6,2__,7,1]

Example 2:

```text
Input: nums = [3], k = 2
Output: 0
Explanation: There are no subarrays of nums where 2 is the least common multiple of all the subarray's elements.
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i], k <= 1000`

__题目描述:__

给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 `nums` 的 __子数组__ 中满足 _元素最小公倍数为 `k`_ 的子数组数目。

子数组 是数组中一个连续非空的元素序列。

数组的最小公倍数 是可被所有数组元素整除的最小正整数。

__示例:__

示例 1 ：

```text
输入：nums = [3,6,2,7,1], k = 6
输出：4
解释：以 6 为最小公倍数的子数组是：
```

- [3,__6__,2,7,1]
- [__3,6__,2,7,1]
- [__3,6,2__,7,1]
- [3,__6,2__,7,1]

示例 2 ：

```text
输入：nums = [3], k = 2
输出：0
解释：不存在以 2 为最小公倍数的子数组。
```

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i], k <= 1000`

__思路:__

```text
1. 暴力法
枚举所有子数组, 计算最小公倍数
如果最小公倍数等于 k, 计数器加一
超过 k 的子数组直接跳过
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. LCM 优化
使用 LCM 的性质
如果 k 不能被 nums[i] 整除, 那么 nums[i] 一定不在子数组中
记录下 nums[i] 的下标和当前 LCM
记录上次的下标 pre
如果当前 LCM == k, 结果加上当前下标 - pre
时间复杂度为 O(NlogK), 空间复杂度为 O(logK)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int subarrayLCM(vector<int>& nums, int k) 
    {
        int result = 0, pre = -1, n = nums.size();
        vector<pair<int, int>> a;
        for (int i = 0, j = 0; i < n; i++, j = 0)
        {
            if (k % nums[i])
            {
                a.clear();
                pre = i;
                continue;
            }
            a.emplace_back(make_pair(nums[i], i));
            for (auto& p : a)
            {
                p.first = lcm(p.first, nums[i]);
                if (a[j].first != p.first) a[++j] = p;
                else a[j].second = p.second;
            }
            a.resize(a.size() - j);
            if (a.front().first == k) result += a.front().second - pre;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int subarrayLCM(int[] nums, int k) {
        int result = 0, n = nums.length;
        for (int i = 0; i < n; i++) {
            int cur = nums[i];
            for (int j = i; j < n; j++) {
                cur = lcm(cur, nums[j]);
                if (k % cur != 0) break;
                if (cur == k) ++result;
            }
        }
        return result;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }

    private int lcm(int a, int b) {
        return a * b / gcd(a, b);
    }
}
```

__Python__:

```Python
class Solution:
    def subarrayLCM(self, nums: List[int], k: int) -> int:
        result, n = 0, len(nums)
        for i, cur in enumerate(nums):
            cur = 1
            for j in range(i, n):
                cur = lcm(cur, nums[j])
                if k % cur:
                    break
                result += cur == k
        return result
```
