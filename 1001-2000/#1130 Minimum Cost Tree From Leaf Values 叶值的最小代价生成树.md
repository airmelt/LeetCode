# 1130 Minimum Cost Tree From Leaf Values 叶值的最小代价生成树

__Description__:
Given an array arr of positive integers, consider all binary trees such that:

Each node has either 0 or 2 children;
The values of arr correspond to the values of each leaf in an in-order traversal of the tree.
The value of each non-leaf node is equal to the product of the largest leaf value in its left and right subtree, respectively.
Among all possible binary trees considered, return the smallest possible sum of the values of each non-leaf node. It is guaranteed this sum fits into a 32-bit integer.

A node is a leaf if and only if it has zero children.

__Example:__

Example 1:

![树 1](https://assets.leetcode.com/uploads/2021/08/10/tree1.jpg)

Input: arr = [6,2,4]
Output: 32
Explanation: There are two possible trees shown.
The first has a non-leaf node sum 36, and the second has non-leaf node sum 32.

Example 2:

![树 2](https://assets.leetcode.com/uploads/2021/08/10/tree2.jpg)

Input: arr = [4,11]
Output: 44

__Constraints:__

2 <= arr.length <= 40
1 <= arr[i] <= 15
It is guaranteed that the answer fits into a 32-bit signed integer (i.e., it is less than 2^31).

__题目描述__:
给你一个正整数数组 arr，考虑所有满足以下条件的二叉树：

每个节点都有 0 个或是 2 个子节点。
数组 arr 中的值与树的中序遍历中每个叶节点的值一一对应。（知识回顾：如果一个节点有 0 个子节点，那么该节点为叶节点。）
每个非叶节点的值等于其左子树和右子树中叶节点的最大值的乘积。
在所有这样的二叉树中，返回每个非叶节点的值的最小可能总和。这个和的值是一个 32 位整数。

__示例 :__

输入：arr = [6,2,4]
输出：32
解释：
有两种可能的树，第一种的非叶节点的总和为 36，第二种非叶节点的总和为 32。

```text
    24            24
   /  \          /  \
  12   4        6    8
 /  \               / \
6    2             2   4
```

__提示:__

2 <= arr.length <= 40
1 <= arr[i] <= 15
答案保证是一个 32 位带符号整数，即小于 2^31。

__思路__:

单调栈
每次需要选出最小的两个数进行合并, 留下较大的数参与后面的合并, 直到最后只剩下一个数
比如 [6, 2, 4, 8] -> [6, 4, 8] -> [6, 8] -> [8]
所以可以使用单调栈记录将单调递增的先合并
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int mctFromLeafValues(vector<int>& arr)
    {
        stack<int> s;
        s.push(INT_MAX);
        int result = 0, n = arr.size();
        for (int i = 0; i < n; i++) 
        {
            while (s.top() < arr[i]) 
            {
                int cur = s.top();
                s.pop();
                result += cur * min(arr[i], s.top());
            }
            s.push(arr[i]);
        }
        while (s.size() > 2) 
        {
            int cur = s.top();
            s.pop();
            result += cur * s.top();
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int mctFromLeafValues(int[] arr) {
        Stack<Integer> stack = new Stack<>();
        stack.push(Integer.MAX_VALUE);
        int result = 0, n = arr.length;
        for (int i = 0; i < n; i++) {
            while (stack.peek() < arr[i]) result += stack.pop() * Math.min(arr[i], stack.peek());
            stack.push(arr[i]);
        }
        while (stack.size() > 2) result += stack.pop() * stack.peek();
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def mctFromLeafValues(self, arr: List[int]) -> int:
        result, stack, n = 0, [float('inf')], len(arr)
        for i in range(n):
            while stack[-1] < arr[i]:
                result += stack.pop() * min(arr[i], stack[-1])
            stack.append(arr[i])
        while (len(stack) > 2):
            result += stack.pop() * stack[-1]
        return result
```
