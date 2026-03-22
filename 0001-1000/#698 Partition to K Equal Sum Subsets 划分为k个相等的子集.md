# 698 Partition to K Equal Sum Subsets 划分为k个相等的子集

__Description__:
Given an integer array nums and an integer k, return true if it is possible to divide this array into k non-empty subsets whose sums are all equal.

__Example:__

Example 1:

Input: nums = [4,3,2,3,5,2,1], k = 4
Output: true
Explanation: It's possible to divide it into 4 subsets (5), (1, 4), (2,3), (2,3) with equal sums.

Example 2:

Input: nums = [1,2,3,4], k = 3
Output: false

__Constraints:__

1 <= k <= nums.length <= 16
1 <= nums[i] <= 10^4
The frequency of each element is in the range [1, 4].

__题目描述__:
给定一个整数数组  nums 和一个正整数 k，找出是否有可能把这个数组分成 k 个非空子集，其总和都相等。

__示例 :__

示例 1：

输入： nums = [4, 3, 2, 3, 5, 2, 1], k = 4
输出： True
说明： 有可能将其分成 4 个子集（5），（1,4），（2,3），（2,3）等于总和。

__提示:__

1 <= k <= len(nums) <= 16
0 < nums[i] < 10000

__思路__:

回溯法
参考[LeetCode #416 Partition Equal Subset Sum 分割等和子集](https://www.jianshu.com/p/634317f992a3)
如果 k == 1, 直接返回 true, 全部分为 1 组即可
先计算数组和 sum
如果 sum % k != 0, 说明不能恰好分割为 k 组, 直接返回 false
准备 k 个容量为 sum / k 的桶
对数组进行排序
然后按顺序放入数组元素
需要对桶进行去重, 如果不是第一个桶, 与前面的桶元素相同就跳过
只有两种情况可以放入元素, 第一是桶剩下的空间刚好等于数组元素, 第二种是桶剩下的空间不小于数组元素和最小元素之和, 这时候可以尝试一个桶放入多个数组元素
直到遍历完数组元素
时间复杂度为 O(2 ^ n * k), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool canPartitionKSubsets(vector<int>& nums, int k) 
    {
        if (k == 1) return true;
        sort(nums.begin(), nums.end());
        int s = accumulate(nums.begin(), nums.end(), 0), n = nums.size();
        if (s % k) return false;
        vector<int> bucket(k, s / k);
        return trackback(nums, bucket, k, n - 1);
    }
private:
    bool trackback(vector<int>& nums, vector<int>& bucket, int k, int cur)
    {
        if (cur < 0) return true;
        for (int i = 0; i < k; i++)
        {
            if (i and bucket[i] == bucket[i - 1]) continue;
            if (bucket[i] == nums[cur] or bucket[i] >= nums[cur] + nums.front())
            {
                bucket[i] -= nums[cur];
                if (trackback(nums, bucket, k, cur - 1)) return true;
                bucket[i] += nums[cur];
            }
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canPartitionKSubsets(int[] nums, int k) {
        if (k == 1) return true;
        int s = 0, n = nums.length;
        for (int num : nums) s += num;
        if (s % k != 0) return false;
        int bucket[] = new int[k];
        Arrays.fill(bucket, s / k);
        Arrays.sort(nums);
        return trackback(nums, bucket, k, n - 1);
    }
    
    private boolean trackback(int[] nums, int[] bucket, int k, int cur) {
        if (cur < 0) return true;
        for (int i = 0; i < k; i++) {
            if (i > 0 && bucket[i] == bucket[i - 1]) continue;
            if (bucket[i] == nums[cur] || bucket[i] >= nums[cur] + nums[0]) {
                bucket[i] -= nums[cur];
                if (trackback(nums, bucket, k, cur - 1)) return true;
                bucket[i] += nums[cur];
            }
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def canPartitionKSubsets(self, nums: List[int], k: int) -> bool:
        if k == 1:
            return True
        bucket = [(s := sum(nums)) // k] * (n := len(nums))
        if s % k:
            return False
        nums.sort()
        
        def trackback(cur: int) -> bool:
            if cur < 0:
                return True
            for i in range(k):
                if i and bucket[i] == bucket[i - 1]:
                    continue
                if bucket[i] == nums[cur] or bucket[i] >= nums[cur] + nums[0]:
                    bucket[i] -= nums[cur]
                    if trackback(cur - 1):
                        return True
                    bucket[i] += nums[cur]
            return False
        
        return trackback(n - 1)
        
```
