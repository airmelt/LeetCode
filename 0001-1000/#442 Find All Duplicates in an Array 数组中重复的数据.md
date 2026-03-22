# 442 Find All Duplicates in an Array 数组中重复的数据

__Description__:

Given an array of integers, 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

Find all the elements that appear twice in this array.

Could you do it without extra space and in O(n) runtime?

__Example:__

Input:
[4,3,2,7,8,2,3,1]

Output:
[2,3]

__题目描述__:

给定一个整数数组 a，其中1 ≤ a[i] ≤ n （n为数组长度）, 其中有些元素出现两次而其他元素出现一次。

找到所有出现两次的元素。

你可以不用到任何额外空间并在O(n)时间复杂度内解决这个问题吗？

__示例 :__

输入:
[4,3,2,7,8,2,3,1]

输出:
[2,3]

__思路__:

注意到 0 <= a[i] <= n
将 i对应的元素取反, 如果遍历到的元素为负数说明出现过, 加入结果中
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> findDuplicates(vector<int>& nums) 
    {
        vector<int> result;
        for (auto num : nums) 
        {
            nums[abs(num) - 1] *= -1;
            if (nums[abs(num) - 1] > 0) result.emplace_back(abs(num));
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        List<Integer> result = new ArrayList<>();
        for (int num : nums) {
            nums[Math.abs(num) - 1] *= -1;
            if (nums[Math.abs(num) - 1] > 0) result.add(Math.abs(num));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findDuplicates(self, nums: List[int]) -> List[int]:
        return [x[0] for x in Counter(nums).items() if x[1] > 1]
```
