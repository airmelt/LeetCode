# 1296 Divide Array in Sets of K Consecutive Numbers 划分数组为连续数字的集合

__Description:__

Given an array of integers nums and a positive integer k, check whether it is possible to divide this array into sets of k consecutive numbers.

Return true if it is possible. Otherwise, return false.

__Example:__

Example 1:

Input: nums = [1,2,3,3,4,4,5,6], k = 4
Output: true
Explanation: Array can be divided into [1,2,3,4] and [3,4,5,6].

Example 2:

Input: nums = [3,2,1,2,3,4,3,4,5,9,10,11], k = 3
Output: true
Explanation: Array can be divided into [1,2,3] , [2,3,4] , [3,4,5] and [9,10,11].

Example 3:

Input: nums = [1,2,3,4], k = 3
Output: false
Explanation: Each array should be divided in subarrays of size 3.

__Constraints:__

1 <= k <= nums.length <= 10^5
1 <= nums[i] <= 10^9

__题目描述：__

给你一个整数数组 nums 和一个正整数 k，请你判断是否可以把这个数组划分成一些由 k 个连续数字组成的集合。
如果可以，请返回 true；否则，返回 false。

__示例：__

示例 1：

输入：nums = [1,2,3,3,4,4,5,6], k = 4
输出：true
解释：数组可以分成 [1,2,3,4] 和 [3,4,5,6]。

示例 2：

输入：nums = [3,2,1,2,3,4,3,4,5,9,10,11], k = 3
输出：true
解释：数组可以分成 [1,2,3] , [2,3,4] , [3,4,5] 和 [9,10,11]。

示例 3：

输入：nums = [3,3,2,2,1,1], k = 3
输出：true

示例 4：

输入：nums = [1,2,3,4], k = 3
输出：false
解释：数组不能分成几个大小为 3 的子数组。

__提示：__

1 <= k <= nums.length <= 10^5
1 <= nums[i] <= 10^9

__思路：__

贪心
如果数组不够分, 直接返回 false
先排序
那么遍历 nums 一定从数组 k 中选到最小的数 x
这时必须保证 [x, x + k) 都在数组中
可以用哈希表存放加快查找速度
时间复杂度为 O(nlgn), 空间复杂度为 O(n), 主要时间用在排序

__代码__:

__C++__:

```C++
class Solution 
{
public:
    bool isPossibleDivide(vector<int>& nums, int k) 
    {
        int n = nums.size(), left = 0, right = 0;
        if (n % k) return false;
        sort(nums.begin(), nums.end());
        unordered_map<int, int> m;
        for (const auto& num : nums) ++m[num];
        for (const auto& num : nums) if (m[num]) for (int j = 0; j < k; j++) if (!m[num + j]--) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isPossibleDivide(int[] nums, int k) {
        int n = nums.length, left = 0, right = 0;
        Map<Integer, Integer> map = new HashMap<>();
        Arrays.sort(nums);
        for (int num : nums) map.put(num, map.getOrDefault(num, 0) + 1);
        for (int num : nums) if (map.getOrDefault(num, 0) != 0) for (int j = 0; j < k; j++) {
            if (map.getOrDefault(num + j, 0) == 0) return false;
            map.put(num + j, map.getOrDefault(num + j, 0) - 1);
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def isPossibleDivide(self, nums: List[int], k: int) -> bool:
        if len(nums) % k:
            return False
        nums.sort()
        count = Counter(nums)
        for num in nums:
            if count[num]:
                for i in range(k):
                    if not count[num + i]:
                        return False
                    count[num + i] -= 1
        return True
```
