# 654 Maximum Binary Tree 最大二叉树

__Description__:
You are given an integer array nums with no duplicates. A maximum binary tree can be built recursively from nums using the following algorithm:

Create a root node whose value is the maximum value in nums.
Recursively build the left subtree on the subarray prefix to the left of the maximum value.
Recursively build the right subtree on the subarray suffix to the right of the maximum value.
Return the maximum binary tree built from nums.

__Example:__

Example 1:

![tree 1](https://assets.leetcode.com/uploads/2020/12/24/tree1.jpg)

Input: nums = [3,2,1,6,0,5]
Output: [6,3,5,null,2,0,null,null,1]
Explanation: The recursive calls are as follow:

- The largest value in [3,2,1,6,0,5] is 6. Left prefix is [3,2,1] and right suffix is [0,5].
  - The largest value in [3,2,1] is 3. Left prefix is [] and right suffix is [2,1].
    - Empty array, so no child.
    - The largest value in [2,1] is 2. Left prefix is [] and right suffix is [1].
      - Empty array, so no child.
      - Only one element, so child is a node with value 1.
  - The largest value in [0,5] is 5. Left prefix is [0] and right suffix is [].
    - Only one element, so child is a node with value 0.
    - Empty array, so no child.

Example 2:

![tree 2](https://assets.leetcode.com/uploads/2020/12/24/tree2.jpg)

Input: nums = [3,2,1]
Output: [3,null,2,null,1]

__Constraints:__

1 <= nums.length <= 1000
0 <= nums[i] <= 1000
All integers in nums are unique.

__题目描述__:
给定一个不含重复元素的整数数组 nums 。一个以此数组直接递归构建的 最大二叉树 定义如下：

二叉树的根是数组 nums 中的最大元素。
左子树是通过数组中 最大值左边部分 递归构造出的最大二叉树。
右子树是通过数组中 最大值右边部分 递归构造出的最大二叉树。
返回有给定数组 nums 构建的 最大二叉树 。

__示例 :__

示例 1：

![树 1](https://assets.leetcode.com/uploads/2020/12/24/tree1.jpg)

输入：nums = [3,2,1,6,0,5]
输出：[6,3,5,null,2,0,null,null,1]
解释：递归调用如下所示：

- [3,2,1,6,0,5] 中的最大值是 6 ，左边部分是 [3,2,1] ，右边部分是 [0,5] 。
  - [3,2,1] 中的最大值是 3 ，左边部分是 [] ，右边部分是 [2,1] 。
    - 空数组，无子节点。
    - [2,1] 中的最大值是 2 ，左边部分是 [] ，右边部分是 [1] 。
      - 空数组，无子节点。
      - 只有一个元素，所以子节点是一个值为 1 的节点。
  - [0,5] 中的最大值是 5 ，左边部分是 [0] ，右边部分是 [] 。
    - 只有一个元素，所以子节点是一个值为 0 的节点。
    - 空数组，无子节点。

示例 2：

![树 2](https://assets.leetcode.com/uploads/2020/12/24/tree2.jpg)

输入：nums = [3,2,1]
输出：[3,null,2,null,1]

__提示:__

1 <= nums.length <= 1000
0 <= nums[i] <= 1000
nums 中的所有整数 互不相同

__思路__:

递归

1. 按照题意每一轮找出最大值, 最大值左边的数组作为左子树, 最大值右边的数组作为右子树递归
时间复杂度 O(n ^ 2), 空间复杂度 O(n)
2. 不用每次都找到最大值, 将根结点设置为一个不可能达到的最大值(比如题目给的范围是 1000, 可以设置任何一个大于 1000 的根结点), 然后向根结点添加一个右结点, 然后记录结点和数组的位置, 按大小分别将结点插入树中即可
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
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) 
    {
        return helper(nums.begin(), nums.end());
    }
private:
    TreeNode* helper(vector<int>::iterator left, vector<int>::iterator right)
    {
        if (left == right) return nullptr;
        auto it = max_element(left, right);
        TreeNode *root = new TreeNode(*it);
        root -> left = helper(left, it);
        root -> right = helper(it + 1, right);
        return root;
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
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        TreeNode root = new TreeNode(Integer.MAX_VALUE);
        helper(nums, root, 0);
        return root.right;
    }
    
    private int helper(int[] nums, TreeNode root, int pos) {
        while (pos > -1 && pos < nums.length) {
            if (nums[pos] < root.val) {
                TreeNode cur = new TreeNode(nums[pos]), right = root.right;
                root.right = cur;
                cur.left = right;
                pos = helper(nums, cur, pos + 1);
            } else return pos;
        }
        return -1;
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
    def constructMaximumBinaryTree(self, nums: List[int]) -> TreeNode:
        if not nums:
            return None
        root, i = TreeNode(max(nums)), nums.index(max(nums))
        root.left, root.right = self.constructMaximumBinaryTree(nums[:i]), self.constructMaximumBinaryTree(nums[i + 1:])
        return root
```
