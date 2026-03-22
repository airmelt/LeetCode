# 448 Find All Numbers Disappeared in an Array 找到所有数组中消失的数字

__Description__:
Given an array of integers where 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

Find all the elements of [1, n] inclusive that do not appear in this array.

Could you do it without extra space and in O(n) runtime? You may assume the returned list does not count as extra space.

__Example:__

Input:
[4,3,2,7,8,2,3,1]

Output:
[5,6]

__题目描述__:
给定一个范围在  1 ≤ a[i] ≤ n ( n = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。

找到所有在 [1, n] 范围之间没有出现在数组中的数字。

您能在不使用额外空间且时间复杂度为O(n)的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。

__示例：__

输入:
[4,3,2,7,8,2,3,1]

输出:
[5,6]

__思路__:

用出现过的数字对应下标, 将下标对应的数组元素改为负数
遍历第二次找出所有正数
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) 
    {
        for (int i = 0; i < nums.size(); i++) nums[abs(nums[i]) - 1] = -abs(nums[abs(nums[i]) - 1]);
        vector<int> result;
        for (int i = 0; i < nums.size(); i++) if (nums[i] > 0) result.emplace_back(i + 1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        for (int i = 0; i < nums.length; i++) nums[Math.abs(nums[i]) - 1] = -Math.abs(nums[Math.abs(nums[i]) - 1]);
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0) result.add(i + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findDisappearedNumbers(self, nums: List[int]) -> List[int]:
        return list(set(range(1, len(nums) + 1))- set(nums)) if nums else []
```
