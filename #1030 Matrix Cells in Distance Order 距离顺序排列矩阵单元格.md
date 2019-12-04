__Description__:
We are given a matrix with R rows and C columns has cells with integer coordinates (r, c), where 0 <= r < R and 0 <= c < C.

Additionally, we are given a cell in that matrix with coordinates (r0, c0).

Return the coordinates of all cells in the matrix, sorted by their distance from (r0, c0) from smallest distance to largest distance.  Here, the distance between two cells (r1, c1) and (r2, c2) is the Manhattan distance, |r1 - r2| + |c1 - c2|.  (You may return the answer in any order that satisfies this condition.)

__Example:__
Example 1:

Input: R = 1, C = 2, r0 = 0, c0 = 0
Output: [[0,0],[0,1]]
Explanation: The distances from (r0, c0) to other cells are: [0,1]

Example 2:

Input: R = 2, C = 2, r0 = 0, c0 = 1
Output: [[0,1],[0,0],[1,1],[1,0]]
Explanation: The distances from (r0, c0) to other cells are: [0,1,1,2]
The answer [[0,1],[1,1],[0,0],[1,0]] would also be accepted as correct.

Example 3:

Input: R = 2, C = 3, r0 = 1, c0 = 2
Output: [[1,2],[0,2],[1,1],[0,1],[1,0],[0,0]]
Explanation: The distances from (r0, c0) to other cells are: [0,1,1,2,2,3]
There are other answers that would also be accepted as correct, such as [[1,2],[1,1],[0,2],[1,0],[0,1],[0,0]].
 
__Note:__

1 <= R <= 100
1 <= C <= 100
0 <= r0 < R
0 <= c0 < C

__题目描述__:
给出 R 行 C 列的矩阵，其中的单元格的整数坐标为 (r, c)，满足 0 <= r < R 且 0 <= c < C。

另外，我们在该矩阵中给出了一个坐标为 (r0, c0) 的单元格。

返回矩阵中的所有单元格的坐标，并按到 (r0, c0) 的距离从最小到最大的顺序排，其中，两单元格(r1, c1) 和 (r2, c2) 之间的距离是曼哈顿距离，|r1 - r2| + |c1 - c2|。（你可以按任何满足此条件的顺序返回答案。）

__示例 :__
示例 1：

输入：R = 1, C = 2, r0 = 0, c0 = 0
输出：[[0,0],[0,1]]
解释：从 (r0, c0) 到其他单元格的距离为：[0,1]

示例 2：

输入：R = 2, C = 2, r0 = 0, c0 = 1
输出：[[0,1],[0,0],[1,1],[1,0]]
解释：从 (r0, c0) 到其他单元格的距离为：[0,1,1,2]
[[0,1],[1,1],[0,0],[1,0]] 也会被视作正确答案。

示例 3：

输入：R = 2, C = 3, r0 = 1, c0 = 2
输出：[[1,2],[0,2],[1,1],[0,1],[1,0],[0,0]]
解释：从 (r0, c0) 到其他单元格的距离为：[0,1,1,2,2,3]
其他满足题目要求的答案也会被视为正确，例如 [[1,2],[1,1],[0,2],[1,0],[0,1],[0,0]]。
 
__提示：__

1 <= R <= 100
1 <= C <= 100
0 <= r0 < R
0 <= c0 < C

__思路__:
1. 先加入所有符合要求的点的坐标, 再按绝对值大小排序
时间复杂度O(nlgn), 空间复杂度O(1)
2. 直接按绝对值大小加入
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<vector<int>> allCellsDistOrder(int R, int C, int r0, int c0) 
    {
        int n = max(max(max(r0 + c0, r0 + C - 1 - c0), R - 1 - r0 + c0), R - 1 - r0 + C - 1 - c0);
        vector<vector<int>> result;
        result.push_back(vector<int>{r0, c0});
        for (int i = 1; i <= n; i++) 
        {
            for (int x = 0; x <= i; x++) 
            {
                if (r0 - x >= 0 && c0 - (i - x) >= 0) result.push_back(vector<int>{r0 - x, c0 - (i - x)});
                if (r0 + x <= R - 1 && c0 - (i - x) >= 0 && x != 0 && i != x) result.push_back(vector<int>{r0 + x, c0 - (i - x)});           
                if (r0 - x >= 0 && c0 + (i - x) <= C - 1  && x != 0 && i != x) result.push_back(vector<int>{r0 - x, c0 + (i - x)});
                if (r0 + x <= R - 1 && c0 + (i - x) <= C - 1) result.push_back(vector<int>{r0 + x, c0 + (i - x)});
            }    
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int[][] allCellsDistOrder(int R, int C, int r0, int c0) {
        int result[][] = new int[R * C][2], index = 0;
        for(int i = 0; i < R; i++) for(int j = 0; j < C; j++) result[index++] = new int[]{i, j};
        Arrays.sort(result, (int[] a, int[] b) -> {
            return Math.abs(a[0] - r0) + Math.abs(a[1] - c0) - Math.abs(b[0] - r0) - Math.abs(b[1] - c0);
        });
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def allCellsDistOrder(self, R: int, C: int, r0: int, c0: int) -> List[List[int]]:
        return sorted(((i,j) for i in range(R) for j in range(C)), key=lambda p:abs(p[0] - r0)+abs(p[1] - c0))
```