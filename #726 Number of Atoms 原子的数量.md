# 726 Number of Atoms 原子的数量

__Description__:
Given a chemical formula (given as a string), return the count of each atom.

The atomic element always starts with an uppercase character, then zero or more lowercase letters, representing the name.

One or more digits representing that element's count may follow if the count is greater than 1. If the count is 1, no digits will follow. For example, H2O and H2O2 are possible, but H1O2 is impossible.

Two formulas concatenated together to produce another formula. For example, H2O2He3Mg4 is also a formula.

A formula placed in parentheses, and a count (optionally added) is also a formula. For example, (H2O2) and (H2O2)3 are formulas.

Given a formula, return the count of all elements as a string in the following form: the first name (in sorted order), followed by its count (if that count is more than 1), followed by the second name (in sorted order), followed by its count (if that count is more than 1), and so on.

__Example:__

Example 1:

Input: formula = "H2O"
Output: "H2O"
Explanation: The count of elements are {'H': 2, 'O': 1}.

Example 2:

Input: formula = "Mg(OH)2"
Output: "H2MgO2"
Explanation: The count of elements are {'H': 2, 'Mg': 1, 'O': 2}.

Example 3:

Input: formula = "K4(ON(SO3)2)2"
Output: "K4N2O14S4"
Explanation: The count of elements are {'K': 4, 'N': 2, 'O': 14, 'S': 4}.

Example 4:

Input: formula = "Be32"
Output: "Be32"

__Constraints:__

1 <= formula.length <= 1000
formula consists of English letters, digits, '(', and ')'.
formula is always valid.

__题目描述__:
给定一个化学式formula（作为字符串），返回每种原子的数量。

原子总是以一个大写字母开始，接着跟随0个或任意个小写字母，表示原子的名字。

如果数量大于 1，原子后会跟着数字表示原子的数量。如果数量等于 1 则不会跟数字。例如，H2O 和 H2O2 是可行的，但 H1O2 这个表达是不可行的。

两个化学式连在一起是新的化学式。例如 H2O2He3Mg4 也是化学式。

一个括号中的化学式和数字（可选择性添加）也是化学式。例如 (H2O2) 和 (H2O2)3 是化学式。

给定一个化学式，输出所有原子的数量。格式为：第一个（按字典序）原子的名子，跟着它的数量（如果数量大于 1），然后是第二个原子的名字（按字典序），跟着它的数量（如果数量大于 1），以此类推。

__示例 :__

示例 1:

输入:
formula = "H2O"
输出: "H2O"
解释:
原子的数量是 {'H': 2, 'O': 1}。

示例 2:

输入:
formula = "Mg(OH)2"
输出: "H2MgO2"
解释:
原子的数量是 {'H': 2, 'Mg': 1, 'O': 2}。

示例 3:

输入:
formula = "K4(ON(SO3)2)2"
输出: "K4N2O14S4"
解释:
原子的数量是 {'K': 4, 'N': 2, 'O': 14, 'S': 4}。

__注意:__

所有原子的第一个字母为大写，剩余字母都是小写。
formula的长度在[1, 1000]之间。
formula只包含字母、数字和圆括号，并且题目中给定的是合法的化学式。

__思路__:

1. 栈
用栈记录出现过的原子和数量
栈中存放 TreeMap 保证原子有序
每次遇到 '(', 栈中存放一个新的 TreeMap 记录当前的原子数
每次遇到 ')', 弹出栈将原子数和括号外的数字组合相乘加入外层的 TreeMap
否则遇到的是普通原子及数量, 处理之后加入 TreeMap 即可
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n)
2. 递归
用一个递归函数返回当前遍历的下标
用 C++ 中的 map 保证原子有序
每次遇到 '(', 进入递归调用, 记录原子和数量
每次遇到 ')', 返回上一层递归
否则遇到的是普通原子及数量, 处理之后加入 map 即可
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string countOfAtoms(string formula) 
    {
        string result = "";
        map<string, int> m;
        int n = formula.size();
        parse(formula, m, 0, n);
        for (auto& [k, v] : m) result += k + (v > 1 ? to_string(v) : "");
        return result;
    }
private:
    int parse(string& formula, map<string, int>& m, int i, int n)
    {
        if (i > n - 1 or formula[i] == ')') return i + 1;
        while (i < n)
        {
            if (formula[i] == '(')
            {
                map<string, int> cur;
                i = parse(formula, cur, i + 1, n);
                int count = 0;
                while (i < n and formula[i] >= '0' and formula[i] <= '9') count = 10 * count + formula[i++] - '0';
                for (auto& [k, v] : cur) m[k] += v * max(count, 1);
            }
            else if (formula[i] == ')') return i + 1;
            else
            {
                string name = string(1, formula[i++]);
                while (i < n and formula[i] >= 'a' and formula[i] <= 'z') name += formula[i++];
                int count = 0;
                while (i < n and formula[i] >= '0' and formula[i] <= '9') count = 10 * count + formula[i++] - '0';
                m[name] += max(count, 1);
            }
        }
        return i;
    }
};
```

__Java__:

```Java
class Solution {
    public String countOfAtoms(String formula) {
        int n = formula.length(), i = 0;
        Stack<Map<String, Integer>> stack = new Stack<>();
        stack.push(new TreeMap<String, Integer>());
        while (i < n) {
            if (formula.charAt(i) == '(') {
                stack.push(new TreeMap<String, Integer>());
                ++i;
            } else if (formula.charAt(i) == ')') {
                Map<String, Integer> top = stack.pop();
                int start = ++i, count = 1;
                while (i < n && Character.isDigit(formula.charAt(i))) ++i;
                if (i > start) count = Integer.parseInt(formula.substring(start, i));
                for (Map.Entry<String, Integer> entry : top.entrySet()) stack.peek().put(entry.getKey(), entry.getValue() * count + stack.peek().getOrDefault(entry.getKey(), 0));
            } else {
                int start = i++, count = 1;
                while (i < n && Character.isLowerCase(formula.charAt(i))) ++i;
                String name = formula.substring(start, i);
                start = i;
                while (i < n && Character.isDigit(formula.charAt(i))) ++i;
                if (i > start) count = Integer.parseInt(formula.substring(start, i));
                stack.peek().put(name, count + stack.peek().getOrDefault(name, 0));
            }
        }
        StringBuilder result = new StringBuilder();
        for (Map.Entry<String, Integer> entry : stack.peek().entrySet()) result.append(entry.getKey()).append(entry.getValue() > 1 ? entry.getValue() : "");
        return result.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def countOfAtoms(self, formula: str) -> str:
        n, i, stack = len(formula), 0, [Counter()]
        while i < n:
            if formula[i] == '(':
                stack.append(Counter())
                i += 1
            elif formula[i] == ')':
                top = stack.pop()
                i += 1
                start = i
                while i < n and formula[i].isdigit(): 
                    i += 1
                count = int(formula[start:i] or 1)
                for k, v in top.items():
                    stack[-1][k] += v * count
            else:
                start = i
                i += 1
                while i < n and formula[i].islower(): 
                    i += 1
                name = formula[start: i]
                start = i
                while i < n and formula[i].isdigit(): 
                    i += 1
                count = int(formula[start:i] or 1)
                stack[-1][name] += count
        return "".join(name + (str(stack[-1][name]) if stack[-1][name] > 1 else '') for name in sorted(stack[-1]))
```
