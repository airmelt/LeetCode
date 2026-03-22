# 78 Subsets 子集

__Description__:
Given a set of distinct integers, nums, return all possible subsets (the power set).

__Note:__
The solution set must not contain duplicate subsets.

__Example:__

Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]

__题目描述__:
给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

__说明：__
解集不能包含重复的子集。

__示例 :__

输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]

__思路__:

1. 回溯法
时间复杂度O(n \* 2 ^ n), 空间复杂度O(n \* 2 ^ n)
2. 递归法, 遍历列表, 将结果中的每一个子集都加上新遍历的元素
时间复杂度O(n \* 2 ^ n), 空间复杂度O(n \* 2 ^ n)
3. 二进制掩码法, 由于对于每一个元素来说都有选择加入或者不选择加入两种, 生成 nums.size()位的二进制掩码即可
时间复杂度O(n \* 2 ^ n), 空间复杂度O(n \* 2 ^ n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> subsets(vector<int>& nums) 
    {
        vector<vector<int>> result;
        vector<int> temp;
        backtrack(result, temp, nums, 0);
        return result;
    }
private:
    void backtrack(vector<vector<int>> &result, vector<int> &temp, vector<int>& nums, const int start)
    {
        result.push_back(temp);
        for (int i = start; i < nums.size(); i++)
        {
            temp.push_back(nums[i]);
            backtrack(result, temp, nums, i + 1);
            temp.pop_back();
        }
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>((int)Math.pow(2, nums.length));
        result.add(new ArrayList<Integer>());
        for (int num : nums) {
            List<List<Integer>> temp = new ArrayList<>();
            for (List<Integer> cur : result) temp.add(new ArrayList<Integer>(cur){{add(num);}});
            result.addAll(temp);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        return [list(filter(lambda x: subarray[nums.index(x)] == 1, nums)) for subarray in [[int(j) for j in bin(i)[2:].zfill(len(nums))] for i in range(2 ** len(nums))]]
```
