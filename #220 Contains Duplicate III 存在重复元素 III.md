__Description__:
Given an array of integers, find out whether there are two distinct indices i and j in the array such that the absolute difference between nums[i] and nums[j] is at most t and the absolute difference between i and j is at most k.

__Example:__
Example 1:

Input: nums = [1,2,3,1], k = 3, t = 0
Output: true

Example 2:

Input: nums = [1,0,1,1], k = 1, t = 2
Output: true

Example 3:

Input: nums = [1,5,9,1,5,9], k = 2, t = 3
Output: false

__题目描述__:
在整数数组 nums 中，是否存在两个下标 i 和 j，使得 nums [i] 和 nums [j] 的差的绝对值小于等于 t ，且满足 i 和 j 的差的绝对值也小于等于 ķ 。

如果存在则返回 true，不存在返回 false。

__示例 :__
示例 1:

输入: nums = [1,2,3,1], k = 3, t = 0
输出: true

示例 2:

输入: nums = [1,0,1,1], k = 1, t = 2
输出: true

示例 3:

输入: nums = [1,5,9,1,5,9], k = 2, t = 3
输出: false

__思路__:
桶排序
将数字分成 t + 1个区间, 相同的区间内两个元素差必定小于等于 t
相邻区间才需要判断绝对值是否在区间内
比如如果一个人出生在 2月, 需要找 30天之内出生的人, 2月出生的一定可以, 1月和 3月出生的人也有可能, 这里是把月份当作桶
由于下标差需要小于等于 k, 所以需要将距离大于 k的元素删除(滑动窗口)
时间复杂度O(n), 空间复杂度O(k), 只需要对每个元素遍历一次, 哈希表查询时间 O(1), 每个桶最多一个数字, 同时最多存在 k个桶

__代码__:
__C++__:
```C++
class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
        if (t < 0 | k < 0) return false;
        long bucket_size = (long)t + 1;
        unordered_map<long, long> m;
        for (int i = 0; i < nums.size(); i++)
        {
            int bucket = nums[i] < 0 ? nums[i] / bucket_size - 1 : nums[i] / bucket_size;
            if (m.find(bucket) != m.end()) return true;
            if ((m.find(bucket + 1) != m.end() && labs(m[bucket + 1] - nums[i]) <= t) | (m.find(bucket - 1) != m.end() && labs(m[bucket - 1] - nums[i]) <= t)) return true;
            m[bucket] = nums[i];
            if (i >= k) m.erase(nums[i - k] / bucket_size);
        }
        return false;
    }
};
```

__Java__:
```Java
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        if (t < 0 | k < 0) return false;
        Map<Long, Long> hash = new HashMap<>();
        long bucketSize = (long)t + 1;
        for (int i = 0; i < nums.length; i++) {
            long m = getBacket(nums[i], bucketSize);
            if (hash.containsKey(m)) return true;
            if ((hash.containsKey(m - 1) && Math.abs(nums[i] - hash.get(m - 1)) < bucketSize) | (hash.containsKey(m + 1) && Math.abs(nums[i] - hash.get(m + 1)) < bucketSize)) return true;
            hash.put(m, (long)nums[i]);
            if (i >= k) hash.remove(getBacket(nums[i - k], bucketSize));
        }
        return false;
    }

    private long getBacket(long i, long bucketSize) {
        return i < 0 ? (i + 1) / bucketSize - 1 : i / bucketSize;
    }
}
```

__Python__:
```Python
class Solution:
    def containsNearbyAlmostDuplicate(self, nums: List[int], k: int, t: int) -> bool:
        if t < 0 or k < 0:
            return False
        buckets, buckets_size = {}, t + 1
        for i in range(len(nums)):
            bucket_num = nums[i] // buckets_size
            if bucket_num in buckets:
                return True
            buckets[bucket_num] = nums[i]
            if (bucket_num - 1) in buckets and abs(buckets[bucket_num - 1] - nums[i]) <= t or (bucket_num + 1) in buckets and abs(buckets[bucket_num + 1] - nums[i]) <= t:
                return True
            buckets[bucket_num] = nums[i]
            if i >= k:
                buckets.pop(nums[i - k] // buckets_size)
        return False
```