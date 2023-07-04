# 1695 Maximum Erasure Value 删除子数组的最大得分

__Description:__

You are given an array of positive integers `nums` and want to erase a subarray containing __unique elements__. The __score__ you get by erasing the subarray is equal to the __sum__ of its elements.

Return the maximum score you can get by erasing exactly one subarray.

An array `b` is called to be a subarray of `a` if it forms a contiguous subsequence of `a`, that is, if it is equal to `a[l],a[l+1],...,a[r]` for some `(l,r)`.

__Example:__

Example 1:

```text
Input: nums = [4,2,4,5,6]
Output: 17
Explanation: The optimal subarray here is [2,4,5,6].
```

Example 2:

```text
Input: nums = [5,2,1,2,5,2,1,2,5]
Output: 8
Explanation: The optimal subarray here is [5,2,1] or [1,2,5].
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 4`

__题目描述:__

给你一个正整数数组 `nums` ，请你从中删除一个含有 __若干不同元素__ 的子数组。删除子数组的 __得分__ 就是子数组各元素之 __和__ 。

返回 只删除一个 子数组可获得的 最大得分 。

如果数组 `b` 是数组 `a` 的一个连续子序列，即如果它等于 `a[l],a[l+1],...,a[r]` ，那么它就是 `a` 的一个子数组。

__示例:__

示例 1：

```text
输入：nums = [4,2,4,5,6]
输出：17
解释：最优子数组是 [2,4,5,6]
```

示例 2：

```text
输入：nums = [5,2,1,2,5,2,1,2,5]
输出：8
解释：最优子数组是 [5,2,1] 或 [1,2,5]
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 4`

__思路:__

```text
滑动窗口
每次将窗口的右边界向右移动一位, 并将右边界的元素加入窗口
如果这时窗口内有重复元素, 则将窗口的左边界向右移动一位, 并将左边界的元素从窗口中移除, 并记录窗口内的和
窗口内都是独特值时更新结果
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumUniqueSubarray(vector<int>& nums) 
    {
        int max_value = *max_element(nums.begin(), nums.end()), left = 0, result = 0, s = 0, n = nums.size(), window[max_value + 1];
        memset(window, 0, sizeof window);
        for (int right = 0; right < n; right++) 
        {
            int num = nums[right];
            if (window[num]) 
            {
                result = max(result, s);
                while (nums[left] != num) 
                {
                    s -= nums[left];
                    window[nums[left++]] = false;
                }
                s -= nums[left++];
            } 
            else window[num] = true;
            s += num;
        }
        return max(result, s);
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumUniqueSubarray(int[] nums) {
        int max = 0, left = 0, result = 0, s = 0, n = nums.length;
        for (int num : nums) max = Math.max(max, num);
        boolean[] window = new boolean[max + 1];
        for (int right = 0; right < n; right++) {
            int num = nums[right];
            if (window[num]) {
                result = Math.max(result, s);
                while (nums[left] != num) {
                    s -= nums[left];
                    window[nums[left++]] = false;
                }
                s -= nums[left++];
            } else window[num] = true;
            s += num;
        }
        return Math.max(result, s);
    }
}
```

__Python__:

```Python
class Solution:
    def maximumUniqueSubarray(self, nums: List[int]) -> int:
        c, result, s, left, n = Counter(), 0, 0, 0, len(nums)
        for right in range(n):
            c[nums[right]] += 1
            s += nums[right]
            while c[nums[right]] > 1:
                c[nums[left]] -= 1
                s -= nums[left]
                left += 1
            result = max(result, s)
        return result
```
