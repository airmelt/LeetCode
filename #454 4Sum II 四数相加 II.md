# 454 4Sum II 四数相加 II

__Description__:
Given four lists A, B, C, D of integer values, compute how many tuples (i, j, k, l) there are such that A[i] + B[j] + C[k] + D[l] is zero.

To make problem a bit easier, all A, B, C, D have same length of N where 0 ≤ N ≤ 500. All integers are in the range of -2^28 to 2^28 - 1 and the result is guaranteed to be at most 2^31 - 1.

__Example:__

Input:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]

Output:
2

Explanation:
The two tuples are:

1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0

__题目描述__:
给定四个包含整数的数组列表 A , B , C , D ,计算有多少个元组 (i, j, k, l) ，使得 A[i] + B[j] + C[k] + D[l] = 0。

为了使问题简单化，所有的 A, B, C, D 具有相同的长度 N，且 0 ≤ N ≤ 500 。所有整数的范围在 -2^28 到 2^28 - 1 之间，最终结果不会超过 2^31 - 1 。

__示例 :__
例如:

输入:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]

输出:
2

解释:
两个元组如下:

1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0

__思路__:

用 map记录 a + b的值, 遍历 c和 d, 找到 -(c + d)就累计到结果中
时间复杂度O(n ^ 2), 空间复杂度O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int fourSumCount(vector<int>& A, vector<int>& B, vector<int>& C, vector<int>& D) 
    {
        unordered_map<int, int> m;
        int result = 0, s = 0;
        for (auto a : A) for (auto b : B) ++m[a + b];
        for (auto c : C) for (auto d : D) if (m.count(s = -(c + d))) result += m[s];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        Map<Integer, Integer> map = new HashMap<>();
        int result = 0;
        for (int a : A) for (int b : B) map.put(a + b, map.getOrDefault(a + b, 0) + 1);
        for (int c : C) for (int d : D) if (map.containsKey(-(c + d))) result += map.get(-(c + d));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def fourSumCount(self, A: List[int], B: List[int], C: List[int], D: List[int]) -> int:
        result, m = 0, dict(collections.Counter([a + b for a in A for b in B]))
        for c in C:
            for d in D:
                s = -(c + d)
                if s in m:
                    result += m[s]
        return result
```
