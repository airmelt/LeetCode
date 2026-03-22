# 167 Two Sum II - Input array is sorted 两数之和 II - 输入有序数组

__Description__:
Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

__Note__:
Your returned answers (both index1 and index2) are not zero-based.
You may assume that each input would have exactly one solution and you may not use the same element twice.

__Example__:

Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.

__题目描述__:
给定一个已按照升序排列 的有序数组，找到两个数使得它们相加之和等于目标数。

函数应该返回这两个下标值 index1 和 index2，其中 index1 必须小于 index2。

__说明__:
返回的下标值（index1 和 index2）不是从零开始的。
你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。

__示例__:

输入: numbers = [2, 7, 11, 15], target = 9
输出: [1,2]
解释: 2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。

__思路__:

1. 可以参考[LeetCode #1 Two Sum 两数之和](https://www.jianshu.com/p/09dbccebd6bf), 使用map求解
2. 由于数组已经排序, 用双指针法相向扫描
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> twoSum(vector<int>& numbers, int target) 
    {
        int i = 0, j = numbers.size() - 1;
        vector<int> result;
        while (i < j) 
        {
            if (numbers[i] + numbers[j] == target) 
            {
                result.push_back(i + 1);
                result.push_back(j + 1);
                return result;
            }
            else if (numbers[i] + numbers[j] < target) ++i;
            else --j;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int i = 0;
        int j = numbers.length - 1;
        while (i < j) {
            if (numbers[i] + numbers[j] == target) return new int[]{i + 1, j + 1};
            else if (numbers[i] + numbers[j] < target) i++;
            else j--;
        }
        return null;
    }
}
```

__Python__:

```Python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        temp = {}
        for i, v in enumerate(numbers):
            if target - v in temp:
                return [temp[target - v], i + 1]
            if not v in temp:
                temp[v] = i + 1
```
