# 3164 Find the Number of Good Pairs II 优质数对的总数 II

__Description:__

You are given 2 integer arrays `nums1` and `nums2` of lengths `n` and `m` respectively. You are also given a __positive__ integer `k`.

A pair `(i, j)` is called __good__ if `nums1[i]` is divisible by `nums2[j] * k` (`0 <= i <= n - 1`, `0 <= j <= m - 1`).

Return the total number of __good__ pairs.

__Example:__

Example 1:

```text
Input: nums1 = [1,3,4], nums2 = [1,3,4], k = 1

Output: 5

Explanation:

The 5 good pairs are (0, 0), (1, 0), (1, 1), (2, 0), and (2, 2).
```

Example 2:

```text
Input: nums1 = [1,2,4,12], nums2 = [2,4], k = 3

Output: 2

Explanation:

The 2 good pairs are (3, 0) and (3, 1).
```

__Constraints:__

- `1 <= n, m <= 10 ^ 5`
- `1 <= nums1[i], nums2[j] <= 10 ^ 6`
- `1 <= k <= 10 ^ 3`

__题目描述:__

给你两个整数数组 `nums1` 和 `nums2`，长度分别为 `n` 和 `m`。同时给你一个 __正整数__ `k`。

如果 `nums1[i]` 可以被 `nums2[j] * k` 整除，则称数对 `(i, j)` 为 __优质数对__（`0 <= i <= n - 1`, `0 <= j <= m - 1`）。

返回 __优质数对__ 的总数。

__示例:__

示例 1：

```text
输入：nums1 = [1,3,4], nums2 = [1,3,4], k = 1

输出：5

解释：

5个优质数对分别是 (0, 0), (1, 0), (1, 1), (2, 0), 和 (2, 2)。
```

示例 2：

```text
输入：nums1 = [1,2,4,12], nums2 = [2,4], k = 3

输出：2

解释：

2个优质数对分别是 (3, 0) 和 (3, 1)。
```

__提示：__

- `1 <= n, m <= 10 ^ 5`
- `1 <= nums1[i], nums2[j] <= 10 ^ 6`
- `1 <= k <= 10 ^ 3`

__思路:__

```text
1. 因数分解
nums1[i] 能够整除 num2[j] * k
那么 nums1[i] 能整除 k 并且 nums1[i] / k 能整除 nums2[j]
将能够整除 k 的 nums1[i] / k 分解因数
对于每一个 d 如果为 nums1[i] / k 的因数, 就把 d 和 nums1[i] / k / d 使用哈希表记录独特的 1 个, 平方值只记录 1 次
这样对于 nums2[j] 来说如果它存在于哈希表, 说明它能和这个数对应的 nums1[i] 构成优质数对
累计哈希表中出现的 nums2 的个数即可
时间复杂度为 O(N * (C / K) ^ 1 / 2 + M), 空间复杂度为 O(C / K), C 为 nums1 的最大值, N, M 分别为 nums1, nums2 数组的大小
2. 枚举倍数
和方法 1 类似
但是枚举 nums2 中 num 的倍数 num, num * 2, ..., num * p
这里限制 num * p 不大于数组 nums1 的最大值 C 即可
时间复杂度为 O(N + M + (C / K) * logM), 空间复杂度为 O(N + M), C 为 nums1 的最大值, N, M 分别为 nums1, nums2 数组的大小
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long numberOfPairs(vector<int>& nums1, vector<int>& nums2, int k) 
    {
        unordered_map<int, long long> m1, m2;
        for (const auto& num : nums1) if (!(num % k)) ++m1[num / k];
        for (const auto& num : nums2) ++m2[num];
        long long result = 0LL;
        int u = *max_element(nums1.begin(), nums1.end());
        for (const auto& [num, v] : m2)
        {
            int cur = 0;
            for (int d = num; d <= u; d += num) cur += m1.count(d) ? m1[d] : 0;
            result += cur * v;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long numberOfPairs(int[] nums1, int[] nums2, int k) {
        var map = new HashMap<Integer, Integer>();
        for (int num : nums1) {
            if (num % k == 0) {
                for (int d = 1; d * d <= num / k; d++) {
                    if (num / k % d == 0) {
                        map.merge(d, 1, Integer::sum);
                        if (d * d < num / k) map.merge(num / k / d, 1, Integer::sum);
                    }
                }
            }
        }
        long result = 0L;
        for (int num : nums2) result += map.getOrDefault(num, 0);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfPairs(self, nums1: List[int], nums2: List[int], k: int) -> int:
        return 0 if not (max1 := max(c1 := Counter(x // k for x in nums1 if not x % k), default=0)) else sum(v * sum(c1[y] for y in range(x, max1 + 1, x)) for x, v in Counter(nums2).items())
```
