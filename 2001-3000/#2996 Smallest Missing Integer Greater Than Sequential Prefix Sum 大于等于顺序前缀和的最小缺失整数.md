# 2996 Smallest Missing Integer Greater Than Sequential Prefix Sum 大于等于顺序前缀和的最小缺失整数

__Description:__

You are given a __0-indexed__ array of integers `nums`.

A prefix `nums[0..i]` is __sequential__ if, for all `1 <= j <= i`, `nums[j] = nums[j - 1] + 1`. In particular, the prefix consisting only of `nums[0]` is __sequential__.

Return _the __smallest__ integer_ `x` _missing from_ `nums` _such that_ `x` _is greater than or equal to the sum of the __longest__ sequential prefix._

__Example:__

Example 1:

```text
Input: nums = [1,2,3,2,5]
Output: 6
Explanation: The longest sequential prefix of nums is [1,2,3] with a sum of 6. 6 is not in the array, therefore 6 is the smallest missing integer greater than or equal to the sum of the longest sequential prefix.
```

Example 2:

```text
Input: nums = [3,4,5,1,12,14,13]
Output: 15
Explanation: The longest sequential prefix of nums is [3,4,5] with a sum of 12. 12, 13, and 14 belong to the array while 15 does not. Therefore 15 is the smallest missing integer greater than or equal to the sum of the longest sequential prefix.
```

__Constraints:__

- `1 <= nums.length <= 50`
- `1 <= nums[i] <= 50`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。

如果一个前缀 `nums[0..i]` 满足对于 `1 <= j <= i` 的所有元素都有 `nums[j] = nums[j - 1] + 1` ，那么我们称这个前缀是一个 __顺序前缀__ 。特殊情况是，只包含 `nums[0]` 的前缀也是一个 __顺序前缀__ 。

请你返回 `nums` 中没有出现过的 __最小__ 整数 `x` ，满足 `x` 大于等于 __最长__ 顺序前缀的和。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,2,5]
输出：6
解释：nums 的最长顺序前缀是 [1,2,3] ，和为 6 ，6 不在数组中，所以 6 是大于等于最长顺序前缀和的最小整数。
```

示例 2：

```text
输入：nums = [3,4,5,1,12,14,13]
输出：15
解释：nums 的最长顺序前缀是 [3,4,5] ，和为 12 ，12、13 和 14 都在数组中，但 15 不在，所以 15 是大于等于最长顺序前缀和的最小整数。
```

__提示：__

- `1 <= nums.length <= 50`
- `1 <= nums[i] <= 50`

__思路:__

```text
模拟
先计算最长顺序前缀和
遍历元素直到后一个元素不是前一个元素的下一个元素
一边遍历一边求出前缀和
将数组转化为哈希表
如果前缀和在哈希表中就自增
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int missingInteger(vector<int>& nums) 
    {
        int result = nums.front(), n = nums.size();
        for (int i = 1; i < n and nums[i] == nums[i - 1] + 1; i++) result += nums[i];
        unordered_set<int> s(nums.begin(), nums.end());
        while (s.count(result)) ++result;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int missingInteger(int[] nums) {
        int result = nums[0], n = nums.length;
        for (int i = 1; i < n && nums[i] == nums[i - 1] + 1; i++) result += nums[i];
        Set<Integer> set = Arrays.stream(nums).boxed().collect(Collectors.toSet());
        while (set.contains(result)) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def missingInteger(self, nums: List[int]) -> int:
        result, s = nums[0], set(nums)
        for x, y in pairwise(nums):
            if x + 1 != y:
                break
            result += y
        while result in s:
            result += 1
        return result
```
