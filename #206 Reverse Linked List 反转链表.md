__Description__:
Reverse a singly linked list.

**Example:**
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL

__Follow up__:
A linked list can be reversed either iteratively or recursively. Could you implement both?

__题目描述__:
反转一个单链表。

__示例__:
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL

__进阶__:
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

__思路__:
1. 将链表的结点全部压入栈然后弹出
时间复杂度O(n), 空间复杂度O(n)
2. 记录结点和结点的后一个结点, 将后一个结点的指针指向该结点
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head || !head -> next) return head;
        ListNode* result = NULL;
        while (head) {
            ListNode* next = head -> next;
            head -> next = result;
            result = head;
            head = next;
        }
        return result;
    }
};
```

__Java__:
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null) return null;
        Stack<ListNode> stack = new Stack<>();
        while (head.next != null) {
            stack.push(head);
            head = head.next;
        }
        ListNode result = head;
        while (!stack.empty()) {
            result.next = stack.pop();
            result = result.next;
        }
        result.next = null;
        return head;
    }
}
```

__Python__:
```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        return self.reverseListNode(head, None)

    def reverseListNode(self, head: ListNode, pre: ListNode) -> ListNode:
        if not head:
            return pre
        next = head.next
        head.next = pre
        return self.reverseListNode(next, head)
```
