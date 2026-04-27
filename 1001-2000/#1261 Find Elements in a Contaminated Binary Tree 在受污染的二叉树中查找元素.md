# 1261 Find Elements in a Contaminated Binary Tree 在受污染的二叉树中查找元素

__Description:__

Given a binary tree with the following rules:

root.val == 0
If treeNode.val == x and treeNode.left != null, then treeNode.left.val == 2 \* x + 1
If treeNode.val == x and treeNode.right != null, then treeNode.right.val == 2 \* x + 2
Now the binary tree is contaminated, which means all treeNode.val have been changed to -1.

Implement the FindElements class:

FindElements(TreeNode* root) Initializes the object with a contaminated binary tree and recovers it.
bool find(int target) Returns true if the target value exists in the recovered binary tree.

__Example:__

Example 1:

![Binary Tree 1](https://assets.leetcode.com/uploads/2019/11/06/untitled-diagram-4-1.jpg)

Input
["FindElements","find","find"]
[[[-1,null,-1]],[1],[2]]
Output
[null,false,true]
Explanation

```Java
FindElements findElements = new FindElements([-1,null,-1]); 
findElements.find(1); // return False 
findElements.find(2); // return True 
```

Example 2:

![Binary Tree 2](https://assets.leetcode.com/uploads/2019/11/06/untitled-diagram-4.jpg)

Input
["FindElements","find","find","find"]
[[[-1,-1,-1,-1,-1]],[1],[3],[5]]
Output
[null,true,true,false]
Explanation

```Java
FindElements findElements = new FindElements([-1,-1,-1,-1,-1]);
findElements.find(1); // return True
findElements.find(3); // return True
findElements.find(5); // return False
```

Example 3:

![Binary Tree 3](https://assets.leetcode.com/uploads/2019/11/07/untitled-diagram-4-1-1.jpg)

Input
["FindElements","find","find","find","find"]
[[[-1,null,-1,-1,null,-1]],[2],[3],[4],[5]]
Output
[null,true,false,false,true]
Explanation

```Java
FindElements findElements = new FindElements([-1,null,-1,-1,null,-1]);
findElements.find(2); // return True
findElements.find(3); // return False
findElements.find(4); // return False
findElements.find(5); // return True
```

__Constraints:__

TreeNode.val == -1
The height of the binary tree is less than or equal to 20
The total number of nodes is between [1, 10^4]
Total calls of find() is between [1, 10^4]
0 <= target <= 10^6

__题目描述：__

给出一个满足下述规则的二叉树：

root.val == 0
如果 treeNode.val == x 且 treeNode.left != null，那么 treeNode.left.val == 2 \* x + 1
如果 treeNode.val == x 且 treeNode.right != null，那么 treeNode.right.val == 2 \* x + 2
现在这个二叉树受到「污染」，所有的 treeNode.val 都变成了 -1。

请你先还原二叉树，然后实现 FindElements 类：

FindElements(TreeNode* root) 用受污染的二叉树初始化对象，你需要先把它还原。
bool find(int target) 判断目标值 target 是否存在于还原后的二叉树中并返回结果。

__示例：__

示例 1：

![二叉树 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/16/untitled-diagram-4-1.jpg)

输入：
["FindElements","find","find"]
[[[-1,null,-1]],[1],[2]]
输出：
[null,false,true]
解释：

```Java
FindElements findElements = new FindElements([-1,null,-1]); 
findElements.find(1); // return False 
findElements.find(2); // return True 
```

示例 2：

![二叉树 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/16/untitled-diagram-4.jpg)

输入：
["FindElements","find","find","find"]
[[[-1,-1,-1,-1,-1]],[1],[3],[5]]
输出：
[null,true,true,false]
解释：

```Java
FindElements findElements = new FindElements([-1,-1,-1,-1,-1]);
findElements.find(1); // return True
findElements.find(3); // return True
findElements.find(5); // return False
```

示例 3：

![二叉树 3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/16/untitled-diagram-4-1-1.jpg)

输入：
["FindElements","find","find","find","find"]
[[[-1,null,-1,-1,null,-1]],[2],[3],[4],[5]]
输出：
[null,true,false,false,true]
解释：

```Java
FindElements findElements = new FindElements([-1,null,-1,-1,null,-1]);
findElements.find(2); // return True
findElements.find(3); // return False
findElements.find(4); // return False
findElements.find(5); // return True
```

__提示：__

TreeNode.val == -1
二叉树的高度不超过 20
节点的总数在 [1, 10^4] 之间
调用 find() 的总次数在 [1, 10^4] 之间
0 <= target <= 10^6

__思路：__

1. DFS
先按照题意还原二叉树
还原的同时将二叉树的值加入哈希表中方便查找
还原时间复杂度为 O(n), 空间复杂度为 O(n), 查找时间复杂度 O(1)
2. 不还原二叉树
因为只用搜索到 target
每次从根节点出发搜索 target即可
还原时间复杂度为 O(1), 空间复杂度为 O(1), 查找时间复杂度 O(n)

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
class FindElements 
{
private:
    int val;
    TreeNode* cur;
    
    bool dfs(TreeNode* cur, int val, int target)
    {
        if (!cur) return false;
        if (val == target) return true;
        return dfs(cur -> left, (val << 1) + 1, target) || dfs(cur -> right, (val << 1) + 2, target);
    }
public:
    FindElements(TreeNode* root) 
    {
        cur = root;
        val = 0;
    }
    
    bool find(int target) 
    {
        return dfs(cur, val, target);
    }
};

/**
 * Your FindElements object will be instantiated and called as such:
 * FindElements* obj = new FindElements(root);
 * bool param_1 = obj->find(target);
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
class FindElements {
    private Set<Integer> set = new HashSet<>();

    public FindElements(TreeNode root) {
        dfs(root, 0);
    }
    
    private void dfs(TreeNode root, int val) {
        if (root != null) {
            root.val = val;
            set.add(val);
            dfs(root.left, (val << 1) + 1);
            dfs(root.right, (val << 1) + 2);
        }
    }
    
    public boolean find(int target) {
        return set.contains(target);
    }
}

/**
 * Your FindElements object will be instantiated and called as such:
 * FindElements obj = new FindElements(root);
 * boolean param_1 = obj.find(target);
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
class FindElements:

    def __init__(self, root: Optional[TreeNode]):
        self.data = set()
        
        def dfs(node: Optional[TreeNode], val: int) -> None:
            if node:
                node.val = val
                self.data.add(val)
                dfs(node.left, (val << 1) + 1)
                dfs(node.right, (val << 1) + 2)
        dfs(root, 0)


    def find(self, target: int) -> bool:
        return target in self.data



# Your FindElements object will be instantiated and called as such:
# obj = FindElements(root)
# param_1 = obj.find(target)
```
