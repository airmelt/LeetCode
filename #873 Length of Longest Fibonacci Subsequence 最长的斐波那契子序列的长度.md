# 873 Length of Longest Fibonacci Subsequence 最长的斐波那契子序列的长度

__Description__:
A sequence x1, x2, ..., xn is Fibonacci-like if:

n >= 3
xi + xi+1 == xi+2 for all i + 2 <= n
Given a strictly increasing array arr of positive integers forming a sequence, return the length of the longest Fibonacci-like subsequence of arr. If one does not exist, return 0.

A subsequence is derived from another sequence arr by deleting any number of elements (including none) from arr, without changing the order of the remaining elements. For example, [3, 5, 8] is a subsequence of [3, 4, 5, 6, 7, 8].

__Example:__

Example 1:

Input: arr = [1,2,3,4,5,6,7,8]
Output: 5
Explanation: The longest subsequence that is fibonacci-like: [1,2,3,5,8].

Example 2:

Input: arr = [1,3,7,11,12,14,18]
Output: 3
Explanation: The longest subsequence that is fibonacci-like: [1,11,12], [3,11,14] or [7,11,18].

__Constraints:__

3 <= arr.length <= 1000
1 <= arr[i] < arr[i + 1] <= 10^9

__题目描述__:
如果序列 X_1, X_2, ..., X_n 满足下列条件，就说它是 斐波那契式 的：

n >= 3
对于所有 i + 2 <= n，都有 X_i + X_{i+1} = X_{i+2}
给定一个严格递增的正整数数组形成序列 arr ，找到 arr 中最长的斐波那契式的子序列的长度。如果一个不存在，返回  0 。

（回想一下，子序列是从原序列 arr 中派生出来的，它从 arr 中删掉任意数量的元素（也可以不删），而不改变其余元素的顺序。例如， [3, 5, 8] 是 [3, 4, 5, 6, 7, 8] 的一个子序列）

__示例 :__

示例 1：

输入: arr = [1,2,3,4,5,6,7,8]
输出: 5
解释: 最长的斐波那契式子序列为 [1,2,3,5,8] 。

示例 2：

输入: arr = [1,3,7,11,12,14,18]
输出: 3
解释: 最长的斐波那契式子序列有 [1,11,12]、[3,11,14] 以及 [7,11,18] 。

__提示:__

3 <= arr.length <= 1000
1 <= arr[i] < arr[i + 1] <= 10^9

__思路__:

动态规划
dp 记录以 arr[i], arr[j] 结尾的最长斐波拉契子数列的长度
m 记录下标对应的值, 方便快速查找
如果油用完了还不能到达 target 返回 -1
时间复杂度为 O(n ^ 2), 空间复杂度为 O(nlgm), 其中 m = max(arr)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int lenLongestFibSubseq(vector<int>& arr) 
    {
        int n = arr.size(), result = 0;
        unordered_map<int, int> m, dp;
        for (int i = 0; i < n; i++) m[arr[i]] = i;
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < i; j++) 
            {
                if (arr[i] - arr[j] < arr[j] and m.count(arr[i] - arr[j])) 
                {
                    int k = m[arr[i] - arr[j]];
                    dp[j * n + i] = dp[k * n + j] + 1;
                    result = max(result, dp[j * n + i] + 2);
                }
            }
        }
        return result > 2 ? result : 0;
    }
};
```

__Java__:

```Java
class Solution {
    public int lenLongestFibSubseq(int[] arr) {
        int n = arr.length, result = 0;
        Map<Integer, Integer> m = new HashMap<>(), dp = new HashMap<>();
        for (int i = 0; i < n; i++) m.put(arr[i], i);
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (arr[i] - arr[j] < arr[j] && m.containsKey(arr[i] - arr[j])) {
                    int k = m.get(arr[i] - arr[j]);
                    dp.put(j * n + i, dp.getOrDefault(k * n + j, 0) + 1);
                    result = Math.max(result, dp.getOrDefault(j * n + i, 0) + 2);
                }
            }
        }
        return result > 2 ? result : 0;
    }
}
```

__Python__:

```Python
class Solution:
    def lenLongestFibSubseq(self, arr: List[int]) -> int:
        n, result, m, dp = len(arr), 0, { v: i for i, v in enumerate(arr) }, defaultdict(int)
        for i in range(n):
            for j in range(i):
                if (cur := arr[i] - arr[j]) < arr[j] and cur in m:
                    dp[j * n + i], result = dp[m[cur] * n + j] + 1, max(result, dp[m[cur] * n + j] + 3)
        return result if result > 2 else 0
```
