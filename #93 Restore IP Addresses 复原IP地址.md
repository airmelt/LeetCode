# 93 Restore IP Addresses 复原IP地址

__Description__:
Given a string containing only digits, restore it by returning all possible valid IP address combinations.

A valid IP address consists of exactly four integers (each integer is between 0 and 255) separated by single points.

__Example:__

Input: "25525511135"
Output: ["255.255.11.135", "255.255.111.35"]

__题目描述__:
给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。

有效的 IP 地址正好由四个整数（每个整数位于 0 到 255 之间组成），整数之间用 '.' 分隔。

__示例 :__

输入: "25525511135"
输出: ["255.255.11.135", "255.255.111.35"]

__思路__:

1. 回溯法
注意前导 0是不符合要求的
时间复杂度O(1), 空间复杂度O(1), 由于 ip地址最长为 12位, 相当于插入 3个 '.', 那么就是 C(11, 3)个需要检查的点
2. 暴力法
依次检查每 1-3个数字是否可以组成合理的 ip地址
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<string> restoreIpAddresses(string s) 
    {
        vector<string> result;
        backtrack(result, s, 0, 1, "");
        return result;
    }
private:
    void backtrack(vector<string>& result, string& s, int start, int n, string temp)
    {
        if (start > s.size()) return;
        for (int i = start; i < s.size() and i < start + 3; i++)
        {
            if (s.size() - 1 - i > 3 * (4 - n) or s.size() - 1 - i < 4 - n) continue;
            const string num = s.substr(start, i - start + 1);
            if (num[0] == '0' and num.size() > 1) continue;
            const int address = stoi(num);
            if (address < 256 and n == 4) 
            {
                temp += num;
                result.push_back(temp);
                return;
            }
            else if (address < 256) backtrack(result, s, i + 1, n + 1, temp + num + '.');
        }
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> restoreIpAddresses(String s) {
        List<String> result = new ArrayList<>();
        StringBuilder sb = new StringBuilder();
        for (int a = 1 ; a < 4 ; a++) for (int b = 1 ; b < 4 ; b++) for (int c = 1 ; c < 4 ; c++) for (int d = 1 ; d < 4 ; d++) {
            if (a + b + c + d == s.length()) {
                int n1 = Integer.parseInt(s.substring(0, a)), n2 = Integer.parseInt(s.substring(a, a + b)), n3 = Integer.parseInt(s.substring(a + b, a + b + c)), n4 = Integer.parseInt(s.substring(a + b + c));
                if (n1 < 256 && n2 < 256 && n3 < 256 && n4 < 256) {
                    sb.append(n1).append('.').append(n2).append('.').append(n3).append('.').append(n4);
                    if (sb.length() == s.length() + 3) result.add(sb.toString());
                    sb.delete(0, sb.length());
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        return list(s[:a] + '.' + s[a:a + b] + '.' + s[a + b:a + b + c] + '.' + s[a + b + c:] for a in range(1, 4) for b in range(1, 4) for c in range(1, 4) for d in range(1, 4) if a + b + c + d == len(s) and int(s[:a]) < 256 and int(s[a:a + b]) < 256 and int(s[a + b:a + b + c]) < 256 and int(s[a + b + c:]) < 256 and (a == 1 or s[:a][0] != '0') and (b == 1 or s[a:a + b][0] != '0') and (c == 1 or s[a + b:a + b + c][0] != '0') and (d == 1 or s[a + b + c:][0] != '0'))
```
