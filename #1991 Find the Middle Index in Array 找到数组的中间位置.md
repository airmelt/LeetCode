# 1991 Find the Middle Index in Array 找到数组的中间位置

__Description:__

Given a __0-indexed__ integer array `nums`, find the __leftmost__ `middleIndex` (i.e., the smallest amongst all the possible ones).

A `middleIndex` is an index where `nums[0] + nums[1] + ... + nums[middleIndex-1] == nums[middleIndex+1] + nums[middleIndex+2] + ... + nums[nums.length-1]`.

If `middleIndex == 0`, the left side sum is considered to be `0`. Similarly, if `middleIndex == nums.length - 1`, the right side sum is considered to be `0`.

Return _the __leftmost___ `middleIndex` _that satisfies the condition, or_ `-1` _if there is no such index_.

__Example:__

Example 1:

```text
Input: nums = [2,3,-1,8,4]
Output: 3
Explanation: The sum of the numbers before index 3 is: 2 + 3 + -1 = 4
The sum of the numbers after index 3 is: 4 = 4
```

Example 2:

```text
Input: nums = [1,-1,4]
Output: 2
Explanation: The sum of the numbers before index 2 is: 1 + -1 = 0
The sum of the numbers after index 2 is: 0
```

Example 3:

```text
Input: nums = [2,5]
Output: -1
Explanation: There is no valid middleIndex.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `-1000 <= nums[i] <= 1000`

Note: This question is the same as [724](https://leetcode.com/problems/find-pivot-index/)

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，请你找到 __最左边__ 的中间位置 `middleIndex` （也就是所有可能中间位置下标最小的一个）。

中间位置 `middleIndex` 是满足 `nums[0] + nums[1] + ... + nums[middleIndex-1] == nums[middleIndex+1] + nums[middleIndex+2] + ... + nums[nums.length-1]` 的数组下标。

如果 `middleIndex == 0` ，左边部分的和定义为 `0` 。类似的，如果 `middleIndex == nums.length - 1` ，右边部分的和定义为 `0` 。

请你返回满足上述条件 __最左边__ 的 `middleIndex` ，如果不存在这样的中间位置，请你返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums = [2,3,-1,8,4]
输出：3
解释：
下标 3 之前的数字和为：2 + 3 + -1 = 4
下标 3 之后的数字和为：4 = 4
```

示例 2：

```text
输入：nums = [1,-1,4]
输出：2
解释：
下标 2 之前的数字和为：1 + -1 = 0
下标 2 之后的数字和为：0
```

示例 3：

```text
输入：nums = [2,5]
输出：-1
解释：
不存在符合要求的 middleIndex 。
```

示例 4：

```text
输入：nums = [1]
输出：0
解释：
下标 0 之前的数字和为：0
下标 0 之后的数字和为：0
```

__提示：__

- `1 <= nums.length <= 100`
- `-1000 <= nums[i] <= 1000`

注意：本题与主站 [724](https://leetcode-cn.com/problems/find-pivot-index/) 题相同

__思路:__

```text
前缀和
计算数组的总和 total, 遍历数组, 计算当前位置左边的和 cur, 如果 (cur << 1) + nums[i] == total, 则返回 i
遍历结束后, 返回 -1
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findMiddleIndex(vector<int>& nums) 
    {
        int total = accumulate(nums.begin(), nums.end(), 0), cur = 0, n = nums.size();
        for (int i = 0; i < n; cur += nums[i++]) if ((cur << 1) + nums[i] == total) return i;
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int findMiddleIndex(int[] nums) {
        int total = Arrays.stream(nums).sum(), cur = 0, n = nums.length;
        for (int i = 0; i < n; cur += nums[i++]) if ((cur << 1) + nums[i] == total) return i;
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def findMiddleIndex(self, nums: List[int]) -> int:
        return ([i for i in range(len(nums)) if sum(nums[:i]) == sum(nums[i + 1:])] + [-1])[0]
```
