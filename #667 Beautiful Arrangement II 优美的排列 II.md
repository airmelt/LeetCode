# 667 Beautiful Arrangement II 优美的排列 II

__Description__:
Given two integers n and k, construct a list answer that contains n different positive integers ranging from 1 to n and obeys the following requirement:

Suppose this list is answer = [a1, a2, a3, ... , an], then the list [|a1 - a2|, |a2 - a3|, |a3 - a4|, ... , |an-1 - an|] has exactly k distinct integers.
Return the list answer. If there multiple valid answers, return any of them.

__Example:__

Example 1:

Input: n = 3, k = 1
Output: [1,2,3]
Explanation: The [1,2,3] has three different positive integers ranging from 1 to 3, and the [1,1] has exactly 1 distinct integer: 1

Example 2:

Input: n = 3, k = 2
Output: [1,3,2]
Explanation: The [1,3,2] has three different positive integers ranging from 1 to 3, and the [2,1] has exactly 2 distinct integers: 1 and 2.

__Constraints:__

1 <= k < n <= 10^4

__题目描述__:
给你两个整数 n 和 k ，请你构造一个答案列表 answer ，该列表应当包含从 1 到 n 的 n 个不同正整数，并同时满足下述条件：

假设该列表是 answer = [a1, a2, a3, ... , an] ，那么列表 [|a1 - a2|, |a2 - a3|, |a3 - a4|, ... , |an-1 - an|] 中应该有且仅有 k 个不同整数。
返回列表 answer 。如果存在多种答案，只需返回其中 任意一种 。

__示例 :__

示例 1：

输入：n = 3, k = 1
输出：[1, 2, 3]
解释：[1, 2, 3] 包含 3 个范围在 1-3 的不同整数，并且 [1, 1] 中有且仅有 1 个不同整数：1

示例 2：

输入：n = 3, k = 2
输出：[1, 3, 2]
解释：[1, 3, 2] 包含 3 个范围在 1-3 的不同整数，并且 [2, 1] 中有且仅有 2 个不同整数：1 和 2

__提示:__

1 <= k < n <= 10^4

__思路__:

数学找规律
初始状态设为 1-n 有序排列
比如 n = 8

```text
1 2 3 4 5 6 7 8
| 1 2 3 4 5 6 7 8  k = 1
1 | 8 7 6 5 4 3 2  k = 2
1 8 | 2 3 4 5 6 7  k = 3
1 8 2 | 7 6 5 4 3  k = 4
```

即对于 1 <= m < k, 每次保留前 m 个数, 然后剩下的数字全部反转
时间复杂度 O(n), 空间复杂度 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> constructArray(int n, int k) 
    {
        vector<int> result(n);
        for (int i = 0; i < n; i++) result[i] = i + 1;
        for (int m = 1; m < k; m++) reverse(result.begin() + m, result.end());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] constructArray(int n, int k) {
        int[] result = new int[n];
        for (int i = 0; i < n; i++) result[i] = i + 1;
        for (int m = 1; m < k; m++) reverse(result, m, n - 1); 
        return result;
    }
    
    private void reverse(int[] result, int i, int j) { 
        while (i < j) {
            result[i] ^= result[j];
            result[j] ^= result[i];
            result[i++] ^= result[j--];
        }
    }
}
```

__Python__:

```Python
class Solution:
    def constructArray(self, n: int, k: int) -> List[int]:
        result = list(range(1, n + 1))
        for i in range(1, k):
            result[i:] = result[:i - 1:-1]
        return result
```
