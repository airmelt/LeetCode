# 331 Verify Preorder Serialization of a Binary Tree 验证二叉树的前序序列化

__Description__:
One way to serialize a binary tree is to use pre-order traversal. When we encounter a non-null node, we record the node's value. If it is a null node, we record using a sentinel value such as #.

```text
     _9_
    /   \
   3     2
  / \   / \
 4   1  #  6
/ \ / \   / \
# # # #   # #
```

For example, the above binary tree can be serialized to the string "9,3,4,#,#,1,#,#,2,#,6,#,#", where # represents a null node.

Given a string of comma separated values, verify whether it is a correct preorder traversal serialization of a binary tree. Find an algorithm without reconstructing the tree.

Each comma separated value in the string must be either an integer or a character '#' representing null pointer.

You may assume that the input format is always valid, for example it could never contain two consecutive commas such as "1,,3".

__Example:__

Example 1:

Input: "9,3,4,#,#,1,#,#,2,#,6,#,#"
Output: true

Example 2:

Input: "1,#"
Output: false

Example 3:

Input: "9,#,#,1"
Output: false

__题目描述__:
序列化二叉树的一种方法是使用前序遍历。当我们遇到一个非空节点时，我们可以记录下这个节点的值。如果它是一个空节点，我们可以使用一个标记值记录，例如 #。

```text
     _9_
    /   \
   3     2
  / \   / \
 4   1  #  6
/ \ / \   / \
# # # #   # #
```

例如，上面的二叉树可以被序列化为字符串 "9,3,4,#,#,1,#,#,2,#,6,#,#"，其中 # 代表一个空节点。

给定一串以逗号分隔的序列，验证它是否是正确的二叉树的前序序列化。编写一个在不重构树的条件下的可行算法。

每个以逗号分隔的字符或为一个整数或为一个表示 null 指针的 '#' 。

你可以认为输入格式总是有效的，例如它永远不会包含两个连续的逗号，比如 "1,,3" 。

__示例 :__

示例 1:

输入: "9,3,4,#,#,1,#,#,2,#,6,#,#"
输出: true

示例 2:

输入: "1,#"
输出: false

示例 3:

输入: "9,#,#,1"
输出: false

__思路__:

记录 "#"的个数, 遍历过程中要比数字小
最后的个数比数字要多 1
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isValidSerialization(string preorder) 
    {
        int num = 0, zero = 0;
        stringstream ss(preorder);
        string node;
        while (getline(ss, node, ','))
        {
            if (zero > num) return false;
            if (node == "#") ++zero;
            else ++num;
        }
        return (zero - num) == 1;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isValidSerialization(String preorder) {
        int n = preorder.length(), num = 0;
        for (int i = n - 1; i > -1; i--) {
            if (preorder.charAt(i) == ',') continue;
            if (preorder.charAt(i) == '#') ++num;
            else {
                while (i > -1 && preorder.charAt(i) != ',') --i;
                if (num >= 2) --num;
                else return false;
            }
        }
        return num == 1;
    }
}
```

__Python__:

```Python
class Solution:
    def isValidSerialization(self, preorder: str) -> bool:
        num = 1
        for node in preorder.split(','):
            num -= 1
            if num < 0:
                return False
            if node != '#':
                num += 2
        return num == 0
```
