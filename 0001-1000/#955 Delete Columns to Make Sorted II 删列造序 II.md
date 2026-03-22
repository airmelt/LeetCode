# 955 Delete Columns to Make Sorted II 删列造序 II

__Description__:
You are given an array of n strings strs, all of the same length.

We may choose any deletion indices, and we delete all the characters in those indices for each string.

For example, if we have strs = ["abcdef","uvwxyz"] and deletion indices {0, 2, 3}, then the final array after deletions is ["bef", "vyz"].

Suppose we chose a set of deletion indices answer such that after deletions, the final array has its elements in lexicographic order (i.e., strs[0] <= strs[1] <= strs[2] <= ... <= strs[n - 1]). Return the minimum possible value of answer.length.

__Example:__

Example 1:

Input: strs = ["ca","bb","ac"]
Output: 1
Explanation:
After deleting the first column, strs = ["a", "b", "c"].
Now strs is in lexicographic order (ie. strs[0] <= strs[1] <= strs[2]).
We require at least 1 deletion since initially strs was not in lexicographic order, so the answer is 1.

Example 2:

Input: strs = ["xc","yb","za"]
Output: 0
Explanation:
strs is already in lexicographic order, so we do not need to delete anything.
Note that the rows of strs are not necessarily in lexicographic order:
i.e., it is NOT necessarily true that (strs[0][0] <= strs[0][1] <= ...)

Example 3:

Input: strs = ["zyx","wvu","tsr"]
Output: 3
Explanation: We have to delete every column.

__Constraints:__

n == strs.length
1 <= n <= 100
1 <= strs[i].length <= 100
strs[i] consists of lowercase English letters.

__题目描述__:
给定由 n 个字符串组成的数组 strs，其中每个字符串长度相等。

选取一个删除索引序列，对于 strs 中的每个字符串，删除对应每个索引处的字符。

比如，有 strs = ["abcdef", "uvwxyz"]，删除索引序列 {0, 2, 3}，删除后 strs 为["bef", "vyz"]。

假设，我们选择了一组删除索引 answer，那么在执行删除操作之后，最终得到的数组的元素是按 字典序（strs[0] <= strs[1] <= strs[2] ... <= strs[n - 1]）排列的，然后请你返回 answer.length 的最小可能值。

__示例 :__

示例 1：

输入：strs = ["ca","bb","ac"]
输出：1
解释：
删除第一列后，strs = ["a", "b", "c"]。
现在 strs 中元素是按字典排列的 (即，strs[0] <= strs[1] <= strs[2])。
我们至少需要进行 1 次删除，因为最初 strs 不是按字典序排列的，所以答案是 1。

示例 2：

输入：strs = ["xc","yb","za"]
输出：0
解释：
strs 的列已经是按字典序排列了，所以我们不需要删除任何东西。
注意 strs 的行不需要按字典序排列。
也就是说，strs[0][0] <= strs[0][1] <= ... 不一定成立。

示例 3：

输入：strs = ["zyx","wvu","tsr"]
输出：3
解释：
我们必须删掉每一列。

__提示:__

n == strs.length
1 <= n <= 100
1 <= strs[i].length <= 100
strs[i] 由小写英文字母组成

__思路__:

贪心
用一个数组记录之前的列的相对大小
比如 'ab' 和 'aa' 第二列需要比较大小
但是 'bz' 和 'ca' 第二列不需要比较大小
遍历按列遍历 strs 数组
如果这次不需要比较大小直接跳过
如果出现逆序直接删除该列, 并增加结果
如果当前行小于下一行, 则记录下一列这一行不需要比较
如果该列需要删除数组沿用, 否则更新数组
时间复杂度为 O(mn), 空间复杂度为 O(m), m 为 strs 数组的长度, n 为 strs 中字符串的长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minDeletionSize(vector<string>& strs) 
    {
        int m = strs.size(), n = strs.front().size(), result = 0;
        vector<int> visited(m);
        for(int j = 0; j < n; j++) 
        {
            vector<int> cur(visited);
            bool flag = false;
            for (int i = 1; i < m; i++) 
            {
                if (cur[i]) continue;
                if (strs[i - 1][j] > strs[i][j])
                {
                    flag = true;
                    break;
                }
                else if (strs[i - 1][j] < strs[i][j]) cur[i] = 1;
            }
            if (flag) ++result;
            else visited = cur;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minDeletionSize(String[] strs) {
        int m = strs.length, n = strs[0].length(), result = 0, visited[] = new int[m];
        for (int j = 0; j < n; j++) {
            int[] cur = Arrays.copyOf(visited, m);
            boolean flag = false;
            for (int i = 1; i < m; i++) {
                if (cur[i] != 0) continue;
                if (strs[i - 1].charAt(j) > strs[i].charAt(j)) {
                    flag = true;
                    break;
                } else if (strs[i - 1].charAt(j) < strs[i].charAt(j)) cur[i] = 1;
            }
            if (flag) ++result;
            else visited = cur;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minDeletionSize(self, strs: List[str]) -> int:
        visited, n, result = [False] * (m := len(strs)), len(strs[0]), 0
        for j in range(n):
            flag, cur = False, visited[:]
            for i in range(1, m):
                if cur[i]:
                    continue
                if strs[i - 1][j] > strs[i][j]:
                    flag = True
                    break
                elif strs[i - 1][j] < strs[i][j]:
                    cur[i] = True
            if flag:
                result += 1
            else:
                visited = cur
        return result
```
