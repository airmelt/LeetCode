__Description__:
Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

The update(i, val) function modifies nums by updating the element at index i to val.

__Example:__

Given nums = [1, 3, 5]

sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8
 
__Constraints:__

The array is only modifiable by the update function.
You may assume the number of calls to update and sumRange function is distributed evenly.
0 <= i <= j <= nums.length - 1

__题目描述__:
给定一个整数数组  nums，求出数组从索引 i 到 j  (i ≤ j) 范围内元素的总和，包含 i,  j 两点。

update(i, val) 函数可以通过将下标为 i 的数值更新为 val，从而对数列进行修改。

__示例 :__

Given nums = [1, 3, 5]

sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8

__说明:__

数组仅可以在 update 函数下进行修改。
你可以假设 update 函数与 sumRange 函数的调用次数是均匀分布的。

__思路__:
采用线段树
线段树中记录 start和 end为 nums的开始和结束区间, 以及 val为区间和, 线段树还有左子树右子树分别是区间的二分
否则继续调用 dfs搜索下一个子字符串
sumRange() 时间复杂度O(lgn), 空间复杂度O(n)
update() 时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class NumArray 
{
private:
    int n;
    vector<int> tree;
public:
    NumArray(vector<int>& nums) : n(nums.size()), tree(n << 1) 
    {
        for (int i = n, j = 0; i < n << 1; ++i, ++j) tree[i] = nums[j];
        for (int i = n - 1; i > 0; --i) tree[i] = tree[i << 1] + tree[(i << 1) + 1];
    }
    
    void update(int i, int val) 
    {
        i += n;
        val -= tree[i];
        while (i) 
        {
            tree[i] += val;
            i >>= 1;
        }
    }
    
    int sumRange(int i, int j) 
    {
        int result = 0;
        for (i += n, j += n; i <= j; i >>= 1, j >>= 1) 
        {
            if (i & 1) result += tree[i++];
            if (!(j & 1)) result += tree[j--];
        }
        return result;
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * obj->update(i,val);
 * int param_2 = obj->sumRange(i,j);
 */
```

__Java__:
```Java
class NumArray {

    class TreeNode {
        int val;
        int start;
        int end;
        TreeNode left;
        TreeNode right;

        public TreeNode(int start, int end) {
            left = null;
            right = null;
            this.start = start;
            this.end = end;
        }
    }


    private TreeNode buildTree(int[] nums, int start, int end) {
        if (start > end) return null;
        TreeNode curr = new TreeNode(start, end);
        if (start == end) curr.val = nums[start];
        else {
            int mid = start + ((end - start) >> 1);
            curr.left = buildTree(nums, start, mid);
            curr.right = buildTree(nums, mid + 1, end);
            curr.val = curr.left.val + curr.right.val;
        }
        return curr;
    }

    TreeNode root = null;

    public NumArray(int[] nums) {
        root = buildTree(nums, 0, nums.length - 1);
    }

    public void update(int i, int val) {
        updateTree(root, i, val);
    }

    public void updateTree(TreeNode node, int i, int val) {
        if (node.start == node.end) node.val = val;
        else {
            int mid = node.start + ((node.end - node.start) >> 1);
            if (i <= mid) updateTree(node.left, i, val);
            else updateTree(node.right, i, val);
            node.val = node.left.val + node.right.val;
        }
    }

    public int sumRange(int i, int j) {
        return queryTree(root, i, j);
    }

    public int queryTree(TreeNode node, int i, int j) {
        if (node.start == i && node.end == j) return node.val;
        else {
            int mid = node.start + ((node.end - node.start) >> 1);
            if (j <= mid) return queryTree(node.left, i, j);
            else if (i >= (mid + 1)) return queryTree(node.right, i, j);
            else return queryTree(node.left, i, mid) + queryTree(node.right, mid + 1, j);
        }
    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * obj.update(i,val);
 * int param_2 = obj.sumRange(i,j);
 */
```

__Python__:
```Python
class NumArray:

    def __init__(self, nums: List[int]):
        self.nums = nums
        

    def update(self, i: int, val: int) -> None:
        self.nums[i] = val
        

    def sumRange(self, i: int, j: int) -> int:
        return sum(self.nums[i:j + 1])


# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(i,val)
# param_2 = obj.sumRange(i,j)
```