# 1248 Count Number of Nice Subarrays 统计「优美子数组」

__Description:__

Given an array of integers nums and an integer k. A continuous subarray is called nice if there are k odd numbers on it.

Return the number of nice sub-arrays.

__Example:__

Example 1:

Input: nums = [1,1,2,1,1], k = 3
Output: 2
Explanation: The only sub-arrays with 3 odd numbers are [1,1,2,1] and [1,2,1,1].

Example 2:

Input: nums = [2,4,6], k = 1
Output: 0
Explanation: There is no odd numbers in the array.

Example 3:

Input: nums = [2,2,2,1,2,2,1,2,2,2], k = 2
Output: 16

__Constraints:__

1 <= nums.length <= 50000
1 <= nums[i] <= 10^5
1 <= k <= nums.length

__题目描述：__

给你一个整数数组 nums 和一个整数 k。如果某个连续子数组中恰好有 k 个奇数数字，我们就认为这个子数组是「优美子数组」。

请返回这个数组中 「优美子数组」 的数目。

__示例：__

示例 1：

输入：nums = [1,1,2,1,1], k = 3
输出：2
解释：包含 3 个奇数的子数组是 [1,1,2,1] 和 [1,2,1,1] 。

示例 2：

输入：nums = [2,4,6], k = 1
输出：0
解释：数列中不包含任何奇数，所以不存在优美子数组。

示例 3：

输入：nums = [2,2,2,1,2,2,1,2,2,2], k = 2
输出：16

__提示：__

1 <= nums.length <= 50000
1 <= nums[i] <= 10^5
1 <= k <= nums.length

__思路：__

位运算 ➕ 前缀和
因为只需要知道奇数的个数, 可以将原数组全部和 1 进行与运算
结果为 1 表示为奇数, 结果为 0 表示为偶数
转换为 [LeetCode #560 Subarray Sum Equals K 和为K的子数组 - 简书 (jianshu.com)](https://www.jianshu.com/p/a061228f047a)
统计新数组的前缀和即可
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution
{
public:
    int numberOfSubarrays(vector<int>& nums, int k) 
    {
        int n = nums.size(), s = 0, result = 0;
        vector<int> count(n);
        for (int i = 0; i < n; i++) count[i] = nums[i] & 1;
        map<int, int> m;
        m[0] = 1;
        for (int num : count)
        {
            s += num;
            result += m[s - k];
            ++m[s];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfSubarrays(int[] nums, int k) {
        int n = nums.length, count[] = new int[n], s = 0, result = 0;
        for (int i = 0; i < n; i++) count[i] = nums[i] & 1;
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
        for (int num : count) {
            s += num;
            result += map.getOrDefault(s - k, 0);
            map.put(s, map.getOrDefault(s, 0) + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfSubarrays(self, nums: List[int], k: int) -> int:
        count, nums, s, result = defaultdict(int), list(map(lambda i: i & 1, nums)), 0, 0
        count[0] = 1
        for num in nums:
            s += num
            result += count[s - k]
            count[s] += 1
        return result
```
