# 2183 Count Array Pairs Divisible by K 统计可以被 K 整除的下标对数目

__Description:__

Given a __0-indexed__ integer array `nums` of length `n` and an integer `k`, return _the __number of pairs___ `(i, j)` _such that:_

- `0 <= i < j <= n - 1` _and_
- `nums[i] * nums[j]` _is divisible by_ `k`.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4,5], k = 2
Output: 7
Explanation: 
The 7 pairs of indices whose corresponding products are divisible by 2 are
(0, 1), (0, 3), (1, 2), (1, 3), (1, 4), (2, 3), and (3, 4).
Their products are 2, 4, 6, 8, 10, 12, and 20 respectively.
Other pairs such as (0, 2) and (2, 4) have products 3 and 15 respectively, which are not divisible by 2.
```

Example 2:

```text
Input: nums = [1,2,3,4], k = 5
Output: 0
Explanation: There does not exist any pair of indices whose corresponding product is divisible by 5.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i], k <= 10 ^ 5`

__题目描述:__

给你一个下标从 __0__ 开始、长度为 `n` 的整数数组 `nums` 和一个整数 `k` ，返回满足下述条件的下标对 `(i, j)` 的数目:

- `0 <= i < j <= n - 1` 且
- `nums[i] * nums[j]` 能被 `k` 整除。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4,5], k = 2
输出：7
解释：
共有 7 对下标的对应积可以被 2 整除：
(0, 1)、(0, 3)、(1, 2)、(1, 3)、(1, 4)、(2, 3) 和 (3, 4)
它们的积分别是 2、4、6、8、10、12 和 20 。
其他下标对，例如 (0, 2) 和 (2, 4) 的乘积分别是 3 和 15 ，都无法被 2 整除。
```

示例 2：

```text
输入：nums = [1,2,3,4], k = 5
输出：0
解释：不存在对应积可以被 5 整除的下标对。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i], k <= 10 ^ 5`

__思路:__

```text
数学
先将 k 的所有因子加入哈希表 divisors 中
然后遍历数组 nums, 统计每个数的因子个数
先加上 m[k / gcd(v, k)]
再更新因子个数
对每一个 k 的因子, 如果当前值 v 能被整除, 则因子个数加一
时间复杂度为 O(NlogK), 空间复杂度为 O(logK)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long countPairs(vector<int>& nums, int k) 
    {
        unordered_set<int> divisors;
        for (int i = 1, n = (int)(sqrt(k)) + 1; i < n; i++) 
        {
            if (!(k % i))
            {
                divisors.insert(i);
                divisors.insert(k / i);
            }
        }
        unordered_map<int, int> m;
        long long result = 0LL;
        for (const auto& v : nums) 
        {
            result += m[k / gcd(v, k)];
            for (const auto& d : divisors) if (!(v % d)) ++m[d];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long countPairs(int[] nums, int k) {
        Set<Integer> divisors = new HashSet<>();
        for (int i = 1, n = (int)(Math.sqrt(k)) + 1; i < n; i++) {
            if (k % i == 0) {
                divisors.add(i);
                divisors.add(k / i);
            }
        }
        Map<Integer, Integer> map = new HashMap<>();
        long result = 0L;
        for (int v : nums) {
            result += map.getOrDefault(k / gcd(v, k), 0);
            for (int d : divisors) if (v % d == 0) map.merge(d, 1, Integer::sum);
        }
        return result;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
class Solution:
    def countPairs(self, nums: List[int], k: int) -> int:
        divisors, result, count = set(), 0, Counter()
        for d in range(1, int(k ** 0.5) + 1):
            if not k % d:
                divisors.add(d)
                divisors.add(k / d)
        for v in nums:
            result += count[k / gcd(v, k)]
            for d in divisors:
                if not v % d:
                    count[d] += 1
        return result
```
