# 1558 Minimum Numbers of Function Calls to Make Target Array 得到目标数组的最少函数调用次数

__Description:__

You are given an integer array `nums`. You have an integer array `arr` of the same length with all values set to `0` initially. You also have the following `modify` function:

![1558-1](https://assets.leetcode.com/uploads/2020/07/10/sample_2_1887.png)

You want to use the modify function to covert `arr` to `nums` using the minimum number of calls.

Return _the minimum number of function calls to make_ `nums` _from_ `arr`.

The test cases are generated so that the answer fits in a 32-bit signed integer.

__Example:__

Example 1:

```text
Input: nums = [1,5]
Output: 5
Explanation: Increment by 1 (second element): [0, 0] to get [0, 1] (1 operation).
Double all the elements: [0, 1] -> [0, 2] -> [0, 4] (2 operations).
Increment by 1 (both elements)  [0, 4] -> [1, 4] -> [1, 5] (2 operations).
Total of operations: 1 + 2 + 2 = 5.
```

Example 2:

```text
Input: nums = [2,2]
Output: 3
Explanation: Increment by 1 (both elements) [0, 0] -> [0, 1] -> [1, 1] (2 operations).
Double all the elements: [1, 1] -> [2, 2] (1 operation).
Total of operations: 2 + 1 = 3.
```

Example 3:

```text
Input: nums = [4,2,5]
Output: 6
Explanation: (initial)[0,0,0] -> [1,0,0] -> [1,0,1] -> [2,0,2] -> [2,1,2] -> [4,2,4] -> [4,2,5](nums).
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`

__题目描述:__

![1558-2](https://assets.leetcode.com/uploads/2020/07/10/sample_2_1887.png)

给你一个与 `nums` 大小相同且初始值全为 0 的数组 `arr` ，请你调用以上函数得到整数数组 `nums` 。

请你返回将 `arr` 变成 `nums` 的最少函数调用次数。

答案保证在 32 位有符号整数以内。

__示例:__

示例 1：

```text
输入：nums = [1,5]
输出：5
解释：给第二个数加 1 ：[0, 0] 变成 [0, 1] （1 次操作）。
将所有数字乘以 2 ：[0, 1] -> [0, 2] -> [0, 4] （2 次操作）。
给两个数字都加 1 ：[0, 4] -> [1, 4] -> [1, 5] （2 次操作）。
总操作次数为：1 + 2 + 2 = 5 。
```

示例 2：

```text
输入：nums = [2,2]
输出：3
解释：给两个数字都加 1 ：[0, 0] -> [0, 1] -> [1, 1] （2 次操作）。
将所有数字乘以 2 ： [1, 1] -> [2, 2] （1 次操作）。
总操作次数为： 2 + 1 = 3 。
```

示例 3：

```text
输入：nums = [4,2,5]
输出：6
解释：（初始）[0,0,0] -> [1,0,0] -> [1,0,1] -> [2,0,2] -> [2,1,2] -> [4,2,4] -> [4,2,5] （nums 数组）。
```

示例 4：

```text
输入：nums = [3,2,2,4]
输出：7
```

示例 5：

```text
输入：nums = [2,4,8,16]
输出：8
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`

__思路:__

```text
数学
由于乘 2 这个操作是对数组中所有数字全部进行一次的
所以可以先求出所有数字的二进制的 1 的个数
然后为了得到数组中最大的数, 需要再加上数字中最大数的二进制长度
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(vector<int>& nums) 
    {
        int result = 0, max_value = *max_element(nums.begin(), nums.end());
        for (const auto& num: nums) result += __builtin_popcount(num);
        return result + (max_value ? 31 - __builtin_clz(max_value) : 0);
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(int[] nums) {
        return Arrays.stream(nums).reduce(0, (a, b) -> a + Integer.bitCount(b)) + Integer.toBinaryString(Arrays.stream(nums).max().getAsInt()).length() - 1;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        return sum(num.bit_count() for num in nums) + max(max(nums).bit_length() - 1, 0)
```
