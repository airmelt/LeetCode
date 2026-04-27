# 2206 Divide Array Into Equal Pairs 将数组划分成相等数对

__Description:__

You are given an integer array `nums` consisting of `2 * n` integers.

You need to divide `nums` into `n` pairs such that:

- Each element belongs to __exactly one__ pair.
- The elements present in a pair are __equal__.

Return `true` _if nums can be divided into_ `n` _pairs, otherwise return_ `false`.

__Example:__

Example 1:

```text
Input: nums = [3,2,3,2,2,2]
Output: true
Explanation: 
There are 6 elements in nums, so they should be divided into 6 / 2 = 3 pairs.
If nums is divided into the pairs (2, 2), (3, 3), and (2, 2), it will satisfy all the conditions.
```

Example 2:

```text
Input: nums = [1,2,3,4]
Output: false
Explanation: 
There is no way to divide nums into 4 / 2 = 2 pairs such that the pairs satisfy every condition.
```

__Constraints:__

- `nums.length == 2 * n`
- `1 <= n <= 500`
- `1 <= nums[i] <= 500`

__题目描述:__

给你一个整数数组 `nums` ，它包含 `2 * n` 个整数。

你需要将 `nums` 划分成 `n` 个数对，满足:

- 每个元素 __只属于一个__ 数对。
- 同一数对中的元素 __相等__ 。

如果可以将 `nums` 划分成 `n` 个数对，请你返回 `true` ，否则返回 `false` 。

__示例:__

示例 1：

```text
输入：nums = [3,2,3,2,2,2]
输出： true
解释：
nums 中总共有 6 个元素，所以它们应该被划分成 6 / 2 = 3 个数对。
nums 可以划分成 (2, 2) ，(3, 3) 和 (2, 2) ，满足所有要求。
```

示例 2：

```text
输入：nums = [1,2,3,4]
输出：false
解释：
无法将 nums 划分成 4 / 2 = 2 个数对且满足所有要求。
```

__提示：__

- `nums.length == 2 * n`
- `1 <= n <= 500`
- `1 <= nums[i] <= 500`

__思路:__

```text
数学
实际上就是每个数不能出现奇数次
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool divideArray(vector<int>& nums) 
    {
        vector<int> c(501);
        for (const auto& num : nums) ++c[num];
        for (const auto& i : c) if (i & 1) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean divideArray(int[] nums) {
        int[] c = new int[501];
        for (int num : nums) ++c[num];
        for (int i : c) if ((i & 1) == 1) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def divideArray(self, nums: List[int]) -> bool:
        return all(not i & 1 for i in Counter(nums).values())
```
