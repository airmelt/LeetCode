# 3153 Sum of Digit Differences of All Pairs 所有数对中数位差之和

__Description:__

You are given an array `nums` consisting of __positive__ integers where all integers have the __same__ number of digits.

The __digit difference__ between two integers is the _count_ of different digits that are in the __same__ position in the two integers.

Return the sum of the __digit differences__ between __all__ pairs of integers in `nums`.

__Example:__

Example 1:

```text
Input: nums = [13,23,12]

Output: 4

Explanation:
We have the following:
```

- The digit difference between 13 and 23 is 1.
- The digit difference between 13 and 12 is 1.
- The digit difference between 23 and 12 is 2.

So the total sum of digit differences between all pairs of integers is 1 + 1 + 2 = 4.

Example 2:

```text
Input: nums = [10,10,10,10]

Output: 0

Explanation:
All the integers in the array are the same. So the total sum of digit differences between all pairs of integers will be 0.
```

__Constraints:__

- `2 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- All integers in `nums` have the same number of digits.

__题目描述:__

你有一个数组 `nums` ，它只包含 __正__ 整数，所有正整数的数位长度都 __相同__ 。

两个整数的 __数位差__ 指的是两个整数 __相同__ 位置上不同数字的数目。

请你返回 `nums` 中 __所有__ 整数对里，__数位差之和__。

__示例:__

示例 1：

```text
输入：nums = [13,23,12]

输出：4

解释：
计算过程如下：
```

- 13 和 23 的数位差为 1 。
- 13 和 12 的数位差为 1 。
- 23 和 12 的数位差为 2 。

所以所有整数数对的数位差之和为 1 + 1 + 2 = 4 。

示例 2：

```text
输入：nums = [10,10,10,10]

输出：0

解释：
数组中所有整数都相同，所以所有整数数对的数位不同之和为 0 。
```

__提示：__

- `2 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `nums` 中的整数都有相同的数位长度

__思路:__

```text
模拟
按照相同位数逐位计算数位差
统计每一个相同位置上不同的数位出现的次数
利用乘法原理求出不同的位数差
或者可以用所有组合减去相同的位
总的位数可以使用 m * n * (n - 1) >> 1 计算得到
时间复杂度为 O(NlogM), 空间复杂度为 O(logM), M 为数组的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long sumDigitDifferences(vector<int>& nums) 
    {
        long long n = nums.size(), m = to_string(nums.front()).size(), result = m * n * (n - 1LL) >> 1LL;
        vector<vector<int>> d(m, vector<int>(10));
        for (int num : nums) for (int i = 0; num > 0; num /= 10) result -= d[i++][num % 10]++;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long sumDigitDifferences(int[] nums) {
        int n = nums.length, m = Integer.toString(nums[0]).length(), d[][] = new int[m][10];
        long result = (long)m * n * (n - 1L) >>> 1L;
        for (int num : nums) for (int i = 0; num > 0; num /= 10) result -= d[i++][num % 10]++;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def sumDigitDifferences(self, nums: List[int]) -> int:
        m, result = len(str(nums[0])), 0
        for i in range(m):
            d, base = defaultdict(int), 10 ** i
            for num in nums:
                d[num // base % 10] += 1
            result += sum(v1 * v2 for k1, v1 in d.items() for k2, v2 in d.items() if k1 != k2)
        return result >> 1
```
