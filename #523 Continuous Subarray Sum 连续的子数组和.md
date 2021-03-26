# 523 Continuous Subarray Sum 连续的子数组和

__Description__:
Given a list of non-negative numbers and a target integer k, write a function to check if the array has a continuous subarray of size at least 2 that sums up to a multiple of k, that is, sums up to n*k where n is also an integer.

__Example:__

Example 1:

Input: [23, 2, 4, 6, 7],  k=6
Output: True
Explanation: Because [2, 4] is a continuous subarray of size 2 and sums up to 6.

Example 2:

Input: [23, 2, 6, 4, 7],  k=6
Output: True
Explanation: Because [23, 2, 6, 4, 7] is an continuous subarray of size 5 and sums up to 42.

__Constraints:__

The length of the array won't exceed 10,000.
You may assume the sum of all the numbers is in the range of a signed 32-bit integer.

__题目描述__:
给定一个包含 非负数 的数组和一个目标 整数 k ，编写一个函数来判断该数组是否含有连续的子数组，其大小至少为 2，且总和为 k 的倍数，即总和为 n * k ，其中 n 也是一个整数。

__示例 :__

示例 1：

输入：[23,2,4,6,7], k = 6
输出：True
解释：[2,4] 是一个大小为 2 的子数组，并且和为 6。

示例 2：

输入：[23,2,6,4,7], k = 6
输出：True
解释：[23,2,6,4,7]是大小为 5 的子数组，并且和为 42。

__说明:__

数组的长度不会超过 10,000 。
你可以认为所有数字总和在 32 位有符号整数范围内。

__思路__:

前缀和
注意 k和 n有可能都为负数, 这里取 k的绝对值简化
如果数组长度小于 2直接返回 false
用一个哈希表记录前缀和与 k的余数及下标
sum(nums[:i]) % k - sum(nums[:j]) % k == 0(i - j > 1) 说明 sum(nums[i:j]) = n \* k
注意到 sum(nums[:i])是可能等于 n \* k的, 所以初始化的时候加入 m[0] = -1, 防止第一个元素是 0的情况的误判
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool checkSubarraySum(vector<int>& nums, int k) 
    {
        int cur = 0, n = nums.size();
        if (n < 2) return false;
        unordered_map<int, int> m;
        m[0] = -1;
        k = abs(k);
        for (int i = 0; i < n; i++) 
        {
            cur += nums[i];
            if (k) cur %= k;
            int pre = m.count(cur) ? m[cur] : i;
            m[cur] = pre;
            if (i - pre > 1) return true;
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        int cur = 0, n = nums.length;
        if (n < 2) return false;
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, -1);
        k = Math.abs(k);
        for (int i = 0; i < n; i++) {
            cur += nums[i];
            if (k != 0) cur %= k;
            int pre = map.getOrDefault(cur, i);
            map.put(cur, pre);
            if (i - pre > 1) return true;
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def checkSubarraySum(self, nums: List[int], k: int) -> bool:
        if len(nums) < 2:
            return False
        dp, cur = {0: -1}, 0
        for i, num in enumerate(nums):
            cur += num
            if k:
                cur %= k
            if i - dp.setdefault(cur, i) > 1:
                return True
        return False
```
