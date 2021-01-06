# 287 Find the Duplicate Number 寻找重复数

__Description__:
Given an array of integers nums containing n + 1 integers where each integer is in the range [1, n] inclusive.

There is only one duplicate number in nums, return this duplicate number.

__Follow-ups__:

How can we prove that at least one duplicate number must exist in nums?
Can you solve the problem without modifying the array nums?
Can you solve the problem using only constant, O(1) extra space?
Can you solve the problem with runtime complexity less than O(n2)?

__Example:__

Example 1:

Input: nums = [1,3,4,2,2]
Output: 2

Example 2:

Input: nums = [3,1,3,4,2]
Output: 3

Example 3:

Input: nums = [1,1]
Output: 1

Example 4:

Input: nums = [1,1,2]
Output: 1

__Constraints:__

2 <= n <= 3 * 10 ^ 4
nums.length == n + 1
1 <= nums[i] <= n
All the integers in nums appear only once except for precisely one integer which appears two or more times.

__题目描述__:
给定一个包含 n + 1 个整数的数组 nums，其数字都在 1 到 n 之间（包括 1 和 n），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。

__示例 :__

示例 1:

输入: [1,3,4,2,2]
输出: 2

示例 2:

输入: [3,1,3,4,2]
输出: 3

__说明：__

1. 不能更改原数组（假设数组是只读的）。
2. 只能使用额外的 O(1) 的空间。
3. 时间复杂度小于 O(n ^ 2) 。
4. 数组中只有一个重复的数字，但它可能不止重复出现一次。

__思路__:

1. 哈希表记录
时间复杂度O(n), 空间复杂度O(n)
2. 排序, 比较两个相邻元素, 会破坏原数组
时间复杂度O(nlgn), 空间复杂度O(1)
3. 参考 [LeetCode #41 First Missing Positive 缺失的第一个正数]
用下标进行交换, 也会破坏原数组
时间复杂度O(n), 空间复杂度O(1)
4. 参考 [LeetCode #142 Linked List Cycle II 环形链表 II](https://www.jianshu.com/p/c45d340f33c9)
将下标和数组值对应起来
比如 [1,3,4,2,2]
0 -> 1
1 -> 3
2 -> 4
3 -> 2
4 -> 2
如果有重复值, 则会出现 2 -> 4 -> 2这种循环
这时快慢指针可能分别指向 2和 4, 需要找到指针的开头
所以可以类似 [#142 Linked List Cycle II 环形链表 II](https://www.jianshu.com/p/c45d340f33c9)的处理方式, 从头开始遍历一次
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findDuplicate(vector<int>& nums) 
    {
        int fast = nums[nums.front()], result = nums.front();
        while (result != fast)
        {
            result = nums[result];
            fast = nums[nums[fast]];
        }
        for (int i = 0; result != i; i = nums[i]) result = nums[result];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findDuplicate(int[] nums) {
        int fast = nums[nums[0]], result = nums[0];
        while (result != fast) {
            fast = nums[nums[fast]];
            result = nums[result];
        }
        for (int i = 0; result != i; i = nums[i], result = nums[result]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        return (sum(nums) - sum(set(nums))) // (len(nums) - len(set(nums)))
```
