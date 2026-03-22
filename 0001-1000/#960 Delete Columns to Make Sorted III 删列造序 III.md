# 960 Delete Columns to Make Sorted III 删列造序 III

__Description__:
You are given an array of n strings strs, all of the same length.

We may choose any deletion indices, and we delete all the characters in those indices for each string.

For example, if we have strs = ["abcdef","uvwxyz"] and deletion indices {0, 2, 3}, then the final array after deletions is ["bef", "vyz"].

Suppose we chose a set of deletion indices answer such that after deletions, the final array has every string (row) in lexicographic order. (i.e., (strs[0][0] <= strs[0][1] <= ... <= strs[0][strs[0].length - 1]), and (strs[1][0] <= strs[1][1] <= ... <= strs[1][strs[1].length - 1]), and so on). Return the minimum possible value of answer.length.

__Example:__

Example 1:

Input: strs = ["babca","bbazb"]
Output: 3
Explanation: After deleting columns 0, 1, and 4, the final array is strs = ["bc", "az"].
Both these rows are individually in lexicographic order (ie. strs[0][0] <= strs[0][1] and strs[1][0] <= strs[1][1]).
Note that strs[0] > strs[1] - the array strs is not necessarily in lexicographic order.

Example 2:

Input: strs = ["edcba"]
Output: 4
Explanation: If we delete less than 4 columns, the only row will not be lexicographically sorted.

Example 3:

Input: strs = ["ghi","def","abc"]
Output: 0
Explanation: All rows are already lexicographically sorted.

__Constraints:__

n == strs.length
1 <= n <= 100
1 <= strs[i].length <= 100
strs[i] consists of lowercase English letters.

__题目描述__:
给定由 N 个小写字母字符串组成的数组 A，其中每个字符串长度相等。

选取一个删除索引序列，对于 A 中的每个字符串，删除对应每个索引处的字符。

比如，有 A = ["babca","bbazb"]，删除索引序列 {0, 1, 4}，删除后 A 为["bc","az"]。

假设，我们选择了一组删除索引 D，那么在执行删除操作之后，最终得到的数组的行中的每个元素都是按字典序排列的。

清楚起见，A[0] 是按字典序排列的（即，A[0][0] <= A[0][1] <= ... <= A[0][A[0].length - 1]），A[1] 是按字典序排列的（即，A[1][0] <= A[1][1] <= ... <= A[1][A[1].length - 1]），依此类推。

请你返回 D.length 的最小可能值。

__示例 :__

示例 1：

输入：["babca","bbazb"]
输出：3
解释：
删除 0、1 和 4 这三列后，最终得到的数组是 A = ["bc", "az"]。
这两行是分别按字典序排列的（即，A[0][0] <= A[0][1] 且 A[1][0] <= A[1][1]）。
注意，A[0] > A[1] —— 数组 A 不一定是按字典序排列的。

示例 2：

输入：["edcba"]
输出：4
解释：如果删除的列少于 4 列，则剩下的行都不会按字典序排列。

示例 3：

输入：["ghi","def","abc"]
输出：0
解释：所有行都已按字典序排列。

__提示:__

1 <= A.length <= 100
1 <= A[i].length <= 100

__思路__:

动态规划
本题本质上是求最长不递减子序列
dp[i] 表示 strs[:i] 中最长的不递减子序列的长度
dp[i] = max(dp[i], dp[j] + 1), 0 <= j < i
遍历所有行, 如果所有 strs[k][i] >= strs[k][j], 则 dp[i] = max(dp[i], dp[j] + 1), 否则不更新 dp
最后返回 len(strs) - max(dp)
时间复杂度为 O(nm ^ 2), 空间复杂度为 O(m), n 为 strs 长度, m 为 strs[0] 长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minDeletionSize(vector<string>& strs) 
    {
        int n = strs.size(), m = strs.front().size();
        vector<int> dp(m, 1);
        for (int i = 1; i < m; i++) 
        {
            for (int j = 0; j < i; j++) 
            {
                int flag = true;
                for (int k = 0; k < n; k++) 
                {
                    if (strs[k][i] < strs[k][j]) 
                    {
                        flag = false; 
                        break;
                    }
                }
                if (flag) dp[i] = max(dp[i], dp[j] + 1);
            }
        }
        return m - *max_element(dp.begin(), dp.end());
    }
};
```

__Java__:

```Java
class Solution {
    public int minDeletionSize(String[] strs) {
        int n = strs.length, m = strs[0].length(), result = m - 1, dp[] = new int[m];
        Arrays.fill(dp, 1);
        for (int i = 1; i < m; i++) {
            for (int j = 0; j < i; j++) {
                boolean flag = true;
                for (int k = 0; k < n; k++) {
                    if (strs[k].charAt(i) < strs[k].charAt(j)) {
                        flag = false; 
                        break;
                    }
                }
                if (flag) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                    result = Math.min(result, m - dp[i]);
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minDeletionSize(self, strs: List[str]) -> int:
        dp = [1] * (n := len(strs[0]))
        for i in range(n - 1, -1, -1):
            for j in range(i + 1, n):
                if all(word[i] <= word[j] for word in strs):
                    dp[i] = max(dp[i], dp[j] + 1)
        return n - max(dp)
```
