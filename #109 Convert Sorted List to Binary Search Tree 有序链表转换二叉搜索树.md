# 109 Convert Sorted List to Binary Search Tree 有序链表转换二叉搜索树

__Description__:
Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

__Example:__

Given the sorted linked list: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

```text
      0
     / \
   -3   9
   /   /
 -10  5
```

__题目描述__:
给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

__示例 :__

给定的有序链表： [-10, -3, 0, 5, 9],

一个可能的答案是：[0, -3, 9, -10, null, 5], 它可以表示下面这个高度平衡二叉搜索树：

```text
      0
     / \
   -3   9
   /   /
 -10  5
```

__思路__:

递归法
使用快慢指针找到链表中点即为根节点
前一半链表为左子树, 后一半链表为右子树
时间复杂度O(nlgn), 空间复杂度O(lgn)

__代码__:
__C++__:

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution 
{
public:
    TreeNode* sortedListToBST(ListNode* head) 
    {
        if (!head) return nullptr;
        TreeNode *root = new TreeNode(head -> val);
        if (!head -> next) return root;
        ListNode *pre = head, *p = pre -> next, *q = p -> next;
        while (q and q -> next)
        {
            pre = pre -> next;
            p = pre -> next;
            q = q -> next -> next;
        }
        pre -> next = nullptr;
        root -> val = p -> val;
        root -> left = sortedListToBST(head);
        root -> right = sortedListToBST(p -> next);
        return root;
    }
};
```

__Java__:

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        if (head == null) return null;
        if (head.next == null) return new TreeNode(head.val);
        ListNode pre = head, p = head.next, q = head.next.next;
        while (q != null && q.next != null) {
            pre = pre.next;
            p = pre.next;
            q = q.next.next;
        }
        pre.next = null;
        TreeNode root = new TreeNode(p.val);
        root.left = sortedListToBST(head);
        root.right = sortedListToBST(p.next);
        return root;
    }
}
```

__Python__:

```Python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def sortedListToBST(self, head: ListNode) -> TreeNode:
        def helper(head: TreeNode, tail: TreeNode) -> TreeNode:
            if head == tail:
                return
            slow, fast = head, head
            while fast != tail and fast.next != tail:
                slow, fast = slow.next, fast.next.next
            root = TreeNode(slow.val)
            root.left, root.right = helper(head,slow), helper(slow.next,tail)
            return root
        return helper(head, None)
```
