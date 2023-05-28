# 1646 Get Maximum in Generated Array 获取生成数组中的最大值

__Description:__

You are given an integer `n`. A __0-indexed__ integer array `nums` of length `n + 1` is generated in the following way:

- `nums[0] = 0`
- `nums[1] = 1`
- `nums[2 * i] = nums[i]` when `2 <= 2 * i <= n`
- `nums[2 * i + 1] = nums[i] + nums[i + 1]` when `2 <= 2 * i + 1 <= n`

Return _the __maximum__ integer in the array_ `nums`​​​.

__Example:__

Example 1:

```text
Input: n = 7
Output: 3
Explanation: According to the given rules:
  nums[0] = 0
  nums[1] = 1
  nums[(1 * 2) = 2] = nums[1] = 1
  nums[(1 * 2) + 1 = 3] = nums[1] + nums[2] = 1 + 1 = 2
  nums[(2 * 2) = 4] = nums[2] = 1
  nums[(2 * 2) + 1 = 5] = nums[2] + nums[3] = 1 + 2 = 3
  nums[(3 * 2) = 6] = nums[3] = 2
  nums[(3 * 2) + 1 = 7] = nums[3] + nums[4] = 2 + 1 = 3
Hence, nums = [0,1,1,2,1,3,2,3], and the maximum is max(0,1,1,2,1,3,2,3) = 3.
```

Example 2:

```text
Input: n = 2
Output: 1
Explanation: According to the given rules, nums = [0,1,1]. The maximum is max(0,1,1) = 1.
```

Example 3:

```text
Input: n = 3
Output: 2
Explanation: According to the given rules, nums = [0,1,1,2]. The maximum is max(0,1,1,2) = 2.
```

__Constraints:__

- `0 <= n <= 100`

__题目描述:__

给你一个整数 `n` 。按下述规则生成一个长度为 `n + 1` 的数组 `nums` :

- `nums[0] = 0`
- `nums[1] = 1`
- 当 `2 <= 2 * i <= n` 时， `nums[2 * i] = nums[i]`
- 当 `2 <= 2 * i + 1 <= n` 时， `nums[2 * i + 1] = nums[i] + nums[i + 1]`

返回生成数组 `nums` 中的 __最大__ 值。

__示例:__

示例 1：

```text
输入：n = 7
输出：3
解释：根据规则：
  nums[0] = 0
  nums[1] = 1
  nums[(1 * 2) = 2] = nums[1] = 1
  nums[(1 * 2) + 1 = 3] = nums[1] + nums[2] = 1 + 1 = 2
  nums[(2 * 2) = 4] = nums[2] = 1
  nums[(2 * 2) + 1 = 5] = nums[2] + nums[3] = 1 + 2 = 3
  nums[(3 * 2) = 6] = nums[3] = 2
  nums[(3 * 2) + 1 = 7] = nums[3] + nums[4] = 2 + 1 = 3
因此，nums = [0,1,1,2,1,3,2,3]，最大值 3
```

示例 2：

```text
输入：n = 2
输出：1
解释：根据规则，nums[0]、nums[1] 和 nums[2] 之中的最大值是 1
```

示例 3：

```text
输入：n = 3
输出：2
解释：根据规则，nums[0]、nums[1]、nums[2] 和 nums[3] 之中的最大值是 2
```

__提示：__

- `0 <= n <= 100`

__思路:__

```text
1. 模拟
可以将奇偶下标的元素合并为
nums[i] = nums[i >> 1] + (i & 1) * nums[(i >> 1) + 1]
通过 i & 1 判断奇偶是否需要加
时间复杂度为 O(N), 空间复杂度为 O(N)
2. 打表
直接预计算 100 以内的所有数字打表即可
时间复杂度为 O(1), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int getMaximumGenerated(int n) 
    {
        vector<int> nums(n + 2);
        nums[1] = 1;
        for (int i = 2; i <= n; ++i) nums[i] = nums[i >> 1] + (i & 1) * nums[(i >> 1) + 1];
        return n > 0 ? *max_element(nums.begin(), nums.end()) : 0;
    }
};
```

__Java__:

```Java
class Solution {
    public int getMaximumGenerated(int n) {
        int[] nums = new int[n + 2];
        nums[1] = 1;
        for (int i = 2; i <= n; ++i) nums[i] = nums[i >> 1] + (i & 1) * nums[(i >> 1) + 1];
        return n > 0 ? Arrays.stream(nums).max().getAsInt() : 0;
    }
}
```

__Python__:

```Python
class Solution:
    def getMaximumGenerated(self, n: int) -> int:
        return [0, 1, 1, 2, 2, 3, 3, 3, 3, 4, 4, 5, 5, 5, 5, 5, 5, 5, 5, 7, 7, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 9, 9, 11, 11, 11, 11, 11, 11, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 14, 14, 14, 14, 15, 15, 18, 18, 18, 18, 18, 18, 18, 18, 19, 19, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21, 21][n]
```
