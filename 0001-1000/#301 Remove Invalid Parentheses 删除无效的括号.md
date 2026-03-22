# 301 Remove Invalid Parentheses 删除无效的括号

__Description__:
Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible results.

__Note:__
The input string may contain letters other than the parentheses ( and ).

__Example:__

Example 1:

Input: "()())()"
Output: ["()()()", "(())()"]

Example 2:

Input: "(a)())()"
Output: ["(a)()()", "(a())()"]

Example 3:

Input: ")("
Output: [""]

__题目描述__:
删除最小数量的无效括号，使得输入的字符串有效，返回所有可能的结果。

__说明:__
输入可能包含了除 ( 和 ) 以外的字符。

__示例 :__

示例 1:

输入: "()())()"
输出: ["()()()", "(())()"]

示例 2:

输入: "(a)())()"
输出: ["(a)()()", "(a())()"]

示例 3:

输入: ")("
输出: [""]

__思路__:

BFS
对每一个 s的字符串进行删除一个多余字符操作
可以保证最少删除字符
去重可以考虑使用 hash表或者约定删除方式
删除时, 可以考虑从左到右删除第一个不是合法的字符
比如 (()(()可以通过删除第 0个和第 3个字符得到 ()()
时间复杂度O(2 ^ n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<string> removeInvalidParentheses(string s) 
    {
        queue<pair<string, int>> q;
        q.push(make_pair(s, 0));
        vector<string> result;
        while (!q.empty()) 
        {
            auto p = q.front();
            q.pop();
            string ss = p.first;
            if (is_valid(ss)) result.push_back(ss);
            else if (result.empty()) for (int i = p.second; i < ss.size(); i++) if ((ss[i] == ')' or ss[i] == '(') and (i == p.second or ss[i] != ss[i - 1])) q.push(make_pair(ss.substr(0, i) + ss.substr(i + 1), i));
        }
        return result;
    }
private:
    bool is_valid(string &s)
    {
        int count = 0;
        for (auto c : s)
        {
            if (c == '(') ++count;
            else if (c == ')')
            {
                if (count > 0) --count;
                else return false;
            }
        }
        return !count;
    }
};
```

__Java__:

```Java
class Solution {
    class Pair {
        private String key;
        private Integer value;
        
        public Pair(String key, Integer value) {
            this.key = key;
            this.value = value;
        }
        
        public String getKey() {
            return this.key;
        }
        
        public Integer getValue() {
            return this.value;
        }
    }
    public List<String> removeInvalidParentheses(String s) {
        Queue<Pair> q = new LinkedList<>();
        q.offer(new Pair(s, 0));
        List<String> result = new ArrayList<>();
        while (!q.isEmpty()) {
            Pair p = q.poll();
            String ss = p.getKey();
            if (isValid(ss)) result.add(ss);
            else if (result.isEmpty()) for (int i = p.getValue(); i < ss.length(); i++) if ((ss.charAt(i) == ')' || ss.charAt(i) == '(') && (i == p.getValue() || ss.charAt(i) != ss.charAt(i - 1))) q.offer(new Pair(ss.substring(0, i) + ss.substring(i + 1), i));
        }
        return result;
    }
    
    private boolean isValid(String s) {
        int count = 0;
        for (char c : s.toCharArray())
        {
            if (c == '(') ++count;
            else if (c == ')')
            {
                if (count > 0) --count;
                else return false;
            }
        }
        return count == 0;
    }
}
```

__Python__:

```Python
class Solution:
    def removeInvalidParentheses(self, s: str) -> List[str]:
        q, result = [(s, 0)], []
        def is_valid(s: str) -> bool:
            count = 0
            for c in s:
                if c == '(':
                    count += 1
                elif c == ')':
                    if count > 0:
                        count -= 1
                    else:
                        return False
            return not count
        while q:
            p = q.pop(0)
            if is_valid(p[0]):
                result.append(p[0][:])
            elif not result:
                for i in range(p[1], len(p[0])):
                    if (p[0][i] == ')' or p[0][i] == '(') and (i == p[1] or p[0][i] != p[0][i - 1]):
                        q.append((p[0][0:i] + p[0][i + 1:], i))
        return result
```
