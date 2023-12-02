# 1901 Find a Peak Element II 寻找峰值 II

__Description:__

A peak element in a 2D grid is an element that is strictly greater than all of its adjacent neighbors to the left, right, top, and bottom.

Given a __0-indexed__ `m x n` matrix `mat` where __no two adjacent cells are equal__, find __any__ peak element `mat[i][j]` and return _the length 2 array_ `[i,j]`.

You may assume that the entire matrix is surrounded by an __outer perimeter__ with the value `-1` in each cell.

You must write an algorithm that runs in `O(mlog(n))` or `O(nlog(m))` time.

__Example:__

Example 1:

![1901-1](https://assets.leetcode.com/uploads/2021/06/08/1.png)

```text
Input: mat = [[1,4],[3,2]]
Output: [0,1]
Explanation: Both 3 and 4 are peak elements so [1,0] and [0,1] are both acceptable answers.
```

Example 2:

![1901-2](https://assets.leetcode.com/uploads/2021/06/07/3.png)

```text
Input: mat = [[10,20,15],[21,30,14],[7,16,32]]
Output: [1,1]
Explanation: Both 30 and 32 are peak elements so [1,1] and [2,2] are both acceptable answers.
```

__Constraints:__

- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 500`
- `1 <= mat[i][j] <= 10 ^ 5`
- No two adjacent cells are equal.

__题目描述:__

一个 2D 网格中的 峰值 是指那些 严格大于 其相邻格子(上、下、左、右)的元素。

给你一个 __从 0 开始编号__ 的 `m x n` 矩阵 `mat` ，其中任意两个相邻格子的值都 __不相同__ 。找出 __任意一个 峰值__ `mat[i][j]` 并 __返回其位置__ `[i,j]` 。

你可以假设整个矩阵周边环绕着一圈值为 `-1` 的格子。

要求必须写出时间复杂度为 `O(mlog(n))` 或 `O(nlog(m))` 的算法

__示例:__

示例 1:

![1901-3](https://assets.leetcode.com/uploads/2021/06/08/1.png)

```text
输入: mat = [[1,4],[3,2]]
输出: [0,1]
解释: 3 和 4 都是峰值，所以[1,0]和[0,1]都是可接受的答案。
```

示例 2:

![1901-4](https://assets.leetcode.com/uploads/2021/06/07/3.png)

```text
输入: mat = [[10,20,15],[21,30,14],[7,16,32]]
输出: [1,1]
解释: 30 和 32 都是峰值，所以[1,1]和[2,2]都是可接受的答案。
```

__提示：__

- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 500`
- `1 <= mat[i][j] <= 10 ^ 5`
- 任意两个相邻元素均不相等.

__思路:__

```text
二分
二分查找每一行的最大值
比较它和它上下两行的最大值的大小关系
如果它大于等于上下两行的最大值, 则它就是峰值
否则取上下两行的最大值中较大的那个, 二分查找它所在的行的部分
时间复杂度为 O(MlogN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findPeakGrid(vector<vector<int>>& mat) 
    {
        int m = mat.size(), n = mat.front().size(), up = 0, down = m;
        while (up < down)
        {
            int mid = up + ((down - up) >> 1), max_up = mid ? *max_element(mat[mid - 1].begin(), mat[mid - 1].end()) : -1, max_down = mid + 1 == m ? -1 : *max_element(mat[mid + 1].begin(), mat[mid + 1].end());
            auto it = max_element(mat[mid].begin(), mat[mid].end());
            if (*it >= max(max_up, max_down)) return {mid, int(it - mat[mid].begin())};
            else if (max_up >= max(*it, max_down)) down = mid;
            else up = mid + 1;
        }
        return {};
    }
};
```

__Java__:

```Java
class Solution {
    public int[] findPeakGrid(int[][] mat) {
        int m = mat.length, n = mat[0].length, up = 0, down = m;
        while (up < down) {
            int mid = up + ((down - up) >>> 1), maxUp = getMax(mid - 1, mat)[1], maxDown = getMax(mid + 1, mat)[1], maxCur[] = getMax(mid, mat);
            if (maxCur[1] >= Math.max(maxUp, maxDown)) return new int[]{mid, maxCur[0]};
            else if (maxUp >= Math.max(maxCur[1], maxDown)) down = mid;
            else up = mid + 1;
        }
        return new int[0];
    }

    private int[] getMax(int cur, int[][] mat) {
        int result[] = new int[]{-1, -1}, m = mat.length, n = mat[0].length;
        if (cur > -1 && cur < m) {
            for (int j = 0; j < n; j++) {
                if (mat[cur][j] > result[1]) {
                    result[1] = mat[cur][j];
                    result[0] = j;
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findPeakGrid(self, mat: List[List[int]]) -> List[int]:
        n, up, down = len(mat[0]), 0, (m := len(mat))
        while up < down:
            max_up, max_down, max_value = max(mat[mid - 1]) if (mid := up + ((down - up) >> 1)) else -1, max(mat[mid + 1]) if mid != m - 1 else -1, max(mat[mid])
            if max_value >= max(max_up, max_down):
                return [mid, mat[mid].index(max_value)]
            elif max_up >= max(max_value, max_down):
                down = mid
            else:
                up = mid + 1
```
