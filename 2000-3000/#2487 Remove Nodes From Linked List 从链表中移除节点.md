# 2487 Remove Nodes From Linked List 从链表中移除节点

__Description:__

You are given the `head` of a linked list.

Remove every node which has a node with a greater value anywhere to the right side of it.

Return _the_ `head` _of the modified linked list._

__Example:__

Example 1:

![2487-1](https://assets.leetcode.com/uploads/2022/10/02/drawio.png)

```text
Input: head = [5,2,13,3,8]
Output: [13,8]
Explanation: The nodes that should be removed are 5, 2 and 3.
```

- Node 13 is to the right of node 5.
- Node 13 is to the right of node 2.
- Node 8 is to the right of node 3.

Example 2:

```text
Input: head = [1,1,1,1]
Output: [1,1,1,1]
Explanation: Every node has value 1, so no nodes are removed.
```

__Constraints:__

- The number of the nodes in the given list is in the range `[1, 10 ^ 5]`.
- `1 <= Node.val <= 10 ^ 5`

__题目描述:__

给你一个链表的头节点 `head` 。

移除每个右侧有一个更大数值的节点。

返回修改后链表的头节点 `head` 。

__示例:__

示例 1：

![2487-2](https://assets.leetcode.com/uploads/2022/10/02/drawio.png)

```text
输入：head = [5,2,13,3,8]
输出：[13,8]
解释：需要移除的节点是 5 ，2 和 3 。
```

- 节点 13 在节点 5 右侧。
- 节点 13 在节点 2 右侧。
- 节点 8 在节点 3 右侧。

示例 2：

```text
输入：head = [1,1,1,1]
输出：[1,1,1,1]
解释：每个节点的值都是 1 ，所以没有需要移除的节点。
```

__提示：__

- 给定列表中的节点数目在范围 `[1, 10 ^ 5]` 内
- `1 <= Node.val <= 10 ^ 5`

__思路:__

```text
1. 递归
如果 head 没有下一个节点, 返回 head
找到下一个节点 cur
如果 cur 的值大于 head 的值, 返回 cur, 相当于删除 head
否则 head 的下一个节点指向 cur, 返回 head
时间复杂度为 O(N), 空间复杂度为 O(N)
2. 迭代
反转链表
遍历链表, 如果当前节点的值小于下一个节点的值, 删除当前节点
最后返回反转后的链表
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution 
{
public:
    ListNode* removeNodes(ListNode* head) 
    {
        if (!head -> next) return head;
        ListNode* cur = removeNodes(head -> next);
        if (cur -> val > head -> val) return cur;
        head -> next = cur;
        return head;
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
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNodes(ListNode head) {
        if (head.next == null) return head;
        ListNode cur = removeNodes(head.next);
        if (cur.val > head.val) return cur;
        head.next = cur;
        return head;
    }
}
```

__Python__:

```Python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNodes(self, head: Optional[ListNode]) -> Optional[ListNode]:
        def reverseList(head: Optional[ListNode]) -> Optional[ListNode]:
            pre, cur = None, head
            while cur:
                nxt = cur.next
                cur.next = pre
                pre = cur
                cur = nxt
            return pre
        cur = head = reverseList(head)
        while cur.next:
            if cur.val > cur.next.val:
                cur.next = cur.next.next
            else:
                cur = cur.next
        return reverseList(head)
```
