# 3192 Minimum Operations to Make Binary Array Elements Equal to One II 使二进制数组全部等于 1 的最少操作次数 II

__Description:__

You are given a __binary array__ `nums`.

You can do the following operation on the array __any__ number of times (possibly zero):

- Choose __any__ index `i` from the array and __flip all__ the elements from index `i` to the end of the array.

__Flipping__ an element means changing its value from 0 to 1, and from 1 to 0.

Return the __minimum__ number of operations required to make all elements in `nums` equal to 1.

__Example:__

Example 1:

```text
Input: nums = [0,1,1,0,1]

Output: 4

Explanation:
We can do the following operations:

Choose the index i = 1. The resulting array will be nums = [0,0,0,1,0].
Choose the index i = 0. The resulting array will be nums = [1,1,1,0,1].
Choose the index i = 4. The resulting array will be nums = [1,1,1,0,0].
Choose the index i = 3. The resulting array will be nums = [1,1,1,1,1].
```

Example 2:

```text
Input: nums = [1,0,0,0]

Output: 1

Explanation:
We can do the following operation:

Choose the index i = 1. The resulting array will be nums = [1,1,1,1].
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 1`

__题目描述:__

给你一个二进制数组 `nums` 。

你可以对数组执行以下操作 __任意__ 次（也可以 0 次）：

- 选择数组中 任意 一个下标 i ，并将从下标 i 开始一直到数组末尾 所有 元素 反转 。
反转 一个元素指的是将它的值从 0 变 1 ，或者从 1 变 0 。

__反转__ 一个元素指的是将它的值从 0 变 1 ，或者从 1 变 0 。

请你返回将 `nums` 中所有元素变为 1 的 __最少__ 操作次数。

__示例:__

示例 1：

```text
输入：nums = [0,1,1,0,1]

输出：4

解释：
我们可以执行以下操作：

选择下标 i = 1 执行操作，得到 nums = [0,0,0,1,0] 。
选择下标 i = 0 执行操作，得到 nums = [1,1,1,0,1] 。
选择下标 i = 4 执行操作，得到 nums = [1,1,1,0,0] 。
选择下标 i = 3 执行操作，得到 nums = [1,1,1,1,1] 。
```

示例 2：

```text
输入：nums = [1,0,0,0]

输出：1

解释：
我们可以执行以下操作：

选择下标 i = 1 执行操作，得到 nums = [1,1,1,1] 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 1`

__思路:__

```text
1. 模拟
从左到右依次操作不为 0 的元素
如果第 i 位和操作次数的奇偶性质相同
操作次数自增
时间复杂度为 O(N), 空间复杂度为 O(1)
2. 贪心
如果第一位不为 1
需要反转一次
以后每次相邻元素不相等就需要额外一次反转
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
        int result = nums.front() ^ 1;
        for (int i = 1, n = nums.size(); i < n; i++) result += nums[i - 1] ^ nums[i];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(int[] nums) {
        return (nums[0] ^ 1) + IntStream.range(1, nums.length).map(i -> nums[i] ^ nums[i - 1]).sum();
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        return (nums[0] ^ 1) + sum(x ^ y for x, y in pairwise(nums))
```
