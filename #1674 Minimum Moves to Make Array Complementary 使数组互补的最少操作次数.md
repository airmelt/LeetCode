# 1674 Minimum Moves to Make Array Complementary 使数组互补的最少操作次数

__Description:__

You are given an integer array `nums` of __even__ length `n` and an integer `limit`. In one move, you can replace any integer from `nums` with another integer between `1` and `limit`, inclusive.

The array `nums` is __complementary__ if for all indices `i` (__0-indexed__), `nums[i] + nums[n - 1 - i]` equals the same number. For example, the array `[1,2,3,4]` is complementary because for all indices `i`, `nums[i] + nums[n - 1 - i] = 5`.

Return the ___minimum__ number of moves required to make_ `nums` ___complementary___.

__Example:__

Example 1:

```text
Input: nums = [1,2,4,3], limit = 4
Output: 1
Explanation: In 1 move, you can change nums to [1,2,2,3] (underlined elements are changed).
nums[0] + nums[3] = 1 + 3 = 4.
nums[1] + nums[2] = 2 + 2 = 4.
nums[2] + nums[1] = 2 + 2 = 4.
nums[3] + nums[0] = 3 + 1 = 4.
Therefore, nums[i] + nums[n-1-i] = 4 for every i, so nums is complementary.
```

Example 2:

```text
Input: nums = [1,2,2,1], limit = 2
Output: 2
Explanation: In 2 moves, you can change nums to [2,2,2,2]. You cannot change any number to 3 since 3 > limit.
```

Example 3:

```text
Input: nums = [1,2,1,2], limit = 2
Output: 0
Explanation: nums is already complementary.
```

__Constraints:__

- `n == nums.length`
- `2 <= n <= 10 ^ 5`
- `1 <= nums[i] <= limit <= 10 ^ 5`
- `n` is even.

__题目描述:__

给你一个长度为 __偶数__ `n` 的整数数组 `nums` 和一个整数 `limit` 。每一次操作，你可以将 `nums` 中的任何整数替换为 `1` 到 `limit` 之间的另一个整数。

如果对于所有下标 `i`（__下标从__ `0` __开始__）， `nums[i] + nums[n - 1 - i]` 都等于同一个数，则数组 `nums` 是 __互补的__ 。例如，数组 `[1,2,3,4]` 是互补的，因为对于所有下标 `i` ， `nums[i] + nums[n - 1 - i] = 5` 。

返回使数组 互补 的 最少 操作次数。

__示例:__

示例 1：

```text
输入：nums = [1,2,4,3], limit = 4
输出：1
解释：经过 1 次操作，你可以将数组 nums 变成 [1,2,2,3]（加粗元素是变更的数字）：
nums[0] + nums[3] = 1 + 3 = 4.
nums[1] + nums[2] = 2 + 2 = 4.
nums[2] + nums[1] = 2 + 2 = 4.
nums[3] + nums[0] = 3 + 1 = 4.
对于每个 i ，nums[i] + nums[n-1-i] = 4 ，所以 nums 是互补的。
```

示例 2：

```text
输入：nums = [1,2,2,1], limit = 2
输出：2
解释：经过 2 次操作，你可以将数组 nums 变成 [2,2,2,2] 。你不能将任何数字变更为 3 ，因为 3 > limit 。
```

示例 3：

```text
输入：nums = [1,2,1,2], limit = 2
输出：0
解释：nums 已经是互补的。
```

__提示：__

- `n == nums.length`
- `2 <= n <= 10 ^ 5`
- `1 <= nums[i] <= limit <= 10 ^ 5`
- `n` 是偶数。

__思路:__

```text
差分数组
假设 result[k] 表示要求 nums[i] + nums[n - i - 1] = k 的操作数
由数据范围可得, 2 <= k <= 2 * limit
我们可以用一个长度为 2 * limit + 1 的数组来记录
如果 nums[i] + nums[n - i - 1] = k, 那么 result[k] += 0
如果 1 + min(nums[i], nums[n - i - 1]) <= k <= limit + max(nums[i], nums[n - i - 1]), 那么 result[k] += 1, 仅需要一次修改就能满足要求
否则需要两次修改
对每一组 nums[i] 和 nums[n - i - 1], 先将 result[k] 都加上 2
然后对于 [1 + min(nums[i], nums[n - i - 1]), limit + max(nums[i], nums[n - i - 1])] 区间内, 将 result[k] 都减去 1
最后将 nums[i] + nums[n - i - 1] 位置上的再减去 1, 即为 nums[i] + nums[n - i - 1] 这一组需要的最少操作次数
将所有数堆都遍历一边, 取最小值即可
这种算法时间复杂度为 O(NM), 需要对每一对进行 M 次操作, M 为 limit 的上限
可以使用差分数组来优化
设 delta[k] = result[k] - result[k - 1] 则 sum(delta[:i]) 就是 result[i] 的值
对于每一组 nums[i] 和 nums[n - i - 1], 先将 delta[1 + min(nums[i], nums[n - i - 1])] 减去 1
将 delta[nums[i] + nums[n - i - 1]] 减去 1
将 delta[nums[i] + nums[n - i - 1] + 1] 加上 1
将 delta[limit + max(nums[i], nums[n - i - 1]) + 1] 加上 1
这样就完成了差分数组的初始化
然后累加 delta 数组取最小的累加和即可
由于 delta[2] 每次都需要加上 2, 可以用初始值 n 代替其的求和, 求和范围为 [2, limit * 2]
时间复杂度为 O(N + M), 空间复杂度为 O(M), M 为 limit 的上限
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minMoves(vector<int>& nums, int limit) 
    {
        int n = nums.size(), delta[(limit << 1) + 2], cur = n, result = n;
        memset(delta, 0, sizeof delta);
        for (int i = 0; i < (n >> 1); i++) 
        {
            --delta[1 + min(nums[i], nums[n - i - 1])];
            --delta[nums[i] + nums[n - i - 1]];
            ++delta[nums[i] + nums[n - i - 1] + 1];
            ++delta[limit + max(nums[i], nums[n - i - 1]) + 1];
        }
        for (int i = 2; i <= (limit << 1); i++) 
        {
            cur += delta[i];
            result = min(result, cur);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minMoves(int[] nums, int limit) {
        int n = nums.length, delta[] = new int[(limit << 1) + 2], cur = n, result = n;
        for (int i = 0; i < (n >> 1); i++) {
            --delta[1 + Math.min(nums[i], nums[n - i - 1])];
            --delta[nums[i] + nums[n - i - 1]];
            ++delta[nums[i] + nums[n - i - 1] + 1];
            ++delta[limit + Math.max(nums[i], nums[n - i - 1]) + 1];
        }
        for (int i = 2; i <= (limit << 1); i++) {
            cur += delta[i];
            result = Math.min(result, cur);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minMoves(self, nums: List[int], limit: int) -> int:
        d, freq, result, c = [0] * ((limit << 1) + 2), defaultdict(int), (n := len(nums)) << 1, 0
        for i in range(n >> 1):
            freq[nums[i] + nums[n - i - 1]] += 1
            d[1 + min(nums[i], nums[n - i - 1])] += 1
            d[limit + max(nums[i], nums[n - i - 1]) + 1] -= 1
        for k in range(2, (limit << 1) + 1):
            c += d[k]
            c1, c2 = c - freq[k], (n >> 1)- c
            result = min(result, c1 + (c2 << 1))
        return result
```
