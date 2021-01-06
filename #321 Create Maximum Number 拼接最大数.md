# 321 Create Maximum Number 拼接最大数

__Description__:
Given two arrays of length m and n with digits 0-9 representing two numbers. Create the maximum number of length k <= m + n from digits of the two. The relative order of the digits from the same array must be preserved. Return an array of the k digits.

__Note:__
You should try to optimize your time and space complexity.

__Example:__

Example 1:

Input:
nums1 = [3, 4, 6, 5]
nums2 = [9, 1, 2, 5, 8, 3]
k = 5
Output:
[9, 8, 6, 5, 3]

Example 2:

Input:
nums1 = [6, 7]
nums2 = [6, 0, 4]
k = 5
Output:
[6, 7, 6, 0, 4]

Example 3:

Input:
nums1 = [3, 9]
nums2 = [8, 9]
k = 3
Output:
[9, 8, 9]

__题目描述__:
给定长度分别为 m 和 n 的两个数组，其元素由 0-9 构成，表示两个自然数各位上的数字。现在从这两个数组中选出 k (k <= m + n) 个数字拼接成一个新的数，要求从同一个数组中取出的数字保持其在原数组中的相对顺序。

求满足该条件的最大数。结果返回一个表示该最大数的长度为 k 的数组。

__说明:__
请尽可能地优化你算法的时间和空间复杂度。

__示例 :__

示例 1:

输入:
nums1 = [3, 4, 6, 5]
nums2 = [9, 1, 2, 5, 8, 3]
k = 5
输出:
[9, 8, 6, 5, 3]

示例 2:

输入:
nums1 = [6, 7]
nums2 = [6, 0, 4]
k = 5
输出:
[6, 7, 6, 0, 4]

示例 3:

输入:
nums1 = [3, 9]
nums2 = [8, 9]
k = 3
输出:
[9, 8, 9]

__思路__:

分治
考虑从一个数组中选出最大的 k个数
可以使用单调栈/双指针实现, 先填充 j个数字到 result数组中, n - i表示 nums数组中还未被选中的部分, j表示已经选中的数, 如果 nums[i]大于已经选中的数, 就倒退指针, 这样就能选出最大的 k个数
这样, 如果从 2个数组中选出最大的 k个数
可以考虑 k分成 0 + k, 1 + (k - 1), ..., k + 0, 第一个数组中选出 i个数, 第二个数组中选出 k - i个数, 选出最大的结果即可
时间复杂度O(k ^ 2 * max(m, n)), 空间复杂度O(max(m, n, k)), m为数组 nums1的长度, n为数组 nums2的长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> maxNumber(vector<int>& nums1, vector<int>& nums2, int k) 
    {
        vector<int> result(k);
        int m = nums1.size(), n = nums2.size();
        for (int i = max(0, k - n); i <= k and i <= m; i++)
        {
            vector<int> pick1 = pick_max(nums1, i), pick2 = pick_max(nums2, k - i);
            vector<int> temp = merge(pick1, pick2, k);
            if (greater(temp, 0, result, 0)) result = temp;
        }
        return result;
    }
private:
    vector<int> pick_max(vector<int> &nums, int k)
    {
        vector<int> result(k);
        int n = nums.size();
        for (int i = 0, j = 0; i < n; i++)
        {
            while (n - i + j > k and j > 0 and nums[i] > result[j - 1]) --j;
            if (j < k) result[j++] = nums[i];
        }
        return result;
    }
    
    vector<int> merge(vector<int> &nums1, vector<int> &nums2, int k)
    {
        vector<int> result(k);
        for (int i = 0, j = 0, r = 0; r < k; r++) result[r] = greater(nums1, i, nums2, j) ? nums1[i++] : nums2[j++];
        return result;
    }
    
    bool greater(vector<int> &nums1, int i, vector<int> &nums2, int j)
    {
        while (i < nums1.size() and j < nums2.size() and nums1[i] == nums2[j])
        {
            ++i;
            ++j;
        }
        return j == nums2.size() or (i < nums1.size() and nums1[i] > nums2[j]);
    }
};
```

__Java__:

```Java
class Solution {
    public int[] maxNumber(int[] nums1, int[] nums2, int k) {
        int result[] = new int[k], m = nums1.length, n = nums2.length;
        for (int i = Math.max(0, k - n); i <= k && i <= m; i++) {
            int temp[] = merge(pickMax(nums1, i), pickMax(nums2, k - i), k);
            if (greater(temp, 0, result, 0)) result = temp;
        }
        return result;
    }
    
    private int[] pickMax(int[] nums, int k) {
        int result[] = new int[k], n = nums.length;
        for (int i = 0, j = 0; i < n; i++) {
            while (n - i + j > k && j > 0 && nums[i] > result[j - 1]) --j;
            if (j < k) result[j++] = nums[i];
        }
        return result;
    }
    
    private int[] merge(int[] nums1, int[] nums2, int k) {
        int result[] = new int[k];
        for (int i = 0, j = 0, r = 0; r < k; r++) result[r] = greater(nums1, i, nums2, j) ? nums1[i++] : nums2[j++];
        return result;
    }
    
    private boolean greater(int[] nums1, int i, int[] nums2, int j) {
        while (i < nums1.length && j < nums2.length && nums1[i] == nums2[j]) {
            ++i;
            ++j;
        }
        return j == nums2.length || (i < nums1.length && nums1[i] > nums2[j]);
    }
}
```

__Python__:

```Python
class Solution:
    def maxNumber(self, nums1: List[int], nums2: List[int], k: int) -> List[int]:
        def pick_max(nums: List[int], k: int) -> List[int]:
            result, drop = [], len(nums) - k
            for num in nums:
                while drop and result and result[-1] < num:
                    result.pop()
                    drop -= 1
                result.append(num)
            return result[:k]
        def merge(nums1: List[int], nums2: List[int]) -> List[int]:
            result = []
            while nums1 or nums2:
                greater = nums1 if nums1 > nums2 else nums2
                result.append(greater.pop(0))
            return result
        return max(merge(pick_max(nums1, i), pick_max(nums2, k - i)) for i in range(k + 1) if i <= len(nums1) and k - i <= len(nums2))
```
