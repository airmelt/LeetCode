# 457 Circular Array Loop 环形数组循环

__Description__:
You are given a circular array nums of positive and negative integers. If a number k at an index is positive, then move forward k steps. Conversely, if it's negative (-k), move backward k steps. Since the array is circular, you may assume that the last element's next element is the first element, and the first element's previous element is the last element.

Determine if there is a loop (or a cycle) in nums. A cycle must start and end at the same index and the cycle's length > 1. Furthermore, movements in a cycle must all follow a single direction. In other words, a cycle must not consist of both forward and backward movements.

__Example:__

Example 1:

Input: [2,-1,1,2,2]
Output: true
Explanation: There is a cycle, from index 0 -> 2 -> 3 -> 0. The cycle's length is 3.

Example 2:

Input: [-1,2]
Output: false
Explanation: The movement from index 1 -> 1 -> 1 ... is not a cycle, because the cycle's length is 1. By definition the cycle's length must be greater than 1.

Example 3:

Input: [-2,1,-1,-2,-2]
Output: false
Explanation: The movement from index 1 -> 2 -> 1 -> ... is not a cycle, because movement from index 1 -> 2 is a forward movement, but movement from index 2 -> 1 is a backward movement. All movements in a cycle must follow a single direction.

__Note:__

-1000 ≤ nums[i] ≤ 1000
nums[i] ≠ 0
1 ≤ nums.length ≤ 5000

__Follow up:__

Could you solve it in O(n) time complexity and O(1) extra space complexity?

__题目描述__:
给定一个含有正整数和负整数的环形数组 nums。 如果某个索引中的数 k 为正数，则向前移动 k 个索引。相反，如果是负数 (-k)，则向后移动 k 个索引。因为数组是环形的，所以可以假设最后一个元素的下一个元素是第一个元素，而第一个元素的前一个元素是最后一个元素。

确定 nums 中是否存在循环（或周期）。循环必须在相同的索引处开始和结束并且循环长度 > 1。此外，一个循环中的所有运动都必须沿着同一方向进行。换句话说，一个循环中不能同时包括向前的运动和向后的运动。

__示例 :__

示例 1：

输入：[2,-1,1,2,2]
输出：true
解释：存在循环，按索引 0 -> 2 -> 3 -> 0 。循环长度为 3 。

示例 2：

输入：[-1,2]
输出：false
解释：按索引 1 -> 1 -> 1 ... 的运动无法构成循环，因为循环的长度为 1 。根据定义，循环的长度必须大于 1 。

示例 3:

输入：[-2,1,-1,-2,-2]
输出：false
解释：按索引 1 -> 2 -> 1 -> ... 的运动无法构成循环，因为按索引 1 -> 2 的运动是向前的运动，而按索引 2 -> 1 的运动是向后的运动。一个循环中的所有运动都必须沿着同一方向进行。

__提示：__

-1000 ≤ nums[i] ≤ 1000
nums[i] ≠ 0
0 ≤ nums.length ≤ 5000

__进阶：__

你能写出时间时间复杂度为 O(n) 和额外空间复杂度为 O(1) 的算法吗？

__思路__:

修改原数组元素以达到空间复杂度O(1)
遍历数组如果数组元素超过 1000, 说明已经访问过, 不用再次访问, 这样均摊下来每个元素最多访问一次
访问过的元素加上 10000 \* (i + 1) + count, count表示出现的次数
每次访问需要保证路径上的数字正负性一致, 可以用 a \* b > 0检查
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool circularArrayLoop(vector<int>& nums) 
    {
        for (int i = 0; i < nums.size(); i++) 
        {
            if (nums[i] > 1000) continue;
            if (helper(nums, i)) return true;
        }
        return false;
    }
private:
    bool helper(vector<int>& nums, int i) 
    {
        int n = nums.size(), base = nums[i], count = 0, magic = 10000 * (i + 1);
        while (nums[i] <= 1000) 
        {
            int cur = nums[i];
            if (base * cur < 0) return false;
            nums[i] = magic + count;
            ++count;
            i = ((i + cur) % n + n) % n;
        }
        return nums[i] >= magic && count - (nums[i] - magic) > 1;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean circularArrayLoop(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 1000) continue;
            if (helper(nums, i)) return true;
        }
        return false;
    }
    
    private boolean helper(int[] nums, int i) {
        int n = nums.length, base = nums[i], count = 0, magic = 10000 * (i + 1);
        while (nums[i] <= 1000) {
            int cur = nums[i];
            if (base * cur < 0) return false;
            nums[i] = magic + count;
            ++count;
            i = ((i + cur) % n + n) % n;
        }
        return nums[i] >= magic && count - (nums[i] - magic) > 1;
    }
}
```

__Python__:

```Python
class Solution:
    def circularArrayLoop(self, nums: List[int]) -> bool:
        for i in range((n := len(nums))):
            if not nums[i]:
                continue
            slow, fast = i, (nums[i] + i) % n
            while nums[slow] * nums[fast] > 0 and nums[slow] * nums[(next := (fast + nums[fast]) % n)] > 0:
                if slow == fast:
                    if slow == (slow + nums[slow]) % n:
                        break
                    return True
                slow, fast = (slow + nums[slow]) % n, (next + nums[next]) % n
        return False
```
