# 148 Sort List 排序链表

__Description__:
Sort a linked list in O(n log n) time using constant space complexity.

__Example:__

Example 1:

Input: 4->2->1->3
Output: 1->2->3->4
Example 2:

Input: -1->5->3->4->0
Output: -1->0->3->4->5

__题目描述__:
在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序。

__示例 :__

示例 1:

输入: 4->2->1->3
输出: 1->2->3->4
示例 2:

输入: -1->5->3->4->0
输出: -1->0->3->4->5

__思路__:

可以采用归并排序
注意要把左链表的尾端置空
并防止断链
时间复杂度O(nlgn), 空间复杂度O(1)

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
    ListNode* sortList(ListNode* head) 
    {
        ListNode *result = new ListNode(0), *p = head;
        result -> next = head;
        int len = 0;
        while (p) 
        {
            ++len;
            p = p -> next;
        }
        for (int step = 1; step < len; step <<= 1) 
        {
            ListNode *cur = result -> next;
            p = result;
            while (cur) 
            {
                ListNode *left = cur, *right = cut(left, step);
                cur = cut(right, step);
                p -> next = merge(left, right);
                while (p -> next) p = p -> next;
            }
        }
        return result -> next;
    }
private:
    ListNode* cut(ListNode *head, int n) 
    {
        ListNode *p = head;
        while (--n and p) p = p -> next;
        if (!p) return p;
        ListNode *q = p -> next;
        p -> next = nullptr;
        return q;
    }
    
    ListNode *merge(ListNode *l1, ListNode *l2) 
    {
        ListNode *result = new ListNode(0);
        ListNode *p = result;
        while (l1 and l2) 
        {
            if (l1 -> val < l2 -> val) 
            {
                p -> next = l1;
                p = l1;
                l1 = l1 -> next;
            } 
            else 
            {
                p -> next = l2;
                p = l2;
                l2 = l2 -> next;
            }
        }
        p -> next = l1 ? l1 : l2;
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
    public ListNode sortList(ListNode head) {
        ListNode result = new ListNode(0), p = head;
        result.next = head;
        int len = 0;
        while (p != null) {
            ++len;
            p = p.next;
        }
        for (int step = 1; step < len; step <<= 1) {
            ListNode cur = result.next;
            p = result;
            while (cur != null) {
                ListNode left = cur, right = cut(left, step);
                cur = cut(right, step);
                p.next = merge(left, right);
                while (p.next != null) p = p.next;
            }
        }
        return result.next;
    }
    
    private ListNode cut(ListNode head, int n) {
        ListNode p = head;
        while (--n != 0 && p != null) p = p.next;
        if (p == null) return p;
        ListNode q = p.next;
        p.next = null;
        return q;
    }
    
    private ListNode merge(ListNode l1, ListNode l2) {
        ListNode result = new ListNode(0);
        ListNode p = result;
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                p.next = l1;
                p = l1;
                l1 = l1.next;
            } else {
                p.next = l2;
                p = l2;
                l2 = l2.next;
            }
        }
        p.next = l1 == null ? l2 : l1;
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
    def sortList(self, head: ListNode) -> ListNode:
        def merge(l1: ListNode, l2: ListNode) -> ListNode:
            if not l1:
                return l2
            if not l2:
                return l1
            if l1.val > l2.val:
                l2.next = merge(l2.next, l1)
                return l2
            else:
                l1.next = merge(l1.next, l2)
                return l1
        def divide(head: ListNode) -> ListNode:
            if not head or not head.next:
                return head
            fast = slow = p = head
            while fast and fast.next:
                p, fast, slow = slow, fast.next.next, slow.next
            p.next = None
            l1, l2 = divide(head), divide(slow)
            return merge(l1, l2)
        return divide(head)
```
