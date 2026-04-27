# 2317 Maximum XOR After Operations 操作后的最大异或和

__Description:__

You are given a __0-indexed__ integer array `nums`. In one operation, select __any__ non-negative integer `x` and an index `i`, then __update__ `nums[i]` to be equal to `nums[i] AND (nums[i] XOR x)`.

Note that `AND` is the bitwise AND operation and `XOR` is the bitwise XOR operation.

Return _the __maximum__ possible bitwise XOR of all elements of_ `nums` _after applying the operation __any number__ of times_.

__Example:__

Example 1:

```text
Input: nums = [3,2,4,6]
Output: 7
Explanation: Apply the operation with x = 4 and i = 3, num[3] = 6 AND (6 XOR 4) = 6 AND 2 = 2.
Now, nums = [3, 2, 4, 2] and the bitwise XOR of all the elements = 3 XOR 2 XOR 4 XOR 2 = 7.
It can be shown that 7 is the maximum possible bitwise XOR.
Note that other operations may be used to achieve a bitwise XOR of 7.
```

Example 2:

```text
Input: nums = [1,2,3,9,2]
Output: 11
Explanation: Apply the operation zero times.
The bitwise XOR of all the elements = 1 XOR 2 XOR 3 XOR 9 XOR 2 = 11.
It can be shown that 11 is the maximum possible bitwise XOR.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 8`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。一次操作中，选择 __任意__ 非负整数 `x` 和一个下标 `i` ，__更新__ `nums[i]` 为 `nums[i] AND (nums[i] XOR x)` 。

注意， `AND` 是逐位与运算， `XOR` 是逐位异或运算。

请你执行 __任意次__ 更新操作，并返回 `nums` 中所有元素 __最大__ 逐位异或和。

__示例:__

示例 1：

```text
输入：nums = [3,2,4,6]
输出：7
解释：选择 x = 4 和 i = 3 进行操作，num[3] = 6 AND (6 XOR 4) = 6 AND 2 = 2 。
现在，nums = [3, 2, 4, 2] 且所有元素逐位异或得到 3 XOR 2 XOR 4 XOR 2 = 7 。
可知 7 是能得到的最大逐位异或和。
注意，其他操作可能也能得到逐位异或和 7 。
```

示例 2：

```text
输入：nums = [1,2,3,9,2]
输出：11
解释：执行 0 次操作。
所有元素的逐位异或和为 1 XOR 2 XOR 3 XOR 9 XOR 2 = 11 。
可知 11 是能得到的最大逐位异或和。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 8`

__思路:__

```text
位运算
首先尝试计算 nums 中所有元素的异或, 或, 和与运算
发现结果就是 nums 中所有元素的或运算
由于 x 可以取任意非负整数, 所以不需要考虑 x 的值
而与运算最多只能保留每个位置的 1 个 1
所以取所有数的或运算即可, 这样就能留下所有数的每一个位的 1
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumXOR(vector<int>& nums) 
    {
        return accumulate(nums.begin(), nums.end(), 0, [](auto a, auto b){ return a | b; });
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumXOR(int[] nums) {
        return Arrays.stream(nums).reduce(0, (x, y) -> x | y);
    }
}
```

__Python__:

```Python
class Solution:
    def maximumXOR(self, nums: List[int]) -> int:
        return reduce(or_, nums)
```
