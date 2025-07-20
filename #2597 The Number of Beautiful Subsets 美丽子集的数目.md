# 2597 The Number of Beautiful Subsets 美丽子集的数目

__Description:__

You are given an array `nums` of positive integers and a __positive__ integer `k`.

A subset of `nums` is __beautiful__ if it does not contain two integers with an absolute difference equal to `k`.

Return _the number of __non-empty beautiful__ subsets of the array_ `nums`.

A __subset__ of `nums` is an array that can be obtained by deleting some (possibly none) elements from `nums`. Two subsets are different if and only if the chosen indices to delete are different.

__Example:__

Example 1:

```text
Input: nums = [2,4,6], k = 2
Output: 4
Explanation: The beautiful subsets of the array nums are: [2], [4], [6], [2, 6].
It can be proved that there are only 4 beautiful subsets in the array [2,4,6].
```

Example 2:

```text
Input: nums = [1], k = 1
Output: 1
Explanation: The beautiful subset of the array nums is [1].
It can be proved that there is only 1 beautiful subset in the array [1].
```

__Constraints:__

- `1 <= nums.length <= 18`
- `1 <= nums[i], k <= 1000`

__题目描述:__

给你一个由正整数组成的数组 `nums` 和一个 __正__ 整数 `k` 。

如果 `nums` 的子集中，任意两个整数的绝对差均不等于 `k` ，则认为该子数组是一个 __美丽__ 子集。

返回数组 `nums` 中 __非空__ 且 __美丽__ 的子集数目。

`nums` 的子集定义为:可以经由 `nums` 删除某些元素（也可能不删除）得到的一个数组。只有在删除元素时选择的索引不同的情况下，两个子集才会被视作是不同的子集。

__示例:__

示例 1：

```text
输入：nums = [2,4,6], k = 2
输出：4
解释：数组 nums 中的美丽子集有：[2], [4], [6], [2, 6] 。
可以证明数组 [2,4,6] 中只存在 4 个美丽子集。
```

示例 2：

```text
输入：nums = [1], k = 1
输出：1
解释：数组 nums 中的美丽数组有：[1] 。
可以证明数组 [1] 中只存在 1 个美丽子集。
```

__提示：__

- `1 <= nums.length <= 18`
- `1 <= nums[i], k <= 1000`

__思路:__

```text
1. 枚举
枚举所有的子集
如果不选当前元素, 必然可以选下一个元素, 即 dfs(start + 1)
如果选当前元素, 则需要保证没选过相邻的两个元素, 即 !m[nums[start] - k] and !m[nums[start] + k], 可以用哈希表或者数组记录出现过的元素
时间复杂度为 O(2 ^ N), 空间复杂度为 O(N)
2. 动态规划
使用哈希表将 nums 中的元素分组, 将所有元素按模 k 分组
对于每个组, 设 dp[i][0] 表示不选第 i 个元素的美丽子集数目, dp[i][1] 表示选第 i 个元素的美丽子集数目
不选第 i 个元素时, dp[i][0] = dp[i - 1][0] + dp[i - 1][1]  
选第 i 个元素, 组内元素最多只能是相邻的元素相差 k, 所以 dp[i][1] = dp[i - 1][0] * (2 ^ count - 1) if cur - pre == k else (dp[i - 1][0] + dp[i - 1][1]) * (2 ^ count - 1)
初始值 dp[0][0] = 1, dp[0][1] = 2 ^ count - 1
最后每组的总和为 dp[m - 1][0] + dp[m - 1][1]
每一组根据乘法原理相乘, 最后减去 1 即为结果
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int beautifulSubsets(vector<int>& nums, int k) 
    {
        int result = -1, n = nums.size();
        unordered_map<int, int> m;
        auto dfs = [&](this auto&& dfs, int start) -> void
        {
            if (start == n) ++result;
            if (start < n) dfs(start + 1);
            if (start < n and !m[nums[start] - k] and !m[nums[start] + k])
            {
                ++m[nums[start]];
                dfs(start + 1);
                --m[nums[start]];
            }
        };
        dfs(0);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int beautifulSubsets(int[] nums, int k) {
        var groups = new HashMap<Integer, Map<Integer, Integer>>();
        for (int num : nums) groups.computeIfAbsent(num % k, key -> new TreeMap<>()).merge(num, 1, Integer::sum);
        int result = 1;
        for (var entry : groups.entrySet()) {
            var group = entry.getValue();
            int m = group.size(), dp[][] = new int[m][2], i = 1;
            dp[0][0] = 1;
            dp[0][1] = (1 << group.get(group.keySet().iterator().next())) - 1;
            var it = group.entrySet().iterator();
            var pre = it.next();
            while (it.hasNext()) {
                var cur = it.next();
                dp[i][0] = dp[i - 1][0] + dp[i - 1][1];
                dp[i][1] = cur.getKey() - pre.getKey() == k ? dp[i - 1][0] * ((1 << cur.getValue()) - 1) : (dp[i - 1][0] + dp[i - 1][1]) * ((1 << cur.getValue()) - 1);
                pre = cur;
                ++i;
            }
            result *= dp[m - 1][0] + dp[m - 1][1];
        }
        return result - 1;
    }
}
```

__Python__:

```Python
class Solution:
    def beautifulSubsets(self, nums: List[int], k: int) -> int:
        result, count, n = -1, [0] * (max(nums) + (k << 1)), len(nums)
        
        def dfs(start: int) -> NoReturn:
            if start == n:
                nonlocal result
                result += 1
                return
            dfs(start + 1)
            if not count[(x := nums[start]) - k] and not count[x + k]:
                count[x] += 1
                dfs(start + 1)
                count[x] -= 1
        dfs(0)
        return result
```
