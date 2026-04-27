# 1222 Queens That Can Attack the King 可以攻击国王的皇后

__Description:__

On an 8x8 chessboard, there can be multiple Black Queens and one White King.

Given an array of integer coordinates queens that represents the positions of the Black Queens, and a pair of coordinates king that represent the position of the White King, return the coordinates of all the queens (in any order) that can attack the King.

__Example:__

Example 1:

![chessboard 1](https://assets.leetcode.com/uploads/2019/10/01/untitled-diagram.jpg)

Input: queens = [[0,1],[1,0],[4,0],[0,4],[3,3],[2,4]], king = [0,0]
Output: [[0,1],[1,0],[3,3]]
Explanation:  
The queen at [0,1] can attack the king cause they're in the same row.
The queen at [1,0] can attack the king cause they're in the same column.
The queen at [3,3] can attack the king cause they're in the same diagnal.
The queen at [0,4] can't attack the king cause it's blocked by the queen at [0,1].
The queen at [4,0] can't attack the king cause it's blocked by the queen at [1,0].
The queen at [2,4] can't attack the king cause it's not in the same row/column/diagnal as the king.

Example 2:

![chessboard 2](https://assets.leetcode.com/uploads/2019/10/01/untitled-diagram-1.jpg)

Input: queens = [[0,0],[1,1],[2,2],[3,4],[3,5],[4,4],[4,5]], king = [3,3]
Output: [[2,2],[3,4],[4,4]]

Example 3:

![chessboard 3](https://assets.leetcode.com/uploads/2019/10/01/untitled-diagram-2.jpg)

Input: queens = [[5,6],[7,7],[2,1],[0,7],[1,6],[5,1],[3,7],[0,3],[4,0],[1,2],[6,3],[5,0],[0,4],[2,2],[1,1],[6,4],[5,4],[0,0],[2,6],[4,5],[5,2],[1,4],[7,5],[2,3],[0,5],[4,2],[1,0],[2,7],[0,1],[4,6],[6,1],[0,6],[4,3],[1,7]], king = [3,4]
Output: [[2,3],[1,4],[1,6],[3,7],[4,3],[5,4],[4,5]]

__Constraints:__

1 <= queens.length <= 63
queens[i].length == 2
0 <= queens[i][j] < 8
king.length == 2
0 <= king[0], king[1] < 8
At most one piece is allowed in a cell.

__题目描述：__

在一个 8x8 的棋盘上，放置着若干「黑皇后」和一个「白国王」。

给定一个由整数坐标组成的数组 queens ，表示黑皇后的位置；以及一对坐标 king ，表示白国王的位置，返回所有可以攻击国王的皇后的坐标(任意顺序)。

__示例：__

示例 1：

![棋盘 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/13/untitled-diagram.jpg)

输入：queens = [[0,1],[1,0],[4,0],[0,4],[3,3],[2,4]], king = [0,0]
输出：[[0,1],[1,0],[3,3]]
解释：
[0,1] 的皇后可以攻击到国王，因为他们在同一行上。
[1,0] 的皇后可以攻击到国王，因为他们在同一列上。
[3,3] 的皇后可以攻击到国王，因为他们在同一条对角线上。
[0,4] 的皇后无法攻击到国王，因为她被位于 [0,1] 的皇后挡住了。
[4,0] 的皇后无法攻击到国王，因为她被位于 [1,0] 的皇后挡住了。
[2,4] 的皇后无法攻击到国王，因为她和国王不在同一行/列/对角线上。

示例 2：

![棋盘 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/13/untitled-diagram-1.jpg)

输入：queens = [[0,0],[1,1],[2,2],[3,4],[3,5],[4,4],[4,5]], king = [3,3]
输出：[[2,2],[3,4],[4,4]]

示例 3：

![棋盘 3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/13/untitled-diagram-2.jpg)

输入：queens = [[5,6],[7,7],[2,1],[0,7],[1,6],[5,1],[3,7],[0,3],[4,0],[1,2],[6,3],[5,0],[0,4],[2,2],[1,1],[6,4],[5,4],[0,0],[2,6],[4,5],[5,2],[1,4],[7,5],[2,3],[0,5],[4,2],[1,0],[2,7],[0,1],[4,6],[6,1],[0,6],[4,3],[1,7]], king = [3,4]
输出：[[2,3],[1,4],[1,6],[3,7],[4,3],[5,4],[4,5]]

__提示：__

1 <= queens.length <= 63
queens[i].length == 2
0 <= queens[i][j] < 8
king.length == 2
0 <= king[0], king[1] < 8
一个棋盘格上最多只能放置一枚棋子。

__思路：__

DFS
转化为用国王攻击皇后
国王从 8 个方向分别搜索
碰到第一个可以攻击的皇后即停止, 并将该皇后的位置加入结果中
时间复杂度为 O(1), 空间复杂度为 O(1), 最多搜索 32 个位置即可得到结果

__代码__:

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> queensAttacktheKing(vector<vector<int>>& queens, vector<int>& king) 
    {
        vector<vector<int>> result, board(8, vector<int>(8));
        int dx[] = {-1, -1, -1, 0, 1, 1, 1, 0}, dy[] = {-1, 0, 1, 1, 1, 0,-1,-1};
        for (const auto& queen : queens) board[queen.front()][queen.back()] = 1;
        for (int i = 0; i < 8; i++) 
        {
            int x = king.front() + dx[i], y = king.back() + dy[i];
            while (x > -1 and x < 8 and y > -1 and y < 8) 
            {
                if (board[x][y]) 
                {
                    result.emplace_back(vector<int>{x, y});
                    break;
                }
                x += dx[i];
                y += dy[i];
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> queensAttacktheKing(int[][] queens, int[] king) {
        List<List<Integer>> result = new ArrayList<>();
        int dx[] = new int[]{-1, -1, -1, 0, 1, 1, 1, 0}, dy[] = new int[]{-1, 0, 1, 1, 1, 0,-1,-1}, board[][] = new int[8][8];
        for (int[] queen : queens) board[queen[0]][queen[1]] = 1;
        for (int i = 0; i < 8; i++) {
            int x = king[0] + dx[i], y = king[1] + dy[i];
            while (x > -1 && x < 8 && y > -1 && y < 8) {
                if (board[x][y] == 1) {
                    result.add(Arrays.asList(x, y));
                    break;
                }
                x += dx[i];
                y += dy[i];
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def queensAttacktheKing(self, queens: List[List[int]], king: List[int]) -> List[List[int]]:
        board, result = [[0] * 8 for _ in range(8)], []
        for i, j in queens:
            board[i][j] = 1
        for x in range(-1, 2):
            for y in range(-1, 2):
                if x or y:
                    i, j = king
                    while True:
                        i, j = i + x, j + y
                        if not (-1 < i < 8 and -1 < j < 8):
                            break
                        if board[i][j]:
                            result.append([i, j])
                            break
        return result
```
