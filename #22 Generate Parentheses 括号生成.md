__Description__:
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

__Example:__
For example, given n = 3, a solution set is:

[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]

__题目描述__:
给出 n 代表生成括号的对数，请你写出一个函数，使其能够生成所有可能的并且有效的括号组合。

__示例 :__
例如，给出 n = 3，生成结果为：

[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]

__思路__:
回溯法, 左边的括号先判断, 右边的括号要小于等于左边的括号
时间复杂度O(2 ^ n / (n ^ 1 / 2)), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<string> generateParenthesis(int n) 
    {
        vector<string> result;
        helper(result, "", 0, 0, n);
        return result;
    }
private:
    void helper(vector<string> &result, string temp, int l, int r, int n) 
    {
        if (temp.size() == (n << 1)) 
        {
            result.push_back(temp);
            return;
        }
        if (l < n) helper(result, temp + "(", l + 1, r, n);
        if (r < l) helper(result, temp + ")", l, r + 1, n);
    }
};
```

__Java__:
```Java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<>();
        helper(result, "", 0, 0, n);
        return result;
    }
    
    private void helper(List<String> result, String temp, int l, int r, int n) {
        if (temp.length() == (n << 1)) {
            result.add(temp);
            return;
        }
        if (l < n) helper(result, temp + "(", l + 1, r, n);
        if (r < l) helper(result, temp + ")", l, r + 1, n);
    }
}
```

__Python__:
```Python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        def dfs(result: List[str], temp: str, l: int, r: int, n: int) -> None:
            if len(temp) == (n << 1):
                result.append(temp)
                return
            if l < n:
                dfs(result, temp + '(', l + 1, r, n)
            if r < l:
                dfs(result, temp + ')', l, r + 1, n)
        result = []
        dfs(result, '', 0, 0, n)
        return result
```