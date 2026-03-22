# 491 Increasing Subsequences 递增子序列

__Description__:
Given an integer array, your task is to find all the different possible increasing subsequences of the given array, and the length of an increasing subsequence should be at least 2.

__Example:__

Input: [4, 6, 7, 7]
Output: [[4, 6], [4, 7], [4, 6, 7], [4, 6, 7, 7], [6, 7], [6, 7, 7], [7,7], [4,7,7]]

__Constraints:__

The length of the given array will not exceed 15.
The range of integer in the given array is [-100,100].
The given array may contain duplicates, and two equal integers should also be considered as a special case of increasing sequence.

__题目描述__:
给定一个整型数组, 你的任务是找到所有该数组的递增子序列，递增子序列的长度至少是2。

__示例 :__

输入: [4, 6, 7, 7]
输出: [[4, 6], [4, 7], [4, 6, 7], [4, 6, 7, 7], [6, 7], [6, 7, 7], [7,7], [4,7,7]]

__说明:__

给定数组的长度不会超过15。
数组中的整数范围是 [-100,100]。
给定数组中可能包含重复数字，相等的数字应该被视为递增的一种情况。

__思路__:

回溯
由于需要不递减元素, 选择放入元素时需要不小于原来放入的元素
这时, 比如 4, 6, 7, 7, 第一个和第二个 7都会加入
这时分为 4种情况, 用二进制来表示就是, 11, 10, 01, 00, 第二种和第三种就是重复的
所以还需要增加一个判断条件, 当前元素和之前的元素不相等才加入, 这样就能去重
时间复杂度O(2 ^ n * n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    vector<int> temp; 
    vector<vector<int>> result;

    void dfs(int cur, int last, vector<int>& nums) 
    {
        if (cur == nums.size()) 
        {
            if (temp.size() > 1) result.emplace_back(temp);
            return;
        }
        if (nums[cur] >= last) 
        {
            temp.emplace_back(nums[cur]);
            dfs(cur + 1, nums[cur], nums);
            temp.pop_back();
        }
        if (nums[cur] != last) dfs(cur + 1, last, nums);
    }
public:
    vector<vector<int>> findSubsequences(vector<int>& nums) 
    {
        dfs(0, INT_MIN, nums);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> findSubsequences(int[] nums) {
        dfs(0, Integer.MIN_VALUE, nums);
        return result;
    }
    
    private List<Integer> temp = new ArrayList<>();
    private List<List<Integer>> result = new ArrayList<>();
    
    private void dfs(int cur, int last, int[] nums) {
        if (cur == nums.length) {
            if (temp.size() > 1) result.add(new ArrayList<>(temp));
            return;
        }
        if (nums[cur] >= last) {
            temp.add(nums[cur]);
            dfs(cur + 1, nums[cur], nums);
            temp.remove(temp.size() - 1);
        }
        if (nums[cur] != last) dfs(cur + 1, last, nums);
    }
}
```

__Python__:

```Python
class Solution:
    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        d = {}
        for num in nums:
            temp = [[num]]
            for k in d:
                if k <= num:
                    for i in d[k]:
                        temp.append(i + [num])
            d[num] = temp
        return list(itertools.chain(*[d[i][1:] for i in d if len(d[i]) > 1]))
```
