# 655 Print Binary Tree 输出二叉树

__Description__:
Print a binary tree in an m x n 2D string array following these rules:

The row numbers m should be equal to the height of the given binary tree.
The column number n should always be an odd number.
The root node's value (in string format) should be put in the exact middle of the first row it can be put. The column and the row where the root node belongs will separate the rest space into two parts (left-bottom part and right-bottom part). You should print the left subtree in the left-bottom part and print the right subtree in the right-bottom part. The left-bottom part and the right-bottom part should have the same size. Even if one subtree is none while the other is not, you don't need to print anything for the none subtree but still need to leave the space as large as that for the other subtree. However, if two subtrees are none, then you don't need to leave space for both of them.
Each unused space should contain an empty string "".
Print the subtrees following the same rules.

__Example:__

Example 1:

![print-tree 1](https://assets.leetcode.com/uploads/2021/05/03/print1-tree.jpg)

Input: root = [1,2]
Output:

```text
[["","1",""],
 ["2","",""]]
```

Example 2:

![print-tree 2](https://assets.leetcode.com/uploads/2021/05/03/print2-tree.jpg)

Input: root = [1,2,3,null,4]
Output:

```text
[["","","","1","","",""],
 ["","2","","","","3",""],
 ["","","4","","","",""]]
```

__Constraints:__

The number of nodes in the tree is in the range [1, 2^10].
-99 <= Node.val <= 99
The depth of the tree will be in the range [1, 10].

__题目描述__:
在一个 m*n 的二维字符串数组中输出二叉树，并遵守以下规则：

行数 m 应当等于给定二叉树的高度。
列数 n 应当总是奇数。
根节点的值（以字符串格式给出）应当放在可放置的第一行正中间。根节点所在的行与列会将剩余空间划分为两部分（左下部分和右下部分）。你应该将左子树输出在左下部分，右子树输出在右下部分。左下和右下部分应当有相同的大小。即使一个子树为空而另一个非空，你不需要为空的子树输出任何东西，但仍需要为另一个子树留出足够的空间。然而，如果两个子树都为空则不需要为它们留出任何空间。
每个未使用的空间应包含一个空的字符串""。
使用相同的规则输出子树。

__示例 :__

示例 1:

输入:

```text
     1
    /
   2
```

输出:

```text
[["", "1", ""],
 ["2", "", ""]]
```

示例 2:

输入:

```text
     1
    / \
   2   3
    \
     4
```

输出:

```text
[["", "", "", "1", "", "", ""],
 ["", "2", "", "", "", "3", ""],
 ["", "", "4", "", "", "", ""]]
```

示例 3:

输入:

```text
      1
     / \
    2   5
   / 
  3 
 / 
4 
```

输出:

```text
[["",  "",  "", "",  "", "", "", "1", "",  "",  "",  "",  "", "", ""]
 ["",  "",  "", "2", "", "", "", "",  "",  "",  "",  "5", "", "", ""]
 ["",  "3", "", "",  "", "", "", "",  "",  "",  "",  "",  "", "", ""]
 ["4", "",  "", "",  "", "", "", "",  "",  "",  "",  "",  "", "", ""]]
```

__注意:__
二叉树的高度在范围 [1, 10] 中。

__思路__:

递归
由观察可得, 每次将根放置在对应区间的中点
按深度将左右子树分别放置在数组对应部分的终点即可
时间复杂度 O(h \* 2 ^ h), 空间复杂度 O(h \* 2 ^ h)

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
public:
    vector<vector<string>> printTree(TreeNode* root) 
    {
        int m = height(root);
        int n = (1 << m) - 1;
        vector<vector<string>> result(m, vector<string>(n, ""));
        helper(result, root, 0, 0, n);
        return result;
    }
private:
    void helper(vector<vector<string>> &result, TreeNode *root, int depth, int left, int right) 
    {
        if (!root) return;
        int mid = (left + right) >> 1;
        result[depth][mid] = to_string(root -> val);
        helper(result, root -> left, depth + 1, left, mid);
        helper(result, root -> right, depth + 1, mid, right);
    }
    
    int height(TreeNode *root) 
    {
        return !root ? 0 : 1 + max(height(root -> left), height(root -> right));
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
public class Solution {
    public List<List<String>> printTree(TreeNode root) {
        int m = height(root);
        int n = (1 << m) - 1;
        String[][] tree = new String[m][n];
        for (String[] line : tree) Arrays.fill(line, "");
        List<List<String>> result = new ArrayList<>();
        helper(tree, root, 0, 0, n);
        for (String[] line : tree) result.add(Arrays.asList(line));
        return result;
    }
    
    private void helper(String[][] result, TreeNode root, int depth, int left, int right) {
        if (root == null) return;
        int mid = (left + right) >> 1;
        result[depth][mid] = String.valueOf(root.val);
        helper(result, root.left, depth + 1, left, mid);
        helper(result, root.right, depth + 1, mid, right);
    }
    
    private int height(TreeNode root) {
        return root == null ? 0 : 1 + Math.max(height(root.left), height(root.right));
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
    def printTree(self, root: TreeNode) -> List[List[str]]:
        
        def height(cur: TreeNode) -> int:
            return max(height(cur.left), height(cur.right)) + 1 if cur else 0
        
        def helper(cur: TreeNode, depth: int, left: int, right: int) -> None:
            if not cur:
                return
            result[depth][(mid := ((left + right) >> 1))] = str(cur.val)
            helper(cur.left, depth + 1, left, mid)
            helper(cur.right, depth + 1, mid, right)
            
        m = height(root)
        n = (1 << m) - 1
        result = [[''] * n for _ in range(m)]
        helper(root, 0, 0, n)
        return result
```
