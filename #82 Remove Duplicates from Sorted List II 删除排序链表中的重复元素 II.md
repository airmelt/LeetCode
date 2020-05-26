__Description__:
Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.

Return the linked list sorted as well.

__Example:__
Example 1:

Input: 1->2->3->3->4->4->5
Output: 1->2->5

Example 2:

Input: 1->1->1->2->3
Output: 2->3

__题目描述__:
给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中 没有重复出现 的数字。

__示例 :__
示例 1:

输入: 1->2->3->3->4->4->5
输出: 1->2->5

示例 2:

输入: 1->1->1->2->3
输出: 2->3

__思路__:
新建一个 dummy结点, dummy结点的下一个结点指向 head
用两个指针分别指向 dummy结点和 head
指向 head的指针作为工作指针, 查找并跳过重复的结点
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
    ListNode* deleteDuplicates(ListNode* head) 
    {
        if (!head || !head -> next) return head;
        ListNode* result = new ListNode(-1);
        result -> next = head;
        ListNode *p = head, *q = result;
        while (p)
        {
            if (p -> next && p -> next -> val == p -> val)
            {
                while (p -> next && p -> next -> val == p -> val) p = p -> next;
                q -> next = p -> next;
                p = p -> next;
            }
            else
            {
                p = p -> next;
                q = q -> next;
            }
        }
        return result -> next;
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
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode result = new ListNode(-1);
        result.next = head;
        ListNode p = head, q = result;
        while (p != null) {
            if (p.next != null && p.next.val == p.val) {
                while (p.next != null && p.next.val == p.val) p = p.next;
                q.next = p.next;
                p = p.next;
            } else {
                p = p.next;
                q = q.next;
            }
        }
        return result.next;
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
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        result = ListNode(-1)
        result.next, p, q = head, head, result
        while p:
            if p.next and p.val == p.next.val:
                while p.next and p.val == p.next.val:
                    p = p.next
                q.next = p.next
                p = p.next
            else:
                p = p.next
                q = q.next
        return result.next
```