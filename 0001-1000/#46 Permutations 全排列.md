# 46 Permutations 全排列

__Description__:
Given a collection of distinct integers, return all possible permutations.

__Example:__

Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]

__题目描述__:
给定一个 没有重复 数字的序列，返回其所有可能的全排列。

__示例 :__

输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]

__思路__:

回溯法

1. 满足结束条件(这里是 list的大小等于 nums数组的大小)就加入到结果中, 并返回
2. 对 nums中的每一个元素进行选择
3. 递归调用
4. 撤销选择
时间复杂度O(n * n!), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> permute(vector<int>& nums) 
    {
        vector<vector<int>> result;
        vector<int> list, visited(nums.size(), 0);
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
        for (int i = 0; i < nums.size(); i++)
        {
            if (visited[i]) continue;
            visited[i] = 1;
            list.push_back(nums[i]);
            helper(result, list, nums, visited);
            list.pop_back();
            visited[i] = 0;
        }
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> list = new ArrayList<>();
        helper(result, list, nums);
        return result;
    }
    
    private void helper(List<List<Integer>> result, List<Integer> list, int[] nums) {
        if (list.size() == nums.length) {
            result.add(new ArrayList<Integer>(list));
            return;
        }
        for (int num : nums) {
            if (!list.contains(num)) {
                list.add(num);
                helper(result, list, nums);
                list.remove(list.size() - 1);
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        return list(itertools.permutations(nums))
```
