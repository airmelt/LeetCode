# 2584 Split the Array to Make Coprime Products 分割数组使乘积互质

__Description:__

You are given a __0-indexed__ integer array `nums` of length `n`.

A __split__ at an index `i` where `0 <= i <= n - 2` is called __valid__ if the product of the first `i + 1` elements and the product of the remaining elements are coprime.

- For example, if `nums = [2, 3, 3]`, then a split at the index `i = 0` is valid because `2` and `9` are coprime, while a split at the index `i = 1` is not valid because `6` and `3` are not coprime. A split at the index `i = 2` is not valid because `i == n - 1`.

Return _the smallest index_ `i` _at which the array can be split validly or_ `-1` _if there is no such split_.

Two values `val1` and `val2` are coprime if `gcd(val1, val2) == 1` where `gcd(val1, val2)` is the greatest common divisor of `val1` and `val2`.

__Example:__

Example 1:

![2584-1](https://assets.leetcode.com/uploads/2022/12/14/second.PNG)

```text
Input: nums = [4,7,8,15,3,5]
Output: 2
Explanation: The table above shows the values of the product of the first i + 1 elements, the remaining elements, and their gcd at each index i.
The only valid split is at index 2.
```

Example 2:

![2584-2](https://assets.leetcode.com/uploads/2022/12/14/capture.PNG)

```text
Input: nums = [4,7,15,8,3,5]
Output: -1
Explanation: The table above shows the values of the product of the first i + 1 elements, the remaining elements, and their gcd at each index i.
There is no valid split.
```

__Constraints:__

- `n == nums.length`
- `1 <= n <= 10 ^ 4`
- `1 <= nums[i] <= 10 ^ 6`

__题目描述:__

给你一个长度为 `n` 的整数数组 `nums` ，下标从 __0__ 开始。

如果在下标 `i` 处 __分割__ 数组，其中 `0 <= i <= n - 2` ，使前 `i + 1` 个元素的乘积和剩余元素的乘积互质，则认为该分割 __有效__ 。

- 例如，如果 `nums = [2, 3, 3]` ，那么在下标 `i = 0` 处的分割有效，因为 `2` 和 `9` 互质，而在下标 `i = 1` 处的分割无效，因为 `6` 和 `3` 不互质。在下标 `i = 2` 处的分割也无效，因为 `i == n - 1` 。

返回可以有效分割数组的最小下标 `i` ，如果不存在有效分割，则返回 `-1` 。

当且仅当 `gcd(val1, val2) == 1` 成立时， `val1` 和 `val2` 这两个值才是互质的，其中 `gcd(val1, val2)` 表示 `val1` 和 `val2` 的最大公约数。

__示例:__

示例 1：

![2584-3](https://assets.leetcode.com/uploads/2022/12/14/second.PNG)

```text
输入：nums = [4,7,8,15,3,5]
输出：2
解释：上表展示了每个下标 i 处的前 i + 1 个元素的乘积、剩余元素的乘积和它们的最大公约数的值。
唯一一个有效分割位于下标 2 。
```

示例 2：

![2584-4](https://assets.leetcode.com/uploads/2022/12/14/capture.PNG)

```text
输入：nums = [4,7,15,8,3,5]
输出：-1
解释：上表展示了每个下标 i 处的前 i + 1 个元素的乘积、剩余元素的乘积和它们的最大公约数的值。
不存在有效分割。
```

__提示：__

- `n == nums.length`
- `1 <= n <= 10 ^ 4`
- `1 <= nums[i] <= 10 ^ 6`

__思路:__

```text
质因数分解
记录每个质因子出现的位置
遍历每个数的质因子
如果该质因子已经出现过, 则更新该质因子最后出现的位置
如果该质因子没有出现过, 则记录该质因子出现的位置
遍历每个数, 如果当前下标大于最大位置, 则返回最大位置
否则更新最大位置
如果遍历完所有数都没有返回, 则返回 -1
时间复杂度为 O(M ^ 1 / 2 * N), 空间复杂度为 O(N), M 为 nums 中的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findValidSplit(vector<int>& nums) 
    {
        unordered_map<int, int> left;
        int n = nums.size(), m = 0;
        vector<int> right(n);
        auto helper = [&](int p, int i) -> void
        {
            if (left.count(p)) right[left[p]] = i;
            else left[p] = i;
        };
        for (int i = 0; i < n; i++) 
        {
            for (int d = 2; d * d <= nums[i]; d++) 
            {
                if (!(nums[i] % d)) 
                {
                    helper(d, i);
                    nums[i] /= d;
                    while (!(nums[i] % d))  nums[i] /= d;
                }
            }
            if (nums[i] > 1) helper(nums[i], i);
        }
        for (int i = 0; i < n; i++) 
        {
            if (i > m) return m;
            m = max(m, right[i]);
        }
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int findValidSplit(int[] nums) {
        var left = new HashMap<Integer, Integer>();
        int n = nums.length, m = 0, right[] = new int[n];
        for (int i = 0; i < n; i++) {
            for (int d = 2; d * d <= nums[i]; d++) {
                if (nums[i] % d == 0) {
                    helper(d, i, left, right);
                    nums[i] /= d;
                    while (nums[i] % d == 0) nums[i] /= d;
                }
            }
            if (nums[i] > 1) helper(nums[i], i, left, right);
        }
        for (int i = 0; i < n; i++) {
            if (i > m) return m;
            m = Math.max(m, right[i]);
        }
        return -1;
    }

    private void helper(int p, int i, Map<Integer, Integer> left, int[] right) {
        if (left.containsKey(p)) right[left.get(p)] = i;
        else left.put(p, i);
    }
}
```

__Python__:

```Python
class Solution:
    def findValidSplit(self, nums: List[int]) -> int:
        left, right, m = {}, [0] * (n := len(nums)), 0

        def helper(p: int, i: int) -> NoReturn:
            if p in left:
                right[left[p]] = i
            else:
                left[p] = i
        for i, num in enumerate(nums):
            d = 2
            while d * d <= num:
                if not num % d:
                    helper(d, i)
                    num //= d
                    while not num % d:
                        num //= d
                d += 1
            num > 1 and helper(num, i)
        for i, r in enumerate(right):
            if i > m:
                return m
            m = max(m, r)
        return -1
```
