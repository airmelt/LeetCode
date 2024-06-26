# 2154 Keep Multiplying Found Values by Two 将找到的值乘以 2

__Description:__

You are given an array of integers `nums`. You are also given an integer `original` which is the first number that needs to be searched for in `nums`.

You then do the following steps:

Return _the __final__ value of_ `original`.

__Example:__

Example 1:

```text
Input: nums = [5,3,6,1,12], original = 3
Output: 24
Explanation: 
```

- 3 is found in nums. 3 is multiplied by 2 to obtain 6.
- 6 is found in nums. 6 is multiplied by 2 to obtain 12.
- 12 is found in nums. 12 is multiplied by 2 to obtain 24.
- 24 is not found in nums. Thus, 24 is returned.

Example 2:

```text
Input: nums = [2,7,9], original = 4
Output: 4
Explanation:
```

- 4 is not found in nums. Thus, 4 is returned.

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i], original <= 1000`

__题目描述:__

给你一个整数数组 `nums` ，另给你一个整数 `original` ，这是需要在 `nums` 中搜索的第一个数字。

接下来，你需要按下述步骤操作：

返回 `original` 的 __最终__ 值。

__示例:__

示例 1：

```text
输入：nums = [5,3,6,1,12], original = 3
输出：24
解释： 
```

- 3 能在 nums 中找到。3 * 2 = 6 。
- 6 能在 nums 中找到。6 * 2 = 12 。
- 12 能在 nums 中找到。12 * 2 = 24 。
- 24 不能在 nums 中找到。因此，返回 24 。

示例 2：

```text
输入：nums = [2,7,9], original = 4
输出：4
解释：
```

- 4 不能在 nums 中找到。因此，返回 4 。

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i], original <= 1000`

__思路:__

```text
位运算
如果找到一个数 num 使得 num % original == 0 
计算 num / original 是 2 的幂次方, 那么将 k = num / original 加入 mask 中
计算 2 的幂次方的方法为 (k & (k - 1)) == 0
对 mask 取反, 即 ~mask, 得到所有不是 2 的幂次方的数
比如 mask = 0b1010, 那么 ~mask = 0b0101, 取最低位的 1 即为 0b0001
又比如 mask = 0b0111, 那么 ~mask = 0b1000, 取最低位的 1 即为 0b1000
取 mask 的 lowbit 位, 即 mask & -mask
最终结果为 original * lowbit
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findFinalValue(vector<int> &nums, int original) 
    {
        int mask = 0, k = 0;
        for (const auto& num : nums) if (!(num % original)) if (!((k = num / original) & (k - 1))) mask |= k;
        mask = ~mask;
        return original * (mask & -mask);
    }
};
```

__Java__:

```Java
class Solution {
    public int findFinalValue(int[] nums, int original) {
        int mask = 0, k = 0;
        for (int num : nums) if (num % original == 0) if (((k = num / original) & (k - 1)) == 0) mask |= k;
        mask = ~mask;
        return original * (mask & -mask);
    }
}
```

__Python__:

```Python
class Solution:
    def findFinalValue(self, nums: List[int], original: int) -> int:
        return (original << 1) if (original in set(nums) and (original << 1) not in set(nums)) else original if original not in set(nums) else self.findFinalValue(nums, original << 1)
```
