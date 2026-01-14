# 2845 Count of Interesting Subarrays 统计趣味子数组的数目

__Description:__

You are given a __0-indexed__ integer array `nums`, an integer `modulo`, and an integer `k`.

Your task is to find the count of subarrays that are interesting.

A __subarray__ `nums[l..r]` is __interesting__ if the following condition holds:

- Let `cnt` be the number of indices `i` in the range `[l, r]` such that `nums[i] % modulo == k`. Then, `cnt % modulo == k`.

Return an integer denoting the count of interesting subarrays.

Note: A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [3,2,4], modulo = 2, k = 1
Output: 3
Explanation: In this example the interesting subarrays are: 
```

The subarray nums[0..0] which is [3].

- There is only one index, i = 0, in the range [0, 0] that satisfies nums[i] % modulo == k.
- Hence, cnt = 1 and cnt % modulo == k.  
The subarray nums[0..1] which is [3,2].
- There is only one index, i = 0, in the range [0, 1] that satisfies nums[i] % modulo == k.  
- Hence, cnt = 1 and cnt % modulo == k.

The subarray nums[0..2] which is [3,2,4].

- There is only one index, i = 0, in the range [0, 2] that satisfies nums[i] % modulo == k.
- Hence, cnt = 1 and cnt % modulo == k.

It can be shown that there are no other interesting subarrays. So, the answer is 3.

Example 2:

```text
Input: nums = [3,1,9,6], modulo = 3, k = 0
Output: 2
Explanation: In this example the interesting subarrays are: 
```

The subarray nums[0..3] which is [3,1,9,6].

- There are three indices, i = 0, 2, 3, in the range [0, 3] that satisfy nums[i] % modulo == k.
- Hence, cnt = 3 and cnt % modulo == k.

The subarray nums[1..1] which is [1].

- There is no index, i, in the range [1, 1] that satisfies nums[i] % modulo == k.
- Hence, cnt = 0 and cnt % modulo == k.

It can be shown that there are no other interesting subarrays. So, the answer is 2.

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= modulo <= 10 ^ 9`
- `0 <= k < modulo`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，以及整数 `modulo` 和整数 `k` 。

请你找出并统计数组中 趣味子数组 的数目。

如果 __子数组__ `nums[l..r]` 满足下述条件，则称其为 __趣味子数组__ :

- 在范围 `[l, r]` 内，设 `cnt` 为满足 `nums[i] % modulo == k` 的索引 `i` 的数量。并且 `cnt % modulo == k` 。

以整数形式表示并返回趣味子数组的数目。

注意：子数组是数组中的一个连续非空的元素序列。

__示例:__

示例 1：

```text
输入：nums = [3,2,4], modulo = 2, k = 1
输出：3
解释：在这个示例中，趣味子数组分别是： 
```

子数组 nums[0..0] ，也就是 [3] 。

- 在范围 [0, 0] 内，只存在 1 个下标 i = 0 满足 nums[i] % modulo == k 。
- 因此 cnt = 1 ，且 cnt % modulo == k 。
子数组 nums[0..1] ，也就是 [3,2] 。
- 在范围 [0, 1] 内，只存在 1 个下标 i = 0 满足 nums[i] % modulo == k 。
- 因此 cnt = 1 ，且 cnt % modulo == k 。
子数组 nums[0..2] ，也就是 [3,2,4] 。
- 在范围 [0, 2] 内，只存在 1 个下标 i = 0 满足 nums[i] % modulo == k 。
- 因此 cnt = 1 ，且 cnt % modulo == k 。

可以证明不存在其他趣味子数组。因此，答案为 3 。

示例 2：

```text
输入：nums = [3,1,9,6], modulo = 3, k = 0
输出：2
解释：在这个示例中，趣味子数组分别是： 
```

子数组 nums[0..3] ，也就是 [3,1,9,6] 。

- 在范围 [0, 3] 内，只存在 3 个下标 i = 0, 2, 3 满足 nums[i] % modulo == k 。
- 因此 cnt = 3 ，且 cnt % modulo == k 。

子数组 nums[1..1] ，也就是 [1] 。

- 在范围 [1, 1] 内，不存在下标满足 nums[i] % modulo == k 。
- 因此 cnt = 0 ，且 cnt % modulo == k 。

可以证明不存在其他趣味子数组，因此答案为 2 。

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= modulo <= 10 ^ 9`
- `0 <= k < modulo`

__思路:__

```text
前缀和
先按 mod modulo 处理原数组
比如 [3,1,9,6] 处理后为 [0, 1, 0, 0]
再将这个数组按照是否等于 k 转化为 bool 数组 [1, 0, 1, 1]
这时只要计算前缀和中有多少个子数组的和 mod modulo 等于 k
这里有 [0:3] 和 [1:1] 两种情况
转化成前缀和 [1, 1, 2, 3]
那么就是求前缀和中有多少 pre[right] - pre[left] mod modulo == k
由于 0 <= k < modulo 所以 k mod modulo = k
移项可得 (pre[right] - k) mod modulo = pre[left] mod modulo
所以可以将 pre[left] 的值存在哈希表中
将 pre[right] - k 的值累加到结果中
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution {
public:
    long long countInterestingSubarrays(vector<int>& nums, int modulo, int k) {
        long long result = 0LL, cur = 0LL;
        unordered_map<long long, long long> c;
        ++c[0LL];
        for (const auto& num : nums) 
        {
            if (num % modulo == k) cur = (cur + 1) % modulo;
            result += c[(cur - k + modulo) % modulo];
            ++c[cur];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long countInterestingSubarrays(List<Integer> nums, int modulo, int k) {
        long result = 0L, cur = 0L;
        var c = new HashMap<Long, Long>();
        c.put(0L, 1L);
        for (int num : nums) {
            if (num % modulo == k) cur = (cur + 1) % modulo;
            result += c.getOrDefault((cur - k + modulo) % modulo, 0L);
            c.merge(cur, 1L, Long::sum);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countInterestingSubarrays(self, nums: List[int], modulo: int, k: int) -> int:
        c, result, cur = Counter([0]), 0, 0
        for num in nums:
            if num % modulo == k:
                cur = (cur + 1) % modulo
            result += c[(cur - k) % modulo]
            c[cur] += 1
        return result 
```
