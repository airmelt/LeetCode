# 2386 Find the K-Sum of an Array 找出数组的第 K 大和

__Description:__

You are given an integer array `nums` and a __positive__ integer `k`. You can choose any __subsequence__ of the array and sum all of its elements together.

We define the __K-Sum__ of the array as the `k ^ th` __largest__ subsequence sum that can be obtained (__not__ necessarily distinct).

Return the K-Sum of the array.

A subsequence is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

__Note__ that the empty subsequence is considered to have a sum of `0`.

__Example:__

Example 1:

```text
Input: nums = [2,4,-2], k = 5
Output: 2
Explanation: All the possible subsequence sums that we can obtain are the following sorted in decreasing order:
- 6, 4, 4, 2, 2, 0, 0, -2.
The 5-Sum of the array is 2.
```

Example 2:

```text
Input: nums = [1,-2,3,4,-10,12], k = 16
Output: 10
Explanation: The 16-Sum of the array is 10.
```

__Constraints:__

- `n == nums.length`
- `1 <= n <= 10 ^ 5`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`
- `1 <= k <= min(2000, 2 ^ n)`

__题目描述:__

给你一个整数数组 `nums` 和一个 __正__ 整数 `k` 。你可以选择数组的任一 __子序列__ 并且对其全部元素求和。

数组的 __第 k 大和__ 定义为:可以获得的第 `k` 个 __最大__ 子序列和（子序列和允许出现重复）

返回数组的 第 k 大和 。

子序列是一个可以由其他数组删除某些或不删除元素派生而来的数组，且派生过程不改变剩余元素的顺序。

__注意:__空子序列的和视作 `0` 。

__示例:__

示例 1：

```text
输入：nums = [2,4,-2], k = 5
输出：2
解释：所有可能获得的子序列和列出如下，按递减顺序排列：
6、4、4、2、2、0、0、-2
数组的第 5 大和是 2 。
```

示例 2：

```text
输入：nums = [1,-2,3,4,-10,12], k = 16
输出：10
解释：数组的第 16 大和是 10 。
```

__提示：__

- `n == nums.length`
- `1 <= n <= 10 ^ 5`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`
- `1 <= k <= min(2000, 2 ^ n)`

__思路:__

```text
优先队列
如果数组中的元素都是非负数
例如 [1, 2, 3]
数组总和为 6 一定是最大的子序列的和, 这个子序列为第 2 ^ n - 1 个子序列
则最小的子序列和为 0, 这个子序列为第 0 个子序列, 为空子序列
第 1 小的子序列就是 nums[0]
第 2 小的子序列就是 nums[1], 这个子序列是第 1 个子序列的最后一个数被 nums[1] 替换
[1] 还可以添加 2, 作为一个新的子序列
总而言之, 子序列可以通过添加或者替换的方式生成
如果数组中有负数, 则可以通过添加负数的绝对值, 替换正数的方式生成子序列
此时最大的子序列和仍然为所有非负数的和
从 0 开始取下一个子序列的时候应该加上最小的正数或者减去最大的负数, 可以统一为加上绝对值最小的数
每次加入序列的时候要么替换, 要么添加
替换时 cur + nums[i] - nums[i - 1]
添加时 cur + nums[i]
将当前的元素和以及下标加入优先队列, 从 0 开始取 k - 1 次, 最后返回总和减去队首中的和
时间复杂度为 O(NlogN + KlogK), 空间复杂度为 O(K)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long kSum(vector<int>& nums, int k) 
    {
        int n = nums.size();
        long long s = 0LL;
        for (int i = 0; i < n; i++) 
        {
            if (nums[i] < 0) nums[i] *= -1;
            else s += nums[i];
        }
        sort(nums.begin(), nums.end());
        priority_queue<pair<long long, int>, vector<pair<long long, int>>, greater<>> pq;
        pq.emplace(0LL, 0);
        while (--k) 
        {
            auto [cur, i] = pq.top();
            pq.pop();
            if (i < n) 
            {
                pq.emplace(cur + nums[i], i + 1);
                if (i) pq.emplace(cur + nums[i] - nums[i - 1], i + 1);
            }
        }
        return s - pq.top().first;
    }
};
```

__Java__:

```Java
class Solution {
    public long kSum(int[] nums, int k) {
        int n = nums.length;
        long s = 0L;
        for (int i = 0; i < n; i++) {
            if (nums[i] < 0) nums[i] *= -1;
            else s += nums[i];
        }
        Arrays.sort(nums);
        PriorityQueue<Pair<Long, Integer>> pq = new PriorityQueue<>((a, b) -> Long.compare(a.getKey(), b.getKey()));
        pq.offer(new Pair<>(0L, 0));
        while (--k > 0) {
            long cur = pq.peek().getKey();
            int i = pq.poll().getValue();
            if (i < n) {
                pq.offer(new Pair<>(cur + nums[i], i + 1));
                if (i > 0) pq.offer(new Pair<>(cur + nums[i] - nums[i - 1], i + 1));
            }
        }
        return s - pq.peek().getKey();
    }
}
```

__Python__:

```Python
class Solution:
    def kSum(self, nums: List[int], k: int) -> int:
        s, nums, h = sum(num for num in nums if num > -1), sorted(list(map(abs, nums))), [(0, 0)]
        for _ in range(k - 1):
            cur, i = heappop(h)
            if i < len(nums):
                heappush(h, (cur + nums[i], i + 1))
                if i:
                    heappush(h, (cur + nums[i] - nums[i - 1], i + 1))
        return s - h[0][0]
```
