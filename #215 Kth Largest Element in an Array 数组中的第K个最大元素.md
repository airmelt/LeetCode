__Description__:
Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

__Example:__
Example 1:

Input: [3,2,1,5,6,4] and k = 2
Output: 5

Example 2:

Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4

__Note:__
You may assume k is always valid, 1 ≤ k ≤ array's length.

__题目描述__:
在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

__示例 :__
示例 1:

输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
示例 2:

输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4

__说明：__

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。

__思路__:
1. 排序
时间复杂度O(nlgn), 空间复杂度O(1)
2. 小根堆
时间复杂度O(n), 空间复杂度O(n)
3. 快速排序
注意加入随机数下标, 以保证时间复杂度最优
时间复杂度O(n), 空间复杂度O(lgn)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int findKthLargest(vector<int>& nums, int k) 
    {
        priority_queue<int,vector<int>, less<int>> q;
        for (int i = 0; i < nums.size(); i++) q.push(nums[i]);
        while (k-- > 1) q.pop();
        return q.top();
    }
};
```

__Java__:
```Java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        return findKthLargest(nums, 0, nums.length - 1, k);
    }
    
    private int findKthLargest(int[] nums, int left, int right, int k) {
        int index = partition(nums, left, right);
        if (index + 1 == k) return nums[index];
        if (index + 1 < k) return findKthLargest(nums, index + 1, right, k);
        return findKthLargest(nums, left, index - 1, k);
    }
    
    private int partition(int[] nums, int left, int right) {
        int random = (int)(Math.random() * (right - left + 1) + left), i = left, j = left + 1, temp = 0;
        swap(nums, left, random, temp);
        while (j <= right) {
            if (nums[j] >= nums[left]) swap(nums, j, ++i, temp);
            ++j;
        }
        swap(nums, left, i, temp);
        return i;
    }
    
    private void swap(int[] nums, int i, int j, int temp) {
        temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

__Python__:
```Python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        return sorted(nums)[-k]
```