# 466 Count The Repetitions 统计重复个数

__Description__:
Define S = [s,n] as the string S which consists of n connected strings s. For example, ["abc", 3] ="abcabcabc".

On the other hand, we define that string s1 can be obtained from string s2 if we can remove some characters from s2 such that it becomes s1. For example, “abc” can be obtained from “abdbec” based on our definition, but it can not be obtained from “acbbe”.

You are given two non-empty strings s1 and s2 (each at most 100 characters long) and two integers 0 ≤ n1 ≤ 10^6 and 1 ≤ n2 ≤ 10^6. Now consider the strings S1 and S2, where S1=[s1,n1] and S2=[s2,n2]. Find the maximum integer M such that [S2,M] can be obtained from S1.

__Example:__

Input:
s1="acb", n1=4
s2="ab", n2=2

Return:
2

__题目描述__:
由 n 个连接的字符串 s 组成字符串 S，记作 S = [s,n]。例如，["abc",3]=“abcabcabc”。

如果我们可以从 s2 中删除某些字符使其变为 s1，则称字符串 s1 可以从字符串 s2 获得。例如，根据定义，"abc" 可以从 “abdbec” 获得，但不能从 “acbbe” 获得。

现在给你两个非空字符串 s1 和 s2（每个最多 100 个字符长）和两个整数 0 ≤ n1 ≤ 10^6 和 1 ≤ n2 ≤ 10^6。现在考虑字符串 S1 和 S2，其中 S1=[s1,n1] 、S2=[s2,n2] 。

请你找出一个可以满足使[S2,M] 从 S1 获得的最大整数 M 。

__示例 :__

输入：
s1 ="acb",n1 = 4
s2 ="ab",n2 = 2

返回：
2

__思路__:

动态规划
题目的意思是, S = [s, n]表示字符串 S = s \* n, 比如 S = ["abc", 2] -> S = "abcabc"
求 S2作为子序列在 S1中出现的次数
首先如果已知 S1和 S2, 那么遍历 S1, 对 S2进行遍历, 每次遍历到结尾的时候就重置到 S2开头, 就可以得到 S2在 S1中的出现次数
如果 s2都在 s1中, 那么 s1重复几次之后, 必然可以在 s1中找到一个 s2子序列
将 s2顺序移动看成环, 比如 abc -> bca -> cba, 分别计算
dp数组记录 s2[i]在 s1中出现的下标和 s2在 s1组成的循环中出现的次数
这样每次循环 s1, 就能找到 s2开头对应的 s1的位置和 s2的出现次数
最终得到的是 s2的出现次数, 要得到 S2, 只需要除以 s2的长度即可
时间复杂度O(mn * n1), 空间复杂度O(n), mn分别为 s1和 s2的长度, n1为题目所给 s1的重复次数

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int getMaxRepetitions(string s1, int n1, string s2, int n2) 
    {
        int m = s1.size(), n = s2.size(), dp[s2.size()][2], result = 0, cur = 0;
        for (int i = 0; i < n; i++) 
        {
            int start = i, count = 0;
            for (int j = 0; j < m; j++) 
            {
                if (s1[j] == s2[start]) 
                {
                    ++start;
                    if (start == n) 
                    {
                        start = 0;
                        ++count;
                    }
                }
            }
            dp[i][0] = start;
            dp[i][1] = count;
        }
        for (int i = 0; i < n1; i++) 
        {
            result += dp[cur][1];
            cur = dp[cur][0];
        }
        return result / n2;
    }
};
```

__Java__:

```Java
class Solution {
    public int getMaxRepetitions(String s1, int n1, String s2, int n2) {
        int m = s1.length(), n = s2.length(), dp[][] = new int[s2.length()][2], result = 0, cur = 0;
        for (int i = 0; i < n; i++) {
            int start = i, count = 0;
            for (int j = 0; j < m; j++) {
                if (s1.charAt(j) == s2.charAt(start)) {
                    ++start;
                    if (start == n) {
                        start = 0;
                        ++count;
                        }
                }
            }
            dp[i][0] = start;
            dp[i][1] = count;
        }
        for (int i = 0; i < n1; i++) {
            result += dp[cur][1];
            cur = dp[cur][0];
        }
        return result / n2;
    }
}
```

__Python__:

```Python
class Solution:
    def getMaxRepetitions(self, s1: str, n1: int, s2: str, n2: int) -> int:
        dp = []
        for i in range(len(s2)):
            start, count = i, 0
            for j in range(len(s1)):
                if s1[j] == s2[start]:
                    start += 1
                    if start == len(s2):
                        start = 0
                        count += 1
            dp.append((start, count))
        result, cur = 0, 0
        for _ in range(n1):
            result += dp[cur][1]
            cur = dp[cur][0]
        return result // n2
```
