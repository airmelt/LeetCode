# 1962 Remove Stones to Minimize the Total 移除石子使总数最小

__Description:__

You are given a __0-indexed__ integer array `piles`, where `piles[i]` represents the number of stones in the `i ^ th` pile, and an integer `k`. You should apply the following operation __exactly__ `k` times:

- Choose any `piles[i]` and __remove__ `floor(piles[i] / 2)` stones from it.

Notice that you can apply the operation on the same pile more than once.

Return _the __minimum__ possible total number of stones remaining after applying the_ `k` _operations_.

`floor(x)` is the _greatest_ integer that is __smaller__ than or __equal__ to `x` (i.e., rounds `x` down).

__Example:__

Example 1:

```text
Input: piles = [5,4,9], k = 2
Output: 12
Explanation: Steps of a possible scenario are:
- Apply the operation on pile 2. The resulting piles are [5,4,5].
- Apply the operation on pile 0. The resulting piles are [3,4,5].
The total number of stones in [3,4,5] is 12.
```

Example 2:

```text
Input: piles = [4,3,6,7], k = 3
Output: 12
Explanation: Steps of a possible scenario are:
- Apply the operation on pile 2. The resulting piles are [4,3,3,7].
- Apply the operation on pile 3. The resulting piles are [4,3,3,4].
- Apply the operation on pile 0. The resulting piles are [2,3,3,4].
The total number of stones in [2,3,3,4] is 12.
```

__Constraints:__

- `1 <= piles.length <= 10 ^ 5`
- `1 <= piles[i] <= 10 ^ 4`
- `1 <= k <= 10 ^ 5`

__题目描述:__

给你一个整数数组 `piles` ，数组 __下标从 0 开始__ ，其中 `piles[i]` 表示第 `i` 堆石子中的石子数量。另给你一个整数 `k` ，请你执行下述操作 __恰好__ `k` 次:

- 选出任一石子堆 `piles[i]` ，并从中 __移除__ `floor(piles[i] / 2)` 颗石子。

注意：你可以对 同一堆 石子多次执行此操作。

返回执行 `k` 次操作后，剩下石子的 __最小__ 总数。

`floor(x)` 为 __小于__ 或 __等于__ `x` 的 __最大__ 整数。（即，对 `x` 向下取整）。

__示例:__

示例 1：

```text
输入：piles = [5,4,9], k = 2
输出：12
解释：可能的执行情景如下：
- 对第 2 堆石子执行移除操作，石子分布情况变成 [5,4,5] 。
- 对第 0 堆石子执行移除操作，石子分布情况变成 [3,4,5] 。
剩下石子的总数为 12 。
```

示例 2：

```text
输入：piles = [4,3,6,7], k = 3
输出：12
解释：可能的执行情景如下：
- 对第 2 堆石子执行移除操作，石子分布情况变成 [4,3,3,7] 。
- 对第 3 堆石子执行移除操作，石子分布情况变成 [4,3,3,4] 。
- 对第 0 堆石子执行移除操作，石子分布情况变成 [2,3,3,4] 。
剩下石子的总数为 12 。
```

__提示：__

- `1 <= piles.length <= 10 ^ 5`
- `1 <= piles[i] <= 10 ^ 4`
- `1 <= k <= 10 ^ 5`

__思路:__

```text
大顶堆
每次取出最大的石子堆, 减去一半后重新放入堆中
重复 k 次后, 返回所有石子堆的和
或者堆顶为 1 时停止
时间复杂度为 O(N + KlogN), 空间复杂度为 O(1), 可以用原地堆化
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minStoneSum(vector<int> &piles, int k) 
    {
        make_heap(piles.begin(), piles.end());
        while (k-- and piles.front() != 1) 
        {
            pop_heap(piles.begin(), piles.end());
            piles.back() -= piles.back() >> 1;
            push_heap(piles.begin(), piles.end());
        }
        return accumulate(piles.begin(), piles.end(), 0);
    }
};
```

__Java__:

```Java
class Solution {
    public int minStoneSum(int[] piles, int k) {
        int result = 0, n = piles.length;
        heapify(piles);
        while (k-- > 0 && piles[0] != 1) {
            piles[0] -= piles[0] >> 1;
            sink(piles, 0, n);
        }
        for (int p : piles) result += p;
        return result;
    }

    private void heapify(int[] heap) {
        for (int n = heap.length, i = (n >> 1) - 1; i > -1; i--) sink(heap, i, n);
    }

    private void sink(int[] heap, int i, int n) {
        while (2 * i + 1 < n) {
            int j = 2 * i + 1;
            if (j + 1 < n && heap[j + 1] > heap[j]) ++j;
            if (heap[j] <= heap[i]) break;
            swap(heap, i, j);
            i = j;
        }
    }

    private void swap(int[] heap, int i, int j) {
        heap[i] ^= heap[j];
        heap[j] ^= heap[i];
        heap[i] ^= heap[j];
    }
}
```

__Python__:

```Python
class Solution:
    def minStoneSum(self, piles: List[int], k: int) -> int:
        heapify(piles := [-p for p in piles])
        while k and piles[0] != -1:
            heapreplace(piles, piles[0] >> 1)
            k -= 1
        return -sum(piles)
```
