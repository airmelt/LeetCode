# 1439 Find the Kth Smallest Sum of a Matrix With Sorted Rows 有序矩阵中的第k个最小数组和

__Description:__

You are given an `m x n` matrix `mat` that has its rows sorted in non-decreasing order and an integer `k`.

You are allowed to choose exactly one element from each row to form an array.

Return _the_ `k ^ th` _smallest array sum among all possible arrays_.

__Example:__

Example 1:

```text
Input: mat = [[1,3,11],[2,4,6]], k = 5
Output: 7
Explanation: Choosing one element from each row, the first k smallest sum are:
[1,2], [1,4], [3,2], [3,4], [1,6]. Where the 5th sum is 7.
```

Example 2:

```text
Input: mat = [[1,3,11],[2,4,6]], k = 9
Output: 17
```

Example 3:

```text
Input: mat = [[1,10,10],[1,4,5],[2,3,6]], k = 7
Output: 9
Explanation: Choosing one element from each row, the first k smallest sum are:
[1,1,2], [1,1,3], [1,4,2], [1,4,3], [1,1,6], [1,5,2], [1,5,3]. Where the 7th sum is 9.
```

__Constraints:__

- `m == mat.length`
- `n == mat.length[i]`
- `1 <= m, n <= 40`
- `1 <= mat[i][j] <= 5000`
- `1 <= k <= min(200, n ^ m)`
- `mat[i]` is a non-decreasing array.

__题目描述:__

给你一个 `m * n` 的矩阵 `mat`，以及一个整数 `k` ，矩阵中的每一行都以非递减的顺序排列。

你可以从每一行中选出 1 个元素形成一个数组。返回所有可能数组中的第 k 个 最小 数组和。

__示例:__

示例 1：

```text
输入：mat = [[1,3,11],[2,4,6]], k = 5
输出：7
解释：从每一行中选出一个元素，前 k 个和最小的数组分别是：
[1,2], [1,4], [3,2], [3,4], [1,6]。其中第 5 个的和是 7 。
```

示例 2：

```text
输入：mat = [[1,3,11],[2,4,6]], k = 9
输出：17
```

示例 3：

```text
输入：mat = [[1,10,10],[1,4,5],[2,3,6]], k = 7
输出：9
解释：从每一行中选出一个元素，前 k 个和最小的数组分别是：
[1,1,2], [1,1,3], [1,4,2], [1,4,3], [1,1,6], [1,5,2], [1,5,3]。其中第 7 个的和是 9 。
```

示例 4：

```text
输入：mat = [[1,1,10],[2,2,9]], k = 7
输出：12
```

__提示：__

- `m == mat.length`
- `n == mat.length[i]`
- `1 <= m, n <= 40`
- `1 <= k <= min(200, n  ^  m)`
- `1 <= mat[i][j] <= 5000`
- `mat[i]` 是一个非递减数组

__思路:__

```text
1. 多路归并
设置 m 个指针分别指向每一行的第一个数
并计算第一列的总和, 由于数组的每一行都是不减的, 所以这个总和是最小值
每次将每一个指针分别向前移动一步, 更新总和并存入小根堆中, 注意去重
这样重复 k 次, 堆顶即为要求的值
时间复杂度为 O(KMlogKM), 空间复杂度为 O(KM ^ 2)
2. 二分法
由于这个数组每一行都不减, 考虑使用二分法
第一列的总和为左边界 left, 最后一列的总和为右边界 right
需要在 [left, right] 中找到一个值 mid 使得 mid 为第 k 个总和
结束条件为 left > right, 这样区间长度才为 0
与多路归并类似, 移动一次数组的指针位置就统计个数, 个数大于 k 或者当前累计和大于 target 可以提前结束(剪枝)
每次搜索要么移动当前列的指针, 要么移动其他列的指针, 直到移动到最后一行
时间复杂度为 O(KlogS), 空间复杂度为 O(MN), 其中 S 为数组最后一列的总和
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    int m, n, count;
    
    void dfs(int row, int s, int target, int k, vector<vector<int>>& mat)
    {
        if (s > target or count > k or row >= m) return;
        dfs(row + 1, s, target, k, mat);
        for (int i = 1; i < n; i++) 
        {
            if (s - mat[row][0] + mat[row][i] > target) break;
            ++count;
            dfs(row + 1, s - mat[row][0] + mat[row][i], target, k, mat);
        }
    }
public:
    int kthSmallest(vector<vector<int>>& mat, int k) 
    {
        m = mat.size();
        n = mat.front().size();
        int left = 0, right = 0;
        for (int i = 0; i < m; i++)
        {
            left += mat[i].front();
            right += mat[i].back();
        }
        int init = left;
        while (left <= right)
        {
            int mid = left + ((right - left) >> 1);
            count = 1;
            dfs(0, init, mid, k, mat);
            if (count >= k) right = mid - 1;
            else left = mid + 1;
        }
        return left;
    }
};
```

__Java__:

```Java
class Solution {
    public int kthSmallest(int[][] mat, int k) {
        int m = mat.length, n = mat[0].length, record[] = new int[m + 1];
        Set<String> set = new HashSet<>();
        Queue<int[]> q = new PriorityQueue<>((a, b) -> a[m] - b[m]);
        for (int i = 0; i < m; i++) record[m] += mat[i][0];
        q.offer(record);
        set.add(Arrays.toString(record));
        while (--k > 0) {
            int[] cur = q.poll();
            for (int i = 0; i < m; i++) {
                int[] temp = (int[])Arrays.copyOf(cur, m + 1);
                if (temp[i] + 1 >= n) continue;
                temp[m] += mat[i][temp[i] + 1] - mat[i][temp[i]++];
                String s = Arrays.toString(temp);
                if (set.contains(s)) continue;
                q.offer(temp);
                set.add(s);
            }
        }
        return q.poll()[m];
    }
}
```

__Python__:

```Python
class Solution:
    def kthSmallest(self, mat: List[List[int]], k: int) -> int:
        return reduce(lambda x, y: nsmallest(k, map(sum, product(x, y))), mat)[-1]
```
