# 90 Subsets II 子集 II

__Description__:
Given a collection of integers that might contain duplicates, nums, return all possible subsets (the power set).

__Note:__
The solution set must not contain duplicate subsets.

__Example:__

Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]

__题目描述__:
给定一个可能包含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

__说明：__
解集不能包含重复的子集。

__示例 :__

输入: [1,2,2]
输出:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]

__思路__:

参考[LeetCode #78 Subsets 子集](https://www.jianshu.com/p/a25ff5bceed9)和[LeetCode #47 Permutations II 全排列 II](https://www.jianshu.com/p/669ed9a64a9c)
在回溯的基础上加上去重
时间复杂度O(n \* 2 ^ n), 空间复杂度O(n \* 2 ^ n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        vector<vector<int> > result;
        for (int i = 0; i < 1 << n; ++i) 
        {
            vector<int> temp;
            bool flag = true;
            for (int j = 0; j < n; j++)
            {
                if (i & (1 << j))
                {
                    if (j != 0 and nums[j] == nums[j - 1] and ((i & (1 << (j - 1))) == 0))
                    {
                        flag = false;
                        break;
                    }
                    temp.push_back(nums[j]);
                }
            }
            if (flag) result.push_back(temp);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> result = new ArrayList<>((int)Math.pow(2, nums.length));
        List<Integer> temp = new ArrayList<>(nums.length);
        Arrays.sort(nums);
        backtrack(result, temp, nums, 0);
        return result;
    }
    
    private void backtrack(List<List<Integer>> result, List<Integer> temp, int[] nums, int start) {
        result.add(new ArrayList<>(temp));
        for (int i = start; i < nums.length; i++) {
            if (i != start && nums[i] == nums[i - 1]) continue;
            temp.add(nums[i]);
            backtrack(result, temp, nums, i + 1);
            temp.remove(temp.size() - 1);
        }
    }
}
```

__Python__:

```Python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        result, temp = [], []
        nums.sort()
        def backtrack(start: int) -> None:
            result.append(temp[:])
            for i in range(start, len(nums)):
                if i != start and nums[i] == nums[i - 1]:
                    continue
                temp.append(nums[i])
                backtrack(i + 1)
                temp.pop()
        backtrack(0)
        return result
```
