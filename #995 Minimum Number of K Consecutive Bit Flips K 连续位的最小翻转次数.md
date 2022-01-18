# 995 Minimum Number of K Consecutive Bit Flips K 连续位的最小翻转次数

__Description__:
You are given a binary array nums and an integer k.

A k-bit flip is choosing a subarray of length k from nums and simultaneously changing every 0 in the subarray to 1, and every 1 in the subarray to 0.

Return the minimum number of k-bit flips required so that there is no 0 in the array. If it is not possible, return -1.

A subarray is a contiguous part of an array.

__Example:__

Example 1:

Input: nums = [0,1,0], k = 1
Output: 2
Explanation: Flip nums[0], then flip nums[2].

Example 2:

Input: nums = [1,1,0], k = 2
Output: -1
Explanation: No matter how we flip subarrays of size 2, we cannot make the array become [1,1,1].

Example 3:

Input: nums = [0,0,0,1,0,1,1,0], k = 3
Output: 3
Explanation:
Flip nums[0],nums[1],nums[2]: nums becomes [1,1,1,1,0,1,1,0]
Flip nums[4],nums[5],nums[6]: nums becomes [1,1,1,1,1,0,0,0]
Flip nums[5],nums[6],nums[7]: nums becomes [1,1,1,1,1,1,1,1]

__Constraints:__

1 <= nums.length <= 10^5
1 <= k <= nums.length

__题目描述__:
在仅包含 0 和 1 的数组 A 中，一次 K 位翻转包括选择一个长度为 K 的（连续）子数组，同时将子数组中的每个 0 更改为 1，而每个 1 更改为 0。

返回所需的 K 位翻转的最小次数，以便数组没有值为 0 的元素。如果不可能，返回 -1。

__示例 :__

示例 1：

输入：A = [0,1,0], K = 1
输出：2
解释：先翻转 A[0]，然后翻转 A[2]。

示例 2：

输入：A = [1,1,0], K = 2
输出：-1
解释：无论我们怎样翻转大小为 2 的子数组，我们都不能使数组变为 [1,1,1]。

示例 3：

输入：A = [0,0,0,1,0,1,1,0], K = 3
输出：3
解释：
翻转 A[0],A[1],A[2]: A变成 [1,1,1,1,0,1,1,0]
翻转 A[4],A[5],A[6]: A变成 [1,1,1,1,1,0,0,0]
翻转 A[5],A[6],A[7]: A变成 [1,1,1,1,1,1,1,1]

__提示:__

1 <= A.length <= 30000
1 <= K <= A.length

__思路__:

差分数组 ➕ 滑动窗口
从左到右遍历数组, 每次碰到 0 就把 [start, start + k) 区间内的所有数组元素翻转
按照前一策略每次都需要翻转 k 个元素, 时间复杂度为 O(nk)
可以利用差分数组的思想每次只需要修改端点的值, 即 ++diff[start], --diff[start + k], diff 用来记录翻转的次数, 当遍历到某个元素时, 翻转次数为 2 的倍数说明需要进行一次翻转, 当前位置翻转次数自增, diff[start + k] 自减, 翻转次数结果自增, 这时时间复杂度为 O(n), 空间复杂度为 O(n)
因为只需要记录是否为 2 的倍数, 可以将当前位置翻转次数用模 2 加法代替(异或)
并且注意到如果 start + k > n, 说明无论如何也不能翻转出全 1 的数组, 直接返回 -1 即可
最后 diff 数组可以在 nums 数组上记录, 如果翻转当前元素, 当前元素记录改为一个不在 [0, 1] 之间的元素即可
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minKBitFlips(vector<int>& nums, int k) 
    {
        int n = nums.size(), result = 0, start = 0;
        for (int i = 0; i < n; i++)
        {
            if (i >= k and nums[i - k] > 1) start ^= 1;
            if (nums[i] == start) 
            {
                if (i + k > n) return -1;
                ++result;
                start ^= 1;
                nums[i] += 2;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minKBitFlips(int[] nums, int k) {
        int n = nums.length, result = 0, start = 0;
        for (int i = 0; i < n; i++) {
            if (i >= k && nums[i - k] > 1) start ^= 1;
            if (nums[i] == start) {
                if (i + k > n) return -1;
                ++result;
                start ^= 1;
                nums[i] += 2;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minKBitFlips(self, nums: List[int], k: int) -> int:
        result, start, n = 0, 0, len(nums)
        for i in range(n):
            if i >= k and nums[i - k] > 1:
                start ^= 1
            if nums[i] == start:
                if i + k > n:
                    return -1
                result += 1
                start ^= 1
                nums[i] += 2
        return result
```
