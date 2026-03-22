# 996 Number of Squareful Arrays 正方形数组的数目

__Description__:
An array is squareful if the sum of every pair of adjacent elements is a perfect square.

Given an integer array nums, return the number of permutations of nums that are squareful.

Two permutations perm1 and perm2 are different if there is some index i such that perm1[i] != perm2[i].

__Example:__

Example 1:

Input: nums = [1,17,8]
Output: 2
Explanation: [1,8,17] and [17,8,1] are the valid permutations.

Example 2:

Input: nums = [2,2,2]
Output: 1

__Constraints:__

1 <= nums.length <= 12
0 <= nums[i] <= 10^9

__题目描述__:
给定一个非负整数数组 A，如果该数组每对相邻元素之和是一个完全平方数，则称这一数组为正方形数组。

返回 A 的正方形排列的数目。两个排列 A1 和 A2 不同的充要条件是存在某个索引 i，使得 A1[i] != A2[i]。

__示例 :__

示例 1：

输入：[1,17,8]
输出：2
解释：
[1,8,17] 和 [17,8,1] 都是有效的排列。

示例 2：

输入：[2,2,2]
输出：1

__提示:__

1 <= A.length <= 12
0 <= A[i] <= 1e9

__思路__:

回溯
先对 nums 进行排序
然后对 nums 进行去重
对 nums 每个排列进行检查, 相邻的两个数之和不为平方数则剪枝
时间复杂度为 O(2 ^ n \* n), 空间复杂度为 O(2 ^ n \* n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numSquarefulPerms(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        int result = 0;
        dfs(nums, 0, 0, result, nums.size());
        return result;
    }
private:
    void dfs(vector<int>& nums, int pos, int pre, int& result, const int n)
    {
        if (pos == n)
        {
            ++result;
            return;
        }
        for (int i = 0; i < n; i++)
        {
            if (nums[i] != -1)
            {
                if (i and nums[i] == nums[i - 1]) continue;
                int record = nums[i], s = sqrt(pre + record);
                if (!pos or s * s == record + pre)
                {
                    nums[i] = -1;
                    dfs(nums, pos + 1, record, result, n);
                    nums[i] = record;
                }
            }
        }
    }
};
```

__Java__:

```Java
class Solution {
    public int numSquarefulPerms(int[] nums) {
        Arrays.sort(nums);
        dfs(nums, 0, 0, nums.length);
        return result;
    }
    
    private int result = 0;
    
    private void dfs(int[] nums, int pos, int pre, int n) {
        if (pos == n) {
            ++result;
            return;
        }
        for (int i = 0; i < n; i++) {
            if (nums[i] != -1) {
                if (i != 0 && nums[i] == nums[i - 1]) continue;
                int record = nums[i], s = (int)Math.sqrt(pre + record);
                if (pos == 0 || s * s == record + pre) {
                    nums[i] = -1;
                    dfs(nums, pos + 1, record, n);
                    nums[i] = record;
                }
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def numSquarefulPerms(self, nums: List[int]) -> int:
        nums.sort()
        n, result = len(nums), 0
        
        def dfs(pos: int, pre: int) -> None:
            nonlocal result
            if pos == n:
                result += 1
                return
            for i in range(n):
                if nums[i] != -1:
                    if i and nums[i] == nums[i - 1]:
                        continue
                    if (s := int(sqrt((record := nums[i]) + pre))) ** 2 == record + pre or not pos:
                        nums[i] = -1
                        dfs(pos + 1, record)
                        nums[i] = record
                        
        dfs(0, 0)
        return result
```
