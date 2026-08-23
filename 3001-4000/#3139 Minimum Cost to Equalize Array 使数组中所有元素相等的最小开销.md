# 3139 Minimum Cost to Equalize Array 使数组中所有元素相等的最小开销

__Description:__

You are given an integer array `nums` and two integers `cost1` and `cost2`. You are allowed to perform __either__ of the following operations __any__ number of times:

- Choose an index `i` from `nums` and __increase__ `nums[i]` by `1` for a cost of `cost1`.
- Choose two __different__ indices `i`, `j`, from `nums` and __increase__ `nums[i]` and `nums[j]` by `1` for a cost of `cost2`.

Return the __minimum cost__ required to make all elements in the array __equal__.

Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: nums = [4,1], cost1 = 5, cost2 = 2

Output: 15

Explanation:

The following operations can be performed to make the values equal:

Increase nums[1] by 1 for a cost of 5. nums becomes [4,2].
Increase nums[1] by 1 for a cost of 5. nums becomes [4,3].
Increase nums[1] by 1 for a cost of 5. nums becomes [4,4].

The total cost is 15.
```

Example 2:

```text
Input: nums = [2,3,3,3,5], cost1 = 2, cost2 = 1

Output: 6

Explanation:

The following operations can be performed to make the values equal:

Increase nums[0] and nums[1] by 1 for a cost of 1. nums becomes [3,4,3,3,5].
Increase nums[0] and nums[2] by 1 for a cost of 1. nums becomes [4,4,4,3,5].
Increase nums[0] and nums[3] by 1 for a cost of 1. nums becomes [5,4,4,4,5].
Increase nums[1] and nums[2] by 1 for a cost of 1. nums becomes [5,5,5,4,5].
Increase nums[3] by 1 for a cost of 2. nums becomes [5,5,5,5,5].

The total cost is 6.
```

Example 3:

```text
Input: nums = [3,5,3], cost1 = 1, cost2 = 3

Output: 4

Explanation:

The following operations can be performed to make the values equal:

Increase nums[0] by 1 for a cost of 1. nums becomes [4,5,3].
Increase nums[0] by 1 for a cost of 1. nums becomes [5,5,3].
Increase nums[2] by 1 for a cost of 1. nums becomes [5,5,4].
Increase nums[2] by 1 for a cost of 1. nums becomes [5,5,5].

The total cost is 4.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`
- `1 <= cost1 <= 10 ^ 6`
- `1 <= cost2 <= 10 ^ 6`

__题目描述:__

给你一个整数数组 `nums` 和两个整数 `cost1` 和 `cost2` 。你可以执行以下 __任一__ 操作 __任意__ 次：

- 从 `nums` 中选择下标 `i` 并且将 `nums[i]` 增加 `1` ，开销为 `cost1`。
- 选择 `nums` 中两个 __不同__ 下标 `i` 和 `j` ，并且将 `nums[i]` 和 `nums[j]` 都 增加 `1` ，开销为 `cost2` 。

你的目标是使数组中所有元素都 __相等__ ，请你返回需要的 __最小开销__ 之和。

由于答案可能会很大，请你将它对 `10 ^ 9 + 7` __取余__ 后返回。

__示例:__

示例 1：

```text
输入：nums = [4,1], cost1 = 5, cost2 = 2

输出：15

解释：

执行以下操作可以使数组中所有元素相等：

将 nums[1] 增加 1 ，开销为 5 ，nums 变为 [4,2] 。
将 nums[1] 增加 1 ，开销为 5 ，nums 变为 [4,3] 。
将 nums[1] 增加 1 ，开销为 5 ，nums 变为 [4,4] 。

总开销为 15 。
```

示例 2：

```text
输入：nums = [2,3,3,3,5], cost1 = 2, cost2 = 1

输出：6

解释：

执行以下操作可以使数组中所有元素相等：

将 nums[0] 和 nums[1] 同时增加 1 ，开销为 1 ，nums 变为 [3,4,3,3,5] 。
将 nums[0] 和 nums[2] 同时增加 1 ，开销为 1 ，nums 变为 [4,4,4,3,5] 。
将 nums[0] 和 nums[3] 同时增加 1 ，开销为 1 ，nums 变为 [5,4,4,4,5] 。
将 nums[1] 和 nums[2] 同时增加 1 ，开销为 1 ，nums 变为 [5,5,5,4,5] 。
将 nums[3] 增加 1 ，开销为 2 ，nums 变为 [5,5,5,5,5] 。

总开销为 6 。
```

示例 3：

```text
输入：nums = [3,5,3], cost1 = 1, cost2 = 3

输出：4

解释：

执行以下操作可以使数组中所有元素相等：

将 nums[0] 增加 1 ，开销为 1 ，nums 变为 [4,5,3] 。
将 nums[0] 增加 1 ，开销为 1 ，nums 变为 [5,5,3] 。
将 nums[2] 增加 1 ，开销为 1 ，nums 变为 [5,5,4] 。
将 nums[2] 增加 1 ，开销为 1 ，nums 变为 [5,5,5] 。

总开销为 4 。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`
- `1 <= cost1 <= 10 ^ 6`
- `1 <= cost2 <= 10 ^ 6`

__思路:__

```text
数学
由于只能增加, 至少需要将所有数字增加到数组最大值
记录最大值 mx = max(nums), 最小值 mn = min(nums)
1. 如果 n == 1 或者 n == 2, 那么只能使用 cost1, 总开销为 (mx - mn) * cost1
2. 当 2 * cost1 <= cost2 时, 只用 cost1 更优, 需要使用 base = n * mx - sum(nums) 次 cost1 总开销为 base * cost1, 刚好可以和第一种情况合并计算
3. 当 2 * cost1 > cost2 时, 需要尽可能使用 cost2 同时增加 nums 中的元素
设最终 nums 所有元素都会增加到 x, 令 s = base + (x - mx) * n, 这是需要进行的加 1 的次数之和
每次可以选择 2 个数字进行加的操作, 这里最多为 s >> 1 次 cost2
还有一种情况是对于 x - mn, 每次操作都带上这个数, 最多可以执行 s - (x - mn) 次
取两者的较小值 min(s >> 1, s - (x - mn)) 即 cost2 的执行次数
剩下的用 cost1 补全
则需要 max((s >> 1) * cost2 + (s & 1) * cost1, (s - d) * cost2 + ((d << 1) - s) * cost1)
从 mx 开始枚举 x, 取到最小值即可
令 d << 1 = s, 可得分界点 i = (x * mx - 2 * mn - base) / (n - 2)
求这附近的值 i - 1, i, i + 1, 以及 mx 和 mx + 1 即可得到最小值
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minCostToEqualizeArray(vector<int>& nums, int cost1, int cost2) 
    {
        int MOD = 1e9 + 7, i = 0;
        auto [mn, mx] = ranges::minmax(nums);
        long long n = nums.size(), s = accumulate(nums.begin(), nums.end(), 0LL), base = n * mx - s;
        auto helper = [&](int x) -> long long
        {
            return max(((base + (x - mx) * n) >> 1LL) * cost2 + ((base + (x - mx) * n) & 1LL) * cost1, ((base + (x - mx) * n) - x + mn) * cost2 + (((x - mn) << 1LL) - (base + (x - mx) * n)) * cost1);
        };
        return (n < 3 or (cost1 << 1) <= cost2) ? base * cost1 % MOD : ((i = ((n * mx - (mn << 1) - base + n - 3) / (n - 2))) <= mx ? min(helper(mx), helper(mx + 1)) % MOD : min({ helper(mx), helper(i - 1), helper(i), helper(i + 1) }) % MOD);
    }
};
```

__Java__:

```Java
class Solution {
    public int minCostToEqualizeArray(int[] nums, int cost1, int cost2) {
        int mx = Arrays.stream(nums).max().getAsInt(), mn = Arrays.stream(nums).min().getAsInt(), MOD = 1_000_000_007, i = 0;
        long n = nums.length, s = Arrays.stream(nums).mapToLong(a -> a).sum(), base = n * mx - s;
        return (n < 3 || (cost1 << 1) <= cost2) ? (int)(base * cost1 % MOD) : (int)((i = (int)((n * mx - (mn << 1) - base + n - 3) / (n - 2))) <= mx ? (Math.min(helper(mx, base, n, mn, mx, cost1, cost2), helper(mx + 1, base, n, mn, mx, cost1, cost2))) % MOD : (Math.min(Math.min(Math.min(helper(mx, base, n, mn, mx, cost1, cost2), helper(i - 1, base, n, mn, mx, cost1, cost2)), helper(i, base, n, mn, mx, cost1, cost2)), helper(i + 1, base, n, mn, mx, cost1, cost2))) % MOD);
    }

    private long helper(int x, long base, long n, int mn, int mx, int cost1, int cost2) {
        return Math.max(((base + (x - mx) * n) >>> 1L) * cost2 + ((base + (x - mx) * n) & 1L) * cost1, ((base + (x - mx) * n) - x + mn) * cost2 + (((x - mn) << 1L) - (base + (x - mx) * n)) * cost1);
    }
}
```

__Python__:

```Python
class Solution:
    def minCostToEqualizeArray(self, nums: List[int], cost1: int, cost2: int) -> int:
        return (n * max(nums) - sum(nums)) * cost1 % (10 ** 9 + 7) if (n := len(nums)) <= 2 or (cost1 << 1) <= cost2 else min(f(max(nums)), f(max(nums) + 1)) % (10 ** 9 + 7) if (f := lambda x: max(((s := (n * max(nums) - sum(nums)) + (x - max(nums)) * n) >> 1) * cost2 + (s & 1) * cost1, (s - (d := x - min(nums))) * cost2 + ((d << 1) - s) * cost1)) and (i := (n * max(nums) - (min(nums) << 1) - (n * max(nums) - sum(nums)) + n - 3) // (n - 2)) <= max(nums) else min(f(max(nums)), f(i - 1), f(i), f(i + 1)) % (10 ** 9 + 7)
```
