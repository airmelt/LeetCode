# 2542 Maximum Subsequence Score 最大子序列的分数

__Description:__

You are given two __0-indexed__ integer arrays `nums1` and `nums2` of equal length `n` and a positive integer `k`. You must choose a __subsequence__ of indices from `nums1` of length `k`.

For chosen indices `i0`, `i1`, ..., `ik - 1`, your __score__ is defined as:

- The sum of the selected elements from `nums1` multiplied with the __minimum__ of the selected elements from `nums2`.
- It can defined simply as: `(nums1[i0] + nums1[i1] +...+ nums1[ik - 1]) * min(nums2[i0] , nums2[i1], ... ,nums2[ik - 1])`.

Return the maximum possible score.

A __subsequence__ of indices of an array is a set that can be derived from the set `{0, 1, ..., n-1}` by deleting some or no elements.

__Example:__

Example 1:

```text
Input: nums1 = [1,3,3,2], nums2 = [2,1,3,4], k = 3
Output: 12
Explanation: 
The four possible subsequence scores are:
```

- We choose the indices 0, 1, and 2 with score = (1+3+3) * min(2,1,3) = 7.
- We choose the indices 0, 1, and 3 with score = (1+3+2) * min(2,1,4) = 6.
- We choose the indices 0, 2, and 3 with score = (1+3+2) * min(2,3,4) = 12.
- We choose the indices 1, 2, and 3 with score = (3+3+2) * min(1,3,4) = 8.

Therefore, we return the max score, which is 12.

Example 2:

```text
Input: nums1 = [4,2,3,1,1], nums2 = [7,5,10,9,6], k = 1
Output: 30
Explanation: 
Choosing index 2 is optimal: nums1[2] * nums2[2] = 3 * 10 = 30 is the maximum possible score.
```

__Constraints:__

- `n == nums1.length == nums2.length`
- `1 <= n <= 10 ^ 5`
- `0 <= nums1[i], nums2[j] <= 10 ^ 5`
- `1 <= k <= n`

__题目描述:__

给你两个下标从 __0__ 开始的整数数组 `nums1` 和 `nums2` ，两者长度都是 `n` ，再给你一个正整数 `k` 。你必须从 `nums1` 中选一个长度为 `k` 的 __子序列__ 对应的下标。

对于选择的下标 `i0` ， `i1` ，...， `ik - 1` ，你的 __分数__ 定义如下:

- `nums1` 中下标对应元素求和，乘以 `nums2` 中下标对应元素的 __最小值__ 。
- 用公式表示: `(nums1[i0] + nums1[i1] +...+ nums1[ik - 1]) * min(nums2[i0] , nums2[i1], ... ,nums2[ik - 1])` 。

请你返回 最大 可能的分数。

一个数组的 __子序列__ 下标是集合 `{0, 1, ..., n-1}` 中删除若干元素得到的剩余集合，也可以不删除任何元素。

__示例:__

示例 1：

```text
输入：nums1 = [1,3,3,2], nums2 = [2,1,3,4], k = 3
输出：12
解释：
四个可能的子序列分数为：
```

- 选择下标 0 ，1 和 2 ，得到分数 (1+3+3) * min(2,1,3) = 7 。
- 选择下标 0 ，1 和 3 ，得到分数 (1+3+2) * min(2,1,4) = 6 。
- 选择下标 0 ，2 和 3 ，得到分数 (1+3+2) * min(2,3,4) = 12 。
- 选择下标 1 ，2 和 3 ，得到分数 (3+3+2) * min(1,3,4) = 8 。

所以最大分数为 12 。

示例 2：

```text
输入：nums1 = [4,2,3,1,1], nums2 = [7,5,10,9,6], k = 1
输出：30
解释：
选择下标 2 最优：nums1[2] * nums2[2] = 3 * 10 = 30 是最大可能分数。
```

__提示：__

- `n == nums1.length == nums2.length`
- `1 <= n <= 10 ^ 5`
- `0 <= nums1[i], nums2[j] <= 10 ^ 5`
- `1 <= k <= n`

__思路:__

```text
优先队列
将 nums2 中的元素按照降序排列, 记录下对应的 nums1 的下标
将前 k 个下标对应的 nums1 元素加入优先队列, 同时计算当前的和
结果初始化为当前和乘以 nums2 中第 k - 1 个下标对应的元素, 即当前 nums2 的最小值
从 k 开始遍历 nums2 中的元素, 如果当前下标对应的 nums1 元素大于优先队列中的最小值, 则将其加入优先队列
同时更新当前和, 并计算新的结果
如果新的结果大于当前结果, 则更新结果
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maxScore(vector<int>& nums1, vector<int>& nums2, int k) 
    {
        vector<int> idx(nums1.size());
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) { return nums2[i] > nums2[j]; });
        long long cur = 0LL;
        priority_queue<int> pq;
        for (int i = 0; i < k; i++) 
        {
            pq.push(-nums1[idx[i]]);
            cur += nums1[idx[i]];
        }
        long long result = cur * nums2[idx[k - 1]];
        for (int i = k, n = nums1.size(); i < n; i++) 
        {
            if (nums1[idx[i]] > -pq.top()) 
            {
                cur += nums1[idx[i]] + pq.top();
                pq.pop();
                pq.push(-nums1[idx[i]]);
                result = max(result, cur * nums2[idx[i]]);
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long maxScore(int[] nums1, int[] nums2, int k) {
        var idx = new Integer[nums1.length];
        for (int i = 0, n = nums1.length; i < n; i++) idx[i] = i;
        Arrays.sort(idx, (i, j) -> nums2[j] - nums2[i]);
        var cur = 0L;
        var pq = new PriorityQueue<Integer>();
        for (int i = 0; i < k; i++) {
            pq.offer(nums1[idx[i]]);
            cur += nums1[idx[i]];
        }
        var result = cur * nums2[idx[k - 1]];
        for (int i = k, n = nums1.length; i < n; i++) {
            if (nums1[idx[i]] > pq.peek()) {
                cur += nums1[idx[i]] - pq.poll();
                pq.offer(nums1[idx[i]]);
                result = Math.max(result, cur * nums2[idx[i]]);
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxScore(self, nums1: List[int], nums2: List[int], k: int) -> int:
        pairs = sorted(zip(nums1, nums2), key=lambda x: -x[1])
        heap = [x for x, _ in pairs[:k]]
        heapify(heap)
        result = (cur := sum(heap)) * pairs[k - 1][1]
        for x, y in pairs[k:]:
            if x > heap[0]:
                result = max(result, (cur := cur + x - heapreplace(heap, x)) * y)
        return result
```
