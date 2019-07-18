__Description__:
You are given two arrays (without duplicates) nums1 and nums2 where nums1’s elements are subset of nums2. Find all the next greater numbers for nums1's elements in the corresponding places of nums2.

The Next Greater Number of a number x in nums1 is the first greater number to its right in nums2. If it does not exist, output -1 for this number.

__Example:__
Example 1:
Input: nums1 = [4,1,2], nums2 = [1,3,4,2].
Output: [-1,3,-1]
Explanation:
    For number 4 in the first array, you cannot find the next greater number for it in the second array, so output -1.
    For number 1 in the first array, the next greater number for it in the second array is 3.
    For number 2 in the first array, there is no next greater number for it in the second array, so output -1.

Example 2:
Input: nums1 = [2,4], nums2 = [1,2,3,4].
Output: [3,-1]
Explanation:
    For number 2 in the first array, the next greater number for it in the second array is 3.
    For number 4 in the first array, there is no next greater number for it in the second array, so output -1.

__Note:__
All elements in nums1 and nums2 are unique.
The length of both nums1 and nums2 would not exceed 1000.

__题目描述__:
给定两个没有重复元素的数组 nums1 和 nums2 ，其中nums1 是 nums2 的子集。找到 nums1 中每个元素在 nums2 中的下一个比其大的值。

nums1 中数字 x 的下一个更大元素是指 x 在 nums2 中对应位置的右边的第一个比 x 大的元素。如果不存在，对应位置输出-1。

__示例：__
示例 1:

输入: nums1 = [4,1,2], nums2 = [1,3,4,2].
输出: [-1,3,-1]

解释:
    对于num1中的数字4，你无法在第二个数组中找到下一个更大的数字，因此输出 -1。
    对于num1中的数字1，第二个数组中数字1右边的下一个较大数字是 3。
    对于num1中的数字2，第二个数组中没有下一个更大的数字，因此输出 -1。

示例 2:

输入: nums1 = [2,4], nums2 = [1,2,3,4].
输出: [3,-1]
解释:
    对于num1中的数字2，第二个数组中的下一个较大数字是3。
    对于num1中的数字4，第二个数组中没有下一个更大的数字，因此输出 -1。

__注意:__

nums1和nums2中所有元素是唯一的。
nums1和nums2 的数组大小都不超过1000。

__思路__:
可以使用单调栈或者循环存储 nums2中每个元素其后第一个比自身大的元素
时间复杂度O(m ^ 2), 空间复杂度O(m), m为数组 nums2的长度

__代码__:
__C++__:
```
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        int s[1000], top = -1;
        map<int,int> m;
        for (int i = 0; i < nums2.size(); i++) {
            if (top == -1) {
                s[++top] = i;
                continue;
            }
            while (top != -1 && nums2[i] > nums2[s[top]]) m[nums2[s[top--]]] = nums2[i];
            s[++top] = i;
        }
        for (int i = 0; i < nums1.size(); i++) nums1[i] = m.count(nums1[i])? m[nums1[i]] : -1;
        return nums1;
    }
};
```

__Java__:
```
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        Map<Integer, Integer> map = new HashMap<>();
        int[] result = new int[nums1.length];
        for (int i = 0; i < nums2.length; i++) {
            int k = i + 1;
            while (k < nums2.length) {
                if (nums2[k] > nums2[i]) {
                    map.put(nums2[i], nums2[k]);
                    k = nums2.length;
                } else k++;
            }
        }
        for (int i = 0; i < nums1.length; i++) result[i] = map.getOrDefault(nums1[i], -1);
        return result;
    }
}
```

__Python__:
```
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        d = {}
        result = nums1[:]
        for i, num in enumerate(nums2):
            while i < len(nums2) - 1:
                if nums2[i + 1] > num:
                    d[num] = nums2[i + 1]
                    i = len(nums2) - 1
                else:
                    i += 1
        for i in range(len(nums1)):
            result[i] = d.get(nums1[i], -1)
        return result
```
