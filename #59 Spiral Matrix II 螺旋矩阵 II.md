__Description__:
Given a positive integer n, generate a square matrix filled with elements from 1 to n2 in spiral order.

__Example:__

Input: 3
Output:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]

__题目描述__:
给定一个正整数 n，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。

__示例 :__

输入: 3
输出:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]

__思路__:
参考[LeetCode #54 Spiral Matrix 螺旋矩阵](https://www.jianshu.com/p/1de47741d08a)
从左上右下四个方向模拟矩阵螺旋
时间复杂度O(n ^ 2), 空间复杂度O(n ^ 2)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<vector<int>> generateMatrix(int n) 
    {
        vector<vector<int>> result(n, vector<int>(n, 0));
        int left = 0, right = n - 1, up = 0, down = n - 1, c = 1;
        while (true)
        {
            for (int i = left; i <= right; i++) result[up][i] = c++;
            if (++up > down) break;
            for (int i = up; i <= down; i++) result[i][right] = c++;
            if (--right < left) break;
            for (int i = right; i >= left; i--) result[down][i] = c++;
            if (--down < up) break;
            for (int i = down; i >= up; i--) result[i][left] = c++;
            if (++left > right) break;
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int[][] generateMatrix(int n) {
        int result[][] = new int[n][n], left = 0, right = n - 1, up = 0, down = n - 1, c = 1, sq = n * n;
        while (c <= sq) {
            for (int i = left; i <= right; i++) result[up][i] = c++;
            ++up;
            for (int i = up; i <= down; i++) result[i][right] = c++;
            --right;
            for (int i = right; i >= left; i--) result[down][i] = c++;
            --down;
            for (int i = down; i >= up; i--) result[i][left] = c++;
            ++left;
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        result, left, right, up, down, c, sq = [[0 for _ in range(n)] for _ in range(n)], 0, n - 1, 0, n - 1, 1, n ** 2;
        while c <= sq:
            for i in range(left, right + 1):
                result[up][i] = c
                c += 1
            up += 1
            for i in range(up, down + 1):
                result[i][right] = c
                c += 1
            right -= 1
            for i in range(right, left - 1, - 1):
                result[down][i] = c
                c += 1
            down -= 1
            for i in range(down, up - 1, -1):
                result[i][left] = c
                c += 1
            left += 1
        return result
```