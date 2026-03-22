# 2176 Count Equal and Divisible Pairs in an Array 统计数组中相等且可以被整除的数对

__Description:__

Given a __0-indexed__ integer array `nums` of length `n` and an integer `k`, return the number of pairs (i, j) where `0 <= i < j < n`, such that `nums[i] == nums[j]` and `(i * j)` is divisible by `k`.

__Example:__

Example 1:

```text
Input: nums = [3,1,2,2,2,1,3], k = 2
Output: 4
Explanation:
There are 4 pairs that meet all the requirements:
```

- nums[0] == nums[6], and 0 * 6 == 0, which is divisible by 2.
- nums[2] == nums[3], and 2 * 3 == 6, which is divisible by 2.
- nums[2] == nums[4], and 2 * 4 == 8, which is divisible by 2.
- nums[3] == nums[4], and 3 * 4 == 12, which is divisible by 2.

Example 2:

```text
Input: nums = [1,2,3,4], k = 1
Output: 0
Explanation: Since no value in nums is repeated, there are no pairs (i,j) that meet all the requirements.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `1 <= nums[i], k <= 100`

__题目描述:__

给你一个下标从 __0__ 开始长度为 `n` 的整数数组 `nums` 和一个整数 `k` ，请你返回满足 `0 <= i < j < n` ， `nums[i] == nums[j]` 且 `(i * j)` 能被 `k` 整除的数对 `(i, j)` 的 __数目__ 。

__示例:__

示例 1：

```text
输入：nums = [3,1,2,2,2,1,3], k = 2
输出：4
解释：
总共有 4 对数符合所有要求：
```

- nums[0] == nums[6] 且 0 * 6 == 0 ，能被 2 整除。
- nums[2] == nums[3] 且 2 * 3 == 6 ，能被 2 整除。
- nums[2] == nums[4] 且 2 * 4 == 8 ，能被 2 整除。
- nums[3] == nums[4] 且 3 * 4 == 12 ，能被 2 整除。

示例 2：

```text
输入：nums = [1,2,3,4], k = 1
输出：0
解释：由于数组中没有重复数值，所以没有数对 (i,j) 符合所有要求。
```

__提示：__

- `1 <= nums.length <= 100`
- `1 <= nums[i], k <= 100`

__思路:__

```text
1. 暴力法
直接统计有多少个数组满足条件即可
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 哈希表
记录每个数因子出现的次数, 然后遍历哈希表, 计算满足条件的数对
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countPairs(vector<int>& nums, int k) 
    {
        int result = 0, n = nums.size();
        for (int i = 0; i < n; i++) for (int j = i + 1; j < n; j++) if (nums[i] == nums[j] and !((i * j) % k)) ++result;
        return result;   
    }
};
```

__Java__:

```Java
class Solution {
    public int countPairs(int[] nums, int k) {
        int result = 0, n = nums.length;
        for (int i = 0; i < n; i++) for (int j = i + 1; j < n; j++) if (nums[i] == nums[j] && (i * j) % k == 0) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countPairs(self, nums: List[int], k: int) -> int:
        # return sum(nums[i] == nums[j] and not i * j % k for i in range(len(nums)) for j in range(i + 1, len(nums)))
        n, c, d, a = len(nums), defaultdict(lambda: defaultdict(int)), Counter(nums), 0
        for factor in range(1, n):
            for multi in range(factor, n, factor):
                c[factor][nums[multi]] += 1
        return d[nums[0]] - 1 + (sum(c[k // gcd((i + 1), k)][v] - (not ((i + 1) ** 2) % k) for i, v in enumerate(nums[1:])) >> 1)
```
