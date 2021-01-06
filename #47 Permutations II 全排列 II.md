# 47 Permutations II 全排列 II

__Description__:
Given a collection of numbers that might contain duplicates, return all possible unique permutations.

__Example:__

Input: [1,1,2]
Output:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]

__题目描述__:
给定一个可包含重复数字的序列，返回所有不重复的全排列。

__示例 :__

输入: [1,1,2]
输出:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]

__思路__:

回溯法
参考[LeetCode #46 Permutations 全排列](https://www.jianshu.com/p/603134948f0f)

1. 排序
2. 剪枝去掉重复的排列
时间复杂度O(n * n!), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) 
    {
        vector<vector<int>> result;
        vector<int> list, visited(nums.size(), 0);
        sort(nums.begin(), nums.end());
        helper(result, list, nums, visited);
        return result;
    }
private:
    void helper(vector<vector<int>>& result, vector<int> list, vector<int>& nums, vector<int>& visited) 
    {
        if (list.size() == nums.size())
        {
            result.push_back(list);
            return;
        }
        long flag = 2147483648L;
        for (int i = 0; i < nums.size(); i++)
        {
            if (visited[i]) continue;
            if ((long)nums[i] != flag) 
            {
                flag = (long)nums[i];
                visited[i] = 1;
                list.push_back(nums[i]);
                helper(result, list, nums, visited);
                list.pop_back();
                visited[i] = 0;
            }
        }
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> list = new ArrayList<>();
        boolean visited[] = new boolean[nums.length];
        Arrays.sort(nums);
        helper(result, list, nums, visited);
        return result;
    }
    
    private void helper(List<List<Integer>> result, List<Integer> list, int[] nums, boolean[] visited) {
        if (list.size() == nums.length) {
            result.add(new ArrayList<Integer>(list));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (visited[i]) continue;
            if (i > 0 && nums[i] == nums[i - 1] && !visited[i - 1]) continue;
            visited[i] = true;
            list.add(nums[i]);
            helper(result, list, nums, visited);
            list.remove(list.size() - 1);
            visited[i] = false;
        }
    }
}
```

__Python__:

```Python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        result, l, visited = [], [], [False for _ in range(len(nums))]
        nums.sort()
        def helper(result: List[List[int]], l: List[int], nums: List[int], visited: List[bool]) -> None:
            if len(l) == len(nums):
                result.append(l[:])
                return
            for i in range(len(nums)):
                if visited[i]:
                    continue
                if i and nums[i] == nums[i - 1] and not visited[i - 1]:
                    continue
                visited[i] = True
                l.append(nums[i])
                helper(result, l, nums, visited)
                l.pop(-1)
                visited[i] = False
        helper(result, l, nums, visited)
        return result
```
