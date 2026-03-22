# 297 Serialize and Deserialize Binary Tree 二叉树的序列化与反序列化

__Description__:
Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

__Clarification:__
The input/output format is the same as how LeetCode serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

__Example:__

Example 1:

![Binary Tree](https://assets.leetcode.com/uploads/2020/09/15/serdeser.jpg)

Input: root = [1,2,3,null,null,4,5]
Output: [1,2,3,null,null,4,5]

Example 2:

Input: root = []
Output: []

Example 3:

Input: root = [1]
Output: [1]

Example 4:

Input: root = [1,2]
Output: [1,2]

__Constraints:__

The number of nodes in the tree is in the range [0, 104].
-1000 <= Node.val <= 1000

__题目描述__:
序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。

请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。

__示例 :__

你可以将以下二叉树：

```text
    1
   / \
  2   3
     / \
    4   5
```

序列化为 "[1,2,3,null,null,4,5]"

__提示:__
这与 LeetCode 目前使用的方式一致，详情请参阅 LeetCode 序列化二叉树的格式。你并非必须采取这种方式，你也可以采用其他的方法解决这个问题。

__说明:__
不要使用类的成员 / 全局 / 静态变量来存储状态，你的序列化和反序列化算法应该是无状态的。

__思路__:

可以用任何一种方法遍历二叉树, 用一个特殊符号或者 null记录空结点, 用特殊符号分割开每一个结点
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Codec 
{
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) 
    {
        if (!root) return "#_";
        string result = to_string(root -> val) + "_";
        result += serialize(root -> left);
        result += serialize(root -> right);
        return result;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) 
    {
        stringstream ss(data);
        string item;
        queue<string> q;
        while (std::getline(ss, item, '_')) q.push(item);
        return helper(q);
    }
private:
    TreeNode* helper(queue<string> &q)
    {
        string val = q.front();
        q.pop();
        if (val == "#") return nullptr;
        TreeNode* root = new TreeNode(stoi(val));
        root -> left = helper(q);
        root -> right = helper(q);
        return root;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec ser, deser;
// TreeNode* ans = deser.deserialize(ser.serialize(root));
```

__Java__:

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if (root == null) return "null";
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        StringBuilder sb = new StringBuilder();
        sb.append(root.val).append(",");
        while (!q.isEmpty()) {
            TreeNode cur = q.poll();
            if (cur.left != null) {
                sb.append(cur.left.val).append(",");
                q.offer(cur.left);
            }
            else sb.append("null,");
            if (cur.right != null) {
                sb.append(cur.right.val).append(",");
                q.offer(cur.right);
            }
            else sb.append("null,");
        }
        return sb.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] node = data.split(",");
        TreeNode root = null;
        if (node[0].equals("null")) return null;
        root = new TreeNode(Integer.parseInt(node[0]));
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        for (int i = 1; i < node.length; i++) {
            TreeNode cur = q.poll();
            if (!node[i].equals("null")) {
                cur.left = new TreeNode(Integer.parseInt(node[i]));
                q.offer(cur.left);
            } else cur.left = null;
            if (!node[++i].equals("null")) {
                cur.right = new TreeNode(Integer.parseInt(node[i]));
                q.offer(cur.right);
            } else cur.right = null;
        }
        return root;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec ser = new Codec();
// Codec deser = new Codec();
// TreeNode ans = deser.deserialize(ser.serialize(root));
```

__Python__:

```Python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Codec:

    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
        def dfs(root):
            if not root:
                return 'null,'
            left, right = dfs(root.left), dfs(root.right)
            return str(root.val) + ',' + left + right
        return dfs(root)

    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """
        def dfs(data):
            val = data.pop(0)
            if val == 'null':
                return None
            node = TreeNode(val)
            node.left = dfs(data)
            node.right = dfs(data)
            return node
        data = data.split(',')
        return dfs(data)
        

# Your Codec object will be instantiated and called as such:
# ser = Codec()
# deser = Codec()
# ans = deser.deserialize(ser.serialize(root))
```
