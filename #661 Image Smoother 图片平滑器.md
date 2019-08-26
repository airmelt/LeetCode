__Description__:
Given a 2D integer matrix M representing the gray scale of an image, you need to design a smoother to make the gray scale of each cell becomes the average gray scale (rounding down) of all the 8 surrounding cells and itself. If a cell has less than 8 surrounding cells, then use as many as you can.

__Example:__
Example 1:

Input:
[[1,1,1],
 [1,0,1],
 [1,1,1]]
Output:
[[0, 0, 0],
 [0, 0, 0],
 [0, 0, 0]]
Explanation:
For the point (0,0), (0,2), (2,0), (2,2): floor(3/4) = floor(0.75) = 0
For the point (0,1), (1,0), (1,2), (2,1): floor(5/6) = floor(0.83333333) = 0
For the point (1,1): floor(8/9) = floor(0.88888889) = 0

__Note:__

The value in the given matrix is in the range of [0, 255].
The length and width of the given matrix are in the range of [1, 150].

__题目描述__:
包含整数的二维矩阵 M 表示一个图片的灰度。你需要设计一个平滑器来让每一个单元的灰度成为平均灰度 (向下舍入) ，平均灰度的计算是周围的8个单元和它本身的值求平均，如果周围的单元格不足八个，则尽可能多的利用它们。

__示例 :__
示例 1:

输入:
[[1,1,1],
 [1,0,1],
 [1,1,1]]
输出:
[[0, 0, 0],
 [0, 0, 0],
 [0, 0, 0]]
解释:
对于点 (0,0), (0,2), (2,0), (2,2): 平均(3/4) = 平均(0.75) = 0
对于点 (0,1), (1,0), (1,2), (2,1): 平均(5/6) = 平均(0.83333333) = 0
对于点 (1,1): 平均(8/9) = 平均(0.88888889) = 0

__注意：__

给定矩阵中的整数范围为 [0, 255]。
矩阵的长和宽的范围均为 [1, 150]。

__思路__:
1. 判断 8个方向进行计算, 注意边界条件即可
2. 可以用偏移数组简化计算
3. 在原数组外面包裹一层 None(或者小数, 能与原数据区分即可), 可以减少代码量
时间复杂度O(mn), 空间复杂度O(1), m为矩阵宽度, n为矩阵高度

__代码__:
__C++__:
```
class Solution {
public:
    vector<vector<int>> imageSmoother(vector<vector<int>>& M) {
        vector<vector<int>> result = M;
        int row = M.size(), col = M[0].size();
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                int count = 1, sum = M[i][j];
                if (i > 0) {
                    count++;
                    sum += M[i - 1][j];
                    if (j > 0) {
                        count++;
                        sum += M[i - 1][j - 1];
                    }
                    if (j < col - 1) {
                        count++;
                        sum += M[i - 1][j + 1];
                    }
                }
                if (i < row - 1) {
                    count++;
                    sum += M[i + 1][j];
                    if (j > 0) {
                        count++;
                        sum += M[i + 1][j - 1];
                    }
                    if (j < col - 1) {
                        count++;
                        sum += M[i + 1][j + 1];
                    }
                }
                if (j > 0) {
                    count++;
                    sum += M[i][j - 1];
                }
                if (j < col - 1) {
                    count++;
                    sum += M[i][j + 1];
                }
                result[i][j] = sum / count;
            }
        }
        return result;
    }
};
```

__Java__:
```
class Solution {
    public int[][] imageSmoother(int[][] M) {
        int row = M.length, col = M[0].length, result[][] = new int[row][col];
        for (int i = 0; i < row; i++) for (int j = 0; j < col; j++) result[i][j] = calculate(M, i, j, row, col);
        return result;
    }
    
    private final int x[] = {-1, 1, 0, 0, -1, -1, 1, 1}, y[] = {0, 0, -1, 1, -1, 1, -1, 1};
    
    private int calculate(int[][] M, int i, int j, int row, int col) {
        int count = 1, sum = M[i][j];        
        for (int k = 0; k < 8; k++) {
            int nextX = i + x[k], nextY = j + y[k]; 
            if (nextX >= 0 && nextX < row && nextY >= 0 && nextY < col) {
                count++;
                sum += M[nextX][nextY];
            }
        }
        return sum / count;
    }
}
```

__Python__:
```
class Solution:
    def imageSmoother(self, M: List[List[int]]) -> List[List[int]]:
        row, col, result = len(M), len(M[0]), [[0] * len(M[0]) for _ in range(len(M))]
        M = [[None] * (col + 2)] + [[None] + i + [None] for i in M] + [[None] * (col + 2)]
        for i in range(1, row + 1):
            for j in range(1, col + 1):
                temp = [M[i - 1][j - 1], M[i][j - 1], M[i + 1][j - 1], M[i - 1][j], M[i][j], M[i + 1][j], M[i - 1][j + 1], M[i][j + 1], M[i + 1][j + 1]]
                temp = [k for k in temp if k is not None]
                result[i - 1][j - 1] = sum(temp) // len(temp)
        return result
```