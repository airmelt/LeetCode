# 1144 Decrease Elements To Make Array Zigzag 递减元素使数组呈锯齿状

__Description__:
Given an array nums of integers, a move consists of choosing any element and decreasing it by 1.

An array A is a zigzag array if either:

Every even-indexed element is greater than adjacent elements, ie. A[0] > A[1] < A[2] > A[3] < A[4] > ...
OR, every odd-indexed element is greater than adjacent elements, ie. A[0] < A[1] > A[2] < A[3] > A[4] < ...
Return the minimum number of moves to transform the given array nums into a zigzag array.

__Example:__

Example 1:

Input: nums = [1,2,3]
Output: 2
Explanation: We can decrease 2 to 0 or 3 to 1.

Example 2:

Input: nums = [9,6,1,6,2]
Output: 4

__Constraints:__

1 <= nums.length <= 1000
1 <= nums[i] <= 1000

__题目描述__:
给你一个整数数组 nums，每次 操作 会从中选择一个元素并 将该元素的值减少 1。

如果符合下列情况之一，则数组 A 就是 锯齿数组：

每个偶数索引对应的元素都大于相邻的元素，即 A[0] > A[1] < A[2] > A[3] < A[4] > ...
或者，每个奇数索引对应的元素都大于相邻的元素，即 A[0] < A[1] > A[2] < A[3] > A[4] < ...
返回将数组 nums 转换为锯齿数组所需的最小操作次数。

__示例 :__

示例 1：

输入：nums = [1,2,3]
输出：2
解释：我们可以把 2 递减到 0，或把 3 递减到 1。

示例 2：

输入：nums = [9,6,1,6,2]
输出：4

__提示:__

1 <= nums.length <= 1000
1 <= nums[i] <= 1000

__思路__:

模拟
要么将所有奇数位置减少到比相邻偶数位置上的数少 1
要么将所有偶数位置减少到比奇数相邻位置上的数少 1
除了两端需要单独考虑
分别求出奇数位置上和偶数位置上需要改变的数之和取较小的即可
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int movesToMakeZigzag(vector<int>& nums) 
    {
        vector<int> t(2);
        for (int i = 0, n = nums.size(); i < n; i++) t[i & 1] += max(0, nums[i] - min(i ? nums[i - 1] : INT_MAX, i < n - 1 ? nums[i + 1] : INT_MAX) + 1);
        return min(t.front(), t.back());
    }
};
```

__Java__:

```Java
class Solution {
    public int movesToMakeZigzag(int[] nums) {
        int t[] = new int[2], n = nums.length;
        for (int i = 0; i < n; i++) t[i & 1] += Math.max(0, nums[i] - Math.min(i > 0 ? nums[i - 1] : Integer.MAX_VALUE, i < n - 1 ? nums[i + 1] : Integer.MAX_VALUE) + 1);
        return Math.min(t[0], t[1]);
    }
}
```

__Python__:

```Python
class Solution:
    def movesToMakeZigzag(self, nums: List[int]) -> int:
        n, t = len(nums), [0, 0]
        for i in range(n):
            t[i & 1] += max(0, nums[i] - min(nums[i - 1] if i else float('inf'), nums[i + 1] if i < n - 1 else float('inf')) + 1)
        return min(t)
```
