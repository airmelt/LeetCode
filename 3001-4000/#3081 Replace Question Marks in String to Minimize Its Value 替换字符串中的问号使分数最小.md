# 3081 Replace Question Marks in String to Minimize Its Value 替换字符串中的问号使分数最小

__Description:__

You are given a string `s`. `s[i]` is either a lowercase English letter or `'?'`.

For a string `t` having length `m` containing __only__ lowercase English letters, we define the function `cost(i)` for an index `i` as the number of characters __equal__ to `t[i]` that appeared before it, i.e. in the range `[0, i - 1]`.

The __value__ of `t` is the __sum__ of `cost(i)` for all indices `i`.

For example, for the string `t = "aab"`:

- `cost(0) = 0`
- `cost(1) = 1`
- `cost(2) = 0`
- Hence, the value of `"aab"` is `0 + 1 + 0 = 1`.

Your task is to __replace all__ occurrences of `'?'` in `s` with any lowercase English letter so that the __value__ of `s` is __minimized__.

Return _a string denoting the modified string with replaced occurrences of_ `'?'`_. If there are multiple strings resulting in the __minimum value__, return the lexicographically smallest one._

__Example:__

Example 1:

```text
Input: s = "???"

Output: "abc"
```

__Explanation:__ In this example, we can replace the occurrences of `'?'` to make `s` equal to `"abc"`.

For `"abc"`, `cost(0) = 0`, `cost(1) = 0`, and `cost(2) = 0`.

The value of `"abc"` is `0`.

Some other modifications of `s` that have a value of `0` are `"cba"`, `"abz"`, and, `"hey"`.

Among all of them, we choose the lexicographically smallest.

Example 2:

```text
Input: s = "a?a?"

Output: "abac"
```

__Explanation:__ In this example, the occurrences of `'?'` can be replaced to make `s` equal to `"abac"`.

For `"abac"`, `cost(0) = 0`, `cost(1) = 0`, `cost(2) = 1`, and `cost(3) = 0`.

The value of `"abac"` is `1`.

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s[i]` is either a lowercase English letter or `'?'`.

__题目描述:__

给你一个字符串 `s` 。 `s[i]` 要么是小写英文字母，要么是问号 `'?'` 。

对于长度为 `m` 且 __只__ 含有小写英文字母的字符串 `t` ，我们定义函数 `cost(i)` 为下标 `i` 之前（也就是范围 `[0, i - 1]` 中）出现过与 `t[i]` __相同__ 字符出现的次数。

字符串 `t` 的 __分数__ 为所有下标 `i` 的 `cost(i)` 之 __和__ 。

比方说，字符串 `t = "aab"` :

- `cost(0) = 0`
- `cost(1) = 1`
- `cost(2) = 0`
- 所以，字符串 `"aab"` 的分数为 `0 + 1 + 0 = 1` 。

你的任务是用小写英文字母 __替换__ `s` 中 __所有__ 问号，使 `s` 的 __分数____最小__ 。

请你返回替换所有问号 `'?'` 之后且分数最小的字符串。如果有多个字符串的 __分数最小__ ，那么返回字典序最小的一个。

__示例:__

示例 1：

```text
输入：s = "???"

输出： "abc"
```

__解释:__ 这个例子中，我们将 `s` 中的问号 `'?'` 替换得到 `"abc"` 。

对于字符串 `"abc"` ， `cost(0) = 0` ， `cost(1) = 0` 和 `cost(2) = 0` 。

`"abc"` 的分数为 `0` 。

其他修改 `s` 得到分数 `0` 的字符串为 `"cba"` ， `"abz"` 和 `"hey"` 。

这些字符串中，我们返回字典序最小的。

示例 2：

```text
输入： s = "a?a?"

输出： "abac"
```

__解释:__ 这个例子中，我们将 `s` 中的问号 `'?'` 替换得到 `"abac"` 。

对于字符串 `"abac"` ， `cost(0) = 0` ， `cost(1) = 0` ， `cost(2) = 1` 和 `cost(3) = 0` 。

`"abac"` 的分数为 `1` 。

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s[i]` 要么是小写英文字母，要么是 `'?'` 。

__思路:__

```text
1. 优先队列
将字符串中所有元素加入优先队列
每次弹出队首
修改为最小的字符再加入优先队列
直到加完所有字符
时间复杂度为 O(NlogN + C), 空间复杂度为 O(C), C 为字符种类, 本题为 26
2. 贪心
先统计每一个字符出现的次数并从小到大排序
找到一个值 threshold 使得之前所有的次数都能改成 threshold
剩下的值再均分到字典序更小的字符上
为了减少边界讨论, 可以添加一个无法到达的哨兵在次数数组的末尾
时间复杂度为 O(N + ClogC), 空间复杂度为 O(C), C 为字符种类, 本题为 26
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string minimizeStringValue(string s) 
    {
        vector<int> freq(27, 0);
        int inf = 1234567, mark = count(s.begin(), s.end(), '?'), cur = 0, limit = 0, extra = 0, j = 0, n = s.size();
        freq.back() = inf;
        for (const auto& c : s) if (c != '?') ++freq[c - 'a'];
        auto f = freq, target = freq;
        sort(f.begin(), f.end());
        for (int i = 1; ; i++)
        {
            if (mark <= (cur = i * (f[i] - f[i - 1])))
            {
                limit = f[i - 1] + mark / i;
                extra = mark % i;
                break;
            }
            mark -= cur;
        }
        for (int i = 0; i < 26; i++)
        {
            if (target[i] <= limit)
            {
                target[i] = limit;
                if (extra)
                {
                    ++target[i];
                    --extra;
                }
            }
        }
        for (int i = 0; i < n; i++)
        {
            if (s[i] == '?')
            {
                while (freq[j] == target[j]) ++j;
                ++freq[j];
                s[i] = (char)('a' + j);
            }
        }
        return s;
    }
};
```

__Java__:

```Java
class Solution {
    public String minimizeStringValue(String s) {
        int freq[] = new int[27], inf = 0x3f3f3f3f, mark = 0, limit = 0, extra = 0, cur = 0, j = 0, n = s.length();
        freq[26] = inf;
        for (var c : s.toCharArray()) {
            if (c != '?') ++freq[c - 'a'];
            else ++mark;
        }
        int[] f = freq.clone(), target = freq.clone();
        Arrays.sort(f);
        for (int i = 1; ; i++) {
            if (mark <= (cur = i * (f[i] - f[i - 1]))) {
                limit = f[i - 1] + mark / i;
                extra = mark % i;
                break;
            }
            mark -= cur;
        }
        for (int i = 0; i < 26; i++) {
            if (freq[i] <= limit) {
                target[i] = limit;
                if (extra > 0) {
                    --extra;
                    ++target[i];
                }
            }
        }
        var sb = new StringBuilder(s);
        for (int i = 0; i < n; i++) {
            if (sb.charAt(i) == '?') {
                while (freq[j] == target[j]) ++j;
                ++freq[j];
                sb.setCharAt(i, (char)('a' + j));
            }
        }
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def minimizeStringValue(self, s: str) -> str:
        freq, mark, cur, result, j = Counter(s), s.count('?'), [], list(s), 0
        heapify(heap := [(freq[c], c) for c in ascii_lowercase])
        for _ in range(mark):
            f, c = heap[0]
            cur.append(c)
            heapreplace(heap, (f + 1, c))
        cur.sort()
        for i, c in enumerate(s):
            if c == '?':
                result[i] = cur[j]
                j += 1
        return ''.join(result)
```
