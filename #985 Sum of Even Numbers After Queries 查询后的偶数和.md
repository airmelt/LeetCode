__Description__:
We have an array A of integers, and an array queries of queries.

For the i-th query val = queries[i][0], index = queries[i][1], we add val to A[index].  Then, the answer to the i-th query is the sum of the even values of A.

(Here, the given index = queries[i][1] is a 0-based index, and each query permanently modifies the array A.)

Return the answer to all queries.  Your answer array should have answer[i] as the answer to the i-th query.

__Example:__
Example 1:

Input: A = [1,2,3,4], queries = [[1,0],[-3,1],[-4,0],[2,3]]
Output: [8,6,2,4]
Explanation: 
At the beginning, the array is [1,2,3,4].
After adding 1 to A[0], the array is [2,2,3,4], and the sum of even values is 2 + 2 + 4 = 8.
After adding -3 to A[1], the array is [2,-1,3,4], and the sum of even values is 2 + 4 = 6.
After adding -4 to A[0], the array is [-2,-1,3,4], and the sum of even values is -2 + 4 = 2.
After adding 2 to A[3], the array is [-2,-1,3,6], and the sum of even values is -2 + 6 = 4.
 
__Note:__

1 <= A.length <= 10000
-10000 <= A[i] <= 10000
1 <= queries.length <= 10000
-10000 <= queries[i][0] <= 10000
0 <= queries[i][1] < A.length

__题目描述__:
给出一个整数数组 A 和一个查询数组 queries。

对于第 i 次查询，有 val = queries[i][0], index = queries[i][1]，我们会把 val 加到 A[index] 上。然后，第 i 次查询的答案是 A 中偶数值的和。

（此处给定的 index = queries[i][1] 是从 0 开始的索引，每次查询都会永久修改数组 A。）

返回所有查询的答案。你的答案应当以数组 answer 给出，answer[i] 为第 i 次查询的答案。

__示例 :__

输入：A = [1,2,3,4], queries = [[1,0],[-3,1],[-4,0],[2,3]]
输出：[8,6,2,4]
解释：
开始时，数组为 [1,2,3,4]。
将 1 加到 A[0] 上之后，数组为 [2,2,3,4]，偶数值之和为 2 + 2 + 4 = 8。
将 -3 加到 A[1] 上之后，数组为 [2,-1,3,4]，偶数值之和为 2 + 4 = 6。
将 -4 加到 A[0] 上之后，数组为 [-2,-1,3,4]，偶数值之和为 -2 + 4 = 2。
将 2 加到 A[3] 上之后，数组为 [-2,-1,3,6]，偶数值之和为 -2 + 6 = 4。
 
__提示：__

1 <= A.length <= 10000
-10000 <= A[i] <= 10000
1 <= queries.length <= 10000
-10000 <= queries[i][0] <= 10000
0 <= queries[i][1] < A.length

__思路__:
先记录数组原来的偶数之和, 之后遍历查询数组, 对每个需要改变的 A数组的元素判断奇偶, 记录之后再改变, 再判断是否需要加入偶数之和
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution {
public:
    vector<int> sumEvenAfterQueries(vector<int>& A, vector<vector<int>>& queries) 
    {
        int even = 0;
        vector<int>result(A.size());
        for (auto a : A) if (!(a & 1)) even += a;
        for (int i = 0; i < queries.size(); i++) 
        {
            if (!(A[queries[i][1]] & 1)) even -= A[queries[i][1]];
            A[queries[i][1]] += queries[i][0];
            if (!(A[queries[i][1]] & 1)) even += A[queries[i][1]];
            result[i] += even;
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int[] sumEvenAfterQueries(int[] A, int[][] queries) {
        int even = 0, result[] = new int[queries.length];
        for (int a : A) if ((a & 1) == 0) even += a;
        for (int i = 0; i < queries.length; i++) {
            if ((A[queries[i][1]] & 1) == 0) even -= A[queries[i][1]];
            A[queries[i][1]] += queries[i][0];
            if ((A[queries[i][1]] & 1) == 0) even += A[queries[i][1]];
            result[i] += even;
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def sumEvenAfterQueries(self, A: List[int], queries: List[List[int]]) -> List[int]:
        result, even = [0] * len(A), sum(a for a in A if not (a & 1))
        for i, item in enumerate(queries):
            if not (A[item[1]] & 1):
                even -= A[item[1]]
            A[item[1]] += item[0]
            if not (A[item[1]] & 1):
                even += A[item[1]]
            result[i] += even
        return result
```