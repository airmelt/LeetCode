__Description__:
Given a singly linked list L: L0→L1→…→Ln-1→Ln,
reorder it to: L0→Ln→L1→Ln-1→L2→Ln-2→…

You may not modify the values in the list's nodes, only nodes itself may be changed.

__Example:__
Example 1:

Given 1->2->3->4, reorder it to 1->4->2->3.

Example 2:

Given 1->2->3->4->5, reorder it to 1->5->2->4->3.

__题目描述__:
给定一个单链表 L：L0→L1→…→Ln-1→Ln ，
将其重新排列后变为： L0→Ln→L1→Ln-1→L2→Ln-2→…

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

__示例 :__
示例 1:

给定链表 1->2->3->4, 重新排列为 1->4->2->3.

示例 2:

给定链表 1->2->3->4->5, 重新排列为 1->5->2->4->3.

__思路__:
此题为 2009年 408原题
首先用快慢指针找到中点
将中点之后到链表反转
将中点之后的链表插入到开头
时间复杂度O(n), 空间复杂度O(1)

__代码__:
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
    void reorderList(ListNode* head) 
    {
        if (!head) return;
        ListNode *fast = head, *slow = head, *p = nullptr, *q = head;
        while (fast and fast -> next)
        {
            fast = fast -> next -> next;
            slow = slow -> next;
        }
        fast = slow -> next;
        slow -> next = nullptr;
        while (fast)
        {
            p = fast -> next;
            fast -> next = slow -> next;
            slow -> next = fast;
            fast = p;
        }
        fast = slow -> next;
        slow -> next = nullptr;
        while (fast)
        {
            p = fast -> next;
            fast -> next = q -> next;
            q -> next = fast;
            q = fast -> next;
            fast = p;
        }
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
    public void reorderList(ListNode head) {
        if (head == null) return;
        ListNode fast = head, slow = head, p = null, q = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        fast = slow.next;
        slow.next = null;
        while (fast != null) {
            p = fast.next;
            fast.next = slow.next;
            slow.next = fast;
            fast = p;
        }
        fast = slow.next;
        slow.next = null;
        while (fast != null) {
            p = fast.next;
            fast.next = q.next;
            q.next = fast;
            q = fast.next;
            fast = p;
        }
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
    def reorderList(self, head: ListNode) -> None:
        """
        Do not return anything, modify head in-place instead.
        """
        if not head or not head.next:
            return
        fast, slow, p = head, head, head
        while fast and fast.next:
            fast, slow = fast.next.next, slow.next
        fast, slow.next = slow.next, None
        while fast:
            fast.next, slow.next, fast = slow.next, fast, fast.next
        fast, slow.next = slow.next, None
        while fast:
            p.next, fast.next, p, fast = fast, p.next, p.next, fast.next
```