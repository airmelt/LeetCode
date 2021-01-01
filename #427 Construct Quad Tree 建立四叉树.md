# 427 Construct Quad Tree 建立四叉树

__Description__:
Given a n * n matrix grid of 0's and 1's only. We want to represent the grid with a Quad-Tree.

Return the root of the Quad-Tree representing the grid.

Notice that you can assign the value of a node to True or False when isLeaf is False, and both are accepted in the answer.

A Quad-Tree is a tree data structure in which each internal node has exactly four children. Besides, each node has two attributes:

val: True if the node represents a grid of 1's or False if the node represents a grid of 0's.
isLeaf: True if the node is leaf node on the tree or False if the node has the four children.

```C
class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;
}
```

We can construct a Quad-Tree from a two-dimensional area using the following steps:

If the current grid has the same value (i.e all 1's or all 0's) set isLeaf True and set val to the value of the grid and set the four children to Null and stop.
If the current grid has different values, set isLeaf to False and set val to any value and divide the current grid into four sub-grids as shown in the photo.
Recurse for each of the children with the proper sub-grid.

If you want to know more about the Quad-Tree, you can refer to the [wiki](https://en.wikipedia.org/wiki/Quadtree).

Quad-Tree format:

The output represents the serialized format of a Quad-Tree using level order traversal, where null signifies a path terminator where no node exists below.

It is very similar to the serialization of the binary tree. The only difference is that the node is represented as a list [isLeaf, val].

If the value of isLeaf or val is True we represent it as 1 in the list [isLeaf, val] and if the value of isLeaf or val is False we represent it as 0.

__Example:__

Example 1:
![grid 1](https://upload-images.jianshu.io/upload_images/16639143-9e99c1aa0fd6f3c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Input: grid = [[0,1],[1,0]]
Output: [[0,1],[1,0],[1,1],[1,1],[1,0]]
Explanation: The explanation of this example is shown below:
![Explanation 1](https://upload-images.jianshu.io/upload_images/16639143-e353d01cbeb90274.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Notice that 0 represnts False and 1 represents True in the photo representing the Quad-Tree.

Example 2:
![grid 2](https://upload-images.jianshu.io/upload_images/16639143-e2a56481328f218c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Input: grid = [[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,1,1,1,1],[1,1,1,1,1,1,1,1],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0]]
Output: [[0,1],[1,1],[0,1],[1,1],[1,0],null,null,null,null,[1,0],[1,0],[1,1],[1,1]]
Explanation: All values in the grid are not the same. We divide the grid into four sub-grids.
The topLeft, bottomLeft and bottomRight each has the same value.
The topRight have different values so we divide it into 4 sub-grids where each has the same value.
Explanation is shown in the photo below:
![Explanation 2](https://upload-images.jianshu.io/upload_images/16639143-7ca5045b297f330b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Example 3:

Input: grid = [[1,1],[1,1]]
Output: [[1,1]]

Example 4:

Input: grid = [[0]]
Output: [[1,0]]

Example 5:

Input: grid = [[1,1,0,0],[1,1,0,0],[0,0,1,1],[0,0,1,1]]
Output: [[0,1],[1,1],[1,0],[1,0],[1,1]]

__Constraints:__

n == grid.length == grid[i].length
n == 2^x where 0 <= x <= 6

__题目描述__:
给你一个 n * n 矩阵 grid ，矩阵由若干 0 和 1 组成。请你用四叉树表示该矩阵 grid 。

你需要返回能表示矩阵的 四叉树 的根结点。

注意，当 isLeaf 为 False 时，你可以把 True 或者 False 赋值给节点，两种值都会被判题机制 接受 。

四叉树数据结构中，每个内部节点只有四个子节点。此外，每个节点都有两个属性：

val：储存叶子结点所代表的区域的值。1 对应 True，0 对应 False；
isLeaf: 当这个节点是一个叶子结点时为 True，如果它有 4 个子节点则为 False 。

```C
class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;
}
```

我们可以按以下步骤为二维区域构建四叉树：

如果当前网格的值相同（即，全为 0 或者全为 1），将 isLeaf 设为 True ，将 val 设为网格相应的值，并将四个子节点都设为 Null 然后停止。
如果当前网格的值不同，将 isLeaf 设为 False， 将 val 设为任意值，然后如下图所示，将当前网格划分为四个子网格。
使用适当的子网格递归每个子节点。

如果你想了解更多关于四叉树的内容，可以参考 [wiki](https://en.wikipedia.org/wiki/Quadtree) 。

四叉树格式：

输出为使用层序遍历后四叉树的序列化形式，其中 null 表示路径终止符，其下面不存在节点。

它与二叉树的序列化非常相似。唯一的区别是节点以列表形式表示 [isLeaf, val] 。

如果 isLeaf 或者 val 的值为 True ，则表示它在列表 [isLeaf, val] 中的值为 1 ；如果 isLeaf 或者 val 的值为 False ，则表示值为 0 。

__示例 :__

示例 1：
![grid 1](https://upload-images.jianshu.io/upload_images/16639143-49165b3ccb6c6d97.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
输入：grid = [[0,1],[1,0]]
输出：[[0,1],[1,0],[1,1],[1,1],[1,0]]
解释：此示例的解释如下：
![Explanation 1](https://upload-images.jianshu.io/upload_images/16639143-6bb72c9521deb6a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
请注意，在下面四叉树的图示中，0 表示 false，1 表示 True 。

示例 2：
![grid 2](https://upload-images.jianshu.io/upload_images/16639143-1a48d72714dffd26.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
输入：grid = [[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,1,1,1,1],[1,1,1,1,1,1,1,1],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0]]
输出：[[0,1],[1,1],[0,1],[1,1],[1,0],null,null,null,null,[1,0],[1,0],[1,1],[1,1]]
解释：网格中的所有值都不相同。我们将网格划分为四个子网格。
topLeft，bottomLeft 和 bottomRight 均具有相同的值。
topRight 具有不同的值，因此我们将其再分为 4 个子网格，这样每个子网格都具有相同的值。
解释如下图所示：
![Explanation 2](https://upload-images.jianshu.io/upload_images/16639143-f9ca68d1081af336.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

示例 3：

输入：grid = [[1,1],[1,1]]
输出：[[1,1]]

示例 4：

输入：grid = [[0]]
输出：[[1,0]]

示例 5：

输入：grid = [[1,1,0,0],[1,1,0,0],[0,0,1,1],[0,0,1,1]]
输出：[[0,1],[1,1],[1,0],[1,0],[1,1]]

__提示：__

n == grid.length == grid[i].length
n == 2^x 其中 0 <= x <= 6

__思路__:

按照左上, 右上, 左下和右下分为 4个区域
检查区域是否一致, 不一致则不是叶子节点
一致则生成叶子节点
将区域减小到 1 / 2(面积为原来的 1 / 4), 递归检查
时间复杂度O(n ^ 2lgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
/*
// Definition for a QuadTree node.
class Node {
public:
    bool val;
    bool isLeaf;
    Node* topLeft;
    Node* topRight;
    Node* bottomLeft;
    Node* bottomRight;
    
    Node() {
        val = false;
        isLeaf = false;
        topLeft = NULL;
        topRight = NULL;
        bottomLeft = NULL;
        bottomRight = NULL;
    }
    
    Node(bool _val, bool _isLeaf) {
        val = _val;
        isLeaf = _isLeaf;
        topLeft = NULL;
        topRight = NULL;
        bottomLeft = NULL;
        bottomRight = NULL;
    }
    
    Node(bool _val, bool _isLeaf, Node* _topLeft, Node* _topRight, Node* _bottomLeft, Node* _bottomRight) {
        val = _val;
        isLeaf = _isLeaf;
        topLeft = _topLeft;
        topRight = _topRight;
        bottomLeft = _bottomLeft;
        bottomRight = _bottomRight;
    }
};
*/

class Solution 
{
public:
    Node* construct(vector<vector<int>>& grid) 
    {
        return quad_tree(grid, 0, 0, grid.size());
    }
private:
    Node* quad_tree(vector<vector<int>>& grid, int x, int y, int offset)
    {
        for (int i = x; i < x + offset; i++) for (int j = y; j < y + offset; j++) if (grid[i][j] != grid[x][y]) return new Node(true, false, quad_tree(grid, x, y, offset >> 1), quad_tree(grid, x, y + (offset >> 1), offset >> 1), quad_tree(grid, x + (offset >> 1), y, offset >> 1), quad_tree(grid, x + (offset >> 1), y + (offset >> 1), offset >> 1));
        return new Node(grid[x][y], true);
    }
};
```

__Java__:

```Java
/*
// Definition for a QuadTree node.
class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;

    
    public Node() {
        this.val = false;
        this.isLeaf = false;
        this.topLeft = null;
        this.topRight = null;
        this.bottomLeft = null;
        this.bottomRight = null;
    }
    
    public Node(boolean val, boolean isLeaf) {
        this.val = val;
        this.isLeaf = isLeaf;
        this.topLeft = null;
        this.topRight = null;
        this.bottomLeft = null;
        this.bottomRight = null;
    }
    
    public Node(boolean val, boolean isLeaf, Node topLeft, Node topRight, Node bottomLeft, Node bottomRight) {
        this.val = val;
        this.isLeaf = isLeaf;
        this.topLeft = topLeft;
        this.topRight = topRight;
        this.bottomLeft = bottomLeft;
        this.bottomRight = bottomRight;
    }
};
*/

class Solution {
    public Node construct(int[][] grid) {
        return quadTree(grid, 0, 0, grid.length);
    }

    private Node quadTree(int[][] grid, int x, int y, int offset) {
        for (int i = x; i < x + offset; i++) for (int j = y; j < y + offset; j++) if (grid[x][y] != grid[i][j]) return new Node(true, false, quadTree(grid, x, y, offset >> 1), quadTree(grid, x, y + (offset >> 1), offset >> 1), quadTree(grid, x + (offset >> 1), y, offset >> 1), quadTree(grid, x + (offset >> 1), y + (offset >> 1), offset >> 1));
        return new Node(grid[x][y] == 1, true);
    }
}
```

__Python__:

```Python
"""
# Definition for a QuadTree node.
class Node:
    def __init__(self, val, isLeaf, topLeft, topRight, bottomLeft, bottomRight):
        self.val = val
        self.isLeaf = isLeaf
        self.topLeft = topLeft
        self.topRight = topRight
        self.bottomLeft = bottomLeft
        self.bottomRight = bottomRight
"""

class Solution:
    def construct(self, grid: List[List[int]]) -> 'Node':
        return Node(bool(s), True) if not (s := sum(map(sum, grid))) or not (m := len(grid) >> 1) or s == 4 * m * m else Node(True, False, self.construct([g[:m] for g in grid[:m]]), self.construct([g[m:] for g in grid[:m]]), self.construct([g[:m] for g in grid[m:]]), self.construct([g[m:] for g in grid[m:]]))
```
