# 3049 Earliest Second to Mark Indices II 标记所有下标的最早秒数 II

__Description:__

You are given two __1-indexed__ integer arrays, `nums` and, `changeIndices`, having lengths `n` and `m`, respectively.

Initially, all indices in `nums` are unmarked. Your task is to mark __all__ indices in `nums`.

In each second, `s`, in order from `1` to `m` (__inclusive__), you can perform __one__ of the following operations:

- Choose an index `i` in the range `[1, n]` and __decrement__ `nums[i]` by `1`.
- Set `nums[changeIndices[s]]` to any __non-negative__ value.
- Choose an index `i` in the range `[1, n]`, where `nums[i]` is __equal__ to `0`, and __mark__ index `i`.
- Do nothing.

Return _an integer denoting the __earliest second__ in the range_ `[1, m]` _when __all__ indices in_ `nums` _can be marked by choosing operations optimally, or_ `-1` _if it is impossible._

__Example:__

Example 1:

```text
Input: nums = [3,2,3], changeIndices = [1,3,2,2,2,2,3]
Output: 6
Explanation: In this example, we have 7 seconds. The following operations can be performed to mark all indices:
Second 1: Set nums[changeIndices[1]] to 0. nums becomes [0,2,3].
Second 2: Set nums[changeIndices[2]] to 0. nums becomes [0,2,0].
Second 3: Set nums[changeIndices[3]] to 0. nums becomes [0,0,0].
Second 4: Mark index 1, since nums[1] is equal to 0.
Second 5: Mark index 2, since nums[2] is equal to 0.
Second 6: Mark index 3, since nums[3] is equal to 0.
Now all indices have been marked.
It can be shown that it is not possible to mark all indices earlier than the 6th second.
Hence, the answer is 6.
```

Example 2:

```text
Input: nums = [0,0,1,2], changeIndices = [1,2,1,2,1,2,1,2]
Output: 7
Explanation: In this example, we have 8 seconds. The following operations can be performed to mark all indices:
Second 1: Mark index 1, since nums[1] is equal to 0.
Second 2: Mark index 2, since nums[2] is equal to 0.
Second 3: Decrement index 4 by one. nums becomes [0,0,1,1].
Second 4: Decrement index 4 by one. nums becomes [0,0,1,0].
Second 5: Decrement index 3 by one. nums becomes [0,0,0,0].
Second 6: Mark index 3, since nums[3] is equal to 0.
Second 7: Mark index 4, since nums[4] is equal to 0.
Now all indices have been marked.
It can be shown that it is not possible to mark all indices earlier than the 7th second.
Hence, the answer is 7.
```

Example 3:

```text
Input: nums = [1,2,3], changeIndices = [1,2,3]
Output: -1
Explanation: In this example, it can be shown that it is impossible to mark all indices, as we don't have enough seconds. 
Hence, the answer is -1.
```

__Constraints:__

- `1 <= n == nums.length <= 5000`
- `0 <= nums[i] <= 10 ^ 9`
- `1 <= m == changeIndices.length <= 5000`
- `1 <= changeIndices[i] <= n`

__题目描述:__

给你两个下标从 __1__ 开始的整数数组 `nums` 和 `changeIndices` ，数组的长度分别为 `n` 和 `m` 。

一开始， `nums` 中所有下标都是未标记的，你的任务是标记 `nums` 中 __所有__ 下标。

从第 `1` 秒到第 `m` 秒（ _包括_ 第 `m` 秒），对于每一秒 `s` ，你可以执行以下操作 __之一__ :

- 选择范围 `[1, n]` 中的一个下标 `i` ，并且将 `nums[i]` __减少__ `1` 。
- 将 `nums[changeIndices[s]]` 设置成任意的 __非负__ 整数。
- 选择范围 `[1, n]` 中的一个下标 `i` ， 满足 `nums[i]` __等于__ `0`, 并 __标记__ 下标 `i` 。
- 什么也不做。

请你返回范围 `[1, m]` 中的一个整数，表示最优操作下，标记 `nums` 中 __所有__ 下标的 __最早秒数__ ，如果无法标记所有下标，返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums = [3,2,3], changeIndices = [1,3,2,2,2,2,3]
输出：6
解释：这个例子中，我们总共有 7 秒。按照以下操作标记所有下标：
第 1 秒：将 nums[changeIndices[1]] 变为 0 。nums 变为 [0,2,3] 。
第 2 秒：将 nums[changeIndices[2]] 变为 0 。nums 变为 [0,2,0] 。
第 3 秒：将 nums[changeIndices[3]] 变为 0 。nums 变为 [0,0,0] 。
第 4 秒：标记下标 1 ，因为 nums[1] 等于 0 。
第 5 秒：标记下标 2 ，因为 nums[2] 等于 0 。
第 6 秒：标记下标 3 ，因为 nums[3] 等于 0 。
现在所有下标已被标记。
最早可以在第 6 秒标记所有下标。
所以答案是 6 。
```

示例 2：

```text
输入：nums = [0,0,1,2], changeIndices = [1,2,1,2,1,2,1,2]
输出：7
解释：这个例子中，我们总共有 8 秒。按照以下操作标记所有下标：
第 1 秒：标记下标 1 ，因为 nums[1] 等于 0 。
第 2 秒：标记下标 2 ，因为 nums[2] 等于 0 。
第 3 秒：将 nums[4] 减少 1 。nums 变为 [0,0,1,1] 。
第 4 秒：将 nums[4] 减少 1 。nums 变为 [0,0,1,0] 。
第 5 秒：将 nums[3] 减少 1 。nums 变为 [0,0,0,0] 。
第 6 秒：标记下标 3 ，因为 nums[3] 等于 0 。
第 7 秒：标记下标 4 ，因为 nums[4] 等于 0 。
现在所有下标已被标记。
最早可以在第 7 秒标记所有下标。
所以答案是 7 。
```

示例 3：

```text
输入：nums = [1,2,3], changeIndices = [1,2,3]
输出：-1
解释：这个例子中，无法标记所有下标，因为我们没有足够的秒数。
所以答案是 -1 。
```

__提示：__

- `1 <= n == nums.length <= 5000`
- `0 <= nums[i] <= 10 ^ 9`
- `1 <= m == changeIndices.length <= 5000`
- `1 <= changeIndices[i] <= n`

__思路:__

```text
二分查找 ➕ 贪心
当 nums[i] 大于 1 的情况使用修改之后再标记会比慢慢减少 nums[i] 更快
但是如果 changeIndices 中出现 nums[i] 特别晚可能来不及标记
找到 nums[i] 第一次出现的位置记录在 first_t 中
从 mid - 1 开始倒序遍历
如果令 i = changeIndices[t] - 1, v = nums[i]
如果 v <= 1 或者 t != first_t[i] 先不对改元素进行操作, ++study
否则如果 study > 0, 尝试用修改的方式, 那么此时 slow 需要减去 v + 1, 即 nums[i] 以及 1 次标记, --study 并把 nums[i] 加入小根堆
如果 study == 0, 在小根堆中选一个更小的反悔, 如果小根堆为空或者 v 比小根堆的堆顶还小就不反悔
否则弹出小根堆顶, slow 加上 v + 1, study - 2, 因为需要两天时间来清零以及标记
加回去是用堆顶的逐项减少来代替当前的清零和标记
时间复杂度为 O(MlogMlogN), 空间复杂度为 O(N), M 为数组 changeIndices 的长度, N 为数组 nums 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int earliestSecondToMarkIndices(vector<int>& nums, vector<int>& changeIndices) 
    {
        int n = nums.size(), m = changeIndices.size(), left = n - 1, right = m + 1, mid = 0;
        if (n > m) return -1;
        long long slow = accumulate(nums.begin(), nums.end(), 0LL) + n;
        vector<int> first_t(n);
        for (int t = m - 1; t > -1; t--) first_t[changeIndices[t] - 1] = t;
        auto check = [&](int mid, auto slow) -> bool
        {
            priority_queue<int, vector<int>, greater<>> pq;
            int study = 0;
            for (int t = mid - 1, i = 0, v = 0; t > -1; t--)
            {
                if ((v = nums[i = changeIndices[t] - 1]) <= 1 or t != first_t[i])
                {
                    ++study;
                    continue;
                }
                if (!study)
                {
                    if (pq.empty() or v <= pq.top())
                    {
                        ++study;
                        continue;
                    }
                    slow += pq.top() + 1;
                    pq.pop();
                    study += 2;
                }
                slow -= v + 1;
                --study;
                pq.push(v);
            }
            return study >= slow;
        };
        while (left + 1 < right) (check(mid, slow) ? right : left) = (mid = left + ((right - left) >> 1));
        return right > m ? -1 : right;
    }
};
```

__Java__:

```Java
class Solution {
    public int earliestSecondToMarkIndices(int[] nums, int[] changeIndices) {
        int n = nums.length, m = changeIndices.length, firstT[] = new int[n], left = n - 1, right = m + 1, mid = 0;
        if (n > m) return -1;
        Arrays.fill(firstT, -1);
        for (int t = m - 1; t > -1; t--) firstT[changeIndices[t] - 1] = t;
        long total = Arrays.stream(nums).asLongStream().sum() + n;
        while (left + 1 < right) {
            if (check(mid = left + ((right - left) >>> 1), nums, changeIndices, firstT, total)) right = mid;
            else left = mid;
        }
        return right > m ? -1 : right;
    }

    private boolean check(int mid, int[] nums, int[] changeIndices, int[] firstT, long slow) {
        int study = 0;
        PriorityQueue<Integer> pq = new PriorityQueue<>((a, b) -> a - b);
        for (int t = mid - 1, i = 0, v = 0; t >= 0; t--) {
            if ((v = nums[i = changeIndices[t] - 1]) <= 1 || t != firstT[i]) {
                ++study;
                continue;
            }
            if (study == 0) {
                if (pq.isEmpty() || v <= pq.peek()) {
                    ++study;
                    continue;
                }
                slow += pq.poll() + 1;
                study += 2;
            }
            slow -= v + 1;
            --study;
            pq.offer(v);
        }
        return study >= slow;
    }
}
```

__Python__:

```Python
class Solution:
    def earliestSecondToMarkIndices(self, nums: List[int], changeIndices: List[int]) -> int:
        first_t, m, total = [-1] * (n := len(nums)), len(changeIndices), n + sum(nums)
        for t in range(m - 1, -1, -1):
            first_t[changeIndices[t] - 1] = t

        def check(mid: int) -> bool:
            slow, study, heap = total, 0, []
            for t in range(mid - 1, -1, -1):
                if (v := nums[i := changeIndices[t] - 1]) <= 1 or t != first_t[i]:
                    study += 1
                    continue
                if not study:
                    if not heap or v <= heap[0]:
                        study += 1
                        continue
                    slow += heappop(heap) + 1
                    study += 2
                slow -= v + 1
                study -= 1
                heappush(heap, v)
            return study >= slow
        return -1 if (result := n + bisect_left(range(n, m + 1), True, key=check)) > m else result
```
