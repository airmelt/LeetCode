# 2587 Rearrange Array to Maximize Prefix Score 重排数组以得到最大前缀分数

__Description:__

You are given a __0-indexed__ integer array `nums`. You can rearrange the elements of `nums` to __any order__ (including the given order).

Let `prefix` be the array containing the prefix sums of `nums` after rearranging it. In other words, `prefix[i]` is the sum of the elements from `0` to `i` in `nums` after rearranging it. The __score__ of `nums` is the number of positive integers in the array `prefix`.

Return the maximum score you can achieve.

__Example:__

Example 1:

```text
Input: nums = [2,-1,0,1,-3,3,-3]
Output: 6
Explanation: We can rearrange the array into nums = [2,3,1,-1,-3,0,-3].
prefix = [2,5,6,5,2,2,-1], so the score is 6.
It can be shown that 6 is the maximum score we can obtain.
```

Example 2:

```text
Input: nums = [-2,-3,0]
Output: 0
Explanation: Any rearrangement of the array will result in a score of 0.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 6 <= nums[i] <= 10 ^ 6`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。你可以将 `nums` 中的元素按 __任意顺序__ 重排（包括给定顺序）。

令 `prefix` 为一个数组，它包含了 `nums` 重新排列后的前缀和。换句话说， `prefix[i]` 是 `nums` 重新排列后下标从 `0` 到 `i` 的元素之和。 `nums` 的 __分数__ 是 `prefix` 数组中正整数的个数。

返回可以得到的最大分数。

__示例:__

示例 1：

```text
输入：nums = [2,-1,0,1,-3,3,-3]
输出：6
解释：数组重排为 nums = [2,3,1,-1,-3,0,-3] 。
prefix = [2,5,6,5,2,2,-1] ，分数为 6 。
可以证明 6 是能够得到的最大分数。
```

示例 2：

```text
输入：nums = [-2,-3,0]
输出：0
解释：不管怎么重排数组得到的分数都是 0 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 6 <= nums[i] <= 10 ^ 6`

__思路:__

```text
排序
排序完成之后从大到小计算前缀和
统计前缀和中大于 0 的个数即可
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxScore(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        long long result = 0LL, s = 0LL;
        for (int n = nums.size(), i = n - 1; i > -1; i--) 
        {
            s += nums[i];
            if (s > 0L) ++result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxScore(int[] nums) {
        Arrays.sort(nums);
        long result = 0L, s = 0L;
        for (int n = nums.length, i = n - 1; i > -1; i--) {
            s += nums[i];
            if (s > 0L) ++result;
        }
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxScore(self, nums: List[int]) -> int:
        return sum(i > 0 for i in list(accumulate(sorted(nums, reverse=True))))
```
