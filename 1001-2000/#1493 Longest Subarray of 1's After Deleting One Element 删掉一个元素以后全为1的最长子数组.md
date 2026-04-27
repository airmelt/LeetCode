# 1493 Longest Subarray of 1's After Deleting One Element 删掉一个元素以后全为1的最长子数组

__Description:__

Given a binary array `nums`, you should delete one element from it.

Return _the size of the longest non-empty subarray containing only_ `1`_'s in the resulting array_. Return `0` if there is no such subarray.

__Example:__

Example 1:

```text
Input: nums = [1,1,0,1]
Output: 3
Explanation: After deleting the number in position 2, [1,1,1] contains 3 numbers with value of 1's.
```

Example 2:

```text
Input: nums = [0,1,1,1,0,1,1,0,1]
Output: 5
Explanation: After deleting the number in position 4, [0,1,1,1,1,1,0,1] longest subarray with value of 1's is [1,1,1,1,1].
```

Example 3:

```text
Input: nums = [1,1,1]
Output: 2
Explanation: You must delete one element.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `nums[i]` is either `0` or `1`.

__题目描述:__

给你一个二进制数组 `nums` ，你需要从中删掉一个元素。

请你在删掉元素的结果数组中，返回最长的且只包含 1 的非空子数组的长度。

如果不存在这样的子数组，请返回 0 。

__提示 1：__

输入：nums = [1,1,0,1]
输出：3
解释：删掉位置 2 的数后，[1,1,1] 包含 3 个 1 。

__示例:__

示例 2：

```text
输入：nums = [0,1,1,1,0,1,1,0,1]
输出：5
解释：删掉位置 4 的数字后，[0,1,1,1,1,1,0,1] 的最长全 1 子数组为 [1,1,1,1,1] 。
```

示例 3：

```text
输入：nums = [1,1,1]
输出：2
解释：你必须要删除一个元素。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `nums[i]` 要么是 `0` 要么是 `1` 。

__思路:__

```text
双指针
用两个指针分别指向两段均为 1 的子数组
指针记录子数组的长度
如果最后所有数均为 1, 返回数组的长度减 1
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestSubarray(vector<int>& nums) 
    {
        int result = 0, pre = 0, cur = 0, n = nums.size();
        for (const auto& num: nums) 
        {
            if (num) ++cur;
            else
            {
                pre = cur;
                cur = 0;
            }
            result = max(result, pre + cur);
        }
        return min(result, n - 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int longestSubarray(int[] nums) {
        int result = 0, pre = 0, cur = 0;
        for (int num: nums) {
            if (num == 1) ++cur;
            else {
                pre = cur;
                cur = 0;
            }
            result = Math.max(result, pre + cur);
        }
        return Math.min(result, nums.length - 1);
    }
}
```

__Python__:

```Python
class Solution:
    def longestSubarray(self, nums: List[int]) -> int:
        return len(nums) - 1 if 0 not in nums else 0 if not (s := ''.join(list(map(str,nums))).split('0')) else max(len(s[i]) + len(s[i + 1]) for i in range(len(s) - 1))
```
