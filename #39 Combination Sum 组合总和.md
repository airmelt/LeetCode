__Description__:
Given a set of candidate numbers (candidates) (without duplicates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

The same repeated number may be chosen from candidates unlimited number of times.

__Note:__

All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.

__Example:__
Example 1:

Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]

Example 2:

Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]

__题目描述__:
给定一个无重复元素的数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的数字可以无限制重复被选取。

__说明:__

所有数字（包括 target）都是正整数。
解集不能包含重复的组合。 

__示例 :__
示例 1:

输入: candidates = [2,3,6,7], target = 7,
所求解集为:
[
  [7],
  [2,2,3]
]

示例 2:

输入: candidates = [2,3,5], target = 8,
所求解集为:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]

__思路__:
回溯法
1. 先对 candidates排序, 保证数组从小到大排列
2. 从 candidates中按大小顺序选出元素加入候选列表
3. 如果 target == 0, 将候选列表加入到结果中
4. 撤销选择
注意这里的元素可以无限重复选择, 所以选择下一层递归函数的时候要从相同下标开始选取
时间复杂度O(n!), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) 
    {
        vector<vector<int>> result;
        vector<int> temp;
        sort(candidates.begin(), candidates.end());
        trackback(result, candidates, target, 0, temp);
        return result;
    }
    
    void trackback(vector<vector<int>> &result, vector<int> &candidates, int target, int start, vector<int> &temp)
    {
        if (target < 0) return;
        if (target == 0) result.push_back(temp);
        for (int i = start; i < candidates.size(); i++)
        {
            temp.push_back(candidates[i]);
            trackback(result, candidates, target - candidates[i], i, temp);
            temp.pop_back();
        }
    }
};
```

__Java__:
```Java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(candidates);
        trackback(result, candidates, target, 0, new ArrayList<>());
        return result;
    }
    
    private void trackback(List<List<Integer>> result, int[] candidates, int target, int start, List<Integer> list) {
        if (target < 0) return;
        if (target == 0) {
            result.add(new ArrayList<>(list));
            return;
        }
        for (int i = start; i < candidates.length; i++) {
            list.add(candidates[i]);
            trackback(result, candidates, target - candidates[i], i, list);
            list.remove(list.size() - 1);
        }
    }
}
```

__Python__:
```Python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        if not target:
            return [[]]
        elif not candidates or target < min(candidates):
            return []
        result = []
        for i in candidates:
            for j in self.combinationSum(list(filter(lambda x: x >= i, candidates)), target - i):
                result.append([i] + j)
        return result
```