# 2306 Naming a Company 公司命名

__Description:__

You are given an array of strings `ideas` that represents a list of names to be used in the process of naming a company. The process of naming a company is as follows:

Return the number of distinct valid names for the company.

__Example:__

Example 1:

```text
Input: ideas = ["coffee","donuts","time","toffee"]
Output: 6
Explanation: The following selections are valid:
```

- ("coffee", "donuts"): The company name created is "doffee conuts".
- ("donuts", "coffee"): The company name created is "conuts doffee".
- ("donuts", "time"): The company name created is "tonuts dime".
- ("donuts", "toffee"): The company name created is "tonuts doffee".
- ("time", "donuts"): The company name created is "dime tonuts".
- ("toffee", "donuts"): The company name created is "doffee tonuts".

Therefore, there are a total of 6 distinct company names.

The following are some examples of invalid selections:

- ("coffee", "time"): The name "toffee" formed after swapping already exists in the original array.
- ("time", "toffee"): Both names are still the same after swapping and exist in the original array.
- ("coffee", "toffee"): Both names formed after swapping already exist in the original array.

Example 2:

```text
Input: ideas = ["lack","back"]
Output: 0
Explanation: There are no valid selections. Therefore, 0 is returned.
```

__Constraints:__

- `2 <= ideas.length <= 5 * 10 ^ 4`
- `1 <= ideas[i].length <= 10`
- `ideas[i]` consists of lowercase English letters.
- All the strings in `ideas` are __unique__.

__题目描述:__

给你一个字符串数组 `ideas` 表示在公司命名过程中使用的名字列表。公司命名流程如下:

返回 不同 且有效的公司名字的数目。

__示例:__

示例 1：

```text
输入：ideas = ["coffee","donuts","time","toffee"]
输出：6
解释：下面列出一些有效的选择方案：
```

- ("coffee", "donuts")：对应的公司名字是 "doffee conuts" 。
- ("donuts", "coffee")：对应的公司名字是 "conuts doffee" 。
- ("donuts", "time")：对应的公司名字是 "tonuts dime" 。
- ("donuts", "toffee")：对应的公司名字是 "tonuts doffee" 。
- ("time", "donuts")：对应的公司名字是 "dime tonuts" 。
- ("toffee", "donuts")：对应的公司名字是 "doffee tonuts" 。

因此，总共有 6 个不同的公司名字。

下面列出一些无效的选择方案：

- ("coffee", "time")：在原数组中存在交换后形成的名字 "toffee" 。
- ("time", "toffee")：在原数组中存在交换后形成的两个名字。
- ("coffee", "toffee")：在原数组中存在交换后形成的两个名字。

示例 2：

```text
输入：ideas = ["lack","back"]
输出：0
解释：不存在有效的选择方案。因此，返回 0 。
```

__提示：__

- `2 <= ideas.length <= 5 * 10 ^ 4`
- `1 <= ideas[i].length <= 10`
- `ideas[i]` 由小写英文字母组成
- `ideas` 中的所有字符串 __互不相同__

__思路:__

```text
模拟
按照单词的首字母分组
将首字母之后的字符串存入哈希表中对应的首字母集合中
对于每个字符串统计在不同首字母集合中的交集大小
用集合大小减去交集大小的乘积累加
最后结果乘以 2 即可
时间复杂度为 O(MN), 空间复杂度为 O(MN), 其中 M 为 ideas 的长度, N 为字符串的平均长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long distinctNames(vector<string>& ideas) 
    {
        unordered_set<string> groups[26];
        for (const auto& idea : ideas) groups[idea.front() - 'a'].insert(idea.substr(1));
        long long result = 0;
        for (int i = 1; i < 26; i++) 
        {
            for (int j = 0, m = 0; j < i; j++, m = 0) 
            {
                for (const auto& s : groups[i]) m += groups[j].count(s);
                result += (long long)(groups[i].size() - m) * (groups[j].size() - m);
            }
        }
        return result << 1;
    }
};
```

__Java__:

```Java
class Solution {
    public long distinctNames(String[] ideas) {
        Set<String>[] groups = new HashSet[26];
        Arrays.setAll(groups, i -> new HashSet<>());
        for (String idea : ideas) groups[idea.charAt(0) - 'a'].add(idea.substring(1));
        long result = 0;
        for (int i = 1; i < 26; i++) {
            for (int j = 0, m = 0; j < i; j++, m = 0) {
                for (String s : groups[i]) if (groups[j].contains(s)) ++m;
                result += (long)(groups[i].size() - m) * (groups[j].size() - m);
            }
        }
        return result << 1;
    }
}
```

__Python__:

```Python
class Solution:
    def distinctNames(self, ideas: List[str]) -> int:
        c, result = defaultdict(set), 0
        for i in ideas:
            c[i[0]].add(i[1:])
        return sum((len(a) - (m := len(a & b))) * (len(b) - m) for a, b in combinations(c.values(), 2)) << 1
```
