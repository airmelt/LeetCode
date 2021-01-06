# 92 Reverse Linked List II 反转链表 II

__Description__:
Reverse a linked list from position m to n. Do it in one-pass.

__Note:__
1 ≤ m ≤ n ≤ length of list.

__Example:__

Input: 1->2->3->4->5->NULL, m = 2, n = 4
Output: 1->4->3->2->5->NULL

__题目描述__:
反转从位置 m 到 n 的链表。请使用一趟扫描完成反转。

__说明：__
1 ≤ m ≤ n ≤ 链表长度。

__示例 :__

输入: 1->2->3->4->5->NULL, m = 2, n = 4
输出: 1->4->3->2->5->NULL

__思路__:

头插法
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
    ListNode* reverseBetween(ListNode* head, int m, int n) 
    {
        ListNode* result = new ListNode(-1);
        result -> next = head;
        ListNode *p = result, *q = result;
        for (int i = 1; i < m; i++) p = p -> next;
        head = p -> next;
        for (int i = m; i < n; i++) 
        {
            q = head -> next;
            head -> next = q -> next;
            q -> next = p -> next;
            p -> next = q;
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
    public ListNode reverseBetween(ListNode head, int m, int n) {
        ListNode result = new ListNode(-1);
        result.next = head;
        ListNode p = result, q = result;
        for (int i = 1; i < m; i++) p = p.next;
        head = p.next;
        for (int i = m; i < n; i++) {
            q = head.next;
            head.next = q.next;
            q.next = p.next;
            p.next = q;
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
    def reverseBetween(self, head: ListNode, m: int, n: int) -> ListNode:
        result = ListNode(-1)
        result.next, p = head, result
        for _ in range(1, m):
            p = p.next
        head = p.next
        for _ in range(m, n):
            q = head.next
            head.next, q.next, p.next = q.next, p.next, q
        return result.next
```
