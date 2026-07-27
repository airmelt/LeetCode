# 3115 Maximum Prime Difference 质数的最大距离

__Description:__

You are given an integer array `nums`.

Return an integer that is the __maximum__ distance between the __indices__ of two (not necessarily different) prime numbers in `nums`_._

__Example:__

Example 1:

```text
Input: nums = [4,2,9,5,3]

Output: 3
```

__Explanation:__ `nums[1]`, `nums[3]`, and `nums[4]` are prime. So the answer is `|4 - 1| = 3`.

Example 2:

```text
Input: nums = [4,8,2,8]

Output: 0
```

__Explanation:__ `nums[2]` is prime. Because there is just one prime number, the answer is `|2 - 2| = 0`.

__Constraints:__

- `1 <= nums.length <= 3 * 10 ^ 5`
- `1 <= nums[i] <= 100`
- The input is generated such that the number of prime numbers in the `nums` is at least one.

__题目描述:__

给你一个整数数组 `nums`。

返回两个（不一定不同的）质数在 `nums` 中 __下标__ 的 __最大距离__。

__示例:__

示例 1：

```text
输入： nums = [4,2,9,5,3]

输出： 3
```

__解释:__ `nums[1]`、 `nums[3]` 和 `nums[4]` 是质数。因此答案是 `|4 - 1| = 3`。

示例 2：

```text
输入： nums = [4,8,2,8]

输出： 0
```

__解释:__ `nums[2]` 是质数。因为只有一个质数，所以答案是 `|2 - 2| = 0`。

__提示：__

- `1 <= nums.length <= 3 * 10 ^ 5`
- `1 <= nums[i] <= 100`
- 输入保证 `nums` 中至少有一个质数。

__思路:__

```text
模拟
分别从数组的左边和右边开始遍历找到第一个质数就停止
寻找质数可以使用筛法或者直接计算, 这里的数值比较小也可以直接打表
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumPrimeDifference(vector<int>& nums) 
    {
        int left = 0, right = nums.size() - 1;
        while (!(PRIME_MASK[nums[left] >> 6] >> (nums[left] % 64) & 1)) ++left;
        while (!(PRIME_MASK[nums[right] >> 6] >> (nums[right] % 64) & 1)) --right;
        return right - left;
    }
private:
    static constexpr uint64_t PRIME_MASK[2]{ 0x28208a20a08a28ac, 0x202088288 };
};
```

__Java__:

```Java
class Solution {
    public int maximumPrimeDifference(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (!isPrime(nums[left])) ++left;
        while (!isPrime(nums[right])) --right;
        return right - left;
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
    def maximumPrimeDifference(self, nums: List[int]) -> int:
        primes, left, right = { 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97 }, 0, 0
        for i, num in enumerate(nums):
            if num in primes:
                left = i
                break
        for i, num in enumerate(nums[::-1]):
            if num in primes:
                right = i
                break
        return len(nums) - right - left - 1
```
