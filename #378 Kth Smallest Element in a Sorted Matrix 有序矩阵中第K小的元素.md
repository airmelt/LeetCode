__Description__:
Given a n x n matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth distinct element.

__Example:__

matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

return 13.

__Note:__
You may assume k is always valid, 1 ≤ k ≤ n^2.

__题目描述__:
给定一个 n x n 矩阵，其中每行和每列元素均按升序排序，找到矩阵中第 k 小的元素。
请注意，它是排序后的第 k 小元素，而不是第 k 个不同的元素。

__示例 :__

matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

返回 13。

__提示：__
你可以假设 k 的值永远是有效的，1 ≤ k ≤ n^2 。

__思路__:
1. 直接展开成一维数组, 再排序
时间复杂度O(n ^ 2lgn), 空间复杂度O(n ^ 2)
2. 归并排序, 将排序结果放入堆中
时间复杂度O(klgn), 空间复杂度O(n)
3. 二分查找
左上角(matrix.front().front(), matrix[0][0])一定是最小的数 l, 右下角(matrix.back().back(), matrix[n - 1][n - 1])一定是最大的数 r
找到 mid = (l + r) / 2使得小于等于 mid的数刚好有 k个即可
每次从左下角(matrix[0][n - 1])开始搜索 n行, 如果大于 mid向右移动, 否则向上移动直到移出矩阵
统计总的数量, 如果小于 k说明 mid不够, 搜索区间改为 [mid + 1, r], 如果大于等于 k, 说明 mid足够, 搜索区间改为 [l, mid], 直到 l >= r, 输出 r即可
时间复杂度O(nlg(r - l)), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) 
    {
        int n = matrix.size() - 1, l = matrix.front().front(), r = matrix.back().back();
        while (l < r)
        {
            int mid = ((r - l) >> 1) + l;
            if (helper(matrix, mid, k, n)) l = mid + 1;
            else r = mid;
        }
        return r;
    }
private:
    bool helper(vector<vector<int>>& matrix, int mid, int k, int n) 
    {
        int i = n, j = 0, count = 0;
        while (i >= 0 && j <= n) 
        {
            if (matrix[i][j] <= mid) 
            {
                count += i + 1;
                ++j;
            } else --i;
        }
        return count < k;
    }
};
```

__Java__:
```Java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>(new Comparator<int[]>() {
            public int compare(int[] a, int[] b) {
                return a[0] - b[0];
            }
        });
        for (int i = 0; i < matrix.length; i++) pq.offer(new int[]{matrix[i][0], i, 0});
        for (int i = 0; i < k - 1; i++) {
            int[] cur = pq.poll();
            if (cur[2] != matrix.length - 1) pq.offer(new int[]{matrix[cur[1]][cur[2] + 1], cur[1], cur[2] + 1});
        }
        return pq.poll()[0];
    }
}
```

__Python__:
```Python
class Solution:
    def kthSmallest(self, matrix: List[List[int]], k: int) -> int:
        return sorted(sum(matrix, []))[k - 1]
```