# 445 Add Two Numbers II 两数相加 II

__Description__:
You are given two non-empty linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

__Follow up:__
What if you cannot modify the input lists? In other words, reversing the lists is not allowed.

__Example:__

```text
Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7
```

__题目描述__:
给你两个 非空 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。

你可以假设除了数字 0 之外，这两个数字都不会以零开头。

__进阶：__

如果输入链表不能修改该如何处理？换句话说，你不能对列表中的节点进行翻转。

__示例 :__

```text
输入：(7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 8 -> 0 -> 7
```

__思路__:

参考[LeetCode #2 Add Two Numbers 两数相加](https://www.jianshu.com/p/e6d073efa29a)
如果不能修改原链表可以用两个栈记录每一个节点的数值, 再相加
如果可以修改原链表, 可以先计算出两个链表的长度再用递归求解
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
        stack<int> s1, s2;
        while (l1)
        {
            s1.push(l1 -> val);
            l1 = l1 -> next;
        }
        while (l2)
        {
            s2.push(l2 -> val);
            l2 = l2 -> next;
        }
        int carry = 0;
        ListNode *head = nullptr;
        while (!s1.empty() or !s2.empty() or carry)
        {
            int cur = carry;
            if (!s1.empty()) 
            {
                cur += s1.top();
                s1.pop();
            }
            if (!s2.empty()) 
            {
                cur += s2.top();
                s2.pop();
            }
            ListNode *node = new ListNode(cur % 10);
            node -> next = head;
            head = node;
            carry = cur / 10;
        }
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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode p = l1, q = l2;
        int len1 = getLength(p), len2 = getLength(q);
        if (len1 < len2) return addTwoNumbers(l2, l1);
        if (getCarry(l1, l2, len1 - len2) != 0) {
            ListNode cur = new ListNode(1);
            cur.next = l1;
            l1 = cur;
        }
        return l1;
    }
    
    private int getLength(ListNode head) {
        int result = 0;
        while (head != null) {
            head = head.next;
            ++result;
        }
        return result;
    }
    
    private int getCarry(ListNode l1, ListNode l2, int gap) {
        if (l1 == null || l2 == null) return 0;
        int s = 0;
        if (gap > 0) s = l1.val + getCarry(l1.next, l2, gap - 1);
        else s = l1.val + l2.val + getCarry(l1.next, l2.next, gap);
        l1.val = s % 10;
        return s / 10;
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
        s1, s2 = [], []
        while l1:
            s1.append(l1.val)
            l1 = l1.next
        while l2:
            s2.append(l2.val)
            l2 = l2.next
        carry, head = 0, None
        while s1 or s2 or carry:
            cur = (s1.pop() if s1 else 0) + (s2.pop() if s2 else 0) + carry
            node = ListNode(cur % 10)
            node.next, head, carry = head, node, cur // 10
        return head
```
