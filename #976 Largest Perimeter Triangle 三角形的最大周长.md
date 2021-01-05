# 976 Largest Perimeter Triangle 三角形的最大周长

__Description__:
Given an array A of positive lengths, return the largest perimeter of a triangle with non-zero area, formed from 3 of these lengths.

If it is impossible to form any triangle of non-zero area, return 0.

__Example:__

Example 1:

Input: [2,1,2]
Output: 5

Example 2:

Input: [1,2,1]
Output: 0

Example 3:

Input: [3,2,3,4]
Output: 10

Example 4:

Input: [3,6,2,3]
Output: 8

__Note:__

3 <= A.length <= 10000
1 <= A[i] <= 10^6

__题目描述__:
给定由一些正数（代表长度）组成的数组 A，返回由其中三个长度组成的、面积不为零的三角形的最大周长。

如果不能形成任何面积不为零的三角形，返回 0。

__示例 :__

示例 1：

输入：[2,1,2]
输出：5

示例 2：

输入：[1,2,1]
输出：0

示例 3：

输入：[3,2,3,4]
输出：10

示例 4：

输入：[3,6,2,3]
输出：8

__提示：__

3 <= A.length <= 10000
1 <= A[i] <= 10^6

__思路__:

先排序, 然后从最大值开始, 遍历到最大边小于另外两边之和, 这个三角形即为最大变长的三角形
时间复杂度O(nlgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int largestPerimeter(vector<int>& A) 
    {
        sort(A.begin(), A.end());
        for (int i = A.size() - 1; i >= 2; i--) if (A[i - 2] + A[i - 1] > A[i]) return A[i - 2] + A[i - 1] + A[i];
        return 0;
    }
};
```

__Java__:

```Java
class Solution {
    public int largestPerimeter(int[] A) {
        Arrays.sort(A);
        for (int i = A.length - 1; i >= 2; i--) if (A[i - 2] + A[i - 1] > A[i]) return A[i - 2] + A[i - 1] + A[i];
        return 0;
    }
}
```

__Python__:

```Python
class Solution:
    def largestPerimeter(self, A: List[int]) -> int:
        A.sort(reverse=True)
        for i in range(2, len(A)):
            if A[i] + A[i - 1] > A[i - 2]:
                return A[i - 2] + A[i - 1] + A[i]
        return 0
```
