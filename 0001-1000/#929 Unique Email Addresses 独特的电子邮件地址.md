# 929 Unique Email Addresses 独特的电子邮件地址

__Description__:
Every email consists of a local name and a domain name, separated by the @ sign.

For example, in ```alice@leetcode.com```, alice is the local name, and leetcode.com is the domain name.

Besides lowercase letters, these emails may contain '.'s or '+'s.

If you add periods ('.') between some characters in the local name part of an email address, mail sent there will be forwarded to the same address without dots in the local name.  For example, "```alice.z@leetcode.com```" and "```alicez@leetcode.com```" forward to the same email address.  (Note that this rule does not apply for domain names.)

If you add a plus ('+') in the local name, everything after the first plus sign will be ignored. This allows certain emails to be filtered, for example ```m.y+name@email.com``` will be forwarded to ```my@email.com```.  (Again, this rule does not apply for domain names.)

It is possible to use both of these rules at the same time.

Given a list of emails, we send one email to each address in the list.  How many different addresses actually receive mails?

__Example:__

Example 1:

Input: ["```test.email+alex@leetcode.com```","```test.e.mail+bob.cathy@leetcode.com```","```testemail+david@lee.tcode.com```"]
Output: 2
Explanation: "```testemail@leetcode.com```" and "```testemail@lee.tcode.com```" actually receive mails

__Note:__

1 <= emails[i].length <= 100
1 <= emails.length <= 100
Each emails[i] contains exactly one '@' character.
All local and domain names are non-empty.
Local names do not start with a '+' character.

__题目描述__:
每封电子邮件都由一个本地名称和一个域名组成，以 @ 符号分隔。

例如，在 alice@leetcode.com中， alice 是本地名称，而 leetcode.com 是域名。

除了小写字母，这些电子邮件还可能包含 '.' 或 '+'。

如果在电子邮件地址的本地名称部分中的某些字符之间添加句点（'.'），则发往那里的邮件将会转发到本地名称中没有点的同一地址。例如，"```alice.z@leetcode.com```” 和 “```alicez@leetcode.com```” 会转发到同一电子邮件地址。 （请注意，此规则不适用于域名。）

如果在本地名称中添加加号（'+'），则会忽略第一个加号后面的所有内容。这允许过滤某些电子邮件，例如 ```m.y+name@email.com``` 将转发到 ```my@email.com```。 （同样，此规则不适用于域名。）

可以同时使用这两个规则。

给定电子邮件列表 emails，我们会向列表中的每个地址发送一封电子邮件。实际收到邮件的不同地址有多少？

__示例 :__

输入：["```test.email+alex@leetcode.com```","```test.e.mail+bob.cathy@leetcode.com```","```testemail+david@lee.tcode.com```"]
输出：2
解释：实际收到邮件的是 "```testemail@leetcode.com```" 和 "```testemail@lee.tcode.com```"。

__提示：__

1 <= emails[i].length <= 100
1 <= emails.length <= 100
每封 emails[i] 都包含有且仅有一个 '@' 字符。

__思路__:

遍历 emails数组, 用 @分割开每一个 email得到 name和 domain, 在 name中查找 '+'和 '.'按题目要求处理之后加入 set, 返回 set大小即可
时间复杂度O(mn), 空间复杂度O(n), n为数组大小, m为字符串长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numUniqueEmails(vector<string>& emails) 
    {
        unordered_set<string> s;
        for (auto &email : emails)
        {
            int pos = email.find('@');
            string domain = email.substr(pos), name = email.substr(0, pos);
            if ((pos = email.find('+')) != string::npos) name = name.substr(0, pos);
            while ((pos = name.find('.')) != string::npos) name = name.erase(pos, 1);
            s.insert(name + domain);
        }
        return s.size();
    }
};
```

__Java__:

```Java
class Solution {
    public int numUniqueEmails(String[] emails) {
        Set<String> set = new HashSet<>();
        for (String email : emails) {
            String temp[] = email.split("@");
            if (temp[0].contains("+")) temp[0] = temp[0].substring(0, temp[0].indexOf("+"));
            temp[0] = temp[0].replaceAll("\\.", "");
            set.add(temp[0] + "@" + temp[1]);
        }
        return set.size();
    }
}
```

__Python__:

```Python
class Solution:
    def numUniqueEmails(self, emails: List[str]) -> int:
        return len(set([(lambda email: email[0][:email[0].find('+')].replace('.', '') + '@' + email[1] if '+' in email[0] else email[0].replace('.', '') + '@' + email[1])(email.split('@')) for email in emails]))
```
