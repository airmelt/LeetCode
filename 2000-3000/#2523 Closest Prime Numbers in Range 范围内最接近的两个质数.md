# 2523 Closest Prime Numbers in Range 范围内最接近的两个质数

__Description:__

Given two positive integers `left` and `right`, find the two integers `num1` and `num2` such that:

- `left <= num1 < num2 <= right` .
- Both `num1` and `num2` are prime numbers.
- `num2 - num1` is the __minimum__ amongst all other pairs satisfying the above conditions.

Return the positive integer array `ans = [num1, num2]`. If there are multiple pairs satisfying these conditions, return the one with the __smallest__ `num1` value. If no such numbers exist, return `[-1, -1]`_._

__Example:__

Example 1:

```text
Input: left = 10, right = 19
Output: [11,13]
Explanation: The prime numbers between 10 and 19 are 11, 13, 17, and 19.
The closest gap between any pair is 2, which can be achieved by [11,13] or [17,19].
Since 11 is smaller than 17, we return the first pair.
```

Example 2:

```text
Input: left = 4, right = 6
Output: [-1,-1]
Explanation: There exists only one prime number in the given range, so the conditions cannot be satisfied.
```

__Constraints:__

- `1 <= left <= right <= 10 ^ 6`

__题目描述:__

给你两个正整数 `left` 和 `right` ，请你找到两个整数 `num1` 和 `num2` ，它们满足:

- `left <= nums1 < nums2 <= right` 。
- `nums1` 和 `nums2` 都是质数。
- `nums2 - nums1` 是满足上述条件的质数对中的 __最小值__ 。

请你返回正整数数组 `ans = [nums1, nums2]` 。如果有多个整数对满足上述条件，请你返回 `nums1` 最小的质数对。如果不存在符合题意的质数对，请你返回 `[-1, -1]` 。

__示例:__

示例 1：

```text
输入：left = 10, right = 19
输出：[11,13]
解释：10 到 19 之间的质数为 11 ，13 ，17 和 19 。
质数对的最小差值是 2 ，[11,13] 和 [17,19] 都可以得到最小差值。
由于 11 比 17 小，我们返回第一个质数对。
```

示例 2：

```text
输入：left = 4, right = 6
输出：[-1,-1]
解释：给定范围内只有一个质数，所以题目条件无法被满足。
```

__提示：__

- `1 <= left <= right <= 10 ^ 6`

__思路:__

```text
数学
筛选质数
找到质数的下界
然后一个个遍历找到最小的差值并记录
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
const int MX = 1e6;
vector<int> primes;

int init = []() 
{
    bool np[MX + 1]{};
    for (int i = 2; i <= MX; ++i) 
    {
        if (!np[i]) primes.push_back(i);
        for (int p: primes) 
        {
            if (i * p > MX) break;
            np[i * p] = true;
            if (i % p == 0) break;
        }
    }
    primes.push_back(MX + 1);
    primes.push_back(MX + 1);
    return 0;
}();

class Solution 
{
public:
    vector<int> closestPrimes(int left, int right) 
    {
        int p = -1, q = -1, i = lower_bound(primes.begin(), primes.end(), left) - primes.begin();
        for (; primes[i + 1] <= right; ++i)
        {
            if (p < 0 or primes[i + 1] - primes[i] < q - p) 
            {
                p = primes[i];
                q = primes[i + 1];
            }
        }
        return {p, q};
    }
};
```

__Java__:

```Java
class Solution {
    private final static int MX = (int) 1e6;
    private final static int[] primes = new int[78500];

    static {
        var np = new boolean[MX + 1];
        var pi = 0;
        for (var i = 2; i <= MX; i++)
            if (!np[i]) {
                primes[pi++] = i;
                for (var j = i; j <= MX / i; ++j)
                    np[i * j] = true;
            }
        primes[pi++] = MX + 1;
        primes[pi++] = MX + 1;
    }

    public int[] closestPrimes(int left, int right) {
        int p = -1, q = -1;
        for (var i = lowerBound(primes, left); primes[i + 1] <= right; ++i)
            if (p < 0 || primes[i + 1] - primes[i] < q - p) {
                p = primes[i];
                q = primes[i + 1];
            }
        return new int[]{p, q};
    }

    private int lowerBound(int[] nums, int target) {
        int left = -1, right = nums.length;
        while (left + 1 < right) {
            int mid = left + ((right - left) >>> 1);
            if (nums[mid] < target) left = mid;
            else right = mid;
        }
        return right;
    }
}
```

__Python__:

```Python
primes, is_prime = [], [True] * (MX := 10 ** 6 + 1)
for i in range(2, MX):
    if is_prime[i]:
        primes.append(i)
        for j in range(i * i, MX, i):
            is_prime[j] = False
primes.extend((MX, MX))

class Solution:
    def closestPrimes(self, left: int, right: int) -> List[int]:
        i, p, q = bisect_left(primes, left), -1, -1
        while primes[i + 1] <= right:
            if p < 0 or primes[i + 1] - primes[i] < q - p:
                p, q = primes[i], primes[i + 1]
            i += 1
        return [p, q]
```
