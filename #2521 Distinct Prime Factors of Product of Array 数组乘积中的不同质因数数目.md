# 2521 Distinct Prime Factors of Product of Array 数组乘积中的不同质因数数目

__Description:__

Given an array of positive integers `nums`, return _the number of __distinct prime factors__ in the product of the elements of_ `nums`.

Note that:

- A number greater than `1` is called __prime__ if it is divisible by only `1` and itself.
- An integer `val1` is a factor of another integer `val2` if `val2 / val1` is an integer.

__Example:__

Example 1:

```text
Input: nums = [2,4,3,7,10,6]
Output: 4
Explanation:
The product of all the elements in nums is: 2 * 4 * 3 * 7 * 10 * 6 = 10080 = 25 * 32 * 5 * 7.
There are 4 distinct prime factors so we return 4.
```

Example 2:

```text
Input: nums = [2,4,8,16]
Output: 1
Explanation:
The product of all the elements in nums is: 2 * 4 * 8 * 16 = 1024 = 210.
There is 1 distinct prime factor so we return 1.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 4`
- `2 <= nums[i] <= 1000`

__题目描述:__

给你一个正整数数组 `nums` ，对 `nums` 所有元素求积之后，找出并返回乘积中 __不同质因数__ 的数目。

注意：

- __质数__ 是指大于 `1` 且仅能被 `1` 及自身整除的数字。
- 如果 `val2 / val1` 是一个整数，则整数 `val1` 是另一个整数 `val2` 的一个因数。

__示例:__

示例 1：

```text
输入：nums = [2,4,3,7,10,6]
输出：4
解释：
nums 中所有元素的乘积是：2 * 4 * 3 * 7 * 10 * 6 = 10080 = 25 * 32 * 5 * 7 。
共有 4 个不同的质因数，所以返回 4 。
```

示例 2：

```text
输入：nums = [2,4,8,16]
输出：1
解释：
nums 中所有元素的乘积是：2 * 4 * 8 * 16 = 1024 = 210 。
共有 1 个不同的质因数，所以返回 1 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 4`
- `2 <= nums[i] <= 1000`

__思路:__

```text
数学
分解质因数
把所有质数因子都加入到哈希表中
由于 nums[i] <= 1000, 所以只需要考虑 2 ~ 997 的质数
可以预先打表求出范围内的所有质数
时间复杂度为 O(NU ^ 1 / 2), 空间复杂度为 O(U / logU), U 为 nums[i] 的最大值, 本题中 U = 1000
```

__代码:__

__C++__:

```C++
class Solution
{
public:
    int distinctPrimeFactors(vector<int>& nums) 
    {
        unordered_set<int> result;
        for (int num : nums) 
        {
            for (int i = 2; i * i <= num; i++) 
            {
                if (!(num % i))
                {
                    result.insert(i);
                    num /= i;
                    while (!(num % i)) num /= i;
                }
            }
            if (num > 1) result.insert(num);
        }
        return result.size();
    }
};
```

__Java__:

```Java
class Solution {
    public int distinctPrimeFactors(int[] nums) {
        var result = new HashSet<Integer>();
        for (int num : nums) {
            for (int i = 2; i * i <= num; i++) {
                if (num % i == 0) {
                    result.add(i);
                    num /= i;
                    while (num % i == 0) num /= i;
                }
            }
            if (num > 1) result.add(num);
        }
        return result.size();
    }
}
```

__Python__:

```Python
class Solution:
    def distinctPrimeFactors(self, nums: List[int]) -> int:
        primes, result = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199, 211, 223, 227, 229, 233, 239, 241, 251, 257, 263, 269, 271, 277, 281, 283, 293, 307, 311, 313, 317, 331, 337, 347, 349, 353, 359, 367, 373, 379, 383, 389, 397, 401, 409, 419, 421, 431, 433, 439, 443, 449, 457, 461, 463, 467, 479, 487, 491, 499, 503, 509, 521, 523, 541, 547, 557, 563, 569, 571, 577, 587, 593, 599, 601, 607, 613, 617, 619, 631, 641, 643, 647, 653, 659, 661, 673, 677, 683, 691, 701, 709, 719, 727, 733, 739, 743, 751, 757, 761, 769, 773, 787, 797, 809, 811, 821, 823, 827, 829, 839, 853, 857, 859, 863, 877, 881, 883, 887, 907, 911, 919, 929, 937, 941, 947, 953, 967, 971, 977, 983, 991, 997}, set()
        for num in nums:
            while num not in primes:
                for p in primes:
                    if not num % p:
                        result.add(p)
                        num //= p
                        break
            result.add(num)
        return len(result)
```
