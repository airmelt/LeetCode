# 449 Serialize and Deserialize BST 序列化和反序列化二叉搜索树

__Description__:
Serialization is converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary search tree. There is no restriction on how your serialization/deserialization algorithm should work. You need to ensure that a binary search tree can be serialized to a string, and this string can be deserialized to the original tree structure.

The encoded string should be as compact as possible.

__Example:__

Example 1:

Input: root = [2,1,3]
Output: [2,1,3]

Example 2:

Input: root = []
Output: []

__Constraints:__

The number of nodes in the tree is in the range [0, 10^4].
0 <= Node.val <= 10^4
The input tree is guaranteed to be a binary search tree.

__题目描述__:
序列化是将数据结构或对象转换为一系列位的过程，以便它可以存储在文件或内存缓冲区中，或通过网络连接链路传输，以便稍后在同一个或另一个计算机环境中重建。

设计一个算法来序列化和反序列化 二叉搜索树 。 对序列化/反序列化算法的工作方式没有限制。 您只需确保二叉搜索树可以序列化为字符串，并且可以将该字符串反序列化为最初的二叉搜索树。

编码的字符串应尽可能紧凑。

__示例 :__

示例 1：

输入：root = [2,1,3]
输出：[2,1,3]

示例 2：

输入：root = []
输出：[]

__提示：__

树中节点数范围是 [0, 10^4]
0 <= Node.val <= 10^4
题目数据 保证 输入的树是一棵二叉搜索树。

__注意：__
不要使用类成员/全局/静态变量来存储状态。 你的序列化和反序列化算法应该是无状态的。

__思路__:

序列化采用后序遍历, 这样反序列化时可以直接从列表的最后拿不需要记录节点下标
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
private:
    void post_order(TreeNode *root, string &result)
    {
        if (!root) return;
        post_order(root -> left, result);
        post_order(root -> right, result);
        result += to_string(root -> val) + " ";
    }
    
    
    TreeNode* helper(int lower, int upper, vector<int> &nums) 
    {
        if (nums.empty()) return nullptr;
        int val = nums.back();
        if (val < lower or val > upper) return nullptr;
        nums.pop_back();
        TreeNode *root = new TreeNode(val);
        root -> right = helper(val, upper, nums);
        root -> left = helper(lower, val, nums);
        return root;
    }
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) 
    {
        string result;
        post_order(root, result);
        return result;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) 
    {
        if (data.empty()) return nullptr;
        stringstream ss;
        string num;
        vector<int> nums;
        ss << data;
        while (ss >> num) nums.emplace_back(stoi(num));
        return helper(INT_MIN, INT_MAX, nums);
    }
};

// Your Codec object will be instantiated and called as such:
// Codec* ser = new Codec();
// Codec* deser = new Codec();
// string tree = ser->serialize(root);
// TreeNode* ans = deser->deserialize(tree);
// return ans;
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
    private StringBuilder postorder(TreeNode root, StringBuilder sb) {
        if (root == null) return sb;
        postorder(root.left, sb);
        postorder(root.right, sb);
        sb.append(root.val);
        sb.append(' ');
        return sb;
    }

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder sb = postorder(root, new StringBuilder());
        if (sb.length() > 0) sb.deleteCharAt(sb.length() - 1);
        return sb.toString();
    }

    public TreeNode helper(Integer lower, Integer upper, ArrayDeque<Integer> nums) {
        if (nums.isEmpty()) return null;
        int val = nums.getLast();
        if (val < lower || val > upper) return null;
        nums.removeLast();
        TreeNode root = new TreeNode(val);
        root.right = helper(val, upper, nums);
        root.left = helper(lower, val, nums);
        return root;
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if (data.isEmpty()) return null;
        ArrayDeque<Integer> nums = new ArrayDeque<Integer>();
        for (String s : data.split("\\s+")) nums.add(Integer.valueOf(s));
        return helper(Integer.MIN_VALUE, Integer.MAX_VALUE, nums);
    }
}

// Your Codec object will be instantiated and called as such:
// Codec ser = new Codec();
// Codec deser = new Codec();
// String tree = ser.serialize(root);
// TreeNode ans = deser.deserialize(tree);
// return ans;
```

__Python__:

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Codec:

    def serialize(self, root: TreeNode) -> str:
        """Encodes a tree to a single string.
        """
        def postorder(root: TreeNode) -> List[int]:
            return postorder(root.left) + postorder(root.right) + [root.val] if root else []
        return ' '.join(map(str, postorder(root)))
        

    def deserialize(self, data: str) -> TreeNode:
        """Decodes your encoded data to tree.
        """
        def helper(lower:int = float('-inf'), upper:int = float('inf')) -> TreeNode:
            if not data or data[-1] < lower or data[-1] > upper:
                return None
            val = data.pop()
            root, root.right, root.left = TreeNode(val), helper(val, upper), helper(lower, val)
            return root
        data = [int(x) for x in data.split(' ') if x]
        return helper()
        

# Your Codec object will be instantiated and called as such:
# Your Codec object will be instantiated and called as such:
# ser = Codec()
# deser = Codec()
# tree = ser.serialize(root)
# ans = deser.deserialize(tree)
# return ans
```
