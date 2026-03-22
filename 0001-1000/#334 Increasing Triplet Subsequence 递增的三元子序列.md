# 334 Increasing Triplet Subsequence 递增的三元子序列

__Description__:
Given an unsorted array return whether an increasing subsequence of length 3 exists or not in the array.

Formally the function should:

Return true if there exists i, j, k
such that arr[i] < arr[j] < arr[k] given 0 ≤ i < j < k ≤ n-1 else return false.

__Note:__
Your algorithm should run in O(n) time complexity and O(1) space complexity.

__Example:__

Example 1:

Input: [1,2,3,4,5]
Output: true

Example 2:

Input: [5,4,3,2,1]
Output: false

__题目描述__:
给定一个未排序的数组，判断这个数组中是否存在长度为 3 的递增子序列。

数学表达式如下:

如果存在这样的 i, j, k,  且满足 0 ≤ i < j < k ≤ n-1，
使得 arr[i] < arr[j] < arr[k] ，返回 true ; 否则返回 false 。

__说明:__
要求算法的时间复杂度为 O(n)，空间复杂度为 O(1) 。

__示例 :__

示例 1:

输入: [1,2,3,4,5]
输出: true

示例 2:

输入: [5,4,3,2,1]
输出: false

__思路__:

记录最小值 min和比最小值大的一个值 mid
如果在后面找到一个比 mid大的值, 返回 true
遍历完还没找到返回 false
比如 min = 2, mid = 3, 如果找到一个值, 1更新 min = 1, 如果后面找到大于 mid的值 4, 则可以满足 234, 如果后面找到大于 min的值 2, 更新 mid = 2, 还是可以继续找递增子序列
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool increasingTriplet(vector<int>& nums) 
    {
        int a = INT_MAX, b = INT_MAX;
        for (auto &num : nums) 
        {
            if (num <= a) a = num;
            else if (num <= b) b = num;
            else return true;
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        int a = Integer.MAX_VALUE, b = Integer.MAX_VALUE;
        for (int num : nums) {
            if (num <= a) a = num;
            else if (num <= b) b = num;
            else return true;
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def increasingTriplet(self, nums: List[int]) -> bool:
        a, b = float('inf'), float('inf')
        for num in nums:
            if num <= a:
                a = num
            elif num <= b:
                b = num
            else:
                return True
        return False
```
