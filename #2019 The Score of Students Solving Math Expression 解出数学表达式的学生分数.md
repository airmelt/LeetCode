# 2019 The Score of Students Solving Math Expression 解出数学表达式的学生分数

__Description:__

You are given a string `s` that contains digits `0-9`, addition symbols `'+'`, and multiplication symbols `'*'` __only__, representing a __valid__ math expression of __single digit numbers__ (e.g., `3+5*2`). This expression was given to `n` elementary school students. The students were instructed to get the answer of the expression by following this __order of operations__:

You are given an integer array `answers` of length `n`, which are the submitted answers of the students in no particular order. You are asked to grade the `answers`, by following these __rules__:

- If an answer __equals__ the correct answer of the expression, this student will be rewarded `5` points;
- Otherwise, if the answer __could be interpreted__ as if the student applied the operators __in the wrong order__ but had __correct arithmetic__, this student will be rewarded `2` points;
- Otherwise, this student will be rewarded `0` points.

Return the sum of the points of the students.

__Example:__

Example 1:

![2019-1](https://assets.leetcode.com/uploads/2021/09/17/student_solving_math.png)

```text
Input: s = "7+3*1*2", answers = [20,13,42]
Output: 7
Explanation: As illustrated above, the correct answer of the expression is 13, therefore one student is rewarded 5 points: [20,13,42]
A student might have applied the operators in this wrong order: ((7+3)*1)*2 = 20. Therefore one student is rewarded 2 points: [20,13,42]
The points for the students are: [2,5,0]. The sum of the points is 2+5+0=7.
```

Example 2:

```text
Input: s = "3+5*2", answers = [13,0,10,13,13,16,16]
Output: 19
Explanation: The correct answer of the expression is 13, therefore three students are rewarded 5 points each: [13,0,10,13,13,16,16]
A student might have applied the operators in this wrong order: ((3+5)*2 = 16. Therefore two students are rewarded 2 points: [13,0,10,13,13,16,16]
The points for the students are: [5,0,0,5,5,2,2]. The sum of the points is 5+0+0+5+5+2+2=19.
```

Example 3:

```text
Input: s = "6+0*1", answers = [12,9,6,4,8,6]
Output: 10
Explanation: The correct answer of the expression is 6.
If a student had incorrectly done (6+0)*1, the answer would also be 6.
By the rules of grading, the students will still be rewarded 5 points (as they got the correct answer), not 2 points.
The points for the students are: [0,0,5,0,0,5]. The sum of the points is 10.
```

__Constraints:__

- `3 <= s.length <= 31`
- `s` represents a valid expression that contains only digits `0-9`, `'+'`, and `'*'` only.
- All the integer operands in the expression are in the __inclusive__ range `[0, 9]`.
- `1 <=` The count of all operators ( `'+'` and `'*'`) in the math expression `<= 15`
- Test data are generated such that the correct answer of the expression is in the range of `[0, 1000]`.
- `n == answers.length`
- `1 <= n <= 10 ^ 4`
- `0 <= answers[i] <= 1000`

__题目描述:__

给你一个字符串 `s` ，它 __只__ 包含数字 `0-9` ，加法运算符 `'+'` 和乘法运算符 `'*'` ，这个字符串表示一个 __合法__ 的只含有 __个位数__ __数字__ 的数学表达式（比方说 `3+5*2`）。有 `n` 位小学生将计算这个数学表达式，并遵循如下 __运算顺序__ :

给你一个长度为 `n` 的整数数组 `answers` ，表示每位学生提交的答案。你的任务是给 `answer` 数组按照如下 __规则__ 打分:

- 如果一位学生的答案 __等于__ 表达式的正确结果，这位学生将得到 `5` 分。
- 否则，如果答案由 __一处或多处错误的运算顺序__ 计算得到，那么这位学生能得到 `2` 分。
- 否则，这位学生将得到 `0` 分。

请你返回所有学生的分数和。

__示例:__

示例 1：

![2019-2](https://assets.leetcode.com/uploads/2021/09/17/student_solving_math.png)

```text
输入：s = "7+3*1*2", answers = [20,13,42]
输出：7
解释：如上图所示，正确答案为 13 ，因此有一位学生得分为 5 分：[20,13,42] 。
一位学生可能通过错误的运算顺序得到结果 20 ：7+3=10，10*1=10，10*2=20 。所以这位学生得分为 2 分：[20,13,42] 。
所有学生得分分别为：[2,5,0] 。所有得分之和为 2+5+0=7 。
```

示例 2：

```text
输入：s = "3+5*2", answers = [13,0,10,13,13,16,16]
输出：19
解释：表达式的正确结果为 13 ，所以有 3 位学生得到 5 分：[13,0,10,13,13,16,16] 。
学生可能通过错误的运算顺序得到结果 16 ：3+5=8，8*2=16 。所以两位学生得到 2 分：[13,0,10,13,13,16,16] 。
所有学生得分分别为：[5,0,0,5,5,2,2] 。所有得分之和为 5+0+0+5+5+2+2=19 。
```

示例 3：

```text
输入：s = "6+0*1", answers = [12,9,6,4,8,6]
输出：10
解释：表达式的正确结果为 6 。
如果一位学生通过错误的运算顺序计算该表达式，结果仍为 6 。
根据打分规则，运算顺序错误的学生也将得到 5 分（因为他们仍然得到了正确的结果），而不是 2 分。
所有学生得分分别为：[0,0,5,0,0,5] 。所有得分之和为 10 分。
```

__提示：__

- `3 <= s.length <= 31`
- `s` 表示一个只包含 `0-9` ， `'+'` 和 `'*'` 的合法表达式。
- 表达式中所有整数运算数字都在闭区间 `[0, 9]` 以内。
- `1 <=` 数学表达式中所有运算符数目（ `'+'` 和 `'*'`） `<= 15`
- 测试数据保证正确表达式结果在范围 `[0, 1000]` 以内。
- `n == answers.length`
- `1 <= n <= 10 ^ 4`
- `0 <= answers[i] <= 1000`

__思路:__

```text
区间 DP
统计答案对应的人数方便求和
计算正确答案
由于只有加法和乘法，用一个栈记录操作数如果遇到加法则直接入栈，如果遇到乘法则将栈顶元素出栈与当前元素相乘再入栈(也可以直接修改栈顶)
最后把栈中元素相加即为正确答案
用区间 DP 计算所有可能的结果
dp[i][j] 表示 s[i:j] 的所有可能结果
为了记录所有结果，用一个哈希表记录
dp[i][j] = dp[i][k] # dp[k + 2][j], i <= k <= j, # 为加法或者乘法
初始化 dp[i][i] = s[i] - '0'
对步长 step 从 2 到 n 进行遍历，对于每个步长，遍历所有可能的分割点，计算所有可能的结果, step = j - i
注意到结果不会超过 1000，所以可以将大于 1000 的结果直接剪枝
最后统计所有可能的结果 dp[0][n - 1]，如果结果等于正确答案则加 5 分，否则如果结果在哈希表中则加 2 分
时间复杂度为 O(N ^ 3 * C ^ 2), 空间复杂度为 O(N ^ 2 * C), 其中 N 为 s 的长度，C 为答案的范围, 这里 C = 1000
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int scoreOfStudents(string s, vector<int>& answers) 
    {
        int total = 1 << 10, n = s.size(), right = 0, zero = '0';
        vector<int> cnt(total);
        for (const auto& a : answers) ++cnt[a];
        stack<int> st;
        st.push(s.front() - zero);
        for (int i = 1; i < n; i += 2)
        {
            if (s[i] == '+') st.push(s[i + 1] - zero);
            else st.top() *= (s[i + 1] - zero);
        }
        while (!st.empty())
        {
            right += st.top();
            st.pop();
        }
        int result = 5 * cnt[right];
        vector<vector<unordered_set<int>>> dp(n + 2, vector<unordered_set<int>>(n + 2));
        for (int i = 0; i < n; i += 2) dp[i][i].insert(s[i] - '0');
        for (int step = 2; step < n; step += 2)
        {
            for (int i = 0; i + step < n; i += 2)
            {
                for (int j = 0; j < step; j += 2)
                {
                    for (const auto& x : dp[i][i + j])
                    {
                        for (const auto& y : dp[i + j + 2][i + step])
                        {
                            if (s[i + j + 1] == '+' and x + y <= 1000) dp[i][i + step].insert(x + y);
                            else if (s[i + j + 1] == '*' and x * y <= 1000) dp[i][i + step].insert(x * y);
                        }
                    }
                }
            }
        }
        for (const auto& a : dp.front()[n - 1]) if (a != right) result += cnt[a] << 1;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int scoreOfStudents(String s, int[] answers) {
        int n = s.length(), total = 1 << 10, count[] = new int[total], zero = '0', right = 0;
        for (int a : answers) ++count[a];
        Stack<Integer> stack = new Stack<>();
        stack.push(s.charAt(0) - zero);
        for (int i = 1; i < n; i += 2) {
            if (s.charAt(i) == '+') stack.push(s.charAt(i + 1) - zero);
            else stack.push(stack.pop() * (s.charAt(i + 1) - zero));
        }
        while (!stack.isEmpty()) right += stack.pop();
        int result = 5 * count[right];
        Set<Integer>[][] dp = new Set[n + 2][n + 2];
        for (int i = 0; i < n + 2; i++) for (int j = 0; j < n + 2; j++) dp[i][j] = new HashSet<>();
        for (int i = 0; i < n; i += 2) dp[i][i].add(s.charAt(i) - zero);
        for (int step = 2; step < n; step += 2) {
            for (int i = 0; i + step < n; i += 2) {
                for (int j = 0; j < step; j += 2) {
                    for (int x : dp[i][i + j]) {
                        for (int y : dp[i + j + 2][i + step]) {
                            if (s.charAt(i + j + 1) == '+' && x + y <= 1000) dp[i][i + step].add(x + y);
                            else if (s.charAt(i + j + 1) == '*' && x * y <= 1000) dp[i][i + step].add(x * y);
                        }
                    }
                }
            }
        }
        for (int a : dp[0][n - 1]) if (a != right) result += count[a] << 1;
        return result;           
    }
}
```

__Python__:

```Python
op = {'+': add, '*': mul}
@lru_cache(None)
def dfs(s):
    return {s[0]} if len(s) == 1 else set([op[s[i]](a, b) for i in range(1, len(s), 2) for a in dfs(s[:i]) for b in dfs(s[i + 1:]) if op[s[i]](a, b) <= 1000])
class Solution:
    def scoreOfStudents(self, s: str, answers: List[int]) -> int:
        return sum(5 if x == correct else 2 for x in answers if x in d) if (correct := eval(s)) is not None and (d := dfs(tuple(ch if ch in ('+', '*') else int(ch) for ch in s))) is not None else 0
```
