__Description__:
Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling next() will return the next smallest number in the BST.

__Example:__
![BST](https://upload-images.jianshu.io/upload_images/16639143-ae24bdc7547b5324.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

BSTIterator iterator = new BSTIterator(root);
iterator.next();    // return 3
iterator.next();    // return 7
iterator.hasNext(); // return true
iterator.next();    // return 9
iterator.hasNext(); // return true
iterator.next();    // return 15
iterator.hasNext(); // return true
iterator.next();    // return 20
iterator.hasNext(); // return false
 
__Note:__

next() and hasNext() should run in average O(1) time and uses O(h) memory, where h is the height of the tree.
You may assume that next() call will always be valid, that is, there will be at least a next smallest number in the BST when next() is called.

__题目描述__:
实现一个二叉搜索树迭代器。你将使用二叉搜索树的根节点初始化迭代器。

调用 next() 将返回二叉搜索树中的下一个最小的数。

__示例 :__
![二叉搜索树](https://upload-images.jianshu.io/upload_images/16639143-dde20ac632694797.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
BSTIterator iterator = new BSTIterator(root);
iterator.next();    // 返回 3
iterator.next();    // 返回 7
iterator.hasNext(); // 返回 true
iterator.next();    // 返回 9
iterator.hasNext(); // 返回 true
iterator.next();    // 返回 15
iterator.hasNext(); // 返回 true
iterator.next();    // 返回 20
iterator.hasNext(); // 返回 false
 
__提示：__

next() 和 hasNext() 操作的时间复杂度是 O(1)，并使用 O(h) 内存，其中 h 是树的高度。
你可以假设 next() 调用总是有效的，也就是说，当调用 next() 时，BST 中至少存在一个下一个最小的数。

__思路__:
由于需要递增存储二叉树, 采用中序遍历
这里新建一个中序遍历的栈需要时间复杂度 O(n)
由于只有 n个结点, 每次 next()也只要返回 n个结点, 实际上均摊时间复杂度 O(1)
时间复杂度O(1), 空间复杂度O(n)

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
class BSTIterator 
{
private:
    stack<TreeNode*> s;
public:
    BSTIterator(TreeNode* root) 
    {
        while (root)
        {
            s.push(root);
            root = root -> left;
        }
    }
    
    /** @return the next smallest number */
    int next() 
    {
        TreeNode* cur = s.top();
        s.pop();
        int result = cur -> val;
        cur = cur -> right;
        while (cur)
        {
            s.push(cur);
            cur = cur -> left;
        }
        return result;
    }
    
    /** @return whether we have a next smallest number */
    bool hasNext() 
    {
        return !s.empty();
    }
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
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
 *     TreeNode(int x) { val = x; }
 * }
 */
class BSTIterator {

    private Stack<TreeNode> s;
    public BSTIterator(TreeNode root) {
        s = new Stack<>();
        while (root != null) {
            s.push(root);
            root = root.left;
        }
    }
    
    /** @return the next smallest number */
    public int next() {
        TreeNode temp = s.pop();
        int result = temp.val;
        temp = temp.right;
        while (temp != null) {
            s.push(temp);
            temp = temp.left;
        }
        return result;
    }
    
    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return !s.isEmpty();
    }
}

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator obj = new BSTIterator(root);
 * int param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
```

__Python__:
```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class BSTIterator:

    def __init__(self, root: TreeNode):
        self.s = []
        while root:
            self.s.append(root)
            root = root.left
        

    def next(self) -> int:
        """
        @return the next smallest number
        """
        temp = self.s.pop()
        result = temp.val
        temp = temp.right
        while temp:
            self.s.append(temp)
            temp = temp.left
        return result

    def hasNext(self) -> bool:
        """
        @return whether we have a next smallest number
        """
        return len(self.s) != 0


# Your BSTIterator object will be instantiated and called as such:
# obj = BSTIterator(root)
# param_1 = obj.next()
# param_2 = obj.hasNext()
```