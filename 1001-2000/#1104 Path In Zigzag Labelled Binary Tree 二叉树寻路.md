# 1104 Path In Zigzag Labelled Binary Tree 二叉树寻路

__Description__:
In an infinite binary tree where every node has two children, the nodes are labelled in row order.

In the odd numbered rows (ie., the first, third, fifth,...), the labelling is left to right, while in the even numbered rows (second, fourth, sixth,...), the labelling is right to left.

![tree](https://assets.leetcode.com/uploads/2019/06/24/tree.png)

Given the label of a node in this tree, return the labels in the path from the root of the tree to the node with that label.

__Example:__

Example 1:

Input: label = 14
Output: [1,3,4,14]

Example 2:

Input: label = 26
Output: [1,2,6,10,26]

__Constraints:__

1 <= label <= 10^6

__题目描述__:
在一棵无限的二叉树上，每个节点都有两个子节点，树中的节点 逐行 依次按 “之” 字形进行标记。

如下图所示，在奇数行（即，第一行、第三行、第五行……）中，按从左到右的顺序进行标记；

而偶数行（即，第二行、第四行、第六行……）中，按从右到左的顺序进行标记。

![树](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/06/28/tree.png)

给你树上某一个节点的标号 label，请你返回从根节点到该标号为 label 节点的路径，该路径是由途经的节点标号所组成的。

__示例 :__

示例 1：

输入：label = 14
输出：[1,3,4,14]

示例 2：

输入：label = 26
输出：[1,2,6,10,26]

__提示:__

1 <= label <= 10^6

__思路__:

数学
从 label 开始反向加入
每次将 label 减半
然后取 label 除第一位取反
比如 14 减半为 7(0b0111)
7 -> 4(0b0100), 可以用异或实现
7 ^ (1 << (bit_length(label) - 1) - 1 = 7 ^ (1 << 2) - 1 = 7 ^ 4 - 1 = 7 ^ 3 = 4
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    int bit_length(int n) 
    {
        return n ? bit_length(n >> 1) + 1 : 0; 
    }
public:
    vector<int> pathInZigZagTree(int label) 
    {
        vector<int> result;
        while (label != 1) 
        {
            result.emplace(result.begin(), label);
            label >>= 1;
            label = label ^ (1 << (bit_length(label) - 1)) - 1;
        }
        result.emplace(result.begin(), 1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> pathInZigZagTree(int label) {
        List<Integer> result = new ArrayList<>();
        while (label != 1) {
            result.add(0, label);
            label >>>= 1;
            label = label ^ (1 << (bitLength(label) - 1)) - 1;
        }
        result.add(0, 1);
        return result;
    }
    
    private int bitLength(int n) {
        return n == 0 ? 0 : bitLength(n >> 1) + 1;
    }
}
```

__Python__:

```Python
class Solution:
    def pathInZigZagTree(self, label: int) -> List[int]:
        result = []
        while label != 1:
            result.append(label)
            label >>= 1
            label = label ^ (1 << (label.bit_length() - 1)) - 1
        return [1] + result[::-1]
```
