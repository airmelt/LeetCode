# 2333 Minimum Sum of Squared Difference 最小差值平方和

__Description:__

You are given two positive __0-indexed__ integer arrays `nums1` and `nums2`, both of length `n`.

The __sum of squared difference__ of arrays `nums1` and `nums2` is defined as the __sum__ of `(nums1[i] - nums2[i]) ^ 2` for each `0 <= i < n`.

You are also given two positive integers `k1` and `k2`. You can modify any of the elements of `nums1` by `+1` or `-1` at most `k1` times. Similarly, you can modify any of the elements of `nums2` by `+1` or `-1` at most `k2` times.

Return _the minimum __sum of squared difference__ after modifying array_ `nums1` _at most_ `k1` _times and modifying array_ `nums2` _at most_ `k2` _times_.

Note: You are allowed to modify the array elements to become negative integers.

__Example:__

Example 1:

```text
Input: nums1 = [1,2,3,4], nums2 = [2,10,20,19], k1 = 0, k2 = 0
Output: 579
Explanation: The elements in nums1 and nums2 cannot be modified because k1 = 0 and k2 = 0. 
The sum of square difference will be: (1 - 2) ** 2 + (2 - 10) ** 2 + (3 - 20) ** 2 + (4 - 19) ** 2 = 579.
```

Example 2:

```text
Input: nums1 = [1,4,10,12], nums2 = [5,8,6,9], k1 = 1, k2 = 1
Output: 43
Explanation: One way to obtain the minimum sum of square difference is: 
```

- Increase nums1[0] once.
- Increase nums2[2] once.

The minimum of the sum of square difference will be:

(2 - 5) \*\* 2 + (4 - 8) \*\* 2 + (10 - 7) \*\* 2 + (12 - 9) \*\* 2 = 43.

Note that, there are other ways to obtain the minimum of the sum of square difference, but there is no way to obtain a sum smaller than 43.

__Constraints:__

- `n == nums1.length == nums2.length`
- `1 <= n <= 10 ^ 5`
- `0 <= nums1[i], nums2[i] <= 10 ^ 5`
- `0 <= k1, k2 <= 10 ^ 9`

__题目描述:__

给你两个下标从 __0__ 开始的整数数组 `nums1` 和 `nums2` ，长度为 `n` 。

数组 `nums1` 和 `nums2` 的 __差值平方和__ 定义为所有满足 `0 <= i < n` 的 `(nums1[i] - nums2[i]) ^ 2` 之和。

同时给你两个正整数 `k1` 和 `k2` 。你可以将 `nums1` 中的任意元素 `+1` 或者 `-1` 至多 `k1` 次。类似的，你可以将 `nums2` 中的任意元素 `+1` 或者 `-1` 至多 `k2` 次。

请你返回修改数组 `nums1` 至多 `k1` 次且修改数组 `nums2` 至多 `k2` 次后的最小 __差值平方和__ 。

注意：你可以将数组中的元素变成 负 整数。

__示例:__

示例 1：

```text
输入：nums1 = [1,2,3,4], nums2 = [2,10,20,19], k1 = 0, k2 = 0
输出：579
解释：nums1 和 nums2 中的元素不能修改，因为 k1 = 0 和 k2 = 0 。
差值平方和为：(1 - 2) ** 2 + (2 - 10) ** 2 + (3 - 20) ** 2 + (4 - 19) ** 2 = 579。
```

示例 2：

```text
输入：nums1 = [1,4,10,12], nums2 = [5,8,6,9], k1 = 1, k2 = 1
输出：43
解释：一种得到最小差值平方和的方式为：
```

- 将 nums1[0] 增加一次。
- 将 nums2[2] 增加一次。

最小差值平方和为：

(2 - 5) \*\* 2 + (4 - 8) \*\* 2 + (10 - 7) \*\* 2 + (12 - 9) \*\* 2 = 43。

注意，也有其他方式可以得到最小差值平方和，但没有得到比 43 更小答案的方案。

__提示：__

- `n == nums1.length == nums2.length`
- `1 <= n <= 10 ^ 5`
- `0 <= nums1[i], nums2[i] <= 10 ^ 5`
- `0 <= k1, k2 <= 10 ^ 9`

__思路:__

```text
贪心
令 k = k1 + k2
首先计算 nums1 和 nums2 的差值的绝对值
如果两个数组的差值的绝对值之和小于等于 k, 则返回 0, 因为可以全部变为 0
令 result 为差值的平方和
将 nums1 按照差值的绝对值从大到小排序
从最大的差值开始遍历
result 先减去当前的差值的平方
尝试将它变为 nums1[i + 1] 的值, 使得差值的绝对值尽可能小
如果 k 足够把 nums1[i] 之前的差值全部变为 nums1[i + 1] 的值, 则减少 k 并且将 i 及之前的值全部变为 nums1[i + 1] 的值
为了减少边界判断, 在 nums1 的末尾添加一个 0
继续遍历
否则 k 不够的时候, nums1[i] 只能减少到 nums1[i] - k / (i + 1)
返回 result + k % (i + 1) * (nums1[i] - 1) * (nums1[i] - 1) + (i + 1 - k % (i + 1)) * nums1[i] * nums1[i], 即为等差数列求和
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minSumSquareDiff(vector<int>& nums1, vector<int>& nums2, int k1, int k2) 
    {
        long long result = 0LL, k = k1 + k2, j, v, c;
        for (int i = 0, n = nums1.size(); i < n; i++)
        {
            nums1[i] = abs(nums1[i] - nums2[i]);
            result += (long long)nums1[i] * nums1[i];
        }
        if (accumulate(nums1.begin(), nums1.end(), 0LL) <= k) return 0;
        sort(nums1.begin(), nums1.end(), greater());
        nums1.emplace_back(0);
        for (int i = 0; ; i++)
        {
            result -= (v = nums1[i]) * nums1[i];
            if ((c = (j = i + 1) * (v - nums1[j])) < k)
            {
                k -= c;
                continue;
            }
            v -= k / j;
            return result + k % j * (v - 1) * (v - 1) + (j - k % j) * v * v;
        }
    }
};
```

__Java__:

```Java
class Solution {
    public long minSumSquareDiff(int[] nums1, int[] nums2, int k1, int k2) {
        long result = 0L, k = k1 + k2, m, v, c;
        for (int i = 0, n = nums1.length; i < n; i++) {
            nums1[i] = Math.abs(nums1[i] - nums2[i]);
            result += (long)nums1[i] * nums1[i];
        }
        if (Arrays.stream(nums1).mapToLong(x -> x).sum() <= k) return 0;
        Arrays.sort(nums1);
        for (int n = nums1.length, i = n - 1; ; i--) {
            result -= (v = nums1[i]) * v;
            if ((c = (m = n - i) * (v - (i > 0 ? nums1[i - 1] : 0))) < k) {
                k -= c;
                continue;
            }
            v -= k / m;
            return result + k % m * (v - 1) * (v - 1) + (m - k % m) * v * v;
        }
    }
}
```

__Python__:

```Python
class Solution:
    def minSumSquareDiff(self, nums1: List[int], nums2: List[int], k1: int, k2: int) -> int:
        k, n, nums1 = k1 + k2, len(nums1), [abs(x - y) for x, y in zip(nums1, nums2)]
        if sum(nums1) <= k:
            return 0
        result = sum(x ** 2 for x in nums1)
        nums1.sort(reverse=True)
        nums1.append(0)
        for i, v in enumerate(nums1):
            result -= v * v
            if (c := (j := i + 1) * (v - nums1[j])) < k:
                k -= c
                continue
            v -= k // j
            return result + k % j * (v - 1) * (v - 1) + (j - k % j) * v * v
```
