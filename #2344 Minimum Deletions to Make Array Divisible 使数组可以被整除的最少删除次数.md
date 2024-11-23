# 2344 Minimum Deletions to Make Array Divisible 使数组可以被整除的最少删除次数

__Description:__

You are given two positive integer arrays `nums` and `numsDivide`. You can delete any number of elements from `nums`.

Return _the __minimum__ number of deletions such that the __smallest__ element in_ `nums` ___divides__ all the elements of_ `numsDivide`. If this is not possible, return `-1`.

Note that an integer `x` divides `y` if `y % x == 0`.

__Example:__

Example 1:

```text
Input: nums = [2,3,2,4,3], numsDivide = [9,6,9,3,15]
Output: 2
Explanation: 
The smallest element in [2,3,2,4,3] is 2, which does not divide all the elements of numsDivide.
We use 2 deletions to delete the elements in nums that are equal to 2 which makes nums = [3,4,3].
The smallest element in [3,4,3] is 3, which divides all the elements of numsDivide.
It can be shown that 2 is the minimum number of deletions needed.
```

Example 2:

```text
Input: nums = [4,3,6], numsDivide = [8,2,6,10]
Output: -1
Explanation: 
We want the smallest element in nums to divide all the elements of numsDivide.
There is no way to delete elements from nums to allow this.
```

__Constraints:__

- `1 <= nums.length, numsDivide.length <= 10 ^ 5`
- `1 <= nums[i], numsDivide[i] <= 10 ^ 9`

__题目描述:__

给你两个正整数数组 `nums` 和 `numsDivide` 。你可以从 `nums` 中删除任意数目的元素。

请你返回使 `nums` 中 __最小__ 元素可以整除 `numsDivide` 中所有元素的 __最少__ 删除次数。如果无法得到这样的元素，返回 `-1` 。

如果 `y % x == 0` ，那么我们说整数 `x` 整除 `y` 。

__示例:__

示例 1：

```text
输入：nums = [2,3,2,4,3], numsDivide = [9,6,9,3,15]
输出：2
解释：
[2,3,2,4,3] 中最小元素是 2 ，它无法整除 numsDivide 中所有元素。
我们从 nums 中删除 2 个大小为 2 的元素，得到 nums = [3,4,3] 。
[3,4,3] 中最小元素为 3 ，它可以整除 numsDivide 中所有元素。
可以证明 2 是最少删除次数。
```

示例 2：

```text
输入：nums = [4,3,6], numsDivide = [8,2,6,10]
输出：-1
解释：
我们想 nums 中的最小元素可以整除 numsDivide 中的所有元素。
没有任何办法可以达到这一目的。
```

__提示：__

- `1 <= nums.length, numsDivide.length <= 10 ^ 5`
- `1 <= nums[i], numsDivide[i] <= 10 ^ 9`

__思路:__

```text
贪心
先计算 numsDivide 中所有元素的最大公约数 g
将 nums 中所有元素按照从小到大排序
找到第一个可以整除 g 的元素对应的下标 i 返回
如果找不到这样的元素，返回 -1
或者不排
直接遍历 nums 数组，找到最小的可以整除 g 的元素 mn
如果找不到这样的元素，返回 -1
遍历 nums 数组，找到所有小于 mn 的元素个数并返回
时间复杂度为 O(N + M + logC), 空间复杂度为 O(1), 其中 N 为 nums 数组的长度, M 为 numsDivide 数组的长度, C 为 numsDivide 数组中的最大值    
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(vector<int>& nums, vector<int>& numsDivide) 
    {
        int g = 0, mn = INT_MAX, result = 0;
        for (int x : numsDivide) g = gcd(g, x);
        for (int x : nums) if (!(g % x)) mn = min(mn, x);
        if (mn == INT_MAX) return -1;
        for (int x : nums) if (x < mn) ++result;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(int[] nums, int[] numsDivide) {
        int g = 0;
        for (int x : numsDivide) g = gcd(g, x);
        Arrays.sort(nums);
        for (int i = 0, n = nums.length; i < n; i++) if (g % nums[i] == 0) return i;
        return -1;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, nums: List[int], numsDivide: List[int]) -> int:
        return sum(x < mn for x in nums) if (g := gcd(*numsDivide)) and (mn := min((x for x in nums if g % x == 0), default=0)) else -1
```
