# 1738 Find Kth Largest XOR Coordinate Value 

__Description:__

You are given a 2D `matrix` of size `m x n`, consisting of non-negative integers. You are also given an integer `k`.

The __value__ of coordinate `(a, b)` of the matrix is the XOR of all `matrix[i][j]` where `0 <= i <= a < m` and `0 <= j <= b < n` __(0-indexed)__.

Find the `k ^ th` largest value __(1-indexed)__ of all the coordinates of `matrix`.

__Example:__

Example 1:

```text
Input: matrix = [[5,2],[1,6]], k = 1
Output: 7
Explanation: The value of coordinate (0,1) is 5 XOR 2 = 7, which is the largest value.
```

Example 2:

```text
Input: matrix = [[5,2],[1,6]], k = 2
Output: 5
Explanation: The value of coordinate (0,0) is 5 = 5, which is the 2nd largest value.
```

Example 3:

```text
Input: matrix = [[5,2],[1,6]], k = 3
Output: 4
Explanation: The value of coordinate (1,0) is 5 XOR 1 = 4, which is the 3rd largest value.
```

__Constraints:__

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 1000`
- `0 <= matrix[i][j] <= 10 ^ 6`
- `1 <= k <= m * n`

__题目描述:__

给你一个二维矩阵 `matrix` 和一个整数 `k` ，矩阵大小为 `m x n` 由非负整数组成。

矩阵中坐标 `(a, b)` 的 __值__ 可由对所有满足 `0 <= i <= a < m` 且 `0 <= j <= b < n` 的元素 `matrix[i][j]`（__下标从 0 开始计数__）执行异或运算得到。

请你找出 `matrix` 的所有坐标中第 `k` 大的值（__`k` 的值从 1 开始计数__）。

__示例:__

示例 1：

```text
输入：matrix = [[5,2],[1,6]], k = 1
输出：7
解释：坐标 (0,1) 的值是 5 XOR 2 = 7 ，为最大的值。
```

示例 2：

```text
输入：matrix = [[5,2],[1,6]], k = 2
输出：5
解释：坐标 (0,0) 的值是 5 = 5 ，为第 2 大的值。
```

示例 3：

```text
输入：matrix = [[5,2],[1,6]], k = 3
输出：4
解释：坐标 (1,0) 的值是 5 XOR 1 = 4 ，为第 3 大的值。
```

示例 4：

```text
输入：matrix = [[5,2],[1,6]], k = 4
输出：0
解释：坐标 (1,1) 的值是 5 XOR 2 XOR 1 XOR 6 = 0 ，为第 4 大的值。
```

__提示：__

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 1000`
- `0 <= matrix[i][j] <= 10 ^ 6`
- `1 <= k <= m * n`

__思路:__

```text

时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int kthLargestValue(vector<vector<int>>& matrix, int k) 
    {
        int m = matrix.length, n = matrix[0].length, pre[] = new int[n];
        PriorityQueue<Integer> pq = new PriorityQueue();
        for (int i = 0, p = 0; i < m; i++, p = 0) {
            for (int j = 0; j < n; j++) {
                p ^= matrix[i][j];
                if (pq.size() < k) pq.offer(p ^ pre[j]);
                else if ((p ^ pre[j]) > pq.peek()) {
                    pq.poll();
                    pq.offer(p ^ pre[j]);
                }
                pre[j] ^= p;
            }
        }
        return pq.peek();
    }
};
```

__Java__:

```Java
class Solution {
    public int kthLargestValue(int[][] matrix, int k) {
        int m = matrix.length, n = matrix[0].length, cur = 0, pre[] = new int[n];
        PriorityQueue<Integer> pq = new PriorityQueue();
        for (int i = 0, p = 0; i < m; i++, p = 0) {
            for (int j = 0; j < n; j++) {
                p ^= matrix[i][j];
                if (pq.size() < k) pq.offer(p ^ pre[j]);
                else if ((cur = p ^ pre[j]) > pq.peek()) {
                    pq.poll();
                    pq.offer(cur);
                }
                pre[j] ^= p;
            }
        }
        return pq.peek();
    }
}
```

__Python__:

```Python
class Solution 
{
public:
    int kthLargestValue(vector<vector<int>>& matrix, int k) 
    {
        int m = matrix.length, n = matrix[0].length, pre[] = new int[n];
        PriorityQueue<Integer> pq = new PriorityQueue();
        for (int i = 0, p = 0; i < m; i++, p = 0) {
            for (int j = 0; j < n; j++) {
                p ^= matrix[i][j];
                if (pq.size() < k) pq.offer(p ^ pre[j]);
                else if ((p ^ pre[j]) > pq.peek()) {
                    pq.poll();
                    pq.offer(p ^ pre[j]);
                }
                pre[j] ^= p;
            }
        }
        return pq.peek();
    }
};
```
