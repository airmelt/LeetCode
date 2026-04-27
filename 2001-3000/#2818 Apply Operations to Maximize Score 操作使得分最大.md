# 2818 Apply Operations to Maximize Score 操作使得分最大

__Description:__

You are given an array `nums` of `n` positive integers and an integer `k`.

Initially, you start with a score of `1`. You have to maximize your score by applying the following operation at most `k` times:

- Choose any __non-empty__ subarray `nums[l, ..., r]` that you haven't chosen previously.
- Choose an element `x` of `nums[l, ..., r]` with the highest __prime score__. If multiple such elements exist, choose the one with the smallest index.
- Multiply your score by `x`.

Here, `nums[l, ..., r]` denotes the subarray of `nums` starting at index `l` and ending at the index `r`, both ends being inclusive.

The __prime score__ of an integer `x` is equal to the number of distinct prime factors of `x`. For example, the prime score of `300` is `3` since `300 = 2 * 2 * 3 * 5 * 5`.

Return _the __maximum possible score__ after applying at most_ `k` _operations_.

Since the answer may be large, return it modulo `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: nums = [8,3,9,3,8], k = 2
Output: 81
Explanation: To get a score of 81, we can apply the following operations:
```

- Choose subarray nums[2, ..., 2]. nums[2] is the only element in this subarray. Hence, we multiply the score by nums[2]. The score becomes 1 * 9 = 9.
- Choose subarray nums[2, ..., 3]. Both nums[2] and nums[3] have a prime score of 1, but nums[2] has the smaller index. Hence, we multiply the score by nums[2]. The score becomes 9 * 9 = 81.

It can be proven that 81 is the highest score one can obtain.

Example 2:

```text
Input: nums = [19,12,14,6,10,18], k = 3
Output: 4788
Explanation: To get a score of 4788, we can apply the following operations: 
```

- Choose subarray nums[0, ..., 0]. nums[0] is the only element in this subarray. Hence, we multiply the score by nums[0]. The score becomes 1 * 19 = 19.
- Choose subarray nums[5, ..., 5]. nums[5] is the only element in this subarray. Hence, we multiply the score by nums[5]. The score becomes 19 * 18 = 342.
- Choose subarray nums[2, ..., 3]. Both nums[2] and nums[3] have a prime score of 2, but nums[2] has the smaller index. Hence, we multiply the score by nums[2]. The score becomes 342 * 14 = 4788.

It can be proven that 4788 is the highest score one can obtain.

__Constraints:__

- `1 <= nums.length == n <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`
- `1 <= k <= min(n * (n + 1) / 2, 10 ^ 9)`

__题目描述:__

给你一个长度为 `n` 的正整数数组 `nums` 和一个整数 `k` 。

一开始，你的分数为 `1` 。你可以进行以下操作至多 `k` 次，目标是使你的分数最大:

- 选择一个之前没有选过的 __非空__ 子数组 `nums[l, ..., r]` 。
- 从 `nums[l, ..., r]` 里面选择一个 __质数分数__ 最高的元素 `x` 。如果多个元素质数分数相同且最高，选择下标最小的一个。
- 将你的分数乘以 `x` 。

`nums[l, ..., r]` 表示 `nums` 中起始下标为 `l` ，结束下标为 `r` 的子数组，两个端点都包含。

一个整数的 __质数分数__ 等于 `x` 不同质因子的数目。比方说， `300` 的质数分数为 `3` ，因为 `300 = 2 * 2 * 3 * 5 * 5` 。

请你返回进行至多 `k` 次操作后，可以得到的 __最大分数__ 。

由于答案可能很大，请你将结果对 `10 ^ 9 + 7` 取余后返回。

__示例:__

示例 1：

```text
输入：nums = [8,3,9,3,8], k = 2
输出：81
解释：进行以下操作可以得到分数 81 ：
```

- 选择子数组 nums[2, ..., 2] 。nums[2] 是子数组中唯一的元素。所以我们将分数乘以 nums[2] ，分数变为 1 * 9 = 9 。
- 选择子数组 nums[2, ..., 3] 。nums[2] 和 nums[3] 质数分数都为 1 ，但是 nums[2] 下标更小。所以我们将分数乘以 nums[2] ，分数变为 9 * 9 = 81 。

81 是可以得到的最高得分。

示例 2：

```text
输入：nums = [19,12,14,6,10,18], k = 3
输出：4788
解释：进行以下操作可以得到分数 4788 ：
```

- 选择子数组 nums[0, ..., 0] 。nums[0] 是子数组中唯一的元素。所以我们将分数乘以 nums[0] ，分数变为 1 * 19 = 19 。
- 选择子数组 nums[5, ..., 5] 。nums[5] 是子数组中唯一的元素。所以我们将分数乘以 nums[5] ，分数变为 19 * 18 = 342 。
- 选择子数组 nums[2, ..., 3] 。nums[2] 和 nums[3] 质数分数都为 2，但是 nums[2] 下标更小。所以我们将分数乘以 nums[2] ，分数变为  342 * 14 = 4788 。

4788 是可以得到的最高的分。

__提示：__

- `1 <= nums.length == n <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`
- `1 <= k <= min(n * (n + 1) / 2, 10 ^ 9)`

__思路:__

```text
单调栈 ➕ 贡献法 ➕ 快速幂
首先用筛法得到每个数的质因数数量
由于只需要比较大小, 可以认为质数的质因数数量为 0
得分要尽可能大就要多选数值大的子数组
考虑最大值 nums[i]
它左边最远能到达 left[i], 右边最远能到达 right[i]
初始化 left 为 -1, right 为 n
那么这段子数组 [left[i], right[i]] 都不能比 nums[i] 的质因数数量多
且左边 [left[i], i] 的质因数数量都必须小于 nums[i] 的质因数
用单调栈记录质因数数量
单调栈中的质因数数量递增
当当前栈顶的质因数数量不如现在的质因数数量
说明之前的栈顶的 right 就是现在的 i, 弹出 i 并更新 right
如果栈非空, 说明栈顶的质因数数量大于等于当前的 i, 那么 left 就是 i
之后将 i 加入单调栈
从大到小排序数组
每个数对于结果的贡献为 cur = (i - left[i]) * (right[i] - i)
对结果的贡献为 nums[i] ^ cur
这一步需要用快速幂
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
constexpr int MX = 1e5 + 1, MOD = 1e9 + 7;
int omega[MX];
int init = []() 
{
    for (int i = 2; i < MX; i++) if (omega[i] == 0) for (int j = i; j < MX; j += i) ++omega[j];
    return 0;
}();

class Solution 
{
public:
    int maximumScore(vector<int> &nums, int k) 
    {
        int n = nums.size();
        vector<int> left(n, -1), right(n, n), idx(n);
        stack<int> st;
        for (int i = 0; i < n; i++) 
        {
            while (!st.empty() and omega[nums[st.top()]] < omega[nums[i]]) 
            {
                right[st.top()] = i;
                st.pop();
            }
            if (!st.empty()) left[i] = st.top();
            st.push(i);
        }
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](const int a, const int b) { return nums[a] > nums[b]; });
        auto helper = [](auto a, auto k) -> long long
        {
            auto result = 1LL;
            for (; k; k >>= 1LL) 
            {
                if (k & 1LL) result = result * a % MOD;
                a = a * a % MOD;
            }
            return result;
        };
        auto result = 1LL, cur = 0LL;
        for (int i : idx) 
        {
            result = result * helper((long long)nums[i], min((long long)k, cur = (long long)(i - left[i]) * (right[i] - i))) % MOD;
            k -= cur;
            if (k <= 0) break;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private static final int MOD = 1_000_000_007, MX = 100_005, omega[] = new int[MX];

    static {
        for (int i = 2; i < MX; i++) if (omega[i] == 0) for (int j = i; j < MX; j += i) ++omega[j];
    }

    public int maximumScore(List<Integer> nums, int k) {
        int n = nums.size(), left[] = new int[n], right[] = new int[n];
        Arrays.fill(left, -1);
        Arrays.fill(right, n);
        var stack = new Stack<Integer>();
        var idx = new Integer[n];
        for (int i = 0; i < n; i++) {
            idx[i] = i;
            while (!stack.isEmpty() && omega[nums.get(stack.peek())] < omega[nums.get(i)]) right[stack.pop()] = i;
            if (!stack.isEmpty()) left[i] = stack.peek();
            stack.push(i);
        }
        Arrays.sort(idx, (a, b) -> nums.get(b) - nums.get(a));
        long result = 1L, cur = 0L;
        for (int i : idx) {
            result = result * helper(nums.get(i), Math.min(k, cur = (long)(i - left[i]) * (right[i] - i))) % MOD;
            k -= cur;
            if (k <= 0) break;
        }
        return (int)result;
    }

    private long helper(long a, long k) {
        var result = 1L;
        for (; k > 0L; k >>>= 1L) {
            if ((k & 1L) == 1L) result = result * a % MOD;
            a = a * a % MOD;
        }
        return result;
    }
}
```

__Python__:

```Python
MX = 10 ** 5 + 1
omega = [0] * MX
for i in range(2, MX): 
    if not omega[i]:
        for j in range(i, MX, i):
            omega[j] += 1

class Solution:
    def maximumScore(self, nums: List[int], k: int) -> int:
        left, right, stack, result, MOD = [-1] * (n := len(nums)), [n] * n, [-1], 1, 10 ** 9 + 7
        for i, num in enumerate(nums):
            while len(stack) > 1 and omega[nums[stack[-1]]] < omega[num]:
                right[stack.pop()] = i
            left[i] = stack[-1]
            stack.append(i)
        for i, num, l, r in sorted(zip(range(n), nums, left, right), key=lambda z: -z[1]):
            result = result * pow(num, min(k, cur := (i - l) * (r - i)), MOD) % MOD
            k -= cur
            if k <= 0:
                break
        return result
```
