# 1096 Brace Expansion II 花括号展开 II

__Description__:
Under the grammar given below, strings can represent a set of lowercase words. Let R(expr) denote the set of words the expression represents.

The grammar can best be understood through simple examples:

Single letters represent a singleton set containing that word.
R("a") = {"a"}
R("w") = {"w"}
When we take a comma-delimited list of two or more expressions, we take the union of possibilities.
R("{a,b,c}") = {"a","b","c"}
R("{{a,b},{b,c}}") = {"a","b","c"} (notice the final set only contains each word at most once)
When we concatenate two expressions, we take the set of possible concatenations between two words where the first word comes from the first expression and the second word comes from the second expression.
R("{a,b}{c,d}") = {"ac","ad","bc","bd"}
R("a{b,c}{d,e}f{g,h}") = {"abdfg", "abdfh", "abefg", "abefh", "acdfg", "acdfh", "acefg", "acefh"}
Formally, the three rules for our grammar:

For every lowercase letter x, we have R(x) = {x}.
For expressions e1, e2, ... , ek with k >= 2, we have R({e1, e2, ...}) = R(e1) ∪ R(e2) ∪ ...
For expressions e1 and e2, we have R(e1 + e2) = {a + b for (a, b) in R(e1) × R(e2)}, where + denotes concatenation, and × denotes the cartesian product.
Given an expression representing a set of words under the given grammar, return the sorted list of words that the expression represents.

__Example:__

Example 1:

Input: expression = "{a,b}{c,{d,e}}"
Output: ["ac","ad","ae","bc","bd","be"]

Example 2:

Input: expression = "{{a,z},a{b,c},{ab,z}}"
Output: ["a","ab","ac","z"]
Explanation: Each distinct word is written only once in the final answer.

__Constraints:__

1 <= expression.length <= 60
expression[i] consists of '{', '}', ','or lowercase English letters.
The given expression represents a set of words based on the grammar given in the description.

__题目描述__:
如果你熟悉 Shell 编程，那么一定了解过花括号展开，它可以用来生成任意字符串。

花括号展开的表达式可以看作一个由 花括号、逗号 和 小写英文字母 组成的字符串，定义下面几条语法规则：

如果只给出单一的元素 x，那么表达式表示的字符串就只有 "x"。R(x) = {x}
例如，表达式 "a" 表示字符串 "a"。
而表达式 "w" 就表示字符串 "w"。
当两个或多个表达式并列，以逗号分隔，我们取这些表达式中元素的并集。R({e_1,e_2,...}) = R(e_1) ∪ R(e_2) ∪ ...
例如，表达式 "{a,b,c}" 表示字符串 "a","b","c"。
而表达式 "{{a,b},{b,c}}" 也可以表示字符串 "a","b","c"。
要是两个或多个表达式相接，中间没有隔开时，我们从这些表达式中各取一个元素依次连接形成字符串。R(e_1 + e_2) = {a + b for (a, b) in R(e_1) × R(e_2)}
例如，表达式 "{a,b}{c,d}" 表示字符串 "ac","ad","bc","bd"。
表达式之间允许嵌套，单一元素与表达式的连接也是允许的。
例如，表达式 "a{b,c,d}" 表示字符串 "ab","ac","ad"​​​​​​。
例如，表达式 "a{b,c}{d,e}f{g,h}" 可以表示字符串 "abdfg", "abdfh", "abefg", "abefh", "acdfg", "acdfh", "acefg", "acefh"。
给出表示基于给定语法规则的表达式 expression，返回它所表示的所有字符串组成的有序列表。

假如你希望以「集合」的概念了解此题，也可以通过点击 “显示英文描述” 获取详情。

__示例 :__

示例 1：

输入：expression = "{a,b}{c,{d,e}}"
输出：["ac","ad","ae","bc","bd","be"]

示例 2：

输入：expression = "{{a,z},a{b,c},{ab,z}}"
输出：["a","ab","ac","z"]
解释：输出中 不应 出现重复的组合结果。

__提示:__

1 <= expression.length <= 60
expression[i] 由 '{'，'}'，',' 或小写英文字母组成
给出的表达式 expression 用以表示一组基于题目描述中语法构造的字符串

__思路__:

1. BFS
结果中保留没有花括号的字符串, 并用集合去重
找到需要处理的字符串中的第一对花括号
将第一对花括号中左花括号以前的元素记为 before, 右花括号以后的元素记为 after, 这两个元素都不包括花括号本身, 并将花括号中的元素按照逗号分割
将 before ➕ 分割的每个元素 ➕ after 记录于队列中继续搜索
最后将集合转换为列表并排序
2. 递归
遇到括号, 进入递归处理子字符串求取并集
遇到字符, 进入连接将结果加上字符
使用集合去重, 并将最后的结果排序
时间复杂度O(2 ^ n), 空间复杂度O(2 ^ n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<string> braceExpansionII(string expression) 
    {
        int i = 0;
        return parse(i, expression);
    }
private:
    vector<string>parse(int& i, string& expression) 
    {
        vector<string> result{""};
        for (;expression[i] == '{' or isalpha(expression[i]); i++)
        {
            if (expression[i] == '{')
            {
                vector<string> cur;
                do
                {
                    for (auto& s : parse(++i, expression)) cur.emplace_back(s);
                } while (expression[i] == ',');
                unordered_set<string> union_set;
                for (const auto& a : result) for (const auto& b : cur) union_set.insert(a + b);
                result = vector<string>(union_set.begin(), union_set.end());
            }
            else for (auto& s : result) s += expression[i];
        }
        sort(result.begin(), result.end());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> braceExpansionII(String expression) {
        Queue<String> queue = new LinkedList<>();
        queue.add(expression);
        Set<String> set = new HashSet<>();
        StringBuilder sb = new StringBuilder();
        while (!queue.isEmpty()) {
            String cur = queue.poll();
            if (cur.indexOf("{") == -1) {
                set.add(cur);
                continue;
            }
            int i = 0, left = 0, right = 0;
            while (cur.charAt(i) != '}') {
                if (cur.charAt(i) == '{') left = i;
                i++;
            }
            right = i;
            String before = cur.substring(0, left), after = cur.substring(right + 1), strs[] = cur.substring(left + 1, right).split(",");
            for (String str : strs) {
                sb.setLength(0);
                queue.add(sb.append(before).append(str).append(after).toString());
            }
        }
        List<String> result = new ArrayList<>(set);
        Collections.sort(result);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def braceExpansionII(self, expression: str) -> List[str]:
        class S(set):
            __add__, __mul__ = lambda s, o: S(s | o), lambda s, o: S(i + j for i, j in itertools.product(s, o))
        return sorted(S(eval(re.sub('(\w+)', r'S(["\1"])', expression).replace(',', '+').replace('{', '(').replace('}', ')').replace(')(',')*(').replace(')S',')*S'))))
```
