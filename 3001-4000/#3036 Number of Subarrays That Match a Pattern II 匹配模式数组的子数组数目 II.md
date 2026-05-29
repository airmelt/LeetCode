# 3036 Number of Subarrays That Match a Pattern II 匹配模式数组的子数组数目 II

__Description:__

You are given a __0-indexed__ integer array `nums` of size `n`, and a __0-indexed__ integer array `pattern` of size `m` consisting of integers `-1`, `0`, and `1`.

A subarray `nums[i..j]` of size `m + 1` is said to match the `pattern` if the following conditions hold for each element `pattern[k]`:

- `nums[i + k + 1] > nums[i + k]` if `pattern[k] == 1`.
- `nums[i + k + 1] == nums[i + k]` if `pattern[k] == 0`.
- `nums[i + k + 1] < nums[i + k]` if `pattern[k] == -1`.

Return _the __count__ of subarrays in_ `nums` _that match the_ `pattern`.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4,5,6], pattern = [1,1]
Output: 4
Explanation: The pattern [1,1] indicates that we are looking for strictly increasing subarrays of size 3. In the array nums, the subarrays [1,2,3], [2,3,4], [3,4,5], and [4,5,6] match this pattern.
Hence, there are 4 subarrays in nums that match the pattern.
```

Example 2:

```text
Input: nums = [1,4,4,1,3,5,5,3], pattern = [1,0,-1]
Output: 2
Explanation: Here, the pattern [1,0,-1] indicates that we are looking for a sequence where the first number is smaller than the second, the second is equal to the third, and the third is greater than the fourth. In the array nums, the subarrays [1,4,4,1], and [3,5,5,3] match this pattern.
Hence, there are 2 subarrays in nums that match the pattern.
```

__Constraints:__

- `2 <= n == nums.length <= 10 ^ 6`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= m == pattern.length < n`
- `-1 <= pattern[i] <= 1`

__题目描述:__

给你一个下标从 __0__ 开始长度为 `n` 的整数数组 `nums` ，和一个下标从 `0` 开始长度为 `m` 的整数数组 `pattern` ， `pattern` 数组只包含整数 `-1` ， `0` 和 `1` 。

大小为 `m + 1` 的子数组 `nums[i..j]` 如果对于每个元素 `pattern[k]` 都满足以下条件，那么我们说这个子数组匹配模式数组 `pattern` :

- 如果 `pattern[k] == 1` ，那么 `nums[i + k + 1] > nums[i + k]`
- 如果 `pattern[k] == 0` ，那么 `nums[i + k + 1] == nums[i + k]`
- 如果 `pattern[k] == -1` ，那么 `nums[i + k + 1] < nums[i + k]`

请你返回匹配 `pattern` 的 `nums` 子数组的 __数目__ 。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4,5,6], pattern = [1,1]
输出：4
解释：模式 [1,1] 说明我们要找的子数组是长度为 3 且严格上升的。在数组 nums 中，子数组 [1,2,3] ，[2,3,4] ，[3,4,5] 和 [4,5,6] 都匹配这个模式。
所以 nums 中总共有 4 个子数组匹配这个模式。
```

示例 2：

```text
输入：nums = [1,4,4,1,3,5,5,3], pattern = [1,0,-1]
输出：2
解释：这里，模式数组 [1,0,-1] 说明我们需要找的子数组中，第一个元素小于第二个元素，第二个元素等于第三个元素，第三个元素大于第四个元素。在 nums 中，子数组 [1,4,4,1] 和 [3,5,5,3] 都匹配这个模式。
所以 nums 中总共有 2 个子数组匹配这个模式。
```

__提示：__

- `2 <= n == nums.length <= 10 ^ 6`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= m == pattern.length < n`
- `-1 <= pattern[i] <= 1`

__思路:__

```text
z 函数
将 pattern 看作是前缀
再接一个不在 pattern 里的元素确保 pattern 和 nums 分开
然后将 nums 里的元素按照要求转化成 -1/0/1 接在 pattern 后面
然后用 z 函数求出前缀
z 函数里面刚好等于 pattern 长度的即为答案
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countMatchingSubarrays(vector<int>& nums, vector<int>& pattern) 
    {
        int m = pattern.size(), n = m + nums.size(), l = 0, r = 0, result = 0;
        pattern.emplace_back(2);
        for (int i = 1, q = nums.size(); i < q; i++) pattern.emplace_back((nums[i] > nums[i - 1]) - (nums[i - 1] > nums[i]));
        vector<int> z(n);
        for (int i = 1; i < n; i++) 
        {
            if (i <= r and z[i - l] < r - i + 1) z[i] = z[i - l];
            else
            {
                z[i] = max(0, r - i + 1);
                while (i + z[i] < n and pattern[z[i]] == pattern[i + z[i]]) ++z[i];
            }
            if (i + z[i] - 1 > r)
            {
                l = i;
                r = i + z[i] - 1;
            }
        }
        for (int i = 0; i < n; i++) result += z[i] == m;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countMatchingSubarrays(int[] nums, int[] pattern) {
        int m = pattern.length, n = m + nums.length, z[] = new int[n], l = 0, r = 0, word[] = new int[n], result = 0;
        for (int i = 0; i < m; i++) word[i] = pattern[i];
        for (int i = m + 1; i < n; i++) word[i] = (nums[i - m] > nums[i - m - 1] ? 1 : 0) - (nums[i - m] < nums[i - m - 1] ? 1 : 0);
        word[m] = 2;
        for (int i = 1; i < n; i++) {
            if (i <= r && z[i - l] < r - i + 1) z[i] = z[i - l];
            else {
                z[i] = Math.max(0, r - i + 1);
                while (i + z[i] < n && word[z[i]] == word[i + z[i]]) ++z[i];
            }
            if (i + z[i] - 1 > r) {
                l = i;
                r = i + z[i] - 1;
            }
        }
        for (int i = 0; i < n; i++) if (z[i] == m) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countMatchingSubarrays(self, nums: List[int], pattern: List[int]) -> int:
        nums, m = pattern + [2] + [(y > x) - (x > y) for x, y in pairwise(nums)], len(pattern)
        z, l, r = [0] * (n := len(nums)), 0, 0
        for i in range(1, n):
            if i <= r and z[i - l] < r - i + 1:
                z[i] = z[i - l]
            else:
                z[i] = max(0, r - i + 1)
                while i + z[i] < n and nums[z[i]] == nums[i + z[i]]:
                    z[i] += 1
            if i + z[i] - 1 > r:
                l, r = i, i + z[i] - 1
        return sum(v == m for v in z[m + 1:])
```
