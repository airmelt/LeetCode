# 1829 Maximum XOR for Each Query 每个查询的最大异或值

__Description:__

You are given a __sorted__ array `nums` of `n` non-negative integers and an integer `maximumBit`. You want to perform the following query `n` __times__:

Return _an array_ `answer`_, where_ `answer[i]` _is the answer to the_ `i ^ th` _query_.

__Example:__

Example 1:

```text
Input: nums = [0,1,1,3], maximumBit = 2
Output: [0,3,2,3]
Explanation: The queries are answered as follows:
1st query: nums = [0,1,1,3], k = 0 since 0 XOR 1 XOR 1 XOR 3 XOR 0 = 3.
2nd query: nums = [0,1,1], k = 3 since 0 XOR 1 XOR 1 XOR 3 = 3.
3rd query: nums = [0,1], k = 2 since 0 XOR 1 XOR 2 = 3.
4th query: nums = [0], k = 3 since 0 XOR 3 = 3.
```

Example 2:

```text
Input: nums = [2,3,4,7], maximumBit = 3
Output: [5,2,6,5]
Explanation: The queries are answered as follows:
1st query: nums = [2,3,4,7], k = 5 since 2 XOR 3 XOR 4 XOR 7 XOR 5 = 7.
2nd query: nums = [2,3,4], k = 2 since 2 XOR 3 XOR 4 XOR 2 = 7.
3rd query: nums = [2,3], k = 6 since 2 XOR 3 XOR 6 = 7.
4th query: nums = [2], k = 5 since 2 XOR 5 = 7.
```

Example 3:

```text
Input: nums = [0,1,2,2,5,7], maximumBit = 3
Output: [4,3,6,4,6,7]
```

__Constraints:__

- `nums.length == n`
- `1 <= n <= 10 ^ 5`
- `1 <= maximumBit <= 20`
- `0 <= nums[i] < 2 ^ maximumBit`
- `nums`​​​ is sorted in __ascending__ order.

__题目描述:__

给你一个 __有序__ 数组 `nums` ，它由 `n` 个非负整数组成，同时给你一个整数 `maximumBit` 。你需要执行以下查询 `n` 次:

请你返回一个数组 `answer` ，其中 `answer[i]`是第 `i` 个查询的结果。

__示例:__

示例 1：

```text
输入：nums = [0,1,1,3], maximumBit = 2
输出：[0,3,2,3]
解释：查询的答案如下：
第一个查询：nums = [0,1,1,3]，k = 0，因为 0 XOR 1 XOR 1 XOR 3 XOR 0 = 3 。
第二个查询：nums = [0,1,1]，k = 3，因为 0 XOR 1 XOR 1 XOR 3 = 3 。
第三个查询：nums = [0,1]，k = 2，因为 0 XOR 1 XOR 2 = 3 。
第四个查询：nums = [0]，k = 3，因为 0 XOR 3 = 3 。
```

示例 2：

```text
输入：nums = [2,3,4,7], maximumBit = 3
输出：[5,2,6,5]
解释：查询的答案如下：
第一个查询：nums = [2,3,4,7]，k = 5，因为 2 XOR 3 XOR 4 XOR 7 XOR 5 = 7。
第二个查询：nums = [2,3,4]，k = 2，因为 2 XOR 3 XOR 4 XOR 2 = 7 。
第三个查询：nums = [2,3]，k = 6，因为 2 XOR 3 XOR 6 = 7 。
第四个查询：nums = [2]，k = 5，因为 2 XOR 5 = 7 。
```

示例 3：

```text
输入：nums = [0,1,2,2,5,7], maximumBit = 3
输出：[4,3,6,4,6,7]
```

__提示：__

- `nums.length == n`
- `1 <= n <= 10 ^ 5`
- `1 <= maximumBit <= 20`
- `0 <= nums[i] < 2 ^ maximumBit`
- `nums`​​​ 中的数字已经按 __升序__ 排好序。

__思路:__

```text
贪心
按照前缀和的方式计算出 nums 数组中的每个元素的异或和
设 mask 为 2 ^ maximumBit - 1, 即为可能的最大异或值
对前缀和中的每个元素异或 mask, 即可得到每个查询的答案
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> getMaximumXor(vector<int>& nums, int maximumBit) 
    {
        int n = nums.size(), mask = (1 << maximumBit) - 1;
        vector<int> result(n, nums.front());
        for (int i = 1; i < n; i++) result[i] = result[i - 1] ^ nums[i];
        for (int i = 0; i < n; i++) result[i] ^= mask;
        reverse(result.begin(), result.end());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] getMaximumXor(int[] nums, int maximumBit) {
        int n = nums.length, mask = (1 << maximumBit) - 1, result[] = new int[n];
        result[0] = nums[0];
        for (int i = 1; i < n; i++) result[i] = result[i - 1] ^ nums[i];
        for (int i = 0; i < n; i++) result[i] ^= mask;
        for (int i = 0; i < (n >> 1); i++) {
            int t = result[i];
            result[i] = result[n - i - 1];
            result[n - i - 1] = t;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def getMaximumXor(self, nums: List[int], maximumBit: int) -> List[int]:
        result, mask = [nums[0]] * (n := len(nums)), (1 << maximumBit) - 1
        for i in range(1, n):
            result[i], result[i - 1] = result[i - 1] ^ nums[i], result[i - 1] ^ mask
        result[-1] ^= mask
        return result[::-1]
```
