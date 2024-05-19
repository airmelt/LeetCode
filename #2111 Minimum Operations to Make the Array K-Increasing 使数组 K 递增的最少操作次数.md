# 2111 Minimum Operations to Make the Array K-Increasing 使数组 K 递增的最少操作次数

__Description:__

You are given a __0-indexed__ array `arr` consisting of `n` positive integers, and a positive integer `k`.

The array `arr` is called __K-increasing__ if `arr[i-k] <= arr[i]` holds for every index `i`, where `k <= i <= n-1`.

- For example, `arr = [4, 1, 5, 2, 6, 2]` is K-increasing for `k = 2` because:

- `arr[0] <= arr[2] (4 <= 5)`
- `arr[1] <= arr[3] (1 <= 2)`
- `arr[2] <= arr[4] (5 <= 6)`
- `arr[3] <= arr[5] (2 <= 2)`

- However, the same `arr` is not K-increasing for `k = 1` (because `arr[0] > arr[1]`) or `k = 3` (because `arr[0] > arr[3]`).

- `arr[0] <= arr[2] (4 <= 5)`
- `arr[1] <= arr[3] (1 <= 2)`
- `arr[2] <= arr[4] (5 <= 6)`
- `arr[3] <= arr[5] (2 <= 2)`

In one __operation__, you can choose an index `i` and __change__ `arr[i]` into __any__ positive integer.

Return _the __minimum number of operations__ required to make the array K-increasing for the given_ `k`.

__Example:__

Example 1:

```text
Input: arr = [5,4,3,2,1], k = 1
Output: 4
Explanation:
For k = 1, the resultant array has to be non-decreasing.
Some of the K-increasing arrays that can be formed are [5,6,7,8,9], [1,1,1,1,1], [2,2,3,4,4]. All of them require 4 operations.
It is suboptimal to change the array to, for example, [6,7,8,9,10] because it would take 5 operations.
It can be shown that we cannot make the array K-increasing in less than 4 operations.
```

Example 2:

```text
Input: arr = [4,1,5,2,6,2], k = 2
Output: 0
Explanation:
This is the same example as the one in the problem description.
Here, for every index i where 2 <= i <= 5, arr[i-2] <= arr[i].
Since the given array is already K-increasing, we do not need to perform any operations.
```

Example 3:

```text
Input: arr = [4,1,5,2,6,2], k = 3
Output: 2
Explanation:
Indices 3 and 5 are the only ones not satisfying arr[i-3] <= arr[i] for 3 <= i <= 5.
One of the ways we can make the array K-increasing is by changing arr[3] to 4 and arr[5] to 5.
The array will now be [4,1,5,4,6,5].
Note that there can be other ways to make the array K-increasing, but none of them require less than 2 operations.
```

__Constraints:__

- `1 <= arr.length <= 10 ^ 5`
- `1 <= arr[i], k <= arr.length`

__题目描述:__

给你一个下标从 __0__ 开始包含 `n` 个正整数的数组 `arr` ，和一个正整数 `k` 。

如果对于每个满足 `k <= i <= n-1` 的下标 `i` ，都有 `arr[i-k] <= arr[i]` ，那么我们称 `arr` 是 __K__ __递增__ 的。

- 比方说， `arr = [4, 1, 5, 2, 6, 2]` 对于 `k = 2` 是 K 递增的，因为:

- `arr[0] <= arr[2] (4 <= 5)`
- `arr[1] <= arr[3] (1 <= 2)`
- `arr[2] <= arr[4] (5 <= 6)`
- `arr[3] <= arr[5] (2 <= 2)`

- 但是，相同的数组 `arr` 对于 `k = 1` 不是 K 递增的（因为 `arr[0] > arr[1]`），对于 `k = 3` 也不是 K 递增的（因为 `arr[0] > arr[3]` ）。

- `arr[0] <= arr[2] (4 <= 5)`
- `arr[1] <= arr[3] (1 <= 2)`
- `arr[2] <= arr[4] (5 <= 6)`
- `arr[3] <= arr[5] (2 <= 2)`

每一次 __操作__ 中，你可以选择一个下标 `i` 并将 `arr[i]` __改成任意__ 正整数。

请你返回对于给定的 `k` ，使数组变成 K 递增的 __最少操作次数__ 。

__示例:__

示例 1：

```text
输入：arr = [5,4,3,2,1], k = 1
输出：4
解释：
对于 k = 1 ，数组最终必须变成非递减的。
可行的 K 递增结果数组为 [5,6,7,8,9]，[1,1,1,1,1]，[2,2,3,4,4] 。它们都需要 4 次操作。
次优解是将数组变成比方说 [6,7,8,9,10] ，因为需要 5 次操作。
显然我们无法使用少于 4 次操作将数组变成 K 递增的。
```

示例 2：

```text
输入：arr = [4,1,5,2,6,2], k = 2
输出：0
解释：
这是题目描述中的例子。
对于每个满足 2 <= i <= 5 的下标 i ，有 arr[i-2] <= arr[i] 。
由于给定数组已经是 K 递增的，我们不需要进行任何操作。
```

示例 3：

```text
输入：arr = [4,1,5,2,6,2], k = 3
输出：2
解释：
下标 3 和 5 是仅有的 3 <= i <= 5 且不满足 arr[i-3] <= arr[i] 的下标。
将数组变成 K 递增的方法之一是将 arr[3] 变为 4 ，且将 arr[5] 变成 5 。
数组变为 [4,1,5,4,6,5] 。
可能有其他方法将数组变为 K 递增的，但没有任何一种方法需要的操作次数小于 2 次。
```

__提示：__

- `1 <= arr.length <= 10 ^ 5`
- `1 <= arr[i], k <= arr.length`

__思路:__

```text
动态规划
注意到 k 组数组之间是独立的, 因此可以分别处理每一组数组
对于每一组数组, 用一个数组 cur 记录当前的最长递增子序列(LIS)
令 step = (n - i) / k + ((n - i) % k != 0 ? 1 : 0), 即当前组数组的长度
遍历数组, 对于每个元素, 用二分查找找到 arr[i + j * k] 在 cur 中的位置 pos, 由于求的是不严格递增, 因此 pos 为 upper_bound/bisect_right
如果 pos == cur.size(), 则将其添加到 cur 的末尾
否则将 cur[pos] = arr[i + j * k]
然后累加 step - cur.size()
时间复杂度为 O(NlogC), 空间复杂度为 O(C), 其中 C = n / k
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int kIncreasing(vector<int>& arr, int k) 
    {
        int n = arr.size(), result = 0;
        for (int i = 0; i < k; i++)
        {
            int step = (n - i) / k + (!!((n - i) % k)), pos = 0;
            vector<int> cur;
            for (int j = 0; j < step; j++)
            {
                if ((pos = (upper_bound(cur.begin(), cur.end(), arr[i + j * k]) - cur.begin())) == cur.size()) cur.emplace_back(arr[i + j * k]);
                else cur[pos] = arr[i + j * k];
            }
            result += step - cur.size();
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int kIncreasing(int[] arr, int k) {
        int n = arr.length, result = 0;
        for (int i = 0; i < k; i++) {
            int step = (n - i) / k + ((n - i) % k != 0 ? 1 : 0), nums[] = new int[step];
            for (int j = 0; j < step; j++) nums[j] = arr[i + (j * k)];
            result += step - helper(nums);
        }
        return result;
    }

    private int helper(int[] nums) {
        int len = 1, n = nums.length, dp[] = new int[n + 1];
        dp[len] = nums[0];
        for (int i = 1; i < n; i++) {
            if (nums[i] >= dp[len]) dp[++len] = nums[i];
            else {
                int l = 1, r = len, pos = 0;
                while (l <= r) {
                    int mid = (l + r) >> 1;
                    if (dp[mid] <= nums[i]) {
                        pos = mid;
                        l = mid + 1;
                    } else r = mid - 1;
                }
                dp[pos + 1] = nums[i];
            }
        }
        return len;
    }
}
```

__Python__:

```Python
class Solution:
    def kIncreasing(self, arr: List[int], k: int) -> int:
        n, result = len(arr), 0
        for i in range(k):
            cur, step = [], (n - i) // k + (not not (n - i) % k)
            for j in range(step):
                if (pos := bisect_right(cur, arr[i + j * k])) == len(cur):
                    cur.append(arr[i + j * k])
                else:
                    cur[pos] = arr[i + j * k]
            result += step  - len(cur)
        return result
```
