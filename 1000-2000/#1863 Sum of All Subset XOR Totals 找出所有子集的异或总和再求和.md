# 1863 Sum of All Subset XOR Totals 找出所有子集的异或总和再求和

__Description:__

The __XOR total__ of an array is defined as the bitwise `XOR` of __all its elements__, or `0` if the array is __empty__.

- For example, the __XOR total__ of the array `[2,5,6]` is `2 XOR 5 XOR 6 = 1`.

Given an array `nums`, return _the __sum__ of all __XOR totals__ for every __subset__ of_ `nums`.

Note: Subsets with the same elements should be counted multiple times.

An array `a` is a __subset__ of an array `b` if `a` can be obtained from `b` by deleting some (possibly zero) elements of `b`.

__Example:__

Example 1:

```text
Input: nums = [1,3]
Output: 6
Explanation: The 4 subsets of [1,3] are:
- The empty subset has an XOR total of 0.
- [1] has an XOR total of 1.
- [3] has an XOR total of 3.
- [1,3] has an XOR total of 1 XOR 3 = 2.
0 + 1 + 3 + 2 = 6
```

Example 2:

```text
Input: nums = [5,1,6]
Output: 28
Explanation: The 8 subsets of [5,1,6] are:
- The empty subset has an XOR total of 0.
- [5] has an XOR total of 5.
- [1] has an XOR total of 1.
- [6] has an XOR total of 6.
- [5,1] has an XOR total of 5 XOR 1 = 4.
- [5,6] has an XOR total of 5 XOR 6 = 3.
- [1,6] has an XOR total of 1 XOR 6 = 7.
- [5,1,6] has an XOR total of 5 XOR 1 XOR 6 = 2.
0 + 5 + 1 + 6 + 4 + 3 + 7 + 2 = 28
```

Example 3:

```text
Input: nums = [3,4,5,6,7,8]
Output: 480
Explanation: The sum of all XOR totals for every subset is 480.
```

__Constraints:__

- `1 <= nums.length <= 12`
- `1 <= nums[i] <= 20`

__题目描述:__

一个数组的 __异或总和__ 定义为数组中所有元素按位 `XOR` 的结果；如果数组为 __空__ ，则异或总和为 `0` 。

- 例如，数组 `[2,5,6]` 的 __异或总和__ 为 `2 XOR 5 XOR 6 = 1` 。

给你一个数组 `nums` ，请你求出 `nums` 中每个 __子集__ 的 __异或总和__ ，计算并返回这些值相加之 __和__ 。

注意：在本题中，元素 相同 的不同子集应 多次 计数。

数组 `a` 是数组 `b` 的一个 __子集__ 的前提条件是:从 `b` 删除几个（也可能不删除）元素能够得到 `a` 。

__示例:__

示例 1：

```text
输入：nums = [1,3]
输出：6
解释：[1,3] 共有 4 个子集：
- 空子集的异或总和是 0 。
- [1] 的异或总和为 1 。
- [3] 的异或总和为 3 。
- [1,3] 的异或总和为 1 XOR 3 = 2 。
0 + 1 + 3 + 2 = 6
```

示例 2：

```text
输入：nums = [5,1,6]
输出：28
解释：[5,1,6] 共有 8 个子集：
- 空子集的异或总和是 0 。
- [5] 的异或总和为 5 。
- [1] 的异或总和为 1 。
- [6] 的异或总和为 6 。
- [5,1] 的异或总和为 5 XOR 1 = 4 。
- [5,6] 的异或总和为 5 XOR 6 = 3 。
- [1,6] 的异或总和为 1 XOR 6 = 7 。
- [5,1,6] 的异或总和为 5 XOR 1 XOR 6 = 2 。
0 + 5 + 1 + 6 + 4 + 3 + 7 + 2 = 28
```

示例 3：

```text
输入：nums = [3,4,5,6,7,8]
输出：480
解释：每个子集的全部异或总和值之和为 480 。
```

__提示：__

- `1 <= nums.length <= 12`
- `1 <= nums[i] <= 20`

__思路:__

```text
1. 暴力法
求出子集
再求所有子集的异或和
时间复杂度为 O(2 ^ N), 空间复杂度为 O(N * 2 ^ N)
2. 位运算
所有子集一共有 2 ^ n 种
对于每个元素都有可选和可不选两种情况
所以, 根据对称性, 每个元素在最终结果中一定出现 2 ^ (n - 1) 次
对二进制的任意一位, 假设该位有 m 个 1
根据二项式定理共有 C(n, m) 个 1, 剩下的 C(n, n - m) 为 0
那么子数组中有 k 个 1 的和为 2 ^ (n - m) * C(m - k)
包含奇数个 1 和偶数个 1 的子集和相等均为 2 ^ (n - 1)
所以只需要求出数组的或运算的结果在左移 n - 1 位
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int subsetXORSum(vector<int>& nums) 
    {
        return reduce(nums.begin(), nums.end(), 0, [](int a, int b) { return a | b; }) << (nums.size() - 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int subsetXORSum(int[] nums) {
        return Arrays.stream(nums).reduce((a, b) -> a | b).getAsInt() << (nums.length - 1);
    }
}
```

__Python__:

```Python
class Solution:
    def subsetXORSum(self, nums: List[int]) -> int:
        return reduce(or_, nums) << (len(nums) - 1)
```
