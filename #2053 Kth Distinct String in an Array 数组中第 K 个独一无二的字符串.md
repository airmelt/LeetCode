# 2053 Kth Distinct String in an Array 数组中第 K 个独一无二的字符串

__Description:__

A distinct string is a string that is present only once in an array.

Given an array of strings `arr`, and an integer `k`, return _the_ `k ^ th` ___distinct string__ present in_ `arr`. If there are __fewer__ than `k` distinct strings, return _an __empty string___ `""`.

Note that the strings are considered in the order in which they appear in the array.

__Example:__

Example 1:

```text
Input: arr = ["d","b","c","b","c","a"], k = 2
Output: "a"
Explanation:
The only distinct strings in arr are "d" and "a".
"d" appears 1st, so it is the 1st distinct string.
"a" appears 2nd, so it is the 2nd distinct string.
Since k == 2, "a" is returned.
```

Example 2:

```text
Input: arr = ["aaa","aa","a"], k = 1
Output: "aaa"
Explanation:
All strings in arr are distinct, so the 1st string "aaa" is returned.
```

Example 3:

```text
Input: arr = ["a","b","a"], k = 3
Output: ""
Explanation:
The only distinct string is "b". Since there are fewer than 3 distinct strings, we return an empty string "".
```

__Constraints:__

- `1 <= k <= arr.length <= 1000`
- `1 <= arr[i].length <= 5`
- `arr[i]` consists of lowercase English letters.

__题目描述:__

独一无二的字符串 指的是在一个数组中只出现过 一次 的字符串。

给你一个字符串数组 `arr` 和一个整数 `k` ，请你返回 `arr` 中第 `k` 个 __独一无二的字符串__ 。如果 __少于__ `k` 个独一无二的字符串，那么返回 __空字符串__ `""` 。

注意，按照字符串在原数组中的 __顺序__ 找到第 `k` 个独一无二字符串。

__示例:__

示例 1:

```text
输入: arr = ["d","b","c","b","c","a"], k = 2
输出: "a"
解释: 
arr 中独一无二字符串包括 "d" 和 "a" `。`
"d" 首先出现，所以它是第 1 个独一无二字符串。
"a" 第二个出现，所以它是 2 个独一无二字符串。
由于 k == 2 ，返回 "a" 。
```

示例 2:

```text
输入：arr = ["aaa","aa","a"], k = 1
输出："aaa"
解释：
arr 中所有字符串都是独一无二的，所以返回第 1 个字符串 "aaa" 。
```

示例 3：

```text
输入：arr = ["a","b","a"], k = 3
输出：""
解释：
唯一一个独一无二字符串是 "b" 。由于少于 3 个独一无二字符串，我们返回空字符串 "" 。
```

__提示：__

- `1 <= k <= arr.length <= 1000`
- `1 <= arr[i].length <= 5`
- `arr[i]` 只包含小写英文字母。

__思路:__

```text
哈希表
用哈希表记录每个字符串出现的次数
然后遍历数组, 找到每个出现次数为 1 的字符串
返回第 k 个即可
不足 k 个返回空字符串
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string kthDistinct(vector<string>& arr, int k) 
    {
        unordered_map<string, int> m;
        for (const auto& i : arr) ++m[i];
        for (const auto& i : arr) if (m[i] == 1) if (!(--k)) return i;
        return "";
    }
};
```

__Java__:

```Java
class Solution {
    public String kthDistinct(String[] arr, int k) {
        Map<String, Integer> map = new HashMap<>();
        for (String i : arr) map.merge(i, 1, Integer::sum);
        for (String i : arr) if (map.get(i) == 1) if (--k == 0) return i;
        return "";
    }
}
```

__Python__:

```Python
class Solution:
    def kthDistinct(self, arr: List[str], k: int) -> str:
```
