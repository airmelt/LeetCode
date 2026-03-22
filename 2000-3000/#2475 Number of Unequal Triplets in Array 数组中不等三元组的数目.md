# 2475 Number of Unequal Triplets in Array 数组中不等三元组的数目

__Description:__

You are given a __0-indexed__ array of positive integers `nums`. Find the number of triplets `(i, j, k)` that meet the following conditions:

- `0 <= i < j < k < nums.length`
- `nums[i]`, `nums[j]`, and `nums[k]` are __pairwise distinct__.

  - In other words, `nums[i] != nums[j]`, `nums[i] != nums[k]`, and `nums[j] != nums[k]`.

Return the number of triplets that meet the conditions.

__Example:__

Example 1:

```text
Input: nums = [4,4,2,4,3]
Output: 3
Explanation: The following triplets meet the conditions:
```

- (0, 2, 4) because 4 != 2 != 3
- (1, 2, 4) because 4 != 2 != 3
- (2, 3, 4) because 2 != 4 != 3

Since there are 3 triplets, we return 3.

Note that (2, 0, 4) is not a valid triplet because 2 > 0.

Example 2:

```text
Input: nums = [1,1,1,1,1]
Output: 0
Explanation: No triplets meet the conditions so we return 0.
```

__Constraints:__

- `3 <= nums.length <= 100`
- `1 <= nums[i] <= 1000`

__题目描述:__

给你一个下标从 __0__ 开始的正整数数组 `nums` 。请你找出并统计满足下述条件的三元组 `(i, j, k)` 的数目:

- `0 <= i < j < k < nums.length`
- `nums[i]`、 `nums[j]` 和 `nums[k]` __两两不同__ 。

  - 换句话说: `nums[i] != nums[j]`、 `nums[i] != nums[k]` 且 `nums[j] != nums[k]` 。

返回满足上述条件三元组的数目。

__示例:__

示例 1：

```text
输入：nums = [4,4,2,4,3]
输出：3
解释：下面列出的三元组均满足题目条件：
```

- (0, 2, 4) 因为 4 != 2 != 3
- (1, 2, 4) 因为 4 != 2 != 3
- (2, 3, 4) 因为 2 != 4 != 3

共计 3 个三元组，返回 3 。

注意 (2, 0, 4) 不是有效的三元组，因为 2 > 0 。

示例 2：

```text
输入：nums = [1,1,1,1,1]
输出：0
解释：不存在满足条件的三元组，所以返回 0 。
```

__提示：__

- `3 <= nums.length <= 100`
- `1 <= nums[i] <= 1000`

__思路:__

```text
1. 暴力法
直接统计所有的三元组
时间复杂度为 O(N ^ 3), 空间复杂度为 O(1)
2. 哈希表
统计每个数字出现的次数
然后枚举每个数字作为中间数字
每个数字的贡献为 a * b * c
其中 a 为当前数字之前的数字的个数
b 为当前数字的个数
c 为当前数字之后的数字的个数
时间复杂度为 O(N), 空间复杂度为 O(N)
3. 排序
排序后相同的数字在一起
统计每个数字出现的次数
然后枚举每个数字作为中间数字
每个数字的贡献为 a * b * c
其中 a 为当前数字之前的数字的个数
b 为当前数字的个数
c 为当前数字之后的数字的个数
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int unequalTriplets(vector<int>& nums) 
    {
        unordered_map<int, int> m;
        for (const auto& num : nums) ++m[num];
        int result = 0, a = 0, c = nums.size();
        for (const auto& [_, b] : m)
        {
            result += a * b * (c -= b);
            a += b;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int unequalTriplets(int[] nums) {
        Arrays.sort(nums);
        int result = 0, pre = 0, n = nums.length;
        for (int i = 0; i < n - 1; i++) {
            if (nums[i] != nums[i + 1]) {
                result += pre * (i - pre + 1) * (n - 1 - i);
                pre = i + 1;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def unequalTriplets(self, nums: List[int]) -> int:
        return sum(nums[i] != nums[j] and nums[i] != nums[k] and nums[j] != nums[k] for i in range(len(nums)) for j in range(i + 1, len(nums)) for k in range(j + 1, len(nums)))
```
