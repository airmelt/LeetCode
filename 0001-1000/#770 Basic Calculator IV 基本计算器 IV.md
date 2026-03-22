# 770 Basic Calculator IV 基本计算器 IV

__Description__:
Given an expression such as expression = "e + 8 - a + 5" and an evaluation map such as {"e": 1} (given in terms of evalvars = ["e"] and evalints = [1]), return a list of tokens representing the simplified expression, such as ["-1*a","14"]

An expression alternates chunks and symbols, with a space separating each chunk and symbol.
A chunk is either an expression in parentheses, a variable, or a non-negative integer.
A variable is a string of lowercase letters (not including digits.) Note that variables can be multiple letters, and note that variables never have a leading coefficient or unary operator like "2x" or "-x".
Expressions are evaluated in the usual order: brackets first, then multiplication, then addition and subtraction.

For example, expression = "1 + 2 * 3" has an answer of ["7"].
The format of the output is as follows:

For each term of free variables with a non-zero coefficient, we write the free variables within a term in sorted order lexicographically.
For example, we would never write a term like "b*a*c", only "a*b*c".
Terms have degrees equal to the number of free variables being multiplied, counting multiplicity. We write the largest degree terms of our answer first, breaking ties by lexicographic order ignoring the leading coefficient of the term.
For example, "a*a*b*c" has degree 4.
The leading coefficient of the term is placed directly to the left with an asterisk separating it from the variables (if they exist.) A leading coefficient of 1 is still printed.
An example of a well-formatted answer is ["-2*a*a*a", "3*a*a*b", "3*b*b", "4*a", "5*c", "-6"].
Terms (including constant terms) with coefficient 0 are not included.
For example, an expression of "0" has an output of [].

__Example:__

Example 1:

Input: expression = "e + 8 - a + 5", evalvars = ["e"], evalints = [1]
Output: ["-1*a","14"]

Example 2:

Input: expression = "e - 8 + temperature - pressure", evalvars = ["e", "temperature"], evalints = [1, 12]
Output: ["-1*pressure","5"]

Example 3:

Input: expression = "(e + 8) \* (e - 8)", evalvars = [], evalints = []
Output: ["1*e*e","-64"]

Example 4:

Input: expression = "a \* b \* c + b \* a \* c \* 4", evalvars = [], evalints = []
Output: ["5*a*b*c"]

Example 5:

Input: expression = "((a - b) \* (b - c) + (c - a)) \* ((a - b) + (b - c) \* (c - a))", evalvars = [], evalints = []
Output: ["-1*a*a*b*b","2*a*a*b*c","-1*a*a*c*c","1*a*b*b*b","-1*a*b*b*c","-1*a*b*c*c","1*a*c*c*c","-1*b*b*b*c","2*b*b*c*c","-1*b*c*c*c","2*a*a*b","-2*a*a*c","-2*a*b*b","2*a*c*c","1*b*b*b","-1*b*b*c","1*b*c*c","-1*c*c*c","-1*a*a","1*a*b","1*a*c","-1*b*c"]

__Constraints:__

1 <= expression.length <= 250
expression consists of lowercase English letters, digits, '+', '-', '*', '(', ')', ' '.
expression does not contain any leading or trailing spaces.
All the tokens in expression are separated by a single space.
0 <= evalvars.length <= 100
1 <= evalvars[i].length <= 20
evalvars[i] consists of lowercase English letters.
evalints.length == evalvars.length
-100 <= evalints[i] <= 100

__题目描述__:
给定一个表达式 expression 如 expression = "e + 8 - a + 5" 和一个求值映射，如 {"e": 1}（给定的形式为 evalvars = ["e"] 和 evalints = [1]），返回表示简化表达式的标记列表，例如 ["-1*a","14"]

表达式交替使用块和符号，每个块和符号之间有一个空格。
块要么是括号中的表达式，要么是变量，要么是非负整数。
块是括号中的表达式，变量或非负整数。
变量是一个由小写字母组成的字符串（不包括数字）。请注意，变量可以是多个字母，并注意变量从不具有像 "2x" 或 "-x" 这样的前导系数或一元运算符 。
表达式按通常顺序进行求值：先是括号，然后求乘法，再计算加法和减法。例如，expression = "1 + 2 * 3" 的答案是 ["7"]。

输出格式如下：

对于系数非零的每个自变量项，我们按字典排序的顺序将自变量写在一个项中。例如，我们永远不会写像 “b*a*c” 这样的项，只写 “a*b*c”。
项的次数等于被乘的自变量的数目，并计算重复项。(例如，"a*a*b*c" 的次数为 4。)。我们先写出答案的最大次数项，用字典顺序打破关系，此时忽略词的前导系数。
项的前导系数直接放在左边，用星号将它与变量分隔开(如果存在的话)。前导系数 1 仍然要打印出来。
格式良好的一个示例答案是 ["-2*a*a*a", "3*a*a*b", "3*b*b", "4*a", "5*c", "-6"] 。
系数为 0 的项（包括常数项）不包括在内。例如，“0” 的表达式输出为 []。

__示例 :__

示例 1:

输入：expression = "e + 8 - a + 5", evalvars = ["e"], evalints = [1]
输出：["-1*a","14"]

示例 2:

输入：expression = "e - 8 + temperature - pressure",
evalvars = ["e", "temperature"], evalints = [1, 12]
输出：["-1*pressure","5"]

示例 3:

输入：expression = "(e + 8) \* (e - 8)", evalvars = [], evalints = []
输出：["1*e*e","-64"]

示例 4:

输入：expression = "7 - 7", evalvars = [], evalints = []
输出：[]

示例 5:

输入：expression = "a \* b \* c + b \* a \* c \* 4", evalvars = [], evalints = []
输出：["5*a*b*c"]

示例 6:

输入：expression = "((a - b) \* (b - c) + (c - a)) \* ((a - b) + (b - c) \* (c - a))",
evalvars = [], evalints = []
输出：["-1*a*a*b*b","2*a*a*b*c","-1*a*a*c*c","1*a*b*b*b","-1*a*b*b*c","-1*a*b*c*c","1*a*c*c*c","-1*b*b*b*c","2*b*b*c*c","-1*b*c*c*c","2*a*a*b","-2*a*a*c","-2*a*b*b","2*a*c*c","1*b*b*b","-1*b*b*c","1*b*c*c","-1*c*c*c","-1*a*a","1*a*b","1*a*c","-1*b*c"]

__提示:__

expression 的长度在 [1, 250] 范围内。
evalvars, evalints 在范围 [0, 100] 内，且长度相同。

__思路__:

Counter 类
如果对 Python 中的 Counter 类比较熟悉其实可以看出来
加减法与 Counter 的操作相同, 乘法则是使用的笛卡尔积
可以使用一个哈希映射来记录变量及对应的数值 map<string, int>, 其中常数可以用 '#' 或者其他非小写字母表示
输出的结果注意把 0 去掉和排序
时间复杂度为 O(2 ^ n + m), 空间复杂度为 O(n + m), 其中 n 为 expression 的长度， m 为 evalvars 和 evalints 的长度。

__代码__:
__C++__:

```C++
class Solution 
{
private:
    unordered_map<string, int> record;
    
    string combine_val(string s1, string s2) 
    {
        vector<string> a;
        s1 += '*';
        int pos = s1.find("*", 0);
        while (pos != s1.npos) 
        {
            string cur = s1.substr(0, pos);
            a.push_back(cur);
            s1 = s1.substr(pos + 1, s1.size());
            pos = s1.find("*", 0);
        }
        s2 += '*';
        pos = s2.find("*", 0);
        while (pos != s2.npos) 
        {
            string cur = s2.substr(0, pos);
            a.push_back(cur);
            s2 = s2.substr(pos + 1, s2.size());
            pos = s2.find("*", 0);
        }
        sort(a.begin(), a.end());
        string new_val = a[0];
        for (int i = 1; i < a.size(); i++) new_val += "*" + a[i];
        return new_val;
    }

    map<string, int> get_number(string &name, int& i) 
    {
        map<string, int> result;
        int n = name.size(), start = i;
        if (i >= n) return result;
        if (name[i] == '(') 
        {
            stack<int> s;
            s.push(i++);
            while (i < n and !s.empty()) 
            {
                if (name[i] == '(') s.push(i);
                if (name[i] == ')') s.pop();
                ++i;
            }
            string sub = name.substr(start + 1, i - start - 2);
            result = calculator(sub);
        } 
        else if (isdigit(name[i])) 
        {
            while (i < n and isdigit(name[i])) ++i;
            string val = name.substr(start, i - start);
            result["#"] = stoi(val);
        } 
        else 
        {
            while (i < n and name[i] != ' ') ++i;
            string val = name.substr(start, i - start);
            if (record.count(val)) result["#"] = record[val];
            else result[val] = 1;
        }
        ++i;
        return result;
    }

    map<string, int> add(map<string, int>& vals_a, map<string, int>& vals_b)
    {
        for (const auto& b : vals_b) vals_a[b.first] += b.second;
        return vals_a;
    }


    map<string, int> sub(map<string, int>& vals_a, map<string, int>& vals_b)
    {
        for (const auto& b : vals_b) vals_a[b.first] -= b.second;
        return vals_a;
    }

    map<string, int> mul(map<string, int>& vals_a, map<string, int>& vals_b) 
    {
        map<string, int> result;
        for (const auto& a : vals_a) 
        {
            for (const auto& b : vals_b) 
            { 
                if (a.first == "#") result[b.first] += a.second * b.second;
                else if (b.first == "#") result[a.first] += a.second * b.second;
                else result[combine_val(a.first, b.first)] += a.second * b.second;
            }
        }
        return result;
    }

    map<string,int> calculator(string expression) 
    {
        if (expression.empty()) return {};
        map<string, int> result, cur;
        expression += " +";
        int n = expression.size();
        char op = '+';
        for (int i = 0; i < n; i += 2) 
        {
            map<string, int> x = get_number(expression, i);
            if (expression[i] == '+' or expression[i] == '-' or expression[i] == '*' or i == n - 1) 
            {
                switch (op) 
                {
                    case '+':
                        cur = add(cur, x);
                        break;
                    case '-':
                        cur = sub(cur, x);
                        break;
                    case '*':
                        cur = mul(cur, x);
                        break;
                }
                if (expression[i] == '+' or expression[i] == '-' or i == n - 1) 
                {
                    result = add(result, cur);
                    cur.clear();
                }
                op = expression[i];
            }
        }
        return result;
    }

public:
    vector<string> basicCalculatorIV(string expression, vector<string>& evalvars, vector<int>& evalints) 
    {
        for (int i = 0; i < evalvars.size(); i++) record[evalvars[i]] = evalints[i];
        map<string, int> tree = calculator(expression);
        vector<pair<int,string>> vals;
        for (auto &x : tree)
        {
            if (x.second and x.first != "#") 
            {
                string name = x.first;
                name += '*';
                int count = 0, pos = name.find("*", 0);
                while (pos != name.npos) 
                {
                    ++count;
                    name = name.substr(pos + 1, name.size());
                    pos = name.find("*", 0);
                }
                vals.push_back({count, x.first});
            }
        }
        sort(vals.begin(), vals.end(), [](const auto& a, const auto& b){ return a.first != b.first ? a.first > b.first : a.second < b.second; });
        int constant = tree["#"], n = vals.size();
        vector<string> result(n);
        for (int i = 0; i < n; i++) result[i] = to_string(tree[vals[i].second]) + "*" + vals[i].second;
        if (constant) result.push_back(to_string(constant));
        return result;
    }
};
```

__Java__:

```Java
public class Solution {

    public List<String> basicCalculatorIV(String expression, String[] evalvars, int[] evalints) {
        class Item implements Comparable<Item> {
            int coeff;
            private ArrayList<String> factors;

            private Item(String factor, int coeff) {
                this.factors = new ArrayList<>();
                this.factors.add(factor);
                this.coeff = coeff;
            }

            private Item(int coeff) {
                this.factors = new ArrayList<>();
                this.coeff = coeff;
            }

            private Item() {
                this.factors = new ArrayList<>();
                this.coeff = 0;
            }

            @Override
            public int compareTo(Item item) {
                if (this.factors.size() == item.factors.size()) {
                    int index = 0;
                    while (index < factors.size() && this.factors.get(index).equals(item.factors.get(index))) {
                        index += 1;
                    }
                    return (index == factors.size()) ? 0 : this.factors.get(index).compareTo(item.factors.get(index));
                } else {
                    return item.factors.size() - this.factors.size();
                }
            }

            @Override
            public String toString() {
                StringBuilder stringBuilder = new StringBuilder();
                stringBuilder.append(coeff);
                for (String factor : factors) {
                    stringBuilder.append("*").append(factor);
                }
                return stringBuilder.toString();
            }

            Item mul(Item item) {
                Item result = new Item();
                result.coeff = this.coeff * item.coeff;
                result.factors.addAll(this.factors);
                result.factors.addAll(item.factors);
                result.factors.sort(String::compareTo);
                return result;
            }
        }

        class Expr {
            private ArrayList<Item> items;

            private Expr(Item item) {
                this.items = new ArrayList<>();
                this.items.add(item);
            }

            private void add(Expr expr) {
                items.addAll(expr.items);
                items.sort(Item::compareTo);
                clean();
            }

            private void mul(Expr expr) {
                ArrayList<Item> result = new ArrayList<>();
                for (Item item1 : items) {
                    for (Item item2 : expr.items) {
                        result.add(item1.mul(item2));
                    }
                }
                items = result;
                items.sort(Item::compareTo);
                clean();
            }

            private void clean() {
                for (int i = 0; i < items.size(); i++) {
                    while (i + 1 < items.size() && items.get(i).compareTo(items.get(i + 1)) == 0) {
                        items.get(i).coeff += items.get(i + 1).coeff;
                        items.remove(i + 1);
                    }
                    if (i < items.size() && items.get(i).coeff == 0) {
                        items.remove(i--);
                    }
                }
            }

            private Expr operate(Expr expr, String op) {
                switch (op) {
                    case "+":
                        add(expr);
                        break;
                    case "-":
                        for (Item item : expr.items) {
                            item.coeff *= -1;
                        }
                        add(expr);
                        break;
                    case "*":
                        mul(expr);
                        break;
                }
                return this;
            }
        }

        HashMap<String, Integer> map = new HashMap<>();
        for (int i = 0; i < evalvars.length; i++) {
            map.put(evalvars[i], evalints[i]);
        }

        LinkedList<Expr> mainStack = new LinkedList<>();
        LinkedList<String> symStack = new LinkedList<>();
        int index = 0;
        while (index < expression.length()) {
            if (expression.charAt(index) == ' ') {
                index += 1;
            } else if (expression.charAt(index) >= '0' && expression.charAt(index) <= '9') {
                int x = 0;
                while (index < expression.length() && expression.charAt(index) >= '0' && expression.charAt(index) <= '9') {
                    x = x * 10 + expression.charAt(index++) - '0';
                }
                mainStack.push(new Expr(new Item(x)));
            } else if (expression.charAt(index) >= 'a' && expression.charAt(index) <= 'z') {
                StringBuilder stringBuilder = new StringBuilder();
                while (index < expression.length() && expression.charAt(index) >= 'a' && expression.charAt(index) <= 'z') {
                    stringBuilder.append(expression.charAt(index++));
                }
                String factor = stringBuilder.toString();
                if (map.containsKey(factor)) {
                    mainStack.push(new Expr(new Item(map.get(factor))));
                } else {
                    mainStack.push(new Expr(new Item(stringBuilder.toString(), 1)));
                }
            } else if (expression.charAt(index) == '(') {
                symStack.push("(");
                index += 1;
            } else if (expression.charAt(index) == ')') {
                while (!symStack.isEmpty() && !symStack.peek().equals("(")) {
                    Expr expr2 = mainStack.pop();
                    Expr expr1 = mainStack.pop();
                    mainStack.push(expr1.operate(expr2, symStack.pop()));

                }
                symStack.pop();
                index += 1;
            } else if (expression.charAt(index) == '*') {
                while (!symStack.isEmpty() && symStack.peek().equals("*")) {
                    Expr expr2 = mainStack.pop();
                    Expr expr1 = mainStack.pop();
                    mainStack.push(expr1.operate(expr2, symStack.pop()));
                }
                symStack.push("*");
                index += 1;
            } else {
                while (!symStack.isEmpty() && (symStack.peek().equals("+") || symStack.peek().equals("-") || symStack.peek().equals("*"))) {
                    Expr expr2 = mainStack.pop();
                    Expr expr1 = mainStack.pop();
                    mainStack.push(expr1.operate(expr2, symStack.pop()));
                }
                symStack.push((expression.charAt(index) == '+') ? "+" : "-");
                index += 1;
            }
        }
        while (!symStack.isEmpty()) {
            Expr expr2 = mainStack.pop();
            Expr expr1 = mainStack.pop();
            mainStack.push(expr1.operate(expr2, symStack.pop()));
        }

        ArrayList<String> result = new ArrayList<>();
        Expr expr = mainStack.pop();
        expr.clean();
        for (Item item : expr.items) {
            result.add(item.toString());
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def basicCalculatorIV(self, expression: str, evalvars: List[str], evalints: List[int]) -> List[str]:
        
        class C(Counter):
            def __add__(self, other):
                self.update(other)
                return self
            
            def __sub__(self, other):
                self.subtract(other)
                return self
            
            def __mul__(self, other):
                product = C()
                for x in self:
                    for y in other:
                        xy = tuple(sorted(x + y))
                        product[xy] += self[x] * other[y]
                return product
            
        vals = dict(zip(evalvars, evalints))
        
        def f(s):
            s = str(vals.get(s, s))
            return C({(s,): 1}) if s.isalpha() else C({(): int(s)})
        
        c = eval(re.sub('(\w+)', r'f("\1")', expression))
        return ['*'.join((str(c[x]),) + x) for x in sorted(c, key=lambda x: (-len(x), x)) if c[x]]
```
