# 719 Find K-th Smallest Pair Distance 找出第 k 小的距离对

__Description__:
The distance of a pair of integers a and b is defined as the absolute difference between a and b.

Given an integer array nums and an integer k, return the kth smallest distance among all the pairs nums[i] and nums[j] where 0 <= i < j < nums.length.

__Example:__

Example 1:

Input: nums = [1,3,1], k = 1
Output: 0
Explanation: Here are all the pairs:

```text
(1,3) -> 2
(1,1) -> 0
(3,1) -> 2
```

Then the 1st smallest distance pair is (1,1), and its distance is 0.

Example 2:

Input: nums = [1,1,1], k = 2
Output: 0

Example 3:

Input: nums = [1,6,1], k = 3
Output: 5

__Constraints:__

n == nums.length
2 <= n <= 10^4
0 <= nums[i] <= 10^6
1 <= k <= n * (n - 1) / 2

__题目描述__:
给定一个整数数组，返回所有数对之间的第 k 个最小距离。一对 (A, B) 的距离被定义为 A 和 B 之间的绝对差值。

__示例 :__

示例 1:

输入：
nums = [1,3,1]
k = 1
输出：0
解释：
所有数对如下：

```text
(1,3) -> 2
(1,1) -> 0
(3,1) -> 2
```

因此第 1 个最小距离的数对是 (1,1)，它们之间的距离为 0。

__提示:__

2 <= len(nums) <= 10000.
0 <= nums[i] < 1000000.
1 <= k <= len(nums) * (len(nums) - 1) / 2.

__思路__:

二分法
先对数组进行排序
绝对值最小为 0, 最大为 nums[-1] - nums[0]
然后用二分法统计数量是否大于 k
小于 k 说明少了, 此时 l = mid + 1, 大于或等于 k 说明多了, 此时 r = mid
求数量使用双指针即可
时间复杂度为 O(nlgn), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int smallestDistancePair(vector<int>& nums, int k) 
    {
        sort(nums.begin(), nums.end());
        int l = 0, r = nums.back() - nums.front(), n = nums.size();
        while (l < r) 
        {
            int mid = l + ((r - l) >> 1);
            if (helper(mid, nums, n) < k) l = mid + 1;
            else r = mid;
        }
        return l;
    }
private:
    int helper(int mid, vector<int>& nums, int n) 
    {
        int j = 0, count = 0;
        for (int i = 1; i < n; i++) 
        {
            while (nums[i] - nums[j] > mid) ++j;
            count += i - j;
        }
        return count;
    }
};
```

__Java__:

```Java
class Solution {
    public int smallestDistancePair(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length;
        int l = 0, r = nums[n - 1] - nums[0];
        while (l < r) {
            int mid = l + ((r - l) >> 1);
            if (helper(mid, nums, n) < k) l = mid + 1;
            else r = mid;
        }
        return l;
    }
    
    private int helper(int mid, int[] nums, int n) {
        int j = 0, count = 0;
        for (int i = 1; i < n; i++) {
            while (nums[i] - nums[j] > mid) ++j;
            count += i - j;
        }
        return count;
    }
}
```

__Python__:

```Python
class Solution:
    def smallestDistancePair(self, nums: List[int], k: int) -> int:
        nums.sort()
        l, r = 0, nums[(n := len(nums)) - 1] - nums[0]
        
        def count(d: int) -> int:
            j = result = 0
            for i in range(1, n):
                while nums[i] - nums[j] > d:
                    j += 1
                result += i - j
            return result
            
        while l < r:
            mid = l + ((r - l) >> 1)
            if count(mid) < k:
                l = mid + 1
            else:
                r = mid
        return l
```
