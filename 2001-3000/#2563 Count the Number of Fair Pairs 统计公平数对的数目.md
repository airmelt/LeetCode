# 2563 Count the Number of Fair Pairs 统计公平数对的数目

__Description:__

Given a __0-indexed__ integer array `nums` of size `n` and two integers `lower` and `upper`, return _the number of fair pairs_.

A pair `(i, j)` is _fair_ if:

- `0 <= i < j < n`, and
- `lower <= nums[i] + nums[j] <= upper`

__Example:__

Example 1:

```text
Input: nums = [0,1,7,4,4,5], lower = 3, upper = 6
Output: 6
Explanation: There are 6 fair pairs: (0,3), (0,4), (0,5), (1,3), (1,4), and (1,5).
```

Example 2:

```text
Input: nums = [1,7,9,2,5], lower = 11, upper = 11
Output: 1
Explanation: There is a single fair pair: (2,3).
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `nums.length == n`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`
- `-10 ^ 9 <= lower <= upper <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始、长度为 `n` 的整数数组 `nums` ，和两个整数 `lower` 和 `upper` ，返回 __公平数对的数目__ 。

如果 `(i, j)` 数对满足以下情况，则认为它是一个 __公平数对__ :

- `0 <= i < j < n`，且
- `lower <= nums[i] + nums[j] <= upper`

__示例:__

示例 1：

```text
输入：nums = [0,1,7,4,4,5], lower = 3, upper = 6
输出：6
解释：共计 6 个公平数对：(0,3)、(0,4)、(0,5)、(1,3)、(1,4) 和 (1,5) 。
```

示例 2：

```text
输入：nums = [1,7,9,2,5], lower = 11, upper = 11
输出：1
解释：只有单个公平数对：(2,3) 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `nums.length == n`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`
- `-10 ^ 9 <= lower <= upper <= 10 ^ 9`

__思路:__

```text
排序
数对和位置无关
先对数组进行排序
可以使用二分查找找到第一个大于 upper - nums[i](即大于等于 upper - nums[i] + 1) 的元素和第一个大于等于 lower - nums[i] 的元素, 夹在中间的即为 nums[i] 对应的配对数
也可以使用双指针
随着 nums[i] 增加 upper - nums[i] 减小, lower - nums[i] 也减小
因此可以遍历, 每次更新 right 和 left 的位置
所有满足条件的 nums[j] 都在 left 和 right 之间
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long countFairPairs(vector<int>& nums, int lower, int upper) 
    {
        sort(nums.begin(), nums.end());
        long long result = 0LL;
        for (int i = 0, n = nums.size(), left = n, right = n; i < n; i++) 
        {
            while (right > 0 and nums[right - 1] > upper - nums[i]) --right;
            while (left > 0 and nums[left - 1] >= lower - nums[i]) --left;
            result += min(right, i) - min(left, i);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long countFairPairs(int[] nums, int lower, int upper) {
        Arrays.sort(nums);
        long result = 0L;
        for (int i = 0, n = nums.length, left = n, right = n; i < n; i++) {
            while (right > 0 && nums[right - 1] > upper - nums[i]) --right;
            while (left > 0 && nums[left - 1] >= lower - nums[i]) --left;
            result += Math.min(right, i) - Math.min(left, i);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countFairPairs(self, nums: List[int], lower: int, upper: int) -> int:
        return 0 if not (nums := sorted(nums)) else sum(bisect_right(nums, upper - x, 0, i) - bisect_left(nums, lower - x, 0, i) for i, x in enumerate(nums))
```
