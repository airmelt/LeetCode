# 3013 Divide an Array Into Subarrays With Minimum Cost II 将数组分成最小总代价的子数组 II

__Description:__

You are given a __0-indexed__ array of integers `nums` of length `n`, and two __positive__ integers `k` and `dist`.

The __cost__ of an array is the value of its __first__ element. For example, the cost of `[1,2,3]` is `1` while the cost of `[3,4,1]` is `3`.

You need to divide `nums` into `k` __disjoint contiguous__ subarrays, such that the difference between the starting index of the __second__ subarray and the starting index of the `kth` subarray should be __less than or equal to__ `dist`. In other words, if you divide `nums` into the subarrays `nums[0..(i1 - 1)], nums[i1..(i2 - 1)], ..., nums[ik-1..(n - 1)]`, then `ik-1 - i1 <= dist`.

Return the minimum possible sum of the cost of these subarrays.

__Example:__

Example 1:

```text
Input: nums = [1,3,2,6,4,2], k = 3, dist = 3
Output: 5
Explanation: The best possible way to divide nums into 3 subarrays is: [1,3], [2,6,4], and [2]. This choice is valid because ik-1 - i1 is 5 - 2 = 3 which is equal to dist. The total cost is nums[0] + nums[2] + nums[5] which is 1 + 2 + 2 = 5.
It can be shown that there is no possible way to divide nums into 3 subarrays at a cost lower than 5.
```

Example 2:

```text
Input: nums = [10,1,2,2,2,1], k = 4, dist = 3
Output: 15
Explanation: The best possible way to divide nums into 4 subarrays is: [10], [1], [2], and [2,2,1]. This choice is valid because ik-1 - i1 is 3 - 1 = 2 which is less than dist. The total cost is nums[0] + nums[1] + nums[2] + nums[3] which is 10 + 1 + 2 + 2 = 15.
The division [10], [1], [2,2,2], and [1] is not valid, because the difference between ik-1 and i1 is 5 - 1 = 4, which is greater than dist.
It can be shown that there is no possible way to divide nums into 4 subarrays at a cost lower than 15.
```

Example 3:

```text
Input: nums = [10,8,18,9], k = 3, dist = 1
Output: 36
Explanation: The best possible way to divide nums into 3 subarrays is: [10], [8], and [18,9]. This choice is valid because ik-1 - i1 is 2 - 1 = 1 which is equal to dist.The total cost is nums[0] + nums[1] + nums[2] which is 10 + 8 + 18 = 36.
The division [10], [8,18], and [9] is not valid, because the difference between ik-1 and i1 is 3 - 1 = 2, which is greater than dist.
It can be shown that there is no possible way to divide nums into 3 subarrays at a cost lower than 36.
```

__Constraints:__

- `3 <= n <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `3 <= k <= n`
- `k - 2 <= dist <= n - 2`

__题目描述:__

给你一个下标从 __0__ 开始长度为 `n` 的整数数组 `nums` 和两个 __正__ 整数 `k` 和 `dist` 。

一个数组的 __代价__ 是数组中的 __第一个__ 元素。比方说， `[1,2,3]` 的代价为 `1` ， `[3,4,1]` 的代价为 `3` 。

你需要将 `nums` 分割成 `k` 个 __连续且互不相交__ 的子数组，满足 __第二__ 个子数组与第 `k` 个子数组中第一个元素的下标距离 __不超过__ `dist` 。换句话说，如果你将 `nums` 分割成子数组 `nums[0..(i1 - 1)], nums[i1..(i2 - 1)], ..., nums[ik-1..(n - 1)]` ，那么它需要满足 `ik-1 - i1 <= dist` 。

请你返回这些子数组的 最小 总代价。

__示例:__

示例 1：

```text
输入：nums = [1,3,2,6,4,2], k = 3, dist = 3
输出：5
解释：将数组分割成 3 个子数组的最优方案是：[1,3] ，[2,6,4] 和 [2] 。这是一个合法分割，因为 ik-1 - i1 等于 5 - 2 = 3 ，等于 dist 。总代价为 nums[0] + nums[2] + nums[5] ，也就是 1 + 2 + 2 = 5 。
5 是分割成 3 个子数组的最小总代价。
```

示例 2：

```text
输入：nums = [10,1,2,2,2,1], k = 4, dist = 3
输出：15
解释：将数组分割成 4 个子数组的最优方案是：[10] ，[1] ，[2] 和 [2,2,1] 。这是一个合法分割，因为 ik-1 - i1 等于 3 - 1 = 2 ，小于 dist 。总代价为 nums[0] + nums[1] + nums[2] + nums[3] ，也就是 10 + 1 + 2 + 2 = 15 。
分割 [10] ，[1] ，[2,2,2] 和 [1] 不是一个合法分割，因为 ik-1 和 i1 的差为 5 - 1 = 4 ，大于 dist 。
15 是分割成 4 个子数组的最小总代价。
```

示例 3：

```text
输入：nums = [10,8,18,9], k = 3, dist = 1
输出：36
解释：将数组分割成 3 个子数组的最优方案是：[10] ，[8] 和 [18,9] 。这是一个合法分割，因为 ik-1 - i1 等于 2 - 1 = 1 ，等于 dist 。总代价为 nums[0] + nums[1] + nums[2] ，也就是 10 + 8 + 18 = 36 。
分割 [10] ，[8,18] 和 [9] 不是一个合法分割，因为 ik-1 和 i1 的差为 3 - 1 = 2 ，大于 dist 。
36 是分割成 3 个子数组的最小总代价。
```

__提示：__

- `3 <= n <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `3 <= k <= n`
- `k - 2 <= dist <= n - 2`

__思路:__

```text
滑动窗口
nums[0] 必须要选
剩下 k - 1 个组需要保证第一个组的开头和最后一个组的开头不超过 dist
相当于从 dist + 1 长的滑动窗口中选择 k - 1 小的数
使用有序队列 L 和 R
将 k - 1 小的数放在 L
滑动窗口中的其他数都放到 R 中
初始化的时候将 [0, dist + 1] 累计求和
将 [1, dist + 1] 的数都放在 L 中
如果 L 的大小超过 k - 1 将超出的数移动到 R
此后先移除 nums[i - dist - 1]
检查该数是在 L 还是在 R 相应移除
再加入 nums[i]
如果 nums[i] 比 L 的最大值要小就加入 L
否则加入 R
最后如果 L 的大小小于 k - 1 则移动 R 中的一个数到 L
如果 L 的大小大于 k - 1 则移动 L 中的一个数到 R
时间复杂度为 O(NlogM), 空间复杂度为 O(M), 其中 N 为 nums 数组的长度, M 为 dist
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minimumCost(vector<int>& nums, int k, int dist) 
    {
        --k;
        multiset<int> L(nums.begin() + 1, nums.begin() + dist + 2), R;
        long long sum_left = accumulate(nums.begin(), nums.begin() + dist + 2, 0LL);
        auto l2r = [&]() -> void
        {
            int num = *L.rbegin();
            L.erase(L.find(num));
            sum_left -= num;
            R.insert(num);
        };
        auto r2l = [&]() -> void
        {
            int num = *R.begin();
            R.erase(R.find(num));
            sum_left += num;
            L.insert(num);
        };
        while (L.size() > k) l2r();
        long long result = sum_left;
        for (int i = dist + 2, n = nums.size(); i < n; i++) 
        {
            int out_val = nums[i - dist - 1], in_val = nums[i];
            auto it = L.find(out_val);
            if (it != L.end()) 
            {
                L.erase(it);
                sum_left -= out_val;
            } 
            else R.erase(R.find(out_val));
            if (in_val < *L.rbegin()) 
            {
                L.insert(in_val);
                sum_left += in_val;
            } 
            else R.insert(in_val);
            if (L.size() == k - 1) r2l();
            else if (L.size() == k + 1) { l2r(); }
            result = min(result, sum_left);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private final TreeMap<Integer, Integer> L = new TreeMap<>();
    private final TreeMap<Integer, Integer> R = new TreeMap<>();
    private long sumLeft;
    private int s;

    public long minimumCost(int[] nums, int k, int dist) {
        --k;
        sumLeft = nums[0];
        for (int i = 1; i < dist + 2; i++) {
            sumLeft += nums[i];
            L.merge(nums[i], 1, Integer::sum);
        }
        s = dist + 1;
        while (s > k) l2r();
        long result = sumLeft;
        for (int i = dist + 2, n = nums.length; i < n; i++) {
            int outVal = nums[i - dist - 1], inVal = nums[i];
            if (L.containsKey(outVal)) {
                minusOne(L, outVal);
                sumLeft -= outVal;
                --s;
            } else minusOne(R, outVal);
            if (inVal < L.lastKey()) {
                L.merge(inVal, 1, Integer::sum);
                sumLeft += inVal;
                ++s;
            } else R.merge(inVal, 1, Integer::sum);
            if (s == k - 1) r2l();
            else if (s == k + 1) { l2r(); }
            result = Math.min(result, sumLeft);
        }
        return result;
    }

    private void l2r() {
        int num = L.lastKey();
        minusOne(L, num);
        sumLeft -= num;
        --s;
        R.merge(num, 1, Integer::sum);
    }

    private void r2l() {
        int num = R.firstKey();
        minusOne(R, num);
        sumLeft += num;
        ++s;
        L.merge(num, 1, Integer::sum);
    }

    private void minusOne(Map<Integer, Integer> map, int num) {
        if (map.merge(num, -1, Integer::sum) == 0) map.remove(num);
    }
}
```

__Python__:

```Python
class Solution:
    def minimumCost(self, nums: List[int], k: int, dist: int) -> int:
        k -= 1
        L, R, sum_left = SortedList(nums[1:dist + 2]), SortedList(), sum(nums[:dist + 2])
        def L2R() -> NoReturn:
            nonlocal sum_left
            sum_left -= (num := L.pop())
            R.add(num)

        def R2L() -> NoReturn:
            nonlocal sum_left
            sum_left += (num := R.pop(0))
            L.add(num)

        while len(L) > k:
            L2R()
        result, n = sum_left, len(nums)
        for i in range(dist + 2, n):
            if (out_val := nums[i - dist - 1]) in L:
                sum_left -= out_val
                L.remove(out_val)
            else:
                R.remove(out_val)
            if (in_val := nums[i]) < L[-1]:
                sum_left += in_val
                L.add(in_val)
            else:
                R.add(in_val)
            if len(L) == k - 1:
                R2L()
            elif len(L) == k + 1:
                L2R()
            result = min(result, sum_left)
        return result
```
