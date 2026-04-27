# 2815 Max Pair Sum in an Array 数组中的最大数对和

__Description:__

You are given an integer array `nums`. You have to find the __maximum__ sum of a pair of numbers from `nums` such that the __largest digit__ in both numbers is equal.

For example, 2373 is made up of three distinct digits: 2, 3, and 7, where 7 is the largest among them.

Return the maximum sum or -1 if no such pair exists.

__Example:__

Example 1:

```text
Input: nums = [112,131,411]

Output: -1

Explanation:

Each numbers largest digit in order is [2,3,4].
```

Example 2:

```text
Input: nums = [2536,1613,3366,162]

Output: 5902

Explanation:

All the numbers have 6 as their largest digit, so the answer is 2536 + 3366 = 5902.
```

Example 3:

```text
Input: nums = [51,71,17,24,42]

Output: 88

Explanation:

Each number's largest digit in order is [5,7,7,4,4].

So we have only two possible pairs, 71 + 17 = 88 and 24 + 42 = 66.
```

__Constraints:__

- `2 <= nums.length <= 100`
- `1 <= nums[i] <= 10 ^ 4`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。请你从 `nums` 中找出和 __最大__ 的一对数，且这两个数数位上最大的数字相等。

返回最大和，如果不存在满足题意的数字对，返回 `-1` _。_

__示例:__

示例 1：

```text
输入：nums = [51,71,17,24,42]
输出：88
解释：
i = 1 和 j = 2 ，nums[i] 和 nums[j] 数位上最大的数字相等，且这一对的总和 71 + 17 = 88 。 
i = 3 和 j = 4 ，nums[i] 和 nums[j] 数位上最大的数字相等，且这一对的总和 24 + 42 = 66 。
可以证明不存在其他数对满足数位上最大的数字相等，所以答案是 88 。
```

示例 2：

```text
输入：nums = [1,2,3,4]
输出：-1
解释：不存在数对满足数位上最大的数字相等。
```

__提示：__

- `2 <= nums.length <= 100`
- `1 <= nums[i] <= 10 ^ 4`

__思路:__

```text
1. 暴力法(Python)
逐个检查每一个数对是否满足题意
记录下最大的数对和
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 缓存
由于最大数字最多为 9
用一个长度为 10 的数组存储当前数位最大的元素
当数组对应元素不为 0 时更新答案
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxSum(vector<int>& nums) 
    {
        int result = -1, max_value[10]{ 0 };
        for (const auto& num : nums) 
        {
            int cur = 0;
            for (int v = num; v > 0; v /= 10) cur = max(cur, v % 10);
            if (max_value[cur]) result = max(result, num + max_value[cur]);
            max_value[cur] = max(num, max_value[cur]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxSum(int[] nums) {
        int result = -1, maxValue[] = new int[10];
        for (int num : nums) {
            int cur = 0;
            for (int v = num; v > 0; v /= 10) cur = Math.max(cur, v % 10);
            if (maxValue[cur] > 0) result = Math.max(result, num + maxValue[cur]);
            maxValue[cur] = Math.max(num, maxValue[cur]);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxSum(self, nums: List[int]) -> int:
        return max([nums[i] + nums[j] for i in range(len(nums)) for j in range(i + 1, len(nums)) if max(str(nums[i])) == max(str(nums[j]))], default=-1)
```
