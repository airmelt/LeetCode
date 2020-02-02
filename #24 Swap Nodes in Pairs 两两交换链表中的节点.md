__Description__:
Given a linked list, swap every two adjacent nodes and return its head.

You may not modify the values in the list's nodes, only nodes itself may be changed.

__Example:__

Given 1->2->3->4, you should return the list as 2->1->4->3.

__题目描述__:
给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

__示例 :__

给定 1->2->3->4, 你应该返回 2->1->4->3.

__思路__:
1. 迭代法, 设置一个头结点, 对当前工作结点的下一个结点和下下个结点进行交换, 工作结点每次向后移动 2步即可
2. 递归法, 将当前结点和后面的结点进行交换, 对当前结点的下下个结点递归调用函数, 出口为空结点或者下一个结点为空
时间复杂度O(n), 空间复杂度O(1), 不考虑递归栈的大小

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
    ListNode* swapPairs(ListNode* head) 
    {
        if (!head || !head -> next) return head;
        ListNode *result = head -> next;
        head -> next = swapPairs(result -> next);
        result -> next = head;
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
    public ListNode swapPairs(ListNode head) {
        ListNode result = new ListNode(-1);
        result.next = head;
        ListNode p = result, q = null;
        while (p.next != null && p.next.next != null) {
            q = p.next;
            p.next = q.next;
            q.next = q.next.next;
            p.next.next = q;
            p = q;
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
    def swapPairs(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        else:
            head, head.next, head.next.next = head.next, head, self.swapPairs(head.next.next)
        return head
```