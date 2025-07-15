# 2592 Maximize Greatness of an Array 最大化数组的伟大值

__Description:__

You are given a 0-indexed integer array `nums`. You are allowed to permute `nums` into a new array `perm` of your choosing.

We define the __greatness__ of `nums` be the number of indices `0 <= i < nums.length` for which `perm[i] > nums[i]`.

Return _the __maximum__ possible greatness you can achieve after permuting_ `nums`.

__Example:__

Example 1:

```text
Input: nums = [1,3,5,2,1,3,1]
Output: 4
Explanation: One of the optimal rearrangements is perm = [2,5,1,3,3,1,1].
At indices = 0, 1, 3, and 4, perm[i] > nums[i]. Hence, we return 4.
```

Example 2:

```text
Input: nums = [1,2,3,4]
Output: 3
Explanation: We can prove the optimal perm is [2,3,4,1].
At indices = 0, 1, and 2, perm[i] > nums[i]. Hence, we return 3.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 0 开始的整数数组 `nums` 。你需要将 `nums` 重新排列成一个新的数组 `perm` 。

定义 `nums` 的 __伟大值__ 为满足 `0 <= i < nums.length` 且 `perm[i] > nums[i]` 的下标数目。

请你返回重新排列 `nums` 后的 __最大__ 伟大值。

__示例:__

示例 1：

```text
输入：nums = [1,3,5,2,1,3,1]
输出：4
解释：一个最优安排方案为 perm = [2,5,1,3,3,1,1] 。
在下标为 0, 1, 3 和 4 处，都有 perm[i] > nums[i] 。因此我们返回 4 。
```

示例 2：

```text
输入：nums = [1,2,3,4]
输出：3
解释：最优排列为 [2,3,4,1] 。
在下标为 0, 1 和 2 处，都有 perm[i] > nums[i] 。因此我们返回 3 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`

__思路:__

```text
1. 排序
先对数组进行排序
如果可以找到一个数 perm[i] > nums[i], 则可以增加伟大值
使用双指针记录可以增加伟大值的下标
返回最后指针停留的位置即可
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
2. 哈希表
使用哈希表记录每个数出现的次数
对于数组中出现最多次数的数
一定找不到一个排列能够使得所有的数都大于它
因此可以找到出现次数最多的数, 返回数组长度减去这个数的出现次数
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution {
public:
    int maximizeGreatness(vector<int>& nums) {
        int max_value = 0, n = nums.size();
        unordered_map<int, int> m;
        for (const auto& num : nums) max_value = max(max_value, ++m[num]);
        return n - max_value;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximizeGreatness(int[] nums) {
        Arrays.sort(nums);
        int result = 0;
        for (int num : nums) if (num > nums[result]) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximizeGreatness(self, nums: List[int]) -> int:
        return len(nums) - max(Counter(nums).values())
```
