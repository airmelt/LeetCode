# 2657 Find the Prefix Common Array of Two Arrays 找到两个数组的前缀公共数组

__Description:__

You are given two __0-indexed__ integer permutations `A` and `B` of length `n`.

A __prefix common array__ of `A` and `B` is an array `C` such that `C[i]` is equal to the count of numbers that are present at or before the index `i` in both `A` and `B`.

Return _the __prefix common array__ of_ `A` _and_ `B`.

A sequence of `n` integers is called a __permutation__ if it contains all integers from `1` to `n` exactly once.

__Example:__

Example 1:

```text
Input: A = [1,3,2,4], B = [3,1,2,4]
Output: [0,2,3,4]
Explanation: At i = 0: no number is common, so C[0] = 0.
At i = 1: 1 and 3 are common in A and B, so C[1] = 2.
At i = 2: 1, 2, and 3 are common in A and B, so C[2] = 3.
At i = 3: 1, 2, 3, and 4 are common in A and B, so C[3] = 4.
```

Example 2:

```text
Input: A = [2,3,1], B = [3,1,2]
Output: [0,1,3]
Explanation: At i = 0: no number is common, so C[0] = 0.
At i = 1: only 3 is common in A and B, so C[1] = 1.
At i = 2: 1, 2, and 3 are common in A and B, so C[2] = 3.
```

__Constraints:__

- `1 <= A.length == B.length == n <= 50`
- `1 <= A[i], B[i] <= n`
- `It is guaranteed that A and B are both a permutation of n integers.`

__题目描述:__

给你两个下标从 __0__ 开始长度为 `n` 的整数排列 `A` 和 `B` 。

`A` 和 `B` 的 __前缀公共数组__ 定义为数组 `C` ，其中 `C[i]` 是数组 `A` 和 `B` 到下标为 `i` 之前公共元素的数目。

请你返回 `A` 和 `B` 的 __前缀公共数组__ 。

如果一个长度为 `n` 的数组包含 `1` 到 `n` 的元素恰好一次，我们称这个数组是一个长度为 `n` 的 __排列__ 。

__示例:__

示例 1：

```text
输入：A = [1,3,2,4], B = [3,1,2,4]
输出：[0,2,3,4]
解释：i = 0：没有公共元素，所以 C[0] = 0 。
i = 1：1 和 3 是两个数组的前缀公共元素，所以 C[1] = 2 。
i = 2：1，2 和 3 是两个数组的前缀公共元素，所以 C[2] = 3 。
i = 3：1，2，3 和 4 是两个数组的前缀公共元素，所以 C[3] = 4 。
```

示例 2：

```text
输入：A = [2,3,1], B = [3,1,2]
输出：[0,1,3]
解释：i = 0：没有公共元素，所以 C[0] = 0 。
i = 1：只有 3 是公共元素，所以 C[1] = 1 。
i = 2：1，2 和 3 是两个数组的前缀公共元素，所以 C[2] = 3 。
```

__提示：__

- `1 <= A.length == B.length == n <= 50`
- `1 <= A[i], B[i] <= n`
- 题目保证 `A` 和 `B` 两个数组都是 `n` 个元素的排列。

__思路:__

```text
位运算
使用两个变量来存储出现过的数字
p |= 1 << nums[i]
这样只需要进行一次与运算就能得到两个数组前缀的公共元素
计算该公共元素的二进制长度即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findThePrefixCommonArray(vector<int>& A, vector<int>& B) 
    {
        vector<int> result(A.size());
        long long p = 0LL, q = 0LL;
        for (int i = 0, n = A.size(); i < n; i++) result[i] = __builtin_popcountll((p |= 1LL << A[i]) & (q |= 1LL << B[i]));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] findThePrefixCommonArray(int[] A, int[] B) {
        int n = A.length, result[] = new int[n];
        long p = 0L, q = 0L;
        for (int i = 0; i < n; i++) result[i] = Long.bitCount((p |= 1L << A[i]) & (q |= 1L << B[i]));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findThePrefixCommonArray(self, A: List[int], B: List[int]) -> List[int]:
        return [len(set(A[:i + 1]) & set(B[:i + 1])) for i in range(len(A))]
```
