# 1476 Subrectangle Queries 子矩形查询

__Description:__

Implement the class  `SubrectangleQueries` which receives a  `rows x cols` rectangle as a matrix of integers in the constructor and supports two methods:

1. `updateSubrectangle(int row1, int col1, int row2, int col2, int newValue)`

    - Updates all values with  `newValue` in the subrectangle whose upper left coordinate is  `(row1,col1)` and bottom right coordinate is  `(row2,col2)`.

2. `getValue(int row, int col)`

    - Returns the current value of the coordinate  `(row,col)` from the rectangle.

__Example:__

Example 1:

```text
Input
["SubrectangleQueries","getValue","updateSubrectangle","getValue","getValue","updateSubrectangle","getValue","getValue"]
[[[[1,2,1],[4,3,4],[3,2,1],[1,1,1]]],[0,2],[0,0,3,2,5],[0,2],[3,1],[3,0,3,2,10],[3,1],[0,2]]
Output
[null,1,null,5,5,null,10,5]
Explanation
SubrectangleQueries subrectangleQueries = new SubrectangleQueries([[1,2,1],[4,3,4],[3,2,1],[1,1,1]]);  
// The initial rectangle (4x3) looks like:
// 1 2 1
// 4 3 4
// 3 2 1
// 1 1 1
subrectangleQueries.getValue(0, 2); // return 1
subrectangleQueries.updateSubrectangle(0, 0, 3, 2, 5);
// After this update the rectangle looks like:
// 5 5 5
// 5 5 5
// 5 5 5
// 5 5 5 
subrectangleQueries.getValue(0, 2); // return 5
subrectangleQueries.getValue(3, 1); // return 5
subrectangleQueries.updateSubrectangle(3, 0, 3, 2, 10);
// After this update the rectangle looks like:
// 5   5   5
// 5   5   5
// 5   5   5
// 10  10  10 
subrectangleQueries.getValue(3, 1); // return 10
subrectangleQueries.getValue(0, 2); // return 5
```

Example 2:

```text
Input
["SubrectangleQueries","getValue","updateSubrectangle","getValue","getValue","updateSubrectangle","getValue"]
[[[[1,1,1],[2,2,2],[3,3,3]]],[0,0],[0,0,2,2,100],[0,0],[2,2],[1,1,2,2,20],[2,2]]
Output
[null,1,null,100,100,null,20]
Explanation
SubrectangleQueries subrectangleQueries = new SubrectangleQueries([[1,1,1],[2,2,2],[3,3,3]]);
subrectangleQueries.getValue(0, 0); // return 1
subrectangleQueries.updateSubrectangle(0, 0, 2, 2, 100);
subrectangleQueries.getValue(0, 0); // return 100
subrectangleQueries.getValue(2, 2); // return 100
subrectangleQueries.updateSubrectangle(1, 1, 2, 2, 20);
subrectangleQueries.getValue(2, 2); // return 20
```

__Constraints:__

- There will be at most  `500` operations considering both methods:  `updateSubrectangle` and  `getValue`.
- `1 <= rows, cols <= 100`
- `rows == rectangle.length`
- `cols == rectangle[i].length`
- `0 <= row1 <= row2 < rows`
- `0 <= col1 <= col2 < cols`
- `1 <= newValue, rectangle[i][j] <= 10 ^ 9`
- `0 <= row < rows`
- `0 <= col < cols`

__题目描述:__

请你实现一个类  `SubrectangleQueries` ，它的构造函数的参数是一个  `rows x cols` 的矩形（这里用整数矩阵表示），并支持以下两种操作：

1. `updateSubrectangle(int row1, int col1, int row2, int col2, int newValue)`

    - 用  `newValue` 更新以  `(row1,col1)` 为左上角且以  `(row2,col2)` 为右下角的子矩形。

2. `getValue(int row, int col)`

    - 返回矩形中坐标  `(row,col)` 的当前值。

__示例:__

示例 1：

```text
输入：
["SubrectangleQueries","getValue","updateSubrectangle","getValue","getValue","updateSubrectangle","getValue","getValue"]
[[[[1,2,1],[4,3,4],[3,2,1],[1,1,1]]],[0,2],[0,0,3,2,5],[0,2],[3,1],[3,0,3,2,10],[3,1],[0,2]]
输出：
[null,1,null,5,5,null,10,5]
解释：
SubrectangleQueries subrectangleQueries = new SubrectangleQueries([[1,2,1],[4,3,4],[3,2,1],[1,1,1]]);  
// 初始的 (4x3) 矩形如下：
// 1 2 1
// 4 3 4
// 3 2 1
// 1 1 1
subrectangleQueries.getValue(0, 2); // 返回 1
subrectangleQueries.updateSubrectangle(0, 0, 3, 2, 5);
// 此次更新后矩形变为：
// 5 5 5
// 5 5 5
// 5 5 5
// 5 5 5 
subrectangleQueries.getValue(0, 2); // 返回 5
subrectangleQueries.getValue(3, 1); // 返回 5
subrectangleQueries.updateSubrectangle(3, 0, 3, 2, 10);
// 此次更新后矩形变为：
// 5   5   5
// 5   5   5
// 5   5   5
// 10  10  10 
subrectangleQueries.getValue(3, 1); // 返回 10
subrectangleQueries.getValue(0, 2); // 返回 5
```

示例 2：

```text
输入：
["SubrectangleQueries","getValue","updateSubrectangle","getValue","getValue","updateSubrectangle","getValue"]
[[[[1,1,1],[2,2,2],[3,3,3]]],[0,0],[0,0,2,2,100],[0,0],[2,2],[1,1,2,2,20],[2,2]]
输出：
[null,1,null,100,100,null,20]
解释：
SubrectangleQueries subrectangleQueries = new SubrectangleQueries([[1,1,1],[2,2,2],[3,3,3]]);
subrectangleQueries.getValue(0, 0); // 返回 1
subrectangleQueries.updateSubrectangle(0, 0, 2, 2, 100);
subrectangleQueries.getValue(0, 0); // 返回 100
subrectangleQueries.getValue(2, 2); // 返回 100
subrectangleQueries.updateSubrectangle(1, 1, 2, 2, 20);
subrectangleQueries.getValue(2, 2); // 返回 20
```

__提示：__

- 最多有  `500` 次 `updateSubrectangle` 和  `getValue` 操作。
- `1 <= rows, cols <= 100`
- `rows == rectangle.length`
- `cols == rectangle[i].length`
- `0 <= row1 <= row2 < rows`
- `0 <= col1 <= col2 < cols`
- `1 <= newValue, rectangle[i][j] <= 10 ^ 9`
- `0 <= row < rows`
- `0 <= col < cols`

__思路:__

```text
1. 暴力法
每次直接更新原矩阵
updateSubrectangle()
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N ^ 2)
getValue()
时间复杂度为 O(1), 空间复杂度为 O(1)
2. 快照
记录下每次更新的范围
从后往前遍历更新范围
如果该坐标在更新范围内直接返回历史值
否则返回原始值
updateSubrectangle()
时间复杂度为 O(1), 空间复杂度为 O(M)
getValue()
时间复杂度为 O(M), 空间复杂度为 O(1)
其中 N 为矩阵的大小, M 为调用 updateSubrectangle() 的次数
```

__代码:__

__C++__:

```C++
class SubrectangleQueries 
{
private:
    vector<vector<int>> rectangle;
    vector<vector<int>> history;
public:
    SubrectangleQueries(vector<vector<int>>& rectangle) : rectangle(rectangle) {}
    
    void updateSubrectangle(int row1, int col1, int row2, int col2, int newValue) 
    {
        history.push_back({row1, col1, row2, col2, newValue});
    }

    int getValue(int row, int col) 
    {
        for (auto i = history.rbegin(); i != history.rend(); i++) if ((*i)[0] <= row && row <= (*i)[2] && (*i)[1] <= col && col <= (*i)[3]) return (*i)[4];
        return rectangle[row][col];
    }
};

/**
 * Your SubrectangleQueries object will be instantiated and called as such:
 * SubrectangleQueries* obj = new SubrectangleQueries(rectangle);
 * obj->updateSubrectangle(row1,col1,row2,col2,newValue);
 * int param_2 = obj->getValue(row,col);
 */
```

__Java__:

```Java
class SubrectangleQueries {
    private int[][] rectangle;

    public SubrectangleQueries(int[][] rectangle) {
        this.rectangle = rectangle;
    }
    
    public void updateSubrectangle(int row1, int col1, int row2, int col2, int newValue) {
        for (int i = row1; i <= row2; i++) for (int j = col1; j <= col2; j++) rectangle[i][j] = newValue;
    }
    
    public int getValue(int row, int col) {
        return rectangle[row][col];
    }
}

/**
 * Your SubrectangleQueries object will be instantiated and called as such:
 * SubrectangleQueries obj = new SubrectangleQueries(rectangle);
 * obj.updateSubrectangle(row1,col1,row2,col2,newValue);
 * int param_2 = obj.getValue(row,col);
 */
```

__Python__:

```Python
import numpy as np
class SubrectangleQueries:
    
    def __init__(self, rectangle: List[List[int]]):
        self.matrix = np.array(rectangle)


    def updateSubrectangle(self, row1: int, col1: int, row2: int, col2: int, newValue: int) -> None:
        self.matrix[row1:row2 + 1, col1:col2 + 1] = newValue


    def getValue(self, row: int, col: int) -> int:
        return int(self.matrix[row, col])


# Your SubrectangleQueries object will be instantiated and called as such:
# obj = SubrectangleQueries(rectangle)
# obj.updateSubrectangle(row1,col1,row2,col2,newValue)
# param_2 = obj.getValue(row,col)
```
