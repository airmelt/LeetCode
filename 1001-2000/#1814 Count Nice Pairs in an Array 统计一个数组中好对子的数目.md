# 1814 Count Nice Pairs in an Array 统计一个数组中好对子的数目

__Description:__

You are given an array `nums` that consists of non-negative integers. Let us define `rev(x)` as the reverse of the non-negative integer `x`. For example, `rev(123) = 321`, and `rev(120) = 21`. A pair of indices `(i, j)` is __nice__ if it satisfies all of the following conditions:

- `0 <= i < j < nums.length`
- `nums[i] + rev(nums[j]) == nums[j] + rev(nums[i])`

Return _the number of nice pairs of indices_. Since that number can be too large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: nums = [42,11,1,97]
Output: 2
Explanation: The two pairs are:
 - (0,3) : 42 + rev(97) = 42 + 79 = 121, 97 + rev(42) = 97 + 24 = 121.
 - (1,2) : 11 + rev(1) = 11 + 1 = 12, 1 + rev(11) = 1 + 11 = 12.
```

Example 2:

```text
Input: nums = [13,10,35,24,76]
Output: 4
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个数组 `nums` ，数组中只包含非负整数。定义 `rev(x)` 的值为将整数 `x` 各个数字位反转得到的结果。比方说 `rev(123) = 321` ， `rev(120) = 21` 。我们称满足下面条件的下标对 `(i, j)` 是 __好的__ :

- `0 <= i < j < nums.length`
- `nums[i] + rev(nums[j]) == nums[j] + rev(nums[i])`

请你返回好下标对的数目。由于结果可能会很大，请将结果对 `10 ^ 9 + 7` _取余_ 后返回。

__示例:__

示例 1：

```text
输入：nums = [42,11,1,97]
输出：2
解释：两个坐标对为：
 - (0,3)：42 + rev(97) = 42 + 79 = 121, 97 + rev(42) = 97 + 24 = 121 。
 - (1,2)：11 + rev(1) = 11 + 1 = 12, 1 + rev(11) = 1 + 11 = 12 。
```

示例 2：

```text
输入：nums = [13,10,35,24,76]
输出：4
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`

__思路:__

```text
哈希表
转化为 nums[i] - rev(nums[i]) 求对数
可以用哈希表记录 nums[i] - rev(nums[i]) 出现的次数并加到结果中
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countNicePairs(vector<int>& nums) 
    {
        int result = 0, MOD = 1e9 + 7, n = nums.size();
        for (int i = 0; i < n; i++) 
        {
            int num = nums[i], rev = 0;
            while (num) 
            {
                rev = rev * 10 + num % 10;
                num /= 10;
            }
            nums[i] -= rev;
        }
        unordered_map<int, int> m;
        for (const auto& num : nums) result = (result + m[num]++) % MOD;
        return result % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    public int countNicePairs(int[] nums) {
        int result = 0, MOD = 1_000_000_007, n = nums.length;
        for (int i = 0; i < n; i++) {
            int num = nums[i], rev = 0;
            while (num > 0) {
                rev = rev * 10 + num % 10;
                num /= 10;
            }
            nums[i] -= rev;
        }
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int num : nums) {
            result = (result + map.getOrDefault(num, 0)) % MOD;
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        return result % MOD;
    }
}
```

__Python__:

```Python
class Solution:
    def countNicePairs(self, nums: List[int]) -> int:
        return sum([(v * (v - 1) >> 1) for k, v in c.items()]) % (10 ** 9 + 7) if (c := Counter(list(map(lambda x: x - int(str(x)[::-1]), nums)))) else 0
```
