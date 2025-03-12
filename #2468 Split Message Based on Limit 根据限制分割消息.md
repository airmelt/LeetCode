# 2468 Split Message Based on Limit 根据限制分割消息

__Description:__

You are given a string, `message`, and a positive integer, `limit`.

You must __split__ `message` into one or more __parts__ based on `limit`. Each resulting part should have the suffix `"<a/b>"`, where `"b"` is to be __replaced__ with the total number of parts and `"a"` is to be __replaced__ with the index of the part, starting from `1` and going up to `b`. Additionally, the length of each resulting part (including its suffix) should be __equal__ to `limit`, except for the last part whose length can be __at most__ `limit`.

The resulting parts should be formed such that when their suffixes are removed and they are all concatenated __in order__, they should be equal to `message`. Also, the result should contain as few parts as possible.

Return _the parts_ `message` _would be split into as an array of strings_. If it is impossible to split `message` as required, return _an empty array_.

__Example:__

Example 1:

```text
Input: message = "this is really a very awesome message", limit = 9
Output: ["thi<1/14>","s i<2/14>","s r<3/14>","eal<4/14>","ly <5/14>","a v<6/14>","ery<7/14>"," aw<8/14>","eso<9/14>","me<10/14>"," m<11/14>","es<12/14>","sa<13/14>","ge<14/14>"]
Explanation:
The first 9 parts take 3 characters each from the beginning of message.
The next 5 parts take 2 characters each to finish splitting message. 
In this example, each part, including the last, has length 9. 
It can be shown it is not possible to split message into less than 14 parts.
```

Example 2:

```text
Input: message = "short message", limit = 15
Output: ["short mess<1/2>","age<2/2>"]
Explanation:
Under the given constraints, the string can be split into two parts: 
```

- The first part comprises of the first 10 characters, and has a length 15.
- The next part comprises of the last 3 characters, and has a length 8.

__Constraints:__

- `1 <= message.length <= 10 ^ 4`
- `message` consists only of lowercase English letters and `' '`.
- `1 <= limit <= 10 ^ 4`

__题目描述:__

给你一个字符串 `message` 和一个正整数 `limit` 。

你需要根据 `limit` 将 `message` __分割__ 成一个或多个 __部分__ 。每个部分的结尾都是 `"<a/b>"` ，其中 `"b"` 用分割出来的总数 _替换_ ， `"a"` 用当前部分所在的编号 __替换__ ，编号从 `1` 到 `b` 依次编号。除此以外，除了最后一部分长度 __小于等于__ `limit` 以外，其他每一部分（包括结尾部分）的长度都应该 __等于__ `limit` 。

你需要确保分割后的结果数组，删掉每部分的结尾并 __按顺序__ 连起来后，能够得到 `message` 。同时，结果数组越短越好。

请你返回 `message` 分割后得到的结果数组。如果无法按要求分割 `message` ，返回一个空数组。

__示例:__

示例 1：

```text
输入：message = "this is really a very awesome message", limit = 9
输出：["thi<1/14>","s i<2/14>","s r<3/14>","eal<4/14>","ly <5/14>","a v<6/14>","ery<7/14>"," aw<8/14>","eso<9/14>","me<10/14>"," m<11/14>","es<12/14>","sa<13/14>","ge<14/14>"]
解释：
前面 9 个部分分别从 message 中得到 3 个字符。
接下来的 5 个部分分别从 message 中得到 2 个字符。
这个例子中，包含最后一个部分在内，每个部分的长度都为 9 。
可以证明没有办法分割成少于 14 个部分。
```

示例 2：

```text
输入：message = "short message", limit = 15
输出：["short mess<1/2>","age<2/2>"]
解释：
在给定限制下，字符串可以分成两个部分：
```

- 第一个部分包含 10 个字符，长度为 15 。
- 第二个部分包含 3 个字符，长度为 8 。

__提示：__

- `1 <= message.length <= 10 ^ 4`
- `message` 只包含小写英文字母和 `' '` 。
- `1 <= limit <= 10 ^ 4`

__思路:__

```text
模拟
可以用数学方式计算出一共有几个部分
也可以枚举部分数量
然后按照题意模拟即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<string> splitMessage(string message, int limit) 
    {
        for (int i = 1, cap = 0, tail, n = message.size(); ; i++) 
        {
            if (i < 10) tail = 5;
            else if (i < 100) 
            {
                if (i == 10) cap -= 9;
                tail = 7;
            } 
            else if (i < 1000) 
            {
                if (i == 100) cap -= 99;
                tail = 9;
            } 
            else 
            {
                if (i == 1000) cap -= 999;
                tail = 11;
            }
            if (tail >= limit) return {};
            cap += limit - tail;
            if (cap < n) continue;
            vector<string> result(i);
            for (int j = 0, k = 0, m = 0; j < i; j++) 
            {
                string part = "<" + to_string(j + 1) + "/" + to_string(i) + ">";
                if (j == i - 1) result[j] = message.substr(k) + part;
                else 
                {
                    result[j] = message.substr(k, m = limit - part.size()) + part;
                    k += m;
                }
            }
            return result;
        }
    }
};
```

__Java__:

```Java
class Solution {
    public String[] splitMessage(String message, int limit) {
        for (int i = 1, cap = 0, tail, n = message.length(); ; i++) {
            if (i < 10) tail = 5;
            else if (i < 100) {
                if (i == 10) cap -= 9;
                tail = 7;
            } else if (i < 1000) {
                if (i == 100) cap -= 99;
                tail = 9;
            } else {
                if (i == 1000) cap -= 999;
                tail = 11;
            }
            if (tail >= limit) return new String[]{};
            cap += limit - tail;
            if (cap < n) continue;
            var result = new String[i];
            for (int j = 0, k = 0, m = 0; j < i; j++) {
                var part = "<" + (j + 1) + "/" + i + ">";
                if (j == i - 1) result[j] = message.substring(k) + part;
                else {
                    result[j] = message.substring(k, k + (m = limit - part.length())) + part;
                    k += m;
                }
            }
            return result;
        }
    }
}
```

__Python__:

```Python
class Solution:
    def splitMessage(self, message: str, limit: int) -> List[str]:
        n, result = len(message), []
        if n <= 9 * (limit - 5):
            m = n // (limit - 5) + (not not n % (limit - 5))
            for i in range(m):
                result.append(message[(limit - 5) * i:(limit - 5) * (i + 1)] + '<' + str(i + 1) + '/' + str(m) + '>')
        elif n <= 9 * (limit - 6) + 90 * (limit - 7):
            m = (k := (n - (9 * (limit - 6)))) // (limit - 7) + (not not k % (limit - 7)) + 9
            for i in range(9):
                result.append(message[(limit - 6) * i:(limit - 6) * (i + 1)] + '<' + str(i + 1) + '/' + str(m) + '>')
            start = 9 * (limit - 6)
            for i in range(9, m):
                result.append(message[start + (limit - 7) * (i - 9):start + (limit - 7) * (i - 8)] + '<' + str(i + 1) + '/' + str(m) + '>')
        elif n <= 9 * (limit - 7) + 90 * (limit - 8) + 900 * (limit - 9):
            n -= 9 * (limit - 7)
            n -= 90 * (limit - 8)
            m = n // (limit - 9) + (not not n % (limit - 9)) + 99
            for i in range(9):
                result.append(message[(limit - 7) * i:(limit - 7) * (i + 1)] + '<' + str(i + 1) + '/' + str(m) + '>')
            start = 9 * (limit - 7)
            for i in range(9, 99):
                result.append(message[start + (limit - 8) * (i - 9):start + (limit - 8) * (i - 8)] + '<' + str(i + 1) + '/' + str(m) + '>')
            start += 90 * (limit - 8)
            for i in range(99, m):
                result.append(message[start + (limit - 9) * (i - 99):start + (limit - 9) * (i - 98)] + '<' + str(i + 1) + '/' + str(m) + '>')
        elif n <= 9 * (limit - 8) + 90 * (limit - 9) + 900 * (limit - 10) + 9000 * (limit - 11):
            n -= 9 * (limit - 8)
            n -= 90 * (limit - 9)
            n -= 900 * (limit - 10)
            m = n // (limit - 11) + (not not n % (limit - 11)) + 999
            for i in range(9):
                result.append(message[(limit - 8) * i:(limit - 8) * (i + 1)] + '<' + str(i + 1) + '/' + str(m) + '>')
            start = 9 * (limit - 8)
            for i in range(9, 99):
                result.append(message[start + (limit - 9) * (i - 9):start + (limit - 9) * (i - 8)] + '<' + str(i + 1) + '/' + str(m) + '>')
            start += 90 * (limit - 9)
            for i in range(99, 999):
                result.append(message[start + (limit - 10) * (i - 99):start + (limit - 10) * (i - 98)] + '<' + str(i + 1) + '/' + str(m) + '>')
            start += 900 * (limit - 10)
            for i in range(999, m):
                result.append(message[start + (limit - 11) * (i - 999):start + (limit - 11) * (i - 998)] + '<' + str(i + 1) + '/' + str(m) + '>')
        return result
```
