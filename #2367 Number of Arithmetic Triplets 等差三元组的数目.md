# 2367 Number of Arithmetic Triplets 等差三元组的数目

__Description:__

You are given a __0-indexed__, __strictly increasing__ integer array `nums` and a positive integer `diff`. A triplet `(i, j, k)` is an __arithmetic triplet__ if the following conditions are met:

- `i < j < k`,
- `nums[j] - nums[i] == diff`, and
- `nums[k] - nums[j] == diff`.

Return the number of unique arithmetic triplets.

__Example:__

Example 1:

```text
Input: nums = [0,1,4,6,7,10], diff = 3
Output: 2
Explanation:
(1, 2, 4) is an arithmetic triplet because both 7 - 4 == 3 and 4 - 1 == 3.
(2, 4, 5) is an arithmetic triplet because both 10 - 7 == 3 and 7 - 4 == 3.
```

Example 2:

```text
Input: nums = [4,5,6,7,8,9], diff = 2
Output: 2
Explanation:
(0, 2, 4) is an arithmetic triplet because both 8 - 6 == 2 and 6 - 4 == 2.
(1, 3, 5) is an arithmetic triplet because both 9 - 7 == 2 and 7 - 5 == 2.
```

__Constraints:__

- `3 <= nums.length <= 200`
- `0 <= nums[i] <= 200`
- `1 <= diff <= 50`
- `nums` is __strictly__ increasing.

__题目描述:__

给你一个下标从 __0__ 开始、__严格递增__ 的整数数组 `nums` 和一个正整数 `diff` 。如果满足下述全部条件，则三元组 `(i, j, k)` 就是一个 __等差三元组__ :

- `i < j < k` ，
- `nums[j] - nums[i] == diff` 且
- `nums[k] - nums[j] == diff`

返回不同 等差三元组 的数目。

__示例:__

示例 1：

```text
输入：nums = [0,1,4,6,7,10], diff = 3
输出：2
解释：
(1, 2, 4) 是等差三元组：7 - 4 == 3 且 4 - 1 == 3 。
(2, 4, 5) 是等差三元组：10 - 7 == 3 且 7 - 4 == 3 。
```

示例 2：

```text
输入：nums = [4,5,6,7,8,9], diff = 2
输出：2
解释：
(0, 2, 4) 是等差三元组：8 - 6 == 2 且 6 - 4 == 2 。
(1, 3, 5) 是等差三元组：9 - 7 == 2 且 7 - 5 == 2 。
```

__提示：__

- `3 <= nums.length <= 200`
- `0 <= nums[i] <= 200`
- `1 <= diff <= 50`
- `nums` __严格__ 递增

__思路:__

```text
1. 暴力法
使用三重循环遍历所有可能的等差三元组
时间复杂度为 O(N ^ 3), 空间复杂度为 O(1)
2. 哈希表
一边加入元素，一边查找等差三元组
可以认为加入的元素为三元组的最大值
所以只需要查找 nums[i] - diff 和 nums[i] - diff * 2 是否在哈希表中即可
时间复杂度为 O(N), 空间复杂度为 O(N)
3. 双指针
令 diff2 = diff * 2
使用两个指针 i 和 j 分别指向等差三元组的第一个和第二个元素
先移动 j 使得 nums[j] + diff >= x
如果 nums[j] + diff == x
则移动 i 使得 nums[i] + diff2 >= x
如果 nums[i] + diff2 == x
则结果加一
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int arithmeticTriplets(vector<int>& nums, int diff) 
    {
        int result = 0, i = 0, j = 1, diff2 = diff << 1;
        for (const auto& x : nums)
        {
            while (nums[j] + diff < x) ++j;
            if (nums[j] + diff > x) continue;
            while (nums[i] + diff2 < x) ++i;
            result += nums[i] + diff2 == x;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int arithmeticTriplets(int[] nums, int diff) {
        int result = 0, diff2 = diff << 1;
        var set = new HashSet<Integer>();
        for (int x : nums) {
            if (set.contains(x - diff) && set.contains(x - diff2)) ++result;
            set.add(x);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def arithmeticTriplets(self, nums: List[int], diff: int) -> int:
        return sum(nums[j] - nums[i] == diff and nums[k] - nums[j] == diff for i in range(len(nums)) for j in range(i + 1, len(nums)) for k in range(j + 1, len(nums)))
```
