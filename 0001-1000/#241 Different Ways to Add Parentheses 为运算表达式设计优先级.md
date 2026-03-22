# 241 Different Ways to Add Parentheses 为运算表达式设计优先级

__Description__:
Given a string of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. The valid operators are +, - and \*.

__Example:__

Example 1:

Input: "2-1-1"
Output: [0, 2]
Explanation:
((2-1)-1) = 0
(2-(1-1)) = 2

Example 2:

Input: "2\*3-4\*5"
Output: [-34, -14, -10, -10, 10]
Explanation:
(2\*(3-(4\*5))) = -34
((2\*3)-(4\*5)) = -14
((2\*(3-4))\*5) = -10
(2\*((3-4)\*5)) = -10
(((2\*3)-4)\*5) = 10

__题目描述__:
给定一个含有数字和运算符的字符串，为表达式添加括号，改变其运算优先级以求出不同的结果。你需要给出所有可能的组合的结果。有效的运算符号包含 +, - 以及 \* 。

__示例 :__

示例 1:

输入: "2-1-1"
输出: [0, 2]
解释:
((2-1)-1) = 0
(2-(1-1)) = 2

示例 2:

输入: "2\*3-4\*5"
输出: [-34, -14, -10, -10, 10]
解释:
(2\*(3-(4\*5))) = -34
((2\*3)-(4\*5)) = -14
((2\*(3-4))\*5) = -10
(2\*((3-4)\*5)) = -10
(((2\*3)-4)\*5) = 10

__思路__:

1. 分而治之
递归➕回溯
比如 "2\*3-4\*5"可以划分成 a: "2", "3-4\*5"; b: "2\*3", "4\*5", c: "2\*3-4", "5"
递归的终点是纯数字, 要么是运算结果, 要么是一个单个的数字
回溯是将分开的两个数字再得到运算结果, 比如 b: "6" - "20" -> -14
时间复杂度O(Cn), 空间复杂度O(Cn), 其中 Cn为卡塔兰数
2. 动态规划
dp[i][j]表示从第 i个数字到第 j个数字的所有情况
dp[i][i]表示一个数字, 就是 input中的数字
时间复杂度O(n ^ 3), 空间复杂度O(n ^ 3)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> diffWaysToCompute(string input) 
    {
        vector<int> nums, ops;
        int num = 0;
        for (int i = 0; i < input.size(); i++)
        {
            if (is_operator(input[i]))
            {
                nums.push_back(num);
                ops.push_back(input[i]);
                num = 0;
            }
            else num = num * 10 + input[i] - '0';
        }
        nums.push_back(num);
        int n = nums.size();
        vector<vector<vector<int>>> dp(n,vector<vector<int>>(n));
        for (int i = 0; i < n; i++) dp[i][i].push_back(nums[i]);
        for (int j = 1; j < n; j++) for (int i = j - 1; i >= 0; i--) for(int k = i; k < j; k++) for (int r1 : dp[i][k]) for(int r2 : dp[k + 1][j]) dp[i][j].push_back(calculate(r1, ops[k], r2));
        return dp[0][n - 1];
    }
private:
    bool is_operator(const char& c)
    {
        return c == '+' or c == '-' or c == '*';
    }

    int calculate(const int& num1, const char& op, const int& num2)
    {
        return op == '+' ? num1 + num2 : (op == '-' ? num1 - num2 : num1 * num2);
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> diffWaysToCompute(String input) {
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < input.length(); i++) {
            char c = input.charAt(i);
            if (c == '+' || c == '-' || c == '*') {
                List<Integer> temp1 = diffWaysToCompute(input.substring(0, i)), temp2 = diffWaysToCompute(input.substring(i + 1));
                for (int num1 : temp1) for (int num2 : temp2) {
                    if (c == '+') result.add(num1 + num2);
                    else if (c == '-') result.add(num1 - num2);
                    else if (c == '*') result.add(num1 * num2);
                }
            }
        }
        return result.isEmpty() ? new ArrayList<Integer>(){{ add(Integer.valueOf(input)); }} : result;
    }
}
```

__Python__:

```Python
class Solution:
    def diffWaysToCompute(self, input: str) -> List[int]:
        return [int(input)] if input.isdigit() else [eval(f'{left}{c}{right}') for i, c in enumerate(input) for left in self.diffWaysToCompute(input[:i]) for right in self.diffWaysToCompute(input[i + 1:]) if c in '+-*']
```
