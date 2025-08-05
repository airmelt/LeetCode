# 2614 Prime In Diagonal 对角线上的质数

__Description:__

You are given a 0-indexed two-dimensional integer array `nums`.

Return _the largest __prime__ number that lies on at least one of the _diagonals_ of_ `nums`. In case, no prime is present on any of the diagonals, return _0._

Note that:

- An integer is __prime__ if it is greater than `1` and has no positive integer divisors other than `1` and itself.
- An integer `val` is on one of the __diagonals__ of `nums` if there exists an integer `i` for which `nums[i][i] = val` or an `i` for which `nums[i][nums.length - i - 1] = val`.

![2614-1](https://assets.leetcode.com/uploads/2023/03/06/screenshot-2023-03-06-at-45648-pm.png)

In the above diagram, one diagonal is [1,5,9] and another diagonal is [3,5,7].

__Example:__

Example 1:

```text
Input: nums = [[1,2,3],[5,6,7],[9,10,11]]
Output: 11
Explanation: The numbers 1, 3, 6, 9, and 11 are the only numbers present on at least one of the diagonals. Since 11 is the largest prime, we return 11.
```

Example 2:

```text
Input: nums = [[1,2,3],[5,17,7],[9,11,10]]
Output: 17
Explanation: The numbers 1, 3, 9, 10, and 17 are all present on at least one of the diagonals. 17 is the largest prime, so we return 17.
```

__Constraints:__

- `1 <= nums.length <= 300`
- `nums.length == nums[i].length`
- `1 <= nums[i][j] <= 4*10 ^ 6`

__题目描述:__

给你一个下标从 __0__ 开始的二维整数数组 `nums` 。

返回位于 `nums` 至少一条 __对角线__ 上的最大 __质数__ 。如果任一对角线上均不存在质数，返回 _0 。_

注意：

- 如果某个整数大于 `1` ，且不存在除 `1` 和自身之外的正整数因子，则认为该整数是一个质数。
- 如果存在整数 `i` ，使得 `nums[i][i] = val` 或者 `nums[i][nums.length - i - 1]= val` ，则认为整数 `val` 位于 `nums` 的一条对角线上。

![2614-2](https://assets.leetcode.com/uploads/2023/03/06/screenshot-2023-03-06-at-45648-pm.png)

在上图中，一条对角线是 [1,5,9] ，而另一条对角线是 [3,5,7] 。

__示例:__

示例 1：

```text
输入：nums = [[1,2,3],[5,6,7],[9,10,11]]
输出：11
解释：数字 1、3、6、9 和 11 是所有 "位于至少一条对角线上" 的数字。由于 11 是最大的质数，故返回 11 。
```

示例 2：

```text
输入：nums = [[1,2,3],[5,17,7],[9,11,10]]
输出：17
解释：数字 1、3、9、10 和 17 是所有满足"位于至少一条对角线上"的数字。由于 17 是最大的质数，故返回 17 。
```

__提示：__

- `1 <= nums.length <= 300`
- `nums.length == nums[i].length`
- `1 <= nums[i][j] <= 4*10 ^ 6`

__思路:__

```text
数学
对角线有两条
分别遍历 nums[i][i] 和 nums[i][n - i - 1], 判断是否为质数
只有大于当前结果的质数才会更新结果
筛选质数可以遍历到 sqrt(n) 的数
时间复杂度为 O(NM ^ 1 / 2), 空间复杂度为 O(N), M 为 nums[i][j] 的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int diagonalPrime(vector<vector<int>>& nums) 
    {
        int result = 0, n = nums.size();
        auto is_prime = [](int n) -> bool
        {
            for (int i = 2; i * i <= n; i++) if (!(n % i)) return false;
            return n >= 2;
        };
        for (int i = 0; i < n; i++) if (nums[i][i] > result and is_prime(nums[i][i])) result = nums[i][i];
        for (int i = 0; i < n; i++) if (nums[i][n - i - 1] > result and is_prime(nums[i][n - i - 1])) result = nums[i][n - i - 1];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int diagonalPrime(int[][] nums) {
        int result = 0, n = nums.length;
        for (int i = 0; i < n; i++) if (nums[i][i] > result && isPrime(nums[i][i])) result = nums[i][i];
        for (int i = 0; i < n; i++) if (nums[i][n - i - 1] > result && isPrime(nums[i][n - i - 1])) result = nums[i][n - i - 1];
        return result;
    }

    private boolean isPrime(int n) {
        for (int i = 2; i * i <= n; i++) if (n % i == 0) return false;
        return n >= 2;
    }
}
```

__Python__:

```Python
class Solution:
    def diagonalPrime(self, nums: List[List[int]]) -> int:
        def is_prime(n: int) -> bool:
            i = 2
            while i * i <= n:
                if not n % i:
                    return False
                i += 1
            return n >= 2 
        return max(max((max((x if is_prime(x) else -1 for x in (row[i], row[-1 - i]))) for i, row in enumerate(nums))), 0)
```
