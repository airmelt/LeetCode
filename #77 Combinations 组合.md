__Description__:
Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.

__Example:__

Input: n = 4, k = 2
Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]

__题目描述__:
给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合。

__示例 :__

输入: n = 4, k = 2
输出:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]

__思路__:
回溯法
时间复杂度O(kC(n, k)), 空间复杂度O(k)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<vector<int>> combine(int n, int k) 
    {
        vector<vector<int>> result;
        vector<int> temp;
        backtrack(result, temp, 1, ++n, k);
        return result;
    }
private:
    void backtrack(vector<vector<int>> &result, vector<int> &temp, const int start, const int n, const int k) 
    {
        if (k == 0) 
        {
            result.push_back(temp);
            return;
        }
        for (int i = start; i < n; i++) 
        {
            temp.push_back(i);
            backtrack(result, temp, i + 1, n, k - 1);
            temp.pop_back();
        }
    }
};
```

__Java__:
```Java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        backtrack(result, temp, 1, ++n, k);
        return result;
    }
    
    private void backtrack(List<List<Integer>> result, List<Integer> temp, int start, int n, int k) {
        if (k == 0) {
            result.add(new ArrayList<Integer>(temp));
            return;
        }
        for (int i = start; i < n; i++) {
            temp.add(i);
            backtrack(result, temp, i + 1, n, k - 1);
            temp.remove(temp.size() - 1);
        }
    }
}
```

__Python__:
```Python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        return list(itertools.combinations(range(1, n + 1), k))
```