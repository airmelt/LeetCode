# 540 Single Element in a Sorted Array 有序数组中的单一元素

__Description__:
You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once. Find this single element that appears only once.

__Follow up:__
Your solution should run in O(log n) time and O(1) space.

__Example:__

Example 1:

Input: nums = [1,1,2,3,3,4,4,8,8]
Output: 2

Example 2:

Input: nums = [3,3,7,7,10,11,11]
Output: 10

__Constraints:__

1 <= nums.length <= 10^5
0 <= nums[i] <= 10^5

__题目描述__:
给定一个只包含整数的有序数组，每个元素都会出现两次，唯有一个数只会出现一次，找出这个数。

__示例 :__

示例 1:

输入: [1,1,2,3,3,4,4,8,8]
输出: 2

示例 2:

输入: [3,3,7,7,10,11,11]
输出: 10

__注意:__
您的方案应该在 O(log n)时间复杂度和 O(1)空间复杂度中运行。

__思路__:

因为每个数组元素要么出现 1 次, 要么出现 2 次, 且只有一个元素出现了 1 次
所以数组长度一定为奇数
由于数组有序, 相同的元素一定在相邻位置上, 那么独特的元素一定在偶数下标位置
所以只需要检查偶数下标
注意到数组有序, 考虑使用二分法
取 mid = (l + r) / 2, 如果 mid 为奇数, 自减移动到偶数下标位置
检查 nums[mid] == nums[mid + 1], 如果成立说明 mid 之前的元素都是成对的, 令 l = mid + 2
否则 r = mid
直到搜索区间只剩下一个元素
时间复杂度 O(n), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int singleNonDuplicate(vector<int>& nums) 
    {
        int l = 0, r = nums.size() - 1;
        while (l < r)
        {
            int mid = l + (r - l >> 1);
            if (mid & 1) --mid;
            if (nums[mid] == nums[mid + 1]) l = mid + 2;
            else r = mid;
        }
        return nums[l];
    }
};
```

__Java__:

```Java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int l = 0, r = nums.length - 1;
        while (l < r) {
            int mid = l + (r - l >>> 1);
            if ((mid & 1) == 1) --mid;
            if (nums[mid] == nums[mid + 1]) l = mid + 2;
            else r = mid;
        }
        return nums[l];
    }
}
```

__Python__:

```Python
class Solution:
    def singleNonDuplicate(self, nums: List[int]) -> int:
        return sum(set(nums[::2]) - set(nums[1::2]))
```
