# 2226 Maximum Candies Allocated to K Children 每个小孩最多能分到多少糖果

__Description:__

You are given a __0-indexed__ integer array `candies`. Each element in the array denotes a pile of candies of size `candies[i]`. You can divide each pile into any number of __sub piles__, but you __cannot__ merge two piles together.

You are also given an integer `k`. You should allocate piles of candies to `k` children such that each child gets the __same__ number of candies. Each child can take __at most one__ pile of candies and some piles of candies may go unused.

Return the maximum number of candies each child can get.

__Example:__

Example 1:

```text
Input: candies = [5,8,6], k = 3
Output: 5
Explanation: We can divide candies[1] into 2 piles of size 5 and 3, and candies[2] into 2 piles of size 5 and 1. We now have five piles of candies of sizes 5, 5, 3, 5, and 1. We can allocate the 3 piles of size 5 to 3 children. It can be proven that each child cannot receive more than 5 candies.
```

Example 2:

```text
Input: candies = [2,5], k = 11
Output: 0
Explanation: There are 11 children but only 7 candies in total, so it is impossible to ensure each child receives at least one candy. Thus, each child gets no candy and the answer is 0.
```

__Constraints:__

- `1 <= candies.length <= 10 ^ 5`
- `1 <= candies[i] <= 10 ^ 7`
- `1 <= k <= 10 ^ 12`

__题目描述:__

给你一个 __下标从 0 开始__ 的整数数组 `candies` 。数组中的每个元素表示大小为 `candies[i]` 的一堆糖果。你可以将每堆糖果分成任意数量的 __子堆__ ，但 __无法__ 再将两堆合并到一起。

另给你一个整数 `k` 。你需要将这些糖果分配给 `k` 个小孩，使每个小孩分到 __相同__ 数量的糖果。每个小孩可以拿走 __至多一堆__ 糖果，有些糖果可能会不被分配。

返回每个小孩可以拿走的 最大糖果数目 。

__示例:__

示例 1：

```text
输入：candies = [5,8,6], k = 3
输出：5
解释：可以将 candies[1] 分成大小分别为 5 和 3 的两堆，然后把 candies[2] 分成大小分别为 5 和 1 的两堆。现在就有五堆大小分别为 5、5、3、5 和 1 的糖果。可以把 3 堆大小为 5 的糖果分给 3 个小孩。可以证明无法让每个小孩得到超过 5 颗糖果。
```

示例 2：

```text
输入：candies = [2,5], k = 11
输出：0
解释：总共有 11 个小孩，但只有 7 颗糖果，但如果要分配糖果的话，必须保证每个小孩至少能得到 1 颗糖果。因此，最后每个小孩都没有得到糖果，答案是 0 。
```

__提示：__

- `1 <= candies.length <= 10 ^ 5`
- `1 <= candies[i] <= 10 ^ 7`
- `1 <= k <= 10 ^ 12`

__思路:__

```text
二分查找
若 m 满足题意, 则 m - 1 也满足题意
所以要找到最大的 m, 使得 m 满足题意
左边界为 1, 右边界为 sum(candies) / k + 1
可以规避除 0 的情况
如果 mid = (left + right) / 2 满足题意, 则 left = mid + 1
否则 right = mid
最后返回 left - 1
时间复杂度为 O(NlogM), 空间复杂度为 O(1), M 为 sum(candies) / k, 不会超过数组的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumCandies(vector<int>& candies, long long k) 
    {
        int left = 1, right = accumulate(candies.begin(), candies.end(), 0LL) / k + left;
        auto check = [&](int i) -> bool 
        {
            long long result = 0LL;
            for (const auto& candy : candies) result += candy / i;
            return result >= k;
        };
        while (left < right) 
        {
            int mid = left + ((right - left) >> 1);
            if (check(mid)) left = mid + 1;
            else right = mid;
        }
        return left - 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumCandies(int[] candies, long k) {
        int left = 1, right = left + (int)(Arrays.stream(candies).sum() / k);
        while (left < right) {
            int mid = left + ((right - left) >>> 1);
            if (check(mid, candies, k)) left = mid + 1;
            else right = mid;
        }
        return left - 1;
    }

    private boolean check(int mid, int[] candies, long k) {
        long result = 0L;
        for (int candy : candies) result += candy / mid;
        return result >= k;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumCandies(self, candies: List[int], k: int) -> int:
        return bisect_right(range(sum(candies) // k), -k, key=lambda x: -sum(v // (x + 1) for v in candies))
```
