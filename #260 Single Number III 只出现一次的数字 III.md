# 260 Single Number III 只出现一次的数字 III

__Description__:
Given an integer array nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once. You can return the answer in any order.

__Follow up:__
Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?

__Example:__

Example 1:

Input: nums = [1,2,1,3,2,5]
Output: [3,5]
Explanation:  [5, 3] is also a valid answer.

Example 2:

Input: nums = [-1,0]
Output: [-1,0]

Example 3:

Input: nums = [0,1]
Output: [1,0]

__Constraints:__

1 <= nums.length <= 30000
 Each integer in nums will appear twice, only two integers will appear once.

__题目描述__:
给定一个整数数组 nums，其中恰好有两个元素只出现一次，其余所有元素均出现两次。 找出只出现一次的那两个元素。

__示例 :__

输入: [1,2,1,3,2,5]
输出: [3,5]

__注意：__

结果输出的顺序并不重要，对于上面的例子， [5, 3] 也是正确答案。
你的算法应该具有线性时间复杂度。你能否仅使用常数空间复杂度来实现？

__思路__:

1. 按出现次数排序
时间复杂度O(nlgn), 空间复杂度O(1)
2. 使用 hash表记录出现的次数
时间复杂度O(n), 空间复杂度O(n)
3. 出现两次的数字可以用异或去掉
整个数组异或的值为只出现一次的数字的两个数的异或的值
然后为了区分两个数字, 将异或值的某位 1拿出来, 这里选择最后 1位
mask & (-mask)就可以得到最后一位 1
然后 mask & num判断是否为 0就可以区分两个不相同的数字
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> singleNumber(vector<int>& nums) 
    {
        int mask = 0, a = 0, b = 0;
        for (auto &num : nums) mask ^= num;
        mask &= (-mask);
        for (auto &num : nums)
        {
            if (num & mask) a ^= num;
            else b ^= num;
        }
        return vector<int>{a, b};
    }
};
```

__Java__:

```Java
class Solution {
    public int[] singleNumber(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        int result[] = new int[2], i = 0;
        for (int num : nums) map.put(num, map.getOrDefault(num, 0) + 1);
        for (Map.Entry<Integer, Integer> item : map.entrySet()) if (item.getValue() == 1) result[i++] = item.getKey();
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def singleNumber(self, nums: List[int]) -> List[int]:
        return sorted(nums, key=lambda x:nums.count(x))[:2]
```
