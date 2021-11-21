# 919 Complete Binary Tree Inserter 完全二叉树插入器

__Description__:
A complete binary tree is a binary tree in which every level, except possibly the last, is completely filled, and all nodes are as far left as possible.

Design an algorithm to insert a new node to a complete binary tree keeping it complete after the insertion.

Implement the CBTInserter class:

CBTInserter(TreeNode root) Initializes the data structure with the root of the complete binary tree.
int insert(int v) Inserts a TreeNode into the tree with value Node.val == val so that the tree remains complete, and returns the value of the parent of the inserted TreeNode.
TreeNode get_root() Returns the root node of the tree.

__Example:__

Example 1:

![Binary Tree Inserter](https://upload-images.jianshu.io/upload_images/16639143-83fb878e2737552a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Input
["CBTInserter", "insert", "insert", "get_root"]
[[[1, 2]], [3], [4], []]
Output
[null, 1, 2, [1, 2, 3, 4]]

Explanation

```Java
CBTInserter cBTInserter = new CBTInserter([1, 2]);
cBTInserter.insert(3);  // return 1
cBTInserter.insert(4);  // return 2
cBTInserter.get_root(); // return [1, 2, 3, 4]
```

__Constraints:__

The number of nodes in the tree will be in the range [1, 1000].
0 <= Node.val <= 5000
root is a complete binary tree.
0 <= val <= 5000
At most 10^4 calls will be made to insert and get_root.

__题目描述__:
完全二叉树是每一层（除最后一层外）都是完全填充（即，节点数达到最大）的，并且所有的节点都尽可能地集中在左侧。

设计一个用完全二叉树初始化的数据结构 CBTInserter，它支持以下几种操作：

CBTInserter(TreeNode root) 使用头节点为 root 的给定树初始化该数据结构；
CBTInserter.insert(int v)  向树中插入一个新节点，节点类型为 TreeNode，值为 v 。使树保持完全二叉树的状态，并返回插入的新节点的父节点的值；
CBTInserter.get_root() 将返回树的头节点。

__示例 :__

示例 1：

输入：inputs = ["CBTInserter","insert","get_root"], inputs = [[[1]],[2],[]]
输出：[null,1,[1,2]]

示例 2：

输入：inputs = ["CBTInserter","insert","insert","get_root"], inputs = [[[1,2,3,4,5,6]],[7],[8],[]]
输出：[null,3,4,[1,2,3,4,5,6,7,8]]

__提示:__

最初给定的树是完全二叉树，且包含 1 到 1000 个节点。
每个测试用例最多调用 CBTInserter.insert  操作 10000 次。
给定节点或插入节点的每个值都在 0 到 5000 之间。

__思路__:

层序遍历
初始化时按照层序遍历加入等待队列
左右子树有空的需要在等待队列中等待插入
左右子树都非空的弹出等待队列
插入时先插入左子树, 右子树插入时即满足两个子树都非空将这个结点从等待队列中弹出
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
class CBTInserter 
{
private:
    TreeNode* root;
    queue<TreeNode*> data;
public:
    CBTInserter(TreeNode* root) 
    {
        this -> root = root;
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) 
        {
            TreeNode* cur = q.front();
            q.pop();
            if (!cur -> left or !cur -> right) data.push(cur);
            if (cur -> left) q.push(cur -> left);
            if (cur -> right) q.push(cur -> right);
        }
    }
    
    int insert(int val) 
    {
        TreeNode* cur = data.front();
        if (!cur -> left) 
        {
            cur -> left = new TreeNode(val);
            data.push(cur -> left);
        } else {
            cur -> right = new TreeNode(val);
            data.pop();
            data.push(cur -> right);
        }
        return cur -> val;
    }
    
    TreeNode* get_root() 
    {
        return root;
    }
};

/**
 * Your CBTInserter object will be instantiated and called as such:
 * CBTInserter* obj = new CBTInserter(root);
 * int param_1 = obj->insert(val);
 * TreeNode* param_2 = obj->get_root();
 */
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
class CBTInserter {
    private Queue<TreeNode> data;
    private TreeNode root;

    public CBTInserter(TreeNode root) {
        this.root = root;
        this.data = new LinkedList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            TreeNode cur = queue.poll();
            if (cur.left == null || cur.right == null) this.data.offer(cur);
            if (cur.left != null) queue.offer(cur.left);
            if (cur.right != null) queue.offer(cur.right);
        }
    }
    
    public int insert(int val) {
        TreeNode cur = this.data.peek();
        if (cur.left == null) {
            cur.left = new TreeNode(val);
            this.data.offer(cur.left);
        } else {
            cur.right = new TreeNode(val);
            this.data.poll();
            this.data.offer(cur.right);
        }
        return cur.val;
    }
    
    public TreeNode get_root() {
        return this.root;
    }
}

/**
 * Your CBTInserter object will be instantiated and called as such:
 * CBTInserter obj = new CBTInserter(root);
 * int param_1 = obj.insert(val);
 * TreeNode param_2 = obj.get_root();
 */
```

__Python__:

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class CBTInserter:

    def __init__(self, root: TreeNode):
        self.root = root
        self.data = deque()
        q = deque([root])
        while q:
            (((x := q.popleft()).left is None or x.right is None) and self.data.append(x)) or (x.left and q.append(x.left)) or (x.right and q.append(x.right))
                

    def insert(self, val: int) -> int:
        x = self.data[0]
        if x.left is None:
            x.left = TreeNode(val)
            self.data.append(x.left)
        else:
            x.right = TreeNode(val)
            self.data.popleft()
            self.data.append(x.right)
        return x.val
    

    def get_root(self) -> TreeNode:
        return self.root


# Your CBTInserter object will be instantiated and called as such:
# obj = CBTInserter(root)
# param_1 = obj.insert(val)
# param_2 = obj.get_root()
```
