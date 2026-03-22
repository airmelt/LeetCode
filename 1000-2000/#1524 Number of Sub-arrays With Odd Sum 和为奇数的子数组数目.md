# 1524 Number of Sub-arrays With Odd Sum 和为奇数的子数组数目

__Description:__

Given an array of integers `arr`, return _the number of subarrays with an __odd__ sum_.

Since the answer can be very large, return it modulo `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: arr = [1,3,5]
Output: 4
Explanation: All subarrays are [[1],[1,3],[1,3,5],[3],[3,5],[5]]
All sub-arrays sum are [1,4,9,3,8,5].
Odd sums are [1,9,3,5] so the answer is 4.
```

Example 2:

```text
Input: arr = [2,4,6]
Output: 0
Explanation: All subarrays are [[2],[2,4],[2,4,6],[4],[4,6],[6]]
All sub-arrays sum are [2,6,12,4,10,6].
All sub-arrays have even sum and the answer is 0.
```

Example 3:

```text
Input: arr = [1,2,3,4,5,6,7]
Output: 16
```

__Constraints:__

- `1 <= arr.length <= 10 ^ 5`
- `1 <= arr[i] <= 100`

__题目描述:__

给你一个整数数组 `arr` 。请你返回和为 __奇数__ 的子数组数目。

由于答案可能会很大，请你将结果对 `10 ^ 9 + 7` 取余后返回。

__示例:__

示例 1：

```text
输入：arr = [1,3,5]
输出：4
解释：所有的子数组为 [[1],[1,3],[1,3,5],[3],[3,5],[5]] 。
所有子数组的和为 [1,4,9,3,8,5].
奇数和包括 [1,9,3,5] ，所以答案为 4 。
```

示例 2 ：

```text
输入：arr = [2,4,6]
输出：0
解释：所有子数组为 [[2],[2,4],[2,4,6],[4],[4,6],[6]] 。
所有子数组和为 [2,6,12,4,10,6] 。
所有子数组和都是偶数，所以答案为 0 。
```

示例 3：

```text
输入：arr = [1,2,3,4,5,6,7]
输出：16
```

示例 4：

```text
输入：arr = [100,100,99,99]
输出：4
```

示例 5：

```text
输入：arr = [7]
输出：1
```

__提示：__

- `1 <= arr.length <= 10 ^ 5`
- `1 <= arr[i] <= 100`

__思路:__

```text
前缀和
设 odd 表示前缀和为奇数的子数组的数目, even 表示前缀和为偶数的子数组的数目，sum 表示前缀和，初始化 even 为 1, 表示 0 个元素的前缀和为 0
由于我们只关心 sum 的奇偶, 可以使用异或运算得到前缀和的奇偶
对每个以 i 结尾的元素, 判断前缀和是奇数还是偶数, 如果是奇数, 则 odd 为 odd + 1, 如果是偶数, 则 even 为 even + 1
当前前缀和是奇数时, arr[i] 可以和前面所有偶数的子数组组合成子数组的和为奇数的子数组, result += even, 偶数类似
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numOfSubarrays(vector<int>& arr) 
    {
        int odd = 0, even = 1, sum = 0, result = 0, MOD = 1e9 + 7;
        for (const auto& num: arr) 
        {
            sum ^= num & 1;
            if (sum & 1) 
            {
                result = (result + even) % MOD;
                ++odd;
            } 
            else 
            {
                result = (result + odd) % MOD;
                ++even;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numOfSubarrays(int[] arr) {
        int odd = 0, even = 1, sum = 0, result = 0, MOD = 1_000_000_007;
        for (int num: arr) {
            sum ^= num & 1;
            if ((sum & 1) == 1) {
                result = (result + even) % MOD;
                ++odd;
            } else {
                result = (result + odd) % MOD;
                ++even;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numOfSubarrays(self, arr: List[int]) -> int:
        odd, even, s, result, mod = 0, 1, 0, 0, 10 ** 9 + 7
        for num in arr:
            s ^= num & 1
            if s & 1:
                odd += 1
                result += even
            else:
                even += 1
                result += odd
        return result % mod
```
