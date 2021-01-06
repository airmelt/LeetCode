# 49 Group Anagrams 字母异位词分组

__Description__:
Given an array of strings, group anagrams together.

__Example:__

Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]

__Note:__

All inputs will be in lowercase.
The order of your output does not matter.

__题目描述__:
给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

__示例 :__

输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]

__说明:__

所有输入均为小写字母。
不考虑答案输出的顺序。

__思路__:

1. 对每个字符串进行排序后分组
时间复杂度O(mnlgn), 空间复杂度O(mn), 其中 m为 strs数组的长度, n为 strs中字符串的最大长度
2. 将每个字符串进行 hash, 可以用质数乘积表示, 也可以用一个字符串表示
时间复杂度O(mn), 空间复杂度O(mn), 其中 m为 strs数组的长度, n为 strs中字符串的最大长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) 
    {
        int primes[26] = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101};
        unordered_map<double, vector<string>> m;
        for (auto &s : strs) 
        {
            double value = 1;
            for (auto &c : s) value *= primes[c - 'a'];
            m[value].push_back(s);
        }
        vector<vector<string>> result;
        for (auto &i : m) result.push_back(i.second);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List> result = new HashMap<String, List>();
        for (String s : strs) {
            char[] c = s.toCharArray();
            Arrays.sort(c);
            String key = String.valueOf(c);
            if (!result.containsKey(key)) result.put(key, new ArrayList());
            result.get(key).add(s);
        }
        return new ArrayList(result.values());
    }
}
```

__Python__:

```Python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        return [[*x] for _, x in itertools.groupby(sorted(strs, key=sorted), sorted)]
```
