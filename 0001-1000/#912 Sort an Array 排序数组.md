# 912 Sort an Array 排序数组

__Description__:
Given an array of integers nums, sort the array in ascending order.

__Example:__

Example 1:

Input: nums = [5,2,3,1]
Output: [1,2,3,5]

Example 2:

Input: nums = [5,1,1,2,0,0]
Output: [0,0,1,1,2,5]

__Constraints:__

1 <= nums.length <= 5 \* 10^4
-5 \* 10^4 <= nums[i] <= 5 \* 10^4

__题目描述__:
给你一个整数数组 nums，请你将该数组升序排列。

__示例 :__

示例 1：

输入：nums = [5,2,3,1]
输出：[1,2,3,5]

示例 2：

输入：nums = [5,1,1,2,0,0]
输出：[0,0,1,1,2,5]

__提示:__

1 <= nums.length <= 50000
-50000 <= nums[i] <= 50000

__思路__:

快速排序
递归地将比 pivot 小的都放在 pivot 左边, 比 pivot 大的都放在 pivot 右边
时间复杂度为 O(nlgn), 空间复杂度为 O(lgn), 空间为递归栈的深度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> sortArray(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        return nums;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] sortArray(int[] nums) {
        quicksort(nums, 0, nums.length - 1);
        return nums;
    }
    
    private void quicksort(int[] nums, int left, int right) {
        if (left >= right) return;
        int pivot = partition(nums, left, right);
        quicksort(nums, left, pivot - 1);
        quicksort(nums, pivot + 1, right);
    }
    
    private int partition(int[] nums, int left, int right) {
        int pivot = nums[left];
        while (left < right) {
            while (left < right && nums[right] >= pivot) --right;
            nums[left] = nums[right];
            while (left < right && nums[left] <= pivot) ++left;
            nums[right] = nums[left];
        }
        nums[left] = pivot;
        return left;
    }
}
```

__Python__:

```Python
class TopVotedCandidate:

    def __init__(self, persons: List[int], times: List[int]):
        self.times, self.winner, d, max_value, cur_winner = times, [], defaultdict(int), 0, persons[0]
        for person in persons:
            d[person] += 1
            if d[person] >= max_value:
                cur_winner = person
                max_value = d[person]
            self.winner.append(cur_winner)


    def q(self, t: int) -> int:
        return self.winner[bisect_right(self.times, t) - 1]


# Your TopVotedCandidate object will be instantiated and called as such:
# obj = TopVotedCandidate(persons, times)
# param_1 = obj.q(t)
```
