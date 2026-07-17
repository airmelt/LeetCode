# 3101 Count Alternating Subarrays 交替子数组计数

__Description:__

You are given a binary array `nums`.

We call a subarray alternating if no two adjacent elements in the subarray have the same value.

Return _the number of alternating subarrays in_ `nums`.

__Example:__

Example 1:

```text
Input: nums = [0,1,1,1]

Output: 5

Explanation:
```

The following subarrays are alternating: `[0]`, `[1]`, `[1]`, `[1]`, and `[0,1]`.

Example 2:

```text
Input: nums = [1,0,1,0]

Output: 10

Explanation:

Every subarray of the array is alternating. There are 10 possible subarrays that we can choose.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `nums[i]` is either `0` or `1`.

__题目描述:__

给你一个二进制数组 `nums` 。

如果一个子数组中 不存在 两个 相邻 元素的值 相同 的情况，我们称这样的子数组为 交替子数组 。

返回数组 `nums` 中交替子数组的数量。

__示例:__

示例 1：

```text
输入： nums = [0,1,1,1]

输出： 5

解释：
```

以下子数组是交替子数组: `[0]` 、 `[1]` 、 `[1]` 、 `[1]` 以及 `[0,1]` 。

示例 2：

```text
输入： nums = [1,0,1,0]

输出： 10

解释：

数组的每个子数组都是交替子数组。可以统计在内的子数组共有 10 个。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `nums[i]` 不是 `0` 就是 `1` 。

__思路:__

```text
贡献法
记录当前的交替子数组的长度 cur, 初始化为 1
设结果为 result, 初始化也为 1 表示最开始的元素单独可以构成一个交替子数组
从第 1 个元素开始遍历
如果和之前的元素相等
cur 重置为 1
否则累加 cur
每一次遍历将 cur 都累加到结果
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long countAlternatingSubarrays(vector<int>& nums) 
    {
        long long result = 1LL, cur = 1LL;
        for (int i = 1, n = nums.size(); i < n; i++) result += (cur = nums[i] == nums[i - 1] ? 1LL : cur + 1LL);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long countAlternatingSubarrays(int[] nums) {
        long result = 1L, cur = 1L;
        for (int i = 1, n = nums.length; i < n; i++) result += (cur = nums[i] == nums[i - 1] ? 1L : cur + 1L);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countAlternatingSubarrays(self, nums: List[int]) -> int:
        result = cur = 1
        for x, y in pairwise(nums):
            result += (cur := 1 if x == y else cur + 1)
        return result
```
