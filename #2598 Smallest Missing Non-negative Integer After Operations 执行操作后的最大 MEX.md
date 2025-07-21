# 2598 Smallest Missing Non-negative Integer After Operations 执行操作后的最大 MEX

__Description:__

You are given a __0-indexed__ integer array `nums` and an integer `value`.

In one operation, you can add or subtract `value` from any element of `nums`.

- For example, if `nums = [1,2,3]` and `value = 2`, you can choose to subtract `value` from `nums[0]` to make `nums = [-1,2,3]`.

The MEX (minimum excluded) of an array is the smallest missing non-negative integer in it.

- For example, the MEX of `[-1,2,3]` is `0` while the MEX of `[1,0,3]` is `2`.

Return _the maximum MEX of_ `nums` _after applying the mentioned operation __any number of times___.

__Example:__

Example 1:

```text
Input: nums = [1,-10,7,13,6,8], value = 5
Output: 4
Explanation: One can achieve this result by applying the following operations:
```

- Add value to nums[1] twice to make nums = [1,0,7,13,6,8]
- Subtract value from nums[2] once to make nums = [1,0,2,13,6,8]
- Subtract value from nums[3] twice to make nums = [1,0,2,3,6,8]

The MEX of nums is 4. It can be shown that 4 is the maximum MEX we can achieve.

Example 2:

```text
Input: nums = [1,-10,7,13,6,8], value = 7
Output: 2
Explanation: One can achieve this result by applying the following operation:
```

- subtract value from nums[2] once to make nums = [1,-10,0,13,6,8]

The MEX of nums is 2. It can be shown that 2 is the maximum MEX we can achieve.

__Constraints:__

- `1 <= nums.length, value <= 10 ^ 5`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和一个整数 `value` 。

在一步操作中，你可以对 `nums` 中的任一元素加上或减去 `value` 。

- 例如，如果 `nums = [1,2,3]` 且 `value = 2` ，你可以选择 `nums[0]` 减去 `value` ，得到 `nums = [-1,2,3]` 。

数组的 MEX (minimum excluded) 是指其中数组中缺失的最小非负整数。

- 例如， `[-1,2,3]` 的 MEX 是 `0` ，而 `[1,0,3]` 的 MEX 是 `2` 。

返回在执行上述操作 __任意次__ 后， `nums` 的最大 MEX _。_

__示例:__

示例 1：

```text
输入：nums = [1,-10,7,13,6,8], value = 5
输出：4
解释：执行下述操作可以得到这一结果：
```

- nums[1] 加上 value 两次，nums = [1,0,7,13,6,8]
- nums[2] 减去 value 一次，nums = [1,0,2,13,6,8]
- nums[3] 减去 value 两次，nums = [1,0,2,3,6,8]

nums 的 MEX 是 4 。可以证明 4 是可以取到的最大 MEX 。

示例 2：

```text
输入：nums = [1,-10,7,13,6,8], value = 7
输出：2
解释：执行下述操作可以得到这一结果：
```

- nums[2] 减去 value 一次，nums = [1,-10,0,13,6,8]

nums 的 MEX 是 2 。可以证明 2 是可以取到的最大 MEX 。

__提示：__

- `1 <= nums.length, value <= 10 ^ 5`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`

__思路:__

```text
哈希表
按照同余分组
从 0 开始遍历
如果当前数的同余分组中有数, 则继续遍历, 并将当前计数减 1
如果当前数的同余分组中没有数, 则返回当前计数
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findSmallestInteger(vector<int>& nums, int value) 
    {
        unordered_map<int, int> m;
        for (const auto& num : nums) ++m[(num % value + value) % value];
        int result = 0;
        while (m[result % value]--) ++result;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findSmallestInteger(int[] nums, int value) {
        var map = new HashMap<Integer, Integer>();
        for (int num : nums) map.merge((num % value + value) % value, 1, Integer::sum);
        int result = 0;
        while (map.merge(result % value, -1, Integer::sum) >= 0) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findSmallestInteger(self, nums: List[int], value: int) -> int:
        c, result = Counter(num % value for num in nums), 0
        while c[result % value]:
            c[result % value] -= 1
            result += 1
        return result
```
