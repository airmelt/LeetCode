# 203 Remove Linked List Elements 移除链表元素

__Description__:
Remove all elements from a linked list of integers that have value val.

**Example:**

Input:  1->2->6->3->4->5->6, val = 6
Output: 1->2->3->4->5

__题目描述__:
删除链表中等于给定值 val 的所有节点。

**示例：**

输入: 1->2->6->3->4->5->6, val = 6
输出: 1->2->3->4->5

__思路__:

1. 递归, 递归终点为 head为空
2. 迭代, 设置一个空的头结点或者判断一下头结点的值是否为要删除的值
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
    ListNode* removeElements(ListNode* head, int val) 
    {
        while (head and head -> val == val) head = head -> next;
        if (!head) return head;
        ListNode *p = head -> next, *q = head;
        while (p) 
        {
            if (p -> val == val) 
            {
                q -> next = p -> next;
                p = q -> next;
            } 
            else 
            {
                p = p -> next;
                q = q -> next;
            }
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
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) return head;
        head.next = removeElements(head.next, val);
        return head.val == val ? head.next : head;
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
    def removeElements(self, head: ListNode, val: int) -> ListNode:
        result = ListNode(0)
        result.next = head
        p = result
        while p.next:
            if p.next.val == val:
                p.next = p.next.next
            else:
                p = p.next
        return result.next
```
