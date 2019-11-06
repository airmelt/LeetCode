__Description__:
You have an array of logs.  Each log is a space delimited string of words.

For each log, the first word in each log is an alphanumeric identifier.  Then, either:

Each word after the identifier will consist only of lowercase letters, or;
Each word after the identifier will consist only of digits.
We will call these two varieties of logs letter-logs and digit-logs.  It is guaranteed that each log has at least one word after its identifier.

Reorder the logs so that all of the letter-logs come before any digit-log.  The letter-logs are ordered lexicographically ignoring identifier, with the identifier used in case of ties.  The digit-logs should be put in their original order.

Return the final order of the logs.

__Example:__
Example 1:

Input: logs = ["dig1 8 1 5 1","let1 art can","dig2 3 6","let2 own kit dig","let3 art zero"]
Output: ["let1 art can","let3 art zero","let2 own kit dig","dig1 8 1 5 1","dig2 3 6"]

__Constraints:__

0 <= logs.length <= 100
3 <= logs[i].length <= 100
logs[i] is guaranteed to have an identifier, and a word after the identifier.

__题目描述__:
你有一个日志数组 logs。每条日志都是以空格分隔的字串。

对于每条日志，其第一个字为字母数字标识符。然后，要么：

标识符后面的每个字将仅由小写字母组成，或；
标识符后面的每个字将仅由数字组成。
我们将这两种日志分别称为字母日志和数字日志。保证每个日志在其标识符后面至少有一个字。

将日志重新排序，使得所有字母日志都排在数字日志之前。字母日志按内容字母顺序排序，忽略标识符；在内容相同时，按标识符排序。数字日志应该按原来的顺序排列。

返回日志的最终顺序。

__示例 :__

输入：["a1 9 2 3 1","g1 act car","zo4 4 7","ab1 off key dog","a8 act zoo"]
输出：["g1 act car","a8 act zoo","ab1 off key dog","a1 9 2 3 1","zo4 4 7"]
 
__提示：__

0 <= logs.length <= 100
3 <= logs[i].length <= 100
logs[i] 保证有一个标识符，并且标识符后面有一个字。

__思路__:
注意到题目中保证, 除了第一个空格之前的, log中的其他记录均为纯数字或纯字母, 只要比较每一个 log的最后一个字符即可
将字母的和数字的分开存放(也可以使用 map), 然后改写排序函数, 自定义排序即可
时间复杂度O(nlgn), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution {
public:
    vector<string> reorderLogFiles(vector<string>& logs) 
    {
        vector<string> result, nums;
        for (string log : logs)
        {
            if (isalpha(log[log.size() - 1])) result.push_back(log);
            else nums.push_back(log);
        }
        sort(result.begin(), result.end(), cmp);
        for (auto num : nums) result.push_back(num);
        return result;
    }
private:
    static bool cmp(const string& s1, const string& s2)
    {
        int i1 = s1.find(' '), i2 = s2.find(' ');
        string temp1 = s1.substr(i1 + 1), temp2 = s2.substr(i2 + 1);
        if (temp1 == temp2) return s1 < s2;
        return temp1 < temp2;
    }
};
```

__Java__:
```Java
class Solution {
    public String[] reorderLogFiles(String[] logs) {
        Arrays.sort(logs, (logs1, logs2) -> {
            String string1[] = logs1.split(" ", 2), string2[] = logs2.split(" ", 2);
            boolean isDigit1 = Character.isDigit(string1[1].charAt(0)), isDigit2 = Character.isDigit(string2[1].charAt(0));
            if (!isDigit1 && !isDigit2) {
                int cmp = string1[1].compareTo(string2[1]);
                if (cmp != 0) return cmp;
                return string1[0].compareTo(string2[0]);
            }
            return isDigit1 ? (isDigit2 ? 0 : 1) : -1;
        });
        return logs;
    }
}
```

__Python__:
```Python
class Solution:
    def reorderLogFiles(self, logs: List[str]) -> List[str]:
        return sorted([log for log in logs if log[-1].isalpha()], key=lambda word:(word.split(' ', 1)[1], word.split(' ', 1)[0])) + [log for log in logs if log[-1].isdigit()]
```