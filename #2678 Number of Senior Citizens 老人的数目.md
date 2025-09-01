# 2678 Number of Senior Citizens 老人的数目

__Description:__

You are given a __0-indexed__ array of strings `details`. Each element of `details` provides information about a given passenger compressed into a string of length `15`. The system is such that:

- The first ten characters consist of the phone number of passengers.
- The next character denotes the gender of the person.
- The following two characters are used to indicate the age of the person.
- The last two characters determine the seat allotted to that person.

Return the number of passengers who are strictly more than 60 years old.

__Example:__

Example 1:

```text
Input: details = ["7868190130M7522","5303914400F9211","9273338290F4010"]
Output: 2
Explanation: The passengers at indices 0, 1, and 2 have ages 75, 92, and 40. Thus, there are 2 people who are over 60 years old.
```

Example 2:

```text
Input: details = ["1313579440F2036","2921522980M5644"]
Output: 0
Explanation: None of the passengers are older than 60.
```

__Constraints:__

- `1 <= details.length <= 100`
- `details[i].length == 15`
- `details[i] consists of digits from '0' to '9'.`
- `details[i][10] is either 'M' or 'F' or 'O'.`
- The phone numbers and seat numbers of the passengers are distinct.

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `details` 。 `details` 中每个元素都是一位乘客的信息，信息用长度为 `15` 的字符串表示，表示方式如下:

- 前十个字符是乘客的手机号码。
- 接下来的一个字符是乘客的性别。
- 接下来两个字符是乘客的年龄。
- 最后两个字符是乘客的座位号。

请你返回乘客中年龄 严格大于 60 岁 的人数。

__示例:__

示例 1：

```text
输入：details = ["7868190130M7522","5303914400F9211","9273338290F4010"]
输出：2
解释：下标为 0 ，1 和 2 的乘客年龄分别为 75 ，92 和 40 。所以有 2 人年龄大于 60 岁。
```

示例 2：

```text
输入：details = ["1313579440F2036","2921522980M5644"]
输出：0
解释：没有乘客的年龄大于 60 岁。
```

__提示：__

- `1 <= details.length <= 100`
- `details[i].length == 15`
- `details[i]` 中的数字只包含 `'0'` 到 `'9'` 。
- `details[i][10]` 是 `'M'` ， `'F'` 或者 `'O'` 之一。
- 所有乘客的手机号码和座位号互不相同。

__思路:__

```text
模拟
检查每一个字符串的第 12 位和第 13 位的数字是否大于 60
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countSeniors(vector<string>& details) 
    {
        return count_if(details.begin(), details.end(), [](const auto &s) { return (s[11] - '0') * 10 + (s[12] - '0') > 60; });
    }
};
```

__Java__:

```Java
class Solution {
    public int countSeniors(String[] details) {
        return (int)Arrays.stream(details).filter(s -> ((s.charAt(11) - '0') * 10 + s.charAt(12) - '0' > 60)).count();
    }
}
```

__Python__:

```Python
class Solution:
    def countSeniors(self, details: List[str]) -> int:
        return sum(int(d[-4:-2]) > 60 for d in details)
```
