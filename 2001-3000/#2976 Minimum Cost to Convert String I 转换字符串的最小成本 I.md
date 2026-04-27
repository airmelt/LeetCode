# 2976 Minimum Cost to Convert String I 转换字符串的最小成本 I

__Description:__

You are given two __0-indexed__ strings `source` and `target`, both of length `n` and consisting of __lowercase__ English letters. You are also given two __0-indexed__ character arrays `original` and `changed`, and an integer array `cost`, where `cost[i]` represents the cost of changing the character `original[i]` to the character `changed[i]`.

You start with the string `source`. In one operation, you can pick a character `x` from the string and change it to the character `y` at a cost of `z` __if__ there exists __any__ index `j` such that `cost[j] == z`, `original[j] == x`, and `changed[j] == y`.

Return _the __minimum__ cost to convert the string_ `source` _to the string_ `target` _using __any__ number of operations. If it is impossible to convert_ `source` _to_ `target`, _return_ `-1`.

__Note__ that there may exist indices `i`, `j` such that `original[j] == original[i]` and `changed[j] == changed[i]`.

__Example:__

Example 1:

```text
Input: source = "abcd", target = "acbe", original = ["a","b","c","c","e","d"], changed = ["b","c","b","e","b","e"], cost = [2,5,5,1,2,20]
Output: 28
Explanation: To convert the string "abcd" to string "acbe":
```

- Change value at index 1 from 'b' to 'c' at a cost of 5.
- Change value at index 2 from 'c' to 'e' at a cost of 1.
- Change value at index 2 from 'e' to 'b' at a cost of 2.
- Change value at index 3 from 'd' to 'e' at a cost of 20.

The total cost incurred is 5 + 1 + 2 + 20 = 28.

It can be shown that this is the minimum possible cost.

Example 2:

```text
Input: source = "aaaa", target = "bbbb", original = ["a","c"], changed = ["c","b"], cost = [1,2]
Output: 12
Explanation: To change the character 'a' to 'b' change the character 'a' to 'c' at a cost of 1, followed by changing the character 'c' to 'b' at a cost of 2, for a total cost of 1 + 2 = 3. To change all occurrences of 'a' to 'b', a total cost of 3 * 4 = 12 is incurred.
```

Example 3:

```text
Input: source = "abcd", target = "abce", original = ["a"], changed = ["e"], cost = [10000]
Output: -1
Explanation: It is impossible to convert source to target because the value at index 3 cannot be changed from 'd' to 'e'.
```

__Constraints:__

- `1 <= source.length == target.length <= 10 ^ 5`
- `source`, `target` consist of lowercase English letters.
- `1 <= cost.length == original.length == changed.length <= 2000`
- `original[i]`, `changed[i]` are lowercase English letters.
- `1 <= cost[i] <= 10 ^ 6`
- `original[i] != changed[i]`

__题目描述:__

给你两个下标从 __0__ 开始的字符串 `source` 和 `target` ，它们的长度均为 `n` 并且由 __小写__ 英文字母组成。

另给你两个下标从 __0__ 开始的字符数组 `original` 和 `changed` ，以及一个整数数组 `cost` ，其中 `cost[i]` 代表将字符 `original[i]` 更改为字符 `changed[i]` 的成本。

你从字符串 `source` 开始。在一次操作中，__如果__ 存在 __任意__ 下标 `j` 满足 `cost[j] == z` 、 `original[j] == x` 以及 `changed[j] == y` 。你就可以选择字符串中的一个字符 `x` 并以 `z` 的成本将其更改为字符 `y` 。

返回将字符串 `source` 转换为字符串 `target` 所需的 __最小__ 成本。如果不可能完成转换，则返回 `-1` 。

__注意__，可能存在下标 `i` 、 `j` 使得 `original[j] == original[i]` 且 `changed[j] == changed[i]` 。

__示例:__

示例 1：

```text
输入：source = "abcd", target = "acbe", original = ["a","b","c","c","e","d"], changed = ["b","c","b","e","b","e"], cost = [2,5,5,1,2,20]
输出：28
解释：将字符串 "abcd" 转换为字符串 "acbe" ：
```

- 更改下标 1 处的值 'b' 为 'c' ，成本为 5 。
- 更改下标 2 处的值 'c' 为 'e' ，成本为 1 。
- 更改下标 2 处的值 'e' 为 'b' ，成本为 2 。
- 更改下标 3 处的值 'd' 为 'e' ，成本为 20 。

产生的总成本是 5 + 1 + 2 + 20 = 28 。

可以证明这是可能的最小成本。

示例 2：

```text
输入：source = "aaaa", target = "bbbb", original = ["a","c"], changed = ["c","b"], cost = [1,2]
输出：12
解释：要将字符 'a' 更改为 'b'：
- 将字符 'a' 更改为 'c'，成本为 1 
- 将字符 'c' 更改为 'b'，成本为 2 
产生的总成本是 1 + 2 = 3。
将所有 'a' 更改为 'b'，产生的总成本是 3 * 4 = 12 。
```

示例 3：

```text
输入：source = "abcd", target = "abce", original = ["a"], changed = ["e"], cost = [10000]
输出：-1
解释：无法将 source 字符串转换为 target 字符串，因为下标 3 处的值无法从 'd' 更改为 'e' 。
```

__提示：__

- `1 <= source.length == target.length <= 10 ^ 5`
- `source`、 `target` 均由小写英文字母组成
- `1 <= cost.length== original.length == changed.length <= 2000`
- `original[i]`、 `changed[i]` 是小写英文字母
- `1 <= cost[i] <= 10 ^ 6`
- `original[i] != changed[i]`

__思路:__

```text
Floyd
建立一个 26 X 26 的小写字符距离映射 dis
初始化距离为 inf, 表示不可达
字符自身的距离为 0, original -> changed 的距离更新为最小的 cost
然后使用 dis 更新所有的字符之间的距离
对于字符 k
如果 dis[i][k] 可以到达
将所有字符 j 更新为 dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j])
如果 source -> target 不可达, 返回 -1
否则返回所有距离之和
时间复杂度为 O(N + M + C ^ 3), 空间复杂度为 O(C ^ 2), 其中 M 为 source/target 的长度, N 为 original/changed/cost 的长度, C 为小写字符的个数, 即 26
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minimumCost(string source, string target, vector<char>& original, vector<char>& changed, vector<int>& cost) 
    {
        int inf = 0x3f3f3f3f, m = source.size(), n = original.size();
        vector<vector<int>> dis(26, vector<int>(26, inf));
        for (int i = 0; i < 26; i++) dis[i][i] = 0;
        for (int i = 0; i < n; i++) dis[original[i] - 'a'][changed[i] - 'a'] = min(dis[original[i] - 'a'][changed[i] - 'a'], cost[i]);
        for (int k = 0; k < 26; k++) for (int i = 0; i < 26; i++) if (dis[i][k] < inf) for (int j = 0; j < 26; j++) dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);
        long result = 0L;
        for (int i = 0; i < m; i++) if (dis[source[i] - 'a'][target[i] - 'a'] == inf) return -1;
        for (int i = 0; i < m; i++) result += dis[source[i] - 'a'][target[i] - 'a'];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long minimumCost(String source, String target, char[] original, char[] changed, int[] cost) {
        int inf = 0x3f3f3f3f, dis[][] = new int[26][26], m = source.length(), n = original.length;
        for (int i = 0; i < 26; i++) Arrays.fill(dis[i], inf);
        for (int i = 0; i < 26; i++) dis[i][i] = 0;
        for (int i = 0; i < n; i++) dis[original[i] - 'a'][changed[i] - 'a'] = Math.min(dis[original[i] - 'a'][changed[i] - 'a'], cost[i]);
        for (int k = 0; k < 26; k++) for (int i = 0; i < 26; i++) if (dis[i][k] < inf) for (int j = 0; j < 26; j++) dis[i][j] = Math.min(dis[i][j], dis[i][k] + dis[k][j]);
        long result = 0L;
        for (int i = 0; i < m; i++) if (dis[source.charAt(i) - 'a'][target.charAt(i) - 'a'] == inf) return -1;
        for (int i = 0; i < m; i++) result += dis[source.charAt(i) - 'a'][target.charAt(i) - 'a'];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumCost(self, source: str, target: str, original: List[str], changed: List[str], cost: List[int]) -> int:
        dis = [[inf] * 26 for _ in range(26)]
        for i in range(26):
            dis[i][i] = 0
        for x, y, c in zip(original, changed, cost):
            dis[x][y] = min(dis[x := ord(x) - ord('a')][y := ord(y) - ord('a')], c)
        for k in range(26):
            for i in range(26):
                if dis[i][k] < inf:
                    for j in range(26):
                        dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j])
        return result if (result := sum(dis[ord(x) - ord('a')][ord(y) - ord('a')] for x, y in zip(source, target))) < inf else -1
```
