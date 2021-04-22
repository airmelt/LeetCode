# 430 Flatten a Multilevel Doubly Linked List 扁平化多级双向链表

__Description__:
You are given a doubly linked list which in addition to the next and previous pointers, it could have a child pointer, which may or may not point to a separate doubly linked list. These child lists may have one or more children of their own, and so on, to produce a multilevel data structure, as shown in the example below.

Flatten the list so that all the nodes appear in a single-level, doubly linked list. You are given the head of the first level of the list.

__Example:__

Example 1:

Input: head = [1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]
Output: [1,2,3,7,8,11,12,9,10,4,5,6]
Explanation:

The multilevel linked list in the input is as follows:

![Multilevel Linked List](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/multilevellinkedlist.png)

After flattening the multilevel linked list it becomes:

![Flatten Linked List](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/multilevellinkedlistflattened.png)

Example 2:

Input: head = [1,2,null,3]
Output: [1,3,2]
Explanation:

The input multilevel linked list is as follows:

```text
  1---2---NULL
  |
  3---NULL
```

Example 3:

Input: head = []
Output: []

How multilevel linked list is represented in test case:

We use the multilevel linked list from Example 1 above:

```text
 1---2---3---4---5---6--NULL
         |
         7---8---9---10--NULL
             |
             11--12--NULL
```

The serialization of each level is as follows:

```text
[1,2,3,4,5,6,null]
[7,8,9,10,null]
[11,12,null]
```

To serialize all levels together we will add nulls in each level to signify no node connects to the upper node of the previous level. The serialization becomes:

```text
[1,2,3,4,5,6,null]
[null,null,7,8,9,10,null]
[null,11,12,null]
```

Merging the serialization of each level and removing trailing nulls we obtain:

```text
[1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]
```

__Constraints:__

The number of Nodes will not exceed 1000.
1 <= Node.val <= 10^5

__题目描述__:
多级双向链表中，除了指向下一个节点和前一个节点指针之外，它还有一个子链表指针，可能指向单独的双向链表。这些子列表也可能会有一个或多个自己的子项，依此类推，生成多级数据结构，如下面的示例所示。

给你位于列表第一级的头节点，请你扁平化列表，使所有结点出现在单级双链表中。

__示例 :__

示例 1：

输入：head = [1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]
输出：[1,2,3,7,8,11,12,9,10,4,5,6]
解释：

输入的多级列表如下图所示：

![多级链表 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/multilevellinkedlist.png)

扁平化后的链表如下图：

![扁平的链表 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/multilevellinkedlistflattened.png)

示例 2：

输入：head = [1,2,null,3]
输出：[1,3,2]
解释：

输入的多级列表如下图所示：

```text
  1---2---NULL
  |
  3---NULL
```

示例 3：

输入：head = []
输出：[]

如何表示测试用例中的多级链表？

以 示例 1 为例：

```text
 1---2---3---4---5---6--NULL
         |
         7---8---9---10--NULL
             |
             11--12--NULL
```

序列化其中的每一级之后：

```text
[1,2,3,4,5,6,null]
[7,8,9,10,null]
[11,12,null]
```

为了将每一级都序列化到一起，我们需要每一级中添加值为 null 的元素，以表示没有节点连接到上一级的上级节点。

```text
[1,2,3,4,5,6,null]
[null,null,7,8,9,10,null]
[null,11,12,null]
```

合并所有序列化结果，并去除末尾的 null 。

```text
[1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]
```

__提示：__

节点数目不超过 1000
1 <= Node.val <= 10^5

__思路__:

按照题目要求, 每次到有孩子节点时就要保存当前节点的下一节点
所以可以考虑用栈来存储节点
注意有孩子节点的需要将孩子节点置空
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* prev;
    Node* next;
    Node* child;
};
*/

class Solution 
{
public:
    Node* flatten(Node* head) 
    {
        Node* cur = head;
        stack<Node*> s;
        while (cur)
        {
            if (cur -> child)
            {
                if (cur -> next) s.push(cur -> next);
                cur -> next = cur -> child;
                cur -> child -> prev = cur;
                cur -> child = nullptr;
            }
            else if (!cur -> next and !s.empty())
            {
                cur -> next = s.top();
                s.pop();
                cur -> next -> prev = cur;
            }
            cur = cur -> next;
        }
        return head;
    }
};
```

__Java__:

```Java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node prev;
    public Node next;
    public Node child;
};
*/

class Solution {
    public Node flatten(Node head) {
        Node cur = head;
        Stack<Node> stack = new Stack<>();
        while (cur != null) {
            if (cur.child != null) {
                if (cur.next != null) stack.push(cur.next);
                cur.child.prev = cur;
                cur.next = cur.child;
                cur.child = null;
            } else if (cur.next == null && !stack.isEmpty()) {
                cur.next = stack.pop();
                cur.next.prev = cur;
            }
            cur = cur.next;
        }
        return head;
    }
}
```

__Python__:

```Python
"""
# Definition for a Node.
class Node:
    def __init__(self, val, prev, next, child):
        self.val = val
        self.prev = prev
        self.next = next
        self.child = child
"""

class Solution:
    def flatten(self, head: 'Node') -> 'Node':
        cur, stack = head, []
        while cur:
            if cur.child:
                if cur.next:
                    stack.append(cur.next)
                cur.child.prev, cur.next, cur.child = cur, cur.child, None
            elif not cur.next and stack:
                cur.next, cur.next.prev = stack.pop(), cur
            cur = cur.next
        return head
```
