__Description__:
You are given two __non-empty__ linked lists representing two non-negative integers. The digits are stored in __reverse order__ and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

__Example:__

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.

__题目描述__:
给出两个 __非空__ 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 __逆序__ 的方式存储的，并且它们的每个节点只能存储 __一位__ 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

__示例 :__

输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807

__思路__:
类似加法, 设置一个进位记录是否有进位, 对链表逐位进行加减, 长度不足的用 0代替
时间复杂度O(n), 空间复杂度O(n)

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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) 
    {
        ListNode* result = new ListNode(0);
        ListNode* r = result;
        int c = 0;
        while (l1 || l2) 
        {
            int x = l1 ? l1 -> val : 0, y = l2 ? l2 -> val : 0;
            r -> next = new ListNode((c + x + y) % 10);
            c = (c + x + y) / 10;
            r = r -> next;
            l1 = l1 ? l1 -> next : nullptr;
            l2 = l2 ? l2 -> next : nullptr;
        }
        if (c > 0) r -> next = new ListNode(c);
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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode result = new ListNode(0);
        int c = 0;
        ListNode r = result;
        while (l1 != null || l2 != null) {
            int x = l1 != null ? l1.val : 0, y = l2 != null ? l2.val : 0;
            r.next = new ListNode((c + x + y) % 10);
            c = (c + x + y) / 10;
            r = r.next;
            l1 = l1 != null ? l1.next : null;
            l2 = l2 != null ? l2.next : null;
        }
        if (c > 0) r.next = new ListNode(c);
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
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        c, result = 0, ListNode(0)
        r = result
        while l1 or l2:
            x, y = l1.val if l1 else 0, l2.val if l2 else 0
            r.next = ListNode((c + x + y) % 10)
            r = r.next
            c = (c + x + y) // 10
            l1, l2 = l1.next if l1 else None, l2.next if l2 else None
        if c > 0:
            r.next = ListNode(c)
        return result.next
```