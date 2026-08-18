# 3134 Find the Median of the Uniqueness Array 找出唯一性数组的中位数

__Description:__

You are given an integer array `nums`. The __uniqueness array__ of `nums` is the sorted array that contains the number of distinct elements of all the __subarrays__ of `nums`. In other words, it is a sorted array consisting of `distinct(nums[i..j])`, for all `0 <= i <= j < nums.length`.

Here, `distinct(nums[i..j])` denotes the number of distinct elements in the subarray that starts at index `i` and ends at index `j`.

Return the __median__ of the __uniqueness array__ of `nums`.

__Note__ that the __median__ of an array is defined as the middle element of the array when it is sorted in non-decreasing order. If there are two choices for a median, the __smaller__ of the two values is taken.

__Example:__

Example 1:

```text
Input: nums = [1,2,3]

Output: 1

Explanation:

The uniqueness array of nums is [distinct(nums[0..0]), distinct(nums[1..1]), distinct(nums[2..2]), distinct(nums[0..1]), distinct(nums[1..2]), distinct(nums[0..2])] which is equal to [1, 1, 1, 2, 2, 3]. The uniqueness array has a median of 1. Therefore, the answer is 1.
```

Example 2:

```text
Input: nums = [3,4,3,4,5]

Output: 2

Explanation:

The uniqueness array of nums is [1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 3, 3, 3]. The uniqueness array has a median of 2. Therefore, the answer is 2.
```

Example 3:

```text
Input: nums = [4,3,5,4]

Output: 2

Explanation:

The uniqueness array of nums is [1, 1, 1, 1, 2, 2, 2, 3, 3, 3]. The uniqueness array has a median of 2. Therefore, the answer is 2.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个整数数组 `nums` 。数组 `nums` 的 __唯一性数组__ 是一个按元素从小到大排序的数组，包含了 `nums` 的所有非空 __子数组__ 中不同元素的个数。

换句话说，这是由所有 `0 <= i <= j < nums.length` 的 `distinct(nums[i..j])` 组成的递增数组。

其中，`distinct(nums[i..j])` 表示从下标 `i` 到下标 `j` 的子数组中不同元素的数量。

返回 `nums` 唯一性数组 的 __中位数__ 。

__注意__，数组的 __中位数__ 定义为有序数组的中间元素。如果有两个中间元素，则取值较小的那个。

__示例:__

示例 1：

```text
输入：nums = [1,2,3]

输出：1

解释：

nums 的唯一性数组为 [distinct(nums[0..0]), distinct(nums[1..1]), distinct(nums[2..2]), distinct(nums[0..1]), distinct(nums[1..2]), distinct(nums[0..2])]，即 [1, 1, 1, 2, 2, 3] 。唯一性数组的中位数为 1 ，因此答案是 1 。
```

示例 2：

```text
输入：nums = [3,4,3,4,5]

输出：2

解释：

nums 的唯一性数组为 [1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 3, 3, 3] 。唯一性数组的中位数为 2 ，因此答案是 2 。
```

示例 3：

```text
输入：nums = [4,3,5,4]

输出：2

解释：

nums 的唯一性数组为 [1, 1, 1, 1, 2, 2, 2, 3, 3, 3] 。唯一性数组的中位数为 2 ，因此答案是 2 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__思路:__

```text
二分查找
数组的所有的子数组一共有 total = n * (n + 1) / 2 个
那么正中间的数为 k = (total + 1) / 2
由于排序之后唯一性数组有单调性, 可以使用二分查找找到第 k 个
使用滑动窗口并保证滑动窗口里只有 mid 个数
然后累加滑动窗口的大小直到超过 k 就返回 true
左边界 left 为 0
右边界 right 为 len(set(nums)), 最多有 right 个不同的数
如果计算完都不到 mid 个返回 false
每一轮都需要遍历一次整个数组需要 N
二分查找需要 logN
总体时间复杂度为 O(NlogN)
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int medianOfUniquenessArray(vector<int>& nums) 
    {
        unordered_set<int> st(nums.begin(), nums.end());
        int n = nums.size(), left = 0, right = st.size() + 1, mid = 0;
        long long k = (((n + 1LL) * n >> 1LL) + 1LL) >> 1LL;
        auto check = [&](int mid) -> bool
        {
            unordered_map<int, int> freq;
            long long cur = 0LL;
            for (int l = 0, r = 0; r < n; r++) 
            {
                ++freq[nums[r]];
                while (freq.size() > mid) if (!--freq[nums[l++]]) freq.erase(nums[l - 1]);
                if ((cur += r - l + 1LL) >= k) return true;
            }
            return false;
        };
        while (left + 1 < right) 
        {
            if (check(mid = left + ((right - left) >> 1))) right = mid;
            else left = mid;
        }
        return right;
    }
};
```

__Java__:

```Java
class Solution {
    public int medianOfUniquenessArray(int[] nums) {
        int n = nums.length, left = 0, right = Arrays.stream(nums).boxed().collect(Collectors.toSet()).size() + 1, mid = 0;
        long k = ((n + 1L) * n >>> 1L) + 1L >>> 1L;
        while (left + 1 < right) {
            if (check(mid = left + ((right - left) >> 1), n, nums, k)) right = mid;
            else left = mid;
        }
        return right;
    }

    private boolean check(int mid, int n, int[] nums, long k) {
        var freq = new HashMap<Integer, Integer>();
        long cur = 0L;
        for (int l = 0, r = 0; r < n; r++) {
            freq.merge(nums[r], 1, Integer::sum);
            while (freq.size() > mid) if (freq.merge(nums[l++], -1, Integer::sum) == 0) freq.remove(nums[l - 1]);
            if ((cur += r - l + 1L) >= k) return true;
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def medianOfUniquenessArray(self, nums: List[int]) -> int:
        def check(mid: int) -> bool:
            cur, l, freq = 0, 0, defaultdict(int)
            for r, num in enumerate(nums):
                freq[num] += 1
                while len(freq) > mid:
                    freq[out := nums[l]] -= 1
                    if not freq[out]:
                        del freq[out]
                    l += 1
                if (cur := cur + r - l + 1) >= (k := (((n := len(nums)) + 1) * n >> 1) + 1 >> 1):
                    return True
            return False
        return bisect_left(range(len(set(nums))), True, 1, key=check)
```
