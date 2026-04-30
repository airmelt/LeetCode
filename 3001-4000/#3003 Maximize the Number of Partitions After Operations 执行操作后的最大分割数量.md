# 3003 Maximize the Number of Partitions After Operations 执行操作后的最大分割数量

__Description:__

You are given a string `s` and an integer `k`.

First, you are allowed to change __at most__ __one__ index in `s` to another lowercase English letter.

After that, do the following partitioning operation until `s` is __empty__:

- Choose the __longest__ __prefix__ of `s` containing at most `k` __distinct__ characters.
- __Delete__ the prefix from `s` and increase the number of partitions by one. The remaining characters (if any) in `s` maintain their initial order.

Return an integer denoting the maximum number of resulting partitions after the operations by optimally choosing at most one index to change.

__Example:__

Example 1:

```text
Input: s = "accca", k = 2

Output: 3

Explanation:
```

The optimal way is to change `s[2]` to something other than a and c, for example, b. then it becomes `"acbca"`.

Then we perform the operations:

Doing the operations, the string is divided into 3 partitions, so the answer is 3.

Example 2:

```text
Input: s = "aabaab", k = 3

Output: 1

Explanation:
```

Initially `s` contains 2 distinct characters, so whichever character we change, it will contain at most 3 distinct characters, so the longest prefix with at most 3 distinct characters would always be all of it, therefore the answer is 1.

Example 3:

```text
Input: s = "xxyz", k = 1

Output: 4

Explanation:
```

The optimal way is to change `s[0]` or `s[1]` to something other than characters in `s`, for example, to change `s[0]` to `w`.

Then `s` becomes `"wxyz"`, which consists of 4 distinct characters, so as `k` is 1, it will divide into 4 partitions.

__Constraints:__

- `1 <= s.length <= 10 ^ 4`
- `s` consists only of lowercase English letters.
- `1 <= k <= 26`

__题目描述:__

给定一个字符串 `s` 和一个整数 `k`。

首先，你最多可以更改 `s` 中的 __一处__ 下标对应字符为另一个小写英文字母。

之后，执行以下分割操作，直到 `s` 变为 __空串__:

- 选择 `s` 的最长 __前缀__，该前缀最多包含 `k` 个 __不同__ 字符。
- 从 `s` 中 __删除__ 这个前缀，并将分割数量加一。如果有剩余字符，它们在 `s` 中保持原来的顺序。

返回一个整数，表示在 最多 改变一处下标对应字符的情况下，经过操作后得到的最大分割数。

__示例:__

示例 1：

```text
输入：s = "accca", k = 2

输出：3

解释：
```

最好的方式是把 `s[2]` 变为除了 a 和 c 之外的东西，比如 b。然后它变成了 `"acbca"`。

然后我们执行以下操作：

进行操作时，字符串被分成 3 个部分，所以答案是 3。

示例 2：

```text
输入：s = "aabaab", k = 3

输出：1

解释：
```

一开始 `s` 包含 2 个不同的字符，所以无论我们改变哪个， 它最多包含 3 个不同字符，因此最多包含 3 个不同字符的最长前缀始终是所有字符，因此答案是 1。

示例 3：

```text
输入：s = "xxyz", k = 1

输出：4

解释：
```

最好的方式是将 `s[0]` 或 `s[1]` 变为 `s` 中字符以外的东西，例如将 `s[0]` 变为 `w`。

然后 `s` 变为 `"wxyz"`，包含 4 个不同的字符，所以当 `k` 为 1，它将分为 4 个部分。

__提示：__

- `1 <= s.length <= 10 ^ 4`
- `s` 只包含小写英文字母。
- `1 <= k <= 26`

__思路:__

```text
1. 动态规划
设 dfs(i, mask, changed) 表示当前遍历到 s[i], 此前已经加入过的字符为 mask, mask 为一个二进制, 每一位 1 表示已经有该类型的小写字符, changed 表示已经修改过字符
当 i 遍历到字符串结尾, 一定要分割出一个前缀, 返回 1
要么直接选 s[i], new_mask = mask | (1 << (s[i] - 'a'))
如果 new_mask 的 1 个个数比 k 大就需要分割出一段新的前缀, 此时 result = dfs(i + 1, (1 << (s[i] - 'a'), changed)) + 1
如果比 k 小可以继续增加前缀 result = dfs(i + 1, new_mask, changed)
如果已经修改, changed = true 就直接返回 result
否额可以选择将 s[i] 修改成别的
遍历 26 个字符, result 取最大的, 讨论方法类似之前不修改的情况
时间复杂度为 O(NK), 空间复杂度为 O(NK)
2. 前后缀分解
首先从左到右找出最长前缀分割出的段数和从右向左是一样的
所以可以使用后缀的方式找到 s[i + 1:] 能够分割成几段记为 suf_seg 并记录每段的 mask 的情况为 suf_mask, 字符种类为 suf_cur
遍历 s 记前缀为 pre_mask, 最后一段分割的段数为 pre_seg, 字符种类为 pre_cur
比较 (pre_mask | suf_mask).bit_count() 如果小于 k 说明 s[i] 不管修改成什么都可以使得段数减少 1
否则如果小于 26, 且 pre_cur = suf_cur = k, 那么将 s[i] 修改成不在前缀和后缀里的字符, 可以使得段数增加 1
其他情况段数都不会变
先从右往左遍历记录下后缀
然后从左往右遍历记录下最大的段数
当 k = 26 时无论怎么分一段都够了直接返回 1
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxPartitionsAfterOperations(string s, int k) 
    {
        int n = s.size(), bit = 0, seg = 1, mask = 0, cur = 0, result = seg;
        if (k == 26) return result;
        vector<pair<int, int>> suf(n + 1);
        auto update = [&](const auto& c) -> void
        {
            if (!(mask & (bit = 1 << (c - 'a'))))
            {
                if (++cur > k) 
                {
                    ++seg;
                    mask = bit;
                    cur = 1;
                } 
                else mask |= bit;
            }
        };
        for (int i = n - 1; i > -1; i--) 
        {
            update(s[i]);
            suf[i].first = seg;
            suf[i].second = mask;
        }
        result = seg;
        seg = 1;
        mask = cur = 0;
        for (int i = 0, j = 0, a = seg + suf[i + 1].first, b = 0; i < n; a = (++i < n) ? seg + suf[i + 1].first : a) 
        {
            if ((j = popcount((uint32_t)mask | suf[i + 1].second)) < k) --a;
            else if (j < 26 and cur == k and popcount((uint32_t)suf[i + 1].second) == k) ++a;
            result = max(result, a);
            update(s[i]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int seg = 1, mask = 0, cur = 0;

    public int maxPartitionsAfterOperations(String s, int k) {
        if (k == 26) return 1;
        int n = s.length(), suf[][] = new int[n + 1][2];
        for (int i = n - 1; i > -1; i--) {
            update(s.charAt(i), k);
            suf[i][0] = seg;
            suf[i][1] = mask;
        }
        int result = seg;
        seg = 1;
        mask = cur = 0;
        for (int i = 0, j = 0, a = seg + suf[i + 1][0], b = 0; i < n; a = (++i < n) ? seg + suf[i + 1][0] : a) {
            if ((j = Integer.bitCount(mask | suf[i + 1][1])) < k) --a;
            else if (j < 26 && cur == k && Integer.bitCount(suf[i + 1][1]) == k) ++a;
            result = Math.max(result, a);
            update(s.charAt(i), k);
        }
        return result;
    }

    private void update(char c, int k) {
        int bit = 1 << (c - 'a');
        if ((mask & bit) == 0) {
            if (++cur > k) {
                ++seg;
                mask = bit;
                cur = 1;
            } else mask |= bit;
        }
    }
}
```

__Python__:

```Python
class Solution:
    def maxPartitionsAfterOperations(self, s: str, k: int) -> int:
        @lru_cache(None)
        def dfs(i: int, mask: int, changed: bool) -> int:
            return 1 if i == len(s) else max(dfs(i + 1, bit, changed) + 1 if (new_mask := mask | (bit := 1 << (ord(s[i]) - ord('a')))).bit_count() > k else dfs(i + 1, new_mask, changed), max(dfs(i + 1, 1 << j, 1) + 1 if (new_mask := mask | (1 << j)).bit_count() > k else dfs(i + 1, new_mask, 1) for j in range(26))) if not changed else dfs(i + 1, bit, changed) + 1 if (new_mask := mask | (bit := 1 << (ord(s[i]) - ord('a')))).bit_count() > k else dfs(i + 1, new_mask, changed)
        return dfs(0, 0, 0)
```
