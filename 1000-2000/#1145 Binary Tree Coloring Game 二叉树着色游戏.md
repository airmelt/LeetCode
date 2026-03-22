# 1145 Binary Tree Coloring Game 二叉树着色游戏

__Description__:
Two players play a turn based game on a binary tree. We are given the root of this binary tree, and the number of nodes n in the tree. n is odd, and each node has a distinct value from 1 to n.

Initially, the first player names a value x with 1 <= x <= n, and the second player names a value y with 1 <= y <= n and y != x. The first player colors the node with value x red, and the second player colors the node with value y blue.

Then, the players take turns starting with the first player. In each turn, that player chooses a node of their color (red if player 1, blue if player 2) and colors an uncolored neighbor of the chosen node (either the left child, right child, or parent of the chosen node.)

If (and only if) a player cannot choose such a node in this way, they must pass their turn. If both players pass their turn, the game ends, and the winner is the player that colored more nodes.

You are the second player. If it is possible to choose such a y to ensure you win the game, return true. If it is not possible, return false.

__Example:__

Example 1:

![Binary Tree](https://assets.leetcode.com/uploads/2019/08/01/1480-binary-tree-coloring-game.png)

Input: root = [1,2,3,4,5,6,7,8,9,10,11], n = 11, x = 3
Output: true
Explanation: The second player can choose the node with value 2.

Example 2:

Input: root = [1,2,3], n = 3, x = 1
Output: false

__Constraints:__

The number of nodes in the tree is n.
1 <= x <= n <= 100
n is odd.
1 <= Node.val <= n
All the values of the tree are unique.

__题目描述__:
有两位极客玩家参与了一场「二叉树着色」的游戏。游戏中，给出二叉树的根节点 root，树上总共有 n 个节点，且 n 为奇数，其中每个节点上的值从 1 到 n 各不相同。

游戏从「一号」玩家开始（「一号」玩家为红色，「二号」玩家为蓝色），最开始时，

「一号」玩家从 [1, n] 中取一个值 x（1 <= x <= n）；

「二号」玩家也从 [1, n] 中取一个值 y（1 <= y <= n）且 y != x。

「一号」玩家给值为 x 的节点染上红色，而「二号」玩家给值为 y 的节点染上蓝色。

之后两位玩家轮流进行操作，每一回合，玩家选择一个他之前涂好颜色的节点，将所选节点一个 未着色 的邻节点（即左右子节点、或父节点）进行染色。

如果当前玩家无法找到这样的节点来染色时，他的回合就会被跳过。

若两个玩家都没有可以染色的节点时，游戏结束。着色节点最多的那位玩家获得胜利 ✌️。

现在，假设你是「二号」玩家，根据所给出的输入，假如存在一个 y 值可以确保你赢得这场游戏，则返回 true；若无法获胜，就请返回 false。

__示例 :__

![二叉树](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/08/04/1480-binary-tree-coloring-game.png)

输入：root = [1,2,3,4,5,6,7,8,9,10,11], n = 11, x = 3
输出：True
解释：第二个玩家可以选择值为 2 的节点。

__提示:__

二叉树的根节点为 root，树上由 n 个节点，节点上的值从 1 到 n 各不相同。
n 为奇数。
1 <= x <= n <= 100

__思路__:

模拟
要么将所有奇数位置减少到比相邻偶数位置上的数少 1
要么将所有偶数位置减少到比奇数相邻位置上的数少 1
除了两端需要单独考虑
分别求出奇数位置上和偶数位置上需要改变的数之和取较小的即可
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution 
{
private:
    int max_value = 0;
    
    int dfs(TreeNode* root, int x, int n)
    {
        if (!root) return 0;
        int l = dfs(root -> left, x, n), r = dfs(root -> right, x, n);
        if (root -> val == x) max_value = max({max_value, n - l - r - 1, l, r});
        return l + r + 1;
    }
public:
    bool btreeGameWinningMove(TreeNode* root, int n, int x) 
    {
        dfs(root, x, n);
        return max_value > n - max_value;
    }
};
```

__Java__:

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    private int max = 0;

    private int dfs(TreeNode root, int x, int n) {
        if (root == null) return 0;
        int l = dfs(root.left, x, n), r = dfs(root.right, x, n);
        if (root.val == x) max = Math.max(max, Math.max(n - l - r - 1, Math.max(l, r)));
        return l + r + 1;
    }

    public boolean btreeGameWinningMove(TreeNode root, int n, int x) {
        dfs(root, x, n);
        return max > n - max;
    }
}
```

__Python__:

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def btreeGameWinningMove(self, root: Optional[TreeNode], n: int, x: int) -> bool:
        max_value = 0
        
        def dfs(cur: Optional[TreeNode]) -> int:
            nonlocal max_value
            if not cur:
                return 0
            left, right = dfs(cur.left), dfs(cur.right)
            if cur.val == x:
                max_value = max([max_value, left, right, n - left - right - 1])
            return left + right + 1
        dfs(root)
        return max_value > n - max_value
```
