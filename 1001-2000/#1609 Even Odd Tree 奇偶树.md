# 1609 Even Odd Tree 奇偶树

__Description:__

A binary tree is named Even-Odd if it meets the following conditions:

- The root of the binary tree is at level index `0`, its children are at level index `1`, their children are at level index `2`, etc.
- For every __even-indexed__ level, all nodes at the level have __odd__ integer values in __strictly increasing__ order (from left to right).
- For every _odd-indexed_ level, all nodes at the level have _even_ integer values in __strictly decreasing__ order (from left to right).

Given the `root` of a binary tree, _return_ `true` _if the binary tree is __Even-Odd__, otherwise return_ `false`_._

__Example:__

Example 1:

![1609-1](https://assets.leetcode.com/uploads/2020/09/15/sample_1_1966.png)

```text
Input: root = [1,10,4,3,null,7,9,12,8,6,null,null,2]
Output: true
Explanation: The node values on each level are:
Level 0: [1]
Level 1: [10,4]
Level 2: [3,7,9]
Level 3: [12,8,6,2]
Since levels 0 and 2 are all odd and increasing and levels 1 and 3 are all even and decreasing, the tree is Even-Odd.
```

Example 2:

![1609-2](https://assets.leetcode.com/uploads/2020/09/15/sample_2_1966.png)

```text
Input: root = [5,4,2,3,3,7]
Output: false
Explanation: The node values on each level are:
Level 0: [5]
Level 1: [4,2]
Level 2: [3,3,7]
Node values in level 2 must be in strictly increasing order, so the tree is not Even-Odd.
```

Example 3:

![1609-3](https://assets.leetcode.com/uploads/2020/09/22/sample_1_333_1966.png)

```text
Input: root = [5,9,1,3,5,7]
Output: false
Explanation: Node values in the level 1 should be even integers.
```

__Constraints:__

- The number of nodes in the tree is in the range `[1, 10 ^ 5]`.
- `1 <= Node.val <= 10 ^ 6`

__题目描述:__

如果一棵二叉树满足下述几个条件，则可以称为 奇偶树 ：

- 二叉树根节点所在层下标为 `0` ，根的子节点所在层下标为 `1` ，根的孙节点所在层下标为 `2` ，依此类推。
- __偶数下标__ 层上的所有节点的值都是 __奇__ 整数，从左到右按顺序 __严格递增__
- __奇数下标__ 层上的所有节点的值都是 __偶__ 整数，从左到右按顺序 __严格递减__

给你二叉树的根节点，如果二叉树为 __奇偶树__ ，则返回 `true` ，否则返回 `false` 。

__示例:__

示例 1：

![1609-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/04/sample_1_1966.png)

```text
输入：root = [1,10,4,3,null,7,9,12,8,6,null,null,2]
输出：true
解释：每一层的节点值分别是：
0 层：[1]
1 层：[10,4]
2 层：[3,7,9]
3 层：[12,8,6,2]
由于 0 层和 2 层上的节点值都是奇数且严格递增，而 1 层和 3 层上的节点值都是偶数且严格递减，因此这是一棵奇偶树。
```

示例 2：

![1609-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/04/sample_2_1966.png)

```text
输入：root = [5,4,2,3,3,7]
输出：false
解释：每一层的节点值分别是：
0 层：[5]
1 层：[4,2]
2 层：[3,3,7]
2 层上的节点值不满足严格递增的条件，所以这不是一棵奇偶树。
```

示例 3：

![1609-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/04/sample_1_333_1966.png)

```text
输入：root = [5,9,1,3,5,7]
输出：false
解释：1 层上的节点值应为偶数。
```

示例 4：

```text
输入：root = [1]
输出：true
```

示例 5：

```text
输入：root = [11,8,6,1,3,9,11,30,20,18,16,12,10,4,2,17]
输出：true
```

__提示：__

- 树中节点数在范围 `[1, 10 ^ 5]` 内
- `1 <= Node.val <= 10 ^ 6`

__思路:__

```text
层序遍历
用一个队列记录每一层的所有节点
按照奇偶判断每一层的元素的奇偶性和大小关系
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

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
    bool isEvenOddTree(TreeNode* root) 
    {
        int even = 1;
        queue<TreeNode*> q{{root}};
        while (!q.empty()) 
        {
            int prev = even ? 0 : 0x3f3f3f3f, s = q.size();
            while (s--)
            {
                root = q.front();
                q.pop();
                if (even and (!(root -> val & 1) or prev >= root -> val)) return false;
                if (!even and (root -> val & 1 or prev <= root -> val)) return false;
                prev = root -> val;
                if (root -> left) q.push(root -> left);
                if (root -> right) q.push(root -> right);
            }
            even ^= 1;
        }
        return true;
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
    public boolean isEvenOddTree(TreeNode root) {
        boolean evem = true;
        Deque<TreeNode> q = new ArrayDeque<>();
        q.offer(root);
        while (!q.isEmpty()) {
            int prev = evem ? 0 : 0x3f3f3f3f, s = q.size();
            while (s-- > 0) {
                root = q.pollFirst();
                if (evem && ((root.val & 1) == 0 || prev >= root.val)) return false;
                if (!evem && ((root.val & 1) == 1 || prev <= root.val)) return false;
                prev = root.val;
                if (root.left != null) q.offer(root.left);
                if (root.right != null) q.offer(root.right);
            }
            evem = !evem;
        }
        return true;
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
    def isEvenOddTree(self, root: TreeNode) -> bool:
        even, q = 1, deque([root])
        while q:
            prev, s = 0 if even else float('inf'), len(q)
            for _ in range(s):
                root = q.popleft()
                if even and (not root.val & 1 or prev >= root.val):
                    return False
                if not even and (root.val & 1 or prev <= root.val):
                    return False
                prev = root.val
                if root.left:
                    q.append(root.left)
                if root.right:
                    q.append(root.right)
            even ^= 1
        return True
```
