# 2156 Find Substring With Given Hash Value 查找给定哈希值的子串

__Description:__

The hash of a __0-indexed__ string `s` of length `k`, given integers `p` and `m`, is computed using the following function:

- `hash(s, p, m) = (val(s[0]) * p ^ 0 + val(s[1]) * p ^ 1 + ... + val(s[k-1]) * p ^ k-1) mod m`.

Where `val(s[i])` represents the index of `s[i]` in the alphabet from `val('a') = 1` to `val('z') = 26`.

You are given a string `s` and the integers `power`, `modulo`, `k`, and `hashValue.` Return `sub`, _the __first__ __substring__ of_ `s` _of length_ `k` _such that_ `hash(sub, power, modulo) == hashValue`.

The test cases will be generated such that an answer always exists.

A substring is a contiguous non-empty sequence of characters within a string.

__Example:__

Example 1:

```text
Input: s = "leetcode", power = 7, modulo = 20, k = 2, hashValue = 0
Output: "ee"
Explanation: The hash of "ee" can be computed to be hash("ee", 7, 20) = (5 * 1 + 5 * 7) mod 20 = 40 mod 20 = 0. 
"ee" is the first substring of length 2 with hashValue 0. Hence, we return "ee".
```

Example 2:

```text
Input: s = "fbxzaad", power = 31, modulo = 100, k = 3, hashValue = 32
Output: "fbx"
Explanation: The hash of "fbx" can be computed to be hash("fbx", 31, 100) = (6 * 1 + 2 * 31 + 24 * 312) mod 100 = 23132 mod 100 = 32. 
The hash of "bxz" can be computed to be hash("bxz", 31, 100) = (2 * 1 + 24 * 31 + 26 * 312) mod 100 = 25732 mod 100 = 32. 
"fbx" is the first substring of length 3 with hashValue 32. Hence, we return "fbx".
Note that "bxz" also has a hash of 32 but it appears later than "fbx".
```

__Constraints:__

- `1 <= k <= s.length <= 2 * 10 ^ 4`
- `1 <= power, modulo <= 10 ^ 9`
- `0 <= hashValue < modulo`
- `s` consists of lowercase English letters only.
- The test cases are generated such that an answer always __exists__.

__题目描述:__

给定整数 `p` 和 `m` ，一个长度为 `k` 且下标从 __0__ 开始的字符串 `s` 的哈希值按照如下函数计算:

- `hash(s, p, m) = (val(s[0]) * p ^ 0 + val(s[1]) * p ^ 1 + ... + val(s[k-1]) * p ^ k-1) mod m`.

其中 `val(s[i])` 表示 `s[i]` 在字母表中的下标，从 `val('a') = 1` 到 `val('z') = 26` 。

给你一个字符串 `s` 和整数 `power`， `modulo`， `k` 和 `hashValue` 。请你返回 `s` 中 __第一个__ 长度为 `k` 的 __子串__ `sub` ，满足 `hash(sub, power, modulo) == hashValue` 。

测试数据保证一定 存在 至少一个这样的子串。

子串 定义为一个字符串中连续非空字符组成的序列。

__示例:__

示例 1：

```text
输入：s = "leetcode", power = 7, modulo = 20, k = 2, hashValue = 0
输出："ee"
解释："ee" 的哈希值为 hash("ee", 7, 20) = (5 * 1 + 5 * 7) mod 20 = 40 mod 20 = 0 。
"ee" 是长度为 2 的第一个哈希值为 0 的子串，所以我们返回 "ee" 。
```

示例 2：

```text
输入：s = "fbxzaad", power = 31, modulo = 100, k = 3, hashValue = 32
输出："fbx"
解释："fbx" 的哈希值为 hash("fbx", 31, 100) = (6 * 1 + 2 * 31 + 24 * 312) mod 100 = 23132 mod 100 = 32 。
"bxz" 的哈希值为 hash("bxz", 31, 100) = (2 * 1 + 24 * 31 + 26 * 312) mod 100 = 25732 mod 100 = 32 。
"fbx" 是长度为 3 的第一个哈希值为 32 的子串，所以我们返回 "fbx" 。
注意，"bxz" 的哈希值也为 32 ，但是它在字符串中比 "fbx" 更晚出现。
```

__提示：__

- `1 <= k <= s.length <= 2 * 10 ^ 4`
- `1 <= power, modulo <= 10 ^ 9`
- `0 <= hashValue < modulo`
- `s` 只包含小写英文字母。
- 测试数据保证一定 __存在__ 满足条件的子串。

__思路:__

```text
模拟
由于和传统计算哈希值的方式不同, 需要倒序计算
首先计算最后 k 个字符的哈希值
使用秦九韶算法计算哈希值
hash = (hash * power + (s[i] & 31)) % modulo
s[i] & 31 表示 s[i] 取后五位, 例如 'a' = 0b00110001, 取后五位为 1
计算 pk = power ^ k % modulo, 方便后续处理 hash 值
再次遍历, 从 n - k - 1 开始, 计算 hash 值
hash = ((hash * power) + (s[i] & 31) - pk * (s[i + k] & 31) % modulo + modulo) % modulo
如果 hash 值等于 hashValue, 更新结果
加 modulo 是为了避免负数
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string subStrHash(string s, int power, int modulo, int k, int hashValue) 
    {
        long long hash = 0LL, pk = 1LL;
        int n = s.size(), result = -1;
        for (int i = n - 1; i >= n - k; i--)
        {
            hash = (hash * power + (s[i] & 31)) % modulo;
            pk = pk * power % modulo;
        }   
        for (int i = n - k - 1; i > -1; i--) if ((hash = ((hash * power) + (s[i] & 31) - pk * (s[i + k] & 31) % modulo + modulo) % modulo) == hashValue) result = i;
        return s.substr(result == -1 ? n - k : result, k);
    }
};
```

__Java__:

```Java
class Solution {
    public String subStrHash(String s, int power, int modulo, int k, int hashValue) {
        int n = s.length(), result = -1;
        long pk = 1L, hash = 0L;
        for (int i = n - 1; i >= n - k; i--) {
            hash = (hash * power + (s.charAt(i) & 31)) % modulo;
            pk = pk * power % modulo;
        }
        for (int i = n - k - 1; i > -1; i--) if ((hash = (hash * power + (s.charAt(i) & 31) - pk * (s.charAt(i + k) & 31) % modulo + modulo) % modulo) == hashValue) result = i;
        return result == -1 ? s.substring(n - k, n) : s.substring(result, result + k);
    }
}
```

__Python__:

```Python
class Solution:
    def subStrHash(self, s: str, power: int, mod: int, k: int, hashValue: int) -> str:
        n, hash, result, pk = len(s), 0, -1, pow(power, k, mod)
        for i in range(n - 1, n - k - 1, -1):
            hash = (hash * power + (ord(s[i]) & 31)) % mod
        for i in range(n - k - 1, -1, -1):
            if (hash := (hash * power + (ord(s[i]) & 31) - pk * (ord(s[i + k]) & 31)) % mod) == hashValue:
                result = i
        return s[result:result + k] if result != -1 else s[-k:]
```
