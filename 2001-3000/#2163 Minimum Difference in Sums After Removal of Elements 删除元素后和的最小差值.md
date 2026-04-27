# 2163 Minimum Difference in Sums After Removal of Elements 删除元素后和的最小差值

__Description:__

You are given a __0-indexed__ integer array `nums` consisting of `3 * n` elements.

You are allowed to remove any __subsequence__ of elements of size __exactly__ `n` from `nums`. The remaining `2 * n` elements will be divided into two __equal__ parts:

- The first `n` elements belonging to the first part and their sum is `sumfirst`.
- The next `n` elements belonging to the second part and their sum is `sumsecond`.

The __difference in sums__ of the two parts is denoted as `sumfirst - sumsecond`.

- For example, if `sumfirst = 3` and `sumsecond = 2`, their difference is `1`.
- Similarly, if `sumfirst = 2` and `sumsecond = 3`, their difference is `-1`.

Return _the __minimum difference__ possible between the sums of the two parts after the removal of_ `n` _elements_.

__Example:__

Example 1:

```text
Input: nums = [3,1,2]
Output: -1
Explanation: Here, nums has 3 elements, so n = 1. 
Thus we have to remove 1 element from nums and divide the array into two equal parts.
```

- If we remove nums[0] = 3, the array will be [1,2]. The difference in sums of the two parts will be 1 - 2 = -1.
- If we remove nums[1] = 1, the array will be [3,2]. The difference in sums of the two parts will be 3 - 2 = 1.
- If we remove nums[2] = 2, the array will be [3,1]. The difference in sums of the two parts will be 3 - 1 = 2.

The minimum difference between sums of the two parts is min(-1,1,2) = -1.

Example 2:

```text
Input: nums = [7,9,5,8,1,3]
Output: 1
Explanation: Here n = 2. So we must remove 2 elements and divide the remaining array into two parts containing two elements each.
If we remove nums[2] = 5 and nums[3] = 8, the resultant array will be [7,9,1,3]. The difference in sums will be (7+9) - (1+3) = 12.
To obtain the minimum difference, we should remove nums[1] = 9 and nums[4] = 1. The resultant array becomes [7,5,8,3]. The difference in sums of the two parts is (7+5) - (8+3) = 1.
It can be shown that it is not possible to obtain a difference smaller than 1.
```

__Constraints:__

- `nums.length == 3 * n`
- `1 <= n <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，它包含 `3 * n` 个元素。

你可以从 `nums` 中删除 __恰好__ `n` 个元素，剩下的 `2 * n` 个元素将会被分成两个 __相同大小__ 的部分。

- 前面 `n` 个元素属于第一部分，它们的和记为 `sumfirst` 。
- 后面 `n` 个元素属于第二部分，它们的和记为 `sumsecond` 。

两部分和的 __差值__ 记为 `sumfirst - sumsecond` 。

- 比方说， `sumfirst = 3` 且 `sumsecond = 2` ，它们的差值为 `1` 。
- 再比方， `sumfirst = 2` 且 `sumsecond = 3` ，它们的差值为 `-1` 。

请你返回删除 `n` 个元素之后，剩下两部分和的 __差值的最小值__ 是多少。

__示例:__

示例 1：

```text
输入：nums = [3,1,2]
输出：-1
解释：nums 有 3 个元素，所以 n = 1 。
所以我们需要从 nums 中删除 1 个元素，并将剩下的元素分成两部分。
```

- 如果我们删除 nums[0] = 3 ，数组变为 [1,2] 。两部分和的差值为 1 - 2 = -1 。
- 如果我们删除 nums[1] = 1 ，数组变为 [3,2] 。两部分和的差值为 3 - 2 = 1 。
- 如果我们删除 nums[2] = 2 ，数组变为 [3,1] 。两部分和的差值为 3 - 1 = 2 。

两部分和的最小差值为 min(-1,1,2) = -1 。

示例 2：

```text
输入：nums = [7,9,5,8,1,3]
输出：1
解释：n = 2 。所以我们需要删除 2 个元素，并将剩下元素分为 2 部分。
如果我们删除元素 nums[2] = 5 和 nums[3] = 8 ，剩下元素为 [7,9,1,3] 。和的差值为 (7+9) - (1+3) = 12 。
为了得到最小差值，我们应该删除 nums[1] = 9 和 nums[4] = 1 ，剩下的元素为 [7,5,8,3] 。和的差值为 (7+5) - (8+3) = 1 。
观察可知，最优答案为 1 。
```

__提示：__

- `nums.length == 3 * n`
- `1 <= n <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__思路:__

```text
优先队列
将数组分为两部分, 一部分为前 n 个元素, 一部分为后 n 个元素
即在中间 n 个元素中选择一个分界点
使得前面选择 n 个元素的和的最小值与后面选择 n 个元素的和的最大值的差值最小
即从 [n, m - n - 1] 中选择一个分界点 i, 使得 pre[i] - suf[i + 1] 最小, 其中 pre 表示 i 前选择 n 个数的和的最小值, suf 表示 i 后选择 n 个数的和的最大值
为了求出 suf, 我们先将所有 2n 之后的数加入最小堆, 每次加入一个数并弹出堆顶
计算 pre 时, 用一个大根堆记录前 n 个数, 每次加入一个数并弹出堆顶
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minimumDifference(vector<int>& nums) 
    {
        int m = nums.size(), n = m / 3;
        long long sum = 0LL, pre = 0LL, result = 0LL;
        priority_queue<int, vector<int>, greater<int>> min_pq;
        for (int i = m - n; i < m; i++)
        {
            min_pq.push(nums[i]);
            sum += nums[i];
        }
        vector<long long> suf_max(m - n + 1);
        suf_max.back() = sum;
        for (int i = m - n - 1; i >= n; i--)
        {
            min_pq.push(nums[i]);
            suf_max[i] = (sum = sum + nums[i] - min_pq.top());
            min_pq.pop();
        }
        priority_queue<int> max_pq;
        for (int i = 0; i < n; i++)
        {
            max_pq.push(nums[i]);
            pre += nums[i];
        }
        result = pre - suf_max[n];
        for (int i = n; i < m - n; i++)
        {
            max_pq.push(nums[i]);
            result = min(result, (pre = pre + nums[i] - max_pq.top()) - suf_max[i + 1]);
            max_pq.pop();
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long minimumDifference(int[] nums) {
        int m = nums.length, n = m / 3;
        PriorityQueue<Integer> minPQ = new PriorityQueue<>(), maxPQ = new PriorityQueue<Integer>(Collections.reverseOrder());
        long sum = 0L, pre = 0L, result = 0L, sufMax[] = new long[m - n + 1];
        for (int i = m - n; i < m; i++) {
            minPQ.add(nums[i]);
            sum += nums[i];
        }
        sufMax[m - n] = sum;
        for (int i = m - n - 1; i >= n; i--) {
            minPQ.add(nums[i]);
            sufMax[i] = (sum = sum + nums[i] - minPQ.poll()); 
        }
        for (int i = 0; i < n; ++i) {
            maxPQ.add(nums[i]);
            pre += nums[i];
        }
        result = pre - sufMax[n];
        for (int i = n; i < m - n; ++i) {
            maxPQ.add(nums[i]);
            result = Math.min(result, (pre = pre + nums[i] - maxPQ.poll()) - sufMax[i + 1]);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumDifference(self, nums: List[int]) -> int:
        heapify(min_pq := nums[-(n := (m := len(nums)) // 3):])
        suf_max = [0] * (m - n + 1)
        suf_max[-1] = s = sum(min_pq)
        for i in range(m - n - 1, n - 1, -1):
            suf_max[i] = (s := s + nums[i] - heappushpop(min_pq, nums[i]))
        heapify(max_pq := [-v for v in nums[:n]])
        result = (pre_min := -sum(max_pq)) - suf_max[n]
        for i in range(n, m - n):
            result = min(result, (pre_min := pre_min + nums[i] + heappushpop(max_pq, -nums[i])) - suf_max[i + 1])
        return result
```
