# 324 Wiggle Sort II 摆动排序 II

__Description__:
Given an unsorted array nums, reorder it such that nums[0] < nums[1] > nums[2] < nums[3]....

__Example:__

Example 1:

Input: nums = [1, 5, 1, 1, 6, 4]
Output: One possible answer is [1, 4, 1, 5, 1, 6].

Example 2:

Input: nums = [1, 3, 2, 2, 3, 1]
Output: One possible answer is [2, 3, 1, 3, 1, 2].

__Note:__
You may assume all input has valid answer.

__Follow up:__
Can you do it in O(n) time and/or in-place with O(1) extra space?

__题目描述__:
给定一个无序的数组 nums，将它重新排列成 nums[0] < nums[1] > nums[2] < nums[3]... 的顺序。

__示例 :__

示例 1:

输入: nums = [1, 5, 1, 1, 6, 4]
输出: 一个可能的答案是 [1, 4, 1, 5, 1, 6]

示例 2:

输入: nums = [1, 3, 2, 2, 3, 1]
输出: 一个可能的答案是 [2, 3, 1, 3, 1, 2]

__说明:__
你可以假设所有输入都会得到有效的结果。

__进阶：__
你能用 O(n) 时间复杂度和 / 或原地 O(1) 额外空间来实现吗？

__思路__:

1. 排序再交叉插入原数组
比如 [1, 2, 3], [4, 5], 插入之后就可以得到 [1, 4, 2, 5, 3]
时间复杂度O(nlgn), 空间复杂度O(1)
2. 利用快速排序的思想, 找到数组的中位数
然后将数组按中位数分开, 那么中位数左边都比中位数小, 右边都比中位数大
然后将中位数两边的数组反向, 再依次插入就能得到摆动数组
比如 [1, 2, 5, 5, 5, 5, 6, 7] -> [5, 5, 2, 1], [7, 6, 5, 5] -> [5, 7, 5, 6, 2, 5, 1, 5]
时间复杂度O(nlgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    void wiggleSort(vector<int>& nums) 
    {
        nth_element(nums.begin(), nums.begin() + (nums.size() >> 1), nums.end());
        int n = nums.size(), mid = nums[nums.size() >> 1], i = 0, j = 0, k = nums.size() - 1;
        #define f(i) nums[(((i) << 1) + 1) % (n | 1)]
        
        while (j <= k) 
        {
            if (f(j) > mid) 
            {
                swap(f(i), f(j));
                ++i;
                ++j;
            } else if (f(j) < mid) 
            {
                swap(f(j), f(k));
                --k;
            } else ++j;
        }
    }
};
```

__Java__:

```Java
class Solution {
    private void swap(int[] nums, int p, int q) {
        int temp = nums[p];
        nums[p] = nums[q];
        nums[q] = temp;
    }
    
    private int getIndex(int n, int i) {
        return i <= (n - 1) / 2 ? i * 2 : (i - (n + 1) / 2) * 2 + 1;
    }
    
    private int getKth(int[] nums, int k) {
        int n = nums.length, start = 0, end = n - 1;
        while (true) {
            int pivot = nums[getIndex(n, end)], p = start, q = p;
            for (int i = start; i < end; i++)
                if (nums[getIndex(n, i)] <= pivot) {
                    swap(nums, getIndex(n, q++), getIndex(n, i));
                    if (nums[getIndex(n, q - 1)] < pivot) swap(nums, getIndex(n, p++), getIndex(n, q - 1));
                }
            swap(nums, getIndex(n, q++), getIndex(n, end));
            if (k < p - start) end = p - 1;
            else if (k < q - start) return pivot;
            else {
                k -= q - start;
                start = q;
            }
        }
    }
    
    public void wiggleSort(int[] nums) {
        int n = nums.length, mid = (n - 1) / 2;
        getKth(nums, mid);
        for (int p = 0, q = mid; p < q; p++, q--) swap(nums, getIndex(n, p), getIndex(n, q));
        for (int p = mid + 1, q = n - 1; p < q; p++, q--) swap(nums, getIndex(n, p), getIndex(n, q));
    }
}
```

__Python__:

```Python
class Solution:
    def wiggleSort(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        nums[0::2], nums[1::2] = sorted(nums)[:len(nums) - (len(nums) >> 1)][::-1], sorted(nums)[len(nums) - (len(nums) >> 1):][::-1]
```
