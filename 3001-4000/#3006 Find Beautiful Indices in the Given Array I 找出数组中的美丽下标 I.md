# 3006 Find Beautiful Indices in the Given Array I 找出数组中的美丽下标 I

__Description:__

You are given a __0-indexed__ string `s`, a string `a`, a string `b`, and an integer `k`.

An index `i` is __beautiful__ if:

- `0 <= i <= s.length - a.length`
- `s[i..(i + a.length - 1)] == a`

There exists an index `j` such that:

- `0 <= j <= s.length - b.length`
- `s[j..(j + b.length - 1)] == b`
- `|j - i| <= k`

Return the array that contains beautiful indices in sorted order from smallest to largest.

__Example:__

Example 1:

```text
Input: s = "isawsquirrelnearmysquirrelhouseohmy", a = "my", b = "squirrel", k = 15
Output: [16,33]
Explanation: There are 2 beautiful indices: [16,33].
```

- The index 16 is beautiful as s[16..17] == "my" and there exists an index 4 with s[4..11] == "squirrel" and |16 - 4| <= 15.
- The index 33 is beautiful as s[33..34] == "my" and there exists an index 18 with s[18..25] == "squirrel" and |33 - 18| <= 15.

Thus we return [16,33] as the result.

Example 2:

```text
Input: s = "abcd", a = "a", b = "a", k = 4
Output: [0]
Explanation: There is 1 beautiful index: [0].
```

- The index 0 is beautiful as s[0..0] == "a" and there exists an index 0 with s[0..0] == "a" and |0 - 0| <= 4.

Thus we return [0] as the result.

__Constraints:__

- `1 <= k <= s.length <= 10 ^ 5`
- `1 <= a.length, b.length <= 10`
- `s`, `a`, and `b` contain only lowercase English letters.

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `s` 、字符串 `a` 、字符串 `b` 和一个整数 `k` 。

如果下标 `i` 满足以下条件，则认为它是一个 __美丽下标__:

- `0 <= i <= s.length - a.length`
- `s[i..(i + a.length - 1)] == a`

存在下标 `j` 使得:

- `0 <= j <= s.length - b.length`
- `s[j..(j + b.length - 1)] == b`
- `|j - i| <= k`

以数组形式按 从小到大排序 返回美丽下标。

__示例:__

示例 1：

```text
输入：s = "isawsquirrelnearmysquirrelhouseohmy", a = "my", b = "squirrel", k = 15
输出：[16,33]
解释：存在 2 个美丽下标：[16,33]。
```

- 下标 16 是美丽下标，因为 s[16..17] == "my" ，且存在下标 4 ，满足 s[4..11] == "squirrel" 且 |16 - 4| <= 15 。
- 下标 33 是美丽下标，因为 s[33..34] == "my" ，且存在下标 18 ，满足 s[18..25] == "squirrel" 且 |33 - 18| <= 15 。

因此返回 [16,33] 作为结果。

示例 2：

```text
输入：s = "abcd", a = "a", b = "a", k = 4
输出：[0]
解释：存在 1 个美丽下标：[0]。
```

- 下标 0 是美丽下标，因为 s[0..0] == "a" ，且存在下标 0 ，满足 s[0..0] == "a" 且 |0 - 0| <= 4 。

因此返回 [0] 作为结果。

__提示：__

- `1 <= k <= s.length <= 10 ^ 5`
- `1 <= a.length, b.length <= 10`
- `s`、 `a`、和 `b` 只包含小写英文字母。

__思路:__

```text
模拟
暴力法比较两个 a 和 b 对应的下标是否在 |i - j| <= k 范围内
时间复杂度为 O(NA + NB), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> beautifulIndices(string_view s, string_view a, string_view b, int k) 
    {
        vector<int> result;
        int i = s.find(a, 0), j = s.find(b, 0);
        while (i != string_view::npos && j != string_view::npos) 
        {
            while (j != string_view::npos && i - j > k) j = s.find(b, j + 1);
            if (j != string_view::npos && abs(i - j) <= k) result.emplace_back(i);
            i = s.find(a, i + 1);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> beautifulIndices(String s, String a, String b, int k) {
        List<Integer> result = new ArrayList<>();
        int i = s.indexOf(a, 0), j = s.indexOf(b, 0);
        while (i != -1 && j != -1) {
            while (j != -1 && i - j > k) j = s.indexOf(b, j + 1);
            if (j != -1 && Math.abs(i - j) <= k) result.add(i);
            i = s.indexOf(a, i + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def beautifulIndices(self, s: str, a: str, b: str, k: int) -> List[int]:
        result, i, j = [], s.find(a) if a in s else -inf, s.find(b) if b in s else -inf
        while i != -inf and j != -inf:
            while j != -inf and i - j > k:
                j = (s[j + 1:].find(b) + j + 1) if b in s[j + 1:] else -inf
            if j != -inf and abs(i - j) <= k:
                result.append(i)
            i = (s[i + 1:].find(a) + i + 1) if a in s[i + 1:] else -inf
        return result
```
