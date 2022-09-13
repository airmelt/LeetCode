# 1310 XOR Queries of a Subarray 子数组异或查询

__Description:__

You are given an array arr of positive integers. You are also given the array queries where queries[i] = [lefti, righti].

For each query i compute the XOR of elements from lefti to righti (that is, arr[lefti] XOR arr[lefti + 1] XOR ... XOR arr[righti] ).

Return an array answer where answer[i] is the answer to the ith query.

__Example:__

Example 1:

Input: arr = [1,3,4,8], queries = [[0,1],[1,2],[0,3],[3,3]]
Output: [2,7,14,8]
Explanation:
The binary representation of the elements in the array are:
1 = 0001
3 = 0011
4 = 0100
8 = 1000
The XOR values for queries are
[0,1] = 1 xor 3 = 2
[1,2] = 3 xor 4 = 7
[0,3] = 1 xor 3 xor 4 xor 8 = 14
[3,3] = 8
Example 2:

Input: arr = [4,8,2,10], queries = [[2,3],[1,3],[0,0],[0,3]]
Output: [8,0,4,4]

__Constraints:__

1 <= arr.length, queries.length <= 3 * 10^4
1 <= arr[i] <= 10^9
queries[i].length == 2
0 <= lefti <= righti < arr.length

__题目描述：__

有一个正整数数组 arr，现给你一个对应的查询数组 queries，其中 queries[i] = [Li, Ri]。

对于每个查询 i，请你计算从 Li 到 Ri 的 XOR 值（即 arr[Li] xor arr[Li+1] xor ... xor arr[Ri]）作为本次查询的结果。

并返回一个包含给定查询 queries 所有结果的数组。

__示例：__

示例 1：

输入：arr = [1,3,4,8], queries = [[0,1],[1,2],[0,3],[3,3]]
输出：[2,7,14,8]
解释：
数组中元素的二进制表示形式是：
1 = 0001
3 = 0011
4 = 0100
8 = 1000
查询的 XOR 值为
[0,1] = 1 xor 3 = 2
[1,2] = 3 xor 4 = 7
[0,3] = 1 xor 3 xor 4 xor 8 = 14
[3,3] = 8

示例 2：

输入：arr = [4,8,2,10], queries = [[2,3],[1,3],[0,0],[0,3]]
输出：[8,0,4,4]

__提示：__

1 <= arr.length <= 3 \* 10^4
1 <= arr[i] <= 10^9
1 <= queries.length <= 3 \* 10^4
queries[i].length == 2
0 <= queries\[i][0] <= queries\[i][1] < arr.length

__思路：__

前缀和
先求出异或运算的前缀和 xors[i] = arr[:i] 的异或和
查询的时候可以使用差分运算直接求出区间的异或和
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    vector<int> xorQueries(vector<int>& arr, vector<vector<int>>& queries) 
    {
        int n = arr.size(), m = queries.size();
        vector<int> xors(n + 1), result(m);
        for (int i = 0; i < n; i++) xors[i + 1] = xors[i] ^ arr[i];
        for (int i = 0; i < m; i++) result[i] = xors[queries[i].front()] ^ xors[queries[i].back() + 1];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] xorQueries(int[] arr, int[][] queries) {
        int[] xors = new int[arr.length + 1], result = new int[queries.length];
        for (int i = 0; i < arr.length; i++) xors[i + 1] = xors[i] ^ arr[i];
        for (int i = 0; i < queries.length; i++) result[i] = xors[queries[i][0]] ^ xors[queries[i][1] + 1];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def xorQueries(self, arr: List[int], queries: List[List[int]]) -> List[int]:
        xors = [0]
        for num in arr:
            xors.append(xors[-1] ^ num)
        return [xors[left] ^ xors[right + 1] for left, right in queries]
```
