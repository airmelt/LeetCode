__Description__:
Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

__Example__:

Given the sorted array: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:
```
      0
     / \
   -3   9
   /   /
 -10  5
```

__题目描述__:
将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

 __示例__：

给定有序数组: [-10,-3,0,5,9],

一个可能的答案是：[0,-3,9,-10,null,5]，它可以表示下面这个高度平衡二叉搜索树：
```
      0
     / \
   -3   9
   /   /
 -10  5
```

__思路__:
分治法, 可以将转化为二叉搜索树的操作看成二分查找, 剩下的分成左右子树继续查找
时间复杂度O(n), 空间复杂度O(lgn), n为树中结点数

[二叉搜索树 Binary search tree-wiki](https://en.wikipedia.org/wiki/Binary_search_tree)
> __二叉查找树__（英语：Binary Search Tree），也称为__二叉搜索树__、__有序二叉树__（ordered binary tree）或__排序二叉树__（sorted binary tree）
> 指一棵空树或者具有下列性质的[二叉树](https://zh.wikipedia.org/wiki/%E4%BA%8C%E5%8F%89%E6%A0%91 "二叉树")：
> 1. 若任意节点的左子树不空，则左子树上所有节点的值均小于它的根节点的值；
> 2. 若任意节点的右子树不空，则右子树上所有节点的值均大于它的根节点的值；
> 3. 任意节点的左、右子树也分别为二叉查找树；
> 4. 没有键值相等的节点。
>
> 二叉查找树相比于其他数据结构的优势在于查找、插入的时间复杂度较低。为O(*logn*)
> 二叉查找树是基础性数据结构，用于构建更为抽象的数据结构，如[集合](https://zh.wikipedia.org/wiki/%E9%9B%86%E5%90%88_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6) "集合 (计算机科学)")、[多重集](https://zh.wikipedia.org/wiki/%E5%A4%9A%E9%87%8D%E9%9B%86 "多重集")、[关联数组](https://zh.wikipedia.org/wiki/%E5%85%B3%E8%81%94%E6%95%B0%E7%BB%84 "关联数组")等。
> 虽然二叉查找树的最坏效率是O(*n*)，但它支持动态查询，且有很多改进版的二叉查找树可以使树高为O(*logn*)，从而将最坏效率降至O(*logn*)，如[AVL树](https://zh.wikipedia.org/wiki/AVL%E6%A0%91 "AVL树")、[红黑树](https://zh.wikipedia.org/wiki/%E7%BA%A2%E9%BB%91%E6%A0%91 "红黑树")等。
> 在二叉搜索树中的查找可以类比有序数组中的二分查找
> 中序遍历二叉查找树可得到一个关键字的有序序列，一个无序序列可以通过构造一棵二叉查找树变成一个有序序列，构造树的过程即为对无序序列进行查找的过程。



__代码__:
__C++__:
```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return sortedArrayToBST(nums, 0, nums.size() - 1);
    }
private:
    TreeNode* sortedArrayToBST(vector<int>& nums, int left, int right) {
        if (right < left) return NULL;
        int mid = left + (right - left) / 2;
        TreeNode* root = new TreeNode(nums[mid]);
        root -> left = sortedArrayToBST(nums, left, mid - 1);
        root -> right = sortedArrayToBST(nums, mid + 1, right);
        return root;
    }
};
```

__Java__:
```
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
    public TreeNode sortedArrayToBST(int[] nums) {
        return sortedArrayToBST(nums, 0, nums.length - 1);
    }

    private TreeNode sortedArrayToBST(int[] nums, int left, int right) {
        if (left > right) return null;
        int mid = left + (right - left) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = sortedArrayToBST(nums, left, mid - 1);
        root.right = sortedArrayToBST(nums, mid + 1, right);
        return root;
    }
}
```

__Python__:
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> TreeNode:
        if not nums:
            return
        mid = len(nums) // 2
        root = TreeNode(nums[mid])
        root.left = self.sortedArrayToBST(nums[:mid])
        root.right = self.sortedArrayToBST(nums[mid + 1:])
        return root
```
