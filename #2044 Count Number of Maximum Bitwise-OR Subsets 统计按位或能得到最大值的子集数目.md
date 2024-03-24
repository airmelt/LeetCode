# 2044 Count Number of Maximum Bitwise-OR Subsets 统计按位或能得到最大值的子集数目

__Description:__

Given an integer array `nums`, find the __maximum__ possible __bitwise OR__ of a subset of `nums` and return _the __number of different non-empty subsets__ with the maximum bitwise OR_.

An array `a` is a __subset__ of an array `b` if `a` can be obtained from `b` by deleting some (possibly zero) elements of `b`. Two subsets are considered __different__ if the indices of the elements chosen are different.

The bitwise OR of an array `a` is equal to a[0] __OR__ a[1] __OR__ ... __OR__ a[a.length - 1] (__0-indexed__).

__Example:__

Example 1:

```text
Input: nums = [3,1]
Output: 2
Explanation: The maximum possible bitwise OR of a subset is 3. There are 2 subsets with a bitwise OR of 3:
```

- [3]
- [3,1]

Example 2:

```text
Input: nums = [2,2,2]
Output: 7
Explanation: All non-empty subsets of [2,2,2] have a bitwise OR of 2. There are 23 - 1 = 7 total subsets.
```

Example 3:

```text
Input: nums = [3,2,1,5]
Output: 6
Explanation: The maximum possible bitwise OR of a subset is 7. There are 6 subsets with a bitwise OR of 7:
```

- [3,5]
- [3,1,5]
- [3,2,5]
- [3,2,1,5]
- [2,5]
- [2,1,5]

__Constraints:__

- `1 <= nums.length <= 16`
- `1 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个整数数组 `nums` ，请你找出 `nums` 子集 __按位或__ 可能得到的 __最大值__ ，并返回按位或能得到最大值的 __不同非空子集的数目__ 。

如果数组 `a` 可以由数组 `b` 删除一些元素（或不删除）得到，则认为数组 `a` 是数组 `b` 的一个 __子集__ 。如果选中的元素下标位置不一样，则认为两个子集 __不同__ 。

对数组 `a` 执行 __按位或__ ，结果等于 a[0] __OR__ a[1] __OR__ ... __OR__ a[a.length - 1]（下标从 __0__ 开始）。

__示例:__

示例 1：

```text
输入：nums = [3,1]
输出：2
解释：子集按位或能得到的最大值是 3 。有 2 个子集按位或可以得到 3 ：
```

- [3]
- [3,1]

示例 2：

```text
输入：nums = [2,2,2]
输出：7
解释：[2,2,2] 的所有非空子集的按位或都可以得到 2 。总共有 23 - 1 = 7 个子集。
```

示例 3：

```text
输入：nums = [3,2,1,5]
输出：6
解释：子集按位或可能的最大值是 7 。有 6 个子集按位或可以得到 7 ：
```

- [3,5]
- [3,1,5]
- [3,2,5]
- [3,2,1,5]
- [2,5]
- [2,1,5]

__提示：__

- `1 <= nums.length <= 16`
- `1 <= nums[i] <= 10 ^ 5`

__思路:__

```text
回溯
由于或运算单调不减, 因此可以将数组中所有元素进行或运算, 得到最大值
然后使用 DFS 进行回溯, 每次遍历数组, 有两种选择, 选或不选
如果累计值等于最大值, 则返回 1 << (n - start)
到达数组末尾, 返回 0
时间复杂度为 O(2 ^ N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countMaxOrSubsets(vector<int>& nums) 
    {
        int n = nums.size(), target = reduce(nums.begin(), nums.end(), 0, bit_or{});
        function<int(int, int)> dfs = [&](int start, int cur)
        {
            if (cur == target) return 1 << (n - start);
            if (n == start) return 0;
            return dfs(start + 1, cur) + dfs(start + 1, cur | nums[start]);
        };
        return dfs(0, 0);
    }
};
```

__Java__:

```Java
class Solution {
    public int countMaxOrSubsets(int[] nums) {
        int target = 0, n = nums.length;
        for (int num : nums) target |= num;
        return dfs(0, 0, n, target, nums);
    }

    private int dfs(int start, int cur, int n, int target, int[] nums) {
        if (start == n) return cur == target ? 1 : 0;
        return dfs(start + 1, cur | nums[start], n, target, nums) + dfs(start + 1, cur, n, target, nums);
    }
}
```

__Python__:

```Python
class Solution:
    def countMaxOrSubsets(self, nums: List[int]) -> int:
        target, n = reduce(or_, nums), len(nums)
        
        def dfs(i: int, cur: int) -> int:
            if i == n:
                return cur == target
            return dfs(i + 1, cur | nums[i]) + dfs(i + 1, cur)
        return dfs(0, 0)
```
