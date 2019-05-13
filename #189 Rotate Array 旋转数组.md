__Description__:
Given an array, rotate the array to the right by k steps, where k is non-negative.

__Example__:
Example 1:
Input: [1,2,3,4,5,6,7] and k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]

Example 2:
Input: [-1,-100,3,99] and k = 2
Output: [3,99,-1,-100]
Explanation:
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]

__Note__:
Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.
Could you do it in-place with O(1) extra space?

__题目描述__:
给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。

__示例__:
示例 1:
输入: [1,2,3,4,5,6,7] 和 k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]

示例 2:
输入: [-1,-100,3,99] 和 k = 2
输出: [3,99,-1,-100]
解释:
向右旋转 1 步: [99,-1,-100,3]
向右旋转 2 步: [3,99,-1,-100]

__说明__:
尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
要求使用空间复杂度为 O(1) 的原地算法。

__思路__:
1. 先将数组反转, 然后分别将[0, k) 和 [k, nums.size())部分反转
2. 每次取出最后一个元素放到第一个元素的位置
3. 递归交换
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int len = nums.size();
        k %= len;
        reverse(nums, 0, len - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, len - 1);
    }
private:
    void reverse(vector<int>& nums, int start, int end) {
        while (start < end) {
            swap(nums[start++], nums[end--]);
        }
    }
};
```

__Java__:
```
class Solution {
    public void rotate(int[] nums, int k) {
        recursiveSwap(nums, k, 0, nums.length);
    }

    private void recursiveSwap(int[] nums, int k, int start, int length) {
        k %= length;
        if (k != 0) {
            for (int i = 0; i < k; i++) swap(nums, start + i, nums.length - k + i);
            recursiveSwap(nums, k, start + k, length - k);
        }
    }

    private void swap(int[] nums, int i, int j) {
        nums[i] ^= nums[j];
        nums[j] ^= nums[i];
        nums[i] ^= nums[j];
    }
}
```

__Python__:
```
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        for i in range(k):
            num = nums.pop()
            nums.insert(0, num)
```
