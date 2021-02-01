# 462 Minimum Moves to Equal Array Elements II 最少移动次数使数组元素相等 II

__Description__:
Given a non-empty integer array, find the minimum number of moves required to make all array elements equal, where a move is incrementing a selected element by 1 or decrementing a selected element by 1.

You may assume the array's length is at most 10,000.

__Example:__

Input:
[1,2,3]

Output:
2

Explanation:
Only two moves are needed (remember each move increments or decrements one element):

```text
[1,2,3]  =>  [2,2,3]  =>  [2,2,2]
```

__题目描述__:
给定一个非空整数数组，找到使所有数组元素相等所需的最小移动数，其中每次移动可将选定的一个元素加1或减1。 您可以假设数组的长度最多为10000。

__示例 :__

例如:

输入:
[1,2,3]

输出:
2

说明：
只有两个动作是必要的（记得每一步仅可使其中一个元素加1或减1）：

```text
[1,2,3]  =>  [2,2,3]  =>  [2,2,2]
```

__思路__:

找到中位数, 遍历数组求与中位数差的绝对值的和
使用排序之后再查找中位数时间复杂度O(nlgn)
快速排序时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minMoves2(vector<int>& nums) 
    {
        int mid = helper(nums, 0, nums.size() - 1, nums.size() >> 1), result = 0;
        for (auto num : nums) result += abs(num - mid);
        return result;
    }
private:
    int helper(vector<int>& nums, int l, int r, int k)
    {
        if (l == r) return nums[l];
        int m = l + r >> 1, i = l;
        swap(nums[m], nums[r]);
        for (int j = l; j < r; j++) if (nums[j] < nums[r]) swap(nums[i++], nums[j]);
        swap(nums[i], nums[r]);
        return i == k ? nums[i] : (i > k ? helper(nums, l, i - 1, k) : helper(nums, i + 1, r, k));
    }
};
```

__Java__:

```Java
class Solution {
    public int minMoves2(int[] nums) {
        Arrays.sort(nums);
        int i = 0, j = nums.length - 1, result = 0;
        while (i < j) result += nums[j--] - nums[i++];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minMoves2(self, nums: List[int]) -> int:
        return sum(heapq.nlargest(len(nums) >> 1, nums)) - sum(heapq.nsmallest(len(nums) >> 1, nums))
```
