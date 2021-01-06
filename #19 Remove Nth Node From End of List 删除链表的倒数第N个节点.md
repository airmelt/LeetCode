# 19 Remove Nth Node From End of List 删除链表的倒数第N个节点

__Description__:
Given a linked list, remove the n-th node from the end of list and return its head.

__Example:__

Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.

__Note:__

Given n will always be valid.

__Follow up:__

Could you do this in one pass?

__题目描述__:
给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

__示例 :__

给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.

__说明：__

给定的 n 保证是有效的。

__进阶：__

你能尝试使用一趟扫描实现吗？

__思路__:

此题为 408原题

1. 先遍历一次链表获取长度, 找到倒数第 n个结点的位置并移除, 需要遍历两次链表
2. 设置快慢指针, 快指针先移动 n个结点, 然后快慢指针同步移动, 移除当快指针为空时慢指针位置的结点即可, 只需要遍历一次链表
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
    ListNode* removeNthFromEnd(ListNode* head, int n) 
    {
        if (!head or !head -> next) return nullptr;
        ListNode *fast = head, *slow = head;
        for (int i = 0; i < n; i++) fast = fast -> next;
        if (!fast) return head -> next;
        while (fast -> next) 
        {
            fast = fast -> next;
            slow = slow -> next;
        }
        slow -> next = slow -> next -> next;
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
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if (head == null || head.next == null) return null;
        ListNode fast = head, slow = head;
        for (int i = 0; i < n; i++) fast = fast.next;
        if (fast == null) return head.next;
        while (fast.next != null) {
            fast = fast.next;
            slow = slow.next;
        }
        slow.next = slow.next.next;
        return head;
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
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        dummy = ListNode(0)
        dummy.next = head
        p = q = dummy
        for i in range(n + 1):
            p = p.next
        while p:
            p, q = p.next, q.next
        q.next = q.next.next
        return dummy.next
```
