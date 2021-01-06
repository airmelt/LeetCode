# 86 Partition List 分隔链表

__Description__:
Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.

You should preserve the original relative order of the nodes in each of the two partitions.

__Example:__

Input: head = 1->4->3->2->5->2, x = 3
Output: 1->2->2->4->3->5

__题目描述__:
给定一个链表和一个特定值 x，对链表进行分隔，使得所有小于 x 的节点都在大于或等于 x 的节点之前。

你应当保留两个分区中每个节点的初始相对位置。

__示例 :__

输入: head = 1->4->3->2->5->2, x = 3
输出: 1->2->2->4->3->5

__思路__:

使用 4个指针
1个指向小于 x的开头, 1个指向大于等于 x的开头, 另外两个工作指针随着 head移动
注意最后将大于等于 x的工作指针指向 nullptr
时间复杂度O(mn), 空间复杂度O(m)

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
    ListNode* partition(ListNode* head, int x) 
    {
        ListNode *result = new ListNode(-1), *p = new ListNode(-1), *q = new ListNode(-1);
        ListNode *low = p, *high = q;
        while (head)
        {
            if (head -> val < x)
            {
                p -> next = head;
                p = p -> next;
            }
            else
            {
                q -> next = head;
                q = q -> next;
            }
            head = head -> next;
        }
        q -> next = nullptr;
        p -> next = high -> next;
        result -> next = low -> next;
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
    public ListNode partition(ListNode head, int x) {
        ListNode result = new ListNode(-1), p = new ListNode(-1), q = new ListNode(-1);
        ListNode low = p, high = q;
        while (head != null) {
            if (head.val < x) {
                p.next = head;
                p = p.next;
            } else {
                q.next = head;
                q = q.next;
            }
            head = head.next;
        }
        q.next = null;
        p.next = high.next;
        result.next = low.next;
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
    def partition(self, head: ListNode, x: int) -> ListNode:
        result, p, q = ListNode(-1), ListNode(-1), ListNode(-1)
        low, high = p, q
        while head:
            if head.val < x:
                p.next = head
                p = p.next
            else:
                q.next = head
                q = q.next
            head = head.next
        q.next = None
        p.next = high.next
        result.next = low.next
        return result.next
```
