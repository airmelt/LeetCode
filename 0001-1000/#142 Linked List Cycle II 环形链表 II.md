# 142 Linked List Cycle II 环形链表 II

__Description__:
Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.

__Note:__
Do not modify the linked list.

__Example:__

Example 1:

![Linked List 1](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.

Example 2:

![Linked List 2](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.

Example 3:

![Linked List 3](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.

__Follow-up:__
Can you solve it without using extra space?

__题目描述__:
给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

__说明：__
不允许修改给定的链表。

__示例 :__

示例 1：

![链表 1](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

输入：head = [3,2,0,-4], pos = 1
输出：tail connects to node index 1
解释：链表中有一个环，其尾部连接到第二个节点。

示例 2：

![Linked List 2](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

输入：head = [1,2], pos = 0
输出：tail connects to node index 0
解释：链表中有一个环，其尾部连接到第一个节点。

示例 3：

![Linked List 3](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

输入：head = [1], pos = -1
输出：no cycle
解释：链表中没有环。

__进阶：__
你是否可以不用额外空间解决此题？

__思路__:

快慢指针
首先设立两个指针, 快指针每次走 2步, 慢指针每次走 1步
如果有环, 快慢指针最终一定会相遇, 且一定不为空
这时快指针刚好比慢指针多绕了一圈
这时如果快指针退回起点, 然后两个指针都每次走 1步
再次相遇的时候刚好就是环的起点
时间复杂度O(n), 空间复杂度O(1)

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
class Solution 
{
public:
    ListNode *detectCycle(ListNode *head) 
    {
        ListNode *fast = head, *slow = head;
        while (fast and fast -> next)
        {
            slow = slow -> next;
            fast = fast -> next -> next;
            if (slow == fast)
            {
                fast = head;
                while (slow != fast)
                {
                    slow = slow -> next;
                    fast = fast -> next;
                }
                return fast;
            }
        }
        return nullptr;
    }
};
```

__Java__:

```Java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head, slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                fast = head;
                while (fast != slow) {
                    fast = fast.next;
                    slow = slow.next;
                }
                return fast;
            }
        }
        return null;
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

class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        fast = slow = head
        while fast and fast.next:
            fast, slow = fast.next.next, slow.next
            if fast is slow:
                fast = head
                while fast is not slow:
                    fast, slow = fast.next, slow.next
                return fast
        return None
```
