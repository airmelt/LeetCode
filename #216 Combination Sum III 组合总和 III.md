__Description__:
Find all possible combinations of k numbers that add up to a number n, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

__Note:__

All numbers will be positive integers.
The solution set must not contain duplicate combinations.

__Example:__
Example 1:

Input: k = 3, n = 7
Output: [[1,2,4]]

Example 2:

Input: k = 3, n = 9
Output: [[1,2,6], [1,3,5], [2,3,4]]

__题目描述__:
找出所有相加之和为 n 的 k 个数的组合。组合中只允许含有 1 - 9 的正整数，并且每种组合中不存在重复的数字。

__说明：__

所有数字都是正整数。
解集不能包含重复的组合。 

__示例 :__
示例 1:

输入: k = 3, n = 7
输出: [[1,2,4]]
示例 2:

输入: k = 3, n = 9
输出: [[1,2,6], [1,3,5], [2,3,4]]

__思路__:
回溯法
终点为长度等于 n且 target == 0
时间复杂度O(n!), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<vector<int>> combinationSum3(int k, int n) 
    {
        vector<vector<int>> result;
        vector<int> track;
        trackback(result, track, k, 1, n);
        return result;
    }
private:
    void trackback(vector<vector<int>>& result, vector<int>& track, int k, int start, int target) 
    {
        if (target < 0) return;
        if (track.size() == k && target == 0) 
        {
            result.push_back(track);
            return;
        }
        for (int i = start; i < 10; i++) 
        {
            track.push_back(i);
            trackback(result, track, k, i + 1, target - i);
            track.pop_back();
        }
    }
};
```

__Java__:
```Java
class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> result = new ArrayList<>();
        trackback(result, new ArrayList<Integer>(), k, 1, n);
        return result;
    }
    
    private void trackback(List<List<Integer>> result, List<Integer> temp, int k, int start, int target) {
        if (target < 0) return;
        if (temp.size() == k && target == 0) {
            result.add(new ArrayList<>(temp));
            return;
        }
        for (int i = start; i < 10; i++) {
            temp.add(i);
            trackback(result, temp, k, i + 1, target - i);
            temp.remove(temp.size() - 1);
        }
    }
}
```

__Python__:
```Python
class Solution:
    def combinationSum3(self, k: int, n: int, r=range(1, 10)) -> List[List[int]]:
        return [list(i) for i in itertools.combinations(range(1, 10), k) if sum(i) ==  n]
```