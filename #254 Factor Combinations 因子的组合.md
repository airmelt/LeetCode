# 254 Factor Combinations 因子的组合

__Description:__

Numbers can be regarded as the product of their factors.

For example, 8 = 2 x 2 x 2 = 2 x 4.
Given an integer n, return all possible combinations of its factors. You may return the answer in any order.

Note that the factors should be in the range [2, n - 1].

__Example:__

Example 1:

Input: n = 1
Output: []

Example 2:

Input: n = 12
Output: [[2,6],[3,4],[2,2,3]]

Example 3:

Input: n = 37
Output: []

__Constraints:__

1 <= n <= 10^7

__题目描述：__

整数可以被看作是其因子的乘积。

例如：

8 = 2 x 2 x 2;
  = 2 x 4.
请实现一个函数，该函数接收一个整数 n 并返回该整数所有的因子组合。

注意：

你可以假定 n 为永远为正数。
因子必须大于 1 并且小于 n。

__示例：__

示例 1：

输入: 1
输出: []

示例 2：

输入: 37
输出: []

示例 3：

输入: 12
输出:
[
  [2, 6],
  [2, 2, 3],
  [3, 4]
]

示例 4:

输入: 32
输出:
[
  [2, 16],
  [2, 2, 8],
  [2, 2, 2, 4],
  [2, 2, 2, 2, 2],
  [2, 4, 4],
  [4, 8]
]

__思路：__

回溯
i 从 2 开始遍历直到 i \* i > n
当 n 能整除 i 时, 以 { i, n / i } 为起始数组进行回溯
回溯时每次从倒数第二的数开始, 这个数也是当前数组第二大的数直到 i \* i > 最后一个数
每次遍历到最后一个数能整除 i, 去掉最后一个数并加入 i 和 最后一个数除 i 的商
时间复杂度为 O(nlgn), 空间复杂度为 O(lgn)

__代码__:

__C++__:

```C++
class Solution 
{
    vector<vector<int>> result;
    void dfs(vector<int>&& start)
    {
        result.emplace_back(start);
        for (int i = *++rbegin(start); i * i <= start.back(); i++)
        {
            if (!(start.back() % i))
            {
                vector<int> cur(start.begin(), start.end() - 1);
                cur.emplace_back(i);
                cur.emplace_back(start.back() / i);
                dfs(move(cur));
            }
        }
    }
public:
    vector<vector<int>> getFactors(int n) 
    {
        for (int i = 2; i * i <= n; i++) if (!(n % i)) dfs({i, n / i});
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private List<List<Integer>> result = new ArrayList<>();
    
    public List<List<Integer>> getFactors(int n) {
        dfs(n, new ArrayList<>());
        return result;
    }
    
    private void dfs(int n, List<Integer> cur) {
        if (n == 1) if (cur.size() > 0) result.add(new ArrayList<>(cur));
        if (n == 1) return;
        for (int i = cur.isEmpty() ? 2 : cur.get(cur.size() - 1); i * i <= n; i++) {
            if (n % i == 0) {
                cur.add(i);
                cur.add(n / i);
                result.add(new ArrayList<Integer>(cur));
                cur.remove(cur.size() - 1);
                dfs(n / i, cur);
                cur.remove(cur.size() - 1);
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def getFactors(self, n: int) -> List[List[int]]:
        def dfs(n: int, r: int) -> List[List[int]]:
            result = []
            for i in range(r, int(sqrt(n)) + 1):
                if not n % i:
                    result.append([i, n // i])
                    for sub in dfs(n // i, i):
                        result.append(sub + [i])
            return result
        return dfs(n, 2)
```
