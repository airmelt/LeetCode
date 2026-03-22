# 503 Next Greater Element II 下一个更大元素 II

__Description__:
Given a circular array (the next element of the last element is the first element of the array), print the Next Greater Number for every element. The Next Greater Number of a number x is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, output -1 for this number.

__Example:__

Example 1:
Input: [1,2,1]
Output: [2,-1,2]
Explanation: The first 1's next greater number is 2;
The number 2 can't find next greater number;
The second 1's next greater number needs to search circularly, which is also 2.

__Note:__
The length of given array won't exceed 10000.

__题目描述__:
给定一个循环数组（最后一个元素的下一个元素是数组的第一个元素），输出每个元素的下一个更大元素。数字 x 的下一个更大的元素是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 -1。

__示例 :__

示例 1:

输入: [1,2,1]
输出: [2,-1,2]
解释: 第一个 1 的下一个更大的数是 2；
数字 2 找不到下一个更大的数；
第二个 1 的下一个最大的数需要循环搜索，结果也是 2。

__注意:__
输入数组的长度不会超过 10000。

__思路__:

单调栈
单调栈中保存不升的下标
遍历两次数组获取下一个更大元素
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> nextGreaterElements(vector<int>& nums) 
    {
        vector<int> result(nums.size(), -1);
        stack<int> s;
        for (int i = 0, n = nums.size(); i < n * 2 - 1; i++) 
        {
            while (!s.empty() and nums[i % n] > nums[s.top()]) 
            {
                result[s.top()] = nums[i % n];
                s.pop();
            }
            if (i < n) s.push(i);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        int result[] = new int[nums.length], n = nums.length;
        Arrays.fill(result, -1);
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < n * 2 - 1; i++) {
            while (!stack.isEmpty() && nums[i % n] > nums[stack.peek()]) result[stack.pop()] = nums[i % n];
            if (i < n) stack.push(i);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        result, stack = [-1] * (n := len(nums)), []
        for i, num in enumerate((nums := nums + nums)):
            while stack and nums[stack[-1]] < num:
                result[stack.pop()] = num
            i < n and stack.append(i)
        return result
```
