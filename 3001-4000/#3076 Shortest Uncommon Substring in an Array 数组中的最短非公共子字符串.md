# 3076 Shortest Uncommon Substring in an Array 数组中的最短非公共子字符串

__Description:__

You are given an array `arr` of size `n` consisting of __non-empty__ strings.

Find a string array `answer` of size `n` such that:

- `answer[i]` is the __shortest__ substring of `arr[i]` that does __not__ occur as a substring in any other string in `arr`. If multiple such substrings exist, `answer[i]` should be the lexicographically smallest. And if no such substring exists, `answer[i]` should be an empty string.

Return _the array_ `answer`.

__Example:__

Example 1:

```text
Input: arr = ["cab","ad","bad","c"]
Output: ["ab","","ba",""]
Explanation: We have the following:
```

- For the string "cab", the shortest substring that does not occur in any other string is either "ca" or "ab", we choose the lexicographically smaller substring, which is "ab".
- For the string "ad", there is no substring that does not occur in any other string.
- For the string "bad", the shortest substring that does not occur in any other string is "ba".
- For the string "c", there is no substring that does not occur in any other string.

Example 2:

```text
Input: arr = ["abc","bcd","abcd"]
Output: ["","","abcd"]
Explanation: We have the following:
```

- For the string "abc", there is no substring that does not occur in any other string.
- For the string "bcd", there is no substring that does not occur in any other string.
- For the string "abcd", the shortest substring that does not occur in any other string is "abcd".

__Constraints:__

- `n == arr.length`
- `2 <= n <= 100`
- `1 <= arr[i].length <= 20`
- `arr[i]` consists only of lowercase English letters.

__题目描述:__

给你一个数组 `arr` ，数组中有 `n` 个 _非空_ 字符串。

请你求出一个长度为 `n` 的字符串数组 `answer` ，满足:

- `answer[i]` 是 `arr[i]` __最短__ 的子字符串，且它不是 `arr` 中其他任何字符串的子字符串。如果有多个这样的子字符串存在， `answer[i]` 应该是它们中字典序最小的一个。如果不存在这样的子字符串， `answer[i]` 为空字符串。

请你返回数组 `answer` 。

__示例:__

示例 1：

```text
输入：arr = ["cab","ad","bad","c"]
输出：["ab","","ba",""]
解释：求解过程如下：
```

- 对于字符串 "cab" ，最短没有在其他字符串中出现过的子字符串是 "ca" 或者 "ab" ，我们选择字典序更小的子字符串，也就是 "ab" 。
- 对于字符串 "ad" ，不存在没有在其他字符串中出现过的子字符串。
- 对于字符串 "bad" ，最短没有在其他字符串中出现过的子字符串是 "ba" 。
- 对于字符串 "c" ，不存在没有在其他字符串中出现过的子字符串。

示例 2：

```text
输入：arr = ["abc","bcd","abcd"]
输出：["","","abcd"]
解释：求解过程如下：
```

- 对于字符串 "abc" ，不存在没有在其他字符串中出现过的子字符串。
- 对于字符串 "bcd" ，不存在没有在其他字符串中出现过的子字符串。
- 对于字符串 "abcd" ，最短没有在其他字符串中出现过的子字符串是 "abcd" 。

__提示：__

- `n == arr.length`
- `2 <= n <= 100`
- `1 <= arr[i].length <= 20`
- `arr[i]` 只包含小写英文字母。

__思路:__

```text
暴力法
由于 n 和 arr[i] 的长度都比较小考虑使用暴力法
对于任何一个 arr[i] 分别查询每一个子字符串
如果子字符串不是其他的字符串的字符串就更新结果
时间复杂度为 O(N ^ 2M ^ 4), 空间复杂度为 O(M), M 为字符串的平均长度, 本题不超过 20
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<string> shortestSubstrings(vector<string>& arr) 
    {
        int n = arr.size();
        vector<string> result(n);
        auto check = [&](const auto& i, const auto& t) 
        {
            for (int j = 0; j < n; j++) if (i != j and arr[j].find(t) != string::npos) return false;
            return true;
        };
        for (int i = 0; i < n; i++) 
        {
            string cur;
            for (int size = 1, m = arr[i].size(); size <= m and cur.empty(); size++) for (int j = size; j <= m; j++) if ((cur.empty() or arr[i].substr(j - size, size) < cur) and check(i, arr[i].substr(j - size, size))) cur = arr[i].substr(j - size, size);
            result[i] = cur;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String[] shortestSubstrings(String[] arr) {
        String[] result = new String[arr.length];
        for (int i = 0, n = arr.length; i < n; i++) {
            String cur = "";
            for (int size = 1, m = arr[i].length(); size <= m && cur.isEmpty(); size++) for (int j = size; j <= m; j++) if ((cur.isEmpty() || arr[i].substring(j - size, j).compareTo(cur) < 0) && check(i, arr[i].substring(j - size, j), arr)) cur = arr[i].substring(j - size, j);
            result[i] = cur;
        }
        return result;
    }

    private boolean check(int i, String t, String[] arr) {
        for (int j = 0, n = arr.length; j < n; j++) if (i != j && arr[j].contains(t)) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def shortestSubstrings(self, arr: List[str]) -> List[str]:
        def check(i: int, t: str) -> bool:
            for j, s in enumerate(arr):
                if i != j and t in s:
                    return False
            return True
        result = [''] * (n := len(arr))
        for i, s in enumerate(arr):
            m, cur = len(s), ''
            for size in range(1, m + 1):
                for j in range(size, m + 1):
                    if (not cur or s[j - size:j] < cur) and check(i, s[j - size:j]):
                        cur = s[j - size:j]
                if cur:
                    break
            result[i] = cur
        return result
```
