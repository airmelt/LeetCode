# 2157 Groups of Strings 字符串分组

__Description:__

You are given a __0-indexed__ array of strings `words`. Each string consists of __lowercase English letters__ only. No letter occurs more than once in any string of `words`.

Two strings `s1` and `s2` are said to be __connected__ if the set of letters of `s2` can be obtained from the set of letters of `s1` by any __one__ of the following operations:

- Adding exactly one letter to the set of the letters of `s1`.
- Deleting exactly one letter from the set of the letters of `s1`.
- Replacing exactly one letter from the set of the letters of `s1` with any letter, __including__ itself.

The array `words` can be divided into one or more non-intersecting __groups__. A string belongs to a group if any __one__ of the following is true:

- It is connected to __at least one__ other string of the group.
- It is the __only__ string present in the group.

Note that the strings in `words` should be grouped in such a manner that a string belonging to a group cannot be connected to a string present in any other group. It can be proved that such an arrangement is always unique.

Return _an array_ `ans` _of size_ `2` _where:_

- `ans[0]` _is the __maximum number__ of groups_ `words` _can be divided into, and_
- `ans[1]` _is the __size of the largest__ group_.

__Example:__

Example 1:

```text
Input: words = ["a","b","ab","cde"]
Output: [2,3]
Explanation:
- words[0] can be used to obtain words[1] (by replacing 'a' with 'b'), and words[2] (by adding 'b'). So words[0] is connected to words[1] and words[2].
- words[1] can be used to obtain words[0] (by replacing 'b' with 'a'), and words[2] (by adding 'a'). So words[1] is connected to words[0] and words[2].
- words[2] can be used to obtain words[0] (by deleting 'b'), and words[1] (by deleting 'a'). So words[2] is connected to words[0] and words[1].
- words[3] is not connected to any string in words.
Thus, words can be divided into 2 groups ["a","b","ab"] and ["cde"]. The size of the largest group is 3.
```

Example 2:

```text
Input: words = ["a","ab","abc"]
Output: [1,3]
Explanation:
- words[0] is connected to words[1].
- words[1] is connected to words[0] and words[2].
- words[2] is connected to words[1].
Since all strings are connected to each other, they should be grouped together.
Thus, the size of the largest group is 3.
```

__Constraints:__

- `1 <= words.length <= 2 * 10 ^ 4`
- `1 <= words[i].length <= 26`
- `words[i]` consists of lowercase English letters only.
- No letter occurs more than once in `words[i]`.

__题目描述:__

给你一个下标从 __0__ 开始的字符串数组 `words` 。每个字符串都只包含 __小写英文字母__ 。 `words` 中任意一个子串中，每个字母都至多只出现一次。

如果通过以下操作之一，我们可以从 `s1` 的字母集合得到 `s2` 的字母集合，那么我们称这两个字符串为 __关联的__ :

- 往 `s1` 的字母集合中添加一个字母。
- 从 `s1` 的字母集合中删去一个字母。
- 将 `s1` 中的一个字母替换成另外任意一个字母（也可以替换为这个字母本身）。

数组 `words` 可以分为一个或者多个无交集的 __组__ 。如果一个字符串与另一个字符串关联，那么它们应当属于同一个组。

注意，你需要确保分好组后，一个组内的任一字符串与其他组的字符串都不关联。可以证明在这个条件下，分组方案是唯一的。

请你返回一个长度为 `2` 的数组 `ans` :

- `ans[0]` 是 `words` 分组后的 __总组数__ 。
- `ans[1]` 是字符串数目最多的组所包含的字符串数目。

__示例:__

示例 1：

```text
输入：words = ["a","b","ab","cde"]
输出：[2,3]
解释：
- words[0] 可以得到 words[1] （将 'a' 替换为 'b'）和 words[2] （添加 'b'）。所以 words[0] 与 words[1] 和 words[2] 关联。
- words[1] 可以得到 words[0] （将 'b' 替换为 'a'）和 words[2] （添加 'a'）。所以 words[1] 与 words[0] 和 words[2] 关联。
- words[2] 可以得到 words[0] （删去 'b'）和 words[1] （删去 'a'）。所以 words[2] 与 words[0] 和 words[1] 关联。
- words[3] 与 words 中其他字符串都不关联。
所以，words 可以分成 2 个组 ["a","b","ab"] 和 ["cde"] 。最大的组大小为 3 。
```

示例 2：

```text
输入：words = ["a","ab","abc"]
输出：[1,3]
解释：
- words[0] 与 words[1] 关联。
- words[1] 与 words[0] 和 words[2] 关联。
- words[2] 与 words[1] 关联。
由于所有字符串与其他字符串都关联，所以它们全部在同一个组内。
所以最大的组大小为 3 。
```

__提示：__

- `1 <= words.length <= 2 * 10 ^ 4`
- `1 <= words[i].length <= 26`
- `words[i]` 只包含小写英文字母。
- `words[i]` 中每个字母最多只出现一次。

__思路:__

```text

时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> groupStrings(vector<string>& words) 
    {
        group = words.size();
        max_size = 0;
        for (const auto& word : words)
        {
            int x = 0;
            for (const auto& c : word) x |= 1 << ((c & 31) - 1);
            parent[x] = x;
            ++weight[x];
            max_size = max(weight[x], max_size);
            group -= weight[x] > 1;
        }
        for (const auto& [x, v] : parent)
        {
            
        }
    }
private:
    unordered_map<int, int> parent, weight;
    int group, max_size;

    int find(int x)
    {
        return parent[x] == x ? x : (parent[x] = find(parent[x]));
    }

    void merge(int x, int y)
    {
        if (parent.count(y))
        {
            if ((x = find(x)) != (y = find(y)))
            {
                parent[x] = y;
                weight[y] += weight[x];
                max_size = max(weight[y], max_size);
                --group;
            }
        }
    }
};
```

__Java__:

```Java
class Solution {
    private Map<Integer, Integer> parent = new HashMap<>(), weight = new HashMap<>();
    private int group = 0, maxSize = 0;

    public int[] groupStrings(String[] words) {
        group = words.length;
        for (String word : words) {
            int x = 0;
            for (char c : word.toCharArray()) x |= 1 << ((c & 31) - 1);
            parent.put(x, x);
            weight.merge(x, 1, Integer::sum);
            maxSize = Math.max(maxSize, weight.get(x));
            if (weight.get(x) > 1) --group;
        }
        parent.forEach((x, _) -> {
            for (int i = 0; i < 26; i++) {
                merge(x, x ^ (1 << i));
                if (((x >> i) & 1) == 1) for (int j = 0; j < 26; j++) if (((x >> j) & 1) == 0) merge(x, x ^ (1 << i) | (1 << j));
            }
        });
        return new int[]{ group, maxSize };
    }

    private int find(int x) {
        if (parent.get(x) != x) parent.put(x, find(parent.get(x)));
        return parent.get(x);
    }

    private void merge(int x, int y) {
        if (parent.containsKey(y)) {
            if ((x = find(x)) != (y = find(y))) {
                parent.put(x, y);
                weight.merge(y, weight.get(x), Integer::sum);
                maxSize = Math.max(maxSize, weight.get(y));
                --group;
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def groupStrings(self, words: List[str]) -> List[int]:
        parent, weight, group, max_size = {}, defaultdict(int), len(words), 0

        def find(x: int) -> int:
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]

        def merge(x: int, y: int) -> NoReturn:
            nonlocal group, max_size
            if y in parent:
                if (x := find(x)) != (y := find(y)):
                    parent[x] = y
                    weight[y] += weight[x]
                    max_size = max(max_size, weight[y])
                    group -= 1

        for word in words:
            x = 0
            for c in word:
                x |= 1 << ((ord(c) & 31) - 1)
            parent[x] = x
            weight[x] += 1
            max_size = max(max_size, weight[x])
            group -= weight[x] > 1
        for x in parent:
            for i in range(26):
                merge(x, x ^ (1 << i))
                if (x >> i) & 1:
                    for j in range(26):
                        if not ((x >> j) & 1):
                            merge(x, x ^ (1 << i) | (1 << j))
        return [group, max_size]
```
