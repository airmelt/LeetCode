__Description__:
Given a sorted linked list, delete all duplicates such that each element appear only once.

__Example__:
Example 1:
Input: 1->1->2
Output: 1->2

Example 2:
Input: 1->1->2->3->3
Output: 1->2->3

__题目描述__:
给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

 __示例__:
示例 1:
输入: 1->1->2
输出: 1->2

示例 2:
输入: 1->1->2->3->3
输出: 1->2->3

__思路__:
注意是排序链表, 所以只需要比较下一个结点即可
可以用递归/迭代的方式
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
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head || !head -> next) return head;
        head -> next = deleteDuplicates(head -> next);
        if (head -> val == head -> next -> val) head = head -> next;
        return head;
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
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null) return head;
        ListNode p = head;
        while (p.next != null) {
            if (p.next.val == p.val) p.next = p.next.next;
            else p = p.next;
        }
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
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        head.next = self.deleteDuplicates(head.next)
        if head.next.val == head.val:
            head = head.next
        return head
```
