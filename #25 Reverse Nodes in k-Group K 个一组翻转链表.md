__Description__:
Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

__Example:__

Given this linked list: 1->2->3->4->5

For k = 2, you should return: 2->1->4->3->5

For k = 3, you should return: 3->2->1->4->5

__Note:__

Only constant extra memory is allowed.
You may not alter the values in the list's nodes, only nodes itself may be changed.

__题目描述__:
给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。

k 是一个正整数，它的值小于或等于链表的长度。

如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

__示例 :__

给定这个链表：1->2->3->4->5

当 k = 2 时，应当返回: 2->1->4->3->5

当 k = 3 时，应当返回: 3->2->1->4->5

__说明 :__

你的算法只能使用常数的额外空间。
你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

__思路__:
- 对每 k个一组的链表需要直接反转
- 对整个链表, 需要找到这 k个一组的链表的开头和结尾, 每次循环处理一组
- 循环结束的条件为: 一组链表中少于 k个结点, 对最后这一组不需要循环直接输出

e.g. head: 1 -> 2 -> 3 -> 4 -> 5 -> 6 ->7 -> 8
1 -> 2 -> 3反转之后成为 3 -> 2 -> 1
然后从 4开始继续反转
 
时间复杂度O(kn), 空间复杂度O(1)

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
    ListNode* reverseKGroup(ListNode* head, int k) 
    {
        ListNode *result = new ListNode(0);
        result -> next = head;
        ListNode *p = result, *q = result;
        while (q -> next)
        {
            for (int i = 0; i < k && q; i++) q = q -> next;
            if (!q) break;
            ListNode *r = p -> next, *s = q -> next;
            q -> next = nullptr;
            p -> next = reverse(r);
            r -> next = s;
            p = r;
            q = p;
        }
        return result -> next;
    }
private:
    ListNode* reverse(ListNode* head)
    {
        ListNode *result = nullptr, *p = head;
        while (p)
        {
            ListNode *q = p -> next;
            p -> next = result;
            result = p;
            p = q;
        }
        return result;
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
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode result = new ListNode(0);
        result.next = head;
        ListNode p = result, q = result;
        while (q.next != null) {
            for (int i = 0; i < k && q != null; i++) q = q.next;
            if (q == null) break;
            ListNode r = p.next, s = q.next;
            q.next = null;
            p.next = reverse(r);
            r.next = s;
            p = r;
            q = p;
        }
        return result.next;
    }
    
    private ListNode reverse(ListNode head) {
        ListNode result = null, p = head;
        while (p != null) {
            ListNode q = p.next;
            p.next = result;
            result = p;
            p = q;
        }
        return result;
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
    def reverseKGroup(self, head: ListNode, k: int) -> ListNode:
        def reverse(head: ListNode) -> ListNode:
            result, p = None, head
            while p:
                p.next, result, p = result, p, p.next
            return result
        result = ListNode(0)
        result.next = head
        p = q = result
        while q.next:
            for i in range(k):
                if not q:
                    break
                q = q.next
            if not q:
                break
            r, s, q.next = p.next, q.next, None
            p.next, r.next, p, q = reverse(r), s, r, r
        return result.next
```