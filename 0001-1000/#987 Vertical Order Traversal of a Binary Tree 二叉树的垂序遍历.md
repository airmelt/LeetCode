# 987 Vertical Order Traversal of a Binary Tree 二叉树的垂序遍历

__Description__:
Given the root of a binary tree, calculate the vertical order traversal of the binary tree.

For each node at position (row, col), its left and right children will be at positions (row + 1, col - 1) and (row + 1, col + 1) respectively. The root of the tree is at (0, 0).

The vertical order traversal of a binary tree is a list of top-to-bottom orderings for each column index starting from the leftmost column and ending on the rightmost column. There may be multiple nodes in the same row and same column. In such a case, sort these nodes by their values.

Return the vertical order traversal of the binary tree.

__Example:__

Example 1:

![vtree1](https://upload-images.jianshu.io/upload_images/16639143-6de3a4657c66b706.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Input: root = [3,9,20,null,null,15,7]
Output: [[9],[3,15],[20],[7]]
Explanation:
Column -1: Only node 9 is in this column.
Column 0: Nodes 3 and 15 are in this column in that order from top to bottom.
Column 1: Only node 20 is in this column.
Column 2: Only node 7 is in this column.

Example 2:

![vtree2](https://upload-images.jianshu.io/upload_images/16639143-3c3ec9974ef47488.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Input: root = [1,2,3,4,5,6,7]
Output: [[4],[2],[1,5,6],[3],[7]]
Explanation:
Column -2: Only node 4 is in this column.
Column -1: Only node 2 is in this column.
Column 0: Nodes 1, 5, and 6 are in this column.
          1 is at the top, so it comes first.
          5 and 6 are at the same position (2, 0), so we order them by their value, 5 before 6.
Column 1: Only node 3 is in this column.
Column 2: Only node 7 is in this column.

Example 3:

![vtree3](https://upload-images.jianshu.io/upload_images/16639143-95cb8057147e5f26.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Input: root = [1,2,3,4,6,5,7]
Output: [[4],[2],[1,5,6],[3],[7]]
Explanation:
This case is the exact same as example 2, but with nodes 5 and 6 swapped.
Note that the solution remains the same since 5 and 6 are in the same location and should be ordered by their values.

__Constraints:__

The number of nodes in the tree is in the range [1, 1000].
0 <= Node.val <= 1000

__题目描述__:
给你二叉树的根结点 root ，请你设计算法计算二叉树的 垂序遍历 序列。

对位于 (row, col) 的每个结点而言，其左右子结点分别位于 (row + 1, col - 1) 和 (row + 1, col + 1) 。树的根结点位于 (0, 0) 。

二叉树的 垂序遍历 从最左边的列开始直到最右边的列结束，按列索引每一列上的所有结点，形成一个按出现位置从上到下排序的有序列表。如果同行同列上有多个结点，则按结点的值从小到大进行排序。

返回二叉树的 垂序遍历 序列。

__示例 :__

示例 1：

![垂直二叉树1](https://upload-images.jianshu.io/upload_images/16639143-2fd9bb4d37bd58fc.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

输入：root = [3,9,20,null,null,15,7]
输出：[[9],[3,15],[20],[7]]
解释：
列 -1 ：只有结点 9 在此列中。
列  0 ：只有结点 3 和 15 在此列中，按从上到下顺序。
列  1 ：只有结点 20 在此列中。
列  2 ：只有结点 7 在此列中。

示例 2：

![垂直二叉树2](https://upload-images.jianshu.io/upload_images/16639143-590798cc72ee762d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

输入：root = [1,2,3,4,5,6,7]
输出：[[4],[2],[1,5,6],[3],[7]]
解释：
列 -2 ：只有结点 4 在此列中。
列 -1 ：只有结点 2 在此列中。
列  0 ：结点 1 、5 和 6 都在此列中。
          1 在上面，所以它出现在前面。
          5 和 6 位置都是 (2, 0) ，所以按值从小到大排序，5 在 6 的前面。
列  1 ：只有结点 3 在此列中。
列  2 ：只有结点 7 在此列中。

示例 3：

![垂直二叉树3](https://upload-images.jianshu.io/upload_images/16639143-467cbbc4a599cdb3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

输入：root = [1,2,3,4,6,5,7]
输出：[[4],[2],[1,5,6],[3],[7]]
解释：
这个示例实际上与示例 2 完全相同，只是结点 5 和 6 在树中的位置发生了交换。
因为 5 和 6 的位置仍然相同，所以答案保持不变，仍然按值从小到大排序。

__提示:__

树中结点数目总数在范围 [1, 1000] 内
0 <= Node.val <= 1000

__思路__:

模拟
遍历整棵树的同时记录下结点的 row, col 及 val 信息
按照 col 排序
最后按 col 输出到列表中
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

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
    void helper(TreeNode* p, int row, int col, map<int, multiset<int>>& m)
    {
        if (!p) return;
        m[col].insert(row * 10000 + p -> val);
        helper(p -> left, row + 1, col - 1, m);
        helper(p -> right, row + 1, col + 1, m);
        return;
    }
public:
    vector<vector<int>> verticalTraversal(TreeNode* root) 
    {
        map<int, multiset<int>> m;
        helper(root, 0, 0, m);
        vector<vector<int>> result;
        for (const auto& [k, s] : m)
        {
            vector<int> cur;
            for (const auto& x : s) cur.emplace_back(x % 10000);
            result.emplace_back(cur);
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
    public List<List<Integer>> verticalTraversal(TreeNode root) {
        List<int[]> nodes = new ArrayList<int[]>();
        helper(root, 0, 0, nodes);
        Collections.sort(nodes, new Comparator<int[]>() {
            public int compare(int[] tuple1, int[] tuple2) {
                if (tuple1[0] != tuple2[0]) return tuple1[0] - tuple2[0];
                else if (tuple1[1] != tuple2[1]) return tuple1[1] - tuple2[1];
                else return tuple1[2] - tuple2[2];
            }
        });
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        int size = 0, lastcol = Integer.MIN_VALUE;
        for (int[] tuple : nodes) {
            int col = tuple[0], row = tuple[1], value = tuple[2];
            if (col != lastcol) {
                lastcol = col;
                result.add(new ArrayList<Integer>());
                size++;
            }
            result.get(size - 1).add(value);
        }
        return result;
    }

    private void helper(TreeNode node, int row, int col, List<int[]> nodes) {
        if (node == null) return;
        nodes.add(new int[]{col, row, node.val});
        helper(node.left, row + 1, col - 1, nodes);
        helper(node.right, row + 1, col + 1, nodes);
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
    def verticalTraversal(self, root: TreeNode) -> List[List[int]]:
        stack, d = [(root, 0, 0)], defaultdict(list)
        while stack:
            node, x, y = stack.pop()
            if node:
                d[x].append((y, node.val)) or stack.extend([(node.right, x + 1, y + 1), (node.left, x - 1, y + 1)])
        return [[val for _, val in sorted(d[x])] for x in sorted(d)]
```
