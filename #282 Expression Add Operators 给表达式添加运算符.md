__Description__:
Given a string that contains only digits 0-9 and a target value, return all possibilities to add binary operators (not unary) +, -, or * between the digits so they evaluate to the target value.

__Example:__
Example 1:

Input: num = "123", target = 6
Output: ["1+2+3", "1*2*3"] 

Example 2:

Input: num = "232", target = 8
Output: ["2*3+2", "2+3*2"]

Example 3:

Input: num = "105", target = 5
Output: ["1*0+5","10-5"]

Example 4:

Input: num = "00", target = 0
Output: ["0+0", "0-0", "0*0"]

Example 5:

Input: num = "3456237490", target = 9191
Output: []
 
__Constraints:__

0 <= num.length <= 10
num only contain digits.

__题目描述__:
给定一个仅包含数字 0-9 的字符串和一个目标值，在数字之间添加二元运算符（不是一元）+、- 或 * ，返回所有能够得到目标值的表达式。

__示例 :__
示例 1:

输入: num = "123", target = 6
输出: ["1+2+3", "1*2*3"] 

示例 2:

输入: num = "232", target = 8
输出: ["2*3+2", "2+3*2"]

示例 3:

输入: num = "105", target = 5
输出: ["1*0+5","10-5"]

示例 4:

输入: num = "00", target = 0
输出: ["0+0", "0-0", "0*0"]

示例 5:

输入: num = "3456237490", target = 9191
输出: []

__思路__:
回溯法
每次选择一种运算符号
当遍历到 num的结尾且运算结果等于 target, 加入结果列表
时间复杂度O(a ^ n), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<string> addOperators(string num, int target) 
    {
        if (num.empty()) return result;
        for (int i = 0; i < num.size(); i++) num_after.emplace_back(stoll(num.substr(i)));
        exp.resize(num.size() * 2);
        dfs(num, target, 0, 0, 0, 1);
        return result;
    }


private:
    string exp;
    vector<string> result;
    vector<long long> num_after;
    
    int dfs(string& num, long long target, int exp_p, int pos, long long now, long long last) 
    {
        now = now * 10 + num[pos] - '0';
        exp[exp_p++] = num[pos];
        long long cur_val = now * last;
        if (pos == num.size() - 1) {
            if (target == cur_val) result.emplace_back(exp.substr(0, exp_p));
            return 0;
        }
        exp[exp_p] = '*';
        dfs(num, target, exp_p + 1, pos + 1, 0, cur_val);
        if (num_after[pos + 1] >= abs(target - cur_val)) {
            exp[exp_p] = '+';
            dfs(num, target - cur_val, exp_p + 1, pos + 1, 0, 1);
            exp[exp_p] = '-';
            dfs(num, target - cur_val, exp_p + 1, pos + 1, 0, -1);
        }
        if (now) dfs(num, target, exp_p, pos + 1, now, last);
        return 0;
    }
};
```

__Java__:
```Java
class Solution {
    public List<String> addOperators(String num, int target) {
        List<String> result = new ArrayList<>();
        trackback(num, target, 0, 0, 0, "", result);
        return result;
    }
    
    private void trackback(String num, int target, long curRes, long diff, int start, String curExp, List<String> expressions) {
        if (start == num.length() && (long)target == curRes) expressions.add(new String(curExp));
        for (int i = start; i < num.length(); i++) {
            String cur = num.substring(start, i + 1);
            if (cur.length() > 1 && cur.charAt(0) == '0') break;
            if (curExp.length() > 0) {
                trackback(num, target, curRes + Long.valueOf(cur), Long.valueOf(cur), i + 1, curExp + "+" + cur, expressions);
                trackback(num, target, curRes - Long.valueOf(cur), -Long.valueOf(cur), i + 1, curExp + "-" + cur, expressions);
                trackback(num, target, (curRes - diff) + diff * Long.valueOf(cur), diff * Long.valueOf(cur),
                      i + 1, curExp + "*" + cur, expressions);
            } else trackback(num, target, Long.valueOf(cur), Long.valueOf(cur), i + 1, cur, expressions);
        }
    }
}
```

__Python__:
```Python
class Solution:
    def addOperators(self, num: str, target: int) -> List[str]:
        result = []
        def trackback(num: str, target: int, cur_result: int, last: int, start: int, cur_exp: str, result: List[str]) -> None:
            if start == len(num) and target == cur_result:
                result.append(cur_exp)
            for i in range(start, len(num)):
                cur = num[start:i + 1]
                if len(cur) > 1 and cur[0] == '0':
                    break
                if cur_exp:
                    trackback(num, target, cur_result + int(cur), int(cur), i + 1, cur_exp + '+' + cur, result)
                    trackback(num, target, cur_result - int(cur), -int(cur), i + 1, cur_exp + '-' + cur, result)
                    trackback(num, target, cur_result - last + last * int(cur), int(cur) * last, i + 1, cur_exp + '*' + cur, result)
                else:
                    trackback(num, target, int(cur), int(cur), i + 1, cur, result)
        trackback(num, target, 0, 0, 0, '', result)
        return result
```