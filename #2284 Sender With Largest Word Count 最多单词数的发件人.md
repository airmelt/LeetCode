# 2284 Sender With Largest Word Count 最多单词数的发件人

__Description:__

You have a chat log of `n` messages. You are given two string arrays `messages` and `senders` where `messages[i]` is a __message__ sent by `senders[i]`.

A message is list of words that are separated by a single space with no leading or trailing spaces. The word count of a sender is the total number of words sent by the sender. Note that a sender may send more than one message.

Return the sender with the largest word count. If there is more than one sender with the largest word count, return the one with the lexicographically largest name.

Note:

- Uppercase letters come before lowercase letters in lexicographical order.
- `"Alice"` and `"alice"` are distinct.

__Example:__

Example 1:

```text
Input: messages = ["Hello userTwooo","Hi userThree","Wonderful day Alice","Nice day userThree"], senders = ["Alice","userTwo","userThree","Alice"]
Output: "Alice"
Explanation: Alice sends a total of 2 + 3 = 5 words.
userTwo sends a total of 2 words.
userThree sends a total of 3 words.
Since Alice has the largest word count, we return "Alice".
```

Example 2:

```text
Input: messages = ["How is leetcode for everyone","Leetcode is useful for practice"], senders = ["Bob","Charlie"]
Output: "Charlie"
Explanation: Bob sends a total of 5 words.
Charlie sends a total of 5 words.
Since there is a tie for the largest word count, we return the sender with the lexicographically larger name, Charlie.
```

__Constraints:__

- `n == messages.length == senders.length`
- `1 <= n <= 10 ^ 4`
- `1 <= messages[i].length <= 100`
- `1 <= senders[i].length <= 10`
- `messages[i]` consists of uppercase and lowercase English letters and `' '`.
- All the words in `messages[i]` are separated by __a single space__.
- `messages[i]` does not have leading or trailing spaces.
- `senders[i]` consists of uppercase and lowercase English letters only.

__题目描述:__

给你一个聊天记录，共包含 `n` 条信息。给你两个字符串数组 `messages` 和 `senders` ，其中 `messages[i]` 是 `senders[i]` 发出的一条 __信息__ 。

一条 信息 是若干用单个空格连接的 单词 ，信息开头和结尾不会有多余空格。发件人的 单词计数 是这个发件人总共发出的 单词数 。注意，一个发件人可能会发出多于一条信息。

请你返回发出单词数 最多 的发件人名字。如果有多个发件人发出最多单词数，请你返回 字典序 最大的名字。

注意：

- 字典序里，大写字母小于小写字母。
- `"Alice"` 和 `"alice"` 是不同的名字。

__示例:__

示例 1：

```text
输入：messages = ["Hello userTwooo","Hi userThree","Wonderful day Alice","Nice day userThree"], senders = ["Alice","userTwo","userThree","Alice"]
输出："Alice"
解释：Alice 总共发出了 2 + 3 = 5 个单词。
userTwo 发出了 2 个单词。
userThree 发出了 3 个单词。
由于 Alice 发出单词数最多，所以我们返回 "Alice" 。
```

示例 2：

```text
输入：messages = ["How is leetcode for everyone","Leetcode is useful for practice"], senders = ["Bob","Charlie"]
输出："Charlie"
解释：Bob 总共发出了 5 个单词。
Charlie 总共发出了 5 个单词。
由于最多单词数打平，返回字典序最大的名字，也就是 Charlie 。
```

__提示：__

- `n == messages.length == senders.length`
- `1 <= n <= 10 ^ 4`
- `1 <= messages[i].length <= 100`
- `1 <= senders[i].length <= 10`
- `messages[i]` 包含大写字母、小写字母和 `' '` 。
- `messages[i]` 中所有单词都由 __单个空格__ 隔开。
- `messages[i]` 不包含前导和后缀空格。
- `senders[i]` 只包含大写英文字母和小写英文字母。

__思路:__

```text
哈希表
记录每个用户发的单词数
可以用 split 方法也可以统计空格的个数再加 1
遍历哈希表, 找到单词数最多的用户, 如果单词数相同, 则返回字典序最大的用户
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string largestWordCount(vector<string>& messages, vector<string>& senders) 
    {
        unordered_map<string, int> m;
        string result = senders.front();
        for (int i = 0, n = messages.size(); i < n; i++) m[senders[i]] += count(messages[i].begin(), messages[i].end(), ' ') + 1;
        for (const auto& [k, v] : m) if (v > m[result] or v == m[result] and k > result) result = k;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String largestWordCount(String[] messages, String[] senders) {
        Map<String, Integer> map = new HashMap<>();
        String result = senders[0];
        for (int i = 0, n = messages.length; i < n; i++) map.merge(senders[i], (int)messages[i].chars().filter(c -> c == ' ').count() + 1, Integer::sum);
        for (var entry : map.entrySet()) if (entry.getValue() > map.get(result) || entry.getValue() == map.get(result) && result.compareTo(entry.getKey()) < 0) result = entry.getKey();
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def largestWordCount(self, messages: List[str], senders: List[str]) -> str:
        d, n, result = defaultdict(int), len(messages), senders[0]
        for message, sender in zip(messages, senders):
            d[sender] += len(message.split())
        for k, v in d.items():
            if v > d[result] or v == d[result] and k > result:
                result = k
        return result
```
