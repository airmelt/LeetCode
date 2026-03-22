# 87 Scramble String 扰乱字符串

__Description__:
Given a string s1, we may represent it as a binary tree by partitioning it to two non-empty substrings recursively.

Below is one possible representation of s1 = "great":

```text
    great
   /    \
  gr    eat
 / \    /  \
g   r  e   at
           / \
          a   t
```

To scramble the string, we may choose any non-leaf node and swap its two children.

For example, if we choose the node "gr" and swap its two children, it produces a scrambled string "rgeat".

```text
    rgeat
   /    \
  rg    eat
 / \    /  \
r   g  e   at
           / \
          a   t
```

We say that "rgeat" is a scrambled string of "great".

Similarly, if we continue to swap the children of nodes "eat" and "at", it produces a scrambled string "rgtae".

```text
    rgtae
   /    \
  rg    tae
 / \    /  \
r   g  ta  e
       / \
      t   a
```

We say that "rgtae" is a scrambled string of "great".

Given two strings s1 and s2 of the same length, determine if s2 is a scrambled string of s1.

__Example:__

Example 1:

Input: s1 = "great", s2 = "rgeat"
Output: true

Example 2:

Input: s1 = "abcde", s2 = "caebd"
Output: false

__题目描述__:
给定一个字符串 s1，我们可以把它递归地分割成两个非空子字符串，从而将其表示为二叉树。

下图是字符串 s1 = "great" 的一种可能的表示形式。

```text
    great
   /    \
  gr    eat
 / \    /  \
g   r  e   at
           / \
          a   t
```

在扰乱这个字符串的过程中，我们可以挑选任何一个非叶节点，然后交换它的两个子节点。

例如，如果我们挑选非叶节点 "gr" ，交换它的两个子节点，将会产生扰乱字符串 "rgeat" 。

```text
    rgeat
   /    \
  rg    eat
 / \    /  \
r   g  e   at
           / \
          a   t
```

我们将 "rgeat” 称作 "great" 的一个扰乱字符串。

同样地，如果我们继续交换节点 "eat" 和 "at" 的子节点，将会产生另一个新的扰乱字符串 "rgtae" 。

```text
    rgtae
   /    \
  rg    tae
 / \    /  \
r   g  ta  e
       / \
      t   a
```

我们将 "rgtae” 称作 "great" 的一个扰乱字符串。

给出两个长度相等的字符串 s1 和 s2，判断 s2 是否是 s1 的扰乱字符串。

__示例 :__

示例 1:

输入: s1 = "great", s2 = "rgeat"
输出: true

示例 2:

输入: s1 = "abcde", s2 = "caebd"
输出: false

__思路__:

1. 递归法
题目中已经给了 s1和 s2长度必然相等
如果两字符串相等, 直接返回 true
否则映射到 ascii码中, 如果字符不相等直接返回 false, 两者一定不是扰乱字符串
然后递归判断 s1[:i]、 s2[:i]和 s1[i:]、s2[i:]相等或者是 s1[:i]、s2[n - i:n]和 s1[i:]、s2[:i]相等, 有一组相等证明两个字符串是扰乱字符串
时间复杂度O(n ^ 4), 空间复杂度O(1), 不考虑递归栈空间
2. 动态规划
dp[i][j][k]表示 s1[i:i + k]是否可以通过变换变化成 s2[j:j + k]
初始状态为 dp[i][j][1] = (s1[i] == s2[j]), 即如果对应位置的字符相等, 即 s1[i]可以通过变换变化成 s2[j], 因为两者相等不需要进行交换
动态转移方程为 dp[i][j][k] = (dp[i][j][1 ~ k-1] && dp[i + 1 ~ k - 1][j + 1 ~ k - 1][k - 1 ~ k - 1]) || (dp[i + 1 ~ k - 1][j][1 ~ k-1] && dp[i][j + 1 ~ k - 1][k - 1 ~ k - 1]), 这里表示从某一个地方分割开两个字符串要对应能够变换, 即s1[:i]、 s2[:i]和 s1[i:]、s2[i:]相等或者是 s1[:i]、s2[n - i:n]和 s1[i:]、s2[:i]相等
时间复杂度O(n ^ 4), 空间复杂度O(n ^ 3)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isScramble(string s1, string s2) 
    {
        return helper(s1, s2);
    }
private:
    bool helper(const string& s1, const string& s2)
    {
        if (s1 == s2) return true;
        int n = s1.size(), count[128]{0};
        for (auto &c : s1) ++count[c];
        for (auto &c : s2) --count[c];
        for (auto &i : count) if (i != 0) return false;
        for (int i = 1; i < n; i++) if (helper(s1.substr(0, i), s2.substr(0, i)) and helper(s1.substr(i, n - i), s2.substr(i, n - i)) or helper(s1.substr(0, i), s2.substr(n - i, i)) and helper(s1.substr(i, n - i), s2.substr(0, n - i))) return true;
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isScramble(String s1, String s2) {
        int n = s1.length();
        boolean dp[][][] = new boolean[n][n][n + 1];
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) dp[i][j][1] = (s1.charAt(i) == s2.charAt(j));
        for (int len = 2; len <= n; len++) {
            for (int i = 0; i <= n - len; i++) {
                for (int j = 0; j <= n - len; j++) {
                    for (int k = 1; k <= len - 1; k++) {
                        if (dp[i][j][k] && dp[i + k][j + k][len - k]) {
                            dp[i][j][len] = true;
                            break;
                        }
                        if (dp[i][j + len - k][k] && dp[i + k][j][len - k]) {
                            dp[i][j][len] = true;
                            break;
                        }
                    }
                }
            }
        }
        return dp[0][0][n];
    }
}
```

__Python__:

```Python
class Solution:
    def isScramble(self, s1: str, s2: str) -> bool:
        if s1 == s2:
            return True
        if sorted(s1) != sorted(s2):
            return False
        for i in range(1, len(s1)):
            if (self.isScramble(s1[:i], s2[:i]) and self.isScramble(s1[i:], s2[i:])) or (self.isScramble(s1[:i], s2[-i:]) and self.isScramble(s1[i:], s2[:-i])):
                return True
        return False
```
