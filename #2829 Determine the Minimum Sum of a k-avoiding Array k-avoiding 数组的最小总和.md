# 2829 Determine the Minimum Sum of a k-avoiding Array k-avoiding 数组的最小总和

__Description:__

You are given two integers, `n` and `k`.

An array of __distinct__ positive integers is called a _k-avoiding_ array if there does not exist any pair of distinct elements that sum to `k`.

Return _the __minimum__ possible sum of a k-avoiding array of length_ `n`.

__Example:__

Example 1:

```text
Input: n = 5, k = 4
Output: 18
Explanation: Consider the k-avoiding array [1,2,4,5,6], which has a sum of 18.
It can be proven that there is no k-avoiding array with a sum less than 18.
```

Example 2:

```text
Input: n = 2, k = 6
Output: 3
Explanation: We can construct the array [1,2], which has a sum of 3.
It can be proven that there is no k-avoiding array with a sum less than 3.
```

__Constraints:__

- `1 <= n, k <= 50`

__题目描述:__

给你两个整数 `n` 和 `k` 。

对于一个由 不同 正整数组成的数组，如果其中不存在任何求和等于 k 的不同元素对，则称其为 k-avoiding 数组。

返回长度为 `n` 的 __k-avoiding__ 数组的可能的最小总和。

__示例:__

示例 1：

```text
输入：n = 5, k = 4
输出：18
解释：设若 k-avoiding 数组为 [1,2,4,5,6] ，其元素总和为 18 。
可以证明不存在总和小于 18 的 k-avoiding 数组。
```

示例 2：

```text
输入：n = 2, k = 6
输出：3
解释：可以构造数组 [1,2] ，其元素总和为 3 。
可以证明不存在总和小于 3 的 k-avoiding 数组。
```

__提示：__

- `1 <= n, k <= 50`

__思路:__

```text
数学
由于要选最小
所以小于 k 的只能从 1 选到 k >> 1
取 m = min(k >> 1, n)
如果没取够
剩下 n - m 个必须在比 k 大的里面选
由等差数列求和公式可得
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumSum(int n, int k) 
    {
        return (min(k >> 1, n) * (min(k >> 1, n) + 1) + (((k << 1) + n - min(k >> 1, n) - 1) * (n - min(k >> 1, n))) >> 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumSum(int n, int k) {
        return (Math.min(k >>> 1, n) * (Math.min(k >>> 1, n) + 1) + (((k << 1) + n - Math.min(k >>> 1, n) - 1) * (n - Math.min(k >>> 1, n))) >>> 1);
    }
}
```

__Python__:

```Python
class Solution:
    def minimumSum(self, n: int, k: int) -> int:
        return ((m := min(k >> 1, n)) * (m + 1) + (((k << 1) + n - m - 1) * (n - m)) >> 1)
```
