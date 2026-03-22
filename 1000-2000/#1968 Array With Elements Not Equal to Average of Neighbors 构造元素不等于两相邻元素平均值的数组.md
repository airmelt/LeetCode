# 1968 Array With Elements Not Equal to Average of Neighbors 构造元素不等于两相邻元素平均值的数组

__Description:__

You are given a __0-indexed__ array `nums` of __distinct__ integers. You want to rearrange the elements in the array such that every element in the rearranged array is __not__ equal to the __average__ of its neighbors.

More formally, the rearranged array should have the property such that for every `i` in the range `1 <= i < nums.length - 1`, `(nums[i-1] + nums[i+1]) / 2` is __not__ equal to `nums[i]`.

Return ___any__ rearrangement of_ `nums` _that meets the requirements_.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4,5]
Output: [1,2,4,5,3]
Explanation:
When i=1, nums[i] = 2, and the average of its neighbors is (1+4) / 2 = 2.5.
When i=2, nums[i] = 4, and the average of its neighbors is (2+5) / 2 = 3.5.
When i=3, nums[i] = 5, and the average of its neighbors is (4+3) / 2 = 3.5.
```

Example 2:

```text
Input: nums = [6,2,0,9,7]
Output: [9,7,6,2,0]
Explanation:
When i=1, nums[i] = 7, and the average of its neighbors is (9+6) / 2 = 7.5.
When i=2, nums[i] = 6, and the average of its neighbors is (7+2) / 2 = 4.5.
When i=3, nums[i] = 2, and the average of its neighbors is (6+0) / 2 = 3.
```

__Constraints:__

- `3 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个 __下标从 0 开始__ 的数组 `nums` ，数组由若干 __互不相同的__ 整数组成。你打算重新排列数组中的元素以满足:重排后，数组中的每个元素都 __不等于__ 其两侧相邻元素的 __平均值__ 。

更公式化的说法是，重新排列的数组应当满足这一属性:对于范围 `1 <= i < nums.length - 1` 中的每个 `i` ， `(nums[i-1] + nums[i+1]) / 2` __不等于__ `nums[i]` 均成立 。

返回满足题意的任一重排结果。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4,5]
输出：[1,2,4,5,3]
解释：
i=1, nums[i] = 2, 两相邻元素平均值为 (1+4) / 2 = 2.5
i=2, nums[i] = 4, 两相邻元素平均值为 (2+5) / 2 = 3.5
i=3, nums[i] = 5, 两相邻元素平均值为 (4+3) / 2 = 3.5
```

示例 2：

```text
输入：nums = [6,2,0,9,7]
输出：[9,7,6,2,0]
解释：
i=1, nums[i] = 7, 两相邻元素平均值为 (9+6) / 2 = 7.5
i=2, nums[i] = 6, 两相邻元素平均值为 (7+2) / 2 = 4.5
i=3, nums[i] = 2, 两相邻元素平均值为 (6+0) / 2 = 3
```

__提示：__

- `3 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 5`

__思路:__

```text
1. 排序
排序之后将数组分为两部分, 一部分为较大的一部分, 一部分为较小的一部分
然后将较大的一部分和较小的一部分交替放入数组中即可
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
2. 贪心
类似摆动数组
奇数位置大于前一个元素
偶数位置小于前一个元素
不满足条件则交换
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> rearrangeArray(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        for (int i = 1, n = nums.size(); i < n - 1; i += 2) swap(nums[i + 1], nums[i]);
        return nums;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] rearrangeArray(int[] nums) {
        Arrays.sort(nums);
        for (int i = 1, n = nums.length; i < n - 1; i += 2) {
            nums[i] ^= nums[i + 1];
            nums[i + 1] ^= nums[i];
            nums[i] ^= nums[i + 1];
        }
        return nums;
    }
}
```

__Python__:

```Python
class Solution:
    def rearrangeArray(self, nums: List[int]) -> List[int]:
        return list(chain.from_iterable([[i, j] for i, j in zip(sorted(nums)[len(nums) >> 1:], sorted(nums)[:len(nums) >> 1])])) + ([max(nums)] if len(nums) & 1 else [])
```
