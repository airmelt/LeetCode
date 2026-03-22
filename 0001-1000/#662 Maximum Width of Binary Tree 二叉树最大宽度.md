# 662 Maximum Width of Binary Tree 二叉树最大宽度

__Description__:
Given the root of a binary tree, return the maximum width of the given tree.

The maximum width of a tree is the maximum width among all levels.

The width of one level is defined as the length between the end-nodes (the leftmost and rightmost non-null nodes), where the null nodes between the end-nodes are also counted into the length calculation.

It is guaranteed that the answer will in the range of 32-bit signed integer.

__Example:__

Example 1:

![width1-tree](https://assets.leetcode.com/uploads/2021/05/03/width1-tree.jpg)

Input: root = [1,3,2,5,3,null,9]
Output: 4
Explanation: The maximum width existing in the third level with the length 4 (5,3,null,9).

Example 2:

![width1-tree](https://assets.leetcode.com/uploads/2021/05/03/width2-tree.jpg)

Input: root = [1,3,null,5,3]
Output: 2
Explanation: The maximum width existing in the third level with the length 2 (5,3).

Example 3:

![width3-tree](https://assets.leetcode.com/uploads/2021/05/03/width3-tree.jpg)

Input: root = [1,3,2,5]
Output: 2
Explanation: The maximum width existing in the second level with the length 2 (3,2).

Example 4:

![width4-tree](https://assets.leetcode.com/uploads/2021/05/03/width4-tree.jpg)

Input: root = [1,3,2,5,null,null,9,6,null,null,7]
Output: 8
Explanation: The maximum width existing in the fourth level with the length 8 (6,null,null,null,null,null,null,7).

__Constraints:__

The number of nodes in the tree is in the range [1, 3000].
-100 <= Node.val <= 100

__题目描述__:
给定一个二叉树，编写一个函数来获取这个树的最大宽度。树的宽度是所有层中的最大宽度。这个二叉树与满二叉树（full binary tree）结构相同，但一些节点为空。

每一层的宽度被定义为两个端点（该层最左和最右的非空节点，两端点间的null节点也计入长度）之间的长度。

__示例 :__

示例 1:

输入:

```text
           1
         /   \
        3     2
       / \     \  
      5   3     9 
```

输出: 4
解释: 最大值出现在树的第 3 层，宽度为 4 (5,3,null,9)。

示例 2:

输入:

```text
          1
         /  
        3    
       / \       
      5   3     
```

输出: 2
解释: 最大值出现在树的第 3 层，宽度为 2 (5,3)。

示例 3:

输入:

```text
          1
         / \
        3   2 
       /        
      5      
```

输出: 2
解释: 最大值出现在树的第 2 层，宽度为 2 (3,2)。

示例 4:

输入:

```text
          1
         / \
        3   2
       /     \  
      5       9 
     /         \
    6           7
```

输出: 8
解释: 最大值出现在树的第 4 层，宽度为 8 (6,null,null,null,null,null,null,7)。

__注意:__
答案在32位有符号整数的表示范围内。

__思路__:

广度优先遍历
整体上还是层序遍历的模板
需要按照完全二叉树记录节点的下标
为了防止溢出, 可以记录节点下标相对于该层第一个节点的下标
时间复杂度 O(n), 空间复杂度 O(n)

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
    int widthOfBinaryTree(TreeNode* root)
    {
        queue<pair<TreeNode*, int>> q;
        q.push({root, 0});
        int left = 0, right = 0, result = 0;
        while (!q.empty()) 
        {
            int s = q.size(), left = q.front().second, right = q.back().second;
            for (int i = 0; i < s; i++) 
            {
                auto cur = q.front();
                q.pop();
                if (cur.first -> left) q.push({cur.first -> left, (cur.second << 1) - (left << 1)});
                if (cur.first -> right) q.push({cur.first -> right, (cur.second << 1) + 1 - (left << 1)});
            }
            result = max(result, right - left + 1);
        }
        return result;
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
    public int widthOfBinaryTree(TreeNode root) {
        Queue<Node> queue = new LinkedList();
        queue.add(new Node(root, 0));
        int left = 0, right = 0, result = 0;
        while (!queue.isEmpty()) {
            int s = queue.size();
            for (int i = 0; i < s; i++) {
                Node cur = queue.poll();
                if (i == 0) left = cur.pos;
                if (i == s - 1) right = cur.pos;
                if (cur.node.left != null) queue.offer(new Node(cur.node.left, cur.pos * 2));
                if (cur.node.right != null) queue.offer(new Node(cur.node.right, cur.pos * 2 + 1));
            }
            result = Math.max(result, right - left + 1);
        }
        return result;
    }
}

class Node {
    TreeNode node;
    int pos;
    Node(TreeNode n, int p) {
        node = n;
        pos = p;
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
    def widthOfBinaryTree(self, root: TreeNode) -> int:
        result, queue = 0, [(root, 0)]
        while queue:
            s, result = len(queue), max(result, queue[-1][1] - queue[0][1] + 1)
            for _ in range(s):
                cur = queue.pop(0)
                (cur[0].left and queue.append((cur[0].left, cur[1] * 2))) or (cur[0].right and queue.append((cur[0].right, cur[1] * 2 + 1)))
        return result
```
