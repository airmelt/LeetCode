# 2012 Sum of Beauty in the Array 数组美丽值求和

__Description:__

You are given a __0-indexed__ integer array `nums`. For each index `i` ( `1 <= i <= nums.length - 2`) the __beauty__ of `nums[i]` equals:

- `2`, if `nums[j] < nums[i] < nums[k]`, for __all__ `0 <= j < i` and for __all__ `i < k <= nums.length - 1`.
- `1`, if `nums[i - 1] < nums[i] < nums[i + 1]`, and the previous condition is not satisfied.
- `0`, if none of the previous conditions holds.

Return _the __sum of beauty__ of all_ `nums[i]` _where_ `1 <= i <= nums.length - 2`.

__Example:__

Example 1:

```text
Input: nums = [1,2,3]
Output: 2
Explanation: For each index i in the range 1 <= i <= 1:
```

- The beauty of nums[1] equals 2.

Example 2:

```text
Input: nums = [2,4,6,4]
Output: 1
Explanation: For each index i in the range 1 <= i <= 2:
```

- The beauty of nums[1] equals 1.
- The beauty of nums[2] equals 0.

Example 3:

```text
Input: nums = [3,2,1]
Output: 0
Explanation: For each index i in the range 1 <= i <= 1:
```

- The beauty of nums[1] equals 0.

__Constraints:__

- `3 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。对于每个下标 `i`（ `1 <= i <= nums.length - 2`）， `nums[i]` 的 __美丽值__ 等于:

- `2`，对于所有 `0 <= j < i` 且 `i < k <= nums.length - 1` ，满足 `nums[j] < nums[i] < nums[k]`
- `1`，如果满足 `nums[i - 1] < nums[i] < nums[i + 1]` ，且不满足前面的条件
- `0`，如果上述条件全部不满足

返回符合 `1 <= i <= nums.length - 2` 的所有 `nums[i]` 的 __美丽值的总和__ 。

__示例:__

示例 1：

```text
输入：nums = [1,2,3]
输出：2
解释：对于每个符合范围 1 <= i <= 1 的下标 i :
```

- nums[1] 的美丽值等于 2

示例 2：

```text
输入：nums = [2,4,6,4]
输出：1
解释：对于每个符合范围 1 <= i <= 2 的下标 i :
```

- nums[1] 的美丽值等于 1
- nums[2] 的美丽值等于 0

示例 3：

```text
输入：nums = [3,2,1]
输出：0
解释：对于每个符合范围 1 <= i <= 1 的下标 i :
```

- nums[1] 的美丽值等于 0

__提示：__

- `3 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__思路:__

```text
前缀和
记录前缀和为 i 以前最大的数, 记录后缀和为 i 以后最小的数
从前往后遍历所以前缀和可以一边遍历一边更新
如果 pre < nums[i] < suf[i + 1] 则美丽值为 2
如果 nums[i - 1] < nums[i] < nums[i + 1] 则美丽值为 1

时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution
 {
public:
    int sumOfBeauties(vector<int>& nums) 
    {
        int n = nums.size(), pre = nums.front(), result = 0;
        vector<int> suf(n);
        suf.back() = nums.back();
        for (int i = n - 2; i > 0; i--) suf[i] = min(suf[i + 1], nums[i]);
        for (int i = 1; i < n - 1; i++)
         {
            result += nums[i] > pre and nums[i] < suf[i + 1] ? 2 : (nums[i - 1] < nums[i] and nums[i] < nums[i + 1] ? 1 : 0);
            pre = max(pre, nums[i]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int sumOfBeauties(int[] nums) {
        int n = nums.length, pre = nums[0], suf[] = new int[n], result = 0;
        suf[n - 1] = nums[n - 1];
        for (int i = n - 2; i > 0; i--) suf[i] = Math.min(suf[i + 1], nums[i]);
        for (int i = 1; i < n - 1; i++) {
            result += nums[i] > pre && nums[i] < suf[i + 1] ? 2 : (nums[i - 1] < nums[i] && nums[i] < nums[i + 1] ? 1 : 0);
            pre = Math.max(pre, nums[i]);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def sumOfBeauties(self, nums: List[int]) -> int:
        pre, suf, result = nums[0], [0] * ((n := len(nums)) - 1) + [nums[-1]], 0
        for i in range(n - 2, 1, -1):
            suf[i] = min(suf[i + 1], nums[i])
        for i in range(1, n - 1):
            result += 2 if pre < nums[i] < suf[i + 1] else 1 if nums[i - 1] < nums[i] < nums[i + 1] else 0
            pre = max(nums[i], pre)
        return result
```
